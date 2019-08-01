작성일자 : 2019-08-01

작성자 : 김성모 



## 1. 서론

카페24 신입 입사자 교육으로 쇼핑몰 구축하는 과제를 수행하게 되었다. 앞으로 추가 작업할 게 많지만 전체적인 내용을 작성하여 실무 투입 전에 역량을 최대로 끌어내야겠다는 생각을 한다. 추가 작업되는 내용들은 기존 내용에 더해가며 정리하도록 하겠다. 

우선적으로 주어진 과제는 2019년 7월 1일부터 7월 12일까지 프로젝트 계획, 요구사항 분석, 설계를 진행하였다. 처음으로 진행해보는 작업이므로 어느 정도의 시간이 걸릴 지, 어느 정도의 수준으로 구현을 할 지, 주어진 시간 내 실현이 가능할 수준인 지 판단을 해야만 했다. 결론적으로 말하자면 첫 시작 단계에서 많은 부분을 제대로 파악하지 못하였고 7월 12일 이 후로도 여러 차례 시나리오와 DB가 변경되어 예상했던 기간을 초과하여 작업을 진행하게 되었다. 제대로 파악하지 못한 부분과 이슈를 해결하는 노력은 뒤에서 다루도록 하겠다. 

프로젝트를 위한 전체적인 일정을 파악한 뒤 개발이 필요한 부분에 대하여 구체적인 내용을 뽑아내었다. 이 후 7월 15일부터 7월 26일(실제로는 8월 1일)까지는 개발 진행을 하게 되었다., 앞에서 파악된 일정을 가지고 공동 작업에서 본인이 계획한 일정을 관리하고 공동 작업자들과 공유할 지를 직접 개발 일정을 관리하며 기한에 맞도록 개발하는 것이 관건이였다. 실제 개발을 진행하며 일정을 관리할 때 일정 궤도에 오르기까지 준비 작업에서 많은 시간을 소비하였다. 예를 들어 처음 해보는 Test case 작성과 REST API 작성을 하는 것, 관련된 내용을 학습하고 이해하는 단계에서 상당한 시간이 소요되었다. 기존에는 클라이언트와 서버를 나누지 않고 웹 서비스를 개발했던 것과는 다르게 API 서버를 구성하여 클라이언트를 붙일 수 있도록 만드는 작업으로 방향이 제시되었으며 이에 따라 초반에 잘못 생각한 부분을 수정하면서 이미 작성된 코드를 새롭게 작성하는 일도 빈번하였다. 

현재 프로젝트가 마무리가 된 상황이 아니고 정확히 중간 단계인 API 서버를 만드는 것이 끝난 것이지만 지금까지 프로젝트를 통해서 내가 겪었던 문제점과 개발 일정을 산정함에 있어서 유의해야 할 점들을 작성하여 실무에서 일정을 효율적으로 산정하고 나에게 주어진 과업(task)와 문제(Issue)를 공유하는 방법에 대하여 시행착오를 통해 배울 수 있었다.





## 2. 본론

### 2-1. 프로젝트 계획과 각 계획별 일정은 어떻게 산정하는가?

#### 2-1-1. Usecase Diagram과 시나리오, 그리고 Sequence Diagram

초기 2주 간의 프로젝트 계획을 진행하였다. 계획 당시에 Usecase diagram 을 우선적으로 작성한 뒤 각각의 Usecase별(이하 UC)로 시나리오를 작성하는 방향으로 생각하였다. 그리하여 다음과 같은 방식으로 각 UC에 대한 시나리오를 작성하였다. 

