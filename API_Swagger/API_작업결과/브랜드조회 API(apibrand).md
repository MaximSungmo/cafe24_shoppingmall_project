### 브랜드조회 API(/api/brand)

■ request

```
GET
  params:
```

■ response

```
200 : 성공
	BrandVo List
```

■ 실제동작코드

- Controller

```java
@ResponseBody
@RequestMapping(value="", method = RequestMethod.GET)
public ResponseEntity<JSONResult> get_brand_list() {
    List<BrandVo> list = brandService.get_brand_list();
    return ResponseEntity.status(HttpStatus.OK).body(JSONResult.success(list));
}	
```

- Service

```
public List<BrandVo> get_brand_list() {
	return brandDao.get_brand_list();
}
```

■ TC CODE

- 준비 코드

```java

```

- 실제 테스트 코드 

```java

```

