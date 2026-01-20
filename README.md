# Kafka Diplom

## Технические инструкции

## Шаг 1. Запуск решения через Docker Compose.

docker compose --profile api up -d
[+] Running 6/6
 ✔ Network custom_network    Created                                                                                                                              0.0s 
 ✔ Container api             Started                                                                                                                              0.8s 
 ✔ Container kafka-1         Started                                                                                                                              0.5s 
 ✔ Container kafka-2         Started                                                                                                                              0.6s 
 ✔ Container kafka-0         Started                                                                                                                              0.6s 
 ✔ Container schemaregistry
 Started                  


docker compose logs api
api  | INFO:root:Сгенерирован и добавлен продукт: 54028
api  | INFO:httpx:HTTP Request: POST http://schemaregistry:8081/subjects/shop-products-value/versions?normalize=False "HTTP/1.1 200 OK"
api  | INFO:root:KAFKA: Сообщение на отправку в топик 'shop-products' поставлено в очередь.
api  | INFO:root:Все ожидающие сообщения успешно отправлены.
api  | INFO:root:FILE: Данные успешно записаны в файл 'data/products.json'.
api  | INFO:root:Сгенерирован и добавлен продукт: 35209
api  | INFO:root:KAFKA: Сообщение на отправку в топик 'shop-products' поставлено в очередь.
api  | INFO:root:Все ожидающие сообщения успешно отправлены.


```bash 
docker compose exec -it api bash
root@api:/opt/app# python api_client.py
\INFO:root:Успешное подключение CLIENT API к Kafka по адресу kafka-0:9092,kafka-1:9092,kafka-2:9092
INFO:root:Успешное подключение SHOP API к Kafka по адресу kafka-0:9092,kafka-1:9092,kafka-2:9092
INFO:root:Успешное подключение к Schema Registry по адресу http://schemaregistry:8081 и загрузка Avro схемы.
INFO:root:Добро пожаловать в CLIENT API!
INFO:root:Ваш идентификатор сессии: user_e4192b

Доступные команды:
  1 - Поиск товара по имени
  2 - Запросить персональные рекомендации
  exit - Выход
Введите команду:1
Введите название товара для поиска: Moscow
INFO:root:--- Поиск товара: 'Moscow' для пользователя 'user_e4192b' ---
INFO:root:KAFKA: Сообщение на отправку в топик 'client-searches' поставлено в очередь.

Доступные команды:
  1 - Поиск товара по имени
  2 - Запросить персональные рекомендации
  exit - Выход
Введите команду: 2
INFO:root:--- Запрос рекомендаций для пользователя 'user_e4192b' ---
INFO:root:KAFKA: Сообщение на отправку в топик 'client-recommendations' поставлено в очередь.

Доступные команды:
  1 - Поиск товара по имени
  2 - Запросить персональные рекомендации
  exit - Выход
Введите команду: exit
INFO:root:Все ожидающие сообщения успешно отправлены.
INFO:root:Завершение работы CLIENT API.
root@api:/opt/app# exit
exit
```



docker compose exec kafka-0 kafka-topics.sh --bootstrap-server kafka-0:9092 --list
__consumer_offsets
_schemas
client-recommendations
client-searches
mm2-offsets.secondary.internal
shop-products

docker compose --profile kafka-sec up -d
[+] Running 3/3
 ✔ Container kafka-secondary-1  Started                                                                                                                           0.7s 
 ✔ Container kafka-secondary-2  Started                                                                                                                           0.7s 
 ✔ Container kafka-secondary-0
 Started  



docker compose --profile kafka-mirror up -d
[+] Running 1/1
 ✔ Container mirrormaker2
Started         

