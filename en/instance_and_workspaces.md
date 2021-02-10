# Understanding main instance and workspaces

So, you've installed Ocelot Brain as a library. What's next?  
Let's talk a bit about how Ocelot Brain works.

## Main static instance

Thanks to SBT tool we had used when building Ocelot Brain, Scala code was translated to suit both Scala and Java applications. 
One of the key differences we will meet now is that most of the classes became `static`, i.e. their public fields and methods can now be accessed without creating an object of that class.
One of such classes is `totoro.ocelot.brain.Ocelot`. 
This class is, technically, responsible for making all the methods in this library useful during runtime. 
In another words, it is used to start, stop and interact with the Ocelot's emulation. 
You see, Ocelot Brain works similar to a subapplication, which, by the way, freezes the entire Thread it was launched in. 
So, if you want to make an emulator with GUI, make sure you think about how you will deal with this 'freezing' first.

Now, moving on to practice-related part. To start the subapplication, invoke the `initialize()` method:
```java
import totoro.ocelot.brain.Ocelot;
/***/
Ocelot.initialize();
```
Ocelot Brain also supports Apache's Log4J logger (comes in Ocelot Brain's .jar). You can use it with the `initialize()` method like this:
```java
import org.apache.logging.log4j.Logger;
import org.apache.logging.log4j.LogManager;
import totoro.ocelot.brain.Ocelot;
/***/
//It's better if you replace 'this.getClass().getName()' with '<className>.class.getName()', where <className> is the name of the class you're invoking this method in
Logger logger = LogManager.getLogger(this.getClass().getName());
Ocelot.initialize(logger);
```
Now, when the subapplication part is started, you can freely use any method of Ocelot Brain. 
Also, remember to invoke `shutdown()` when you'll need to stop the emulation:
```java
import totoro.ocelot.brain.Ocelot;
/***/
Ocelot.shutdown();
```

## Workspaces

Workspaces are, as described in source code comments:

> All things inside of Ocelot are usually grouped by workspaces.
> Workspace is like a world in Minecraft. 
> It has it's own timeflow, it's own name, random numbers generator and a list of entities ('blocks' and 'items' in Minecraft). 
> Workspace can be serialized to an NBT tag, and then restored back from it. 
> Workspace is also responsible for the lifecycle of all its entities.

To summarize - workspace **is** a Minecraft world. To begin work with Ocelot Brain one should also create a Workspace after initializing the subapplication. For example, this code:
```java
import java.io.File;
import java.nio.file.Path;
import totoro.ocelot.brain.workspace.Workspace;
/***/
Path path = new File(System.getProperty("user.dir") + "workspace").toPath();
Workspace workspace = new Workspace(path);
```
creates a Workspace in a `workspace` directory in the program's folder.

After creating a Workspace, you can add event handlers, computers and their components. The ways of doing so will be described in the next guide.

Further reading: [Use of components and their configuration ->](https://vladg24yt.github.io/Ocelot-Java-Wiki/component_configuration)
