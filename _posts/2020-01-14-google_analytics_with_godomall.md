---
layout: post
title: 고도몰 - 구글 애널리틱스 전자상거래 코드
subtitle :
tags: [javascript, godomall]
author: i2801
comments : true
---

![hashids.js](/assets/post/godo_google.png)

고도몰 솔루션을 이용하는 쇼핑몰에 구글 애널리틱스 전자상거래 코드를 삽입 시 아래와 같이 코드를 작성해야 정상적으로 동작을 한다. (결제완료 페이지에 삽입)

*해당 코드는 Google Tag Implementation Team 에서 제공받은 고도몰 전용코드이다.*
```javascript
document.addEventListener('DOMContentLoaded', function(event) {
    var affiliation = '쇼핑몰명';
    var brand= '브랜드명(쇼핑몰명)';
    var e = $('input[name=naver-common-inflow-script-order-item]');
    var totalPrice = 0;
    var products = [];
    for(var i = 0; i < e.length; i++){
        var detail = eval('(' + e[i].value + ')');
        products[i] = {
            'name': detail.goodsnm,
            'id': detail.goodsno,
            'price':  detail.price/detail.ea,
            'brand': brand,
            'category':detail.sno,
            'quantity': detail.ea
        }
        totalPrice += detail.price;
    }
    gtag('event', 'purchase', {
        "transaction_id": eval('(' + e[0].value + ')').ordno,
        "affiliation":affiliation,
        "value": totalPrice,
        "currency": "KRW",
        "shipping": 0,
        "items": products
    });
});
```

또한, 코드가 작동하는지 여부는 크롬 브라우져를 이용하여 [구글 태그 어시스턴트](https://chrome.google.com/webstore/detail/tag-assistant-by-google/kejbdjndbnbjgmefkgdddjlbokphdefk?hl=ko){: target="_blank"} 확장프로그램 등록 후 결제완료 페이지에서 해당 확장프로그램 클릭 후 해당 계정의 Events 탭을 통하여 동작여부를 확인할 수 있다.


