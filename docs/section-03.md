# HTTPS 연결하기 (ELB)
## ELB란? / TLS, SSL과 HTTPS
### ✅ ELB(Elastic Load Balancer)란? 
> 한 줄 요약 : 트래픽(부하)을 적절하게 분배해주는 장치이다. 

트래픽(부하)를 적절하게 분배해주는 장치를 보고 전문적인 용어로 로드밸런서(Load Balancer)라고 부른다.<br> 
서버를 2대 이상 가용할 때 ELB를 필수적으로 도입하게 된다. 

<img src="https://github.com/hyewon218/kim-jpa2/assets/126750615/b2044505-6f5b-4f91-8f0b-eb949f3ef7b0" width="30%"/><br>

> 하지만 지금은 ELB의 로드밸런서 기능을 사용하지 않고, ELB의 부가 기능인 **SSL/TLS(HTTPS)를 적용시키는 방법**에 대해 배울 것이다.

### ✅ SSL/TLS란 ? 
**SSL/TLS** 쉽게 표현하자면 **HTTP를 HTTPS로 바꿔주는 인증서**이다.
위에서 말했다시피 **ELB**는 **SSL/TLS 기능**을 제공한다고 했다.<br> 
**SSL/TLS 인증서를 활용해 HTTP가 아닌 HTTPS로 통신할 수 있게 만들어준다.**

### ✅ HTTPS ? 
> HTTPS를 적용시켜야 하는 이유는 무엇일까? 
1. **보안적인 이유**<br>
데이터를 서버와 주고 받을 때 **암호화**를 시켜서 통신을 한다. 암호화를 하지 않으면 누군가 중간에서 데이터를 가로채서 해킹할 수도 있다. 보안에 좋지 않다.
2. **사용자 이탈**<br>
어떤 사이트에 들어갔는데 아래와 같이 보인다면 왠지 믿음직스럽지 못한 사이트라고 생각할 것이다.<br>
<img src="https://github.com/hyewon218/kim-jpa2/assets/126750615/635cd71b-c5ed-4880-b0eb-225107c1633c" width="40%"/><br>

### ✅ 현업에서는 ?
대부분의 웹 사이트에서 HTTPS를 적용시킨다.<br>
<img src="https://github.com/hyewon218/kim-jpa2/assets/126750615/ce72aa66-eaf6-46ab-a3a6-cf931dbbad09" width="40%"/><br>
HTTPS 인증을 받은 웹 사이트가 백엔드 서버와 통신하려면, 백엔드 서버의 주소도 HTTPS 인증을 받아야 한다.<br> 
따라서 백엔드 서버와 통신할 때도 IP 주소로 통신하는 게 아니라, **HTTPS 인증을 받은 도메인 주소로 통신**을 한다. 

주로 도메인을 구성할 때 아래와 같이 많이 구성한다.
- 웹 사이트 주소 : `**https**://jscode-edu.co.kr`
- 백엔드 API 서버 주소 : `**https**://api.jscode-edu.co.kr`

<br>

## ELB를 활용한 아키텍처 구성
### ✅ ELB를 활용한 아키텍처 구성
<img src="https://github.com/hyewon218/kim-jpa2/assets/126750615/f4466d76-639d-4dbc-be70-2cc0fcf4b810" width="20%"/><br>
ELB 도입 전 아키텍처<br>
<img src="https://github.com/hyewon218/kim-jpa2/assets/126750615/77ffc65f-83d2-4090-b0c9-4e4adfb08f97" width="30%"/><img src="https://github.com/hyewon218/kim-jpa2/assets/126750615/470f0ee1-e531-4c9a-9f9a-ba4157baacf3" width="30%"/><br>
ELB 도입 후 아키텍처<br>

> ELB를 사용하기 전의 아키텍처는 사용자들이 EC2의 IP 주소 또는 도메인 주소에 직접 요청을 보내는 구조였다.<br>
> 하지만 ELB를 추가적으로 도입함으로써 사용자들이 EC2에 직접적으로 요청을 보내지 않고 `ELB`를 향해 요청을 보내도록 구성할 것이다.<br> 
> 그래서 EC2 달았던 **도메인도 ELB에** 달 것이고(ELB->EC2), **HTTPS도 ELB의 도메인에** 적용시킬 예정이다.

<br>

## [실습] 1. ELB 셋팅하기 - 기본 구성
### ✅ 1. 리전 선택하기
`AWS EC2` `로드밸런서` 서비스로 들어가서 리전(Region)을 선택해야 한다.<br>
<img src="https://github.com/hyewon218/kim-jpa2/assets/126750615/9faa5eb7-7662-4af3-af19-2c40fbc3dd92" width="20%"/><img src="https://github.com/hyewon218/kim-jpa2/assets/126750615/32ee4237-41fc-4d84-9623-184c36a84c4e" width="40%"/><br>

