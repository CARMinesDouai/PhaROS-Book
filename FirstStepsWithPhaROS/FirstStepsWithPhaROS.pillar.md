

##First Steps with PhaROS
<a name="first"></a>




###1\. Install PhaROS tool
This tool ease the installation and the use of ROS middleware\. First of all,  we need to install PhaROS\-Tool, then we can use it to ease the configuration of our own nodes\.



####1\.1\. Download PhaROS\-Tool

You can download PhaROS\-Tool with:



```bash
$ wget http://car.mines-douai.fr/wp-content/uploads/2014/01/pharos.deb
```



Once downloaded just execute the following lines to install it and update it\.



```bash
$ sudo dpkg -i pharos.deb
$ pharos update-tool
```



Executing the following line  validate the correct installation and will give you an help\.




```bash
$ pharos --help
```



The result is:



```bash
pharos usage
  pharos install			- Installs a package
  pharos create 			- Creates a new package based on an archetype
  pharos create-repository	- Creates a smalltalk script for creating your own repository
  pharos register-repository	- Register a new package repositry
  pharos list-repositories	- List registered repostiries
  pharos update-tool	      	- check if there is any update of the tool, and it update it.
  pharos ros-install		- installs a ros
```



Each of these commands will be explained in this document\.



####1\.2\. Installing ROS with PhaROS\-Tool: pharos ros\-install

The following example line will install the desktop\-full of the hydro version of ROS:




```bash
$ pharos ros-install --version=hydro --type-installation=desktop-full
```



You can install the version of ROS you want\. This process works only on Ubuntu\.
The possible options are available executing this line:



```bash
$ pharos ros-install --help
```



They are:



- 
           ~~<del>\-version : you define the version of ROS \(fuerte \| groovy \| hydro\)\. By default it is groovy\.
    </del>~~\-type\-installation : you can define the type of ros installation \(desktop \| desktop\-full \| ros\-base\)\. By default, it is desktop\.
    ~~<del>\-configure : It indicates if the installation should be configured\. It modifies the \.bashrc file\. The value can be true or false\. By default, it is true\.
    </del>~~\-create\-workspace : it indicates if a workspace should be created\. It will create a workspace at ~/ros/worskpace\. The value can be true or false\. By default, it is true\.




####1\.3\. Knowing which repositories are registered: pharos list\-repositories

The repositories listed are the places where your projects are saved\. By default there are two repositories: the first one is the main repository of PhaROS, the second is the material for peripherics we already plugged on PhaROS\. For example, there is a package for the turtlebot in this repository\.

You can list repositories with the folowing command:



```bash
$ pharos list-repositories
```



By default, it will give you the two following repositories, and attached information\.



```bash
-------
url = http://car.mines-douai.fr/squeaksource/PhaROS
user =
directory = PhaROSDeploymentDirectory
password =
repository = PhaROSDeploymentDirectory
-------
url = http://car.mines-douai.fr/squeaksource/Peripherics
user =
directory = PeriphericsDirectory
password =
repository = PeriphericsDirectory
-------
```





####1\.4\. Adding a new repository: pharos register\-repository

This command allows one to add one new repository\. To use the ev3 ROS package, you have to register the following repository:



```bash
$ pharos register-repository --url=http://smalltalkhub.com/mc/JLaval/Ev3ROS/main --package=EV3ROSDirectory
```



The package EV3ROSDirectory contains all the metadata needed to build a ROS package\. It is explained after in the chapter\.

Tip: If your repository requires user/password for reading add ~~<del>user=User </del>~~password=Password to the example\.
Pay attention that the User/Password will be stored in a text file without any security\.

After adding the new repository, you can verify it is included it is available in the list of repository:



```bash
$ pharos list-repositories
```



Yes, it is \!




```bash
-------
url = http://car.mines-douai.fr/squeaksource/PhaROS
user =
directory = PhaROSDeploymentDirectory
password =
repository = PhaROSDeploymentDirectory
-------
url = http://car.mines-douai.fr/squeaksource/Peripherics
user =
directory = PeriphericsDirectory
password =
repository = PeriphericsDirectory
-------
url = http://smalltalkhub.com/mc/JLaval/Ev3ROS/main
user =
directory = EV3ROSDirectory
password =
repository = EV3ROSDirectory
-------
```



Adding a repository would need some options\. Here are the available options to register a repository:


&nbsp;

