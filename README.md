# Kafka Java Consumer application

## Create a Kafka Consumer Java application with VSCode

We create a new folder to place the Java application.

We right click inside the folder and we select the option **Open with VSCode**.

![image](https://github.com/luiscoco/Kafka_Java_Training/assets/32194879/b6e778bf-c0cf-444c-b94c-42914f54180e)

![image](https://github.com/luiscoco/Kafka_Java_Training/assets/32194879/eddfc3ef-f7ed-42b6-aa8f-5ff29088b145)

We press the keys **Ctl+Shift+P** to create a **new Java application in VSCode**

![image](https://github.com/luiscoco/Kafka_Java_Training/assets/32194879/c5b3aed1-3a50-40c1-8cab-51f17e74fc6c)

We select the first option **No build tools**

![image](https://github.com/luiscoco/Kafka_Java_Training/assets/32194879/dbd6f666-a160-412b-9bbc-902249801a13)

Now we select the folder where to place the new Java application

![image](https://github.com/luiscoco/Kafka_Java_Training/assets/32194879/3d4d2d05-ddd3-4b93-8b8f-59e9d20f739f)

Rename the App.java to **KafkaConsumerApp.java**, and then input the following **source code**:

```java
import org.apache.kafka.clients.consumer.Consumer;
import org.apache.kafka.clients.consumer.ConsumerConfig;
import org.apache.kafka.clients.consumer.ConsumerRecords;
import org.apache.kafka.clients.consumer.KafkaConsumer;
import org.apache.kafka.common.serialization.StringDeserializer;

import java.time.Duration;
import java.util.Collections;
import java.util.Properties;

public class KafkaConsumerApp {
    public static void main(String[] args) {
        // Set up consumer properties
        Properties properties = new Properties();
        properties.put(ConsumerConfig.BOOTSTRAP_SERVERS_CONFIG, "localhost:9092");
        properties.put(ConsumerConfig.GROUP_ID_CONFIG, "your-group-id");
        properties.put(ConsumerConfig.KEY_DESERIALIZER_CLASS_CONFIG, StringDeserializer.class.getName());
        properties.put(ConsumerConfig.VALUE_DESERIALIZER_CLASS_CONFIG, StringDeserializer.class.getName());

        // Create Kafka consumer
        Consumer<String, String> consumer = new KafkaConsumer<>(properties);

        // Subscribe to the topic
        String topic = "your-topic";
        consumer.subscribe(Collections.singletonList(topic));

        // Poll for new messages
        Duration timeout = Duration.ofMillis(100);
        while (true) {
            ConsumerRecords<String, String> records = consumer.poll(timeout);
            records.forEach(record -> {
                System.out.println("Received message:");
                System.out.println("Key: " + record.key());
                System.out.println("Value: " + record.value());
                System.out.println("Partition: " + record.partition());
                System.out.println("Offset: " + record.offset());
                System.out.println("----------------------");
            });
        }
    }
}
```

![image](https://github.com/luiscoco/Kafka_Java_Training/assets/32194879/c5ae7bae-8cd6-453e-b186-d2bbba519bff)

Then we create the **log4j.properties** file in the application root:

```
log4j.rootLogger=INFO, stdout

log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.Target=System.out
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern=%d{yyyy-MM-dd HH:mm:ss} %-5p %c{1}:%L - %m%n
```

![image](https://github.com/luiscoco/Kafka_Java_Training/assets/32194879/977cd74d-9523-4b3a-9adc-86639e519abf)

Then we **donwload Kafka JAR files** from Apache Kafka web page (https://kafka.apache.org/downloads), and we place the JAR files in the Kafka producer Java application **lib** folder

![image](https://github.com/luiscoco/Kafka_Java_Training/assets/32194879/3c5dd16e-8d37-4f45-8d28-09a16cc0bf61)

![image](https://github.com/luiscoco/Kafka_Java_Training/assets/32194879/d0f37880-3f67-4dd9-a2bc-01eef311bc08)

![image](https://github.com/luiscoco/Kafka_Java_Training/assets/32194879/4d6e6392-6ec9-4c0b-afe5-e63f8582a769)

![image](https://github.com/luiscoco/Kafka_Java_Training/assets/32194879/aa7729cc-90c5-4d03-b32b-57de9898b4d2)

To **compile** the Kafka Consumer Java application

```
C:\Kafka with Java\OrderConsumer> javac -cp "lib/*;src" src/KafkaConsumerApp.java
```

To **run** the Kafka Consumer Java application

```
C:\Kafka with Java\OrderConsumer> java -cp "lib/*;src;." KafkaConsumerApp
```

## How to run the Consumer and the Producer Java applications

### 1. First we have to run the **zookeeper-server-start** command

```
zookeeper-server-start C:\kafka_2.13-3.6.0\config\zookeeper.properties
```

![image](https://github.com/luiscoco/Kafka_Java_Training/assets/32194879/adcdaea3-ba9a-4feb-87de-db5f65dc850c)

### 2. Open a command prompt window and run the command **kafka-server-start**

```
kafka-server-start C:\kafka_2.13-3.6.0\config\server.properties
```

![image](https://github.com/luiscoco/Kafka_Java_Training/assets/32194879/64a7c126-3dbf-489a-876d-993a9986197b)

### 3. Then open with VSCode the **Kafka Consumer** application and run it

To **compile** the Kafka Consumer Java application

```
C:\Kafka with Java\OrderConsumer> javac -cp "lib/*;src" src/KafkaConsumerApp.java
```

To **run** the Kafka Consumer Java application

```
C:\Kafka with Java\OrderConsumer> java -cp "lib/*;src;." KafkaConsumerApp
```

![image](https://github.com/luiscoco/Kafka_Java_Training/assets/32194879/0c03893c-0fd5-465c-905a-6fb88752fae1)

### 4. Then open with VSCode the **Kafka Producer** application and run it

To **compile** the Kafka Consumer Java application

```
C:\Kafka with Java\OrderProducer> javac -cp "lib/*;src" src/KafkaProducerApp.java
```

To **run** the Kafka Producer Java application

```
C:\Kafka with Java\OrderProducer> java -cp "lib/*;src;." KafkaProducerApp
```

![image](https://github.com/luiscoco/Kafka_Java_Training/assets/32194879/40403620-3a24-498e-8f76-a33ea5737ca0)

