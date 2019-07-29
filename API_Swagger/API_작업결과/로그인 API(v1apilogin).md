### 로그인요청 API(/v1/api/customer/login)

■ request

```
POST
  params:
	body:
      {
      "email": "string",		// 이메일(ID)
      "password": "string",		// 비밀번호
      }
```

■ response

```
200 : 성공
	customer_vo(회원정보리스트)
400 : 실패
	입력정보 오류 내용
```

■ 실제동작코드

- Controller

```java
@ResponseBody
@RequestMapping(value="/login", method = RequestMethod.POST)
public ResponseEntity<JSONResult> login(@RequestBody CustomerVo vo) {
    CustomerVo customer_vo = customerService.login(vo);

    Validator validator = Validation.buildDefaultValidatorFactory().getValidator();
    Set<ConstraintViolation<CustomerVo>> validatorResults =
        validator.validateProperty(vo, "email");
    if( validatorResults.isEmpty()== false) {
        for(ConstraintViolation<CustomerVo> validatorResult : validatorResults ) {
            //				String message = validatorResult.getMessage();
            String message = messageSource.getMessage("Email.customerVo.email", null, LocaleContextHolder.getLocale());
            JSONResult result = JSONResult.fail(message);
            return ResponseEntity.status(HttpStatus.BAD_REQUEST).body(result);
        }
    }

    return ResponseEntity.status(HttpStatus.OK).body(JSONResult.success(customer_vo));
}
```

- Service

```
public CustomerVo login(CustomerVo vo) {
	return customerDao.login(vo);
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
	 * login_success_test()
	 * :로그인 성공
	 * @throws Exception
	 */
	@Test
	public void login_success_test() throws Exception {	
		// ## login_success_test() 업데이트 성공테스트
		ResultActions resultActions = 
		mockMvc.perform(post("/api/customer/login")
				.contentType(MediaType.APPLICATION_JSON).content(new Gson().toJson(list.get(0))));				
		resultActions
		.andExpect(status().isOk())
		.andExpect(jsonPath("$.result").value("success"));
	}
	
	/**
	 * login_fail_test()
	 * :로그인 실패 
	 * @throws Exception
	 */
	@Test
	public void login_fail_test() throws Exception {	
		// ## login_fail_test() 업데이트 성공테스트
		// 패스워드 형식 오류 
		CustomerVo vo1 = new CustomerVo("EMAIL@TEST.COM", "PASSWORD"); 
		ResultActions resultActions = 
		mockMvc.perform(post("/api/customer/login")
				.contentType(MediaType.APPLICATION_JSON).content(new Gson().toJson(vo1)));				
		resultActions
		.andExpect(status().isOk())
		.andExpect(jsonPath("$.result").value("success"))
		.andExpect(jsonPath("$.data").value(vo1.getNo()));	
		
		// ## login_fail_test() 업데이트 성공테스트
		// 이메일 형식 오류 
		CustomerVo vo2 = new CustomerVo("EMAILTEST.COM", "PASSWORD"); 
		ResultActions resultActions2 = 
		mockMvc.perform(post("/api/customer/login")
				.contentType(MediaType.APPLICATION_JSON).content(new Gson().toJson(vo2)));				
		resultActions2
		.andExpect(status().isBadRequest())
		.andExpect(jsonPath("$.result").value("fail"))
		.andExpect(jsonPath("$.message").value("이메일 형식이 올바르지 않습니다."));	
	}
```