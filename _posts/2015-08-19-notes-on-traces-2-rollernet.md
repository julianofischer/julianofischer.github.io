---
title: Notes on traces, part 2 - Rollernet
author: juliano
---

Hi all, how are you?

Let's starts the series of posts about data traces reviewing the [Rollernet](http://www-rp.lip6.fr/rollernet/en/index.html).

Rollernet is a project whose the origin was in the [LIP6 lab](http://www.lip6.fr/) at the Université Pierre et Marie Curie. The project proposes to study the mobility of rollerblading tours participants. During the experiment 62 iMotes (an iMote is a device with storage and processing capabilities, equipped with a Bluetooth radio) were distributed to group of people in order to collect opportunistic sighting of other Bluetooth devices (including the other iMotes).

The data available for download at the [Crawdad Project](http://crawdad.org/) contains, besides the contacts between the 62 iMotes (ids ranging from 1 to 62), contacts with other 1050 devices (with ids ranging from 62 to 1112). The data format is as follows:

    51      377     1156089135      1156089164      7       498

The first and second columns show the IDs of the devices. The third and fourth columns describe the beginning and the end time of the contact. The fifth and last columns are for reading convenience. The fifth enumerate contacts between same nodes (e.g, 51 and 377) as 1, 2, 3 ... n. The last column gives the time difference between the current contact and the last contact between the two nodes concerned (time between contacts).

An interesting fact about Rollernet is that it presents a phenomenon named by the authors as ["Accordion Phenomenon"](http://citeseer.ist.psu.edu/viewdoc/summary?doi=10.1.1.142.7799): the network topology expands and shrinks with time. That is, the connectivity increases when the nodes are stopped, usually clustered waiting for the start of an rollerblading round, and decreases when the round is started, because the participants tend to spread over the runway. 

Well, obviously we need to process the data trace generating a format which can be read by [ONE](http://www.netlab.tkk.fi/tutkimus/dtn/theone/). Furthermore, we may want to analyze the network comprising only the iMotes (62 nodes), there are advantages to this: the simulation is fast (30 to 60 seconds, in my case).

We will analyze how to convert and filter the data in a next post!


Thanks for your attention!
