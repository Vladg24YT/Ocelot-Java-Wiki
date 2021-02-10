# Getting started with Ocelot Brain

In this section of the wiki I will go over installation and some general facts about Ocelot Brain.

## What is Ocelot Brain?

Yes, we begin with answering that question. 
Generally speaking, Ocelot Brain is a low-level back-end library for emulating [OpenComputers](https://www.curseforge.com/minecraft/mc-mods/opencomputers) machines. 
The biggest difference compared to traditional emulators like OCEmu or OCVM is that Ocelot Brain is, in fact, pure OpenComputers code, cut from the Minecraft mod and made compatible with Scala/Java environment.

## How to install Ocelot Brain?

The installation is really simple:
1. Install Git
2. Perform `git clone https://gitlab.com/cc-ru/ocelot/ocelot-brain.git`
3. `cd` to the cloned repo
4. `sbt assembly`
5. Add `ocelot-brain-0.6.5.jar` as a library to your project

Further reading: [Understanding main instance and workspaces ->](https://vladg24yt.github.io/Ocelot-Java-Wiki/instance_and_workspaces)
