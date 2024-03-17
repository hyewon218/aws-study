# 파일 및 이미지 업로드 (S3)

## S3를 활용한 아키텍처 구성
### ✅ 이미지 파일 업로드 과정
<img src="https://github.com/hyewon218/kim-jpa2/assets/126750615/5077ca4c-c380-4259-b71c-3699522e9a40" width="80%"/><br>


### ✅ 이미지 파일 다운로드 과정
<img src="https://github.com/hyewon218/kim-jpa2/assets/126750615/cd5e6741-1d38-4633-834f-dddce511e251" width="80%"/><br>

## [실습] 1. S3 버킷 생성하기
### ✅ S3에서만 사용하는 용어 정리
S3에서는 버킷(Bucket)과 객체(Object)라는 용어를 사용한다. 이 용어에 대해 알아보자.
> 버킷(Bucket)이란?
> ⇒ 깃헙(Github)을 보면 여러 개의 Repository를 만들 수 있다. S3에서도 여러 개의 저장소를 만들 수 있다. 
> 여기서 하나의 저장소를 버킷(Bucket)이라고 부른다. 

> 객체(Object)이란?
> ⇒ S3에 업로드한 파일을 보고, S3에서는 파일(File)이라 부르지 않고 객체(Object)라고 부른다. 
> 즉, 객체(Object)란 S3 버킷에 업로드된 파일을 의미한다. 


### ✅ 1. S3 버킷 생성하기

### ✅ 2. 버킷에 정책 추가하기
> 정책(Policy)이란 ?
> ⇒ 권한(Permission)을 정의하는 JSON 문서를 의미한다. AWS는 기본적으로 대부분의 권한이 주어져있지 않다. 
> AWS의 특정 소스에 접근하려면 권한을 허용해주어야 한다. 권한을 허용할 때 작성해야 하는 게 정책(Policy)이다. 

> 특정 서비스에서 상품 이미지를 모든 사용자한테 보여주고 싶다고 가정해보자. 그러면 버킷에서 상품 이미지를 다운로드해서 사용할 수 있어야 한다. 
> 버킷에서 이미지 파일을 조회할 수 있게 정책을 추가해보겠다.

## [실습] 2. S3에 파일 업로드 할 수 있도록 IAM에서 액세스 키 발급받기

