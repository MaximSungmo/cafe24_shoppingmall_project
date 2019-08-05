### 브랜드삭제 API(/api/brand/{brand_no})

■ request

```
POST
  params:
  	body:
    {
      "address": "string",
      "delete_dt": "string",
      "name": "string",
      "no": 0,
      "phone_no": "string",	
      "register_dt": "string",
      "use_fl": "string"
	}
```

■ response

```
200 : 성공
	True
400 : 잘못된 요청
	입력 에러 메세지 
500 : 내부 서버 에러
	"Server Error"
```

■ 실제동작코드

- Controller

```java
@ResponseBody
@RequestMapping(value = { "/{brand_no}" }, method = RequestMethod.DELETE)
public ResponseEntity<JSONResult> delete_category(@RequestBody BrandVo vo) {
    Boolean delete_result = brandService.delete_brand(vo);
    if (delete_result) {
        return ResponseEntity.status(HttpStatus.OK).body(JSONResult.success(delete_result));
    } else {
        return ResponseEntity.status(HttpStatus.INTERNAL_SERVER_ERROR).body(JSONResult.fail("Server Error"));
    }
}
```

- Service

```
public Boolean delete_brand(BrandVo brandvo) {
	return brandDao.delete_brand(brandvo)==1;
}
```

■ TC CODE

- 준비 코드

```java

```

- 실제 테스트 코드 

```java

```

