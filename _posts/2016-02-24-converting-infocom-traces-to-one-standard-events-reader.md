---
title: Converting Infocom to ONE's Format
author: juliano
---

Hello, 2016!

After demonstrating how to convert [Rollernet](http://www-rp.lip6.fr/rollernet/en/index.html) traces to a format readable by the simulator, this post shows how to convert another dataset.
The concerned dataset is called [Infocom](http://crawdad.org/~crawdad/cambridge/haggle/20090529/imote/) and is also available at [Crawdad Project](http://crawdad.org/).


The data was collected by the [Haggle](http://www.haggleproject.org/) Project, a project about opportunistic communication, during the 4 days of the Infocom conference in Barcelon, in 2006.


> Important! There are two datasets collected in the Infocom conference in 2005 and 2006. For this post the dataset collected in 2006 is used. In order to distinguish them, hereinafter this dataset is called Infocom06.

I still using Python and also still encouraging you to [**Learn Python**](http://learnpythonthehardway.org/book/).

Get to work:

By downloading the Infocom06 dataset from the Crawdad website you will see the following format:

    51  377  1156089135  1156089164  7  498

Recalling the latest post: the first and second columns show the IDs of the devices. The third and fourth columns describe the beginning and the end time of the contact. The fifth and last columns are for reading convenience. The fifth enumerate contacts between same nodes (e.g, 51 and 377) as 1, 2, 3 ... n. The last column gives the time difference between the current contact and the last contact between the two nodes concerned (time between contacts).

First we need to open the infocom file.

```python
#python comments are preceded by '#' like this line
#first we need to create an empty list

thelist = []
with open('infocom.txt') as f:
    thelist = [list(map(int,x.split())) for x in f.readlines()]
    #x.split() returns a list of strings
    #map convert all the values inside the list into integer

thelist = sorted(thelist, key=lambda line : line[2])
#sorting by the index number two
```

This snippet uses two not common features available in Python: [List Comprehensions](https://docs.python.org/2/tutorial/datastructures.html#list-comprehensions) and the [with statement](https://docs.python.org/2/reference/compound_stmts.html#the-with-statement). I suggest that those who have questions, visit the links provided.

In the above code snippet, the lines are read (**f.readlines()**) from the file and splitted (**x.split()**), in order to separate the columns.
To illustrate and facilitate the understanding, the following expression:

```python
    s = "51  377  1156089135  1156089164  7  498".split()
```

Returns the following list:

```python
    [51, 377, 1156089135, 1156089164, 7, 498]
```

So at the end of the provided code snippet, the var *thelist* is a list of lists of the kind (values are fictional):

```python
    [[51, 277, 1156089135, 1156089164, 7, 498],
     [50, 321, 1156089170, 1156089191, 9, 500],
     [37, 151, 1156089235, 1156089264, 8, 984], ...,]
```

The next step is to read the information contained into the lists (a list per line of the data trace file) and process these information.

```python
other_list = []

for l in list:

    # minus 1 because ids in infocom raw data
    # starts with 1 and in ONE it starts with 0
    from_node = int(l[0])-1 
    to_node = int(l[1])-1

    init_time = int(l[2])
    end_time = int(l[3])

    connection_beginning = [init_time,
                            "CONN", 
                            from_node,
                            to_node,
                            "up"]

    connection_end = [end_time,
                      "CONN",
                      from_node,
                      to_node,
                      "down"]
    other_list.append(connection_beginning)
    other_list.append(connection_end)

other_list = sorted(other_list, key=lambda line : line[0])
```

Now we have to guarantee that the connections are opened and closed only once. To achieve, we may define a function to do the post processing:

```python
def post_processing(the_list):
    return_list = []
    open_connections = []

    for l in the_list:
        conn_id = (l[2],l[3]) if l[2] < l[3] else (l[3], l[2])

        if l[4] == "up":
            if conn_id in open_connections:
               #ignore because it was already added
                print("Ignoring...\n")
                pass
            else:
                open_connections.append(conn_id)
                return_list.append(l)

        elif l[4] == "down":
            if conn_id in open_connections:
                open_connections.remove(conn_id)
                return_list.append(l)
            else:
               #ignore because it was already removed
               print("ignore removed\n")
               pass

    return return_list

```

In this function, the connections are opened only once, when the first node sees the other node. The connections are closed only once too, when the first node stop seeing the other node.

Now we have to call this function, passing the list as a parameter. Then the items of the list must be converted to *strings* so it can be saved into the file.

```python
other_list = post_processing(other_list)
other_list = [map(str, x) for x in other_list]
```

The last step is to write the information into a new file.

```python
with open(rollernet_output,'rw') as f:
    for l in other_list:
        f.write("\t".join(l)+"\n")
```

Now, you should be able to convert the Infocom06 dataset for a format readable by ONE simulator.

An improved version of the script can be found [here](http://github.com/julianofischer/dtnscripts). Feel free to clone/download then check the usage with the command (under Linux):

    ./convert_infocom06.py -h

You also might create issues to report bugs in Github. Your feedback will be appreciated.
	
Thanks for your attention!
