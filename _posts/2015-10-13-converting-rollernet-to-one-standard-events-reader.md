---
title: Converting Rollernet to ONE Standard Events Reader
author: juliano
---

Hi all, how are you?
This post is intended to show you how to convert the [Rollernet](http://www-rp.lip6.fr/rollernet/en/index.html) data available at [Crawdad Project](http://crawdad.org/) in order to be used in simulations, via the standard events reader.

More information about the Rollernet dataset is available [here](../notes-on-traces-2-rollernet/) in my previous post.

In order to achieve the goal, I am using [Python](https://www.python.org/) programming language. In short, Python is a open source, multi-paradigm programming language, which emphatizes readability by opting for a simplified syntax, leading to a relative low learning curve. Besides, Python has a very good and extensive docummentation, a plenty of good books and an [awesome community](https://www.python.org/community/), ready to welcome you. **Most important tip: learn Python probably will help you in your scientific career**.

Let's go to the what really matters. By downloading the Rollernet dataset from the Crawdad website, you will get date in the following format:

    51      377     1156089135      1156089164      7       498

Recalling the latest post: the first and second columns show the IDs of the devices. The third and fourth columns describe the beginning and the end time of the contact. The fifth and last columns are for reading convenience. The fifth enumerate contacts between same nodes (e.g, 51 and 377) as 1, 2, 3 ... n. The last column gives the time difference between the current contact and the last contact between the two nodes concerned (time between contacts).

First we need to open the rollernet file.

{% highlight python %}
#python comments are preceded by '#' like this line
#first we need to create an empty list

thelist = []
with open('rollernet.txt') as f:
    thelist = [list(map(int,x.split())) for x in f.readlines()]
    #x.split() returns a list of strings
    #map convert all the values inside the list into integer

sorted(thelist, key=lambda line : line[2])#sorting by the index number two
{% endhighlight %}

This snippet uses two not common features available in Python: [List Comprehensions](https://docs.python.org/2/tutorial/datastructures.html#list-comprehensions) and the [with statement](https://docs.python.org/2/reference/compound_stmts.html#the-with-statement). I suggest that those who have questions, visit the links provided.

In the above code snippet, the lines are read (**f.readlines()**) from the file and splitted (**x.split()**), in order to separate the columns.
To illustrate and facilitate the understanding, the following expression:

{% highlight python %}
s = "51      377     1156089135      1156089164      7       498"
{% endhighlight %}

Returns the following list:

    [51, 377, 1156089135, 1156089164, 7, 498]

So at the end of the provided code snippet, the var *thelist* is a list of lists of the kind (values are fictional):
{% highlight python %}
    [[51, 277, 1156089135, 1156089164, 7, 498],
     [50, 321, 1156089170, 1156089191, 9, 500],
     [37, 151, 1156089235, 1156089264, 8, 984], ...,]
{% endhighlight %}

The next step is to read the information contained into the lists (a list per line of the data trace file) and process these information.

{% highlight python %}
other_list = []

initial_time = int(list[0][2]) #in order to convert timestamp to simulation time

for l in list:
    from_node = int(l[0])-1 #minus 1 because ids in rollernet raw data starts with 1 
                            #and in ONE it starts with 0
    to_node = int(l[1])-1
    init_time = int(l[2]) - initial_time
    end_time = int(l[3]) - initial_time

    connection_beginning = [str(init_time),
                            "CONN", 
                            str(from_node),
                            str(to_node),
                            "up"]

    connection_end = [str(end_time),
                      "CONN",
                      str(from_node),
                      str(to_node),
                      "down"]
    other_list.append(connection_beginning)
    other_list.append(connection_end)
{% endhighlight %}

The last step is to write the information into a new file.

{% highlight python %}
with open(rollernet_output,'rw') as f:
    for l in other_list:
        f.write("\t".join(l)+"\n")
{% endhighlight %}

An additional step is required if you need to remove some nodes from the trace. Suppose we need to consider only iMotes (ids 1 to 62), the *for* loop would be:

{% highlight python %}
for l in list:
    from_node = int(l[0])-1
    to_node = int(l[1])-1
    init_time = int(l[2]) - initial_time
    end_time = int(l[3]) - initial_time

    if init_time <= 61 and end_time <= 61:
        connection_beginning = [str(init_time), 
                                "CONN", 
                                str(from_node),
                                str(to_node),
                                "up"]

        connection_end = [str(end_time),
                          "CONN",
                          str(from_node),
                          str(to_node),
                          "down"]
        other_list.append(connection_beginning)
        other_list.append(connection_end)
{% endhighlight %}

Nice huh?
Finally, I hope you have a nice overview about Python power and the above code can help you anyway.
An improved version of the script can be found [here](http://github.com/julianofischer/dtnscripts). Feel free to clone/downloa then check the usage with de command (under Linux):

    ./convert_rollernet -h

I plan a new post soon, however this is a delay-tolerant blog, right? :)

Thanks for your attention!
