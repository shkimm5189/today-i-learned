# kubectl apt-versions
해당 클러스터에서 지원되는 API 버전확인
``kubectl api-versions``
해당 클러스터에서 지원되는 API 리소스 확인
``kubectl api-resources``

- alpha
```
버그가 있을수 있음
기본적으로 비활성화
기술지원이 언제든지 중단될수있음
사용하지 않길 권장
```
- beta
```
테스트 되었고 활성화해도됨
기본적으로 활성화
내용이 변경될수있으나 기술지원이 중단되않음
다음버전에서 호환되지 않을수있으나 이전할수있는 가이드 제공
API 오브젝트의 삭제,편집,재생성이 필요할수있음 -> 다운타임 발생가능성
다음 릴리즈에서 호환되지 않을수있으니 중요한곳에서 사용 x
```


## Object 정의  .yaml

```
kubectl explain pod
정의 시 반드시 볼것

계층적 구조로 되어있으므로 단계적으로 찾아볼수있다.
kubectl explain pod.spec.containers
정의 가능한 필드
- apiVersion
- kind
- metadata
- spec
status는 kubernetes에서 정의함.
<[]Object> -> list 형식
```
