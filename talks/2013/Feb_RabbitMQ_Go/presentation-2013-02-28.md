# Go & RabbitMQ

## It's bunnies, all the way down...

_(This presentation made with [Landslide](https://github.com/adamzap/landslide).)_

![Golang](images/golang-logo.png)

---

# What we do

![Cat](images/cat.png)

---

# What we own

- Integration of internal and external systems

- Financial and supply-chain data

- Order fulfillment process

- Inventory

---

# What we care about

- Scalability
    - Latency tolerance
    - Throughput
    - Reducing uptime cost (through small, efficient, applications

- Decoupling systems

- Developer happiness

- Data integrity

---

# Why messaging queues

- Scalability
    - easy to scale consumers horizontally

- Decoupling systems
    - standardize communication between systems
    - application isolation

- Data integrity
    - message guarantees

---

# Why RabbitMQ

- Message durability and routing

- Copious language bindings

- Clustering and high availibity support

- Flexible messaging patterns

---

# Why Go

- Scalability
    - fast
    - small footprint

- Data integrity
    - static typing

- Developer happiness
    - small language
    - batteries included
    - tooling
    - c integration
    - community

---

# Producer

Writes 10 messages to the example exchange

    !go
    connection, _ := amqp.Dial(amqpURI)

    channel, _ := connection.Channel()

    for i := 0; i < 10; i++ {
        channel.Publish(
            "example", //exchange
            "hello.world", //routingKey
            false,
            false,
            amqp.Publishing{
                Headers: amqp.Table{},
                ContentType: "text/plain",
                ContentEncoding: "UTF-8",
                Body: []byte("Hi"),
                DeliveryMode: amqp.Transient,
                Priority: 0,
            },
        )
    }

---

# Consumer

Reads from the example\_queue forever

    !go
    c.conn, _ = amqp.Dial(amqpURI)

    c.channel, _ = c.conn.Channel()

    deliveries, _ := c.channel.Consume(
        "example_queue", //queue
        c.tag,
        false,
        false,
        false,
        false,
        nil)

    for d := range deliveries {
        log.Printf("got %s", d.Body)
    }

---

# Why our architecture

- Scalability
    - small, single-purpose applications

- Decoupling systems
    - application isolation
    - testing

- Data integrity
    - message idempotence

---

# Architecture

![Architecture](images/architecture.png)

---

# References

- Go AMQP library
    - [https://github.com/streadway/amqp](https://github.com/streadway/amqp)

- RabbitMQ
    - [http://www.rabbitmq.com](http://www.rabbitmq.com)

----

# Bunnies


    !text
           ,_ ,_        (\/)    _, _,
           | '. '.       \/   .' .' |
           \   \  \          /  /   /
            '.__\_|_        _|_/__.'
                /`  '.    .'   `\
               /    ^ )  ( ^     \
              /   __.'    '.__    \
            .'   (_          _)    '.
          .'      \'-._  _.-'/       '.
         /         '.__)(__.'          \
        ;        .-. '.  .' .-.         ;
      /`|       /   '._)(_.'   \        |`\
     |   \     /--.          .--\      /   |
      '--'\   '-.__)        (__.-'    /'--'
       jgs )_____)            (______(
