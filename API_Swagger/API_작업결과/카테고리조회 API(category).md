### 카테고리조회 API(/category)

■ request

```
GET
  params:
```

■ response

```
200 : 성공
	List<CategoryVo> (카테고리 목록)
```

■ 실제동작코드

- Controller

```java
@ResponseBody
@RequestMapping(value="", method = RequestMethod.GET)
public ResponseEntity<JSONResult> get_category_list() {
    List<CategoryVo> list = categoryService.get_category_list();
    return ResponseEntity.status(HttpStatus.OK).body(JSONResult.success(list));
}
```

- Service

```
public List<CategoryVo> get_category_list() {
	return categoryDao.get_category_list();
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
	 * get_category_list_test() 
	 * @throws Exception
	 * :카테고리 목록 가져오기.
	 */
@Test
public void get_category_list_test() throws Exception {
    // ## get_category_list() 성공 테스트
    ResultActions resultActions = 
        mockMvc.perform(get("/api/category")
                        .contentType(MediaType.APPLICATION_JSON));				

    resultActions
        .andExpect(jsonPath("$.result").value("success"))
        .andExpect(jsonPath("$.data[0].no").value(list.get(0).getNo()))	
        .andExpect(jsonPath("$.data[0].name").value(list.get(0).getName()))
        .andExpect(jsonPath("$.data[1].no").value(list.get(1).getNo()))	
        .andExpect(jsonPath("$.data[1].name").value(list.get(1).getName()))
        .andExpect(jsonPath("$.data[1].parent_no").value(list.get(1).getParent_no()))
        .andExpect(jsonPath("$.data[2].no").value(list.get(2).getNo()))	
        .andExpect(jsonPath("$.data[2].name").value(list.get(2).getName()))
        .andExpect(jsonPath("$.data[2].parent_no").value(list.get(2).getParent_no()))
        .andExpect(jsonPath("$.data[3].no").value(list.get(3).getNo()))	
        .andExpect(jsonPath("$.data[3].name").value(list.get(3).getName()))
        .andExpect(jsonPath("$.data[3].parent_no").value(list.get(3).getParent_no()));
}
```

