### 관리자 주문 조회API(/api/orders/{customer_no}/{product_detail_no})

■ request

```
POST
  params:
  	path:
        {
       	"customer_no" : 0,
       	"product_detail_no" : 0,
        }
```

■ response

```
200 : 성공
	orders_vo_list
```

■ 실제동작코드

- Controller

```java
@ResponseBody
@RequestMapping(value = { "/{customer_no}/{product_detail_no}" }, method = RequestMethod.GET)
public ResponseEntity<JSONResult> get_orders_by_admin(
    @PathVariable(value="customer_no") Optional<Long> customer_no
			, @PathVariable(value="product_detail_no") Optional<Long> product_detail_no) {
		Long checked_customer_no = customer_no.isPresent()? customer_no.get(): null;
		Long checked_product_detail_no = product_detail_no.isPresent()? product_detail_no.get(): null;

		List<OrdersVo> orders_list = ordersService.get_orders_by_admin(checked_customer_no, checked_product_detail_no);
		return ResponseEntity.status(HttpStatus.OK).body(JSONResult.success(orders_list));
	}

```

- Service

```
public List<OrdersVo> get_orders_by_admin(Long checked_customer_no, Long checked_product_detail_no) {
    Map<String, Long> map = new HashMap<String, Long>();
    map.put("customer_no", checked_customer_no);
    map.put("product_detail_no", checked_product_detail_no);	
    return ordersDao.get_orders_by_admin(map);
}
```

■ TC CODE

- 준비 코드

```java

```

- 실제 테스트 코드 

```java

```

