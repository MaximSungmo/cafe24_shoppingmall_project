작성일자 : 2019-08-01

작성자 : 김성모 



## 1. 서론

카페24 신입 입사자 교육으로 쇼핑몰 구축하는 과제를 수행하게 되었다. 앞으로 추가 작업할 게 많지만 전체적인 내용을 작성하여 실무 투입 전에 역량을 최대로 끌어내야겠다는 생각을 한다. 추가 작업되는 내용들은 기존 내용에 더해가며 정리하도록 하겠다. 

우선적으로 주어진 과제는 2019년 7월 1일부터 7월 12일까지 프로젝트 계획, 요구사항 분석, 설계를 진행하였다. 처음으로 진행해보는 작업이므로 어느 정도의 시간이 걸릴 지, 어느 정도의 수준으로 구현을 할 지, 주어진 시간 내 실현이 가능할 수준인 지 판단을 해야만 했다. 결론적으로 말하자면 첫 시작 단계에서 많은 부분을 제대로 파악하지 못하였고 7월 12일 이 후로도 여러 차례 시나리오와 DB가 변경되어 예상했던 기간을 초과하여 작업을 진행하게 되었다. 제대로 파악하지 못한 부분과 이슈를 해결하는 노력은 뒤에서 다루도록 하겠다. 

프로젝트를 위한 전체적인 일정을 파악한 뒤 개발이 필요한 부분에 대하여 구체적인 내용을 뽑아내었다. 이 후 7월 15일부터 7월 26일(실제로는 8월 1일)까지는 개발 진행을 하게 되었다., 앞에서 파악된 일정을 가지고 공동 작업에서 본인이 계획한 일정을 관리하고 공동 작업자들과 공유할 지를 직접 개발 일정을 관리하며 기한에 맞도록 개발하는 것이 관건이였다. 실제 개발을 진행하며 일정을 관리할 때 일정 궤도에 오르기까지 준비 작업에서 많은 시간을 소비하였다. 예를 들어 처음 해보는 Test case 작성과 REST API 작성을 하는 것, 관련된 내용을 학습하고 이해하는 단계에서 상당한 시간이 소요되었다. 기존에는 클라이언트와 서버를 나누지 않고 웹 서비스를 개발했던 것과는 다르게 API 서버를 구성하여 클라이언트를 붙일 수 있도록 만드는 작업으로 방향이 제시되었으며 이에 따라 초반에 잘못 생각한 부분을 수정하면서 이미 작성된 코드를 새롭게 작성하는 일도 빈번하였다. 

현재 프로젝트가 마무리가 된 상황이 아니고 정확히 중간 단계인 API 서버를 만드는 것이 끝난 것이지만 지금까지 프로젝트를 통해서 내가 겪었던 문제점과 개발 일정을 산정함에 있어서 유의해야 할 점들을 작성하여 실무에서 일정을 효율적으로 산정하고 나에게 주어진 과업(task)와 문제(Issue)를 공유하는 방법에 대하여 시행착오를 통해 배울 수 있었다.





## 2. 본론





 

USECASE Diagram, 



Issue

1. 기획 및 설계



2. 일정 관리 



3. 개발 및 














1. 3주차 과제(2019-07-15 ~ 2019-07-19), 4주차 과제(2019-07-22 ~ 2019-07-26)

- 3, 4주차에서는 개발 속도와 실제 시나리오를 구현하는 것
- API 백엔드 개발에서 1만~2만 LINE의 결과물이 나올 수 있도록 해야함 
- 시퀀스다이어그램과 TC에 대하여 작업 TASK에 대하여 표시하기

어떠한 API에 대해서 작업이 완료가 되었는 지, 예외사항은 무엇이고 테스트케이스는 무엇인 지 작성하기

API로 프론트엔드에서 데이터 가져오기를 해야함. 

공동 작업을 위해서 전체적인 작업(TASK)에 대한 관리를 본인 스스로 할 수 있어야 함. 

