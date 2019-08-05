### 주문 조회 API(/api/orders/{customer_no}/{product_detail_no})

■ request

```
POST
  params:
  	path:
        {
       	"customer_no" : 0,
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
@RequestMapping(value = { "/{customer_no}" }, method = RequestMethod.GET)
public ResponseEntity<JSONResult> get_orders(
    @PathVariable(value="customer_no") Long customer_no) {
    List<OrdersVo> orders_list = ordersService.get_orders(customer_no);
    return ResponseEntity.status(HttpStatus.OK).body(JSONResult.success(orders_list));
}
```

- Service

```
public List<OrdersVo> get_orders(Long customer_no) {
	return ordersDao.get_orders(customer_no);
}
```

■ TC CODE

- 준비 코드

```java

```

- 실제 테스트 코드 

```java

```

