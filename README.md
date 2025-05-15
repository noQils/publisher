### a. How much data your publisher program will send to the message broker in one run?

The amount of data your publisher program sends to the message broker in one run depends entirely on how the publisher is implemented. Typically, in an AMQP-based application, the publisher sends one or more messages—each of which might contain a string, JSON, binary payload, or other content. If, for example, your program sends 10 text messages where each message is 100 bytes, then the total data sent would be around 1,000 bytes (1 KB), not counting protocol overhead. To get an exact figure, you would need to check how many messages are being sent in the code and the size of each message.

### b. The URL of: `amqp://guest:guest@localhost:5672` is the same as in the subscriber program, what does it mean?

The URL `amqp://guest:guest@localhost:5672` in both the publisher and subscriber programs means that **both are connecting to the same AMQP broker**, which is running on the local machine (`localhost`) on the default AMQP port (`5672`). The `guest:guest` part indicates they are both using the same default credentials to authenticate. This ensures that the publisher can send messages to the broker and the subscriber can receive them from the same message queue, enabling communication between the two programs through RabbitMQ or any compatible AMQP message broker.

### Running RabbitMQ

![alt text](image.png)

### Cargo Run 

#### Subscriber

![alt text](image-1.png)

#### Publisher

![alt text](image-2.png)

#### What's happening?

When the publisher is executed using `cargo run`, it initializes and dispatches five `UserCreatedEventMessage` events to a RabbitMQ queue under the routing key `user_created`. Each event contains distinct `user_id` and `user_name` fields. After publishing the messages, the publisher terminates its process.

On the other hand, when the subscriber is launched with `cargo run`, it begins listening to the same `user_created` queue. As messages arrive, the subscriber processes them sequentially, printing each message's details `user_id` and `user_name` to the console. The subscriber operates in an infinite loop, continuously awaiting new messages.

The subscriber's output displays repeated instances of the same messages, indicating potential reprocessing. This behavior may stem from the queue's configuration, such as the absence of message acknowledgment or settings like `auto_delete: false` and `durable: false`, which prevent messages from being automatically removed after delivery.

#### After running publisher for a few more times

![alt text](image-3.png)
When the publisher is executed, it sends multiple `UserCreatedEventMessage` events to the RabbitMQ queue, which is reflected in the RabbitMQ management interface as noticeable spikes in the message rates chart. These spikes indicate a sudden increase in message throughput, confirming that the publisher successfully dispatches events to the queue. Additionally, metrics such as memory usage and Erlang processes under the **Global counts** section may temporarily rise due to the increased workload. Repeated executions of the publisher amplify these spikes, demonstrating RabbitMQ's real-time processing capabilities and its responsiveness to event-driven workloads. The visualization of these spikes in the **Chart statistics** section serves as direct evidence of the publisher's impact on the messaging system.  

#### After adding `thread::sleep(ten_millis);` in subscriber main.rs

![alt text](image-4.png)
After modifying the subscriber to include a 1 second delay per message processing and running multiple publisher instances quickly, RabbitMQ's queue accumulates messages faster than the subscriber can process them. The "Total" count in the queue (20 in this case) represents the backlog of unprocessed messages waiting in the queue. This occurs because the publisher can rapidly send messages while the slow subscriber processes them sequentially with delays. The queue acts as a buffer, storing messages until the subscriber is ready to handle them, demonstrating RabbitMQ's ability to manage differing producer and consumer speeds. The exact number in your queue may vary depending on how many times you executed the publisher before checking the dashboard.