2. 5주차 과제(2019-07-29 ~ 2019-08-02)

- 본사에서 DB교육하러 나오실 예정이며 3일동안 진행될 것
- 추가로 앱에 대한 개선이 있을 예정 

3. **\~ 8월 9일** 까지 평가가 인사팀에 전달이 될 것이며 어떠한 업무를 수행하고 결과물이 어떠한 지 확인!!!



문제 경험 및 해결 방법에 대하여 기록하고 극복하는 방식에 대해서 작성할 것



Front - Back  - DB

API를 생성하는 프로젝트는 1개를 만들 예정

API를 사용하는 클라이언트(현재 만든 MYSITE 같은 것)들은 또 다른 프로젝트로 만들어져야함



내용 일부변경.


쿼리



```xml
<!-- 상품 - 상품디테일 - 상품 이미지 모두 가져오기 -->
	<resultMap type="productvo" id="product_result_map"> 
		<id property="no" column="no" /> 
		<result property="name" column="name" />
		<result property="description" column="description" />
		<result property="status" column="status" />
		<result property="use_fl" column="use_fl" />
		<result property="like_cnt" column="like_cnt" />
		<result property="register_dt" column="register_dt" />
		<result property="category_no" column="category_no" />
		<result property="brand_no" column="brand_no" />
			

		<collection property="product_detail_list" column="no" 
			javaType="java.util.ArrayList" ofType="product_detail_result_map" 
			select="get_product_detail_list1"/>
		
		<collection property="product_image_list" column="no" 
			javaType="java.util.ArrayList" ofType="product_image_result_map" 
			select="get_product_image_list"/>

	</resultMap> 
	
	<resultMap type="com.cafe24.shop.vo.ProductDetailVo" id="product_detail_result_map"> 
		<id property="no" column="no" /> 
		<result property="product_no" column="product_no" /> 
		<result property="product_option" column="product_option" /> 
		<result property="stock_cd" column="stock_cd" /> 
		<result property="stock_cnt" column="stock_cnt" /> 
		<result property="warehouse_no" column="warehouse_no" /> 
	</resultMap> 
	
	<resultMap type="com.cafe24.shop.vo.ProductImageVo" id="product_image_result_map"> 
		<id property="no" column="no" /> 
		<result property="product_no" column="no" /> 
		<result property="url" column="url" /> 
		<result property="register_dt" column="register_dt" /> 
		<result property="use_fl" column="use_fl" /> 
		<result property="product_image_category_no" column="product_image_category_no" /> 
	</resultMap> 

	<select id="get_product_list_by_result_map" resultMap="product_result_map"> 
		<![CDATA[
			SELECT p.no, p.NAME, p.DESCRIPTION, p.STATUS, p.USE_FL, p.LIKE_CNT, p.REGISTER_DT, p.CATEGORY_NO, p.BRAND_NO 
			FROM PRODUCT p
		]]>
	</select>
	
	<select id="get_product_detail_list1" resultMap="product_detail_result_map"> 
		<![CDATA[
			SELECT pd.no, pd.PRODUCT_NO, pd.PRODUCT_OPTION, pd.PRICE, pd.STOCK_CD, pd.STOCK_CNT, pd.WAREHOUSE_NO
			FROM PRODUCT_DETAIL pd
			WHERE pd.PRODUCT_NO=#{no};
		]]>
	</select>
	
	<select id="get_product_image_list" resultMap="product_image_result_map"> 
		<![CDATA[
			SELECT pi.no, pi.PRODUCT_NO, pi.URL, pi.REGISTER_DT, pi.USE_FL,
			pi.PRODUCT_IMAGE_CATEGORY_NO
			FROM PRODUCT_IMAGE pi
			WHERE pi.PRODUCT_NO=#{no};
		]]>
	</select>
```

