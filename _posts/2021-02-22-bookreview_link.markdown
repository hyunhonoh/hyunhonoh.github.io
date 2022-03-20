---
layout: post
title:  "북리뷰 템플릿 테마에 대해서"
date:   2021-02-22 23:23:35 +0900
categories: 
- life
tags:
- book
- blog
- linkprice
classes: wide
published: true
---

도서리뷰만을 위해서 따로 템플릿을 찾아보고 수정해서 정리한 페이지에 북리뷰를 올리고 있습니다.
템플릿에 도서 링크를 넣어두는게 있어 교보문고의 도서 링크를 추가하고 기록을 하고 있습니다. 정상적으로만 열리는 것을 확인하고 사용중이었는데 얼마전에 클릭 후 주소를 다시 보니 원래 넣었던 것에 추가적인 내용들이 들어간 주소로 변경되어 있었습니다.

예를 들면 다음과 같은 주소를 클릭해서 가면 아래와 같이 생성이 되네요.

```
http://www.kyobobook.co.kr/product/detailViewKor.laf?barcode=9788901219080
```

- 변경 주소
```
http://www.kyobobook.co.kr/product/detailViewKor.laf?barcode=9788901219080&OV_REFFER=http://click.linkprice.com/click.php?m=kbbook&a=A100532541&l=9999&l_cd1=0&u_id=klhv7tqnzj02l3o202yqe&l_cd2=0&tu=http%3A%2F%2Fwww.kyobobook.co.kr%2Fproduct%2FdetailViewKor.laf%3Fbarcode%3D9788901219080
```

자세히 보면 중간에 linkprice로 넘어가고 코드가 붙는 것으로 보입니다.

찾아보니 해당 코드는 교보문고 코드라는 내용이 있네요. 외부에서 접근하면 linkprice코드가 붙으면서 교보문고에 도움이 되는 형태로 운영이 되는 것 같습니다.

앞으로는 간간히 읽는 대부분의 책이 리디셀렉트에서 읽는 책이라 리디북스로 연결을 바꿀려고 합니다.

