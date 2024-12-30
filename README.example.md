# redis_cluster

## requirements

```agsl
redis 7.2.3
```

## installment
```
1. 인스턴스 내 docker 설치
2. 아래 파일과 밑으로 설명하는 파일은 모두 한 폴더에 업로드 
- redis-server.yml 
- entrypoint.sh
- redis-start.sh

3. 아래 파일은 클러스터 설정을 할 인스턴스에 업로드
- cluster-setting-mesh.sh 

4. 클러스터 토폴로지에 맞도록 conf 파일 업로드
- conf 설정은 아래 참조
```


## conf 파일 설정 
```agsl
1. 띄우고자 하는 포트 갯수 별로 conf 파일을 복사
e.g. 아래 이미지 기준으로는 각 인스턴스 별로 
 6300, 6301, 6302, 6400, 6401, 6402 포트로 각각 노드를 
 구동해야 하며 이에 따라 6개 conf 파일을 작성해야 한다. 
 (devptConf, demoptConf 폴더 내 파일 참조)   

2. 설정해야 하는 항목 
# 외부 통신시 알려줄 현재 인스턴스 공인 IP
cluster-announce-ip 
# 외부 통신시 알려줄 현재 인스턴스 포트번호
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
출처: https://co-de.tistory.com/24 [코알못:티스토리]
```

### 토폴로지 예시
![redis-cluster down moments full mash.jpg](redis%2Fimages%2Fredis-cluster%20down%20moments%20full%20mash.jpg)