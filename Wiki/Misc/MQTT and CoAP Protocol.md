# MQTT and COAP protocol

# CoAP

## Reference

1.  [](https://tools.ietf.org/html/rfc7252)[https://tools.ietf.org/html/rfc7252](https://tools.ietf.org/html/rfc7252)
2.  [](http://programmingwithreason.com/article-iot-coap.html)[http://programmingwithreason.com/article-iot-coap.html](http://programmingwithreason.com/article-iot-coap.html)
    1.  this article talks about various scenarios of connection request and response between devices
    2.  it talks in detail about resource discovery

# MQTT

## Reference

1.  [](https://www.hivemq.com/blog/mqtt-essentials-wrap-up/)[https://www.hivemq.com/blog/mqtt-essentials-wrap-up/](https://www.hivemq.com/blog/mqtt-essentials-wrap-up/)
2.  [](https://github.com/mqtt/mqtt.github.io/wiki/blog_posts)[https://github.com/mqtt/mqtt.github.io/wiki/blog\_posts](https://github.com/mqtt/mqtt.github.io/wiki/blog_posts)

## Feedback

1.  QOS section not present

## General

### Will Message

The last will message is part of the Last Will and Testament (LWT) feature of MQTT. This message notifies other clients when a client disconnects ungracefully. When a client connects, it can provide the broker with a last will in the form of an MQTT message and topic within the CONNECT message. If the client disconnects ungracefully, the broker sends the LWT message on behalf of the client. You can learn more about LWT in part 9 of this series

## SYS topic info

1. [Command reference](https://github.com/mqtt/mqtt.github.io/wiki/SYS-Topics)
2. This topic give information about the broker

## QOS

[MQTT provides 3 QOS levels- QOS 0,1,2](http://www.steves-internet-guide.com/understanding-mqtt-qos-levels-part-1/)

### QOS 0 - Only Once

1. No ACK required

### QOS 1 - At least Once

1.  At least once
2.  Requires 2 messages to complete the transaction
3.  Waits for ACK,
4.  Resents with DUP flag

### QOS 2 - Just Once

1. Requires at least 4 messages to complete the transaction
2. Idea behind this publishing is to even acknowledge that the client has forwarded the message
3. Pub, ACK