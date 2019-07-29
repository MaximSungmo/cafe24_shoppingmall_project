### 카테고리추가 API(/api/category)

■ request

```
POST
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
201 : 생성
	true
400 : 실패 (parent_no 참조 시 없는 경우)
	"상위 카테고리 정보 오류가 발생하였습니다."
```

■ 실제동작코드

- Controller

```java
@ResponseBody
@RequestMapping(value="", method = RequestMethod.POST)
public ResponseEntity<JSONResult> add_category(@RequestBody @Valid CategoryVo vo, BindingResult bindResult) {	
    if(bindResult.hasErrors()) {
        List<ObjectError> list = bindResult.getAllErrors();
        for(ObjectError error : list) {
            return ResponseEntity.status(HttpStatus.BAD_REQUEST).body(JSONResult.fail(error.getDefaultMessage()));
        }
    }
    // parent_no에 입력된 카테고리 번호는 기존에 존재하는 카테고리 번호가 맞는 지 확인 (번호가 있는 경우(true)에만 insert 작업 진행 
    Boolean check_available_parent_no = categoryService.get_category_by_no(vo.getParent_no());
    if(vo.getParent_no()==null || check_available_parent_no) {
        Boolean insert_result = categoryService.insert_category(vo);
        return ResponseEntity.status(HttpStatus.CREATED).body(JSONResult.success(insert_result));			
    }else {
        return ResponseEntity.status(HttpStatus.BAD_REQUEST).body(JSONResult.fail("상위 카테고리 정보 오류가 발생하였습니다."));
    }
}
```

- Service

```
public Boolean insert_category(CategoryVo vo) {
	return categoryDao.insert_category(vo)==1;
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
import org.junit.Ignore;
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

import com.cafe24.shop.vo.CategoryVo;
import com.google.gson.Gson;

@RunWith(SpringRunner.class)
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.MOCK)
@AutoConfigureMockMvc
@Transactional
public class CategoryControllerTest {
	

	@Autowired
	SqlSession sqlSession;
	/*
	 * 임시 테스트 데이터
	 */
	List<CategoryVo> list = new ArrayList<CategoryVo>();
	CategoryVo vo1 = new CategoryVo(null, "1번카테고리", null);
	CategoryVo vo2 = new CategoryVo(null, "2번카테고리", null);
	CategoryVo vo3 = new CategoryVo(null, "3번카테고리", null);
	CategoryVo vo4 = new CategoryVo(null, "4번카테고리", null);
	public List<CategoryVo> testData(){
//		sqlSession.delete("category.deleteAll");
		
		//	Test용 데이터 생성(DB)
		list.add(vo1);		
		list.add(vo2);
		list.add(vo3);
		list.add(vo4);
		
		sqlSession.insert("category.insert", list.get(0));
		vo2.setParent_no(list.get(0).getNo());
		sqlSession.insert("category.insert", list.get(1));
		vo3.setParent_no(list.get(1).getNo());
		sqlSession.insert("category.insert", list.get(2));
		vo4.setParent_no(list.get(1).getNo());
		sqlSession.insert("category.insert", list.get(3));	
		return list;
	}
	
	
	@Autowired
	private MockMvc mockMvc;
	@Autowired
	private WebApplicationContext webApplicationContext;
	
	
	@Before
	public void setup() throws Exception {
		mockMvc = MockMvcBuilders.webAppContextSetup(webApplicationContext)
	                  .alwaysDo(print())
	                  .build();
		list=testData();
		System.out.println(list);
	}
```

- 실제 테스트 코드 

```java
/**
	 * add_category_test();
	 * @throws Exception
	 * :카테고리 추가하기 (성공테스트, 중복값으로 인한 실패테스트)
	 */
@Test
public void add_category_success_test() throws Exception {
    CategoryVo vo = new CategoryVo(null, "1번카테고리", null);

    // ## add_category() 성공 테스트
    ResultActions resultActions = 
        mockMvc.perform(post("/api/category")
                        .contentType(MediaType.APPLICATION_JSON).content(new Gson().toJson(vo)));				

    resultActions
        .andExpect(status().isCreated())
        .andExpect(jsonPath("$.result").value("success"))
        .andExpect(jsonPath("$.data").value(true));	
}

@Test
public void add_category_fail_test() throws Exception {
    CategoryVo vo = new CategoryVo(null, "오류 카테고리", 999999L);

    // ## add_category() 실패 테스트 - 상위카테고리 키 FK 참조 오류
    ResultActions resultActions =
        mockMvc.perform(post("/api/category")
                        .contentType(MediaType.APPLICATION_JSON).content(new Gson().toJson(vo)));	
    resultActions
        .andExpect(status().isBadRequest())
        .andExpect(jsonPath("$.result").value("fail"))
        .andExpect(jsonPath("$.message").value("상위 카테고리 정보 오류가 발생하였습니다."));			
}
```

