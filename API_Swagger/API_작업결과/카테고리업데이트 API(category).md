###  카테고리업데이트 API(/category)

■ request

```
PUT
  params:
  	body:
  	{
  		"name": "string",		// 카테고리명
  		"no": 0,				// 카테고리 번호
  		"parent_no": 0,			// 상위 카테고리 번호
  	}
```

■ response

```
200 : 성공
	CategoryVo(카테고리 정보)
400 : 잘못된 요청 
	입력 에러 메세지
400 : 잘못된 요청 
	"상위 카테고리 정보 오류가 발생하였습니다."
```

■ 실제동작코드

- Controller

```java
@ResponseBody
@RequestMapping(value="", method = RequestMethod.PUT)
public ResponseEntity<JSONResult> update_category(@RequestBody @Valid CategoryVo vo, BindingResult bindResult) {

    if(bindResult.hasErrors()) {
        List<ObjectError> list = bindResult.getAllErrors();
        for(ObjectError error : list) {
            return ResponseEntity.status(HttpStatus.BAD_REQUEST).body(JSONResult.fail(error.getDefaultMessage()));
        }
    }
    Boolean check_available_parent_no = categoryService.get_category_by_no(vo.getParent_no());
    if(vo.getParent_no()==null || check_available_parent_no) {
        categoryService.update_category(vo);
        return ResponseEntity.status(HttpStatus.OK).body(JSONResult.success(vo));
    }else {
        return ResponseEntity.status(HttpStatus.BAD_REQUEST).body(JSONResult.fail("상위 카테고리 정보 오류가 발생하였습니다."));
    }
}
```

- Service

```
public Boolean update_category(CategoryVo vo) {
	return categoryDao.update_category(vo)==1;
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
	 * update_category_success_test() 
	 * @throws Exception
	 * :카테고리 업데이트.
	 */
@Test
public void update_category_success_test() throws Exception {		
    // ## get_category_list() 성공 테스트
    ResultActions resultActions = 
        mockMvc.perform(put("/api/category")
                        .contentType(MediaType.APPLICATION_JSON).content(new Gson().toJson(list.get(0))));				

    resultActions
        .andExpect(jsonPath("$.result").value("success"))
        .andExpect(jsonPath("$.data.no").value(list.get(0).getNo()))
        .andExpect(jsonPath("$.data.name").value(list.get(0).getName()))
        .andExpect(jsonPath("$.data.parent_no").value(list.get(0).getParent_no()));
}

/**
	 * update_category_success_test() 
	 * @throws Exception
	 * :카테고리 업데이트.
	 */
@Test
public void update_category_fail_test() throws Exception {		
    list.get(0).setParent_no(999999L);
    // ## get_category_list() 성공 테스트
    ResultActions resultActions = 
        mockMvc.perform(put("/api/category")
                        .contentType(MediaType.APPLICATION_JSON).content(new Gson().toJson(list.get(0))));				

    resultActions
        .andExpect(status().isBadRequest())
        .andExpect(jsonPath("$.result").value("fail"))
        .andExpect(jsonPath("$.message").value("상위 카테고리 정보 오류가 발생하였습니다."));	
}
```

