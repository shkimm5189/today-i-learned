# 사용할 DB Setting
> 테스트시 localhost에만 접근하고 이 세팅값은 깃허브에 ginore에 포함 시켜서 볼수없게 해야한다.
유출 될 경우 진짜 ㅈ될수있다.  
```
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',  # 사용할 DB
        'NAME': 'django_test',                 # database 이름
        'USER': 'django',                        # 접속 계정
        'PASSWORD': 'test',              # 접속 계정 비밀번호
        'HOST': 'localhost',                    # DB 주소
        'PORT': '3306'                         # port
    }
}
```

mysql 8.0 이상 버전부터 보안이 강화 되었기 때문에 권한을 지정해주어야한다.

```sql
# 루트 계정으로 접속
mysql -u root -p

# 생성하 계정에 권한 부여
grant all privileges on *.* to 'django'@localhost identified by 'test';
```
