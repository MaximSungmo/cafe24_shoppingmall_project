###  회원정보업데이트 API(/customer/{no})

■ request

```
PUT
  params:
  	body:
      {
      "agreement_fl": "string",	// 약관동의서 동의 유무
      "email": "string",		// 이메일(ID)
      "gender": "string",		// 성별
      "name": "string",			// 이름
      "no": 0,					// 번호 
      "password": "string",		// 비밀번호
      "phone": "string",		// 핸드폰 번호
      "terms_of_use_no": 0,		// 약관동의서 번호
      }
```

■ response

```
200 : 성공
	CustomerVo(사용자 정보)
400 : 잘못된 요청 
	입력 에러 메세지 
500 : 내부 서버 에러
	"update-fail"
```

■ 실제동작코드

- Controller

```java
	
	/**
	 * 수정 요청한 정보(param:vo)로 고객의 정보를 업데이트
	 * @Auth를 통해서 고객 정보 비교가 필요함 
	 * @param vo
	 * @param bindResult
	 * @return
	 */
	@ApiOperation(value = "회원정보 업데이트")
    @ApiImplicitParams({
        @ApiImplicitParam(name = "CustomerVo", value = "CustomerVo", dataType = "CustomerVo", paramType = "body"),
    })
	@ResponseBody
	@RequestMapping(value="/{no}", method = RequestMethod.PUT)
	public ResponseEntity<JSONResult> update(@RequestBody @Valid CustomerVo vo,
			BindingResult bindResult) {
		// ### @Valid 통과 불가할 시 error 전달
		if(bindResult.hasErrors()) {
			List<ObjectError> list = bindResult.getAllErrors();
			for(ObjectError error : list) {
				return ResponseEntity.status(HttpStatus.BAD_REQUEST).body(JSONResult.fail(error.getDefaultMessage()));
			}
		}
		Boolean update_result = customerService.update_customer(vo);
		//ResponseBody로 진행 전 비밀번호 표시하지 않기 위하여 공백처리 
		vo.setPassword("");
		if(update_result) {
			return ResponseEntity.status(HttpStatus.OK).body(JSONResult.success(vo));
		}else {
			return ResponseEntity.status(HttpStatus.INTERNAL_SERVER_ERROR).body(JSONResult.fail("update-fail"));
		}
	}
```

- Service

```
/**
* 회원정보 업데이트, 업데이트 완료 시 true
* @param userVo
* @return true, false
*/
public Boolean update_customer(CustomerVo vo) {
return customerDao.update_customer(vo) == 1;
}
```

■ TC CODE

- 준비 코드