![UC-A01, UC-A02](https://github.com/MaximSungmo/cafe24_shoppingmall_project/blob/master/Usecase/3%EC%B0%A8/admin_%EC%8B%9C%EB%82%98%EB%A6%AC%EC%98%A4_20190705_1(%EB%B3%B5%EC%82%AC%EB%B3%B8).png)

[전체 UC에 대한 시나리오 확인하러 가기](#)

필요한 작업에 대하여 상세하게 작성하였으며 시나리오를 작성하면서 필요한 API 에 대하여 목록을 뽑아낼 수 있었다. 

하지만 내가 작성한 시나리오 방식에 대해서 개인적으로 아쉬움이 많이 남았다. 그 이유는 글로 작성된 시나리오는 한 번에 내용을 파악하기에는 어려움이 있었기 때문이다. 따라서 Sequence diagram 으로 작성을 해보았다. 

![Auth_user Sequence diagram](https://github.com/MaximSungmo/cafe24_shoppingmall_project/blob/master/Usecase/Sequence/auth_user.png) Sequence diagram은 한 눈에 모든 절차를 파악할 수 있다는 장점이 있었다. 하지만 위의 사진에서 보이는 Sequence diagram을 작성할 당시에는 요구받았던 API 서버를 만드는 것에 대해서 단순하게 JSON형식으로 데이터를 주고 받는 웹페이지를 만드는 것이라고 파악을 하였다. 잘못된 생각이였다. 현재 카페24에서 서비스하고 있는 쇼핑몰센터의 구조를 명확히 파악했더라면, REST API를 사용하는 것에 대한 의미를 조금 더 파악했더라면 이주영 팀장님께서 요청하신 API 기능을 구현한다는 것이 API 서버와 클라이언트로 구성되어 여러 클라이언트가 언제든 확장 가능하도록 만든다는 것을 파악할 수 있었을텐데, 당시에는 그러지 못하였다. API를 만들어서 DB에서 데이터를 가져온 후 JSON형식의 데이터를 View 를 표현하는 페이지 안에서 처리를 할 것으로 생각하였고 따라서 위의 사진에서 보이는 것처럼 URL을 요청하며 html 파일을 맵핑시켜주는 생각을 하고 있었다. 

아쉽게도 Sequence diagram을 통해서 한 눈에 절차를 파악할 수 있도록 시나리오를 추가 작성하고 싶었으나 착오를 하는 바람에 시간적으로 부족하여 글로 작성된 시나리오를 수정하여야만 했다. 

계획 및 설계가 잘 되어야 일정에 맞추어 개발을 할 수 있다는 내용을 어디선가 들었는데, 몸소 체험할 수 있었다. 

#### 2-1-2.  확장성을 생각한 Entity-Relation 모델링, 개발 과정에서의 변경

UC를 작성한 뒤 Entity-Relation 모델링을 진행하였다. 첫 단계에서는 다음과 같이 Entity를 시나리오에 맞추어 구성하였다. 

![ERD_20190703(1)](https://github.com/MaximSungmo/cafe24_shoppingmall_project/blob/master/ERD/1%EC%B0%A8/ERD_20190703(1).PNG)

![ERD_20190703(2)](https://github.com/MaximSungmo/cafe24_shoppingmall_project/blob/master/ERD/1%EC%B0%A8/ERD_20190703(2).PNG)

Relation에 관하여는 생각하지 않고 Entity만 구분하였으며 대략적인 관계만 생각하여 배치하였다.  

이 후 테이블명 자체에서 혼란을 줄 수 있는 이름은 변경하여 각 테이블이 가진 역할을 명확히 하고자 했다. 또한 여러 쇼핑몰을 확인하며 화면에 보이는 정보와 보이진 않지만 필요한 정보들을 고려하여 Entity의 attribute를 추가, 삭제하였다. 마지막으로 확장을 고려하여 테이블을 나누고 참조를 변경하는 시도를 하였다. 

카테고리 테이블(category)의 경우에는 카테고리가 대, 중, 소 뿐 아니라 여러 계층으로 나뉠 수 있다는 점을 고려하여 recusive하게 구성하였다.

- 카테고리 테이블   

![카테고리 테이블](https://github.com/MaximSungmo/cafe24_shoppingmall_project/blob/master/ERD/%EC%82%AC%EC%A7%84/%EC%B9%B4%ED%85%8C%EA%B3%A0%EB%A6%AC.PNG)  

회원 테이블 또한 추천인이 있는 경우에도 동일하게 recusive하게 참조할 수 있도록 작성하였다. 

또한 주문이 완료된 테이블은 참조로 인하여 정보가 수정될 수 있으므로 반정규화를 진행하여 주문이 완료된 상품 관련 정보는 바뀌지 않도록 하였다. 

- 기존 ERD 주문 관련 테이블   

![기존 ERD 주문 관련 테이블](https://github.com/MaximSungmo/cafe24_shoppingmall_project/blob/master/ERD/%EC%82%AC%EC%A7%84/ERD_%EA%B3%BC%EA%B1%B0_%EC%A3%BC%EB%AC%B8%EC%83%81%EC%84%B8.PNG)   



- 최종 ERD 주문 관련 테이블    

![최종 ERD 주문 관련 테이블](https://github.com/MaximSungmo/cafe24_shoppingmall_project/blob/master/ERD/%EC%82%AC%EC%A7%84/ERD_%EC%B5%9C%EC%A2%85_%EC%A3%BC%EB%AC%B8%EC%83%81%EC%84%B8.PNG)    

- ERD_전체(기준일 : 2019-08-01)

![ERD_최종](https://github.com/MaximSungmo/cafe24_shoppingmall_project/blob/master/ERD/ERD_%EC%B5%9C%EC%A2%85.PNG)  

이 외에도 여러 칼럼이 추가되고 관계가 변경되었으며 최종적으로 변경이 된 DB는 위의 사진과 같다.   

E-R 모델링 과정에서 상당한 어려움을 겪었으며 현재도 부족하다고 느끼는 부분이 많아 지속적으로 수정할 계획을 하고 있다.  



상품 옵션의 경우, 가장 크게 고민을 해야만 했다. 물론 현재도 답은 나오지 않았다고 본다. 다만, 너무 시간을 끌 수 없으므로 매우 불만족스럽지만 진행할 수 밖에 없었다. 이와 관련해서는 DB와 관련된 부서의 여러 담당자님들에게 질문을 하기도 하였다. 현재 시간 관계상 적용은 못한 상태이지만 기존에 생각했던 방식과 변경된 방식에 대해서 이야기하고자 한다. 

우선 고려했던 사항은 다음과 같다.

1. 사이즈, 컬러 이 외에도 사용자의 요건에 따라 자유롭게 추가할 수 있어야만 한다. 
2. 옵션은 복합적으로 설정이 되며 사이즈, 컬러가 각각 3개씩 있다면 총 9개의 옵션이 생성되어야 하며 재고 개수도 관리가 되어야 한다. 

위 조건들을 충족시키는 방향으로 생각을 해보니 “옵션설정-템플릿”을 만들어서 클라이언트가 자신에게 필요한 옵션들을 추가할 수 있는 템플릿을 DB에 담은 뒤, 자바스크립트로 프론트단에서 정보를 카테시안 프로덕트와 같이 나올 수 있도록 화면에 뿌려준 뒤 각 옵션을 가져와 원자성을 무시한 채 정보를 넣는 방식으로 생각했다. 테이블을 보자면 다음과 같다. 

| 옵션설정템플릿번호 | 옵션세트명 | 옵션사이즈 |
| ------------------ | ---------- | ---------- |
| 1                  | 사이즈     | 100        |
| 2                  | 사이즈     | 90         |
| 3                  | 사이즈     | 80         |
| 4                  | 컬러       | 검정       |
| 5                  | 컬러       | 흰색       |
| 6                  | 컬러       | 보라       |

위의 표와 같이 옵션설정템플릿을 설정해주면 프론트단에서 화면으로 해당 옵션세트명으로 데이터를 가져간 뒤 각각의 아이템을 옵션세트가 다른 정보끼리 조합하여 “100/검정, 100/흰색, 100/보라....80/보라“ 와 같은 방식으로 상품상세정보 테이블에 옵션을 넣어주는 것으로 생각을 하였다. 참 말도 안되는 생각을 하였다고 판단하여 여러 방식으로 변경할 수 있는 방법은 없을 지 찾아보았지만 당시에는 별다른 방법이 떠오르지 않았고 우선 개발은 진행해야만 했다.   

- 기존 옵션 테이블 

![상품상세정보 테이블](https://github.com/MaximSungmo/cafe24_shoppingmall_project/blob/master/ERD/%EC%82%AC%EC%A7%84/%EC%83%81%ED%92%88%EC%83%81%EC%84%B8%EC%A0%95%EB%B3%B4.PNG)  

![옵션설정-템플릿](https://github.com/MaximSungmo/cafe24_shoppingmall_project/blob/master/ERD/%EC%82%AC%EC%A7%84/%EC%98%B5%EC%85%98%EC%84%A4%EC%A0%95-%ED%85%9C%ED%94%8C%EB%A6%BF.PNG)  





 





Issue

1. 기획 및 설계



2. 일정 관리 



3. 개발 및 











 





Issue

1. 기획 및 설계



2. 일정 관리 



3. 개발 및 





 





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
