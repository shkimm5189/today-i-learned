# Django
python도 처음으로 다뤄봤고 Django로 웹사이트 제작은 처음이다. 하나씩 가보자

# 기본 환경 세팅
```
anaconda navigator : 2.0.3 (파이썬 가상환경)
python : 3.8.10 (개발 당시 최신버전)
Django : 3.2.3
```

# Django 파일 구조 분석
```
urls.py -  페이지 요청시 url과 뷰함수를 1:1로 연
```
DB MYSQL로 변경
```
db_setting.py 파일 생성 후

# DB 확인
python manage.py inspectdb

```


# 추가한 앱 URL mapping
```
config/urls.py는 기본 설정이기때문에 이제 App에서 사용될 추가 url 주소들은 앱자체의 urls.py를 추가하여 관리 하도록 한다.
```

# ORM 사용의 장단점
```
장점
1. 동일한 목적을 같는 쿼리문도 개발자마다 다를수있기때문에 통일성이 깨질수있다.
2. 개발자가 쿼리문을 잘못 작성하게 되면 시스템의 성능이 저하될수있다.
3. DB를 변경하면 특정 DB에 의존하는 모든것을 수정 해야 하므로 유지보수가 어려울수있다.

```
> 개인 생각 : ORM은 개념 자체가 어렵고 러닝커브가 높기 떄문에 이해가 잘된 상태에서 시스템에 적용해야 한다고 생각한다. 성능적 측면에서는 sql을 그냥 타이핑 해넣는게 나을수있기때문이다. N+1문제나 orm을 적용했을 때 나타나는 성능적 문제를 충분히 고려해야한다고 생각한다.


# django에서 Rest Api 사용
```
django에서는 django 서버와 별개로 rest 서버가 운영 되어야 하기때문에 rest api 처리용 app을 추가한다.

app은 기능별로 쪼개놓은것 정도로 이해하면 좋다.
```




# 서버 분할
centos 7 - mysql 5.7
```
순서
1. mysql server 설치
2. mysql 외부 접속 허용 계정 추가
# 모든 대역의 ip 허용
grant all privileges on *.* to 'root'@'%' identified by '비밀번호'

flush privileges

# 유저 확인하는 법
use mysql
select user, host from user
% root 있어야함.

3. 방화벽 mysql 포트 번호 추가
 firewall-cmd --add-port=3306/tcp --permanent
```


centos 7  - django client
```
centos 7 에서 python 설치시 장고 설치 버전과 맞아야하고 파이프 버전도 맞아야한다. (당ㅡ연)

wget https://www.python.org/ftp/python/3.8.10/Python-3.8.10.tgz

pip는 다른 레포지토리를 활성화 시켜야함.
yum install epel-release

#pip3 설치
yum install python3-pip
# django 설치
pip3 install django

# django rest framework 설치
pip3 install djangorestframework

#django rest_auth 설치
pip3 install django-rest-auth
# all auth 설치
django install django-allauth


# mysql client 설치
yum install python3-devel mysql-devel
pip3 install mysql.connector
pip3 install mysqlclient


https://galid1.tistory.com/318 httpd modwsgi 서버 설정

error
core.wsgi를 못찾는 경우
 yum install python3-mod_wsgi

권한이 없다고 하는경우
chmod로 실행 권한을 준다.
selinux 해제

실행 안된다면
 vi /var/log/httpd/error_log 에서 확인하며 트러블 슈팅
```

centos - apache server
```
yum install -y httpd

```