### ✅ 2. 로드 밸런서 유형 선택하기
**2.1. 로드 밸런서 생성하기**<br>
<img src="https://github.com/hyewon218/kim-jpa2/assets/126750615/bff145d7-7d90-469e-ae72-be5eddc66efc" width="60%"/><br>

**2.2. 로드 밸런서 유형 선택하기**
3가지 로드 밸런서 유형 중 Application Load Balancer(ALB)를 선택하면 된다.<br>
<img src="https://github.com/hyewon218/kim-jpa2/assets/126750615/ea91ef51-0b3d-43d7-b301-fe72a367b312" width="60%"/><br>
**참고)** **Application Load Balancer를 선택한 이유**를 간단하게 들자면 **HTTP, HTTPS에 대한 특징을 활용하기 위함**이다.<br>
Application Load Balancer, Network Load Balancer, Gateway Load Balancer의 차이를 아는 건 AWS 입문자 입장에서 크게 중요한 부분이 아니다.<br> 
그러니 Application Load Balancer를 선택한 이유가 이해되지 않아도 넘어가도 괜찮다.

### ✅ 3. 기본 구성
<img src="https://github.com/hyewon218/kim-jpa2/assets/126750615/ef0441f7-d2a1-4405-898a-501928c9b511" width="60%"/><br>
- **인터넷 경계**와 **내부**라는 옵션이 있다. **내부** 옵션은 Private IP를 활용할 때 사용한다.<br> 
   입문 강의에서는 VPC, Private IP에 대한 개념을 활용하지 않을 예정이라 **인터넷 경계** 옵션을 선택하면 된다.
- **IPv4**와 **듀얼 스택**이라는 옵션이 있다. IPv6을 사용하는 EC2 인스턴스가 없다면 **IPv4**를 선택하면 된다.<br>
   우리가 만든 EC2 인스턴스는 전부 IPv4로 이루어져 있다.
    - **참고) IPv4와 IPv6의 차이**
      IPv4 주소는 `121.13.0.5`와 같은 IP 주소를 의미한다. <br>
      그런데 IPv4 주소가 고갈될 것으로 예측하고 IPv6을 추가로 만들어낸다.<br> 
      IPv6은 IPv4보다 훨씬 더 많은 주소값을 만들어낼 수 있게 구성했다. IPv6의 형태는 `2dfc:0:0:0:0217:cbff:fe8c:0`와 같다.

### ✅ 4. 네트워크 매핑
로드 밸런서가 어떤 **가용 영역**으로만 트래픽을 보낼 건지 제한하는 기능이다.<br>
아직 가용 영역에 대한 개념을 배우지 않았다. AWS 입문자한테는 별로 중요한 개념이 아니다.<br>
가용 영역에 제한을 두지 않고 모든 영역에 트래픽을 보내게 설정하자. 즉, **모든 가용 영역에 다 체크하자.**<br>
<img src="https://github.com/hyewon218/kim-jpa2/assets/126750615/67e0945c-82bc-4297-a512-d441d79528a8" width="60%"/><br>

<br>

## [실습] 2. ELB 셋팅하기 - 보안그룹
### ✅ 보안 그룹
#### `AWS EC2 보안 그룹`에서 보안 그룹 생성하기
<img src="https://github.com/hyewon218/kim-jpa2/assets/126750615/dca1cb85-0ba1-4ea3-8561-8808594b8a0c" width="60%"/><br>
- ELB의 특성상 인바운드 규칙에 `80`(HTTP), `443`(HTTPS) 포트로 모든 IP에 대해 요청을 받을 수 있게 설정해야 한다.

#### ELB 만드는 창으로 돌아와서 보안 그룹 등록하기
<img src="https://github.com/hyewon218/kim-jpa2/assets/126750615/9d6ba7f9-e4f1-470c-89e7-3ca534a72f4f" width="60%"/><br>


<br>

## [실습] 3. ELB 셋팅하기 - 리스너 및 라우팅 / 헬스 체크
### ✅ 1. 대상 그룹(Target Group) 설정하기
리스너 및 라우팅 설정은 ELB로 들어온 요청을 **어떤 EC2 인스턴스에 전달할 건지**를 설정하는 부분이다. 

1. .<br>
<img src="https://github.com/hyewon218/kim-jpa2/assets/126750615/eda2299c-c446-4ca8-adb6-85fca9fab278" width="60%"/><br>
ELB로 들어온 요청을 ‘어떤 곳’으로 전달해야 하는데, 여기서 **‘어떤 곳’** 을 대상 그룹(Target Group)이라고 표현한다.<br> 
즉, ELB로 들어온 요청을 어디로 보낼지 대상 그룹을 만들어야 한다. 

