**172.19.91.155**

**Containers:  
  
**base-kafka-1  
base-mongo-1  
base-keycloak-1  
base-postgres-1  
base-elastic-1  
base-zookeeper-1  
  
POSTGRES_USER=postgres  
POSTGRES_PASSWORD=123456  
POSTGRES_DB=postgres  
port=5432  

KEYCLOAK_ADMIN: keycloakadmin  
KEYCLOAK_ADMIN_PASSWORD: 123456  
KEYCLOAK_LOGLEVEL: WARN  
user - 123456  
admin -123456  
port=8080  
  

MONGO_INITDB_DATABASE=mongo-storage

KAFKA_BROKER_ID=1  
KAFKA_CFG_LISTENERS=PLAINTEXT://:9092  
KAFKA_CFG_ADVERTISED_LISTENERS=PLAINTEXT://${EXTERNAL_IP:-127.0.0 .1}:  
**9092**  
KAFKA_CFG_ZOOKEEPER_CONNECT=zookeeper:2181  
KAFKA_CFG_LOG_RETENTION_HOURS=4  
KAFKA_CFG_LOG_CLEANUP_POLICY=delete  
KAFKA_CFG_MESSAGE_MAX_BYTES=20000000  
KAFKA_CFG_LOG_CLEANER_ENABLE=true  
KAFKA_CFG_DELETE_TOPIC_ENABLE=true  
ALLOW_PLAINTEXT_LISTENER=yes  
DH_KAFKA_PREFIX="lanit”  

  

ELK

port=9200

  

Возможно стоит добавить комментарий к приложениям, а именно для чего они нужны и какое у них назначение. В инструкции по **платформе**, раздел **3.1**.

Уточнение по поводу дефолтных прав в keycloak

Возможно стоит явно указывать директорию для распаковки архива с приложением.  
  

- пункт 4.3.1.4  
    не понятно было откуда брать информацию по поводу некоторых переменных, что-бы отредактировать файл  
    **install.properties** если ранее не работал с kafka, keycloak, elastic.