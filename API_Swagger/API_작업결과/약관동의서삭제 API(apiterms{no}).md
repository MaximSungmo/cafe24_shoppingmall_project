###  약관동의서삭제 API(/api/terms/{no})

■ request

```
DELETE
  params:
  	path:
    	{          
          "no": 0,				   // 약관동의서 번호
        }
```

■ response

```
200 : 성공
	no(약관동의서번호)
```

■ 실제동작코드

- Controller

```java
@ResponseBody
@RequestMapping(value="/{no}", method = RequestMethod.DELETE)
public ResponseEntity<JSONResult> delete_terms_of_use(@PathVariable Long no) {
    termsOfUseVoService.delete_terms_of_use_template(no);
    return ResponseEntity.status(HttpStatus.OK).body(JSONResult.success(no));
}
```

- Service

```
public Boolean delete_terms_of_use_template(Long no) {
	return termsOfUseVoDao.delete_terms_of_use_template(no) == 1;
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
	 * 회원약관동의서 삭제(use_fl = 'N')
	 * @throws Exception
	 */
@Test
public void delete_terms_of_use_template_success_test() throws Exception {
    ResultActions resultActions = 
        mockMvc.perform(delete("/api/terms/"+termsofuse_list.get(0).getNo())
                        .contentType(MediaType.APPLICATION_JSON));				
    resultActions
        .andExpect(status().isOk())
        .andExpect(jsonPath("$.result").value("success"))
        .andExpect(jsonPath("$.data").value(termsofuse_list.get(0).getNo()));	
}
```