- 
    - 
        - url	: this option is mandatory, it is the url of a monticello repository
        - package	: this option is mandatory, it is the name of the Directory package in the monticello repository\.
        - directory	: This is the name of the class that works as directory\. This class must implement  emph\{\#includesPackage:\} and emph\{\#deployUnitForPackage:\}\. By default, the value is the same as the package name\.
        - user : it is the username to access the monticello repository\. The default value is empty\.
        - password : it is the password for the monticello repository\. The default value is empty\.






####1\.5\. Installing a PhaROS package: pharos install

After having added the ev3 repository, as explained in the previous section, you can install the package Ev3 for ROS\. This package is named ev3\. The command should be run in the ros workspace\. By default, the ROS workspace is in emph\{\{textasciitilde\}/ros/workspace/\}\.



```bash
$ cd ros/workspace/
$ pharos install ev3 --version=3.0 --ros-distro=hydro --pharo-user=JLaval --force-new
```



The previous line search the ev3 package in the available repositories\. When it found it, it install it in a new pharo image\. Here, we are using the version 3\.0 of Pharo\. It is configured for ROS Hydro, the version of ROS that I previously installed\. I also configure the user name in the Pharo image\. All the source code that I produce with this image will have as author the name JLaval\. Finally, I force a new installation\. It means that if the package already exists, it will be replaced and all the changes will be lost\.

The usage of this command can be completed with the following options:


          ~~<del>\-location : You can give the absolute path to a valid catkin workspace \(not the source folder\)\. The default value is the current directory\.
          </del>~~\-silent : The values can be true or false\. If silent is false you will be able to see the installation of the output image\. The default value is true\.
          ~~<del>\-version : It indicates the version of Pharo to download\. The default value is 2\.0\. It can be 1\.4, 2\.0 or 3\.0\.
          </del>~~\-ros\-distro : It indicates the distribution of ROS that you want to use\. By default it is groovy, but you can configure it with groovy, hydro or fuerte\.
          ~~<del>\-package : It indicates the name of the package to install \(If you use <<pharos install aPackage </del>~~package=otherPackage>> the one to be installed is otherPackage\)\.
          ~~<del>\-package\-version : It indicates the package version\. By default it takes the latest one\.
          </del>~~\-pharo\-user : It indicates the user name for the result image\.
          ~~<del>\-force\-new : It DELETES the package if it exists in the pointed location\.


\!\!Run PhaROS software

To edit the source code, just run the following line\. It will open the Pharo image used for your package\.


```bash
$ rosrun ev3 edit
```



For example, you can browse the method EV3ROSPackage>>scriptPublishColour in PhaROS\. It is the script to run a ROS node that publishes the color sensor data\.

Each scripts in PhaROS has the signature format like emph\{scriptWord1Word2\}\. It begins with emph\{script\} followed by the words of the method\.

Each script when compiled generates a \.sh script\. Like that, when you need to execute your script without touching the source code, you don't need to open PhaROS\. You can access to the scripts generated by executing the following lines:


```bash
$ roscd ev3
$ ls image/script
```



The first line change the directory to the package ev3\. The second line lists the available scripts\. When writting these lines, there are 3 available scripts:


```bash
move_meanwhile_you_can.st
publish_colour.st
publish_sonar.st
```



The first script make the robot move meanwhile it detects an obstacle\. The second provide a publisher for the color sensor\. The third script publish the sonar data\.

As expected we find the script that publish the color sensor data\. The name given to the script is related to the format of the name given inside PhaROS\. A method signature like emph\{scriptWord1Word2\} become emph\{word1\_word2\.st\}\.


Now, you can run the scripts without seeing PhaROS environment as following:


```bash
$ rosrun ev3 headless publish_colour
```



or run the script with the environment:


```bash
$ rosrun ev3 pharos publish_colour
```




\!\!Create your ROS package

There are three steps to create your packages: first, you will create your PhaROS package, that contains your source code\. Then, you will create a configuration that gives PhaROS the possibility to load your source code\. Finally, you will create a method that contains the ROS Package information\.

\!\!\!Create your package : pharos create

To create a Pharo image for developing your own software, you would use the command [¿?](#pharos create)\. In the following line, I am creating a phaROS configuration for the package  [¿?](#my_super_ros_package)\.

\[\[\[language=bash
$ cd ~/ros/workspace/
$ pharos create my\_super\_ros\_package </del>~~version=3\.0 ~~<del>ros\-distro=hydro </del>~~author=JLaval ~~<del>author\-email=jannik\.laval@gmail\.com
\]\]\]

Pay attention that your package name follows the naming conventions\. It should start with a lower case letter and only contain lower case letters, digits and underscores\.


The usage of this command can be completed with the following options:


          </del>~~\-help : It shows this text
         ~~<del>\-location : You can give the absolute path to a valid catkin workspace \(not the source folder\)\. The default value is the current directory\.
          </del>~~\-silent : The values can be true or false\. If silent is false you will be able to see the installation of the output image\. The default value is true\.
         ~~<del>\-version : It indicates the version of Pharo to download\. The default value is 2\.0\. It can be 1\.4, 2\.0 or 3\.0\.
          </del>~~\-ros\-distro : It indicates the distribution of ROS that you want to use\. By default it is groovy, but you can configure it with groovy, hydro or fuerte\.


          ~~<del>\-archetype : It indicates the name of the archetype to use for creating things\. By default, it is basic\-archetype\.
          </del>~~\-author :	It indicates the name of the author\.
          ~~<del>\-maintainer : It indicates the name of the maintainer\. The default value is the one indicated in author\.
          </del>~~\-author\-email : It indicates the email  of the author\.
          ~~<del>\-maintainer\-email : 	It indicates the email of the maintainer\. The default value is the one indicated in author\-email\.
          </del>~~\-description : It indicates the description of the package\.
          ~~<del>\-pharo\-user : It indicates the user name for the result image\.
          </del>~~\-force\-new : It DELETES the package if it exists in the pointed location\.



Inside PhaROS environment, one package, emph\{My\_super\_ros\_packagePackage\} was created with 2 classes inside: emph\{My\_super\_ros\_packagePackage\} and emph\{My\_super\_ros\_packageNodelets\}\. Inside the class emph\{My\_super\_ros\_packagePackage\}, you will find some example methods\.



####1\.6\. Create the configuration

Create the ConfigurationOf inside PhaROS, in a package ConfigurationOfMySuperRosPackage\. To create a configuration in PhaROS, yo can find more documentation in the free book \`\`Deep into Pharo'' available on url\{http://www\.deepintopharo\.com/\}\.
For example, for the ev3 package, I created this class:

\[\[\[language=Smalltalk
Object subclass: \#ConfigurationOfEv3ROS
  instanceVariableNames: 'project'
  classVariableNames: 'LastVersionLoad'
  category: 'ConfigurationOfEv3ROS'
\]\]\]

This class contains the method emph\{baseline10:\}, which load my source code\.

\[\[\[language=Smalltalk
ConfigurationOfEv3ROS>>baseline10: spec
  <version:'1\.0\-baseline'>
  spec for: \#common do: \[
      spec blessing: \#baseline\.
      spec repository: 'http://smalltalkhub\.com/mc/JLaval/Ev3ROS/main'\.

      spec project: 'JetStorm' with: \[
        spec
          className: 'ConfigurationOfJetStorm';
          file: 'ConfigurationOfJetStorm' ;
          version: \#bleedingEdge;
          repository: 'http://smalltalkhub\.com/mc/JLaval/JetStorm/main'\.
      \]\.
      spec project: 'PhaROS' with: \[
        spec
          className: 'ConfigurationOfPhaROS';
          file: 'ConfigurationOfPhaROS' ;
          version: \#bleedingEdge;
          repository: 'http://car\.mines\-douai\.fr/squeaksource/PhaROS'\.
      \]\.

      spec package: 'EV3ROSPackage' with: \[
        spec requires: \#\('JetStorm' 'PhaROS'\)
      \]\.

  \]\.
\]\]\]

\!\!\!Create the ROS metadata: pharos create\-repository

Creating a directory for your own project repository needs a st file\. Then, you have to load it in a pharo image and personalize it\.

For example, I used the following command line to create my file for Ev3\.

\[\[\[language=bash
$ pharos create\-repository EV3ROS \-\-user=JLaval > eve\.st
\]\]\]

Then, I just file in the file\.st in PhaROS\. It will create a package EV3ROSDirectory that contains the meta data\.

You can do it for My\_super\_ros\_package:

\[\[\[language=bash
$ pharos create\-repository My\_super\_ros\_package \-\-user=JLaval > myROS\.st
\]\]\]

It will create a package My\_super\_ros\_packageDirectory that contains the meta data\.
This command can have two options:


  \-\-\-user : gives the name of the user for generation by default is 'Generated'
  \-\-\-output path to where the script will be dumped\. By default is the standar output\.


When the file is integrated in PhaROS, you can see the methods emph\{EV3ROSDirectory class>>archetypeEv3\} and emph\{EV3ROSDirectory class>>ev3RosPackage\}, respectively emph\{My\_super\_ros\_packageDirectory class>>archetypeExample\} and emph\{My\_super\_ros\_packageDirectory class>>exampleMetacelloPackage\} for My\_super\_ros\_package\. There are other files that are not necessary here\. You can browse and discover them\.

\[\[\[language=Smalltalk
EV3ROSDirectory class>>archetypeEv3
  ^ \{
      \#archetype \-> 'EV3ROS\-archetype' \.
      \#description \-> 'Minimum dependancies for using EV3\+ROS' \.
      \#license \->  'MIT'  \.
      \#version \-> '0\.1\.0' \.
      \#maintainer \-> \(\{
        \#name\-> 'JLaval' \.
        \#email \-> 'jannik\.laval@gmail\.com'
      \}  asDictionary\) \.
      \#author \-> \(\{
        \#name\-> 'JLaval' \.
        \#email \-> 'jannik\.laval@gmail\.com'
      \} asDictionary\) \.
      \#metacello \-> \(\{
        \#url \-> 'http://smalltalkhub\.com/mc/JLaval/ev3ros/main' \.
        \#configurationOf \-> 'ConfigurationOfEv3ROS' \.
        \#package \-> 'EV3ROSArchetype'
      \} asDictionary\)
     \} asDictionary
\]\]\]


\[\[\[language=Smalltalk
EV3ROSDirectory class>>ev3RosPackage
  ^ \{
      \#name \-> 'ev3' \.
      \#description \-> 'ROS package for managing Ev3' \.
      \#license \->  'MIT'  \.
      \#version \-> '0\.1\.0' \.
      \#maintainer \-> \(\{
        \#name\-> 'JLaval' \.
        \#email \-> 'jannik\.laval@gmail\.com'
      \}  asDictionary\) \.
      \#author \-> \(\{
        \#name\-> 'JLaval' \.
        \#email \-> 'jannik\.laval@gmail\.com'
      \} asDictionary\) \.
      \#metacello \-> \(\{
        \#url \-> 'http://smalltalkhub\.com/mc/JLaval/Ev3ROS/main' \.
        \#configurationOf \-> 'ConfigurationOfEv3ROS' \.
        \#package \-> 'EV3ROSPackage'
      \} asDictionary\)
     \} asDictionary
\]\]\]

As you can see, each method represents the metadata used for the publication in ROS\. You can use and complete your data for your own PhaROS package\.

Finally, do not forget to change the method emph\{EV3ROSDirectory class>>deployUnitsMetadata\} to call the correct configuration method\.

\[\[\[language=Smalltalk
EV3ROSDirectory class>>deployUnitsMetadata
  ^ \{
    self archetypeEv3 \.
    self ev3RosPackage\.
   \}
\]\]\]

\!\!Create your ROS node source code

Last but not least, we can create the behavior in the PhaROS environment\.
In this section, I present the source code used by the three nodes: the first node publish the sonar sensor data, the second publish the colour sensor data, the third one subscribe to the sonar publisher and move until the data is lower than 10 centimeters\.

First, I initialise the node with a connection to the robot\.

\[\[\[language=Smalltalk
EV3ROSPackage>>initialize
  super initialize\.
  ev3 := Ev3Vehicle newIp: '192\.168\.1\.4' daisyChain: \#EV3\.
\]\]\]

Then, following there are the two scripts used to publish sensor data\.

\[\[\[language=Smalltalk
EV3ROSPackage>>scriptPublishColour
  \| colourSensor colourPublisher \|
  colourSensor := ev3 sensor3\.
  colourPublisher := self node topicPublisher: '/ev3/colour' typedAs: 'std\_msgs/Int32'\.

  self paralellize looping publish: colourSensor at: colourPublisher\.
\]\]\]

\[\[\[language=Smalltalk
EV3ROSPackage>>scriptPublishSonar
  \| sonarSensor sonarPublisher \|
  sonarSensor := ev3 sensor4\.
  sonarPublisher := self node topicPublisher: '/ev3/sonar' typedAs: 'std\_msgs/Float32'\.

  self paralellize looping publish: sonarSensor at: sonarPublisher \.
\]\]\]

The usefull method used in the two previous method:

\[\[\[language=Smalltalk
EV3ROSPackage>>publish: aSensor at: aPublisher\.
  aPublisher send: \[ : m \|
    m data: aSensor read\.
   \]\.
  0\.5 hz wait\.
\]\]\]

The following methods allow the robot to move and to stop\.

\[\[\[language=Smalltalk
EV3ROSPackage>>letEv3Move
  ev3 motorB isRunning ifFalse: \[ ev3 motorB startAtSpeed: 30 \]\.
  ev3 motorC isRunning ifFalse: \[ ev3 motorC startAtSpeed: 30 \]\.
\]\]\]

\[\[\[language=Smalltalk
EV3ROSPackage>>stopEv3
  ev3 motorB stop\.
  ev3 motorC stop\.
\]\]\]


Then this is the script that allows the robot to subscribe to the sonar sensor and move\.

\[\[\[language=Smalltalk
EV3ROSPackage>>scriptMoveMeanwhileYouCan
  \(self node buildConnectionFor: '/ev3/sonar'\)
      typedAs: 'std\_msgs/Float32';
      for: \[ : m \|  \(m data < 10\) ifTrue: \[  self stopEv3\.  \]  ifFalse: \[ self letEv3Move \]\];
      connect\.
\]\]\]

Here is the end of this tutorial\.
