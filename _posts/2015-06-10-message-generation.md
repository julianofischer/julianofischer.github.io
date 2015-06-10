---
title: Message Generation
author: juliano
---

Hi!

Message generation is a recurring question in the [mailing list](https://www.netlab.tkk.fi/mailman/listinfo/theone).
Recently, Mr. Barun Saha wrote a good [post](http://delay-tolerant-networks.blogspot.in/2015/06/specifying-source-and-destination-of-messages.html) about the MessageEventGenerator class, focused on solve doubts about how to specify the source and destination of messages.

Sometimes, we need a better control over the number of messages generated and other parameters. It may help us to understand the results achieved. The good news is that there is a script which may be used to generate message create events, located at "/toolkit/createCreates.pl". Following I will introduce the script usage and some benefits.

Let's look at the script's usage.

{% highlight bash linenos %}
 Usage:
 -time <startTime>:<endTime> -nrof <nrofMessages>
 -hosts <minHostAddr>:<maxHostAddr> -sizes <minMsgSize>:<maxMsgSize>
 [-rsizes <respMinSize>:<respMaxSize>] [-seed <randomSeed>]

 Minimum values are inclusive, max values are exclusive.
{% endhighlight %}

An example of using this script is:

{% highlight bash linenos %}
./createCreates.pl -time 1:10000 -nrof 100 -hosts 0:10 \
    -sizes 500000:500000 -seed 1
{% endhighlight %}

This command will create 100 message events (-nrof 100) where the message creation events will be generated between 1 and 9999 simulation seconds (max values are exclusive). All messages will have 500,000 bytes of size (-sizes 500000:500000). Following I transcribe a short snippet of the generated output:

{% highlight bash linenos %}
74.5	C	M1	4	7	500000
153.1	C	M2	2	5	500000
210.8	C	M3	6	1	500000
245.5	C	M4	3	6	500000
307.0	C	M5	4	6	500000
464.1	C	M6	7	4	500000
521.5	C	M7	8	7	500000
684.6	C	M8	7	9	500000
796.6	C	M9	2	0	500000
910.6	C	M10	2	1	500000
{% endhighlight %}

It is worthwhile to point out the usage of the ***seed*** parameter. This parameter is used in order to generate distinct message events. Suppose you need create 5 message event files. A way to achieve it is as following:

{% highlight bash linenos %}
./createCreates.pl -time 1:10000 -nrof 100 -hosts 0:10 \
    -sizes 500000:500000 -seed 1
./createCreates.pl -time 1:10000 -nrof 100 -hosts 0:10 \
    -sizes 500000:500000 -seed 2
./createCreates.pl -time 1:10000 -nrof 100 -hosts 0:10 \
    -sizes 500000:500000 -seed 3
./createCreates.pl -time 1:10000 -nrof 100 -hosts 0:10 \
    -sizes 500000:500000 -seed 4
./createCreates.pl -time 1:10000 -nrof 100 -hosts 0:10 \
    -sizes 500000:500000 -seed 5
{% endhighlight %}

Note that everytime you use the same seed, identical events will be generated.
You can use [redirection operators](https://www.gnu.org/software/bash/manual/html_node/Redirections.html) in order to create a file given the command output.

Well, what should I do with these files?
Now you can use the ExternalEventsQueue class to read the input file.

Your configuration file should include the following lines
{% highlight bash linenos %}
Events.nrof = 1
Events1.class = ExternalEventsQueue 
Events1.filePath = GeneratedFile.txt
{% endhighlight %}

	
Bonus: the simulation time may be reduced. Try it yourself =)
	
Juliano.
