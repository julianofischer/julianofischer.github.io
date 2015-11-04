---
title: Using Connection Traces in ONE
author: juliano
---

Hello there, how are you?

Continuing a series of posts about the traces and the standard events reader in ONE, the today post is intended to show how to use connection traces in the simulator through the **StandardEventsReader**.

For connections, the StandardEventsReader format is:

    <time> <actionId> <hostId> <host2Id> [up/down]

A two-line example (one connection) of a connection trace would be as following:

    10 CONN 7 9 up
    27 CONN 7 9 down

You can get more detailed information [here](/the-standard-events-reader).

Hereinafter, the required modifications which must be applied to the scenario file will be discussed.

- #### Scenario.simulateConnections

The first configuration we need to change is to set:

    Scenario.simulateConnections = false

This means that the simulation **will not** create a connection if two nodes are in the range each other. This prevents connections being created by both StandardEventsReader and the movement model (MovementModel). Since we want connections to be created only by reading events from the event file, it is a crucial configuration.

- #### Group.movementModel

Another configuration we need to do is:

    Group.movementModel  = StationaryMovement

This means the simulation nodes should not move.

- #### Events.nrof 

This setting must be done in order to comprise the number of *events* generators you are using in your simulation. For instance, if you are already using a MessageEventGenerator, your will need two *events* generators, one for MessageEventGenerator and one for reading connections from file.

    Events.nrof = 2

- #### Events2.class

I am assuming you set up:

    Events1.class = MessageEventGenerator

So you need to set up your second events generator to read connections from file.  To this end, you must set up:

    Events2.class = ExternalEventsQueue

The **ExternalEventsQueue** class is responsible for reading the events from the event file.  According to the documentation:

> Queue of external events. This class also takes care of buffering  the events and preloading only a proper amount of them.

The default number of preload events is 500. You can set up the number of preload events to 1000 as follows:

    ExternalEventsQueue.nrofPreload = 1000

or

    Events2.nrofPreload = 1000

or just

    nrofPreload = 1000

For matter of readability, and [readability counts](https://www.python.org/dev/peps/pep-0020/), I suggest one of the first two.

*Note: you can optionally use StandardEventsReader instead of ExternalEventsQueue*

- #### Events2.filePath

The last **mandatory** step is to set up where the ExternalEventsQueue should look for your event file.

    Events2.filePath = myfilename.txt
	

#### Observations about settings
---

Hereinafter, some worth mentioning observations are made.

- #### interface.transmitRange

Altough simulateConnections is set up to false, you need to set up your interface transmission range greater than or equal to 1. A less or equal to zero value will result in an exception.

- #### MovementModel.worldSize

Sincerely I do not know if this setting is mandatory in order to run the simulation. However, when I use connection traces, I set up it as following:

    MovementModel.worldSize = 0, 0

- #### Group.nodeLocation

This is a mandatory setting. In this case, I set up it as following:

    Group.nodeLocation = 0,0

- #### ExternalEventsQueue

It seems there is a bug in ExternalEventsQueue. For more information, please refer to the mail sent to the mailing list by Mr. Alexej Ismailov, at March 16, 2015.

I hope I have helped you in some way. Thank you very much for your attention!

Related posts:

- [The Standard Events Reader](/the-standard-events-reader)
 - [Converting Rollernet to ONE Standard Events Reader](/converting-rollernet-to-one-standard-events-reader)
- [Notes on traces, part 1](/notes-on-traces-1)
- [Notes on traces, part 2 - Rollernet](/notes-on-traces-2-rollernet)