2. 대상 유형 선택하기<br>
<img src="https://github.com/hyewon218/kim-jpa2/assets/126750615/a5ff594f-430e-4611-9b90-387e063de4b3" width="60%"/><br>
   EC2에서 만든 특정 인스턴스로 트래픽을 전달할 것이기 때문에 인스턴스 옵션을 선택한다. 

3. 프토토콜, IP 주소 유형, 프로토콜 버전 설정<br>
<img src="https://github.com/hyewon218/kim-jpa2/assets/126750615/62a054e6-86f9-48b1-9c62-6869b8c5be77" width="60%"/><br>

ELB가 사용자로부터 트래픽을 받아 대상 그룹에게 어떤 방식으로 전달할지 설정하는 부분이다. <br>
위 그림은 HTTP(HTTP1), 80번 포트, IPv4 주소로 통신을 한다는 걸 뜻한다. 이 방식이 흔하게 현업에서 많이 쓰이는 셋팅 방법이다. 

4. 상태 검사 설정하기<br>
<img src="https://github.com/hyewon218/kim-jpa2/assets/126750615/1e9cb141-b265-4323-bbe5-a05fb3fa7d68" width="60%"/><br>
> ELB의 부가 기능으로 상태 검사(= Health Check, 헬스 체크) 기능이 있다. 이 기능은 굉장히 중요한 기능 중 하나이므로 확실하게 짚고 넘어가자.<br>
>
> 실제 ELB로 들어온 요청을 대상 그룹에 있는 여러 EC2 인스턴스로 전달하는 역할을 가진다.<br> 
> (@ELB를 활용한 아키텍처 구성) 그런데 만약 특정 EC2 인스턴스 내에 있는 서버가 예상치 못한 에러로 고장났다고 가정해보자.<br> 
> 그럼 ELB 입장에서 고장난 서버한테 요청(트래픽)을 전달하는 게 비효율적인 행동이다. 
>
> 이런 상황을 방지하기 위해 ELB는 주기적으로(기본 30초 간격) 대상 그룹에 속해있는 각각의 EC2 인스턴스에 요청을 보내본다.<br> 
> 그 요청에 대한 200번대(HTTP Status Code) 응답이 잘 날라온다면 서버가 정상적으로 잘 작동되고 있다고 판단한다.<br> 
> 만약 요청을 보냈는데 200번대의 응답이 날라오지 않는다면 서버가 고장났다고 판단해서, ELB가 고장났다고 판단한 EC2 인스턴스로는 요청(트래픽)을 보내지 않는다.
> 
> 이러한 작동 과정을 통해 조금 더 효율적인 요청(트래픽)의 분배가 가능해진다. 
>
> 위에서 설정한 값을 해석해보자면, 대상 그룹의 각각의 EC2 인스턴스에 `GET /health`(HTTP 프로토콜 활용)으로 요청을 보내게끔 설정한 것이다.<br> 
> 정상적인 헬스 체크 기능을 위해 EC2 인스턴스에서 작동하고 있는 백엔드 서버에 Health Check용 API를 만들어야 한다. 뒤에서 곧 만들 예정이다. 

5. 대상 등록하기<br>
<img src="https://github.com/hyewon218/kim-jpa2/assets/126750615/89f5bc08-8890-49a0-8b6a-08715f9db1c0" width="60%"/><br>

6. ELB 만드는 창으로 돌아와서 대상 그룹(Target Group) 등록하기<br>
<img src="https://github.com/hyewon218/kim-jpa2/assets/126750615/758161e0-a948-41a0-bfc9-1589eaeec461" width="60%"/><br>
위 설정을 해석하자면 ELB에 HTTP를 활용해 80번 포트로 들어온 요청(트래픽)을 설정한 대상 그룹으로 전달하겠다는 의미이다. 

7. 로드 밸런서 생성하기<br>
나머지 옵션들은 그대로 두고 로드 밸런서를 생성하면 된다.<br>
<img src="https://github.com/hyewon218/kim-jpa2/assets/126750615/883f29ff-2f74-4cee-a9ee-760711f337a8" width="60%"/><br>


### ✅ 2. Health Check API 추가하기
위의 샘플 프로젝트처럼 ELB의 상태 검사(= Health Check, 헬스 체크)에 응답할 수 있는 API를 추가하자.<br>
그런 뒤에 EC2 인스턴스의 서버를 업데이트 시켜주자. 

### ✅ 3. 로드밸런서 주소를 통해 서버 접속해보기
<img src="https://github.com/hyewon218/kim-jpa2/assets/126750615/11cd78c0-4f10-41fe-aa98-54c3d6d286de" width="60%"/><br>
<img src="https://github.com/hyewon218/kim-jpa2/assets/126750615/f9d597f0-0676-41d6-bd72-159e8eaa3490" width="60%"/><br>

<br>

