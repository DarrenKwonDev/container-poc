# container

docker를 이해하기 위한 예제 및 도커와 쿠버네티스에 대한 정확하지 않은 잡썰

## description

[go syscall doc](https://pkg.go.dev/syscall)에 따르면 `$GOOS`와 `$GOARCH`에 따라 syscall의 인터페이스가 달라집니다.  
해당 repo는 linux only field를 사용하므로 envChange.sh를 통해 환경 변수를 변경합니다.

based on : https://www.infoq.com/articles/build-a-container-golang/

## OCI & CRI

OCI(open container initiative) → **저수준 컨테이너 런타임 표준 구성**

- 컨테이너 기술의 표준이 없었음. 그래서 여러 이해 관계자(클라우드 벤더사 및 기타 컨테이너 기술 회사)가 모여서 컨테이너 포맷과 런타임에 대한 표준을 만듦
- 컨테이너 돌아가려면 리눅스의 cgroup과 namespace가 필요한데 이들을 다루는 방법은 시스템마다 다르고, 커널 버전마다 다르기 때문에 LXC(Linux Container)나 libvirt같은 중간 매개채를 통해 간접적으로 관리했음.
- LXC나 libvirt는 외부 라이브러리라서 불안함. kernel의 가상화를 위한 인터페이스를 자체적으로 만들고자 하였고 libcontainer를 거쳐 runC가 나왔다.
- 결론적으론 OCI를 준수하는 런타임으로 runC가 대표적.
- https://github.com/opencontainers/runc

CRI(container runtime interface) → **고수준 컨테이너 런타임 표준 구성**

- https://kubernetes.io/blog/2016/12/container-runtime-interface-cri-in-kubernetes/
- 쿠버에서 컨테이너를 돌리려면 CRI를 준수해야 해. 안 그럼 우리가 도커나 특정 기타 컨테이너 기술사한테 끌려가서 쿠버가 발전을 못 할거야.
- 그러면 CRI 준수하는 컨테이너 런타임 만들면 도커 아니어도 쿠버 위에서 돌아가겠네? → CRI-O 등 컨테이너 런타임이 개발됨.
  - 예전엔 dockershim이라는 브릿지 서비스가 docker API와 CRI의 변환을 해주었는데 이제 이게 필요 없어졌음. 주도권이 바뀐 거임.
  - CRI 준수하는 컨테이너 런타임은 CRI-O와 containrd(docker) 임.
  - docker engine을 통째로 쓰는 것보다 containerd를 사용하니 더 가벼워짐.
  - 도커로 빌드된 이미지들 역시 스펙을 만족하기 때문에 쿠버에서 문제 없이 돌아감. 다만 주도권이 도커가 아니라 도커 개발진들이 따라가야 함.