```java
package com.cafe24.shop.api;
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.delete;
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.get;
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.post;
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.put;
import static org.springframework.test.web.servlet.result.MockMvcResultHandlers.print;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.jsonPath;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.status;

import java.util.ArrayList;
import java.util.List;

import org.apache.ibatis.session.SqlSession;
import org.junit.Before;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.web.servlet.AutoConfigureMockMvc;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.http.MediaType;
import org.springframework.test.context.junit4.SpringRunner;
import org.springframework.test.web.servlet.MockMvc;
import org.springframework.test.web.servlet.ResultActions;
import org.springframework.test.web.servlet.setup.MockMvcBuilders;
import org.springframework.transaction.annotation.Transactional;
import org.springframework.web.context.WebApplicationContext;

import com.cafe24.shop.controller.api.CustomerController;
import com.cafe24.shop.vo.CustomerVo;
import com.cafe24.shop.vo.TermsOfUseVo;
import com.google.gson.Gson;

@RunWith(SpringRunner.class)
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.MOCK)
@AutoConfigureMockMvc
@Transactional
public class CustomerControllerTest {
	
	@Autowired 
	private CustomerController customerController;
	@Autowired
	private SqlSession sqlSession;
	 
	
//	Test용 데이터 생성(DB)
	List<TermsOfUseVo> termsofuse_list = new ArrayList<TermsOfUseVo>();
	// 조회 및 업데이트를 위한 사전 데이터 삽입
		public List<TermsOfUseVo> test_data_terms(){
			TermsOfUseVo term_vo1 = new TermsOfUseVo(null, "약관동의서1","약관동의내용1", "Y", "Y", null, null);
			TermsOfUseVo term_vo2 = new TermsOfUseVo(null, "약관동의서2","약관동의내용2", "Y", "Y", null, null);
			termsofuse_list.add(term_vo1);
			termsofuse_list.add(term_vo2);
			for(int i=0; i<termsofuse_list.size(); i++) {
				sqlSession.insert("termsofuse.insert", termsofuse_list.get(i));
			}		
			return termsofuse_list;
		}
		
	List<CustomerVo> list = new ArrayList<CustomerVo>();
	public List<CustomerVo> test_data(){
		termsofuse_list = test_data_terms();		
		Long termsofuse_vo1_no = termsofuse_list.get(0).getNo();
		Long termsofuse_vo2_no = termsofuse_list.get(0).getNo();
		CustomerVo vo1 = new CustomerVo(null, "성공테스트", "ksm5318@naver.com", "PASSWORD1!", "010-1234-1234", "M", termsofuse_vo1_no, "Y");
		CustomerVo vo2 = new CustomerVo(null, "성공테스트2", "EMAIL2@TEST.COM", "PASSWORD12!", "010-2222-2222", "M", termsofuse_vo2_no, "Y");
		list.add(vo1);
		list.add(vo2);

		for(int i=0; i<list.size(); i++) {
			sqlSession.insert("customer.insert", list.get(i));
		}
		
		
		return list;
	}
	
	
	
	/**
	 * 기본 mockMvc 설정 및 applicationContext 주입 
	 */
	@Autowired
	private MockMvc mockMvc;
	@Autowired
	private WebApplicationContext webApplicationContext;
	@Before
	public void setup() throws Exception {
		mockMvc = MockMvcBuilders.webAppContextSetup(webApplicationContext)
	                  .alwaysDo(print())
	                  .build();
		list = test_data();
		termsofuse_list = test_data_terms();
	}
```

- 실제 테스트 코드 

```java
/**
	 * update_success_test
	 * : 회원정보 업데이트- 성공테스트
	 * @throws Exception
	 */
@Test
public void update_success_test() throws Exception {	
    // ## update_success_test() 업데이트 성공테스트

    list.get(0).setName("변경이름");
    list.get(0).setPhone("010-5000-4949");

    ResultActions resultActions = 
        mockMvc.perform(put("/api/customer/"+list.get(0).getNo())
                        .contentType(MediaType.APPLICATION_JSON).content(new Gson().toJson(list.get(0))));	

    resultActions
        .andExpect(status().isOk())
        .andExpect(jsonPath("$.result").value("success"))
        .andExpect(jsonPath("$.data.no").value(list.get(0).getNo()))
        .andExpect(jsonPath("$.data.name").value("변경이름"))
        .andExpect(jsonPath("$.data.password").value(""))
        .andExpect(jsonPath("$.data.phone").value("010-5000-4949"))
        .andExpect(jsonPath("$.data.email").value(list.get(0).getEmail()));		
}

/**
	 * update_fail_test
	 * :업데이트 실패, 이름 형식 오류 
	 * @throws Exception
	 */
@Test
public void update_fail_test() throws Exception {	
    // ## update_faill_test() 이름 형식 오류로 인한  업데이트 실패테스트
    CustomerVo vo2 = new CustomerVo(101L, "안ㅏㅈ", "EMAIL@TEST.COM", "PASSWORD1!", "010-1234-1234", "M", 1L, "Y"); 
    ResultActions resultActions2 = 
        mockMvc.perform(put("/api/customer/"+vo2.getNo())
                        .contentType(MediaType.APPLICATION_JSON).content(new Gson().toJson(vo2)));				
    resultActions2
        .andExpect(status().isBadRequest())
        .andExpect(jsonPath("$.result").value("fail"))
        .andExpect(jsonPath("$.message").value("이름 형식이 맞지 않습니다."));	
}
```

