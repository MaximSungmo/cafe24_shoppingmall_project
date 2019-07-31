### 상품 상세정보  삭제API(/api/{product_no}/detail/{product_detail_no})

■ request

```
DELETE
  params:
  	path:
        {
          "product_detail_no": 0,		// 상품상세정보 번호   
        }
```

■ response

```
200 : 성공
	"product_detail_no" 				// 삭제된 상품상세정보 번호
```

■ 실제동작코드

- Controller

```java
@ResponseBody
@RequestMapping(value = { "/{product_no}/detail/{product_detail_no}" }, method = RequestMethod.DELETE)
public ResponseEntity<JSONResult> delete_product_detail(
    @PathVariable(value="product_no") Long product_no,
    @PathVariable(value="product_detail_no") Long product_detail_no) {

    productService.delete_product_detail(product_no, product_detail_no);
    return ResponseEntity.status(HttpStatus.OK).body(JSONResult.success(product_detail_no));
}
```

- Service

```
public Boolean delete_product_detail(Long product_no, Long product_detail_no) {
    Map<String, Long> map = new HashMap<String, Long>();
    map.put("product_no", product_no);
    map.put("product_detail_no", product_detail_no);
    return productDao.delete_product_detail(map)==1;
}
```

■ TC CODE

- 준비 코드

