---
title: "IE 스크롤 제약 사항(IE scroll limit)"
categories: 
  - frontEnd
last_modified_at: 2018-08-15T22:42:00+09:00
tags:
  - ie
  - html
  - scroll
toc: true
---

## Explain

IE에는 스크롤할 수 있는 양의 제약이 있다.

정확히 말하자면 스크롤 내에 들어갈 수 있는 픽셀수가 제약이 있는데, 이는 대략 1533767px 이다.

저 정도를 넘어갈일은 없지만 대용량 데이터를 로딩하는 경우 일부 데이터가 스크롤이 안될 문제가 발생할수 있겠다.

아래 예제를 ie에서 해보자.

## Example
<div>
  <div style="float:left; width:150px;">1704803px</div>
  <div style="float:left; width: 34px; height: 150px; overflow-y: auto; margin-left: -17px; -ms-overflow-y: auto;" onscroll="this.nextSibling.nextSibling.innerHTML = this.scrollTop">
      <div style="width: 1px; height: 1704803px; overflow: hidden;">&nbsp;</div>
  </div>
  
  <div style="float:left; width:150px;">&nbsp;</div>
  <div style="float:left; width:150px;">1531000px</div>

  <div style="float:left; width: 34px; height: 150px; overflow-y: auto; margin-left: -17px; -ms-overflow-y: auto;" onscroll="this.nextSibling.nextSibling.innerHTML = this.scrollTop">
      <div id="s" style="width: 1px; height: 1531000px; overflow: hidden;">&nbsp;</div>
  </div>
  
  <div style="float:left; width:150px;">&nbsp;</div>
</div>

