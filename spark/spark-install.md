
# spark 설치 및 기본 conf 세팅

## pre-installed
>  설치는 ubuntu 환경입니다.

1.  java 설치 
 - sudo apt install openjdk-8-jdk
 - JAVA_HOME 설정
    - ~/.profile or ~/.bashrc 이동후 java path 성정
    - 설정후 soruce {path} 실행을 통해 적용후 shell에서 echo $JAVA_HOME 테스트
  
2 spark 설치
 - wget http://apache.tt.co.kr/spark/spark-2.4.0/spark-2.4.0-bin-hadoop2.7.tgz
 - tar xvfz spark-2.4.0-bin-hadoop2.7.tgz 
 - ln -Tfs spark-2.4.0-bin-hadoop2.7 spark
 - spark-cassandra connector 다운로드
 - wget http://dl.bintray.com/spark-packages/maven/datastax/spark-cassandra-connector/2.4.0-s_2.11/spark-cassandra-connector-2.4.0-s_2.11.jar
 - 폴더생성 
   - mkdir pid work
 - 라이브러리 설치
 -- wget http://www.java2s.com/Code/JarDownload/jsr166e/jsr166e-1.0.0.jar.zip
 -- spark/jars 폴더로 이동, unzip  

 ## conf 세팅
 > 세팅을 안하게 되면 기본 값이 적용됩니다.

  1. cp conf/spark-defaults.conf.template conf/spark-defaults.conf
```sh
 spark.master                     spark://{ip}:7077
 spark.eventLog.enabled           true
 spark.eventLog.dir               /spark/logs
 spark.serializer                 org.apache.spark.serializer.KryoSerializer
 spark.driver.memory              144g /default 메모리
 spark.driver.cores               10 / default 코어
 spark.jars                       jars/spark-cassandra-connector-2.3.0-s_2.11.jar
 spark.jars.packages              datastax:spark-cassandra-connector:2.3.0-s_2.11
 spark.cassandra.connection.host  {ip}
 spark.cassandra.auth.username    {id}
 spark.cassandra.auth.password    {pw}
 # spark.deploy.defaultCores        4
```

2. cp conf/spark-defaults.conf.template conf/spark-env.conf
```sh
export SPARK_LOCAL_IP={ip}
export SPARK_MASTER_HOST={ip}
export SPARK_MASTER_IP={ip}
export SPARK_WORKER_CORES=1
export SPARK_WORKER_MEMOEY=1g
export SPARK_WORKER_INSTANCES=4
export SPARK_WORKER_DIR=${SPARK_HOME}/work
export SPARK_PID_DIR=${SPARK_HOME}/pid

 # worker setting & master setting 
```
3. conf 적용
```sh
sbin/spark-config.sh
conf/spark-env.sh
```

4 실행
> [추가 argument ](https://spark.apache.org/docs/latest/spark-standalone.html)

>  아래 코드 실행후에 {ip}:8080에 접속하게 되면 spark ui 확인가능

>  bin/spark-shell or SPAKR_HOM 설정을 하였다면 spark-shell을 실행하게 되면 {ip}:4040 접속하게 되면 마스터 spark ui 확인가능
```sh
$master
sbin/start-master.sh
sbin/stop-master.sh

$slave
sbin/start-slave.sh  -c 4 spark://{ip}:7077
sbin/stop-slave.sh

bin/spark-shell --master spark://172.16.2.238:7077 --total-executor-cores 1 # conf에서 적용 한것이 아닌 직접 적용가능
```