```java
import static org.hamcrest.Matchers.hasSize;
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.delete;
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.get;
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.post;
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.put;
import static org.springframework.test.web.servlet.result.MockMvcResultHandlers.print;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.jsonPath;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.status;

import java.util.ArrayList;
import java.util.Collections;
import java.util.Comparator;
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
import com.cafe24.shop.vo.ProductDetailVo;
import com.cafe24.shop.vo.ProductImageCategoryVo;
import com.cafe24.shop.vo.ProductImageVo;
import com.cafe24.shop.vo.ProductVo;
import com.google.gson.Gson;

@RunWith(SpringRunner.class)
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.MOCK)
@AutoConfigureMockMvc
@Transactional
public class ProductControllerTest {
	
	/*
	 * 테스트 데이터 생성
	 */
	@Autowired
	private SqlSession sqlSession;
	List<ProductVo> product_list = new ArrayList<ProductVo>();
	List<ProductDetailVo> detail_list = new ArrayList<ProductDetailVo>();
	List<ProductImageVo> image_list = new ArrayList<ProductImageVo>();

	CategoryVo category_vo1;
	CategoryVo category_vo2;
	@Transactional
	public List<ProductVo> test_data() {
		//테스트용 데이터 생성
		category_vo1 = new CategoryVo(null, "임시카테고리1", null);
		category_vo2 = new CategoryVo(null, "임시카테고리2", null);

		sqlSession.insert("category.insert", category_vo1);
		sqlSession.insert("category.insert", category_vo2);

		ProductVo vo1 = new ProductVo(null, "상품1", "상품1상세내용", "상태1", "Y", 1L, null, category_vo1.getNo(), null);
		ProductVo vo2 = new ProductVo(null, "상품2", "상품2상세내용", "상태2", "Y", 1L, null, category_vo1.getNo(), null);
		ProductVo vo3 = new ProductVo(null, "상품3", "상품3상세내용", "상태3", "Y", 1L, null, category_vo2.getNo(), null);
		ProductVo vo4 = new ProductVo(null, "상품4", "상품4상세내용", "상태4", "N", 1L, null, category_vo2.getNo(), null);
		product_list.add(vo1);
		product_list.add(vo2);
		product_list.add(vo3);
		product_list.add(vo4);
		
		for(int i=0; i<product_list.size(); i++) {
			sqlSession.insert("product.insert_product", product_list.get(i));
		}
		return product_list;
	}
	
	public List<ProductDetailVo> product_detail_test_data_() {
		//테스트용 데이터 생성
		Long product_vo1_no = product_list.get(1).getNo();
		Long product_vo2_no = product_list.get(2).getNo();
		Long product_vo3_no = product_list.get(3).getNo();

		ProductDetailVo detail_vo1 = new ProductDetailVo(501L, product_vo1_no, "사이즈270-1", 100L, "STOCK", 40L, null);
		ProductDetailVo detail_vo2 = new ProductDetailVo(502L, product_vo2_no, "사이즈270-2", 200L, "STOCK", 40L, null);
		ProductDetailVo detail_vo3 = new ProductDetailVo(503L, product_vo3_no, "사이즈270-3", 300L, "STOCK", 40L, null);
		detail_list.add(detail_vo1);
		detail_list.add(detail_vo2);
		detail_list.add(detail_vo3);
		sqlSession.insert("product.insert_product_detail", detail_list);
		return detail_list;
	}
	
	@Transactional
	public List<ProductImageVo> product_image_test_data(){
		
		// product_image_caregory 등록 
		ProductImageCategoryVo productImageCategoryVo1 = new ProductImageCategoryVo(null, "메인화면이미지1", null, null, "Y");
		ProductImageCategoryVo productImageCategoryVo2 = new ProductImageCategoryVo(null, "메인화면이미지2", null, null, "Y");
		ProductImageCategoryVo productImageCategoryVo3 = new ProductImageCategoryVo(null, "메인화면이미지3", null, null, "Y");
		sqlSession.insert("product.insert_product_image_category", productImageCategoryVo1);
		sqlSession.insert("product.insert_product_image_category", productImageCategoryVo2);
		sqlSession.insert("product.insert_product_image_category", productImageCategoryVo3);

		
		Long product_vo1_no = product_list.get(1).getNo();
		Long product_vo2_no = product_list.get(2).getNo();
		Long product_vo3_no = product_list.get(3).getNo();
		
		
		
		ProductImageVo image_vo1_1 = new ProductImageVo(101L, product_vo1_no, "URL 1-1", null, "Y", productImageCategoryVo1.getNo());
		ProductImageVo image_vo1_2 = new ProductImageVo(102L, product_vo1_no, "URL 1-2", null, "Y", productImageCategoryVo2.getNo());
		ProductImageVo image_vo1_3 = new ProductImageVo(103L, product_vo1_no, "URL 1-3", null, "Y", productImageCategoryVo3.getNo());
		image_list.add(image_vo1_1);
		image_list.add(image_vo1_2);
		image_list.add(image_vo1_3);
		
		ProductImageVo image_vo2_1 = new ProductImageVo(104L, product_vo2_no, "URL 2-1", null, "Y", productImageCategoryVo1.getNo());
		ProductImageVo image_vo2_2 = new ProductImageVo(105L, product_vo2_no, "URL 2-2", null, "Y", productImageCategoryVo2.getNo());
		ProductImageVo image_vo2_3 = new ProductImageVo(106L, product_vo2_no, "URL 2-3", null, "Y", productImageCategoryVo3.getNo());
		image_list.add(image_vo2_1);
		image_list.add(image_vo2_2);
		image_list.add(image_vo2_3);
		
		ProductImageVo image_vo3_1 = new ProductImageVo(107L, product_vo3_no, "URL 3-1", null, "Y", productImageCategoryVo1.getNo());
		ProductImageVo image_vo3_2 = new ProductImageVo(108L, product_vo3_no, "URL 3-2", null, "Y", productImageCategoryVo2.getNo());
		ProductImageVo image_vo3_3 = new ProductImageVo(109L, product_vo3_no, "URL 3-3", null, "Y", productImageCategoryVo3.getNo());
		image_list.add(image_vo3_1);
		image_list.add(image_vo3_2);
		image_list.add(image_vo3_3);
		
		sqlSession.insert("product.insert_product_image_list", image_list);
		return image_list;
	}
	
		
	@Autowired
	private MockMvc mockMvc;
	@Autowired
	private WebApplicationContext webApplicationContext;

	@Before
	public void setup() throws Exception {
		mockMvc = MockMvcBuilders.webAppContextSetup(webApplicationContext).alwaysDo(print()).build();
		
		product_list = test_data();
		detail_list = product_detail_test_data_();
		image_list = product_image_test_data();
	}
```

- 실제 테스트 코드 

```java
@Test
public void delete_product_detail_success_test() throws Exception {

    ResultActions resultActions = mockMvc
        .perform(delete("/api/product/"
                        +detail_list.get(0).getProduct_no()
                        +"/detail/"+detail_list.get(0).getNo())
                 .contentType(MediaType.APPLICATION_JSON));

    resultActions
        .andExpect(status().isOk())
        .andExpect(jsonPath("$.result").value("success"));
}
```