현 상황에서는 상품1개를 SELECT할 때 상품의 개수 * 2번(product_detail_result_map, product_image_result_map) +1을 사용하게 된다. 따라서 상품 100개를 SELECT 할 경우 201번, 1000개일 경우 2001번의 쿼리가 진행이 되므로 성능상에 큰 문제가 발생한다.



이를 해결하기 위하여 다음과 같이 조인쿼리를 이용하여 마이바티스의 장점을 최대한 사용하였다.

이를 해결하는 과정에서 각종 오류와 사용자 실수를 발견하였으며 이를 통해 경험적으로 체득한 내용은 table 설계 시 naming, query 진행 시 “ , ” 의 배치, 쉽게 읽을 수 있도록 작성하는 방법에 대하여 부수적으로 습득할 수 있었다.

```xml
<!-- 상품 - 상품디테일 - 상품 이미지 모두 가져오기 -->
	<resultMap type="productvo" id="product_result_map"> 
		<id property="no" column="no" /> 
		<result property="name" column="name" />
		<result property="description" column="description" />
		<result property="status" column="status" />
		<result property="use_fl" column="use_fl" />
		<result property="like_cnt" column="like_cnt" />
		<result property="register_dt" column="register_dt" />
		<result property="category_no" column="category_no" />
		<result property="brand_no" column="brand_no" />
			
		<collection property="product_detail_list" resultMap="product_detail_result_map"/> 
		<collection property="product_image_list" resultMap="product_image_result_map"/>
	</resultMap> 
	
	<resultMap type="com.cafe24.shop.vo.ProductDetailVo" id="product_detail_result_map"> 
		<id property="no" column="pd_no" /> 
		<result property="product_no" column="product_no" /> 
		<result property="product_option" column="product_option" /> 
		<result property="price" column="price" /> 
		<result property="stock_cd" column="stock_cd" /> 
		<result property="stock_cnt" column="stock_cnt" /> 
		<result property="warehouse_no" column="warehouse_no" /> 
	</resultMap> 
	
	<resultMap type="com.cafe24.shop.vo.ProductImageVo" id="product_image_result_map"> 
		<id property="no" column="PI_NO" /> 
		<result property="product_no" column="product_no" /> 
		<result property="url" column="PI_URL" /> 
		<result property="register_dt" column="register_dt" /> 
		<result property="use_fl" column="PI_USE_FL" /> 
		<result property="product_image_category_no" column="product_image_category_no" /> 
	</resultMap> 

	<select id="get_product_list_by_result_map" resultMap="product_result_map"> 
		<![CDATA[
			SELECT 
			p.no
			, p.NAME
			, p.DESCRIPTION
			, p.STATUS
			, p.USE_FL
			, p.LIKE_CNT
			, p.REGISTER_DT
			, p.CATEGORY_NO
			, p.BRAND_NO 
			
			, pd.no as pd_no
			, pd.PRODUCT_NO
			, pd.PRODUCT_OPTION
			, pd.PRICE
			, pd.STOCK_CD
			, pd.STOCK_CNT
			, pd.WAREHOUSE_NO
			
			, pi.no as PI_NO
			, pi.PRODUCT_NO
			, pi.URL as PI_URL
			, pi.REGISTER_DT
			, pi.use_fl as PI_USE_FL
			, pi.PRODUCT_IMAGE_CATEGORY_NO
			
			FROM PRODUCT p
			LEFT JOIN PRODUCT_DETAIL pd on(p.no=pd.PRODUCT_NO)
			LEFT JOIN PRODUCT_IMAGE pi on(p.no=pi.PRODUCT_NO);
		]]>
	</select>
```

즉, JOIN SELECT문 한번으로 가져온 모든 데이터를 각 칼럼에 맞게 배치될 수 있도록 Mapping하였으며 상품이 늘어나더라도 1번의 쿼리로 해결을 할 수 있게 되는 것이다. 정확한 성능상의 비교는 추 후 시간이 된다면 진행하도록 하겠다.
