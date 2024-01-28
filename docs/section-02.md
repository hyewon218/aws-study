# 도메인 연결하기 (Route 53)

## ✅ Route 53이란? 
> 💡한 줄 요약 : 도메인을 발급하고 관리해주는 서비스이다. 

혹시나 도메인(Domain)의 의미를 모르는 분들을 위해 설명드리자면,<br> 
www.naver.com, daum.net, youtube.com과 같은 문자로 표현된 인터넷 주소를 의미한다. 

Route 53을 조금 더 전문적인 용어로 표현하자면 **DNS(Domain Name System) 서비스**이다. 

> DNS가 뭔지 알아보자. 

## ✅ DNS(Domain Name System)란?
도메인(Domain)이 없던 시절에는 특정 컴퓨터와 통신하기 위해서 IP 주소(ex. 12.134.122.11)를 사용했다.<br> 
이 **IP**는 **특정 컴퓨터를 가리키는 주소의 역할**을 한다. 

하지만 IP 주소는 많은 숫자들로 이루어져 있어서 일일이 외우기가 너무 불편했다.<br> 
이 때문에 사람들이 기억하기 쉬운 문자로 컴퓨터의 주소를 나타낼 수는 없을까에 대해 고민하게 됐다.<br> 
숫자로 이루어져 있는 IP 주소를 문자로 구성하기에는 한계가 있었다.<br> 
왜냐하면 컴퓨터가 처리하기 쉬운 값의 형태는 문자가 아니라 숫자이기 때문이다. 

이를 해결하기 위해 **문자를 IP 주소로 변환해주는 하나의 시스템(서버)** 을 만들게 됐다. 이게 바로 **DNS(Domain Name System)** 이다.<br>
DNS가 생기고나서부터 사람들은 특정 컴퓨터와 통신하기 위해 복잡한 IP 주소를 일일이 외울 필요가 없게 됐다.

<img src="https://github.com/hyewon218/kim-jpa2/assets/126750615/2c85f97e-8715-40e8-80ed-3c388eb8546e" width="60%"/><br>

## ✅ 현업에서의 Route53 활용 여부
프론트 웹 페이지든 백엔드 서버든 일반적으로 IP를 기반으로 통신하지 않고 도메인을 기반으로 통신한다.<br> 
이유는 여러가지지만 그 이유 중 하나는 HTTPS 적용 때문이다. **IP 주소에는 HTTPS 적용을 할 수가 없다**.<br> 
도메인 주소가 있어야만 HTTPS 적용을 할 수 있다. 이 때문에 특정 서비스를 운영할 때 도메인은 필수적으로 사용하게된다.

하지만 DNS의 역할을 하는 서비스는 AWS Route 53 뿐만 아니라 다양하게 존재한다.<br> 
가비아(gabia), 후이즈(whois) 등에서도 도메인을 구매하고 관리할 수 있다. 즉, DNS의 역할을 서비스를 하고 있다는 뜻이다.

현업에서는 무조건적으로 AWS Route 53을 고집하진 않는다. 왜냐면 각 서비스마다 구매할 수 있는 도메인의 종류가 다르기 때문이다.<br> 
쉽게 예를 들어, 가비아(gabia)에는 내가 원하는 형태의 도메인이 있는데 AWS Route 53에는 없을 수도 있다.<br> 
따라서 내가 원하는 도메인이 존재하는 곳의 DNS 서비스를 활용하자.

<br>

# [실습] 1. Route53에서 도메인 구매
## ✅ Route 53에서 도메인 구매
1.
<img src="https://github.com/hyewon218/kim-jpa2/assets/126750615/41ecdc06-cdaf-4ac0-86b8-d3613a9bf553" width="60%"/><br>
2.
<img src="https://github.com/hyewon218/kim-jpa2/assets/126750615/3a9563ce-5e12-4480-b7a6-62d740e92b9d" width="60%"/><br>
3.
<img src="https://github.com/hyewon218/kim-jpa2/assets/126750615/2bf2ca47-59f9-409b-b0fa-ac7c67cf6054" width="60%"/><br>
4.
<img src="https://github.com/hyewon218/kim-jpa2/assets/126750615/780afe11-a476-4223-86f2-072d7667a9e8" width="60%"/><br>

