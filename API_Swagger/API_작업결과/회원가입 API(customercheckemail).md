###  회원가입 API(/api/customer)

■ request

```
POST
  params:
  	body:
      {
      "agreement_dt": "string", // 약관동의서 동의 날짜
      "agreement_fl": "string",	// 약관동의서 동의 유무
      "auth_grade": "string",	// 승인등급
      "email": "string",		// 이메일(ID)
      "gender": "string",		// 성별
      "name": "string",			// 이름
      "no": 0,					// 번호 
      "password": "string",		// 비밀번호
      "phone": "string",		// 핸드폰 번호
      "recommender_id": "string",// 추천인 아이디
      "terms_of_use_no": 0,		// 약관동의서 번호
      "use_fl": "string"		// 아이디 사용 유무
      }
```

■ response

```
201 : 생성
	true
500 : 내부 서버 에러
	"Server Error"
```

■ 실제동작코드

- Controller

```java
@ResponseBody
@RequestMapping(value="", method = RequestMethod.POST)
public ResponseEntity<JSONResult> join(
    @RequestBody @Valid CustomerVo vo, BindingResult bindResult) {
    // ### @Valid 통과 불가할 시 error 전달
    if(bindResult.hasErrors()) {
        List<ObjectError> list = bindResult.getAllErrors();
        for(ObjectError error : list) {
            return ResponseEntity.status(HttpStatus.BAD_REQUEST).body(JSONResult.fail(error.getDefaultMessage()));
        }
    }
    System.out.println(vo);
    // 데이터가 정상적으로 DB에 입력이 되면 true 값을 반환한다. 
    Boolean insert_result = customerService.join(vo);
    if(insert_result) {
        return ResponseEntity.status(HttpStatus.CREATED).body(JSONResult.success(insert_result));
    }else {
        return ResponseEntity.status(HttpStatus.INTERNAL_SERVER_ERROR).body(JSONResult.fail("Server Error"));
    }		
}
```

- Service

