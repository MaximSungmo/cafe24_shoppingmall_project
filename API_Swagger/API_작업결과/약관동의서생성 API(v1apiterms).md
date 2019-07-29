###  약관동의서생성 API(/api/terms)

■ request

```
POST
  params:
  	body:
    	{
          "content": "string",		// 약관동의서 내용
          "necessary_fl": "string",	// 약관동의서 필수 유무
          "no": 0,				   // 약관동의서 번호
          "title": "string",	    // 약관동의서 제목
          "use_fl": "string"		// 약관동의서 사용 유무
        }
```

■ response

```
200 : 성공
	Terms_of_use_vo(약관동의서)
400 : 잘못된 요청 
	입력 에러 메세지 
500 : 내부 서버 에러
	"Server Error"
```

■ 실제동작코드

- Controller

```java
@ResponseBody
@RequestMapping(value="", method = RequestMethod.POST)
public ResponseEntity<JSONResult> add_terms_of_use_template(
    @RequestBody @Valid TermsOfUseVo vo,
    BindingResult bindResult) {
    // ### @Valid 통과 불가할 시 error 전달
    if(bindResult.hasErrors()) {
        List<ObjectError> list = bindResult.getAllErrors();
        for(ObjectError error : list) {
            return ResponseEntity.status(HttpStatus.BAD_REQUEST).body(JSONResult.fail(error.getDefaultMessage()));
        }
    }
    Boolean insert_result = termsOfUseVoService.insert_terms_of_use_template(vo);
    if(insert_result) {
        return ResponseEntity.status(HttpStatus.CREATED).body(JSONResult.success(vo));
    }else {
        return ResponseEntity.status(HttpStatus.INTERNAL_SERVER_ERROR).body(JSONResult.fail("Server Error"));
    }	
}
```

- Service

```
public Boolean insert_terms_of_use_template(TermsOfUseVo vo) {
	return termsOfUseVoDao.insert_terms_of_use_template(vo) == 1;
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
	 * 회원약관동의서 생성
	 * @throws Exception
	 */
@Test
public void add_terms_of_use_template_success_test() throws Exception {

    ResultActions resultActions = 
        mockMvc.perform(post("/api/terms")
                        .contentType(MediaType.APPLICATION_JSON).content(new Gson().toJson(vo)));				
    resultActions
        .andExpect(status().isCreated())
        .andExpect(jsonPath("$.result").value("success"))
        .andExpect(jsonPath("$.data.title").value(vo.getTitle()))
        .andExpect(jsonPath("$.data.content").value(vo.getContent()))		
        .andExpect(jsonPath("$.data.use_fl").value(vo.getUse_fl()))
        .andExpect(jsonPath("$.data.necessary_fl").value(vo.getNecessary_fl()));		
}	
```

