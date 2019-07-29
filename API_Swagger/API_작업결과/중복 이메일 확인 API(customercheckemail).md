###  중복 이메일 확인 API(/customer/checkemail)

■ request

```
GET
  params:
 	path:
      {
      "email": "string",		// 이메일(ID)
      }
```

■ response

```
200 : 성공(중복 email 있을 시)
	email
200 : 성공(중복 email 없을 시)
	"Available_email"
```

■ 실제동작코드

- Controller

```java
@ResponseBody
@RequestMapping(value="/checkemail", method = RequestMethod.GET)
public ResponseEntity<JSONResult> checkEmail(@RequestParam(value="email", required=true, defaultValue="") String email) {
    boolean checked_email = customerService.exist_email(email);
    if(checked_email) { 
        return ResponseEntity.status(HttpStatus.OK).body(JSONResult.success(checked_email));
    }else {
        return ResponseEntity.status(HttpStatus.OK).body(JSONResult.fail("Available_email"));
    }
}
```

- Service

```
/**
* 동일 이메일이 존재하는 경우 true,
* (중복이 되지 않아 사용 가능한 경우)존재하지 않는 경우 false
* @param email
* @return true, false
*/
public Boolean exist_email(String email) {
CustomerVo vo = customerDao.get_customer_by_email(email);
return vo != null;
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
	 * check_email_list_success_test
	 * @throws Exception
	 * :이메일 중복 확인(동일 이메일 존재하는 경우) parameter:{email:ksm5318@naver.com}, MockData:{email:ksm5318@naver.com}
	 */
	@Test
	public void check_email_list_success_test() throws Exception {

		// ## check_email, 동일 이메일 존재하는 경우 테스트
		ResultActions resultActions = 
		mockMvc.perform(get("/api/customer/checkemail?email={email}", "ksm5318@naver.com")
				.contentType(MediaType.APPLICATION_JSON));				
		
		resultActions
		.andExpect(status().isOk())
		.andExpect(jsonPath("$.result").value("success"))
		.andExpect(jsonPath("$.data").value(true));
	}
	
	/**
	 * check_email_list_fail_test
	 * @throws Exception
	 * :이메일 중복 확인(동일 이메일 존재하지 않는 경우), parameter:{email:ksm5318}, MockData:{email:ksm5318@naver.com}
	 */
	@Test
	public void check_email_list_fail_test() throws Exception {

		ResultActions resultActions =
		mockMvc.perform(get("/api/customer/checkemail?email={email}", "ksm5318")
				.contentType(MediaType.APPLICATION_JSON));	

		// ## check_email 실패 테스트
		resultActions
		.andExpect(status().isOk())
		.andExpect(jsonPath("$.result").value("fail"))
		.andExpect(jsonPath("$.message").value("Available_email"));
	}
```