```
/**
* 회원가입 시 유저 정보를 받아서 insert하여 줌 
* @param userVo
* @return true, false
*/
@Transactional
public Boolean join(CustomerVo vo) {
customerDao.insert_checked_terms_of_use(vo);
return customerDao.insert(vo);
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
	 *  join_success_test
	 *  @throws Exception
	 *  :회원가입, request body : {uservo:uservo}, 
	 *  정상적으로 가입완료 시 result : success, data : vo.getEmail(); 
	 */
	@Test
	public void join_success_test() throws Exception {		
		// ## join_test() 성공 테스트
		//success 
		CustomerVo join_vo = list.get(0);
		ResultActions resultActions = 
		mockMvc.perform(post("/api/customer/")
				.contentType(MediaType.APPLICATION_JSON).content(new Gson().toJson(join_vo)));				
		resultActions
		.andExpect(status().isCreated()) 
		.andExpect(jsonPath("$.result").value("success"))
		.andExpect(jsonPath("$.data").value(true));
	}
	
	/**
	 * join_validate_fail_test
	 * :@Valid에 의한 회원가입 입력데이터 validation 
	 * @throws Exception
	 */
	@Test
	public void join_validate_fail_test() throws Exception {	
		// ## join_test() 실패테스트
		// 이름 형식 오류 
		CustomerVo vo2 = new CustomerVo(101L, "안ㅏㅈ", "EMAIL@TEST.COM", "PASSWORD1!", "010-1234-1234", "M", 1L, "Y"); 
		ResultActions resultActions2 = 
		mockMvc.perform(post("/api/customer/")
				.contentType(MediaType.APPLICATION_JSON).content(new Gson().toJson(vo2)));				
		resultActions2
		.andExpect(status().isBadRequest())
		.andExpect(jsonPath("$.result").value("fail"))
		.andExpect(jsonPath("$.message").value("이름 형식이 맞지 않습니다."));
		
		// ## join_test() 실패테스트
		// 이름 길이 오류 
		CustomerVo vo3 = new CustomerVo(101L, "안", "EMAIL@TEST.COM", "PASSWORD1!", "010-1234-1234", "M", 1L, "Y"); 
		ResultActions resultActions3 = 
		mockMvc.perform(post("/api/customer/")
				.contentType(MediaType.APPLICATION_JSON).content(new Gson().toJson(vo3)));				
		resultActions3
		.andExpect(status().isBadRequest())
		.andExpect(jsonPath("$.result").value("fail"))
		.andExpect(jsonPath("$.message").value("이름은 2~8자리 사이로 입력해주세요."));
		
		
		// ## join_test() 실패테스트
		// 이메일 형식 오류 
		CustomerVo vo4 = new CustomerVo(101L, "실패테스트", "EMAILTEST.COM", "PASSWORD1!", "010-1234-1234", "M", 1L, "Y"); 
		ResultActions resultActions4 = 
		mockMvc.perform(post("/api/customer/")
				.contentType(MediaType.APPLICATION_JSON).content(new Gson().toJson(vo4)));				
		resultActions4
		.andExpect(status().isBadRequest())
		.andExpect(jsonPath("$.result").value("fail"))
		.andExpect(jsonPath("$.message").value("이메일 형식이 올바르지 않습니다."));
		
		
		// ## join_test() 실패테스트
		// 비밀번호 형식 오류 
		CustomerVo vo5 = new CustomerVo(101L, "실패테스트", "EMAIL@TEST.COM", "PASSWORD11", "010-1234-1234", "M", 1L, "Y"); 
		ResultActions resultActions5 = 
		mockMvc.perform(post("/api/customer/")
				.contentType(MediaType.APPLICATION_JSON).content(new Gson().toJson(vo5)));				
		resultActions5
		.andExpect(status().isBadRequest())
		.andExpect(jsonPath("$.result").value("fail"))
		.andExpect(jsonPath("$.message").value("비밀번호는 특수문자/문자/숫자 최소 8~16자리 이내 입력 바랍니다."));

		
		// ## join_test() 실패테스트
		// 비밀번호 길이 오류 
		CustomerVo vo6 = new CustomerVo(101L, "실패테스트", "EMAIL@TEST.COM", "PASSWOR", "010-1234-1234", "M", 1L, "Y"); 
		ResultActions resultActions6 = 
		mockMvc.perform(post("/api/customer/")
				.contentType(MediaType.APPLICATION_JSON).content(new Gson().toJson(vo6)));				
		resultActions6
		.andExpect(status().isBadRequest())
		.andExpect(jsonPath("$.result").value("fail"))
		.andExpect(jsonPath("$.message").value("비밀번호는 특수문자/문자/숫자 최소 8~16자리 이내 입력 바랍니다."));
		
		
		// ## join_test() 실패테스트
		// 전화번호 형식 오류 
		CustomerVo vo7 = new CustomerVo(101L, "실패테스트", "EMAIL@TEST.COM", "PASSWORD1!", "0101234-1234", "M", 1L, "Y"); 
		ResultActions resultActions7 = 
		mockMvc.perform(post("/api/customer/")
				.contentType(MediaType.APPLICATION_JSON).content(new Gson().toJson(vo7)));				
		resultActions7
		.andExpect(status().isBadRequest())
		.andExpect(jsonPath("$.result").value("fail"))
		.andExpect(jsonPath("$.message").value("전화번호 형식이 올바르지 않습니다."));
		
		// ## join_test() 실패테스트
		// 성별 형식 오류 
		CustomerVo vo8 = new CustomerVo(101L, "실패테스트", "EMAIL@TEST.COM", "PASSWORD1!", "010-1234-1234", "s", 1L, "Y"); 
		ResultActions resultActions8 = 
		mockMvc.perform(post("/api/customer/")
				.contentType(MediaType.APPLICATION_JSON).content(new Gson().toJson(vo8)));				
		resultActions8
		.andExpect(status().isBadRequest())
		.andExpect(jsonPath("$.result").value("fail"))
		.andExpect(jsonPath("$.message").value("Invalid Gender"));
	}
```

