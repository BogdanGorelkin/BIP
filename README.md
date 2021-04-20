# Formal modelling of block robots #

Intern:
  * [Bogdan Gorelkin](https://b.gorelkin.me)  <bogdan@gorelkin.me>

Supervisor:
  * [Simon Bliudze](https://pro.univ-lille.fr/en/simon-bliudze/) <simon.bliudze@inria.fr> _INRIA Lille_
  * [Olga Kouchnarenko](https://members.femto-st.fr/Olga-Kouchnarenko/en) <olga.kouchnarenko@univ-fcomte.fr> _FEMTO-ST_  

Project relaized in [BIP](https://www-verimag.imag.fr/Rigorous-Design-of-Component-Based.html)
---
- [x] Installling BIP
  - [x] Installing the compiler
  - [x] Declaring global variables
  - [x] Connecting C ++ to BIP
- [x] Running initial-helloworld project

<a name="menu"></a>
##  Content:
1. [Getting started with BIP](#1)
   1. [Installing the BIP](#1_1) </br>
   2. [Running the first project](#1_2) 
2. [Blinky Blocks implementation in BIP](#2)
   1. [What are blinky blocks](#2_1) </br>
   2. [Implementation of one Blinky Block in the BIP](#2_2) </br>
   3. [Implementation few Blinky Blocks in a one system](#2_3) </br>

##  1. Getting started with BIP<a name="1"></a>  [|↑|](#menu)
_BIP (Behavior, Interaction, Priority)_ is a general framework encompassing rigorous design. It uses the BIP language and an associated toolset supporting the design flow. The BIP language is a notation which allows building complex systems by coordinating the behavior of a set of atomic components. Behavior is described as a Petri net extended with data and functions described in C.

The description of coordination between components is layered. The first layer describes the interactions between components. The second layer describes dynamic priorities between the interactions and is used to express scheduling policies. The combination of interactions and priorities characterizes the overall architecture of a component. It confers BIP strong expressiveness that cannot be matched by other languages.

BIP has clean operational semantics that describes the behavior of a composite component as the composition of the behaviors of its atomic components. This allows a direct relation between the underlying semantic model (transition systems) and its implementation
### Installing the BIP<a name="1_1"></a>  [|↑|](#1)
In this description, the framework is installed under controll the Ubuntu 20.04 operating system. Notice that these tools are only provided for GNU/Linux systems. They are known to work correctly on Mac OSX, and probably other Unices. 

#### Installing dependencies
To install all the dependencies required for the compiler you simply need to install packets **default-jre, g++, cmake**, e.g. using the following command in a terminal:
```       
 $ sudo apt-get install default-jre g++ cmake
```
You will also need to install Apache Ant. To do this, run the command in the terminal
```
 $ sudo apt install ant
```   
#### Downloading the compiler
To download the program archive, visit [this page](https://gricad-gitlab.univ-grenoble-alpes.fr/verimag/bip/compiler). After successful download, unpack the archive to a convenient place for you, for example, on your desktop.

#### Installing the compiler

Open the _compiler-master/distribution_ folder in a terminal and run _wrap.sh_ with the command 
```
 $ ./wrap.sh
```
This command automatically creates a build folder into which the program engines will be installed.

Make sure that the program also set global variables by typing the command
```
 $ echo $PATH
```
Unfortunately in Ubuntu 20.04 this does not happen automatically, so we will have to do it manually. If you do this with the command
`sudo nano / etc / profile` then changes are not saved when you reboot your operating system. In order for everything to work permanently, we will have to perform the following steps:

1. Add those variables like **BIP2_ENGINE_LIB_DIR**, **BIP2_ENGINE_SPECIFIC_DIR**, **BIP2_ENGINE_GENERIC_DIR** to `/etc/environment` file. To do so run the following command. Instead of `gedit` you can also use `thisvim` or `nano`
```
 $ sudo gedit /etc/environment
```
Add following line:

>BIP2_ENGINE_LIB_DIR=/home/bogdan/Desktop/BIP/compiler-master/distribution/build/ENGINE-reference-engine/_CPack_Packages/Linux/TGZ/BIP-reference-engine-2021.03.204831-DEV_Linux-x86_64/lib/static
>
>BIP2_ENGINE_SPECIFIC_DIR=/home/bogdan/Desktop/BIP/compiler-master/distribution/build/ENGINE-reference-engine/_CPack_Packages/Linux/TGZ/BIP-reference-engine-2021.03.204831-DEV_Linux-x86_64/include/specific
>
>BIP2_ENGINE_GENERIC_DIR=/home/bogdan/Desktop/BIP/compiler-master/distribution/build/ENGINE-reference-engine/_CPack_Packages/Linux/TGZ/BIP-reference-engine-2021.03.204831-DEV_Linux-x86_64/include/generic

2. According to your Operating system create a envoromint path in the `.sh` script in the folder:`etc/profile.d/[name of your script].sh`
and add in this file following 4 lines and don't forgot to do a changes according to your compiler engine location and your OS version
>export PATH=/home/bogdan/Desktop/BIP/compiler-master/distribution/build/bipc-latest/bin:$PATH
>
>export BIP2_ENGINE_LIB_DIR=/home/bogdan/Desktop/BIP/compiler-master/distribution/build/ENGINE-reference-engine/_CPack_Packages/Linux/TGZ/BIP-reference-engine-2021.03.204831-DEV_Linux-x86_64/lib/static
>
>export BIP2_ENGINE_SPECIFIC_DIR=/home/bogdan/Desktop/BIP/compiler-master/distribution/build/ENGINE-reference-engine/_CPack_Packages/Linux/TGZ/BIP-reference-engine-2021.03.204831-DEV_Linux-x86_64/include/specific
>
>export BIP2_ENGINE_GENERIC_DIR=/home/bogdan/Desktop/BIP/compiler-master/distribution/build/ENGINE-reference-engine/_CPack_Packages/Linux/TGZ/BIP-reference-engine-2021.03.204831-DEV_Linux-x86_64/include/generic

### Running the first project<a name="1_2"></a>  [|↑|](#1)

In order to test the functionality of the installed framework, download the project folder, for example initial-helloworld by clicking [here](https://www-verimag.imag.fr/TOOLS/DCS/bip/doc/latest/examples/).

The framework is configured in such a way that you can build your project anywhere on your computer. To do this, run the script `build.sh`. 
The program will automatically create two folders _build_ and _output_.

To run the program run the command:
```
 $ ./build/system
```
<div align="center">
<img src="https://user-images.githubusercontent.com/74824667/113238790-bfcddc80-92a9-11eb-972a-7dd6cc7446f1.png"></br>
</div>

##  2. Blinky Blocks implementation in BIP<a name="2"></a>  [|↑|](#menu)
### What are blink blocks<a name="2_1"></a></br>
**The Blinky Blocks** system is a modular distributed execution environment composed of centimeter-size blocks that are attached to each other using magnets. Each block, roughly a 40mm cube, has sufficient processing, communication (serial link with up to 6 neighbors), and storage capabilities to implement a wide variety of distributed tasks.
<div align="center">
<img src="https://gitlab.inria.fr/Spirals/bip-visualsim/-/blob/master/Images/blinky_alone.png"></br>
</div>

####Overview
Each module has its own computational power, as well as sensors and actuators such as RGB leds, that can glow in different colors according to the programmer’s will. Blocks can detect their current orientation and impulses such as shaking or tapping. They are also able to play and capture sounds.

All the blocks of a system run the same program. They are attached to each other using magnets. A block can have up to 6 neighbors with which he can communicate through serial links on the block faces (neighbor-to-neighbor communication model). Only one block needs to be connected to a power supply to a power the whole ensemble. Power is distributed through the system using dedicated pins.

Ensembles of Blinky Blocks are manually reconfigurable at will: users can plug and unplug units during runtime to enable changes in behavior based on new physical groupings.

<div align="center">
<img src="https://gitlab.inria.fr/Spirals/bip-visualsim/-/blob/master/Images/hardware_blinky.png"></br>
</div>


### Implementation of one Blinky Block in the BIP<a name="2_2"></a></br>
### Implementation few Blinky Blocks in a one system<a name="2_3"></a> </br>


