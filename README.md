### a. How much data your publisher program will send to the message broker in one run?

The amount of data your publisher program sends to the message broker in one run depends entirely on how the publisher is implemented. Typically, in an AMQP-based application, the publisher sends one or more messages—each of which might contain a string, JSON, binary payload, or other content. If, for example, your program sends 10 text messages where each message is 100 bytes, then the total data sent would be around 1,000 bytes (1 KB), not counting protocol overhead. To get an exact figure, you would need to check how many messages are being sent in the code and the size of each message.

### b. The URL of: `amqp://guest:guest@localhost:5672` is the same as in the subscriber program, what does it mean?

The URL `amqp://guest:guest@localhost:5672` in both the publisher and subscriber programs means that **both are connecting to the same AMQP broker**, which is running on the local machine (`localhost`) on the default AMQP port (`5672`). The `guest:guest` part indicates they are both using the same default credentials to authenticate. This ensures that the publisher can send messages to the broker and the subscriber can receive them from the same message queue, enabling communication between the two programs through RabbitMQ or any compatible AMQP message broker.

### Running RabbitMQ

![alt text](image.png)