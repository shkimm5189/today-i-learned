# Go

## Go Path
```
Go는 대표적인 중앙 관리소가 없다
ex) Pyhton의 pip,java의 maven,gradle
```
아래 명령어를 통해 설정한 Go path를 볼수있다.
```
go env
```

여기서 볼것은 **GOPATH**와 **GOROOT**이다.
```
GOPATH : 'go get' 명령어를 통해 받은 라이브러리,소스가 위치하는 곳이다.
기본적으로 bin,pkg,src 폴더가 위치한다.
*없으면 만들어주면 됨.

GOROOT : Go를 설치하면 관련된 실행 파일과 SDK등이 위치하는곳이다.
```

### GO 워크 스페이스 (관련 설정)
프로젝트 생성 후 반드시 ``bin``, ``pkg``, ``src`` 3개의 폴더를 생성해야한다.
- bin : 소스 컴파일 후 운영체제별 실행 가능한 바이너리 파일이 저장됨.
- pkg : 프로젝트에 필요한 패키지가 컴파일 되어 라이브러리파일이 저장.
- src : 직접 작성한 소스 코드 및 오픈 소스 코드 저장.


### 함수
Go에서 파라미터 전달 방식은 크게
- Pass By Value

- Pass By Reference
