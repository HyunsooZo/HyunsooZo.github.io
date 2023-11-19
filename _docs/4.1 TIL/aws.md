---
title: Aws 스왑
category: TIL
order: 12
---

### 문제점

리눅스에서 메모리 상태 확인하는 명령어 : `free -h`

프리티어 사용 시 EC2의 메모리는 1GB이다.<br>
이 정도 용량의 메모리로는 젠킨스 에서 도커 이미지를 만들어 컨테이너에서 `.jar` file 실행 시 서버가 죽어버리는 현상이 나타난다. <br>
메모리 증설 또는 스왑을 통해해결하능하며 해당 포스팅에서는 스왑을 사용한다.

### 해결방법 (swap)

```bash
$ sudo dd if=/dev/zero of=/swapfile bs=128M count=16
$ sudo chmod 600 /swapfile
$ sudo mkswap /swapfile
$ sudo swapon /swapfile
$ sudo swapon -s
$ sudo vi /etc/fstab
## vi 편집기 파일을 연 후, 파일 끝에 이것을 추가한 후,저장한 다음 종료
/swapfile swap swap defaults 0 0
```

`dd` 명령을 사용하여 루트 파일 시스템에 스왑 파일을 생성한다.<br>
`bs` : 블록 크기,<br> `count` : 블록의 수
지정한 블록 크기는 인스턴스에서 사용 가능한 메모리보다 작아야 한다. (그렇지 않을 시, memory exhausted 오류가 발생한다.)<br>
`chmod 600 /swapfile` - 스왑 파일에 대한 읽기 및 쓰기 권한을 업데이트<br>   
`mkswap /swapfile` - Linux 스왑 영역을 설정한다.<br>
`swapon /swapfile` - 스왑 공간에 스왑 파일을 추가하여 스왑 파일을 즉시 사용할 수 있도록 만든다 <br>
`swapon -s`절차가 성공했는지 확인<br>
`vi /etc/fstab/etc/fstab` 파일을 편집하여 부팅 시 스왑 파일을 활성화<br>
