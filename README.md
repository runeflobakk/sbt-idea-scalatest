SBT, Scalatest and IDEA
========================


This is a step-by-step guide to set up a new SBT project from scratch with Scalatest as testing framework, and how to safely get the project into IntelliJ IDEA. It assumes that the following is already installed:

* [Scala, at least version 2.8.1](http://www.scala-lang.org/) 
* [SBT](http://code.google.com/p/simple-build-tool/)
* [IntelliJ IDEA, any version with Scala and SBT plugin installed](http://www.jetbrains.com/idea/download/)



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

Wait for SBT to create the project, and after a few seconds you should be greeted with a `>` prompt, ready to accept SBT commands. For instance,
you can type `test` to compile and test your project. Of course, there will be no code to compile, nor tests to run, but the build lifecycle will
run and hopefully print `[success] Successful.` at the end.



IDEA support
------------

To enable SBT to create project files for IDEA, we are going to install [sbt-idea](https://github.com/mpeltonen/sbt-idea/) as a SBT processor.
This will make the `idea` command globally available for all SBT projects for the current user. It is possible to include sbt-idea as a plugin
for the project, but it makes more sense to install it as a processor to avoid IDE-specific settings in the project. It will also be a one-time
task when installed as a processor.

At the SBT prompt, execute the following (be sure to include the leading asterisks):
    *sbtIdeaRepo at http://mpeltonen.github.com/maven/
    *idea is com.github.mpeltonen sbt-idea-processor 0.3.0
    ...
    update
    ...
    idea

The last `idea` command will generate project files for IDEA. When it is done, open up IDEA, select "Open project", and find the folder for the
project. You should see the familiar src/main and src/test folders, and a project folder which is where SBT keeps its project definition files.

If you have the IDEA SBT plugin correctly installed, you should see a SBT Console button at the down-left. This enables you to run SBT commands
directly from your IDE. To test it, you can open it, click the green "Play" button to launch an SBT session, and run the same `test` command as
you did earlier. It should produce the same output with the `[success] Successful.` at the end as you've already seen.




Testing with Scalatest
------------------------

To start writing tests, we must specify a dependency to the testing framework we'd like to use. For this guide we will use
[ScalaTest](http://www.scalatest.org). So far, SBT has used a default project, as we have yet to create a project definition. SBT projects are
defined using a regular Scala class implementing `sbt.Project`, usually by extending `sbt.DefaultProject`, in the `project/build` folder.

In IDEA, create the "build" folder inside the "project" folder, and then create a new Scala class called TestDoodlingProject with this content:
    import sbt._
    class TestDoodlingProject(info: ProjectInfo) extends DefaultProject(info) {
      val scalatest = "org.scalatest" % "scalatest" % "1.3" % "test"
      val junit = "junit" % "junit" % "4.8.1" % "test"
      val junitInterface = "com.novocode" % "junit-interface" % "0.5" % "test"
    }

This will get ScalaTest, JUnit, and lastly the interface which enables SBT to run JUnit-based tests.

To activate these changes, switch to the SBT prompt (either in the SBT Console in IDEA or a terminal window), and run:
    reload
    update
    idea

Right-click the project root, select "Synchronize 'test-doodling'", choose to reload project and click OK. A `lib_managed` folder should now appear
and contain 4 jar files for junit, junit-interface, scalatest and test-interface.

In `src/test/scala` create a new Scala class called MyFirstScalaTest which extends AssertionsForJUnit. It should contain one method annotated with
`org.junit.Test` where the method body only contains `assert(false)`. Run `test` in SBT and verify that MyFirstScalaTest is failing. Change to
`assert(true)`, run `test` again and verify that you get `[info] All tests PASSED.` and a successful build.

Create some test methods, either testing classes you create in `src/main/scala` or just tests doing asserts on dummy objects. Try the
ShouldMatchersForJUnit trait as described [scalatest.org/getting_started_with_junit_4](http://scalatest.org/getting_started_with_junit_4).




Creating BDD specs
------------------

See the page at [scalatest.org/getting_started_with_bdd](http://scalatest.org/getting_started_with_bdd). Use the BDD-style unit testing
capabilities offered by your prefered base class; Spec, WordSpec or FlatSpec to start implementing an `Inventory` class. Inventory is a storage
for anything you throw at it. So far it has only the following requirements:

* when just instantiated it should be empty
* when just instantiated it should not give anything back when asked to find an object of given class and String-id.

The test (specification) can start like this:
    import org.scalatest.matchers.ShouldMatchers
    import org.scalatest.{AbstractSuite, Spec}
    
    class InventorySpec extends Spec with ShouldMatchers {
    
    }
