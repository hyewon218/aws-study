# RDS란? / RDS를 왜 사용하는걸까? / 현업에서의 RDS
## ✅ RDS(Relational Database Service)란? 
> 한 줄 요약 : 관계형 데이터베이스 서비스이다.

MySQL, MariaDB 등 여러 관계형 데이터베이스 서비스를 AWS로부터 빌려서 사용하는 형태이다. 

## ✅ RDS(Relational Database Service)를 왜 사용하는 걸까? 
로컬 환경(ex. 내 노트북)에서 개발할 때는 로컬 환경에 설치된 MySQL와 같은 DB를 연결해서 사용한다.<br> 
하지만 서버를 배포하고 나면 서버가 내 컴퓨터에 설치된 MySQL과 연결을 할 수가 없다.<br> 
DB도 외부 인터넷에서 접근할 수 있게 같이 배포해주어야 한다.<br>
이러한 이유 때문에 AWS RDS라는 데이터베이스를 빌려서 사용하는 것이다.<br> 
이 외에도 AWS RDS는 여러 편리한 부가기능(자동 백업, 모니터링, 다중 AZ 등)을 많이 가지고 있다.

## ✅ EC2에 MySQL을 직접 설치해서 운영하면 안 되나요? 
AWS에 대해 공부하면서 검색하다보면 하나의 EC2에 백엔드 서버와 MySQL을 같이 설치해서 쓰는 걸 볼 수 있다.<br> 
즉, RDS를 사용하지 않고 EC2에 MySQL을 직접 설치해서 사용하기도 한다.

EC2에 MySQL을 설치하면 별도의 RDS의 비용이 나오지 않기 때문에 비용을 절감할 수 있다.<br> 
비용의 이점 때문에 학생 때 자주 활용했던 구성 방법이다.<br> 
하지만 **실제 현업에서는 하나의 EC2에 백엔드 서버와 MySQL을 같이 사용하지 않는다.**<br> 
왜냐하면 **백엔드 서버에 장애로 인해 EC2 컴퓨터가 죽을 경우, 애꿎은 MySQL도 같이 죽기 때문이다.**<br> 
또한 `RDS`가 제공하는 **부가적인 편리한 기능**이 많아서 MySQL을 직접 설치해서 쓰지 않고 RDS를 쓰기도 한다.

정리하자면 현업에서는 **EC2와 RDS를 분리**해서 인프라를 구성하는 경우가 대부분이다.<br> 
하지만 비용이 부담된다면 하나의 EC2에 백엔드 서버와 MySQL을 직접 설치해서 사용해도 무방하다. 

<br>

# RDS를 활용한 아키텍처 구성
## ✅RDS를 활용한 아키텍처 구성
<img src="https://github.com/hyewon218/kim-jpa2/assets/126750615/03a6e86f-6ca8-4911-b97e-f17dd524939b" width="40%"/><br>

<br>

# [실습] 1. RDS 생성하기
## ✅ 1. 리전 선택
<img src="https://github.com/hyewon218/kim-jpa2/assets/126750615/c9de88d9-137b-4c1f-9e87-8b33631fe738" width="30%"/><br>

## ✅ 2. 데이터베이스 종류 선택
<img src="https://github.com/hyewon218/kim-jpa2/assets/126750615/6e1b08c6-b972-452a-9e5a-ca927d2000bf" width="60%"/><br>

## ✅ 3. 템플릿 선택
<img src="https://github.com/hyewon218/kim-jpa2/assets/126750615/1d00bcf7-855c-4717-92ca-54c121b63305" width="40%"/><br>
프리 티어를 선택하면 된다.
여기서 많이들 오해하는 게 프리 티어는 학습할 때나 테스트할 때만 쓰는 안 좋은 사양의 컴퓨터라고 생각한다.<br> 
하지만 실제 서비스에서 활용해도 될 정도로 나름 괜찮은 사양이다. <br> 
하루 방문자 수가 2,000명 정도였던 서비스를 운영했었는데 문제 없이 잘 돌아갔다. 성능에 문제가 직접적으로 생기기 전까지는 너무 걱정하지 말자. 

## ✅ 4. 설정
<img src="https://github.com/hyewon218/kim-jpa2/assets/126750615/24263bd7-b750-43fd-a8d6-2286899e1212" width="60%"/><br>
- 마스터 사용자 이름과 마스터 암호는 데이터베이스에 접근하기 위한 아이디와 비밀번호와 같은 값이다. 따라서 마스터 사용자 이름과 마스터 암호는 따로 적어두자.

## ✅ 5. 인스턴스 구성, 스토리지
(기본값으로 그대로 두자.)

## ✅ 6. 연결
<img src="https://github.com/hyewon218/kim-jpa2/assets/126750615/63eb2d8f-b702-4fd3-b251-277b6e647be8" width="60%"/><br>
나중에 보안에 조금 더 신경써서 구성하고 싶을 때는 `퍼블릭 액세스`를 아니오로 체크하고 RDS를 만들 때도 있다.<br> 
하지만 입문할 때는 불편하기 때문에 퍼블릭 액세스를 예로 두고 많이 사용한다. 

