Kafka-php 配置参数
==================

| Property	                 | C/P	 | Range	             | Default   | Desc                                                                        |
|---------------------------|------|--------------------|-----------|-----------------------------------------------------------------------------|
| clientId                  | C/P  |                    | kafka-php | 客户端标识                                                                       |
| brokerVersion             | C/P  | 大于 0.8.0           | 0.10.1.0  | 为了计算 Kafka 请求的协议版本                                                          |
| metadataBrokerList        | C/P  |                    |           | 指定 Kafka Broker 列表，多个用逗号分隔                                                  |
| messageMaxBytes           | C/P  | 1000 .. 1000000000 | 1000000   | 消息最大长度                                                                      |
| metadataRequestTimeoutMs  | C/P  | 10 .. 900000       | 60000     | 获取 meta 信息超时时间                                                              |
| metadataRefreshIntervalMs | C/P  | 10 .. 3600000      | 300000    | 获取同步 meta 信息的时间间隔                                                           |
| metadataMaxAgeMs          | C/P  | 1 .. 86400000      | -1        | meta 信息有效期                                                                  |
| sslEnable                 | C/P  | true/false         | false     | 是否开启 Ssl 连接                                                                 |
| sslEnableAuthentication   | C/P  | true/false         | true      | 通过SSL启用身份验证                                                                 |
| sslLocalCert              | C/P  | File path          |           | 本地证书路径                                                                      |
| sslLocalPk                | C/P  | File path          |           | 如果使用独立的文件来存储证书（local_cert）和私钥， 那么使用此选项来指明私钥文件的路径。                           |
| sslVerifyPeer             | C/P  | true/false         | false     | 是否需要验证 SSL 证书。                                                              |
| sslPassphrase             | C/P  |                    |           | local_cert 文件的密码。                                                           |
| sslCafile                 | C/P  |                    |           | 当设置 sslVerifyPeer 为 true 时， 用来验证远端证书所用到的 CA 证书。 本选项值为 CA 证书在本地文件系统的全路径及文件名。 |
| sslPeerName               | C/P  |                    |           | 要连接的服务器名称。如果未设置，那么服务器名称将根据打开 SSL 流的主机名称猜测得出。                                |
| groupId                   | C    |                    |           | 消费模块的分组 ID                                                                  |
| sessionTimeout            | C    | 1 .. 3600000       | 30000     | 分组中消费者的有效时间                                                                 |
| rebalanceTimeout          | C    | 1 .. 3600000       | 30000     | 分组 rebalance 等待 join 时间                                                     |
| topics                    | C    |                    |           | 将要消费的 kafka topic 名称                                                        |
| offsetReset               | C    | latest,earliest    | latest    | 如果消费 offset 失效的时候重置 offset 的策略                                              |
| maxBytes                  | C    |                    | 65536     | 单次 FETCH 请求对于单个分区请求的最大字节数                                                   |
| maxWaitTime               | C    |                    | 100       | 等待服务端响应 FETCH 请求的最大时间                                                       |
| requiredAck               | P    | -1 .. 1000         | 1         | 生产消息确认策略                                                                    |
| timeout                   | P    | 1 .. 900000        | 5000      | 生产消息请求超时时间                                                                  |
| isAsyn                    | P    | true, false        | false     | 是否采用异步生产消息                                                                  |
| requestTimeout            | P    | 1 .. 900000        | 6000      | 消费消息整体超时时间, 该值一定要大于 timeout 参数                                              |
| produceInterval           | P    | 1 .. 900000        | 100       | 在异步生产消息时执行生产消息的请求的时间间隔                                                      |

#### 注意

如上所有的参数均通过 setXxx 方式设置，例如要设置 `clientId` 参数

```php
<?php

$config = \Kafka\ConsumerConfig::getInstance();
$config->setClientId('test');
```

无论是消费模块还是生产模块，如果参数设置不符合规则时都会抛 `\Kafka\Exception\Config` 异常
