# Use of components and their configuration

As we all know, OpenComputers' computers are modular, just like real-life ones. 
In Ocelot Brain every computer's component is a standalone Entity in our Workspace.  

## Creating a basic computer

Let's build a simple computer. It will consist of:
* Tier 2 APU
* 2x Tier 3.5 Memory sticks
* Tier 1 Data Card
* Lua BIOS EEPROM
* OpenOS Floppy
* Tier 2 HDD
* Tier 2 Screen

In Ocelot Brain it is preferrable to add components in two ways:
* If a component is a **block** in Minecraft, add it to the `Workspace` via 
```java
ComponentClass component = workspace.add(new ComponentClass(tier));
caseVariable.connect(component); //Only if ComponentClass is not Case
```
* If a component is an **item** in Minecraft, add it to the `Case` via 
```java
ComponentClass component = new ComponentClass(tier);
caseVariable.add(component);
```

Here's a table of supported components, all of them are imported from `totoro.ocelot.brain.entity`:

| Component class name | Component type | How to create |
| --- | --- | --- |
| APU | Item | `APU apu = new APU(tier);` |
| CPU | Item | `CPU cpu = new CPU(tier);`,<br>set tier to `Tier.Four()` for creative CPU |
| Case | Block | `Case computer = workspace.add(new Case(tier));`,<br>set tier to `Tier.Four()` for creative Case |
| DataCard | Item | `DataCard dataCard = new DataCard(tier);` |
| EEPROM | Item | `EEPROM eeprom = new EEPROM();` |
| FloppyDiskDrive | Block | `FloppyDiskDrive diskDrive = workspace.add(new FloppyDiskDrive());` |
| GraphicsCard | Item | `GraphicsCard graphicsCard = new GraphicsCard(tier);` |
| HDDManaged | Item | `HDDManaged hdd = new HDDManaged(tier);` |
| InternetCard | Item | `InternetCard internetCard = new InternetCard();` |
| LinkedCard | Item | `LinkedCard linkedCard = new LinkedCard();` |
| Memory | Item | `Memory memory = new Memory(tier);` |
| NetworkCard | Item | `NetworkCard networkCard = new NetworkCard();` |
| Redstone | Item | `Redstone redstone = new Redstone.Tier1();`<br>or<br>`Redstone redstone = new Redstone.Tier2();`,<br> then add `import totoro.ocelot.brain.entity.Redstone.Tier1;` to the imports' section (replace Tier1 with Tier2 if needed) |
| Relay | Block | `Relay relay = workspace.add(new Relay());` |
| Screen | Block | `Screen screen = workspace.add(new Screen(tier));` |
| WirelessNetworkCard | Item | `WirelessNetworkCard wirelessNetwork = new WirelessNetworkCard();` |

Here's an example of creating a computer described above:
```java
import java.nio.file.Files;
import totoro.ocelot.brain.entity.APU;
import totoro.ocelot.brain.entity.Case;
import totoro.ocelot.brain.entity.DataCard;
import totoro.ocelot.brain.entity.HDDManaged;
import totoro.ocelot.brain.entity.Memory;
import totoro.ocelot.brain.entity.Screen;
import totoro.ocelot.brain.loot.Loot;
import totoro.ocelot.brain.util.Tier;
import totoro.ocelot.brain.workspace.Workspace;
/***/
Workspace work = new Workspace(Files.createTempDirectory("workspace"));
/***/
//Case is a block
Case computer = work.add(new Case(Tier.Three()));

//Items
APU apu = new APU(Tier.Two());
computer.add(apu);
Memory memory1 = new Memory(Tier.Six());
computer.add(memory1);
Memory memory2 = new Memory(Tier.Six());
computer.add(memory2);
HDDManaged hdd = new HDDManaged(Tier.Two());
computer.add(hdd);
DataCard dataCard = new DataCard(Tier.One());
computer.add(dataCard);

computer.add(Loot.OpenOSEEPROM().create()); //Adding Lua BIOS EEPROM
computer.add(Loot.OpenOSFloppy().create()); //Adding the OpenOS floppy

//Screen is a block
Screen screen = workspace.add(new Screen(Tier.Two()));
computer.connect(screen);
```

But what if we want to add more customization? Well, Ocelot Brain allows us to do that.

## Custom EEPROMs

By default, Ocelot Brain provides the following EEPROMs:
| EEPROM class name | EEPROM info |
| --- | --- |
| AdvLoaderEEPROM | [Advanced Loader](https://oc.cil.li/topic/1707-advancedloader-better-bios/) script by Luca_S |
| CyanBIOSEEPROM | [Cyan BIOS](https://github.com/BrightYC/Cyan) loader by BrightYC |
| MineOSEFIEEPROM | EFI script, used in [MineOS](https://github.com/IgorTimofeev/MineOS) operating system |
| OpenOSEEPROM | Default **EEPROM (Lua BIOS)** |

If that's not enough for you, you can create your own.  
**NOTE: *When adding a custom EEPROM, you will have to use unsafe casting. Do not use custom EEEPROMs, if you want to avoid that.***

To make a custom EEPROM, replace the `computer.add(Loot.OpenOSEEPROM().create());` with the following code:
```java
EEPROM eeprom = new EEPROM();
eeprom.set(computer.machine(), new Arguments((Seq<Object>) Arrays.asList(
        "\n--[[Your"
        + "\nEEPROM"
        + "\nLua"
        + "\ncode"
        + "\ngoes"
        + "\nhere]]"
        .getBytes("UTF-8"))));
        //Don't forget about `\n`s in the beginning (or in the end) of every line
eeprom.setLabel(computer.machine(), new Arguments((Seq<Object>) new ArrayList<byte[]>(Arrays.asList("Custom BIOS".getBytes()))));
computer.add(eeprom);
```
Replace `"Custom BIOS"` with the name you want to give to your EEPROM.

Further reading: [Event handling ->](https://vladg24yt.github.io/Ocelot-Java-Wiki/en/event_handling)
