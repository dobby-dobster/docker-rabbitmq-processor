# docker-rabbitmq-processor

This is a docker deployment of RabbitMQ with Python producer and consumer.

## Deployment

1. Clone this repo
```bash
git clone https://github.com/dobby-dobster/docker-rabbitmq-processor.git
```
2. Deploy
```bash
cd docker-rabbitmq-processor
docker-compose up -d
```
3. Validate
Three containers should now be running
  - docker-rabbitmq-processor_rabbitmq_1
  - docker-rabbitmq-processor_producer_1
  - docker-rabbitmq-processor_consumer_1
```bash
docker container ls | grep docker-rabbitmq
```
You can browse to http://0.0.0.0:15672 and login as guest/guest to access the Rabbit management UI.

![logo](Rabbit_Screenshot.png)

## Containers
- docker-rabbitmq-processor_rabbitmq_1 - standard rabbitmq container
- docker-rabbitmq-processor_producer_1 - producer container, runs send.py which generates a 32 randaom character string and publishes message to queue (RandomStrings). Sleeps for 1 secoond before publishing next message.
docker-rabbitmq-processor_consumer_1 - consumer container, runs fetch.py which consumes messages from the queue (RandomStrings).

Producer and consumer are based on centos8 image. Image totals to around 400MB vs Python image is over 900MB.

## Logs
```bash
docker logs docker-rabbitmq-processor_producer_1 | head
```
```
Random string generated: RBXF9CJPKNBCFTBX6S75U5QQ41HBKMH3
Random string (RBXF9CJPKNBCFTBX6S75U5QQ41HBKMH3) sent
Sleeping for 1 seconds..
Random string generated: RJ0GVCD2X97FPSJATG3N675MRQVF3DWZ
Random string (RJ0GVCD2X97FPSJATG3N675MRQVF3DWZ) sent
Sleeping for 1 seconds..
Random string generated: 08X2BDMAPIC36Y301T6RTOV0RXWJACPA
Random string (08X2BDMAPIC36Y301T6RTOV0RXWJACPA) sent
Sleeping for 1 seconds..
```
```bash
docker logs docker-rabbitmq-processor_consumer_1 | head
```
```
[*] Waiting for messages. To exit press CTRL+C
[x] Received b'NB254LOQOU88BHMTP06S2V47AUQODYB2'
[x] Received b'VN2EDMC2WU8YILOVDMI5VGGID4EX5UM4'
[x] Received b'CZ32S1D22QBH7P9ACJCBT7HXRNC5VX6U'
[x] Received b'CLT0HRW7QY3JNDLO7J3O0M3U8KVXED9X'
[x] Received b'LPS804O2BD2Q85XXNNDTN4VX5DZ0TX26'
[x] Received b'65JSFUYADV4CH3T961C2SHN1110ECUSQ'
[x] Received b'5ZRCCA1N8WG9F65LLZVS6STSKPZR7YY2'
```
