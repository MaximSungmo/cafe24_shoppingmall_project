작성일자 : 2019-08-05

작성자 : 김성모 

# 목차 

[TOC]
![목차](https://github.com/MaximSungmo/cafe24_shoppingmall_project/blob/master/%EB%AA%A9%EC%B0%A8.PNG)
  

## 1. 서론

카페24 신입 입사자 교육으로 쇼핑몰 구축하는 과제를 수행하게 되었다. 앞으로 추가 작업할 게 많지만 전체적인 내용을 작성하여 실무 투입 전에 역량을 최대로 끌어내야겠다는 생각을 한다. 추가 작업되는 내용들은 기존 내용에 더해가며 정리하도록 하겠다. 

우선적으로 주어진 과제는 2019년 7월 1일부터 7월 12일까지 프로젝트 계획, 요구사항 분석, 설계를 진행하였다. 처음으로 진행해보는 작업이므로 어느 정도의 시간이 걸릴 지, 어느 정도의 수준으로 구현을 할 지, 주어진 시간 내 실현이 가능할 수준인 지 판단을 해야만 했다. 결론적으로 말하자면 첫 시작 단계에서 많은 부분을 제대로 파악하지 못하였고 7월 12일 이 후로도 여러 차례 시나리오와 DB가 변경되어 예상했던 기간을 초과하여 작업을 진행하게 되었다. 제대로 파악하지 못한 부분과 이슈를 해결하는 노력은 뒤에서 다루도록 하겠다. 

프로젝트를 위한 전체적인 일정을 파악한 뒤 개발이 필요한 부분에 대하여 구체적인 내용을 뽑아내었다. 이 후 7월 15일부터 7월 26일(실제로는 8월 1일)까지는 개발 진행을 하게 되었다., 앞에서 파악된 일정을 가지고 공동 작업에서 본인이 계획한 일정을 관리하고 공동 작업자들과 공유할 지를 직접 개발 일정을 관리하며 기한에 맞도록 개발하는 것이 관건이였다. 실제 개발을 진행하며 일정을 관리할 때 일정 궤도에 오르기까지 준비 작업에서 많은 시간을 소비하였다. 예를 들어 처음 해보는 Test case 작성과 REST API 작성을 하는 것, 관련된 내용을 학습하고 이해하는 단계에서 상당한 시간이 소요되었다. 기존에는 클라이언트와 서버를 나누지 않고 웹 서비스를 개발했던 것과는 다르게 API 서버를 구성하여 클라이언트를 붙일 수 있도록 만드는 작업으로 방향이 제시되었으며 이에 따라 초반에 잘못 생각한 부분을 수정하면서 이미 작성된 코드를 새롭게 작성하는 일도 빈번하였다. 

현재 프로젝트가 마무리가 된 상황이 아니고 정확히 중간 단계인 API 서버를 만드는 것이 끝난 것이지만 지금까지 프로젝트를 통해서 내가 겪었던 문제점과 개발 일정을 산정함에 있어서 유의해야 할 점들을 작성하여 실무에서 일정을 효율적으로 산정하고 나에게 주어진 과업(task)와 문제(Issue)를 공유하는 방법에 대하여 시행착오를 통해 배울 수 있었다.


 


## 2. 본론

### 2-1. 프로젝트 계획과 각 계획별 일정은 어떻게 산정하는가?

#### 2-1-1. Usecase Diagram과 시나리오, 그리고 Sequence Diagram

초기 2주 간의 프로젝트 계획을 진행하였다. 계획 당시에 Usecase diagram 을 우선적으로 작성한 뒤 각각의 Usecase별(이하 UC)로 시나리오를 작성하는 방향으로 생각하였다. 그리하여 다음과 같은 방식으로 각 UC에 대한 시나리오를 작성하였다.   

##### Usecase Diagram

![Usecase Diagram](https://github.com/MaximSungmo/cafe24_shoppingmall_project/blob/master/Usecase/2%EC%B0%A8/cafe24_shoppingmall_usecase_diagram_20190705.PNG)
<br/>  

##### Usecase 식별자 목록

| 식별자 |    행위자    |     설명      |
| :----: | :----------: | :-----------: |
| UC-A01 | 관리자, 회원 |  사용자 인증  |
| UC-A02 |    관리자    | 카테고리 등록 |
| UC-A03 |    관리자    | 카테고리 수정 |
| UC-A04 |    관리자    | 카테고리 삭제 |
| UC-A05 |    관리자    |   상품 등록   |
| UC-A06 |    관리자    |   상품 수정   |
| UC-A07 |    관리자    |   상품 삭제   |
| UC-A08 |    관리자    |   회원 조회   |
| UC-A09 |    관리자    |   회원 삭제   |

| 식별자 | 행위자 |        설명        |
| :----: | :----: | :----------------: |
| UC-C01 |  고객  |   상품 목록 조회   |
| UC-C02 |  고객  |     상품 검색      |
| UC-C03 |  고객  |     상품 주문      |
| UC-C04 |  고객  | 장바구니 상품 등록 |
| UC-C05 |  고객  | 장바구니 상품 삭제 |
| UC-C06 |  회원  |   관심 상품 등록   |
| UC-C07 |  회원  |     상품 주문      |
| UC-C08 |  회원  |      리뷰작성      |

  

![UC-A01, UC-A02](https://github.com/MaximSungmo/cafe24_shoppingmall_project/blob/master/Usecase/3%EC%B0%A8/admin_%EC%8B%9C%EB%82%98%EB%A6%AC%EC%98%A4_20190705_1(%EB%B3%B5%EC%82%AC%EB%B3%B8).png)    



##### [첨부 1. 전체 UC에 대한 시나리오 확인하러 가기](https://github.com/MaximSungmo/cafe24_shoppingmall_project/blob/master/Usecase%20%EC%8B%9C%EB%82%98%EB%A6%AC%EC%98%A4/1%EC%B0%A8/usecase%20form.pdf)

필요한 작업에 대하여 상세하게 작성하였으며 시나리오를 작성하면서 필요한 API 에 대하여 목록을 뽑아낼 수 있었다. 

하지만 내가 작성한 시나리오 방식에 대해서 개인적으로 아쉬움이 많이 남았다. 그 이유는 글로 작성된 시나리오는 한 번에 내용을 파악하기에는 어려움이 있었기 때문이다. 따라서 Sequence diagram 으로 작성을 해보았다. 

![Auth_user Sequence diagram](https://github.com/MaximSungmo/cafe24_shoppingmall_project/blob/master/Usecase/Sequence/auth_user.png) Sequence diagram은 한 눈에 모든 절차를 파악할 수 있다는 장점이 있었다. 하지만 위의 사진에서 보이는 Sequence diagram을 작성할 당시에는 요구받았던 API 서버를 만드는 것에 대해서 단순하게 JSON형식으로 데이터를 주고 받는 웹페이지를 만드는 것이라고 파악을 하였다. 잘못된 생각이였다. 현재 카페24에서 서비스하고 있는 쇼핑몰센터의 구조를 명확히 파악했더라면, REST API를 사용하는 것에 대한 의미를 조금 더 파악했더라면 이주영 팀장님께서 요청하신 API 기능을 구현한다는 것이 API 서버와 클라이언트로 구성되어 여러 클라이언트가 언제든 확장 가능하도록 만든다는 것을 파악할 수 있었을텐데, 당시에는 그러지 못하였다. API를 만들어서 DB에서 데이터를 가져온 후 JSON형식의 데이터를 View 를 표현하는 페이지 안에서 처리를 할 것으로 생각하였고 따라서 위의 사진에서 보이는 것처럼 URL을 요청하며 html 파일을 맵핑시켜주는 생각을 하고 있었다. 

아쉽게도 Sequence diagram을 통해서 한 눈에 절차를 파악할 수 있도록 시나리오를 추가 작성하고 싶었으나 착오를 하는 바람에 시간적으로 부족하여 글로 작성된 시나리오를 수정하여야만 했다. 

계획 및 설계가 잘 되어야 일정에 맞추어 개발을 할 수 있다는 내용을 어디선가 들었는데, 몸소 체험할 수 있었다. 

#### 2-1-2. 확장성을 생각한 Entity-Relation 모델링, 개발 과정에서의 변경

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

- 기존 상품상세정보의 상품옵션

![상품상세정보 테이블](https://github.com/MaximSungmo/cafe24_shoppingmall_project/blob/master/ERD/%EC%82%AC%EC%A7%84/%EC%83%81%ED%92%88%EC%83%81%EC%84%B8%EC%A0%95%EB%B3%B4.PNG)   

- 기존의 옵션설정-템플릿  

![옵션설정-템플릿](https://github.com/MaximSungmo/cafe24_shoppingmall_project/blob/master/ERD/%EC%82%AC%EC%A7%84/%EC%98%B5%EC%85%98%EC%84%A4%EC%A0%95-%ED%85%9C%ED%94%8C%EB%A6%BF.PNG)  

- 기존에 생각했던 옵션설정-템플릿 테이블

| 옵션설정템플릿번호 | 옵션세트명 | 옵션사이즈 |
| ------------------ | ---------- | ---------- |
| 1                  | 사이즈     | 100        |
| 2                  | 사이즈     | 90         |
| 3                  | 사이즈     | 80         |
| 4                  | 컬러       | 검정       |
| 5                  | 컬러       | 흰색       |
| 6                  | 컬러       | 보라       |

위의 표와 같이 옵션설정템플릿을 설정해주면 프론트단에서 화면으로 해당 옵션세트명으로 데이터를 가져간 뒤 각각의 아이템을 옵션세트가 다른 정보끼리 조합하여 “100/검정, 100/흰색, 100/보라....80/보라“ 와 같은 방식으로 상품상세정보 테이블에 옵션을 넣어주는 것으로 생각을 하였다. 참 말도 안되는 생각을 하였다고 판단하여 여러 방식으로 변경할 수 있는 방법은 없을 지 찾아보았지만 당시에는 별다른 방법이 떠오르지 않았고 우선 개발은 진행해야만 했다.   

- 기존에 생각했던 테이블 예시  

| 상품상세번호 | 상품번호 | 상품옵션      | 재고 수량 |
| ------------ | -------- | ------------- | --------- |
| 1            | 1        | 1/4(100/검정) | 2         |
| 2            | 1        | 1/5(100/흰색) | 3         |
| 3            | 1        | 1/6(100/보라) | 4         |
| 4            | 1        | ...           | 5         |
| 5            | 1        | ...           | 0         |
| 6            | 1        | ...           | 0         |
| 7            | 1        | ...           | 1         |
| 8            | 1        | 3/8(80/흰색)  | 2         |
| 9            | 1        | 3/9(80/보라)  | 3         |

   

- 변경 예정인 테이블 예시

> 설명필요 (추 후 작성 예정)

DB에 관해서는 변경이 되거나 고려했던 사항들에 대해서 뒤에서 설명하도록 하겠다.



#### 2-1-3 프로젝트 계획과 설계를 통한 개발 일정 산정, 과연 타당한가? 

현재까지 프로젝트를 계획하여 UC를 뽑아내고 E-R 모델링을 하며 전체적인 그림을 그려왔다. 따라서 필요한 API 목록을 뽑아내어 주어진 시간 내 개발할 수 있도록 시간을 분배하는 과정이 남아있다. 

우선 한 가지 짚고 넘어가자면 본인은 개발 경험이 많지 않고 더군다나 일정을 고려하며 개발한 적은 단 한번도 없었다. 개인 포트폴리오를 위하여 일이 끝난 뒤 취미와 같이 간단한 부분만 구현했던 것이 전부였다. 결과적으로 본인의 역량과 API를 개발하는 부분에 대해서 정확한 파악을 하지 못했다. 또한 예상하지 못했거나 또는 변경 및 오류 해결로 인해 소요되는 시간 등에 대해서 매우 적은 비율로 전체 시간을 배분하였다. 

설계가 끝난 뒤(정확히 말하자면 설계를 잠시 뒤로 미뤄두고) 첫 째로 API의 목록을 개발하기 위해서 Test case(이하 TC)를 작성하게 되었다. TC에 대해서는 현재 교육하는 과정의 동료들과 함께 스터디를 진행하며 사전에 학습을 한 경험은 있었지만 실제로 적용해서 무언가 프로젝트를 진행한 적은 없었다. 따라서 TC를 작성하기 위한 사용법을 실전에 적용하기 위한 학습을 하는 데 많은 시간이 소요가 되었다(약 3일). TC를 작성할 당시에는 DB에 연결하는 부분은 배제하고 Mock web 연결과 데이터 흐름이 제대로 진행이 되는 부분에 대해서만 확인을 하였다. 작업이 상당히 빠르게 진행되었다. SQL문을 따로 생각하지 않고 전체적인 데이터의 흐름만 생각하였으며 모든 데이터가 DB를 거치지 않으니 오류가 발생할 일이 적었다. 그래서 하나의 API를 개발하는 과정에서 소요되는 시간이 매우 짧았다. 그래서 일정을 짧게 잡았다. 하지만 개발 도중에 DB를 연결하여 API를 개발하게 되어 SQL문을 작성하고 기존에 작성한 API의 변경이 필요하여 예상했던 시간보다 상당히 많은 시간을 소요하게 되었다.    

- ##### [개발 일정 관리표 확인하기](https://github.com/MaximSungmo/cafe24_shoppingmall_project/blob/master/%EA%B0%9C%EB%B0%9C%20%EC%9D%BC%EC%A0%95%20%EA%B4%80%EB%A6%AC%ED%91%9C.md)

- ##### [API LIST 확인하기](https://github.com/MaximSungmo/cafe24_shoppingmall_project/blob/master/API_Swagger/README.md)   

또한 추가적으로 Query 의 성능에 대한 문제를 생각하다보니 SQL문을 변경하고 다양한 방법에 대해 고민하게 되었다. 

##### 2-1-3-1  SQL 성능을 위한 처리(벌크 인서트...)

단순하게 생각했을 때 Query를 전송하기 위하여 SqlSession에 For문을 통해 **반복적인 삽입**을 진행하였다.

상품 상세 정보 테이블을 예시로 보자면 우선 기존에 생각했던 SQL은 다음과 같다.

- SqlSession에서의 배치 처리 

```XML
<insert id="insert_product_detail" parameterType="productdetailvo">
    <![CDATA[
        insert into PRODUCT_DETAIL 
        values(null, #{product_no}, #{product_option}, #{stock_cd}, #{stock_cnt}, #{warehouse_no});
    ]]>
</insert>
```

```java
public Integer add_product_detail(List<ProductDetailVo> list) {
    for(int vo=0; i<list.size(); i++){
        sqlSession.insert("insert_product_detail",vo);
    }
}
```

여기서 궁금한 점이 생겼다. 리스트 형태로 데이터를 받아와서 sqlSession을 계속 호출하게 되는 것은 문제가 없을 지, 다른 방식으로 데이터를 효율적으로 처리할 수 없을 지 생각하게 되었으며 관련 내용을 찾아보았다. list 형태의 데이터를 mybatis xml에서 처리 가능하다는 것을 알게 되었다. 과연 둘 중 어느 것이  효율적일까?

관련된 테스트를 진행한 결과가 있어서 직접 테스트하지 않고 결과를 공유하고자 한다. 결론은 xml에서의 foreach가 SqlSession에서의 배치처리보다 훨씬 효율적인 처리가 가능하다는 것을 알게 되었다.  

약 10만건의 데이터를 update, insert 한 결과가 다음과 같이 나왔다고 한다.

| 분류(건수)           | sqlSession 배치의 처리 시간 | xml에서 Foreach의 처리 시간 |
| -------------------- | --------------------------- | --------------------------- |
| update(100,000 rows) | 68(ms)                      | 7(ms)                       |
| insert(100,000 rows) | 75(ms)                      | 12(ms)                      |

[출처:https://yookeun.github.io/java/2015/06/19/mybatis-batch/](https://yookeun.github.io/java/2015/06/19/mybatis-batch/)



따라서 위의 코드는 다음의 방식으로 변경되었다.

- xml에서 Foreach

```xml
<insert id="insert_product_detail" parameterType="java.util.List">
		<![CDATA[
			insert into PRODUCT_DETAIL 
		    values
		]]>
		<foreach item="item" index="index" collection="list" separator=", " >
		<![CDATA[  
		    (
			    null, 
			    #{item.product_no},
			    #{item.product_option}, 
			    #{item.price}, 
			    #{item.stock_cd}, 
			    #{item.stock_cnt}, 
			    #{item.warehouse_no} 
		    )
		]]>
		</foreach>
</insert>
```

```java
public Integer add_product_detail(List<ProductDetailVo> list) {
		return sqlSession.insert("product.insert_product_detail", list);
}
```



이 외에도 카테고리, 이미지, 장바구니에 적용하였지만 데이터가 많지 않아서 실제로 속도의 차이가 크게 날 것으로 보이지는 않는다.   

추가적인 고민으로는 과연 여러 건의 데이터가 입력될 때 오류로 인한 실패가 발생하여 rollback하는 경우와 batch를 나누어서 입력하는 경우에 대한 학습이 필요할 것으로 생각된다.   



##### 2-1-3-2  SQL 성능을 위한 처리(Join)

상품의 정보를 가져와서 리스트로 화면에 보여주어야 한다. 다만 상품의 정보는 썸네일을 포함한 여러 이미지 파일, 다양한 옵션이 필요하다. 따라서 처음 데이터 가져오는 방식은 다음과 같았다.

```XML
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

모든 상품 리스트(또는 해당하는 카테고리에서의 상품 리스트)를 가져온 후 각각의 PK값을 For문으로 반복하여 데이터를 가져오는 방식으로 생각하였다. 

```java
List<ProductVo> products = sqlSession("get_product_list_by_result_map");
for(int i=0; product_no<products.size(); i++){
    product_no = products.get(i).getNo();
    products.get(i).setProduct_detail_list(sqlSession("get_product_detail_list1", product_no));
    products.get(i).setProduct_detail_image(sqlSession("get_product_image_list", product_no));
}
```

위의 코드로 sqlSession 접속 횟수를 식으로 표현하자면 다음과 같다 .

> products 목록 가져오는 데 1번 + products 목록 중 각각의 product_detail_list (n) +  product_detail_image (n)

만약에 상품이 100개에서 10000개가 되었다면 sqlSession으로 요청하는 갯수는 다음과 같게 된다. 

> 1 + 100 + 100 = 201번, 
>
> 1 + 10000 + 10000 = 20001번 

상품의 개수가 커질수록 엄청난 양의 쿼리를 요청하게 된다. 과부하로 인한 문제점이 예상이 되었다. 

따라서 간단하게 해결하는 방식이 필요하였고 JOIN, 단 한번의 쿼리로 정보를 받아오는 방식으로 생각하였다. 

우선, ERD를 보면 테이블의 PK값으로 가지고 있는 no 값의 명칭이 중복되는 것을 알 수 있다. 이를 해결하기 위하여 각 column에 해당하는 자료를 alias를 사용하여 명칭을 명확히 구분하였다. 

```xml
<select id="get_product_list_by_result_map" resultMap="product_result_map" parameterType="long"> 
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
        , pd.PRODUCT_NO as pd_product_no
        , pd.PRODUCT_OPTION
        , pd.PRICE
        , pd.STOCK_CD
        , pd.STOCK_CNT
        , pd.WAREHOUSE_NO

        , pi.no as pi_no
        , pi.PRODUCT_NO as pi_product_no
        , pi.URL as pi_url
        , pi.REGISTER_DT as pi_register_dt
        , pi.use_fl as pi_use_fl
        , pi.PRODUCT_IMAGE_CATEGORY_NO

        FROM PRODUCT p
        LEFT JOIN PRODUCT_DETAIL pd on(p.no=pd.PRODUCT_NO)
        LEFT JOIN PRODUCT_IMAGE pi on(p.no=pi.PRODUCT_NO)
    ]]>
    <if test='value!=0'></if>
        <![CDATA[
            WHERE p.category_no=#{value}
        ]]>
</select>	
```

이제 Join을 활용하여 데이터를 가지고 올 수 있게 준비가 되었다. 동일 product_no를 갖는 데이터에 대해서만 매칭되어 결과 값으로 나오게 될 것이다. 하지만 이를 받아줄 데이터형이 필요하다.  현재 만들어 놓은 객체들은 DB와 동일하게 구성한 객체 클래스로 가지고 있다. DTO를 활용하여 만들어주는 방법도 고려할 수 있으나 현재 만들어 놓은 객체에 SELECT된 결과가 들어오면 좋겠다고 생각을 했다. 

```XML
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
    <result property="product_no" column="pd_product_no" /> 
    <result property="product_option" column="product_option" /> 
    <result property="price" column="price" /> 
    <result property="stock_cd" column="stock_cd" /> 
    <result property="stock_cnt" column="stock_cnt" /> 
    <result property="warehouse_no" column="warehouse_no" /> 
</resultMap> 

<resultMap type="com.cafe24.shop.vo.ProductImageVo" id="product_image_result_map"> 
    <id property="no" column="pi_no" /> 
    <result property="product_no" column="pi_product_no" /> 
    <result property="url" column="pi_url" /> 
    <result property="register_dt" column="pi_register_dt" /> 
    <result property="use_fl" column="pi_use_fl" /> 
    <result property="product_image_category_no" column="product_image_category_no" /> 
    <association property="product_image_category_vo" resultMap="product_image_category_result_map"/>
</resultMap> 
```

위와 같이 처리를 해줌으로써 데이터는 JSON형태로 보았을 때 다음과 같은 형식으로 반환이 될 것이다.

```json
{
  "brand_no": 0,
  "category_no": 0,
  "categoryvo": {
    "name": "string",
    "no": 0,
    "parent_no": 0,
    "product_list": [
      {}
    ]
  },
  "description": "string",
  "like_cnt": 0,
  "name": "string",
  "no": 0,
  "product_detail_list": [
    {
      "no": 0,
      "price": 0,
      "product_no": 0,
      "product_option": "string",
      "stock_cd": "string",
      "stock_cnt": 0,
      "warehouse_no": 0
    }
  ],
  "product_image_list": [
    {
      "no": 0,
      "product_image_category_no": 0,
      "product_no": 0,
      "register_dt": "string",
      "url": "string",
      "use_fl": "string"
    }
  ],
  "register_dt": "string",
  "status": "string",
  "use_fl": "string"
}
```

따라서 한 번의 쿼리로 모든 상품의 대한 상세정보, 이미지 포함된 정보가 반환이 되므로 상품에 대한 반복쿼리에 대한 고민을 해결할 수 있게 되었다.



##### 2-1-3-3  SQL 성능을 위한 처리( 변경 데이터 row, static한 SQL 쿼리 캐싱률 높이기)

데이터를 수정하는 경우에는 update 진행을 하게 되는데, SQL문은 고정이 되어 있으므로 모든 정보가 업데이트된다. 회원정보를 수정하는 작업에서도 변경하려고 건드리지 않은 데이터도 모두 제 값으로 한번 변경이 된다. 

만약 많은 데이터가 변경이 되어야 하는 경우에는 성능을 개선할 수 있는 방법은 없을까? 생각하였다. 

- 기존 Query 예시

```xml
<update id="update_product_image_list" parameterType="java.util.List">		
	<foreach item="item" index="index" collection="list" separator=";" open="" close=";">
        <![CDATA[  
            UPDATE PRODUCT_IMAGE
            SET 
            url = #{item.url}
            , use_fl = #{item.use_fl}
            , product_image_category_no = #{item.product_image_category_no}
            WHERE 
            no = #{item.no}           
        ]]>  
    </foreach>
</update>
```

동일한 객체가 변경이 동일값으로 업데이트를 요청하는 경우 또는 여러 객체를 어쩔 수 없이 한번에 업데이트 요청하는데 정보 변경이 안된 데이터는 굳이 query가 진행되지 않아야만 불필요한 작업을 하지 않을 수 있다. 이에 따라서 SQL문을 다음과 같이 변경하였다.

- 변경된 Query 예시

```xml
<update id="update_product_image_list" parameterType="java.util.List">		
	<foreach item="item" index="index" collection="list" separator=";" open="" close=";">
        <![CDATA[  
            UPDATE PRODUCT_IMAGE
            SET 
            url = #{item.url}
            , use_fl = #{item.use_fl}
            , product_image_category_no = #{item.product_image_category_no}
            WHERE 
            no = #{item.no}
            and ((use_fl <> #{use_fl}) or (url <>#{item.url}) or 
            (product_image_category_no <> #{item.product_image_category_no}) )
        ]]>  
    </foreach>
</update>
```

변경되는 데이터가 없다면 query는 진행되지 않도록 하였다. 확대하여 생각해보면 약 100만건의 데이터를 업데이트하는 경우가 발생하였을 때 실제 변경이 된 약 1만건의 데이터만 업데이트가 진행되어야 하는 경우, 모든 데이터를 업데이트하는 것보다 실제 변경이 된 1만건만 진행될 수 있도록 하는 것이 성능에 도움이 된다. 



DB optimizer에 관한 교육을 3일간 들으면서 알게된 내용으로는 DB가 요청받은 SQL문을 가지고 캐시를 남기어 이 후에는 DB 서버가 재부팅되기 전까지 검색 성능을 좋게 만들 캐시들을 가지고 있다고 하였다.

단, 조건에 따라 SQL 문장이 달라지는 경우(byte가 달라지는 경우, 대 소문자가 다른 경우도 포함)에는 다른 SQL문으로 캐시해놓는다. 따라서 분기문을 사용하는 경우에는 각각 다른 조건의 SQL문이 캐시가 된다. 

**어떻게하면 동일한 SQL 문으로 조건을 분기시키는 것과 동일한 결과를 뽑아낼 수 있을까?**  기존의 query는 동적으로 값을 할당하였다. 조건문으로 분기하여 성능이 느려지고 한 눈에 보기에도 여러 조건으로 나뉘게 되어 복잡한 구조를 만들어내었다. 

- 기존 상품 가져오는 API

```xml
<select id="get_product_list" parameterType="map" resultType="productvo">
    <![CDATA[
   select no, name, description, status, use_fl, like_cnt, register_dt, category_no, brand_no 
   from PRODUCT		 
 	]]>	

    <if test='category_no!=0'>
        <![CDATA[
        where category_no=#{category_no}
       ]]>	
        <if test="kwd!=''">
            <![CDATA[
             and name like CONCAT('%',#{kwd},'%')
            ]]>
        </if>
    </if>

    <if test='category_no==0'>
        <if test="kwd!=''">
            <![CDATA[
             where name like CONCAT('%',#{kwd},'%')
            ]]>
        </if>
    </if>
    <![CDATA[
       order by no asc
       limit #{get_count} offset #{last_product_no}
  	]]>	
</select>
```



```XML
<select id="get_product_list" parameterType="map" resultType="productvo">
    <![CDATA[
   	select no, name, description, status, use_fl, like_cnt, register_dt, category_no, brand_no 
   	from PRODUCT		 
 	
	where 
		(category_no = #{category_no} and #{category_no}<>0) 
		and (name like CONCAT('%', #{kwd}, '%') and #{kwd}<>'')
   	order by no asc
    limit #{get_count} offset #{last_product_no}
	]]>	
</select>
```

이와 같이 Static Query문을 작성하여 자주 사용하게 될 검색 DB의 성능을 개선시키고자 하였다.

이를 해결하는 과정에서 각종 오류와 개발자가 범할 수 있는 간단한 실수를 발견하였으며 이를 통해 경험적으로 체득한 내용은 table 설계 시 naming, query 진행 시 “ , ” 의 배치, 쉽게 읽을 수 있도록 작성하는 방법에 대하여 부수적으로 습득할 수 있었다.



간단하게만 생각했던 SQL 처리에서 예상하지 못한 시간들이 많이 소요되며 기존에 세웠던 일정의 변경이 필요하게 되었다.



#### 2-1-4 개인정보 암호화, 패스워드 암호화는 Application 에서? DB에서?

[KISA, 암호화 조치 안내서](http://www.kisa.or.kr/uploadfile/201806/201806120949471644.pdf)

개인정보에 대한 암호화는 카페24에서 사용하는 방식인 AES-128 방식으로 진행할 예정이다.

패스워드는 SHA-256을 패스워드 암호화 방식으로 사용하려고 하였다. 하지만 지난 교육때 SHA-256은 사용하지 않아야 한다는 이야기를 들었으므로 변경이 필요하였다. 왜 SHA-256은 사용하면 안되는 지에 대한 이유는 다음의 링크를 통해서 확인하기 바란다. 

[안전한 패스워드 저장- 네이버 D2기술블로그 ](https://d2.naver.com/helloworld/318732)

[카페24 스터디 - Password Security(작성자: 김영호님)](https://github.com/hanseonghye/finalFirstTeamInCafe24/wiki/Password-Security)

결론적으로 SHA-256은 사용하지 않기로 하였으므로 SHA-512와 다른 암호화 함수들에 대하여 고민하게 되었다. 각각의 암호화 알고리즘이 달랐으나 후보군으로 2개의 암호화 함수를 고려하게 되었다.

1. SHA-512
2. Bcript 

네이버 D2 기술 블로그에 내용을 일부 발췌하자면.... 

> MD5, SHA-1, SHA-256, SHA-512 등의 해시 함수는 메시지 인증과 무결성 체크를 위한 것이다. 이것을 패스워드 인증을 위해 사용하면 앞에서 말한 인식 가능성과 빠른 처리 속도에 기인하는 취약점이 존재한다.

이 외에도 현재 API Server는 Spring security를 적용할 것인데, Bcrypt 를 지원하여 준다. 따라서 Bcrypt 함수로 고려하게 되었다.

[Bcrypt 사용방식](https://gompangs.tistory.com/entry/Spring-Password-Encoder)

이제 남은 것은 Application에서 암호화를 진행할 것인가, DBMS에서 진행할 것인가에 대한 답변이다.

내용을 찾아본 결과 API 방식과 TDE 방식, Plug-in 방식등이 있는 것에 대해서 알게 되었다. 암, 복호화의 위치에 따라 다른 방식이라고 볼 수 있는데, DB 서버에 성능 영향을 주지 않고 카페24 특성을 보았을 때 트랜잭션이 매우 많을 것이므로 업무 특성에 맞게 API방식을 적용하고자 한다. 

[암호화 적용 방식 선정 - LG CNS](https://blog.lgcns.com/1996)



## 3. 결론 

현재까지 부족한 것은 OAuth를 적용한 API 서버의 인증 역할, SQL Injection, XSS 공격에 대한 방어, 다량의 사용자 접속 시 부하 발생하는 원인 및 해결, Test case 작성 등이 있다.  현재 파악한 내용으로 SQL injection과 XSS 방어는 적용할 수 있겠으나 OAuth 적용, 사용자 접속 자동화를 통한 testing 환경 구축, Test case 작성에 대해서는 추가적인 학습이 필요할 것으로 보인다. 

API 서버를 만들면서 개발 일정과 성능 및 보안에 대하여 고려해야할 부분이 매우 많다는 것을 알게 되었다. 단순하게 하나의 기능만 만들어가며 시간이 어느정도 걸리게 될 지, 예상되는 이슈는 무엇이고 어떻게 해결해야 좋을 지, 무슨 방식을 적용하는 것이 합리적일 지에 대해서 스스로 고민할 수 있는 시간이였다. 아직은 정말 많이 부족하지만 전체적인 개발 일정을 스스로 산정하는 방식에 대해서, 일정을 관리하고 일정 내 작업 완료가 불가능하다면 이유는 무엇인 지에 대해서는 설명할 수 있는 힘을 기른 것 같아 실제 업무에서 팀원들에게 좀 더 명확한 내용에 대해 의견 제시를 할 수 있을 것이라는 생각을 하게 되었다. 또한 단순하게 개인적인 프로젝트 진행에서는 고려하지 않을 수 있는 보안 및 외부 공격에 대한 이슈, 성능에 대한 이슈는 깊게 생각해 본 적이 없었는데, 카페 24에서는 대용량의 트래픽이 발생하므로 자연스럽게 고민할 수 있었으며 간단하게나마 성능을 개선하는 방식들을 실제로 적용하고자 노력해보았다. 또한 설계부터 구현까지 진행되며 고민하는 과정에서 발생하는 문제들을 해결하기 위한 논리적인 사고 방식들은 흥미롭게 느껴져 앞으로 하게 될 개발자의 업무들이 기대되어 설레인다. 





##### [별첨1. API LIST](https://github.com/MaximSungmo/cafe24_shoppingmall_project/blob/master/API_Swagger/README.md)
