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
GOPATH : 'go get' 명령어를 통해받은 라이브러리,소스가 위치하는 곳이다.
기본적으로 bin,pkg,src 폴더가 위치한다. 
*없으면 만들어주면 됨.

GOROOT : Go를 설치하면 관련된 실행 파일과 SDK등이 위치하는곳이다.
```