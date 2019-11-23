*If my info has been helpful for your hacking & you'd like to say thanks:* [*https://www.patreon.com/berryberrysneaky*](https://www.patreon.com/berryberrysneaky)

**NEWS 30**: Final Fight is not a 2nd-Gen game. It uses the same board as all the other 1st-Gen games. Controls match SF2 exactly (but FF uses only 2 buttons per player.) Should be trivial to add FF's games to a SF board, to make a "7-in-1".

**NEWS 29**: Added additional "2nd Gen" hardware and software info - 256MB RAM, runs Armbian. Uprights use 17" 1280x1024 monitor like Gen1. Countercades use 8" 1024x768 monitor. Countercades are Gen2, even if the games were a Gen1 release. (Pacman, etc.)

**NEWS 28**: Added "1st Gen" and "2nd Gen" hardware sections. Added early info about 2nd gen hardware. (2nd Gen = MK2/~~FF/~~GT and later, including the counter-cade mini machines.)

# HARDWARE INFO:

There are now two distinct hardware types - *1st Gen* games (AtariDeluxe12in1/Asteroids/AsteroidsDeluxe/Centipede/FinalFight/Galaga/PacMan/Rampage/StreetFighter2/SpaceInvaders 3/4 scale uprights) and *2nd Gen* games (MK2, ~~likely Final Fight,~~ Golden Tee, and all the countertop-size mini cabinets).  1st Gen use a single-core CPU, and 2nd Gen use a quad-core CPU.

# Hardware Info (1st Gen):

