# build docker

docker를 이해하기 위한 예제

## description

[go syscall doc](https://pkg.go.dev/syscall)에 따르면 `$GOOS`와 `$GOARCH`에 따라 syscall의 인터페이스가 달라집니다.  
해당 repo는 linux only field를 사용하므로 envChange.sh를 통해 환경 변수를 변경합니다.

based on : https://www.infoq.com/articles/build-a-container-golang/
