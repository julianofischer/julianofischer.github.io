---
title: Setting up ONE in Eclipse IDE
author: juliano
---

Hi!
How to set up [ONE](http://www.netlab.tkk.fi/tutkimus/dtn/theone/) in popular IDEs is a common question, therefore, I intend to show a brief tutorial on how to set up ONE in two most used Java IDEs: [Eclipse](https://eclipse.org/) and [Netbeans](https://netbeans.org/).
Following I describe how to configure the simulator in Eclipse.

After downloading Eclipse, open it and click on the menu "File" >> "New" >> "Project". The following window should appear:

![Creating a new project in Eclipse](/assets/new_java_project_eclipse.png)

After selecting "Java Project" and clicking "Next", a configuration windows will be shown. You must insert you project name (to your liking) and make sure you ***select Java 1.6*** as your default JRE, as shown in the following figure. Furthermore, by default "Use default location" is checked but we need to ***uncheck it*** then we navigate to ONE project folder (~/workspace/one_1.5.1-RC2 in my case) and select it. This step will import the source code for our new project.

![Project Configuration in Eclipse](/assets/project_config_eclipse.png)

After clicking "Next" a new configuration window will be shown. Now we need to select the libraries that the simulator uses.

![Project Configuration in Eclipse](/assets/project_config_2_eclipse.png)

Select the tab "Libraries"

![Project Configuration in Eclipse](/assets/tab-libraries-eclipse.png)

Then click "Add External Jars.." and navigate to the ***lib*** dir inside the simulator root directory, then select ***DTNConsoleConnection.jar*** and ***ECLA.jar***. Note that you will need to import these files in order to compile the source code properly. Also, we need to add the JUnit4.x in order to run the tests. We can do it by clicking "Add Library..." button, then selecting JUnit, clicking "Next" and selecting "JUnit 4" at the combobox. Finally, click "Finish".

Our development environment is prepared for fun but we not yet finished :smile:
At running the project (Ctrl + F11), we will need to select the default class: just select core.DTNSim class and the GUI will show up.

With the above conditions, ONE will start using GUI and the ***default.txt*** settings. Obviously, you can rename your scenario configuration name to "default.txt" in order to test your configurations. However, this can become boring and not always you will want to run ONE in GUI mode but in [batch mode](on-batch-simulations/). However, *that is a topic for another post*.

Thank you for your attention.

