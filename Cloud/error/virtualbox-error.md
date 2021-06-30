# Vgrant error

```
There was an error while executing VBoxManage, a CLI used by Vagrant for controlling VirtualBox. The command and stderr is shown below.

Command: ["startvm", "dff6693e-52c8-4c9e-922a-243d18c7f666", "--type", "headless"]

Stderr: VBoxManage: error: The VM session was closed before any attempt to power it on VBoxManage: error: Details: code NS_ERROR_FAILURE (0x80004005), component SessionMachine, interface ISession
```

vagrant up 명령어를 쳤을때 이러한 문구가 나온다면

1. Virtaulbox 재설치
2. Virtaulbox 확장 프로그램을 설치

---
