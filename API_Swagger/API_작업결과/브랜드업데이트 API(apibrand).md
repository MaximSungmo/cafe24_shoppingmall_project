### 브랜드업데이트 API(/api/brand)

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
	"브랜드 정보가 정상적으로 수정되지 않았습니다."
```

■ 실제동작코드

- Controller

```java
@ResponseBody
@RequestMapping(value="", method = RequestMethod.PUT)
public ResponseEntity<JSONResult> update_brand(@RequestBody @Valid BrandVo brandvo, BindingResult bindResult) {	
    if(bindResult.hasErrors()) {
        List<ObjectError> list = bindResult.getAllErrors();
        for(ObjectError error : list) {
            return ResponseEntity.status(HttpStatus.BAD_REQUEST).body(JSONResult.fail(error.getDefaultMessage()));
        }
    }
    Boolean update_result = brandService.update_brand(brandvo);
    if(update_result) {
        return ResponseEntity.status(HttpStatus.OK).body(JSONResult.success(brandvo));
    }else {
        return ResponseEntity.status(HttpStatus.BAD_REQUEST).body(JSONResult.fail("브랜드 정보가 정상적으로 수정되지 않았습니다."));
    }
}
```

- Service

```
public Boolean update_brand(BrandVo brandvo) {
	return brandDao.update_brand(brandvo)==1;
}
```

■ TC CODE

- 준비 코드

```java

```

- 실제 테스트 코드 

```java

```

