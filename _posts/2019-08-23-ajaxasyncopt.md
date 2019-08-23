---
title: "AjaxCall의 Async가 false일 때"
categories: 
  - frontEnd
last_modified_at: 2018-08-23T23:45:00+09:00
tags:
  - ajax
  - async
toc: true
---

## Explain

ajax로 여러가지를 하다보니 다음과 같은 현상이 있었다.

1. 기본적으로 ajaxcall을 할 때 사용하는 XMLHttpRequest.open에서 async 옵션을 false로 줘도 ajaxcall 이전의 것은 painting을 한다.

2. 1의 조건에서 onreadystatechange에 alert시 painting을 하지 않는다.  

3. 1의 조건에서 서버측이 thread.sleep을 가지고 있는 경우 ajaxcall이전의 것도 그려지지 않는다.(painting이 안됨)

3번을 막으려면 아마도 setTimeOut으로 따로 ajaxcall을 큐에 넣어 앞의 것을 painting 해야하지 않을까 싶은데..


## Example
1번 - onreadystatechange를 실행하기 전 red로 배경색이 칠해짐
```markdown 
<div style="height:100px; background-color:red"></div>
<div id='main'></div>
<script type="text/javascript">
  const ajax = () => {
    var request = new XMLHttpRequest();
    
    request.onreadystatechange = function ()  {
      if (this.readyState == 4 && this.status == 200){ 
        // alert(1);
        document.querySelector("#main").innerText = this.responseText
      }
    }
    request.open("GET", "./test.jsp", false); // 동기 상태
    request.send();
  }
  ajax();  
</script>
```

2번 - onreadystatechange를 실행하기 전 red로 배경색이 칠해지지 않음
```markdown 
<div style="height:100px; background-color:red"></div>
<div id='main'></div>
<script type="text/javascript">
  const ajax = () => {
    var request = new XMLHttpRequest();
    
    request.onreadystatechange = function ()  {
      if (this.readyState == 4 && this.status == 200){ 
        alert(1);
        document.querySelector("#main").innerText = this.responseText
      }
    }
    request.open("GET", "./test.jsp", false); // 동기 상태
    request.send();
  }
  ajax();  
</script>
```

3번 - onreadystatechange를 실행하기 전 red로 배경색이 칠해지지 않음
```markdown 
// 클라이언트
<div style="height:100px; background-color:red"></div>
<div id='main'></div>
<script type="text/javascript">
  const ajax = () => {
    var request = new XMLHttpRequest();
    
    request.onreadystatechange = function ()  {
      if (this.readyState == 4 && this.status == 200){
        document.querySelector("#main").innerText = this.responseText
      }
    }
    request.open("GET", "./test.jsp", false); // 동기 상태
    request.send();
  }
  ajax();  
</script>

// 서버
<%@ page language="java" contentType="text/html; charset=UTF-8"  pageEncoding="UTF-8"%>
<%
  request.setCharacterEncoding("utf-8");
  response.setContentType("text/html");
  response.setStatus(200);
  
  Thread.sleep(5000)
  String result = "받았어요";
  
  out.print(result);
  
  
%>
```