이렇게 설정하면 보안적으로 치명적인 위험이 있지 않을까 걱정하는 분들이 있다. 생각보다 그렇진 않다.<br> 
그러니 안심하고 퍼블릭 액세스를 예로 체크하고 RDS를 만들어서 연습하자.

## ✅ 7. 데이터베이스 인증, 모니터링
(기본값으로 그대로 두자.)

## ✅ 8. 비용
프리 티어일 경우 아래에서 책정된 금액 정도의 비용이 나올 일은 없으니 걱정하지 말자.<br>
(아주 약간의 비용은 나올 수 있다.)<br>
<img src="https://github.com/hyewon218/kim-jpa2/assets/126750615/52d21b32-610c-4b9a-8a06-0cc7c97db739" width="60%"/><br>

<br>

<img src="https://github.com/hyewon218/kim-jpa2/assets/126750615/ec41871c-5961-4227-9bb3-6228af70381f" width="60%"/><br>
<img src="https://github.com/hyewon218/kim-jpa2/assets/126750615/a75ee80a-da68-4af9-8113-f74f7c16e912" width="60%"/><br>

<br>

# [실습] 2. 보안그룹 설정하기
## ✅ 1. 보안그룹 생성하기

### ‘AWS EC2 - 보안 그룹’ 메뉴 선택
<img src="https://github.com/hyewon218/kim-jpa2/assets/126750615/b9e3c2e8-9b20-4d89-89ff-152d26ff04a7" width="20%"/><br>

### 인바운드 규칙, 아웃바운드 규칙 설정하기
별도의 설정을 하지 않았다면 RDS의 MySQL은 3306번 포트에서 실행된다. DB에 접근하기 위해 `3306`번 포트를 인바운드 규칙에 추가해주자.<br>
아웃바운드 규칙에는 모든 트래픽을 허용하는 규칙을 추가해주자.<br>
<img src="https://github.com/hyewon218/kim-jpa2/assets/126750615/9e548a36-f36f-43dc-a651-8416f11705be" width="60%"/><br>


## ✅ 2. 생성한 보안그룹을 RDS에 붙이기
<img src="https://github.com/hyewon218/kim-jpa2/assets/126750615/ad92d76d-9e99-4f75-aacd-91a87573fe49" width="60%"/><br>
<img src="https://github.com/hyewon218/kim-jpa2/assets/126750615/6a571d52-7908-49e2-95a5-7d35581699d3" width="60%"/><br>

<br>

## [실습] 3. 파라미터 그룹 추가하기
### ✅ 파라미터 그룹 생성하기
<img src="https://github.com/hyewon218/kim-jpa2/assets/126750615/e7cc63f3-03be-4fdb-8f63-5b81447a0c84" width="20%"/><br>
**1. 아래 속성 전부 `utf8mb4`로 설정하기**<br>
<img src="https://github.com/hyewon218/kim-jpa2/assets/126750615/1d0f04a1-f621-406d-a9b8-1a16f7b38412" width="100%"/><br>
- `character_set_client`
- `character_set_connection`
- `character_set_database`
- `characater_set_filesystem`
- `characater_set_results`
- `character_set_server`

**참고)** `utf8` 대신에 `utf8mb4`를 사용하는 이유는 ‘한글’ 뿐만 아니라 ‘이모티콘’도 지원이 가능하도록 하기 위해서이다.

**2. 아래 속성 전부 `utf8mb4_unicode_ci`로 설정하기**
<img src="https://github.com/hyewon218/kim-jpa2/assets/126750615/1874065b-87fc-4426-b315-a83a2b02dc00" width="100%"/><br>
- `collation_connection`
- `collation_server`

**참고)** `utf8mb4_unicode_ci`은 정렬, 비교 방식을 나타낸다.

**3. `time_zone`을 `Asia/Seoul`로 설정하기**
<img src="https://github.com/hyewon218/kim-jpa2/assets/126750615/05a9679f-fedf-449b-a066-503daed3bac4" width="100%"/><br>

### ✅ RDS의 파라미터 그룹 변경하기
RDS의 DB 인스턴스 수정을 통해 DB 파라미터 그룹 변경하면 된다.
<img src="https://github.com/hyewon218/kim-jpa2/assets/126750615/0d4fc5cc-f30c-4eba-a8fa-fa02296502f3" width="60%"/><br>
<img src="https://github.com/hyewon218/kim-jpa2/assets/126750615/e1d99803-a6ba-402d-a2e9-2e258929fa23" width="60%"/><br>

**주의) DB 파라미터 그룹을 변경한 뒤에는 RDS의 DB를 재부팅해야만 정상적으로 적용된다.**
