---
title: Converting Rollernet to ONE's Standard Events Reader Format
author: juliano
---

Hi all, how are you?
This post is intended to show you how to convert the [Rollernet](http://www-rp.lip6.fr/rollernet/en/index.html) data available at [Crawdad Project](http://crawdad.org/) in order to be used in simulations, via the standard events reader.

More information about the Rollernet dataset is available [here](../notes-on-traces-2-rollernet/) in my previous post.

In order to achieve the goal, I am using [Python](https://www.python.org/) programming language. In short, Python is a open source, multi-paradigm programming language, which emphatizes readability by opting for a simplified syntax, leading to a relative low learning curve. Besides, Python has a very good and extensive docummentation, a plenty of good books and an [awesome community](https://www.python.org/community/), ready to welcome you. **Most important tip: learn Python probably will help you in your scientific career**.

Let's go to the what really matters. By downloading the Rollernet dataset from the Crawdad website, you will come across the following format:

    1    3    51293    51293    1    0

The first and second columns show the IDs of the devices. The third and fourth columns describe the beginning and the end time of the contact. The fifth and last columns are for reading convenience.

So we get:

    ID1    ID2    init_time    end_time

Some may have noticed that the format is identical to the Rollernet format. ***However***, there are particularities. The time in Rollernet begins with a large number, time in Infocom06 begins with 1. Furthermore, while the contact in Rollernet is bidirectional (the data was processed and for each contact the initial time is the first time ID1 saw the ID2 OR the first time ID2 saw the ID1. Similarly, the end time is the last time the ID1 saw ID2 OR the last time ID2 saw ID1. The same methodology was not used in the Infocom06, where the third and fourth column describe, respectively, the first and last time when the address of ID2 were recorded by ID1 for this contact. To illustrate, in Infocom06 we may have the following situation:
    1    3    51293    51294    1    0
    3    1    51291    51295    1    0

That is, due to the radio characteristics, node 3 saw node 1 before node 1 saw it. Moreover, node 3 continues to see node 1 stopped to see node 3 (OMG, I hope I am not being too messy). The fact is that due this particularity, a extra step is required, regarding to the steps applied to Rollernet.


First of all: open the file.

```python
#python comments are preceded by '#' like this line
#first we need to create an empty list

thelist = []
with open('rollernet.txt') as f:
    thelist = [list(map(int,x.split())) for x in f.readlines()]
    #x.split() returns a list of strings
    #map convert all the values inside the list into integer

thelist = sorted(thelist, key=lambda line : line[2])#sorting by the index number two
```

This snippet uses two not common features available in Python: [List Comprehensions](https://docs.python.org/2/tutorial/datastructures.html#list-comprehensions) and the [with statement](https://docs.python.org/2/reference/compound_stmts.html#the-with-statement). I suggest that those who have questions, visit the links provided.

In the above code snippet, the lines are read (**f.readlines()**) from the file and splitted (**x.split()**), in order to separate the columns.
To illustrate and facilitate the understanding, the following expression:

```
s = "51      377     1156089135      1156089164      7       498"
```

Returns the following list:

    [51, 377, 1156089135, 1156089164, 7, 498]

So at the end of the provided code snippet, the var *thelist* is a list of lists of the kind (values are fictional):

```
    [[51, 277, 1156089135, 1156089164, 7, 498],
     [50, 321, 1156089170, 1156089191, 9, 500],
     [37, 151, 1156089235, 1156089264, 8, 984], ...,]
```

The next step is to read the information contained into the lists (a list per line of the data trace file) and process these information.

```python
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
```

The last step is to write the information into a new file.

```python
with open(rollernet_output,'rw') as f:
    for l in other_list:
        f.write("\t".join(l)+"\n")
```

An additional step is required if you need to remove some nodes from the trace. Suppose we need to consider only iMotes (ids 1 to 62), the *for* loop would be:

```python
for l in list:
    from_node = int(l[0])-1
    to_node = int(l[1])-1
    init_time = int(l[2]) - initial_time
    end_time = int(l[3]) - initial_time

    if from_node <= 61 and to_node <= 61:
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
```

Nice huh?
Finally, I hope you have a nice overview about Python power and the above code can help you anyway.
An improved version of the script can be found [here](http://github.com/julianofischer/dtnscripts). Feel free to clone/download then check the usage with the command (under Linux):

    ./convert_rollernet.py -h

I plan a new post soon, however this is a delay-tolerant blog, right? :)

Thanks for your attention!
