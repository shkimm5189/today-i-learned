# 프로젝트 진행하면서 겪은 문제

### App의 migrations 폴더에 있는 내용을 전부 삭제 했을때
```
migrations안에 있는 파일들은 삭제에 유의 해야한다. 한번 삭제 해버리면 되돌릴수없다.

내 경우 DB를 인식해도 mysql에 반영되지않았다.

1. __init__.py 파일이 있는지 확인
2. SET SQL_SAFE_UPDATES = 0; # safe 모드 해제

3. delete from django_migrations where app="rest_api_board"; # 해당 App의 migrations 초기화

```
