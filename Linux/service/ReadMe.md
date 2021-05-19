# Linux Service
# 1. SELinux

## 동작모드
1. Enforcing
- 커널 모듈을 메모리에 로드.
- 활성화 상태
- 권한,컨텍스트와 부울을 확인하고 필요시 차단
2. Permissive
- 커널 모듈을 메모리에 로드
- 활성화 되긴 하지만 정책을 강요하진않는다.
- 정책 위반시 경고 메시지만 남김.
3. Disabled
- 커널 모듈을 메모리에 로드하지않는다.
- 완전한 비활성화 상태
> 컨텍스트 : 파일/디렉토리에 대한 접근제어
부울 : 서비스 동작별 제어(스위치)
포트 : 네트워크 포트별 서비스 할

> /etc/selinux/config 에서 동작모드를 설정할수있다.
특히, disabled로 되어있다면 커널에 올라가지 않아 setenforce명령어 자체가 동작하지않는다.

## selinux 명령어 (chcon)
1. chcon
2. restorecon
3. semanage fcontext


각 파일에 대한 컨텍스트 설정
semanage fcontext -a -t "context" "dir/.*" 기본값 변경

restorecon -RFv "dir" : 해당 디렉토리와 그 하위 파일의 컨텍스트를 기본값으로 설정

포트에 대한 레이블 설정
semanage port -a -t "Label" port -p tcp/udp 특정 포트 번호에 이름 부여
