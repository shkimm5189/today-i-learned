# Kafka Setup
[상호 지원 가능한 버전](https://docs.confluent.io/platform/current/installation/versions-interoperability.html)
- Practice environment
  - java version: java 1.8  (최소 8 or 11)
  - apache kafka 2.8.0 (latest ver: 2.8.0)

## VM environment (vagrant)
```ruby
Vagrant.configure("2") do |config|
  config.vm.define "kafka" do |config|
    config.vm.box = "ubuntu/focal64"
    config.vm.provider "virtualbox" do |vb|
      vb.name = "kafka"
      vb.cpus = 2
      vb.memory = 4000
      unless File.exist?('./.disk/ceph1.vdi')
       vb.customize ['createmedium', 'disk', '--filename', './.disk/ceph1.vdi', '--size', 10240]
      end
      vb.customize ['storageattach', :id, '--storagectl', 'SCSI', '--port', 2, '--device', 0, '--type', 'hdd', '--medium',
'./.disk/ceph1.vdi']
    end
    config.vm.hostname = "kafka"
    config.vm.network "private_network", ip: "192.168.100.100"
  end
end
```

## 실행 순서
zookeeper On -> kafka Server On -> topic create

``bin/zookeeper-server-start.sh config/zookeeper.properties``

``bin/kafka-server-start.sh config/server.properties``


## server.properties
```bash
# The id of the broker. This must be set to a unique integer for each broker.
# broker의 id. 각 broker는 고유한 정수값으로 설정 되어야함.
broker.id=0


# The address the socket server listens on. It will get the value returned from
# java.net.InetAddress.getCanonicalHostName() if not configured.
#   FORMAT:
#     listeners = listener_name://host_name:port
#   EXAMPLE:
#     listeners = PLAINTEXT://your.host.name:9092
#listeners=PLAINTEXT://:9092

# Hostname and port the broker will advertise to producers and consumers. If not set,
# it uses the value for "listeners" if configured.  Otherwise, it will use the value
# returned from java.net.InetAddress.getCanonicalHostName().
#advertised.listeners=PLAINTEXT://your.host.name:9092

# Maps listener names to security protocols, the default is for them to be the same. See the config documentation for more details
#listener.security.protocol.map=PLAINTEXT:PLAINTEXT,SSL:SSL,SASL_PLAINTEXT:SASL_PLAINTEXT,SASL_SSL:SASL_SSL

# The number of threads that the server uses for receiving requests from the network and sending responses to the network
# 네트워크로 부터 요청을 수신하고 네트워크에 응답을 보내는 서버가 사용하는 Thread의 갯수 / 기본값 3
num.network.threads=3

# The number of threads that the server uses for processing requests, which may include disk I/O
# I/O가 생길 때, 생성되는 Thread의 갯수 / 기본값 8
num.io.threads=8

# The send buffer (SO_SNDBUF) used by the socket server
# 소켓 서버가 사용하는 송신 버퍼의 크기
socket.send.buffer.bytes=102400

# The receive buffer (SO_RCVBUF) used by the socket server
# 소켓 서버가 사용하는 수신 버퍼의 크기
socket.receive.buffer.bytes=102400

# The maximum size of a request that the socket server will accept (protection against OOM)
# 서버가 받을 수있는 최대 요청 사이즈/ 서버 메모리가 고갈되는것을 방지
# JAVA의 Heap 메모리 보다 작게 설정함. / 기본값 104857600
socket.request.max.bytes=104857600

# A comma separated list of directories under which to store log files
# 로그 파일이 저장되는 디렉토리 (쉼표로 구분하여 나눔)
log.dirs=/home/vagrant/kafka_2.12-2.8.0/data/kafka

# The default number of log partitions per topic. More partitions allow greater
# parallelism for consumption, but this will also result in more files across
# the brokers.
# 토픽당 로그 파티션의 수
# 숫자가 커질수록 병렬처리를 할수있지만 그만큼 브로커에 파일도 늘어나게됨.
# 기본값 1
num.partitions=1

# The number of threads per data directory to be used for log recovery at startup and flushing at shutdown.
# This value is recommended to be increased for installations with data dirs located in RAID array.
num.recovery.threads.per.data.dir=1


# The replication factor for the group metadata internal topics "__consumer_offsets" and "__transaction_state"
# For anything other than development testing, a value greater than 1 is recommended to ensure availability such as 3.
# 내부 Topic "__consumer_offsets" , "__transaction_state" 에 대한 복제 수
# 개발환경에서는 1, 운영환경에서는 1이상 권장 (3 정도)
offsets.topic.replication.factor=1
transaction.state.log.replication.factor=1
transaction.state.log.min.isr=1


# Messages are immediately written to the filesystem but by default we only fsync() to sync
# the OS cache lazily. The following configurations control the flush of data to disk.
# There are a few important trade-offs here:
#    1. Durability: Unflushed data may be lost if you are not using replication.
#    2. Latency: Very large flush intervals may lead to latency spikes when the flush does occur as there will be a lot of data to flush.
#    3. Throughput: The flush is generally the most expensive operation, and a small flush interval may lead to excessive seeks.
# The settings below allow one to configure the flush policy to flush data after a period of time or
# every N messages (or both). This can be done globally and overridden on a per-topic basis.

# The number of messages to accept before forcing a flush of data to disk
#log.flush.interval.messages=10000

# The maximum amount of time a message can sit in a log before we force a flush
#log.flush.interval.ms=1000


# The following configurations control the disposal of log segments. The policy can
# be set to delete segments after a period of time, or after a given size has accumulated.
# A segment will be deleted whenever *either* of these criteria are met. Deletion always happens
# from the end of the log.

# The minimum age of a log file to be eligible for deletion due to age
# 세그먼트 파일의 삭제 주기 (7일)
log.retention.hours=168

# A size-based retention policy for logs. Segments are pruned from the log unless the remaining
# segments drop below log.retention.bytes. Functions independently of log.retention.hours.
#log.retention.bytes=1073741824

# The maximum size of a log segment file. When this size is reached a new log segment will be created.
# 로그 세그먼트 파일의 최대 크기 (1GB)
# 해당 용량에 다다르면 새로운 로그 세그먼트 생성
log.segment.bytes=1073741824

# The interval at which log segments are checked to see if they can be deleted according
# to the retention policies
# 보존 정책에 따라 세그먼트 파일의 삭제 여부를 체크하는 주기 (기본값 5분)
log.retention.check.interval.ms=300000

############################# Zookeeper #############################

# Zookeeper connection string (see zookeeper docs for details).
# This is a comma separated host:port pairs, each corresponding to a zk
# server. e.g.
# 주키퍼의 접속 정보
# 쉼표로 연결 서버를 추가 할수있음 (IP주소:포트) "127.0.0.1:3000,127.0.0.1:3001,127.0.0.1:3002".
# You can also append an optional chroot string to the urls to specify the
# root directory for all kafka znodes.
# 모든 kafka znode의 root dir
zookeeper.connect=localhost:2181

# Timeout in ms for connecting to zookeeper
zookeeper.connection.timeout.ms=18000


############################# Group Coordinator Settings #############################

# The following configuration specifies the time, in milliseconds, that the GroupCoordinator will delay the initial consumer rebalance.
# The rebalance will be further delayed by the value of group.initial.rebalance.delay.ms as new members join the group, up to a maximum of max.poll.interval.ms.
# The default value for this is 3 seconds.
# We override this to 0 here as it makes for a better out-of-the-box experience for development and testing.
# However, in production environments the default value of 3 seconds is more suitable as this will help to avoid unnecessary, and potentially expensive, rebalances during application startup.
# 개발 환경: 테스트를 위해 0으로 설정
# 운영 환경: 3초의 기본값을 두는게 좋음
group.initial.rebalance.delay.ms=0
```
