# Usecase 

# Usecase Diagram
![Usecase  Diagram 1차](https://github.com/MaximSungmo/cafe24_shoppingmall_project/blob/master/Usecase/1%EC%B0%A8/cafe24_shoppingmall_usecase_diagram_20190701.PNG)
---
![Usecase  Diagram 1차](https://github.com/MaximSungmo/cafe24_shoppingmall_project/blob/master/Usecase/2%EC%B0%A8/cafe24_shoppingmall_usecase_diagram_20190705.PNG)


# Usecase 식별자 목록

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



# Usecase 시나리오

## 관리자 시나리오



## 고객 시나리오


# 암호화 전략 
1. 개인정보 : 이름, 비밀번호, 주소, 일반전화, 휴대전화, 이메일 
>> AES-128

2. 패스워드 : 복호화가 가능한 함수를 사용해야함
현재 카페24에서는 기본적으로 SHA-256을 사용하는 것으로 알고 있으나 암호화 전략을 찾아본 결과
Adaptive Key Derivation Function으로 **PBKDF2** 사용할 예정이다. 
Salting과 Key Stretching을 반복하여 패스워드를 저장하고, 입력값을 추가하는 등 보안의 강도를 선택할 수 있다.
주요 특징으로는 
- 가장 널리 사용되는 Key Derivation Function은 PBKDF2 이다.
- Password-Based Key Derivation Function의 약자이며, 해시함수들의 컨테이너이다.
- 이미 검증된 해시함수들을 사용하고, 구현이 쉬우며, 여러 번의 반복되더라도 데이터의 손실이 거의 없다.

# 수정내역
1차 : 2019-07-04 / 전체적인 윤곽 잡기
 추후 예정 : 세부사항 파악, 기능 통합 삭제 필요.


