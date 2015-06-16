---
title: Setting up ONE in Netbeans IDE
author: juliano
---

Hi, my sincere thanks to [Sahil Gupta](https://sites.google.com/site/sahilgupta221231/file-cabinet) who contributed to this post via e-mail.
After we learned [how to set up ONE in Eclipse](/setting-up-ONE-in-eclipse), now we will learn how to set up it in Netbeans.

Click on *New Project* (ctrl + shift + n)  then select *Java Project with Existing Sources*.

![New Java Project](/assets/new_java_project_netbeans.png)

Click *Next*. Put your project name at the field *Project Name* then click *Next*.

![Project Configuration](/assets/project_config_netbeans.png)

Now you have to add the source code. Click *Add Folder* and navigate to the simulator root folder, then select it.

![Project Configuration](/assets/project_config_2_netbeans.png)

The following message may appear:

![Project Configuration](/assets/netbeans_message.png)

Click *Delete* and proceed. Right-click on *Libraries* at the left side panel, then *Add Library*. Select ***JUnit 4.10*** then click *Add Library* button.

![Add Library](/assets/add_library_netbeans.png)

Right-click *Libraries* again, now click *Add Jar/Folder*. Navigate to ONE source code root enter lib dir then select ***DTNConsoleConnection.jar*** and ***ECLA.jar***. Click *OK*.

![Add Library](/assets/add_jar_netbeans.png)

Go to the project properties, select *Libraries*, choose *JDK 1.6* as Java Platform. Now, select *Run* and choose the *Working Directory* as the ONE simulator root folder.
	
Run the project, select core.DTNSim as main class and click *OK*.
Now, you should be able to run ONE right from Netbens. :smile:

Thanks for your attention!


