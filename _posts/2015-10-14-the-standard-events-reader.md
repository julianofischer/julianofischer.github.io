---
title: The Standard Events Reader
author: juliano
---

Hi all, how are you?

The [latest post](/converting-rollernet-to-one-standard-events-reader) was dedicated to show a way to convert the raw data provided by [Rollernet dataset](http://crawdad.org/upmc/rollernet/20090202/) into a format which can be read by [ONE simulator](https://www.netlab.tkk.fi/tutkimus/dtn/theone/). In this post, this standard format will be shown in more detail.

ONE simulator provides a class which read external events for a standard format. This class is named **StandardEventsReader** and is located into *input* package. According to the [documentation](https://www.netlab.tkk.fi/tutkimus/dtn/theone/javadoc/input/StandardEventsReader.html) the syntax for this standard format has the following general syntax:

    <time> <actionId> <msgId> <hostId> [<host2Id> [<size>] [<respSize>]]

As can be seen above, the first column describes the time at which the event occurred and its understanding is trivial. The actionId column describes what event is taking place and there are 7 possible events, listed below.

## Connection (CONN)
The connection event has a simplified syntax:

    <time> <actionId> <hostId> <host2Id> [up/down]

The connection action is followed by the two hosts and then either "up" or "down" ("UP" or "DOWN" is fine), depending on whether the nodes are connecting or disconnecting. An example of connection events is shown below.

    10 CONN 7 9 up
    27 CONN 7 9 down

In the above example a connection between the nodes 7 and 9 is created at 10 seconds of simulation time and destroyed at 27 seconds of simulation time.

## Message Create Event (C)

The message create event has the following syntax:

    <time> <actionId> <msgId> <hostId> <host2Id> <size> [<respSize>]

The column size is required and and additional respSize (size of the response) if a response to the created message is requested. An example of connection events is shown below.

    10 C 1 4 6 1000
    27 C 2 6 4 1000 500

In the above example a message is created at 10 seconds of simulation time, the message id is 1, the message source is the node 4, the message destination is the node 6, and the message has 1000 bytes in size.  Another message is created at 27 seconds of simulation time, the message id is 2, the message source is the node 6, the message destination is the node 4, the message has 1000 bytes in size, and a response with 500 bytes in size is requested.

## Message Send Event (S)

The message send event has the following syntax:

    <time> <actionId> <msgId> <hostId> <host2Id>

In the following example, a message (with ID 17) is sent from the node 18 to the node 19 at the simulation time of 500.

     500 S 17 18 19

## Message Delivered Event (DE)
The message send event has the following syntax:

    <time> <actionId> <msgId> <hostId> <host2Id>

An example where the message with ID 5 is delivered by the node 11 to the node 22 at the instant 312.

    312 DE 5 11 22
    
## Message Transfer Aborted Event (A)
The message send event has the following syntax:

    <time> <actionId> <msgId> <hostId> <host2Id>

An example where a transfer of the message with ID 7 between the nodes 11 and 22 (from 11 to 22) is aborted at the instant 100.

    100 A 7 11 22

## Message Dropped Event (DR)
The message send event has the following syntax:

    <time> <actionId> <msgId> <hostId>

A message dropped event can use the wildcard char **\*** as the message ID for referring to all messages the node has in message buffer, in such case, all messages will be dropped. This case can be seen in the following example where all the messages are dropped from the buffer of the node 7.

    100 DR * 7

## Message Removed Event (R)
The message send event has the following syntax:

    <time> <actionId> <msgId> <hostId>

As well as the message dropped event, the message removed event can use the wildcard char **\*** as the message ID for referring to all messages the node has in message buffer, in such case, all messages will be removed. In the following example, the message with ID 18 is removed from the buffer of the node 18 at the instant 18.

    18 R 18 18

# Conclusions
Once learned, standard events are very easy to understand (and use, as we shall see in the next post). The events can be generated by external tools, as our [converting script](/converting-rollernet-to-one-standard-events-reader) or the **dtnsim2parser.pl** found within the toolkit folder. Frequently I use this resource to perform [sanity tests](https://en.wikipedia.org/wiki/Sanity_check) in order to check if my implemenations are "behaving as they should" and I think many other can benefit from this tool. 

I hope I have helped you in some way. Thank you very much for your attention!
