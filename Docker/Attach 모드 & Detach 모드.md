<details>
  <summary>Attach 모드  &  Detach 모드</summary>

### Attach 모드
attach 모드는 도커 컨테이너의 표준 입력(stdin), 표준 출력(stdout), 표준 오류(stderr) 스트림에 연결하여 직접 상호작용하는 모드입니다. 이 모드를 사용하면 실행 중인 컨테이너의 콘솔에 접속하여 실시간으로 로그를 확인하거나 입력을 전달할 수 있습니다.

사용 예시: docker attach <container_id> 또는 docker attach <container_name>을 사용한다.<br/>
특징:
컨테이너의 현재 실행 상태를 확인하고, 실시간으로 로그를 볼 수 있다.
터미널에서 직접 명령어를 입력할 수 있다.
Ctrl+C를 누르면 컨테이너가 종료된다.


### Detach 모드
detach 모드는 컨테이너를 백그라운드에서 실행하며, 사용자가 직접 컨테이너의 콘솔에 접속할 필요가 없는 모드입니다. 이 모드는 주로 서버 애플리케이션이나 백그라운드 작업을 실행할 때 사용된다.

사용 예시: docker run -d <image> 또는 docker run --detach <image>를 사용한다.<br/>
특징:
컨테이너가 백그라운드에서 실행되므로 터미널을 점유하지 않는다.
컨테이너의 로그를 확인하거나 상호작용하려면 docker logs, docker exec 등을 사용해야 한다.
Ctrl+C를 눌러도 컨테이너는 계속 실행된다.
</details>