## [실습] 4. ELB에 도메인 연결하기
### ✅ 1. Route 53에서 EC2에 연결되어 있던 레코드 삭제
<img src="https://github.com/hyewon218/kim-jpa2/assets/126750615/4c1b07dc-d32c-4a63-917b-72470437afbf" width="60%"/><br>
### ✅ 2. Route 53에서 ELB에 도메인 연결하기
<img src="https://github.com/hyewon218/kim-jpa2/assets/126750615/0defceff-2bc6-429e-9ed1-415c98bbc8c8" width="60%"/><br>

<br>

## [실습] 5. HTTPS 적용을 위해 인증서 발급받기
> HTTPS를 적용하기 위해서는 인증서를 발급받아야 한다.
### ✅ 1. AWS Certificate Manager 서비스로 들어가서 인증서 요청 버튼 누르기
<img src="https://github.com/hyewon218/kim-jpa2/assets/126750615/551116d7-da26-4d82-8483-202e2e2d424c" width="60%"/><br>
### ✅ 2. 인증서 요청하기
<img src="https://github.com/hyewon218/kim-jpa2/assets/126750615/128499cd-c06f-4841-84fd-3098ed7e0848" width="60%"/><br>
<img src="https://github.com/hyewon218/kim-jpa2/assets/126750615/394b0560-3f7c-481b-9cee-2c862d8abdb9" width="60%"/><br>
### ✅ 3. 인증서 검증하기
내가 소유한 도메인이 맞는 지 검증하는 과정이다.<br>
**1.**<br>
<img src="https://github.com/hyewon218/kim-jpa2/assets/126750615/92411194-0adb-4979-9caf-105c8226ac90" width="60%"/><br>
<img src="https://github.com/hyewon218/kim-jpa2/assets/126750615/8dd30cd8-2720-4e86-a7de-7fb89731a12f" width="60%"/><br>

**2. 검증 완료**<br>
3분 정도 기다렸다가 AWS Certificate Manager 창을 새로고침하면 아래와 같이 검증이 완료된다. (길게는 10분 정도 소요될 때도 있다.)<br>
<img src="https://github.com/hyewon218/kim-jpa2/assets/126750615/2ec3e67f-8691-4710-8fe0-4209795d0919" width="60%"/><br>

<br>

## [실습] 6. ELB에 HTTPS 설정하기
### ✅1. ELB의 리스너 및 규칙 수정하기
1. **HTTPS에 대한 리스너 추가하기**<br>
<img src="https://github.com/hyewon218/kim-jpa2/assets/126750615/8e9a61f9-1abf-4b5d-91cf-3663e1e9e82f" width="60%"/><br>
<img src="https://github.com/hyewon218/kim-jpa2/assets/126750615/a45fb182-723d-4bf7-ad58-7ad78185d6d0" width="60%"/><br>
- 433 포트로 요청이 들어온다면 그 요청에 대해서 대상그룹에 트래픽을 전달해 주겠다.<br>
<img src="https://github.com/hyewon218/kim-jpa2/assets/126750615/dbb479e1-e0e4-463d-83b6-768abf5eff58" width="60%"/><br>
> 위와 같이 설정하면 HTTPS가 한 5초 정도 있다가 바로 적용된다.

### ✅2. HTTPS가 잘 적용됐는 지 확인하기
구매한 도메인에 https를 붙여서 접속해보자.<br>
<img src="https://github.com/hyewon218/kim-jpa2/assets/126750615/78a22afe-1b0f-4bbb-893b-7b68995c94e0" width="60%"/><br>

### ✅3. HTTP로 접속할 경우 HTTPS로 전환되도록 설정하기
아직까지 아쉬운 점은 http를 붙여서 접속할 경우 HTTPS를 사용하지 않고 접속이 가능하다는 점이다.<br> 
http를 붙여서 접속하더라도 자동으로 HTTPS로 전환(Redirect)되도록 만들어보자.<br>
<img src="https://github.com/hyewon218/kim-jpa2/assets/126750615/c4e1718e-362a-4a6b-a988-c912df00b029" width="60%"/><br>

1. 기존 HTTP:80 리스너를 삭제하기<br>
<img src="https://github.com/hyewon218/kim-jpa2/assets/126750615/044024c0-a0eb-40e3-a04a-8feaa44f7216" width="60%"/><br>
2. 리스너 추가하기<br>
<img src="https://github.com/hyewon218/kim-jpa2/assets/126750615/bfc2062c-be3b-469d-b070-36989418836f" width="60%"/><br>

### ✅4. HTTP로 접속해도 HTTPS로 잘 전환되는 지 확인하기
<img src="https://github.com/hyewon218/kim-jpa2/assets/126750615/4160101c-941f-4172-a3fe-3c063f3b7436" width="60%"/><br>
