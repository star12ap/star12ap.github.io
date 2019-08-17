---
title: "마우스 클릭 이벤트 순서(mouse click event)"
categories: 
  - frontEnd
last_modified_at: 2018-08-17T21:42:00+09:00
tags:
  - mouse
  - event
  - onmouseup
  - onmousedown
  - onclick
toc: true
---

## Explain

마우스 클릭시 발생하는 이벤트 순서는 onmousedown -> onmouseup -> click 순이다.

onmousedown되고 onclick 되고 onmouseup아닌가라고 생각하기 쉬운데 아니더라.. ㅋ

참고로 onmousedown에서 alert를 띄우면 클릭해도 onmouseup, onclick 이벤트 모두 발생하지 않지만, onmouseup에서 alert를 띄우면 onclick까지 정상적으로 먹는다.

보아하니 down에서 alert시에는 이벤트가 끊기지만 up에서 alert를 하는 경우는 down에서 먹은 이벤트가 up과는 별개로 click까지 가는 모양이다.


## Example
<script type="text/javascript">
  const eventWriter = (e) => {
    document.getElementById("test").textContent += (event && event.type) + " " ;
  }
</script>
<div style="vertical-align: middle">
  <div onclick="eventWriter(this)" onmousedown="if (document.querySelector('select').selectedIndex == 1) {alert('onmousedown');}eventWriter(this)" onmouseup="if (document.querySelector('select').selectedIndex == 2) {alert('onmouseup');} eventWriter(this)" style="font-size: initial; background-color:black; text-align: center; color:white; width:50px; height:25px; display:inline-block; cursor:pointer">
    클릭
  </div>
  <div style="font-size: initial; background-color:black; text-align: center; color:white; width:75px; height:25px; cursor:pointer; display:inline-block;" onclick="document.getElementById('test').textContent=''">클리어</div>
  <label style="font-size: initial; display:inline-block; margin: 0px 0px 0px 25px; vertical-align: middle;"> alert </label>
  <SELECT style="font-size: initial; display:inline-block; margin: 0px; padding:0px; height:28px; vertical-align: middle">
    <option>추가 안함(default)</option>
    <option>mousedown에 추가</option>
    <option>mouseup에 추가</option>
  </SELECT>
  <div id="test"></div>
</div>
