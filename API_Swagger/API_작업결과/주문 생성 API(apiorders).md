### 주문 생성 API(/api/orders)

■ request

```
POST
  params:
  	body:
        {
        "address": "string",
        "card_no": "string",
        "customer_no": 0,
        "customer_request": "string",
        "delevery_status": "string",
        "no": 0,
        "payment_method": "string",
        "payment_no": 0,
        "phone_no": "string",
        "price": 0,
        "receiver_nm": "string",
        "shipping_method": "string"
        }
```

■ response

```
200 : 성공
	True
```

■ 실제동작코드

- Controller

```java
@ResponseBody
@RequestMapping(value = { "" }, method = RequestMethod.POST)
public ResponseEntity<JSONResult> add_cart(
    @RequestBody OrdersVo ordersvo
			, @RequestBody List<OrdersDetailVo> orders_detail_list) {

		Boolean inserted_orders_result=ordersService.add_orders(ordersvo, orders_detail_list);		
		return ResponseEntity.status(HttpStatus.OK).body(JSONResult.success(inserted_orders_result));
	}
```

- Service

```
@Transactional
public Boolean add_orders(OrdersVo ordersvo, List<OrdersDetailVo> orders_detail_list) {
    Boolean flag = false;
    Long customer_no = ordersvo.getCustomer_no();
    Boolean add_orders_result = (ordersDao.add_orders(ordersvo)==1);
    if(add_orders_result) {
    	ordersDao.add_orders_detail(orders_detail_list);
        // 카트 업데이트 시키기 
        List<CartVo> cartvo_list = new ArrayList<CartVo>();
        for(int i =0; i<cartvo_list.size(); i++) {
            CartVo tempvo = new CartVo();
            tempvo.setProduct_detail_no(orders_detail_list.get(i).getProduct_detail_no());
            tempvo.setCustomer_no(customer_no);
            tempvo.setOrdered_cart("Y");
            cartvo_list.add(tempvo);
        }
            cartDao.update_cart_list_by_order(cartvo_list);
            flag = true;
    }else {
    	flag = false;
    }
    return flag;
}
```

■ TC CODE

- 준비 코드

```java

```

- 실제 테스트 코드 

```java

```

