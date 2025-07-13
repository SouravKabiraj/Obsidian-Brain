![youtube](https://www.youtube.com/watch?v=_5mu7lZz5X4)


| #            | RebbitMQ                                  | Kafka                                                         |
| ------------ | ----------------------------------------- | ------------------------------------------------------------- |
| Storage      | In Memory Message Broker                  | Log Based Message Broker                                      |
| Scalability  | No Limitation                             | Processing Limitation = Number of Partition                   |
| Sequence     | Not Preserved                             | Preserved (per partition)                                     |
| Ideology     | Consumer Centric (one consumer per topic) | Publisher Centric (one publisher per topic)                   |
| Message Push | RebbitMQ can push message to consumer     | Kafka can't push message to consumer                          |
| Flexibility  | Flexible                                  | Scale down from N partition to N-1 partition is not possible. |
| Velocity     | Fast                                      | Slow                                                          |
| Replay       | NA                                        | Possible                                                      |

>Kafka
```mermaid 

graph TD

P[Publisher] --> K[Kafka topic:on_blog_publish]

K --> C1[Consumer_1: total_published_blog_counter++;]

K --> C2[Consumer_2: Send Notification]
```

>RebbitMQ
```mermaid 

graph TD

P[Publisher] --> K[RebbitMQ queue:blog_publish]

K --> C1[Consumer: 
total_published_blog_counter++;
SendNotification;
]
```