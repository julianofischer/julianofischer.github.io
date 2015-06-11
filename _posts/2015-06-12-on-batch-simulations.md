---
title: On Batch Simulations in the ONE
author: juliano
---

Hi!
This post shows some tips about batch simulation in [ONE](http://www.netlab.tkk.fi/tutkimus/dtn/theone/).

When we do not intend to use the GUI, we can run simulations in batch mode using ***-b*** option.

{% highlight bash %}
./one.sh -b my_setting_file.txt
{% endhighlight %}

So far, so good.
However, the main advantage of using batch simulations is to run several simulations with a single command.

Let's take a look at the example taken from the simulator site.
The following command, will run 22 simulations with 11 different message copy counts and with binary and normal mode.

{% highlight bash %}
./one.sh -b 22 snw_comparison_settings.txt
{% endhighlight %}

Below are the settings so we can understand what is happening.

    Scenario.name = SNWComp-%%SprayAndWaitRouter.nrofCopies%%-B%%SprayAndWaitRouter.binaryMode%%
    Group.router = SprayAndWaitRouter
    SprayAndWaitRouter.nrofCopies = [1;2;3;4;5;6;7;8;9;10;11]
    SprayAndWaitRouter.binaryMode = [true;false]
    MovementModel.rngSeed = 0
    Report.nrofReports = 1
    Report.report1 = MessageStatsReport
    Report.reportDir = snwcomp_reports/

There are two things we must note. The scenario name and the variables of the scenario (***Scenario.nrofCopies*** and ***Scenario.binaryMode***).

###On the variables
When running batch simulations, ONE uses the variables in the following way. At the simulation 1, ONE will take the values for nrofCopies and binaryMode from index 1, that is, 1 and true, respectively. For the second simulation, ONE will take the values at the index 2, that is, 2 and false. At the simulation 3, ONE will take the values at the index 3, that is, 3 and ... 

Oops! *** What happens now?***

The answer is quite simple. The value for binaryMode is taken from the beginning, that is, at the simulation 3, ONE will take the values 3 and true. Simple huh?
Mathematically, we could say that the value is taken from the rest of the division between the simulation number and the variable list size (the size of [true;false] is two).

The same effect could be achieved as follows, which I consider to be easier to understand, although it is ugly:

    SprayAndWaitRouter.nrofCopies = [1;2;3;4;5;6;7;8;9;10;11]
    SprayAndWaitRouter.binaryMode = [true;true;true;true;true;true;true;true;true;true;true;
        false;false;false;false;false;false;false;false;false;false;false]

### On the scenario name

In order to ensure your report files will not be overridden, you need to include some variable information in the scenario name. You can do it by including the scenario variables in the scenario name, this way you will be able to identify which is the scenario settings for a particular report file. Let's take a look at our example.

    SNWComp-%%SprayAndWaitRouter.nrofCopies%%-B%%SprayAndWaitRouter.binaryMode%%

There are two variables in the scenario name, ***SprayAndWaitRouter.nrofCopies*** and ***SprayAndWaitRouter.binaryMode***. We insert variables in the scenario name by simple adding the variable surrounded by two percentage sign (%%our_variable%%).

***Tip:*** in addition to avoid the override of the report files, insert variables names in the scenario name will help us to identify possible scenario configuration mistakes. I often add the name of the routing class I'm using (%%Group.router%%)

That is all for now.
Thank you very much for your attention!


