Kafka-php
==========

[English Document](README.md)

[![Packagist](https://img.shields.io/packagist/dm/luxin88/kafka-php.svg?style=plastic)]()
[![Packagist](https://img.shields.io/packagist/dd/luxin88/kafka-php.svg?style=plastic)]()
[![Packagist](https://img.shields.io/packagist/dt/luxin88/kafka-php.svg?style=plastic)]()
[![GitHub issues](https://img.shields.io/github/issues/luxin88/kafka-php.svg?style=plastic)](https://github.com/luxin88/kafka-php/issues)
[![GitHub forks](https://img.shields.io/github/forks/luxin88/kafka-php.svg?style=plastic)](https://github.com/luxin88/kafka-php/network)
[![GitHub stars](https://img.shields.io/github/stars/luxin88/kafka-php.svg?style=plastic)](https://github.com/luxin88/kafka-php/stargazers)
[![GitHub license](https://img.shields.io/badge/license-Apache%202-blue.svg?style=plastic)](https://raw.githubusercontent.com/luxin88/kafka-php/master/LICENSE)

Kafka-php 使用纯粹的PHP 编写的 kafka 客户端，目前支持 0.8.x 以上版本的 Kafka，该项目 v0.2.x 和 v0.1.x 不兼容，如果使用原有的 v0.1.x 的可以参照文档 [Kafka PHP v0.1.x Document](https://github.com/weiboad/kafka-php/blob/v0.1.6/README.md), 不过建议切换到 v0.2.x 上。v0.2.x 使用 PHP 异步执行的方式来和kafka broker 交互，较 v0.1.x 更加稳定高效, 由于使用 PHP 语言编写所以不用编译任何的扩展就可以使用，降低了接入与维护成本


## 安装环境要求

* PHP 版本大于 7.1
* Kafka Server 版本大于 0.8.0
* 消费模块 Kafka Server 版本需要大于 0.9.0

## Installation

## 使用 Composer 安装

添加 composer 依赖 `luxin88/kafka-php` 到项目的 `composer.json` 文件中即可，如：

```
{
	"require": {
		"luxin88/kafka-php": "0.2.*"
	}
}
```

## 配置

配置参数见 [配置](docs/Configure.md)

## Produce

### 异步回调方式调用

```php
<?php
require '../vendor/autoload.php';
date_default_timezone_set('PRC');
use Monolog\Logger;
use Monolog\Handler\StdoutHandler;
// Create the logger
$logger = new Logger('my_logger');
// Now add some handlers
$logger->pushHandler(new StdoutHandler());

$config = \Kafka\ProducerConfig::getInstance();
$config->setMetadataRefreshIntervalMs(10000);
$config->setMetadataBrokerList('10.13.4.159:9192');
$config->setBrokerVersion('0.9.0.1');
$config->setRequiredAck(1);
$config->setIsAsyn(false);
$config->setProduceInterval(500);
$producer = new \Kafka\Producer(function() {
	return array(
		array(
			'topic' => 'test',
			'value' => 'test....message.',
			'key' => 'testkey',
		),
	);
});
$producer->setLogger($logger);
$producer->success(function($result) {
	var_dump($result);
});
$producer->error(function($errorCode) {
	var_dump($errorCode);
});
$producer->send(true);
```

### 同步方式调用生产者

```
<?php
require '../vendor/autoload.php';
date_default_timezone_set('PRC');
use Monolog\Logger;
use Monolog\Handler\StdoutHandler;
// Create the logger
$logger = new Logger('my_logger');
// Now add some handlers
$logger->pushHandler(new StdoutHandler());

$config = \Kafka\ProducerConfig::getInstance();
$config->setMetadataRefreshIntervalMs(10000);
$config->setMetadataBrokerList('127.0.0.1:9192');
$config->setBrokerVersion('0.9.0.1');
$config->setRequiredAck(1);
$config->setIsAsyn(false);
$config->setProduceInterval(500);
$producer = new \Kafka\Producer();
$producer->setLogger($logger);

for($i = 0; $i < 100; $i++) {
        $result = $producer->send(array(
                array(
                        'topic' => 'test1',
                        'value' => 'test1....message.',
                        'key' => '',
                ),
        ));
        var_dump($result);
}
```

## Consumer

```php
<?php
require '../vendor/autoload.php';
date_default_timezone_set('PRC');
use Monolog\Logger;
use Monolog\Handler\StdoutHandler;
// Create the logger
$logger = new Logger('my_logger');
// Now add some handlers
$logger->pushHandler(new StdoutHandler());

$config = \Kafka\ConsumerConfig::getInstance();
$config->setMetadataRefreshIntervalMs(10000);
$config->setMetadataBrokerList('10.13.4.159:9192');
$config->setGroupId('test');
$config->setBrokerVersion('0.9.0.1');
$config->setTopics(array('test'));
//$config->setOffsetReset('earliest');
$consumer = new \Kafka\Consumer();
$consumer->setLogger($logger);
$consumer->start(function($topic, $part, $message) {
	var_dump($message);
});
```

## Basic Protocol

基础协议 API 调用方式见 [Example](https://github.com/luxin88/kafka-php/tree/master/example)