Development platform was [this Olimex board](https://www.olimex.com/Products/OLinuXino/A13/A13-OLinuXino/open-source-hardware).

**CPU** is a single-core [AllWinner A13](https://linux-sunxi.org/A13) SoC ("System On a Chip") +  [AXP209](https://linux-sunxi.org/AXP209) power controller - both are used in many low-end Android tablets.

**Memory** 128MB DDR3, **Flash storage** 128MB connected via 4-channel SPI interface

**Sound** goes through a NSIWAY NS4165 \~3W mono amplifier chip to a single 4" speaker on the control panel. On all boards I've seen, only the RIGHT audio is passed to the amp. For most games this isn't a problem, but SF2 and Gauntlet (Rampage cab) use stereo sound, and the missing left channel means missing sound effects. [Hardware repair service available on eBay](https://www.ebay.com/str/geeksales) or you can [DIY for free](https://www.reddit.com/r/Arcade1Up/comments/a02870/hack_original_board_not_just_replace_w_pietc/eah3h1k).Volume is *software-controlled* at 0%, 50%, or 100%, depending on position of the sliding volume selector.

**- Add volume control to the stock control panel & speaker**: Wire an [L-Pad](https://amzn.to/2CwPdr6) like [this diagram](https://www.facebook.com/berryberrysneaky/photos/a.1324642604359579/1324642294359610). If you disconnect the stock volume slider, max volume will be the same as the "low"/50% setting.

**USB** is accessible via TP27/TP30, near the corner farthest from the power jack. Pins are GND, D-, D+, 5V. (The middle two are reversed from standard USB wiring.) Newer boards don't have TP27/30 marked, and are missing several components from that area of the board, but it doesn't seem to affect USB operation. If you don't mind voiding your warranty, [hack up a USB extension cable and attach it here](https://www.reddit.com/r/Arcade1Up/comments/a02870/hack_original_board_not_just_replace_w_pietc/ebbk2ms) to add a USB port. Adding USB [allows a USB trackball or mouse to be used in MAME](https://www.youtube.com/watch?v=UocefehlILQ) for MAME-using cabinets.

**UART** (serial port) is accessible via pads of CON4, which is slightly covered by the large 40-pin connector. Pin-out (starting at the outside edge of the board): GND, TX, RX, ~~3.3V~~ (Use only TX, RX & Gnd). An inexpensive [UART<->USB](https://amzn.to/2Hyu97G) [adapter](https://amzn.to/2UKZZBi) and free [PuTTY client](https://en.wikipedia.org/wiki/PuTTY) will let you interact with the embedded Linux O/S. Settings to talk with board -115200,N,8,1. (Visit Windows' Device Manager to see what COM# was assigned to your USB<->UART adapter.) Username & Password  - *default*:*mimebox or newborg*:*newborg*. (To gain root access, visit software section below)

**LCD Monitor** is 17" SXGA (1280x1024) non-widescreen "4x3" ([technically 5x4](https://en.wikipedia.org/wiki/Graphics_display_resolution#SXGA_(1280%C3%971024))), with LED backlights. Video signal is sent via a 30-pin [LVDS](https://en.wikipedia.org/wiki/Low-voltage_differential_signaling) connection. LED backlight driver (traditionally called an "inverter") is powered by the 6-pin/3-wire connection: Red=12V, Black=GND, Yellow=Enable. ("Enable" input requires \~2.5V or more to turn the backlight on.)

Mainboard & LCD backlight driver board offer no way to adjust brightness, but here's [how to add brightness control to the stock monitor](https://www.facebook.com/berryberrysneaky/posts/1302581003232406)**,** or **purchase one of my** [**Plug-n-Play kits**](https://www.reddit.com/r/Arcade1Up/comments/apysg1). For the monitors that are washed out or have a lot of edge bleed, this kit can make a large improvement.

**Power Supply:** Stock power supply is 12V @ 3A (36W), with a standard 5.5mm plug (2.1mm inner pin), tip positive. I tested multiple boards and two different monitors, and found peak power draw was about 1.5A (18W). So, the stock power supply has \~18W (1.5A) of power available for other uses. **Replacement 12V 3A power supplies** via Amazon: [Option 1](https://amzn.to/2Jr6pnR), [Option 2](https://amzn.to/2URbdnC), [Option 3](https://amzn.to/2YahHQz).

**Controls** interface directly to CPU's GPIO pins, so they should have very low latency.

Trackball and Spinner use the normal [quadrature encoding](https://en.wikipedia.org/wiki/Incremental_encoder#Quadrature_outputs), but the signals are transmitted to the main board in a non-standard [serial format at settings of 115200,7,N,1](https://www.reddit.com/r/Arcade1Up/comments/abornm/trackball_interface_update/eds0c4m). (Connecting directly to the sensors would likely allow you to use a conventional USB optical encoder, for a Pi/PC/etc.)

&#x200B;

**Control panel wiring:**

Except for the speaker and power switch pins, all switches connect to ground when closed.  (Speaker & power switch each have both their connections run directly to the main board - no connection to ground.

If you're wanting to connect a Raspberry Pi, JAMMA board, etc to the original control panel, here are two different "breakout board" options. Each gives you easy screw-down terminals for the control panel's 40 pin cable, including power/volume switches and speaker. [Option 1](https://amzn.to/2UJi2aV), [Option 2](https://amzn.to/2udppf8).

|**Pin #**|**Atari 12-in-1/Asteroids**|**Centipede**|**Rampage**|**StreetFighter2**|**Galaga/Pacman/SpcInvdrs**|
|:-|:-|:-|:-|:-|:-|
|1|P1 Button A|P1 Start|P3 Up|P2 Up||
|2|UART pin 2 (track/spin)|UART pin 2 (track/spin)|P3 Down|P2 Down||
|3|P1 Button B|P2 Start|P3 Left|P2 Left||
|4|UART pin 1 (track/spin)|UART pin 1 (track/spin)|P3 Right|P2 Right||
|5|P1 Button C|P1 Button A|P3 Button A|P2 Button A||
|6|P1 Button D|P1 Button B|P3 Button B|P2 Button D (SF2 Only)||
|7|P1 Button E|P1 Button C|P2 Up|P2 Button B||
|8|P1 Start||P2 Down|P2 Button E (SF2 Only)||
|9|P2 Start||P2 Left|P2 Button C (SF2 Only)||
|10|||P2 Right|P2 Button F (SF2 Only)||
|11|||P2 Button A|P2 Start|P2 Start|
|12|||P2 Button B|P1 Start|P1 Start|
|13|UART pin 4 (5V)|UART pin 4 (5V)|(to pin 17)|(to pin 17)|(to pin 17)|
|14|GND|GND|GND|GND|GND|
|15||||||
|16||||||
|17|UART pin 4 (5V)|UART pin 4 (5V)|(to pin 13)|(to pin 13)|(to pin 13)|
|18|GND|GND|GND|GND|GND|
|19|||P1 Up|P1 Up|P1 Up (Pac only)|
|20|||P1 Down|P1 Down|P1 Down (Pac only)|
|21|||P1 Left|P1 Left|P1 Left|
|22|||P1 Right|P1 Left|P1 Right|
|23|||P1 Button A|P1 Button A|P1 Button A (Gal/SI only)|
|24|||P1 Button B|P1 Button D (SF2 Only)||
|25|||P3 Start|P1 Button B||
|26|||P2 Start|P1 Button E (SF2 Only)||
|27|||P1 Start|P1 Button C (SF2 Only)||
|28||||P1 Button F (SF2 Only)||
|29|GND|GND|GND|GND|GND|
|30|GND|GND|GND|GND|GND|
|31|GND|GND|GND|GND|GND|
|32|GND|GND|GND|GND|GND|
|33||||||
|34||||||
|35|Power Switch|Power Switch|Power Switch|Power Switch|Power Switch|
|36|Power Switch|Power Switch|Power Switch|Power Switch|Power Switch|
|37|Vol\_A|Vol\_A|Vol\_A|Vol\_A|Vol\_A|
|38|Vol \_B|Vol\_B|Vol \_B|Vol\_B|Vol \_B|
|39|Speaker|Speaker|Speaker|Speaker|Speaker|
|40|Speaker|Speaker|Speaker|Speaker|Speaker|

# Hardware Info ("2nd Gen"):

Gen 2 games include MK2/[FF](https://arcade1up.com/products/final-fight)/GT. Countertop-size cabinets *(including previously-release-as-Gen1-upright-games like PacMan)* are Gen2.

**CPU** is a quad-core [Allwinner H3](https://linux-sunxi.org/H3) SoC ("System On a Chip")

**Memory** 256MB, **Flash storage** ???MB.

**Sound** goes through a NSIWAY NS4165 mono amplifier chip to a single 4" speaker on the control panel. Gen2 boards have traces for a second amplifier chip, but only one is installed on the boards I've seen.

**Video** likely leaves the H3 SoC as HDMI, and is converted to LVDS by an external chip.

**LCD Monitor:** *Upright* cabinets use the same 17" 5x4 1280x1024 as Gen1. (Some have different backlight wiring, some are the same as Gen1.) *Counter-cades* use an 8" 1024x768 monitor. This means that countercade mainboards won't display on upright monitors and vice versa.

**Control panel wiring (Gen 2):**

Except for the speaker and power switch pins, all pins connect to ground when switch is closed.  (Speaker & power switch each have both their connections run directly to the main board - no connection to ground.

If you're wanting to connect a Raspberry Pi, JAMMA board, etc to the original control panel, here are two different "breakout board" options. Each gives you easy screw-down terminals for the control panel's 40 pin cable, including power/volume switches and speaker. [Option 1](https://amzn.to/2UJi2aV), [Option 2](https://amzn.to/2udppf8).

|**Pin #**|**Pacman**|
|:-|:-|
|1||
|2||
|3||
|4||
|5||
|6||
|7||
|8||
|9||
|10||
|11|P2 Start|
|12|P1 Start|
|13||
|14|GND|
|15||
|16||
|17||
|18|GND|
|19|P1 Up|
|20|P1 Down|
|21|P1 Left|
|22|P1 Right|
|23||
|24||
|25||
|26||
|27||
|28||
|29|GND|
|30|GND|
|31|GND|
|32|GND|
|33|**Power Switch** (Gen1=N/C)|
|34|**Power Switch** (Gen1=N/C)|
|35|**????** (Gen1=Power Switch)|
|36|**????** (Gen1=Power Switch)|
|37|**????** (Gen1=Vol\_A)|
|38|**????** (Gen1=Vol\_B)|
|39|Speaker|
|40|Speaker|

&#x200B;

# CABINET INFO (Gen1 & Gen2 UPRIGHT):

Approx 19" wide, approx 46.5" tall. (This height was likely chosen to avoid over-48" surcharges from shipping companies.) Uses 17" 1280x1024 5x4 LCD monitor with LED backlights. Some have a different backlight "inverter" and wiring (internal to the monitor chassis), some are identical to Gen1.

All appear to have been designed without the ability to adjust brightness. Brightness adjustment kit available here:

The only cabinet known to be different in structure is [Golden Tee](https://arcade1up.com/products/golden-tee). The control panel is deeper and slightly angled, and the monitor is leaned back. The side panels and control panel are definitely different from the other cabinets.

[Here's a guide](https://imgur.com/gallery/DyS36SR) to adjust the monitor to a more "leaned back" angle, closer to original arcade cabinets. (Golden Tee uses a similar angle.)

# CABINET INFO (Gen2 COUNTERCADE):

Uses 8" 4x3 1024x768 monitor, connected via 8-bit 1-channel LVDS interface. Uses

# SOFTWARE INFO (generic to all "1st Gen" cabinets):

Flash chip is 128MB total. Partition 1 is 8MB FAT, contains uImage kernel. Partition 2 is 100MB EXT4, contains Linux, MAME, ROMs, etc. Partitions 3/4 are incomplete and can be ignored.

**TEST MODE** \- allows you to see the version of the software on your board. To enter, turn machine off, set volume control to middle setting, press and hold "all" the buttons, and turn the machine on. Among the info displayed will be the software version number. (No one seems to know exactly *which* buttons are required, but pressing all or as-many-as-you-can seems to work.)

**ROOT ACCESS** \- Connect using settings listed in Hardware section. Login with any of the known accounts, use [vi](https://en.wikipedia.org/wiki/Vi#Interface) to edit /root/mameload.sh. Add a new line at the top, containing:  `echo -e "pass\npass" | passwd root` and reboot the device. Select and start any game to cause [mameload.sh](https://mameload.sh) to run, exit the game, and cycle power. (Even though the file is in \\root, its permissions are set so any user can edit it. And since it runs at whenever you start a game, it's an easy way to reset the root password to a known value - "root".) After it has run once, you can edit the file again to remove the added line.

**EMULATORS** \- there are ~~two~~ *~~three~~* ***four*** different emulations systems in use in - *MAME*, the "*MOO*" commercial emulator, RetroArch + Libretro + FBA (for Gauntlet on Rampage v1.0.1, MAME for the rest), RetroArch + Libretro + MAME2003 (for Gauntlet on Rampage v1.0.4 & v1.0.5, MAME for the rest):

* **MAME** is v0.139u1 on the 12-in-1, Centipede, Asteroids, & Rampage cabinets. Each cabinet has it's own compiled build of MAME, configured to understand that cabinet's control panel layout. (See controls in the pin-out "spreadsheet" above.) If you [add a USB port](https://www.reddit.com/r/Arcade1Up/comments/a02870/hack_original_board_not_just_replace_w_pietc/ebbk2ms), a PC keyboard allows access all the standard MAME options - *including spinner sensitivity*. USB mouse functions as a trackball (at least on 12-in-1)
* **MOO** is a commercial emulator, and appears to be built/licensed per cabinet; it's hard-coded to support *only* a specific small number of ROMs - the games that are in the cabinet. Used in SF2, Galaga, PacMan, and Space Invaders. All future cabinets are very likely to use this same emulator.
* **RetroArch + Libretro + MAME 2003** I have near-zero familiarity with. Who can give me a quick rundown on how it works, how it differs from MAME, how it's similar, etc?
* **RetroArch + FinalBurn Alpha** I have near-zero familiarity with. Who can give me a quick rundown on how it works, how it differs from MAME, how it's similar, etc?

# SOFTWARE INFO (unique per "1st Gen" cabinet):

**(See above section "Test Mode" for determining which software version your board has.)**

"Extra" ROMs (more than what cabinet was sold with) are highlighted in **bold**.

* **Atari Deluxe 12-in-1,** model 7017 (**v1.0.1**, 2018-05-24)

Uses [MAME v0.1391, compiled 2018-06-21](https://i.imgur.com/igb56ll.jpg).Linux boot process captured via the serial terminal: [https://pastebin.com/9MB5i6r0](https://pastebin.com/9MB5i6r0)File/folder structure: [https://pastebin.com/F2TUgNgJ](https://pastebin.com/F2TUgNgJ)Here's the /root/mame.ini file: [https://pastebin.com/N5xiz6SP](https://pastebin.com/N5xiz6SP).

*/root/roms* folder contains astdelux.zip, asteroid.zip, ccastles.zip, centiped.zip, gravitar.zip, liberatr.zip, llander.zip, mhavoc.zip, milliped.zip, missile.zip, bak.quantum1.zip, quantum.zip, quantum1.zip, quantump.zip, **sbrkout**.zip, **sbrkout3**.zip, & tempest.zip.

* **Asteroids:** model 6650 (**v1.0.1**, 2018-06-03):

Uses [MAME v0.139u1, compiled 2018-05-28](https://www.reddit.com/r/Arcade1Up/comments/a02870/hack_original_board_not_just_replace_w_pietc/ec6sgm6/?context=8&depth=9). Controls are connected the same as the 12-in-1, minus the trackball.

*/root/roms* folder contains **astdelux**.zip, asteroid.zip, **gravitar**.zip, llander.zip, mhavoc.zip, tempest.zip & **sf2ce**.zip

* **Asteroids:** model 6650 (**v1.0.2**, 2018-06-25):

Only difference I noticed from v1.0.1 board is /root/565/04.load - the "how to play" screen for Lunar Lander. (v1.0.1 indicated the spinner was used to move. This was removed from v1.0.2)

* **Centipede** model 6653 (**v1.0.1**, 2018-06-14):

*Version and date info gathered from log files on v1.0.2 cabinet. No other info known.*

* **Centipede** model 6653 (**v1.0.2**, 2018-06-16):

Uses [MAME v0.139u1, compiled 2018-06-21](https://imgur.com/ke9Iiwh). Controls mostly match up with Atari 12-in-1. See table above for details.

/root/roms folder contains ccastles.zip, centiped.zip, **liberatr**.zip, milliped.zip, missile.zip, & **sbrkout**.zip.

* **Street Fighter 2** model 6658 (**v1.0**, 2018-05-24)**:**

Uses the commercial "MOO" emulator.

Linux boot process captured via the serial terminal: [https://pastebin.com/rSQHKNhb](https://pastebin.com/rSQHKNhb)

File/folder structure: [https://pastebin.com/LG10cN6K](https://pastebin.com/LG10cN6K)

Emulator executable is /root/**MOO-Capcom-ShipMusl-SF** and full command line is `./MOO-Capcom-ShipMusl-SF L0 \\cat /tmp/game\.`

ROMs are located at /root/zassets/Capcom, and  *.sav.zip* files are at /root/zassets.

Additional MOO .sav.zip files (but *not* ROMs or emulator) exist at /root/docs, and shows a game we haven't seen elsewhere - "1944TheLoopMaster". Also has Final Fight, Ghosts N Goblins, and Strider. Likely this [unreleased cabinet](https://www.arcade1up.com/final-fight) will be using the MOO commercial emulator as well.

* **Galaga** model 7032, (**v1.0**, 2018-05-24)**:**

Uses "MOO" emulator too - a separate build from the SF2 one. Supports only two ROMs - Galaga & Galaxian.

File/folder structure: [https://pastebin.com/SMnwYQdv](https://pastebin.com/SMnwYQdv)

Emulator executable is /root/**MOO-MIME-ShipMusl-GG** and full command line is: `./MOO-MIME-ShipMusl-GG O3 L0 \\cat /tmp/game\`.

File system has leftover .cfg files for some MAME usage, similar to SF2 cabinet.

Early control panel PCBs have connections for joystick Up/Down, and a 4-way joystick will plug right in. (Control Panel has to be modified from a "slot" to a round hole to allow joystick to physically move up/down.) Early control Panel PCB marked: "Galaga, Pacman, Space Invaders". Later ones likely use "Final Fight" control panel PCB, but are missing the jacks for P1 Up/Down, P1 B button, and all P2 controls.

Info from MOO readme.txt:

>^(MOO-NAMCO-GG contains the data and executable for the NAMCO GALAGA / GALAXIAN cabinet.)  
>  
>^(The executable is named MOO-MIME-ShipMusl-GG Executables can be renamed to whatever you want. Executables can be placed whereever you want; they will look for their data in ./zassets)  
>  
>^(Command line parameters:)  
>  
>^(g# = Game Number \[default G0\]G0   = Galaxian \[NAMCO ROM DROP, modified version of mame:"galaxian" = Namco - Set 1.\]G1 = Galaga \[NAMCO ROM DROP, modified version of mame:"galagao"  = Namco - Rev A\])  
>  
>^(P#	= Pixel Scaling	\[default P1\]P1 = Even Pixels (game has blank area around edge, pixels are evenly sized.P2 = Full Screen (game has blank area around edge, pixels are irregularly sized))))  
>  
>^(L# = enable/disable local save  \[default L1\]L1   = Load local saved state \[if it exists\] from ./docs/gamename.sav.zip. If no local save exists, fixed save state will be loaded from ./zassets/gamename.save.zip. Local game state will be saved to ./docs/gamename.sav.zip when exiting game via 1P-START button.L0   = Never load or save local game state.)  
>  
>^(O# = Orientation of Screen \[default O1\]O1 = Top of Game on RIGHT of normal monitor \[monitor should be LEFT SIDE DOWN\]O3 = Top of Game on LEFT of normal monitor  \[monitor should be RIGHT SIDE DOWN\])  
>  
>^(-mode screensaver: instant exit from any button press)  
>  
>^(In-Game Features:EXIT:   Hold (1P-START for 3 seconds \[if local save is enabled, saves local save to ./docs\]RESET:  Hold 2P-START for 3 seconds \[loads fixed save state from ./zassets\])

&#x200B;

* **Galaga** model 7032, (**v1.0.5**, 2018-xx-xx)**:**

Info will be updated when I have time to dig through the software.

&#x200B;

* **Pacman** model 7030 (**v1.0.1**, 2018-06-30)**:**

Early control panel PCBs have connection for P1 Button "A". Button can be plugged in, and the control panel will work for Galaga or Space Invaders boards. Early control Panel PCB marked: "Galaga, Pacman, Space Invaders". Later ones marked "Final Fight", and are missing the jacks for P1 A & B buttons and all P2 controls.

&#x200B;

Uses "MOO" emulator as well.  Info from MOO readme:

>^(MOO-NAMCO-PP contains the data and executable for the NAMCO PAC-MAN / PAC-MAN PLUS cabinet.)  
>  
>^(The executable is named MOO-MIME-ShipMusl-PPExecutables can be renamed to whatever you want. Executables can be placed whereever you want; they will look for their data in ./zassets)  
>  
>^(Command line parameters:)  
>  
>^(g# = Game Number \[default G0\]g0 = Pac-Man \[NAMCO ROM DROP - modified version of mame 'pacman' Midway - US Version Set 1.\]g1 = Pac-Man Plus \[NAMCO ROM DROP - modified version of mame 'pacplus' Midway\])  
>  
>^(P#	= Pixel Scaling \[default P1\]P1 = Even Pixels \[game has blank area around edge, pixels are evenly sized.\]P2 = Full Screen \[game has blank area around edge, pixels are irregularly sized\])  
>  
>^(K#	= Graphics Patches \[Pac-Man Plus\] \[default K1\]K0 = No patches \[original graphics with COKE can.\]K1 = Patched graphics \[PAC can instead of COKE can\])  
>  
>^(L# = enable/disable local save \[default L1\]L1 = Load local saved state \[if it exists\] from ./docs/gamename.sav.zip. If no local save exists, fixed save state will be loaded from ./zassets/gamename.save.zip. Local game state will be saved to ./docs/gamename.sav.zip when exiting game via 1P-START button.L0 = Never load or save local game state.)  
>  
>^(O# = Orientation of Screen \[default O1\]O1 = Top of Game on RIGHT of normal monitor \[monitor should be LEFT SIDE DOWN\]O3 = Top of Game on LEFT of normal monitor  \[monitor should be RIGHT SIDE DOWN\])  
>  
>^(-mode screensaver: instant exit from any button press)  
>  
>^(In-Game FeaturesEXIT:   Hold 1P-START for 3 seconds \[if local save is enabled, saves local save to ./docs\]RESET:  Hold 2P-START for 3 seconds \[loads fixed save state from ./zassets\])

&#x200B;

* **Rampage** model 6657 (**v1.0.1**, 2018-06-14):

Uses MAME v 0.139u1, compiled 2018-*xx*\-*xx*. Also uses [RetroArch](http://www.retroarch.com/) \+ [*FBAlpha*](https://www.fbalpha.com/), for Gauntlet only.

*/root/roms* folder contains **ccastles**.zip, **centiped**.zip, defender.zip, gauntlet.zip, gauntlet2p.zip, gauntlet2pg.zip, gauntlet2pg1.zip, gauntlet2pj.zip, gauntlet2pj2.zip, gauntlet2pj3.zip, gauntlet2pr3.zip, joust.zip, **liberatr**.zip, **milliped**.zip, **missile**.zip, rampage.zip & **sbrkout**.zip.

* **Rampage** model 6657 (**v1.0.3**):

No info at this time.

* **Rampage** model 6657 (**v1.0.4**, 2018-*xx*\-*xx*):

Uses MAME v 0.139u1, compiled 2018-*xx*\-*xx*. Also uses [RetroArch](http://www.retroarch.com/) \+ [*Libreto MAME2003*](https://github.com/libretro/mame2003-libretro), for Gauntlet only.

They removed the extra ROMs left on earlier versions. It does have several versions of Gauntlet 2-player ROMs. */root/roms* folder contains only defender.zip, gaunt2pr3.zip, gauntlet.zip, gauntlet2p.zip, gauntlet2pg.zip, gauntlet2pg1.zip, gauntlet2pj.zip, gauntlet2pj2.zip, joust.zip & rampage.zip.

* **Rampage** model 6657 (**v1.0.5**, 2018-10-19, sometimes referred to as "*version 2*" or "fixed version" in various forums):

Uses MAME v 0.139u1, compiled 2018-10-xx. Also uses [RetroArch](http://www.retroarch.com/) \+ [*Libreto MAME2003*](https://github.com/libretro/mame2003-libretro), for Gauntlet only.

They removed the extra ROMs left on earlier versions. It does have several versions of Gauntlet 2-player ROMs. */root/roms* folder contains only defender.zip, gaunt2pr3.zip, gauntlet.zip, gauntlet2p.zip, gauntlet2pg.zip, gauntlet2pg1.zip, gauntlet2pj.zip, gauntlet2pj2.zip, joust.zip & [rampage.zip](https://rampage.zip).

* **Space Invaders** model 6999 (**v1.0.3**, 2018-10-xx):

Only has one ROM file - /root/zassets/Taito/SpaceInvaders.zip, which contains 10 .WAV samples, sicv.maincpu, sicv.proms, and sisv.maincpu.

[https://tcrf.net/Space\_Invaders\_(Arcade)](https://tcrf.net/Space_Invaders_(Arcade)) details the various versions of Space Invaders, including the "CV" and "SV" versions used here.

Uses MOO emulator. Info from MOO readme.txt:

>^(MOO-TAITO-SI contains the data and executable for the SPACE INVADERS cabinet.The SI executable is named MOO-MIME-ShipMusl-SIExecutables can be renamed to whatever you want. Executables can be placed whereever you want; they will look for their data in ./zassets)  
>  
>^(Command line parameters:)  
>  
>^(g# = game numberg0   = Space Invaders \[B & W, 'SISV'\]g1   = Space Invaders \[Color, 'SICV'\])  
>  
>^(S#  = SAMPLED SOUND OPTIONS \[default S1\]S0 = Emulated SoundS1 = Sampled Sound)  
>  
>^(P#	= Pixel ScalingP1 = Even Pixels \[game has blank area around edge, pixels are evenly sized\]P2 = Full Screen \[game has blank area around edge, pixels are irregularly sized\])  
>  
>^(L# = enable/disable local save  \[default L1\]L1   = Load local saved state \[if it exists\] from ./docs/gamename.sav.zip If no local save exists, fixed save state will be loaded from ./zassets/gamename.save.zip. Local game state will be saved to ./docs/gamename.sav.zip when exiting game via \[1P-START button.\]L0   = Never load or save local game state.)  
>  
>^(O# = Orientation of Screen \[default O1\]O1 = Top of Game on RIGHT of normal monitor \[monitor should be LEFT SIDE DOWN\]O3 = Top of Game on LEFT of normal monitor  \[monitor should be RIGHT SIDE DOWN\])  
>  
>^(In-Game FeaturesEXIT:   Hold 1P-START for 3 seconds \[if local save is enabled, saves local save to ./docs\]RESET:  Hold 2P-START for 3 seconds \[loads fixed save state from ./zassets\])

# SOFTWARE INFO (generic to all "2nd Gen" cabinets):

Runs [Armbian](https://www.armbian.com/), using a 2018 release of [U-Boot](https://www.denx.de/wiki/U-Boot). (Gen1 was a 2014 release of U-Boot.)

# MENU SYSTEM ("Gen 1" cabinets only):

The menu system works similarly for all 1st-gen cabinets. At boot, script /etc/init.d/*S99games* displays the boot video and license screen, then runs script /etc/init.d/*mame.sh* to start the menu executable (/root/*menu*) and passes it the number of game choices (Galaga=*2*, SF2=*3*, 12in1=*12, etc*) and which game was previously selected: `./menu X $(cat /tmp/selected)` (*X*=# of game choices)

A text file exists for each menu choice  (/root/*game****X,*** where **X**=1-12), consisting of only one line. For MAME it contains the the ROM name ("asteroid","tempest", etc). For MOO it's a hard-coded game number ("g0","g1","g2").

Two image files exist for each game - /root/565/***X***.\****565*** and /root/565/***X***.\****load*** (**X**=game number, 1-12). \*.***565*** are the main menu screens, and \*.***load*** are the "*how to play this game*" screens.  They are 640x480 images stored in an unusual RAW format - 24-bit RGB + 8-bit alpha channel for a total of 32-bit/pixel (\~1.2MB each). **UPDATE**: [Here's how to convert to/from PNG](https://www.reddit.com/r/Arcade1Up/comments/asv44m/how_to_convert_arcade1up_menu_screens_tofrom/) to edit the menu screens

The "menu" application displays the *.565* image matching the current choice, and changes the image shown as user rotates through the game choices. When a game is selected, contents of the matching */root/game****X*** is written to /tmp/game, and menu displays the matching *.load* image ("*How to play*" screen)*.* After P1 is pressed to start game, MAME/MOO is executed and pointed at the game name in /tmp/game:  `./mame $(cat /tmp/game)` (12-in-1), `./MOO-Capcom-ShipMusl-SF L0 $(cat /tmp/game)` (SF2), or `./MOO-MIME-ShipMusl-GG O3 L0 $(cat /tmp/game)` (Galaga).

I dug into the menu code, and it has a hard limit of 12 choices. If set to *13* on the 12-in-1 cabinet, an "invisible" 13th option exists on the main menu, available between the 12th and 1st games. The screen doesn't update, but if you push "A" then "P1 Start" while on this invisible choice, MAME opens and provides a list of games. On cabinets with fewer games, an additional page *can* ~~likely~~ be added to the menu, configured to run MAME so it will prompt with a list of ROMs. Done! See [sneak peek video](https://www.youtube.com/watch?v=iE1fIJeSeRM) & [video # 2](https://www.youtube.com/watch?v=6aUwCC_8u_o).)

\---

**NEWS 27**: Added info about **gaining root access** by modifying */root/mameload.sh*. Added how to access "**Test Mode**" to view software version number.

**NEWS 26**: Added info for Asteroids v1.0.2. (Same as v1.0.1, except for the Lunar Lander "how to play" screen.)

**NEWS 25**: Added info for Space Invaders v1.0.3. As expected, uses MOO emulator and the controls match up with Galaga & Pacman. Has only one .ZIP file that contains both games and ten .WAV audio samples. Emulator can be configured to use samples or emulate the sound effects.

**NEWS 24**: Added info on [modifying/replacing the stock menu screens](https://www.reddit.com/r/Arcade1Up/comments/asv44m). Listed all included ROMs on each version of the boards that use MAME - any "extra" games available via USB mod + MAME menu are **bolded**. Visit each game below in the "SOFTWARE" section for the added info. Confirmed memory is DDR3 (SoC supports 2 & 3), and flash ROM is connected via quad-channel SPI interface.(*OLDER NEWS AT BOTTOM OF POST*)

**NEWS 23:** As promised, [here's how to wire](https://imgur.com/H7ri6Yn) an [L-Pad](https://amzn.to/2CwPdr6) as a volume control with the stock control panel/speaker. First batch of [Plug-n-Play kits for brightness control](https://www.reddit.com/r/Arcade1Up/comments/apysg1) available. (Still free to [DIY your own brightness control](https://imgur.com/gallery/Iir2wcE).)

**NEWS 22:**  Added info for adding an [L-Pad](https://amzn.to/2CwPdr6) as an inexpensive volume control, and several options for [replacement](https://amzn.to/2Jr6pnR) [power](https://amzn.to/2URbdnC) [supplies](https://amzn.to/2YahHQz).*UPDATE: More information coming about installing the volume control. I have one on the way from Amazon. :)*

**NEWS 21:**  Added  how-to for [**adding brightness adjustment to stock LCD backlight**](https://imgur.com/gallery/Iir2wcE). Added technical details about the stock monitor, the interface between it and the board. Added info about Rampage 1.0.4.

**NEWS 20:**  A Redditor asked about how much power the stock system draws. I tested several boards and two different LCDs. Peak power draw was during the boot video, and was 1.3A-1.5A (depending on monitor, they vary a bit.) So, **\~18W (\~1.5A)  of the 36W (3A) power adapter is available for other uses.**

**NEWS 19**: Update on Trackball/Spinner protocol (serial @ 115200,7,N,1, and added info for Asteroids (control panel wiring is same as the 12-in-1.)) *Trackball should be easy to add to Asteroids cab.*

**NEWS 18**: After many requests, I've partnered with an eBay store to offer ***USB mod + Sound fix  + UART pins installation services***, available here: [***https://www.ebay.com/itm/163461358596***](https://www.ebay.com/itm/163461358596) *(UPDATE:* ***or just the sound fix:*** [https://www.ebay.com/itm/153374535545](https://www.ebay.com/itm/153374535545))

**^(NEWS 17:)** ^(Determined the specific games supported by each MAME build;) [^(info added here)](https://www.reddit.com/r/Arcade1Up/comments/a02870/hack_original_board_not_just_replace_w_pietc/ecuw3ho)^(. Uploaded a) [^(second YouTube video)](https://www.youtube.com/watch?v=6aUwCC_8u_o)^(, showcasing most of the games playable w/Rampage's MAME.)

**^(NEWS 16:)** ^(Christmas update: Bad news on the MAME front. We knew each game had a copy compiled to support the specific controls in that cabinet, but it turns out each may be compiled to support \*only the games in that cabinet\*. I tweaked the config to run MAME from USB and added a number of ROMs, but the menu only showed the games already included in the cabinet. Troubleshooting, I ran "MAME -listclones" on Rampage's MAME, and only \~10 games (plus their many clones show up. Didn't know MAME could be compiled to support only a handful of games, but I suppose it would save space to not support systems/CPUs/etc that won't be used. Will check other cabinets' MAMEs soon.))

**^(NEWS 15:)** ^(Added section for control panel wiring, which allows me to confirm that MAME from the Rampage cabinet will work for Pacman/Galaga cabinets! Already knew the basic joystick functions worked, but confirmed that everything for Player 1 matches exactly, except P1/P2 Start. (And they match up w/ P2 buttons A & B, so can easily be remapped. Also, appears we can easily add a second fire button to Galaga/Pacman, even though it's not on the circuit board - Pin 24 & GND. (or add 1 or 2 buttons to PacMan)))

**^(NEWS 14:)** ^(New info gathered from Rampage v1.0.5 - a third emulator has entered the mix. Success at getting MAME to see/run ROMs stored on USB flash drive. Info added for Centipede. Software info split out into "generic" (all cabs and "specific" (per cab.)))

**^(NEWS 13:)** ^(Added a guide for adjusting the monitor to a more original-arcade-cabinet angle:) [^(https://imgur.com/gallery/DyS36SR)](https://imgur.com/gallery/DyS36SR)

**^(NEWS 12:)** ^(Uploaded a sneak preview video, showing MAME running on a "MOO" cabinet:) [^(https://www.youtube.com/watch?v=iE1fIJeSeRM)](https://www.youtube.com/watch?v=iE1fIJeSeRM)

^(\*\*NEWS 11\*\*: Info about the MOO emulator discovered on Galaga board. Added serial terminal login that works on all games I've seen so far - \*default\*:\*mimebox\*.)

**^(NEWS 10:)** ^(Added basic info for Galaga, with more to come. Adding an invisible 13th option to 12-in-1 menu opens MAME directly, and MAME displays a list of all the ROMs on the system. \*\*This is probably our best bet for adding games to the MAME cabinet with the least amount of effort\*\*.  Will try to create a process that moderately-technical folks can follow.)

**^(NEWS 9:)** ^(Created instructions for) [^(adding a USB port)](https://www.reddit.com/r/Arcade1Up/comments/a02870/hack_original_board_not_just_replace_w_pietc/ebbk2ms) ^(to 12-in-1, to access MAME options. (Probably useful for Asteroids, Centipede, Rampage, maybe others. Cleaned up info above, separated the two emulation systems, and added details about menu operation.))

^(\*\*NEWS 8\*\*: Emulation-related updates. On Atari12in1,) [^(MAME is v 0.139u1)](https://i.imgur.com/igb56ll.jpg)^(. Adding a USB port to the board and connecting a PC keyboard allows you to) [^(access all the standard MAME options)](https://i.imgur.com/qpE9DQD.jpg) ^((TAB to open menu, and the settings stick after a reboot.)) **^(This includes adjusting screen scaling options, spinner sensitivity, etc!)** ^(Currently, the only way to access these options is to solder a USB connection to the board. \*This change will almost certainly void your warranty\*, but I'm looking at creating a solder-free USB add-on.)

^(\*\*NEWS 7\*\*: Successfully connected USB flash drive to both SF2 and Atari 12-in-1 boards, and used "dd" via the serial terminal connection to create a full image of each device. Flash storage on both appears to be 128MB, not larger as I originally thought. (The 3rd partition referenced appears to be a remnant from the development platform. Attempting to mount it fails, and nothing seems to access it. When directly reading flash chip via external hardware, only the first \~12MB of 3rd partition is readable - 8MB + 100MB + 12MB = 128MB. 3rd partition can be ignored. The Atari 12-in-1 definitely uses MAME. SF2 appears to use a self-contained emulator + external ROM files. (Some MAME files exist, but don't seem to be used. Executable is named "MOO-Capcom-ShipMusl-SF". When run, shows "MOO Emulation Copyright - Built Apr 13 2018 03:30:15".))

^(Number of games is set in /etc/init.d/ - line is: ./menu,) **^(XX)** ^(\`cat /tmp/selected\` where XX is number of games - 3, 12, etc.)

^(Front-end menu looks like it will be pretty easy to modify, other than an odd image file format. Menu on both boards uses two RAW 640x480 images for each game, game\*\*XX\*\*.565 and load\*\*XX\*\*.565. (XX=01-03 on SF2, 01-12 on Atari-12-in-1 The first image is the on-screen "menu", the second is the "how to play this game" screen displayed after a game is chosen/activated. The "menu" system is just displaying each "game\*\*XX\*\*" image, one at a time, as joystick movement selects a game. But the images are created in a way that)) *^(looks)* ^(like a scrolling menu system - each image shows game(s above/below current selection, has page #s, etc. Images seem to be in a rgb565-type format, but have)) *^(4)* ^(bytes per pixel rather than 3 - 1228800 bytes per image. (Alpha channel? Can preview them in GIMP, but can't open.There is a "game\*\*X\*\*"  text file per game, and each contains only the ROM name, and a blank line.  ("game1" = astdelux, "game12" = liberatr, etc.. Menu must read the appropriate game)))*^(X)* ^(file, and pass that ROM name to MAME.)

^(\*\*NEWS 6\*\*: Login name and password for serial interface on SF2 board are both "\*\*newborg\*\*". This same combination does) *^(not)* ^(work on the Atari 12-in-1 board.)

**^(NEWS 5:)** ^(Gathered serial output for Atari 12-in-1 cab too. Added above.)

**^(NEWS 4:)**  ^(Successfully pulled the 2nd partition from the flash - it's \~100MB and  contains Linux, MAME, the ROMS, etc! (Based on some partition info I  gathered, I believe it's 512MB in total, with a 3rd unknown partition taking up the remainder.)

**^(NEWS 3:)**  ^(Successfully interfaced with the UART (serial port. Here's a capture  of the serial output at boot, from a Street Fighter 2 board:)) [^(https://pastebin.com/rSQHKNhb)](https://pastebin.com/rSQHKNhb)^(And from  the Atari 12-in-1:) [^(https://pastebin.com/9MB5i6r0)](https://pastebin.com/9MB5i6r0)

^(Some interesting info right off the bat:)**^(CPU:   Allwinner A13 (SUN5I\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*)****\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*)** ^(& \*\*DRAM:  128 MiB\*\*- We already knew the CPU, but RAM size is good to know.)**^(Board: A13-OLinuXino)** ^(- Interesting. Their development platform was very likely) [^(this board)](https://www.olimex.com/Products/OLinuXino/A13/A13-OLinuXino/open-source-hardware)^(.)**^(\[    0.055267\] \[usb\_manager\]: CONFIG\_USB\_SW\_SUNXI\_USB0\_HOST\_ONLY)**  ^(- This explains why I had trouble interfacing via USB; I was attempting  to access it as a peripheral/slave device (as USB mass storage, etc. It  can) ~~^(probably)~~ *^(access)* ^(a USB flash device, but won't act like one.  (Update: changing this setting did) *^(not)* ^(allow a PC to access it, but can confirm the board) *^(can)* ^(access a USB flash drive.))**^(\[    1.317732\\\] mousedev: PS/2 mouse device common for all mice)**  ^(- The trackball and spinner electronics look very similar to the  workings of a PS/2 mouse with a scroll wheel from 10+ years ago. Could  be how they're interfaced/used.)**^(Playing arcade1up.avi)** ^(&) **^(VIDEO:  \[H264\]  640x480  24bpp  24.000 fps  1380.3 kbps (168.5 kbyte/s\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*)****\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*)** ^(- The startup video can almost certainly be edited/replaced.)**^(menu ver:May 28 2018)** ^(&) **^(game count=3)** ^(\\- The first info I've seen about games, menu info, etc.)

^(After booting, the board displays info about each button press,including volume up/down:)

    keysDown 1P UP down 12 update_image: 2 down 13 keysDown 1P DOWN down 13 update_image: 0 down 13 keysDown 1P DOWN down 13 keysDown 1P DOWN down 13 update_image: 1 down 12 volume: 50   volume\_cb: 50   volume: 100   volume\_cb: 100   volume: 50   volume\_cb: 50

^(Volume is \*CPU-controlled\*, and gets set to 0, 50, or 100, depending on the position of the switch - "-"(left connects one input to GND or "+" (right connects another.))

**^(NEWS 2:)**  ^(I've successfully pulled the) ~~^(software image)~~ *^(8MB boot partition (uImage)**\*\*\*\*\*\*\*\*\*\*)* ^(from my SF2 board!)

**^(NEWS 1:)** ^(First hardware hack found -) [^(Missing left-channel channel audio in SF2 fixed)](https://www.reddit.com/r/Arcade1Up/comments/a02870/hack_original_board_not_just_replace_w_pietc/eah3h1k)^(. UPDATE - Gauntlet has similar issue, and the same fix works.)
