---
title: RabbitMQ报错(406, "PRECONDITION_FAILED - parameters for queue 'test_queue' in vhost '/test' not equivalent")
date: 2020-03-18 11:01:00
tags: 前端
categories: 其他
---
- 报错如下

  ```python
  File "C:\projects\project_name\mq\rabbitclient.py", line 26, in __init__
  self.channel.queue_declare(queue=conf.queue_name)
  File "C:\env\project_name\lib\site-packages\pika\adapters\blocking_connection.py", line 2507, in queue_declare
  self._flush_output(declare_ok_result.is_ready)
  File "C:\env\project_name\lib\site-packages\pika\adapters\blocking_connection.py", line 1340, in _flush_output
  raise self._closing_reason # pylint: disable=E0702
  
  pika.exceptions.ChannelClosedByBroker: (406, "PRECONDITION_FAILED - parameters for queue 'test_queue' in vhost '/test' not equivalent")
  ```

- 代码如下

  ```python
  # -*- coding: utf-8 -*-
   
  import json
  import pika
  import Config
   
   
  class RabbitMQClient(object):
      """
      RabbitMQ Client
      """
      def __init__(self):
          conf = Config()
          credentials = pika.PlainCredentials(conf.rabbit_user, conf.rabbit_password)
          self.conn = pika.BlockingConnection(pika.ConnectionParameters(
              host=conf.rabbit_ip,
              port=conf.rabbit_port,
              virtual_host=conf.vhost,
              credentials=credentials
          ))
   
          self.channel = self.conn.channel()
          self.exchange = conf.exchange
          self.routing_key = conf.routing_key
          self.channel.exchange_declare(exchange=self.exchange, exchange_type='direct', durable=True)
          self.channel.queue_declare(queue=conf.queue_name)
          self.channel.queue_bind(
              queue=conf.queue_name,
              exchange=self.exchange,
              routing_key=self.routing_key
          )
   
      def send(self, msg):
          try:
              self.channel.basic_publish(
                  exchange=self.exchange,
                  routing_key=self.routing_key,
                  body=json.dumps(msg),
                  properties=pika.BasicProperties(
                      delivery_mode=2
                  )
              )
          except Exception as e:
              raise e
   
      def close(self):
          self.conn.close()
   
   
  if __name__ == '__main__':
          client = RabbitMQClient()
          client.send(exec_info)
          client.close()
  ```

- 定位问题

  ```
  我们在声明exchange的时候加了如下参数：durable=True，代表持久化；
  发布信息的时候加了如下参数：delivery_mode=2，代表持久化消息；
  我们再看声明queue的时候如何？
  在rabbitmq中，想要重启后不丢失消息，要为信息加delivery_mode=2参数，只为消息加持久化限制，MQ重启之后，exchange和queue全部丢失，也是不行的，所以也要为exchange和queue做持久化，都是由durable=True控制。
  
  所以，修改上述代码第26行如下：
  self.channel.queue_declare(queue=conf.queue_name, durable=True)
  再次测试便不报错了。
  ```

  