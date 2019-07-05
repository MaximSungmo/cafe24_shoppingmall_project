# ERD 작업 파일을 시간순으로 정리

1차(2019-07-03) / 테이블 정의, 관계 설정하지 않음. 
-> 추후 예정 : 테이블 분리가 필요한 지 확인, 관계 설정
![1차 ERD 사진(1)](https://github.com/MaximSungmo/cafe24_shoppingmall_project/blob/master/ERD/1차/ERD_20190703(1).PNG)
![1차 ERD 사진(2)](https://github.com/MaximSungmo/cafe24_shoppingmall_project/blob/master/ERD/1차/ERD_20190703(2).PNG)
---
2차 변경내역(2019-07-04) 
  1. 테이블명 변경 (장바구니 -> 장바구니내역)
  2. 테이블 추가 (옵션, 옵션설정-템플릿)
  3. 게시판, 게시글 테이블 속성 변경, Relation 변경 
  4. 회원 테이블 self reference로 추천회원 속성 추가 
  5. 카테고리 테이블 self reference로 상위카테고리 변경 
  6. 장바구니내역, 관심상품, 주문상세내역 테이블 약한결합으로 변경 (PK값 제외하였음)
  7. 상품상세정보테이블 속성 변경 

![2차 ERD 사진(1)](https://github.com/MaximSungmo/cafe24_shoppingmall_project/blob/master/ERD/2차/ERD_20190704(1).PNG)
![2차 ERD 사진(2)](https://github.com/MaximSungmo/cafe24_shoppingmall_project/blob/master/ERD/2차/ERD_20190704(2).PNG)

---
2-1차 변경내역(2019-07-04) 

1. 리뷰 테이블 관계 테이블 변경
2. 결제 테이블 추가 
3. 약관동의여부, 약관동의서 테이블 추가 
4. 상품상세정보 테이블에 재고분류코드 속성 추가 
5. 테이블명 변경(회원 -> 고객)
6. 고객 테이블 회원권한 속성 추가 
![2-1차 ERD 사진(1)](https://github.com/MaximSungmo/cafe24_shoppingmall_project/blob/master/ERD/2차/ERD_20190704-2(1).PNG)
---
3차 변경내역(2019-07-05)

1. DATA TYPE, 물리 이름 작성, 논리 이름 수정 
2. 배송지 테이블 추가, 주문테이블 
3. 주문상세내역 테이블 PK키 생성
4. 게시판 -> 게시판_카테고리 (테이블명 변경)
5. 고객 테이블 내 배송지 관련 속성 삭제, 주소 테이블 생성 

![3차 ERD 사진(1)](https://github.com/MaximSungmo/cafe24_shoppingmall_project/blob/master/ERD/3차/ERD_20190705(1).PNG)
![3차 ERD 사진(2)](https://github.com/MaximSungmo/cafe24_shoppingmall_project/blob/master/ERD/3차/ERD_20190705(2).PNG)
