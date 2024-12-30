# redis_cluster

## requirements

```agsl
redis 7.2.3
```

## 설치 폴더 구조
```markdown
ThreeInstances
--- - A-Instance 
    |
    - B-Instance
    |
    - C-Instance
```

## installment
```
1. 서버 내 docker 를 설치한다.

2. .conf 파일을 각 서버에 맞도록 PUBLIC_IP 로 적힌 부분을 공인 아이피로 치환해준다.
- conf 설정은 아래 참조

3. ThreeInstances 폴더 내 파일들 각각 서버 별로 분배하되 서버 별로 redis폴더를 
별도 만들어서 해당 폴더 밑으로 넣는다.

4. 서버 3대를 기준으로 각각 아래 포트 번호를 인바운드로 허용한다.
- A: 6300, 6400, 16300, 16400
- B: 6301, 6401, 16301, 16401
- C: 6302, 6402, 16302, 16402

5. redis 폴더 안에 logs 폴더 생성한다.

6. redis-start.sh 과 cluster-setting-three-instance.sh 에 chmod 755 정도로 실행 권한을 
부여한다.

7. redis-start.sh 실행 시켜 docker container를 띄운다.

8. cluster-setting-three-instance.sh 파일 내 A,B,C 서버의 (공인)IP 를 각각 기재한다.
(A,B,C-SERVER-IP)

9. cluster-setting-three-instance.sh 를 실행시켜서 클러스터 구성을 완료한다.
```


## [참고]conf 파일 설정 
#### - commonConf 하위 conf 파일 참조
```agsl
1. 설정해야 하는 항목 
# 외부 통신시 알려줄 현재 서버 공인 IP
cluster-announce-ip 
# 외부 통신시 알려줄 현재 서버 포트번호
cluster-announce-port
# redis 노드 포트번호
port [각자포트]
# 백그라운드에서 시작하도록 설정
daemonize yes
# 클러스터를 사용하겠다.
cluster-enabled yes 
# 클러스터 구성 내용을 저장한는 파일명 지정 (자동 생성됨)
cluster-config-file nodes-[각자포트].conf 
# 클러스터 노드가 다운되었는지 판단하는 시간 (e.g. 3s)
cluster-node-timeout 3000 
# Appendonly를 yes로 설정하면 rdb에 저장 안되고 aof에 저장됨 (각각 장단점이 있으니 해당 부분은 선택 사항)
appendonly yes 
# append only yes 시 해당 부분도 수정
appendfilename appendonly_[각자포트].aof 
# 프로세스 아이디 저장 경로 설정
pidfile /var/run/redis_[각자포트].pid
# 로그 파일 저장 경로 지정
logfile logs/redis_[각자포트].log
```

### 토폴로지 예시
![redis-cluster-for-three-instances.jpg](redis%2Fimages%2Fredis-cluster-for-three-instances.jpg)