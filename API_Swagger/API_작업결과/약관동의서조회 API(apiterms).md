###  약관동의서조회 API(/api/terms)

■ request

```
GET
  params:
```

■ response

```
200 : 성공
	List<TermsOfUseVo>(약관동의서 목록)
```

■ 실제동작코드

- Controller

```java
@ResponseBody
@RequestMapping(value="", method = RequestMethod.GET)
public ResponseEntity<JSONResult> get_terms_of_use_template_list() {
    List<TermsOfUseVo> terms_of_use_template_list = termsOfUseVoService.get_terms_of_use_template_list();
    return ResponseEntity.status(HttpStatus.OK).body(JSONResult.success(terms_of_use_template_list));
}
```

- Service

```
public List<TermsOfUseVo> get_terms_of_use_template_list() {
	return termsOfUseVoDao.get_terms_of_use_template_list();
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
import org.springframework.validation.BindingResult;
import org.springframework.web.context.WebApplicationContext;

import com.cafe24.shop.controller.api.TermsOfUseTemplateController;
import com.cafe24.shop.vo.TermsOfUseVo;
import com.google.gson.Gson;
import static org.mockito.Mockito.mock;

@RunWith(SpringRunner.class)
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.MOCK)
@AutoConfigureMockMvc
@Transactional
public class TermsOfUseTemplateControllerTest {
	
	/**
	 * 기본 mockMvc 설정 및 applicationContext 주입 
	 */
	@Autowired
	private MockMvc mockMvc;
	@Autowired
	private WebApplicationContext webApplicationContext;
	@Autowired
	private TermsOfUseTemplateController termsOfUseTemplateController;
	
	//테스터 데이터 생성
	List<TermsOfUseVo> termsofuse_list;
	TermsOfUseVo vo = new TermsOfUseVo(null, "약관동의서1","약관동의내용1", "Y", "Y", null, null);
	TermsOfUseVo vo1 = new TermsOfUseVo(null, "약관동의서2","약관동의내용2", "Y", "Y", null, null);
	// 조회 및 업데이트를 위한 사전 데이터 삽입
	public List<TermsOfUseVo> test_data(){
		BindingResult result = mock(BindingResult.class);
		termsofuse_list = new ArrayList<TermsOfUseVo>();
		termsofuse_list.add(vo);
		termsofuse_list.add(vo1);
		
		for(int i=0; i<termsofuse_list.size(); i++) {
			termsOfUseTemplateController.add_terms_of_use_template(termsofuse_list.get(i), result);
		}
		return termsofuse_list;
	}
	
	@Before
	public void setup() throws Exception {
		mockMvc = MockMvcBuilders.webAppContextSetup(webApplicationContext)
	                  .alwaysDo(print())
	                  .build();	
		test_data();
	}
```

- 실제 테스트 코드 

```java
/**
	 * 회원약관동의서 조회
	 * @throws Exception
	 */
@Test
public void get_terms_of_use_template_list_success_test() throws Exception {
    //test data 임시 주입 및 비교 		
    ResultActions resultActions = 
        mockMvc.perform(get("/api/terms")
                        .contentType(MediaType.APPLICATION_JSON));				
    resultActions
        .andExpect(status().isOk())
        .andExpect(jsonPath("$.result").value("success"));

    for(int i=0; i<termsofuse_list.size(); i++) {
        resultActions
            .andExpect(jsonPath("$.data["+i+"].no").value(termsofuse_list.get(i).getNo()))
            .andExpect(jsonPath("$.data["+i+"].title").value(termsofuse_list.get(i).getTitle()))
            .andExpect(jsonPath("$.data["+i+"].content").value(termsofuse_list.get(i).getContent()));
    }
}
```