주의) 다른 건 몰라도 이메일은 정확히 입력해야 한다. 10~20분 있다가 등록한 이메일로 메일이 날라온다. 이 메일을 확인해야만 정상적으로 도메인이 등록된다.
<img src="https://github.com/hyewon218/kim-jpa2/assets/126750615/ef63e230-f17a-4099-a4e8-80b700ea9a25" width="60%"/><br>

5.
<img src="https://github.com/hyewon218/kim-jpa2/assets/126750615/5826a8cb-6e2d-46f4-8785-4e767d0b60bb" width="60%"/><br>

## ✅ 도메인 구매 잘 됐는지 확인하기

[호스팅 영역]<br>
<img src="https://github.com/hyewon218/kim-jpa2/assets/126750615/540854da-6796-44b6-8c10-6d8fa8a77bca" width="60%"/><br>
[등록된 도메인]<br>
<img src="https://github.com/hyewon218/kim-jpa2/assets/126750615/f373115d-d596-4c9c-84f2-f9cefee56df7" width="60%"/><br>
위와 같이 등록된 도메인은 도메인을 구매한 시점으로부터 10~20분 뒤에 등록된다. 

<br>

# [실습] 2. Route53의 도메인을 EC2에 연결하기
## ✅ Route53의 도메인을 EC2에 연결하기
1. Route 53의 `호스팅 영역` 메뉴에 들어가서 `레코드 생성` 버튼 누르기
<img src="https://github.com/hyewon218/kim-jpa2/assets/126750615/b98bb6e2-2f49-42a8-87d0-b6540c631942" width="60%"/><br>
<img src="https://github.com/hyewon218/kim-jpa2/assets/126750615/c8e3a2ad-af3c-4a16-bfb1-6c69500b4eab" width="60%"/><br>

2. 레코드 생성하기<br>
<img src="https://github.com/hyewon218/kim-jpa2/assets/126750615/603aec61-7f4b-42db-a6eb-03dcdf2b2fb5" width="60%"/><br>
<img src="https://github.com/hyewon218/kim-jpa2/assets/126750615/e6e92380-63cd-470d-8454-ed0966f478e1" width="60%"/><br>

위와 같이 설정할 경우 jscode-edu.link의 도메인으로 접속했을 때 52.79.34.240의 IP 주소로 연결시켜준다는 뜻이다. 레코드 유형은 아래에서 자세히 설명하겠다.

만약 api.jscode-edu.link의 도메인으로 접속했을 때 52.79.34.240의 IP 주소로 연결시켜주고 싶다면 레코드 이름의 빈칸에 api를 적으면 된다. 이와 같이 하나의 도메인만 구매하더라도 여러 개의 서브 도메인을 사용할 수 있다. (jscode-edu.link가 주 도메인이고, ___.jscode-edu.link 형태의 도메인이 서브 도메인이다.)

3. 잘 접속되는지 확인하기
> A 레코드로 추가하고나서 도메인이 적용되는데 5~10분 정도 걸린다. 그러고 도메인에 접속해서 테스트해보자

<img src="https://github.com/hyewon218/kim-jpa2/assets/126750615/93554917-cd2c-4644-9afe-ed1ee863fc28" width="60%"/><br>

## ✅ 레코드 유형
DNS에 공통적으로 있는 설정 중 하나가 레코드 유형이다. 많은 레코드 유형이 있지만 2가지(A 레코드, CNAME 레코드)만 확실히 알고 있으면 충분하다. [파레토의 법칙]
<img src="https://github.com/hyewon218/kim-jpa2/assets/126750615/567703ec-c508-4644-90f5-621cc51bb044" width="60%"/><br>

1. **A 레코드**<br>
   도메인을 **특정 IPv4 주소에 연결시키고 싶을 때 사용**하는 레코드 유형이다.

2. **CNAME 레코드**<br>
   도메인을 **특정 도메인 주소에 연결시키고 싶을 때 사용**하는 레코드 유형이다.
   만약 CNAME 레코드의 값으로 `www.naver.com`을 적었다고 가정하자. 그러면 해당 도메인으로 접속했을 때, `www.naver.com`으로 연결되어 이동한다.