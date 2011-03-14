SBT, Scalatest and IDEA
========================


This is a step-by-step guide to set up a new SBT project from scratch with Scalatest as testing framework, and how to safely get the project into IntelliJ IDEA. It assumes that the following is already installed:

* [Scala, at least version 2.8.1](http://www.scala-lang.org/) 
* [SBT](http://code.google.com/p/simple-build-tool/)
* [IntelliJ IDEA, any version](http://www.jetbrains.com/idea/download/)



Creating a new SBT project
------------------------


Execute the following commands in a terminal window:
    mkdir test-doodling
    cd test-doodling
    sbt

Provide some sensible values for the prompts. Obviously say *y* to the first question, and be sure to specify *2.8.2* as Scala version.
    Project does not exist, create new project? (y/N/s) y
    Name: test-doodling
    Organization: bekk            
    Version [1.0]: 
    Scala version [2.7.7]: 2.8.1
    sbt version [0.7.4]: 

Wait for SBT to create the project, and after a few seconds you should be greeted with a `>` prompt, ready to accept SBT commands.