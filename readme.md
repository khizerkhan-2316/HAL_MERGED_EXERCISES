# Exercise 1: Getting Started with Linux Development (Rpi)

Søren Hansen \<sh@ece.au.dk\>, revised Peter Høgh Mikkelsen
\<phm@ece.au.dk\>

## Purpose

This exercise gives you basic knowledge and experience with using Ubuntu
and the shell. You will also learn how to connect to- and copy files to
another device using the secure shell tools ssh and scp.\
With this exercise you are expected to fulfill the following
objectives:
| **Objective** | **Part 1** | **Part 2** | **Part 3** |
|---------------------|-------------------------------------|-----------------------------------|-------------------------------|
| **File System** | Navigate in the file system using the shell | Create/delete files | Change file permissions |
| **Program Control** | Create and execute scripts | Create background tasks | Control task execution |
| **Remote Connection**| Establish SSH connection to a remote target | Copy files to a remote target | Execute tasks on a remote target |

## Prerequisites

- Have VMware Player installed (Fusion Personal for Mac / Workstation
  Pro Personal for Windows)
  ([Windows/Linux/X86Mac](https://www.mikeroysoft.com/post/download-fusion-ws/))
  or Parallels ([Mx Mac](https://www.parallels.com/eu/))
- Have downloaded the compressed VMware Develper Golden Image
  ([Intel-AMD-PC-Mac](https://redweb.ece.au.dk/files/DeveloperVM.7z) )
  ([ARM64-Mac-Mx](https://redweb.ece.au.dk/files/DeveloperVM-arm64.7z)
  ) to your PC (5GB)
- Have unpacked the image using [7-zip](https://www.7-zip.org/)
  - **Errors seen with WinRAR etc!!!**
- Have downloaded the Raspberry Pi SD card image
  ([rpizerowifi-image-raspberrypi0-wifi.rpi-sdimg](https://redweb.ece.au.dk/files/rpizerowifi-image-raspberrypi0-wifi.rpi-sdimg)
  ) to your PC (1GB)
- Have burned the image to your Raspberry Pi's SD card
  ([Windows](https://redweb.ece.au.dk/devs/projects/raspberry-zero/wiki/Update_SD_Card_Image_in_Windows))

**Please download the files prior to class to prevent overloading our
server and wireless network**

## Getting started with Linux

### Booting Lubuntu

Start VMWare Player and open the Golden Image (In VMWare Player: File -
Open, browse to the directory where you unpacked the Golden Image). Then
turn on the Golden Image virtual machine (green arrow, top left-hand
corner) and watch Lubuntu boot

### Login

When Lubuntu has booted you will be faced with a login screen. Choose
the login name _stud_ and use the password _stud_

### Getting acquainted with the terminal

Start a terminal - click on the icon on the Desktop or in the Start
menu.\
Before you start be aware that the shell running in your terminal is
called bash. Help for some of the following commands can be found by
examining the manual on bash. The manual can examined by running _man
bash_ in the terminal, an alternative is obviously to JFGI.

#### File & Directory operations

Open a terminal and write down which commands you use to:

- Display the full path of the current folder?
- What does the "∼" mean in relation to a path in Unix?
- Get a list of all ﬁles and folders in the current folder?
- Change directory to /home/stud?
- Create a directory /home/stud/hal
- Change directory to the subdirectory /home/stud/hal
- Create a directory exercise1
- Change directory to the subdirectory exercise1
- Create a ﬁle text1 containing "hello nano" using nano?
- Create a ﬁle text2 containing "hello echo" using echo?
- Append 1234567890 to file text1 using echo?
- Dump the contents of text1 to the terminal window?
- Create a directory temp
- Copy text1 to the directory temp
- Remove the subfolder temp and its content
- Delete text1 and text2 in one go?
- Install Visual Studio Code extensions (Code itself is already
  installed) \[\[Install_vscode\]\]
- Create a small c program, hello_linux.c that prints out the text
  "Hello Linux!"
- Compile the program using gcc "(tips
  here)":https://itsfoss.com/run-c-program-linux/
- Run the program, change file permissions with chmod if required
  "(tips here)":https://itsfoss.com/run-c-program-linux/

Explain how ﬁle permissions work - check the chmod and read its man
page.

#### Program control

Write down which commands you use to:

- Run the program nano in the background
- Now kill the program nano you just started

Now, write a small shell script that echos "hello world" every second.
Search for bash-scripts, while-do, sleep and echo. Remember to make your
shell script executable using the program chmod.

Note down your script and the apropriate commands to make it executable
and how to terminate it.

#### Acquiring system information

Write down which commands you use to:

- Get a list of the currently running processes (programs)
- Display the current date and time in the terminal
- Find the IP address of the network adapter ens33xxx
- Explain what the ﬁle /var/log/syslog does
- Try using running less /var/log/syslog and read the manual for less.
  What is it good for?
- What happens when you run dmesg?
- Extending the above like this: dmesg\|less, what does it do?
- Determine the CPU type by looking the directory /proc

There is a lot of system information to ﬁnd in /proc, mention at least 3
diﬀerent ﬁles and what they tell you?

Use Google or other resources to ﬁnd the command(s) necessary to
complete the exercise.

### Connecting and using a remote Linux target

The next couple of steps will enable you connect to a remote Linux
target over the network. With network access, you can run remote
terminals, copy files back and forth and much more.

#### Connect to the target

The host and target can be connected by diﬀerent means as shown below.
One is via a serial connection, another is via a network connection,
that works via the USB subsystem and finally there is WIFI. We are using
the Ethernet over USB connection (also known as tethering), because WIFI
(eduroam) is going to give too much headache at this point (addressing,
security and more) and the serial port has too limited functionaliy. The
USB/Ethernet connection is configured to have fixed IP address on the
Raspberry Pi, which makes it easy to locate from your PC.

![width: 80%](../images/pc_rpi_connections.png)

Now, connect your remote device (Rpi) to your PC using the USB cable.
Wait a minute until it has booted and the green led is steady. Now the
remote device (RPI) should become available in the VM-Ware
(Player-\>Removable Devices) as a "Netchip RNDIS/ Ethernet Gadget". It
is important to enable it in VM-Ware and thus make it available to
Ubuntu. if it's not available in VM-Ware you may have forgotton to
insert/program the SD card or be using the wrong USB plug on the RPI.

> The Raspberry Pi is preconfigured to have the IP address of 10.9.8.2 on the RNDIS/Ethernet Gadget interface. In Ubuntu, the RNDIS/Ethernet Gadget interface is preconfigured to use the name "usb0" and to use the IP address 10.9.8.1. This means we have an RNDIS/Ethernet Gadget connection between Ubuntu (10.9.8.1) and Raspberry Pi (10.9.8.2).

When done open a terminal and check that the usb device interface is now
avaliable:

    ifconfig

Next up is verifying that the connection actually works, which the focus
of the next steps

### Test connection

You can start out by pinging the remote device, to check that it is
available on the network:

     ping 10.9.8.2

You can stop pinging by pressing ctrl-c.

If you are not able to ping the Raspberry Pi, the problem may be caused by a race condition during setup, that we (the teachers!) have failed to resolve. The work-around is to disconnect the RNDIS Ethernet gadget in VMWare and reconnect it. Now it should work!

Now, connect to the target using Secure Shell (SSH) - see this
[How-To](https://support.suso.com/supki/SSH_Tutorial_for_Linux) for
details on how to use SSH.\
The Rpi is configured to have only one user, root, with no password. So consider what login credentials to use with SSH....?.\
If you do not wish to specify an empty password each time you connect to
target see http://linuxproblem.org/art_9.html

If you get the error message ssh: connect to host 10.9.8.2 port 22: No route to host, your USB interface is probably down. Return to the previous point to resolve this.

You may be warned about a "MAN-IN-THE-MIDDLE-ATTACK". Read the message carefully and do as it is suggested to remove the offending key:

```bash
ssh-keygen -R 10.9.8.2
```

Now try to establish the connection again.

> When an ssh client (Ubuntu) connects to a ssh server (Raspberry Pi) it > stores the servers IP address together with it unique ID. If the client at a later point connects to the server at a given IP address and the ID does not match, it could be that someone is faking to be that server (to snoop your password etc). This is a
> "MAN-IN-THE-MIDDLE-ATTACK". In your case it may happen because you are using VMWare image with an old connection log.

### Move a ﬁle and execute it on target

Copy your hello world script to target using Secure Copy, scp. Check
that the ﬁle is actually present on the target. Use the man-pages if you
need help on how it works.\
Execute the script on the target (you must have a ssh connection, this
could be in another terminal window)\
Open a new shell window in Ubuntu and create a new ssh connection to
target (script still running).\
Locate the process ID of the script running in the other remote
connection with "ps -a"\
Kill the script (which is running in the other shell terminal)

This shows how to start, but also how to kill a unresponsive task
running on a remote target. (Some tasks run as background services and
will restart automatically, that's another story....)

### Create shell scripts for ssh and scp (optional)

Albeit optional but very convenient to have completed. The commands for
ssh and scp are repetitive and tedious. Create two small shell scripts
as\
follows:

- conn2tgt should establish an SSH connection to the Rpi on IP address
  10.9.8.2 (the USB interface)
- cp2tgt \[file\] should copy the ﬁle/folder file to the Rpi

Checkout the program sshpass, it is very useful when/if you have a
target that has a password, as you can pass the password to ssh or scp
via command line options to sshpass. If its not installed use the
command: _sudo apt-get install sshpass_.

### Run your C-program on the Raspberry Pi (optional)

To run your C-program on the Raspberry Pi, you will have to compile it
with a cross-compiler, that can create binary code for an ARM processor.
To build you and run your program you must:

- Compile your code with arm-rpizw-gcc (instead of just gcc)
- Copy the binary file to the Raspberry Pi using scp
- Connect to the Raspberry Pi using ssh (Can be in a new terminal
  window, so you can have one local and one remote)
- Run the program on the Raspberry Pi, change file permissions if
  needed

  # Øvelse 2: Kernel Interface (Rpi)

  ## Formål

  Denne øvelse giver erfaring med at anvende metoder til at tilgå ressourcer ved hjælp af operativsystemet. Ved at skrive og bygge applikationer som anvender file I/O til at tilgå forskellige devices, øves brugen af dette, samt der opnås erfaring med at bygge programmer som kan køre i Linux på en PC og på et embedded target (Rpi). Dette er nødvendigt for senere at kunne skrive (test)programmer til test af de device drivers som der senere skal arbejdes på.

  Du forventes med øvelsen at opfylde følgende mål:
  |Mål |Delmål 1 |Delmål 2 |Delmål 3 |
  |-------------------------------------------|---------------------------------------|---------------------------------------|-----------------------------|
  |Anvende filoperationer i shell |Læse diagrammer | Konfigurere GPIOer | Læse/skrive Switch/LED |
  |Anvende filoperationer i C |Skrive koden | Bygge (evt Makefile) |Overføre/eksekvere på Rpi |
  |Anvende filoperationer i C til I2C |Forklare hvordan man læser fra LM75 |Anvende systemkald korrekt |Overføre/eksekvere på Rpi|

  ## Godkendelse

  Denne opgave indgår ikke i de opgaver som skal godkendes for at gå til eksamen, men indholdet er alligevel vigtigt.

  ## Forberedelse

  - Du skal have læst kapitel 4 i "The Linux Programming Interface".
  - Du skal have et funktionelt VM-Ware Image
  - Du skal have en Rpi Zero (W), med ASE fHAT monteret med lysdioder, knapper og LM75 (SW1-2, LED1-3, R1-5, U2), se figur:

  ![ASE fHAT Board](../images/ase_fhat_board_ex1.png){width: 35%}

  BEMÆRK! Der skal monteres et hunstik på undersiden af printet, således at printet kan monteres ovenpå Raspberry Pi

  ## Øvelsen

  Øvelsen indeholder underopgaver som anvender filoperationer til at tilgå system devices og tekstfiler.

  ### System adgang fra shell

  I Linux finder du de fleste devices under /dev/. Enkelte devices finder du imidlertid kun i SysFS folderen, /sys/. SysFS uddybes i en senere lektion, for nu skal vi blot tilgå nogle filer her.

  ASE fHat har nogle lysdioder (LEDs) og trykknapper som vist på figuren ovenfor.

  LEDs og knapper er forbundet via pin-headeren til Rpi's processor's GPIO porte. Forbindelserne på ASE fHat kan ses i diagrammet [diagrammet over ASE fHAT](https://redweb.ece.au.dk/devs/projects/raspberry-zero/repository/hardware/revisions/master/raw/datasheets/ase_fhat_schematic.pdf). GPIO portene er benævnt som på processoren. Man kan følge forbindelserne fra pin-header til processor i [Raspberry Zero W diagram](https://redweb.ece.au.dk/devs/projects/raspberry-zero/repository/hardware/revisions/master/raw/datasheets/RPI-ZERO-V1_3_reduced.pdf). Et udsnit af forbindelserne er vist nedenfor

  ![Raspberry Zero W Schematic](../images/rpi_zw_schematic_gpio.png){width: 60%}

  Som nævnt gælder:"In Unix (Linux), everything is a file! If it is not a file, it's a process", så man kan tilgå disse devices som var de filer. Dvs. man kan åbne filen som repræsenterer devicet, og læse/skrive til filen for at anvende devicet.

  Vi vil nu prøve at tilgå knapper og LEDs, som filer, ved at anvende shell kommandoerne _cat_ og _echo_ som netop læser og skriver filer.

  a) Vælg en trykknap (SW1 eller SW2) og find dens tilhørende GPIO port i [diagrammet over ASE fHAT](https://redweb.ece.au.dk/devs/projects/raspberry-zero/repository/hardware/revisions/master/raw/datasheets/ase_fhat_schematic.pdf). Herefter skal du skrive dette nummer til en fil /sys/class/gpio/export) hvorved at en speciel user-space gpio driver kaldes og opretter en folder indeholdende funktioner til netop denne gpio:

  ```sh
  root@raspberrypi0-wifi:~# echo <gpio nummer/sys/class/gpio/export
  ```

  Du skal erstatte `<gpio nummer>` med nummeret som den pågældende gpio har, f.eks. 56

  Herefter kan du checke om porten har den rigtige retning (input/output) ved at læse portens _direction_ attribute, som er beskrevet i en tilhørende fil:

  ```sh
  root@raspberrypi0-wifi:~#  cat /sys/class/gpio/gpio<gpio nummer>/direction
  ```

  Hvis den er forkert rettes den ved at skrive til attributten med _echo_. Mulighederne er _in_ og _out_

  b) Læs nu portens tilstand ved at læse attributten _value_. Læs flere gange og tryk-/slip knappen og observér at værdien ændrer sig.

  c) Gør tilsvarende for en LED af eget valg. Dokumentér gerne din test med et billede på din wiki.

  Spørgsmål til reflektion: _Hvad gør hhv cat og echo programmerne? Hvilke C I/O funktioner kalder de mon underliggende? Hvordan kan de tilgå den indbyggede GPIO driver og dermed hardware?_

  ### Adgang til LED/Key fra egen applikation

  Vi vil nu selv skrive og bygge en C-applikation til at tilgå hardware enhederne.

  d) Skriv en simpel C-applikation som anvender Posix File-I/O (Se The Linux Programming Interface kapitel 4) til at tænde og slukke en LED med 1 sekund interval. Du kan bruge nano, Emacs, eller hvilken som helst tekst editor efter eget ønske. Din applikation kan eventuelt tage LED nummeret som parameter vha argv/argc, som vist i slides, men prøv først uden.

  Du bygger applikationen på følgende vis:

  ```sh
  stud@stud-virtual-machine:~$ arm-rpizw-gcc -o my_exe my_source.c
  ```

  Programmet kopieres til target vha _scp_ og eksekveres med:

  ```sh
  root@raspberrypi0-wifi:~#  ./my_exe
  ```

  ### Adgang til I2C temperatursensor

  ARM processoren har et antal ["I2C"](https://en.wikipedia.org/wiki/I%C2%B2C) busser. Enheder på en I2C bus er nummerede, de har hver en "device" adresse.

  På ASE fHAT er monteret temperatursensoren LM75, som har et I2C interface. Chippen har tre ben til at sætte de 3 nederste bits af adressen (A2-A0). De er som udgangspunkt '0', men kan sættes '1' ved at montere R8/R9 og skære MEK7/8 over (Se [wiki afsnit](https://redweb.ece.au.dk/devs/projects/raspberry-zero/wiki/ASE_Extension_Module)).

  Kun processoren kan tage initiativ til en data overførsel. For at lave en læsning, sender den en forespørgsel til en device addresse og modtager et resultat. Et eksempel er vist herunder:

  ![LM75 16-bit read](../images/800px-Lm75_16-bit_read.png){width: 80%}

  For den pågældende temperatursensor, ["LM75"](https://datasheets.maximintegrated.com/en/ds/LM75.pdf), gælder der at den første byte læst indeholder temperaturen i heltal, mens den anden indeholder en decimal. Se databladet for mere info.

  Rpi giver direkte adgang til I2C busserne fra en applikation, da der er installeret en speciel driver, ["i2c-dev"](https://www.kernel.org/doc/Documentation/i2c/dev-interface). Denne skal dog først loades:

  ```sh
  root@raspberrypi0-wifi:~#  modprobe i2c-dev
  ```

  Hvis du vil have loaded den automatisk hver gang du booter Rpi, så kan du oprette en fil med indhold som vist:

  ```sh
  root@raspberrypi0-wifi:~# echo i2c-dev /etc/modules-load.d/i2c_dev.conf
  ```

  Når der loades moduler under boot, bliver folderen /etc/modules-load.d/ skannet igennem og indholdet af filerne læst. Her skriver vi at den skal loade modulet i2c-dev. Hvis den ikke længere skal loade i2c-dev, skal du blot slette filen.

  For at benytte i2c-dev til at læse og skrive i2c data, skal man først anvende et systemkald til at sætte adressen på det device som man vil kommunikere med, dette system kald tilgås ved hjælp af _ioctl_ funktionen. Man kan efterfølgende anvende _read_ og _write_, blot skal beskederne være byte (aka char) orienterede. Vil man læse 3 bytes sætter man derfor læselængden til 3 og husker selvfølgelig at have et byte (char) array som er stort nok til returværdien (eller en ptr til et allokeret område).

  ```c
  fd = open("/dev/i2c-<busno>", O_RDONLY);
  int err = ioctl(fd, 0x0703, <i2c device address>); // i2cdev sys call (0x0703) to set I2C addr
  read(fd, <data>, <datalen>);
  close(fd);
  ```

  Se flere detaljer omkring i2c-dev [kernel dokumentationen](https://www.kernel.org/doc/html/latest/i2c/dev-interface.html). Vær opmærksom på at der er installeret en i2c utility på Raspberry Pi, [i2cdetect](https://linux.die.net/man/8/i2cdetect) hvormed man kan probe hvilke devices som sidder på hvilke i2c busser.

  e) Opdater din applikation til at udlæse temperaturen fra LM75 med 1 sekunds interval. Brug _printf()_ til at skrive til terminalen. Det er frivilligt at udlæse decimalen i temperaturen.

  Spørgsmål til reflektion: _Hvorfor mon vi bliver nødt til at anvende et ioctl kald til at angive den specifikke I2C adresse? Hvordan finder jeg ud af om der faktisk bliver responderet af et i2c device på den givne adresse? Hvorfor er det nødvendigt at angive datalen, selvom vi godt ved længden af arrayet som vi sender med?_

  ### Lav over-temperatur alarm

  f) Opdater din applikation således at lysdioden lyser når temperaturen kommer over en given selvvalgt temperatur. Skriv desuden en tekst advarsel til stdout (husk stdout har prædefineret file pointer)

  ### Skriv til web-server

  Der kører en web-server på Rpi. Vi vil nu på simplest mulig måde gøre således at temperaturen kan udlæses fra Rpi'ens hjemmeside.

  På Rpi finder du dens start hjemmeside her:

  ```sh
  /www/pages/index.html
  ```

  Indholdet er beskedent:

  ```html
  <html>
    <body>
      <h1>It Works!</h1>
    </body>
  </html>
  ```

  g) Det er din opgave skrive en tekststring til den pågældende fil, men hvor "It works!!!" er erstattet af f.eks. "Temperatur: 27.5 grader". Du kan anvende _sprintf()_ til at formattere tekststrengen, inden du skriver til filen.

  Dette er et eksempel på hvordan vi også kan anvende tilgå almindelige tekstfiler med read/write -og hvordan filer anvender overalt i systemet.

  Du kan teste resultatet i Ubuntu:

  ```sh
  stud@stud-virtual-machine:~$ firefox 10.9.8.2
  ```

  Bemærk at du selv må refreshe browseren (F5) for at få opdateret temperaturen, mere intelligent er vores løsning desværre ikke ;-)

# Øvelse: Linux Kernel Modules (Rpi)

## Formål

Denne øvelse giver erfaring med at skrive, kompilere og indsætte kernemoduler. Kernemoduler er de "containere" i hvilke vi fremadrette vil lægge vore Linux Device Drivere. Forståelsen af disse er derfor grundlæggende for at kunne skrive device drivere. Som første trin i at kunne lave device drivers, koncentrerer vi os derfor om kerne moduler.

Du forventes med øvelsen at opfylde følgende mål:
| Mål | Delmål 1 | Delmål 2 | Delmål 3 |
|-------------------------------------------|---------------------------------------|---------------------------------------|-----------------------------|
|Kode et kernemodul | Skrive koden | Skrive Makefile | Bygge et modul |
|Anvende et kernemodul |Overføre | Indsætte/udtage |Observere kernel log |
|Bruge Git til versionsstyring|Forbinde til repository |Add/commit kode |Push/Pull til/fra server|

## Godkendelse

Denne opgave indgår ikke i de opgaver som skal godkendes for at gå til eksamen, men indholdet er alligevel vigtigt.

## Forberedelse

- Du skal have et funktionelt VM-Ware Image og en Rpi.

## Øvelses Steps

Første stop på vejen til at skrive en device driver, er at bygge et kerne modul og prøve at indsætte det i kernen. Dette er hvad den første del af øvelsen beskæftiger sig med. Den anden halvdel handler om at komme i gang med Git.

### Step 1: Byg Linux

Inden vi kan bygge et kernemodul, skal vi have bygget Linux, da vi herved får konfigureret Linux til det rigtige target. I løbet af byggeprocessen opdateres desuden et antal header filer, som er nødvendige når vi skal bygge kernemodulet.

a) Stil dig hen hvor Linux source koden ligger:

```sh
cd ~/sources/rpi-5.4.83
```

Bemærk at dette er source koden til den specifikke version af Linux som bruges i vores SD-kort image til target.

b) Ikke noget, spring videre til (c) ;-)

c) Konfigurer Linux til at bygge til Rpi:

```sh
make ARCH=arm CROSS_COMPILE=arm-poky-linux-gnueabi- rpizw_defconfig
```

Selve konfigurationsfilen finder du under linux.../arch/arm/configs/rpiwz*defconfig og du kan se/ændre den ved at køre "make menuconfig"
\_ARCH* fortæller compileren at den skal se i arch/arm folderen for CPU arkitekturspecifikke filer, _CROSS_COMPILE_ at den skal bruge den nævnte cross compiler (med -gcc eller -g++ til sidst)

d) Byg Linux:

For at kunne bygge kernemoduler og drivere skal du som minimum køre (1-2 min) :

```sh
make ARCH=arm CROSS_COMPILE=arm-poky-linux-gnueabi- modules_prepare
```

Hvis du gerne vil bygge hele Linux, skal du køre (40-50 min):

```sh
make ARCH=arm CROSS_COMPILE=arm-poky-linux-gnueabi- zImage modules
```

Processen kan evt speedes up ved at tilføje parameteren -j 3, hvor 3 i dette tilfælde er 1,5 x antallet af CPU kerner. VM-Ware skal dog også være konfigureret til at bruge flere kerner. Bygger man med _modules_prepare_ vil det give warnings når man bygger kernemoduler og indsætter dem pga et manglende versionsnummer på den tilhørende kerne, men det er ikke problematisk i kurset.

Det tager længere tid og kræver mere diskplads at bygge hele Linux. Vil du gerne rydde op efter dig kan du kalde _make distclean_ og _make clean_.

Når kompileringen er færdig kan Linux binær filen findes som linux.../arch/arm/boot/zImage. Denne fil kan principielt kopieres til den første partition på SD kortet, ''LABEL1'' for at benytte denne nyligt kompilerede version, men det er der ikke behov for i denne omgang.

Bemærk at kompileringen af kernemoduler kan være lidt besværlig første gang, men når først tingene er sat rigtig op vha. make filer, source tree mm, så kører det ret gnidningsløst.

#### Spørgsmål

- Hvordan vælger hvilken platform som vi ønsker at bygge Linux til?
- Hvorfor skal vi angive en specifik compiler?

### Step 2: Lav dit første Kernemodul

e) Skriv ”hello_world” kernemodulet: Lad dig inspirere kraftigt af kapitlet (se kapitel 2) i [Linux Device Drivers (LDD3)](http://lwn.net/Kernel/LDD3/). Dermed også sagt driveren skal kunne det samme som "hello_world" driveren, hverken mere eller mindre end driver der beskrives i LDD3.

f) Skriv en Makefile. Se slides igennem og kapitlet i bogen...

Savner I inspiration, så se makefilen i [HALs repository](https://gitlab.au.dk/au-ece-sw3hal/sw3hal-student/-/tree/master/makefile?ref_type=heads)

Vær opmærksom på at Makefiles (ligesom Python) anvender [TAB](https://en.wikipedia.org/wiki/Tab_key) til at markere undersektioner i koden, på samme måde som man i C/C++ anvender tuborg-klammer, {}. Efter hvert target laver man en TAB for at angive hvilke kommandoer som skal kaldes. Man kan sagtens lave flere linjer med kommandoer, de skal blot være indrykket med en TAB.

```makefile
#Makefile
target:
<TAB>echo "hello world"
<TAB>echo "hello another world"
```

I Makefile skal man angive hvor souce koden til Linux ligger (-C <path>). Dette skal vel at mærke være netop den version af Linux som ligger på target og denne skal på forhånd være konfigureret til det rigtige target og bygget. I jeres VMWare/Ubuntu er source træet lagt ind under /home/stud/sources/rpi-x-y-z, hvor x-y-z angiver den aktuelle version (kig med ls i folderen). Det er nødvendigt at bygge Linux, da der i processen oprettes en masse headerfiler mm. som benyttes i kompilerings- og linkningsprocesserne. Vores lokale kompilering bruger opsætningen fra det Linux source træ, som vi peger på. I makefilen, der er refereret ovenfor, skal I huske at sætte stien rigtigt. Bemærk konstruktionen og forhold jer til hvad der rent faktisk sker når I sætter en kompilering i gang. Det kan være en fordel at beskrive stepsne i prosaform for at få en bedre forståelse.

g) Kompilér programmet og kopiér det til target vha scp (dvs du skal bruge USB kablet)

```sh
scp hello.ko root@10.9.8.2:
```

Kan du ikke få forbindelse til target, så disconnect og connect dit "USB/Ethernet Gadget" VM-Ware under "Removable Devices"

Nu burde du have forbindelse, check med ping 10.9.8.2.

For at læse kerne beskeder, altså de som bliver outputtet med ”printk”, kan du benytte ”dmesg” kommandoen på target. Evt kan du åbne en ny terminal.

h) Indsæt modulet i kernen (insmod) og check med dmesg at det lykkedes.

i) Tag modulet ud igen (rmmod) og se at det også lykkedes.

### Step 3: Kom i gang med Git

På gitlab.au.dk skal du sammen med din gruppe oprette et repository. Der er følgende krav

- Der skal være adgang for alle gruppemedlemmer (dvs det skal være i en gruppe med disse medlemmer, eller de skal tilføjes projektet)
- Det skal være et private projekt
- Der det skal initialiseres med en README.md fil

Husk at have opsat en SSH public key på gitlab.au.dk som vist i videoen!

Husk at konfigurere Gitlab lokalt med brugernavn og email.

Når projektet er oprettet skal alle gruppemedlemmerne clone repositoriet:

```sh
git clone git@gitlab.au.dk:path/to/repo
```

Med 'ls' ser du den nye folder, eks. gruppe-22/

I denne folder vil det være en god idé at benytte en filstruktur á la:

```sh
gruppe-22/exercise-1/README.md
gruppe-22/exercise-1/my_script.sh
gruppe-22/exercise-2/README.md
gruppe-22/exercise-2/my_program.c
gruppe-22/exercise-3/README.md
gruppe-22/exercise-3/hello.c
gruppe-22/exercise-3/Makefile
```

osv.

Gruppens deltagere tager nu hver én opgave og lægger den ind i repositoriet. Dvs. Lars tager exercise-1, Benny exercise-2 og Hanna tager exercise-3 og lægger ind dem repositoriet på hver sin computer.

I træner herved git add, commit, push, pull (og måske merge).

Det vil også være en god idé at tilføje en .gitignore fil. I kan finde en .gitignore fil (.gitignore.kernelmodule) til kernemoduler i repositoriet. Husk at omdøbe filen til .gitignore og læg den i rodfolderen af repositoriet (ex. gruppe-22/)

Når I er færdige, bør alle 3 øvelser være lagt ind i repositoriet og kunne ses på Gitlab og hos alle gruppens deltagere.

# Øvelse: Linux Device Driver med GPIO (Rpi)

## Formål

Denne øvelse giver erfaring med at skrive, kompilere og indsætte en basal Linux Device Driver.

Du forventes med øvelsen at opfylde følgende mål:
| Mål | Delmål 1 | Delmål 2 | Delmål 3 |
|-------------------------------------------|---------------------------------------|---------------------------------------|-----------------------------|
|Initialisere af driver|init gpio interfaces | Init major/minor numre og cdev| Lave fejlhåndtering |
|Implementere filoperationer |Impl. og test open/release | Impl. read |Impl.write|
|Anvende driver|Indsætte driver og oprette major/minor numre |Anvende file operations |Anvende kernel log til debugging/test|

## Godkendelse

Besvarelser skal indeholde relevante kodeudsnit som der henvises til i forklarende tekst, tests og diskussion af resultater heraf.
Komplet kode inklusiv Makefiles lægges i repository.

## Forberedelse

- Du skal have et funktionelt VM-Ware Image.
- Du skal være i stand til at kunne kompilere et kerne modul, indsætte og fjerne det.
- Du skal have læst Linux Device Driver Development s.376-381: GPIO Subsystem (kun legacy, se bort fra interrupts).
- Du skal have en fungerende Makefile, som den I fandt frem til ved sidste opgave (eller fik udleveret).
- Du skal have en Raspberry Pi Zero, samt en ASE fHAT med komponenter monteret som vist på figuren herunder:

![ASE fHAT Board](../images/ase_fhat_board_ex1.png)

**BEMÆRK!** Der skal monteres et hunstik på undersiden af printet, således at printet kan monteres ovenpå Raspberry Pi.

## Øvelsen

Lav 2 drivere, der hhv. håndterer læsning og skrivning.

- Driver 1: Læsning fra devicet (/dev/sw2) returnerer hvorvidt SW2 er trykket ned eller ej.
- Driver 2: Skrivning til devicet (/dev/led3) tænder og slukker LED3.

NB! 2 Drivere medfører 2x makefile + 2x driver .c fil, navnene er desuden givet ovenfor. Placér hver driver i sin egen folder.

Vi anvender SW2 og LED3. Du kan finde deres GPIO numre i diagrammet, ligesom i tidligere øvelse.

Mht. brugen af gpio er det så smart indrettet, at der allerede er lavet understøttelse for processorens GPIO'er i Linux kernen. Der er med andre ord allerede implementeret en lav-niveau GPIO driver som skriver til GPIO_OE, GPIO_DATAIN og GPIO_DATAOUT registrene i processoren. Den eksisterende GPIO driver har et eksternt API som giver os mulighed for at tilgå gpio driverens funktionalitet fra andre steder i kernen. Vi benytter således dette API istedet for at skrive direkte til register adresserne. I en mindre udviklet kerne kunne det modsatte sagtens være tilfældet.

Metoderne til håndtering af gpio finder vi i "/home/stud/sources/rpi-5.4.83/include/linux/gpio.h". Se dokumentationen under "~/sources/rpi-5.4.83/Documentation/gpio.txt". Bemærk at det er yderst simpelt at håndtere gpio, vi skal blot angive gpio numrene anvendt af processoren for at specificere hvilken GPIO vi ønsker at arbejde på. GPIO nummeret er et positivt heltal 0-n.

Vigtige metoder allerede defineret i `gpio.h` (dvs du skal inkludere gpio.h og benytte disse):

```c
 /* request GPIO, returning 0 or negative errno.
 * non-null labels may be useful for diagnostics.
 */
 int gpio_request(unsigned gpio, const char *label);

 /* release previously-claimed GPIO */
 void gpio_free(unsigned gpio);

 /* set as input or output, returning 0 or negative errno */
 int gpio_direction_input(unsigned gpio);
 int gpio_direction_output(unsigned gpio, int default_value);

 /* GPIO INPUT: return zero or nonzero */
 int gpio_get_value(unsigned gpio);

 /* GPIO OUTPUT */
 void gpio_set_value(unsigned gpio, int value);
```

**Bemærk!** Parameteren `const char *label` læses som at her forventes et char array (C-streng), f.eks "Hello".

Include filer til jeres driver, så I kan komme i gang:

```c
 #include <linux/gpio.h
 #include <linux/fs.h>
 #include <linux/cdev.h>
 #include <linux/device.h>
 #include <linux/uaccess.h>
 #include <linux/module.h>
```

Derudover, når I leder efter en funktion i kernen (header filer samt andre drivere) bla. for at se dens signatur og evt. brugsmønster, så brug [Elixir Bootlin](https://elixir.bootlin.com/linux/latest/source). Ellers se slides / lektie, der vil I kunne finde alt det I søger. Alternativt, så er Google altid til tjeneste.

### GPIO LDD driverne i steps

Nedenfor er vist en overfladisk skabelon som kan bruges som inspiration når I skal lave de 2 drivere. Bemærk at driver 1 ikke har en `write()` funktion, da den kun bør kunne læse fra SW2. Den gpio som svarer til SW2 skal altså være sat til input, som er default. Derimod skal driver 2 have både en `write()` og en `read()` funktion. `write()` funktionen er naturligvis for at kunne tænde/slukke for LED3, mens `read()` er til for at kunne få at vide om den er tændt eller slukket. I det sidste tilfælde (LED3 driveren) skal gpio'en være et output.

**HUSK!** At udfylde `module_init`, `module_exit`, samt `licens`, `author` og `description`.

a) Implementer "`init`" og "`exit`" funktionerne. Det er her I skal requeste og free gpio’s, samt sætte gpio_direction til at styre pin’ens retning. Samtidigt er det afgørende at du/I laver en ordentlig fejlhåndtering i init().

```c
 static int mygpio_init(void)
 {
 // Request GPIO
 // Set GPIO direction (in or out)
 // Alloker Major/Minor
 // Class Create
 // Cdev Init
 // Add Cdev
 }
```

```c
 static void mygpio_exit(void)
 {
 // Delete Cdev
 // Unregister Device
 // Class Destroy
 // Free GPIO
 }
```

Test ved at bygge driveren og indsætte den. Du skulle gerne kunne finde det tildelte major nummer og tilhørende label i filen /proc/devices. Check desuden for fejl i dmesg.

b) Implementer `open` og `release`. Dvs den kode som eksekveres når en applikation forsøger at åbne/lukke et device (fil). Brug følgende funktioner som de er (De skriver en besked til kernel loggen om hhv open/release med hvilket major og minor nummer):

```c
 int mygpio_open(struct inode *inode, struct file *filep)
 {
 int major, minor;
 major = MAJOR(inode->i_rdev);
 minor = MINOR(inode->i_rdev);
 printk("Opening MyGpio Device [major], [minor]: %i, %i\n", major, minor);
 return 0;
 }

 int mygpio_release(struct inode *inode, struct file *filep)
 {
 int minor, major;

 major = MAJOR(inode->i_rdev);
 minor = MINOR(inode->i_rdev);
 printk("Closing/Releasing MyGpio Device [major], [minor]: %i, %i\n", major, minor);

 return 0;
 }
```

Husk at udfylde files_operation struct'en med open og release metoderne!!!

Test ved at bygge-, indsætte driveren, oprette en node. Ex her med major = 62 og minor = 0:

```bash
mknod /dev/mygpio c 62 0
```

Bruger I statisk allokering af major/minor kender I på forhånd disse numre. Bruger I derimod dynamisk allokering kan I finde disse /proc/devices. Prøv nu at åbne- og lukke node-filen. "cat" eller "echo" duer ikke her, da de vil forsøge at læse/skrive til filen og disse metoder er endnu ikke implementeret. Du kan ganske let lave en testapplikation som åbner og lukker en fil... (Alternativt kan du se på [exec 4</dev/mynode](http://unix.stackexchange.com/questions/131801/closing-a-file-descriptor-vs), [exec 4>&-](http://unix.stackexchange.com/questions/131801/closing-a-file-descriptor-vs)).

c) Implementer `read` (Driver 1 - læsning af SW1 + Driver 2 - Er LED3 tændt eller?). Dvs den kode som eksekveres i kernel-space når en user-space applikation forsøger at læse fra et device (fil).

```c
 ssize_t mygpio_read(struct file *filep, char __user *buf,
            size_t count, loff_t *f_pos)
 {
 // Hvilke trin er det der skal udføres?
 // Hint konvertering fra int til string kan gøres via sprintf() - Tekststrenge frem for binære værdier gør det nemmere at læse værdien i user-space med f.eks. cat.

 *f_pos += len;
 return len;
 }
```

Husk at udfylde files_operation struct'en med read metoden!!!

Test ved at bygge, scp og indsætte device driveren, samt oprette node. Prøv nu at læse fra noden, den læste værdi skal gerne afspejle tilstanden af trykknapperne. I kan bruge "cat" eller udvide jeres lille testapplikation. Du kan blive inspireret [her](https://redweb.ece.au.dk/devs/projects/devkit8000/wiki/PosixFileRead) og kompilere den med "arm-rpizw-gcc -o rd rd.c".

Bemærk at programmer som f.eks "cat" læser indtil de møder en End of File karakter (EOF). "cat" vil derfor læse uendeligt fra driveren og læsse bunkevis af data ud på terminalen.

I kan med fordel lave en lille script fil som indsætter modulet og opretter device nodes. Nodes behøves kun oprettet en gang (indtil I hiver strømmen).

d) Implementer `write` (Driver 2 - Skrivning til LED3 - altså om den er tændt eller ej.)

```c
 ssize_t mygpio_write(struct file *filep, const char __user *ubuf,
            size_t count, loff_t *f_pos)
 {
 // Hvilke trin er det der skal udføres?
 // Hint konvertering fra string til int kan gøres via sscanf() - antagelsen er at det er strenge der sendes til og fra user space.
 return count;
 }
```

Husk at udfylde files_operation struct'en med write metoden!!!

Test ved at bygge, scp og indsætte device driveren, samt oprette node. Skriv til noden for at tænde og slukke led'en.

e) Frivillig! Kombinér de to drivere til en enkelt, som anvender to minor numre.
De to drivers kan merges sammen til en enkelt. Det kræver blot at vi anvender to minor numre som repræsenterer hhv en LED og en trykknap. (ex mknod /dev/led2 c 62 0 ; mknod /dev/sw0 c 62 1) Dvs.

- I init skal begge GPIO'er requestes og antal devices sættes til minimum 2.
- I read() skal der læses fra SW2 _HVIS_ minor nummeret svarer til det som ex /dev/sw0 noden er oprettet med (Ex. 1).
- I write() skal der skrives fra LED3 _HVIS_ minor nummeret svarer til det som ex /dev/led2 noden er oprettet med (Ex. 0).
- I exit skal begge GPIO'er frigives.

I read()/write() kan minor nummeret udlæses vha:

```c
 int minor = iminor(filep->f_inode);
```

# Øvelse: Device Driver med Interrupts (Raspberry Pi)

## Formål

Formålet med denne øvelse er at opnå erfaring med at anvende eksterne interrupts i Linux og undersøge Linux's begrænsninger når det kommer til latenstid og behandling af realtidsinput. Øvelsen udbygger GPIO character driveren fra sidste øvelse, ved at indføre interrupts og gøre _read_ metoden blokerende. Man vil herved opleve at læsninger til device noden vil blive sat på pause, indtil at der sker et event på den tilhørende indgang.

Øvelsen har følgende mål:

|_.Mål |_.Delmål 1 |_.Delmål 2 |_.Delmål 3 |
|Initialisere driver|initialisere interrupt | implementere ISR med printk | Anvende kernel log og /proc information til debugging/test|
|Lave blokerende fops |Impl. blokerende read()| Implementere ISR|Teste blocking read på Rpi|
|Undersøge Rpi/Linux Responstid|Opdatér driver|Undersøge interrupt wake-up tid|Undersøge read wake-up tid|

## Godkendelse

Denne opgave indgår ikke i de opgaver som skal godkendes for at gå til eksamen, men indholdet er alligevel vigtigt.

- Besvarelser skal indeholde relevante kodeudsnit som der henvises til i forklarende tekst, tests og diskussion af resultater heraf.
- Komplet kode inklusiv Makefiles lægges i repository.

## Forberedelse

- Du skal have et funktionelt VM-Ware Image.
- Du skal have en fungerende GPIO device driver.
- Du kan evt. kopiere din GPIO device driver til et nyt passende navn og arbejde ud fra den.
- Du skal MEDBRINGE din Analog Discovery.
- Du skal have en Raspberry Pi Zero, samt en ASE fHAT med komponenter monteret som vist på figuren herunder:

![ASE fHAT Board](../images/ase_fhat_board_irq.png)

## Øvelsen

Øvelsen består af tre dele:

- Opdatér gpio driver til at anvende interrupts.
- Udvid driver, således at den kan bruges til at vurdere performance.
- Test grænserne for dit system.

I en tidligere øvelse lavede du to gpio drivere som kunne henholdsvis læse fra- og skrive til en GPIO port. Driverens funktioner var lavet således at de returnerede- eller satte GPIO værdien. Skulle man detektere en ændring på GPIO inputtet, måtte man læse mange gange, og i software beslutte om der var sket en ændring. Dette kaldes også polling.

Man har ofte behov for at reagere indenfor kort tid, når der sker et event. Dette kan kræve hyppig polling, hvilket sjældent er effektivt. I stedet kan man ønske at blive notificeret, når der sker et event. Til dette benytter vi interrupts. I Linux håndteres interrupts i kernen. Applikationer har ikke rettigheder til at køre kode i interrupt kontekst, det kan kun kernen! Applikationer kan derimod forespørge på data vha. blocking read/write og dermed blive lagt til hvile, indtil kernel siger at data er tilgængelig, typisk implementet vha en interruptfunktion.

Du skal tilføje følgende headerfiler (ifht sidste gang):

```c
#include <linux/interrupt.h>
#include <linux/wait.h>
#include <linux/sched.h>
```

Vigtige metoder fra _gpio.h_ (kan findes under [gpio.h](http://git.kernel.org/?p=linux/kernel/git/stable/linux-stable.git;a=blob;f=include/linux/gpio.h), _tal refererer til linjenumre i header fil_):

```c
/* Get interrupt line attached to GPIO port number */
79  unsigned int gpio_to_irq(unsigned int gpio);
```

Vigtige metoder fra _interrupt.h_ (kan findes under [interrupt.h](http://git.kernel.org/?p=linux/kernel/git/stable/linux-stable.git;a=blob;f=include/linux/interrupt.h)):

```c
/* Request IRQ (Return value must be checked!) */
143 static inline int __must_check
144 request_irq(unsigned int irq, irq_handler_t handler, unsigned long flags,
145 const char *name, void *dev)

/* Free IRQ */
167 void free_irq(unsigned int, void *dev);

/* ISR funktions prototype */
92 typedef irqreturn_t (*irq_handler_t)(int, void *);
/* Example: irqreturn_t my_irq_handler(int irq_number, void *dev_data); */
```

Vigtige konstanter fra [interrupt.h](http://git.kernel.org/?p=linux/kernel/git/stable/linux-stable.git;a=blob;f=include/linux/interrupt.h):

```c
22 /*
23 * These correspond to the IORESOURCE_IRQ_* defines in
24 * linux/ioport.h to select the interrupt line behaviour. When
25 * requesting an interrupt without specifying a IRQF_TRIGGER, the
26 * setting should be assumed to be "as already configured", which
27 * may be as per machine or firmware initialisation.
28 */
29 #define IRQF_TRIGGER_NONE 0x00000000
30 #define IRQF_TRIGGER_RISING 0x00000001
31 #define IRQF_TRIGGER_FALLING 0x00000002
32 #define IRQF_TRIGGER_HIGH 0x00000004
33 #define IRQF_TRIGGER_LOW 0x00000008
```

BEMÆRK!!!!! Bogen nævner flaget _SA_INTERRUPT_, dette er ikke understøttet længere! Ovenstående anvendes i stedet for ifb. med _request_irq()_.

Vigtige metoder fra _wait.h_ (kan findes under [wait.h](http://git.kernel.org/?p=linux/kernel/git/stable/linux-stable.git;a=blob;f=include/linux/wait.h)):

```c
/* Declare 'wait queue' to be used: */
58 DECLARE_WAIT_QUEUE_HEAD(name);

/* Wake-up wait queue */
201 wake_up_interruptible(wait_queue_head_t *queue);

427 /**
428 * wait_event_interruptible - sleep until a condition gets true
441 */
442 wait_event_interruptible(queue, condition);
```

Bemærk at mange af funktionerne i wait.h er defineret som "preprocessor macroer":http://www.cplusplus.com/doc/tutorial/preprocessor/ . Macro funktioner udføres så vidt muligt pre-compile-time og inlines i koden inden den bliver kompileret. Koden kan derved eksekveres hurtigere run-time, hvilket er særdeles vigtigt når det handler om interrupts og vækning af tråde og processer.

### Implementering af interrupts i driver

Du skal nu tilpasse sw2 driveren således at den understøtter blokerende læsning. Driveren skal benytte en ISR til at vække ”read” metoden når SW2 enten trykkes ned eller slippes.

a) Request interruptlinje og implementer Interrupt handler.
Start med at deklarere en isr rutine øverst i modulet ex:

```c
static irqreturn_t mygpio_isr(int irq, void *dev_id) {
  return IRQ_HANDLED;
}
```

Lav derefter _request_irq()_ i _init_ metoden og _free_irq()_ i _exit_ metoden. Se øverst på denne side hvilke flag _request_irq()_ kan kaldes med.
Hint! _gpio_to_irq()_ samt _request_irq()_ kan først kaldes når den GPIO der repræsenterer SW2 er requested (_gpio_request()_) og sat til input. Benyt _printk()_ til at udskrive den tildelte IRQ linie. Kompilér din driver og indsæt modulet i kernen.
_Hvilken interrupt linje er driveren blevet tildelt? Er det det samme hver gang?_

b) Opdatér din interrupt service routine (se LDD3 side 270 (udelad dog den sidste parameter som vist nedenfor)).

```c
static irqreturn_t mygpio_isr(int irq, void *dev_id) {
// YOUR CODE HERE
return IRQ_HANDLED;
}
```

Routinen skal i denne delopgave blot indeholde en printk("IRQ event at irq line: %i\n", irq); således at vi kan se at den faktisk bliver kaldt. Pil det gamle modul ud og indsæt det nye i kernen. Tryk og slip knappen og se om det kommer ud. Brug evt. dmesg til se udskriften. Kommer der et enkelt event når vi trykker på knappen? Hvad sker der hvis vi ændrer på flagene i request_irq()?

Bemærk det at benytte printk() i en ISR IKKE er smart og er normalt ikke noget der må gøres! Vi vælger alligvel at gøre det her da det er den letteste og hurtigste måde at se om det vi har lavet rent faktisk virker. Derfor skal den også fjernes så snart I har overbevist jer selv om at jeres ISR routine virker. Men hvorfor bør man ikke have en _printk() i sin ISR?_

c) Implementér read() og opdater mygpio_isr() . Vi kan genbruge read() koden fra sidst, blot skal vi vente indtil at data er klar, dvs at ISR er blevet kaldt. Her skulle det måske overvejes at benytte en wait_event_int… på et strategisk sted. Se eksemplet fra LDD3 side 150.

I ISR routinen skal vi bruge modparten for at vække read() metoden op af sin dybe søvn. Kig igen på eksemplet i LDD3 side 150. Husk at vi skal vække i ISR rutinen.

Generelle overvejelser

    I opgaven placeres request_irq() i init() . Er det smart eller kunne den med fordel placeres i open() - Under hvilke forudsætning bør/kan den ligge hhv. det ene og det andet sted?
    Hvad betyder det om tilstanden af gpio’en læses i hhv. ISR eller i read?

## Udvid driver til performance måling

Vi vil nu undersøge hvor hurtigt man kan kalde ISR funktionen efter hinanden og tilsvarende for read funktionen. ISR kører i interrupt kontekst, hvor intet andet på systemet kører, med read funktionen sover i proces kontekst og først skal vågnes. Vi forventer at der er forskel, men hvor er grænsen for vores Raspberry Pi? Vi vil undersøge dette påtrykke et signal med en given pulsbredde med vore Analog Discovery Waveform Generator og se hvad vi detekterer i hhv. ISR og i read funktioner.

d) Ændr driverens GPIO port og test med Analog Discover.
Opdatér din sw2 driver til at anvende GPIO19 i stedet for SW2's GPIO port. Konfigurér Analog Discovery's Waveform generator til at lave et firkant signal på 1 Hz med et signaludsving på 0-3 volt. BEMÆRK at som default kører den +/- spænding ud, så du skal justere offset!! Check med Oscilloscope indgangen!!! Når spændingen ser rigtig ud, deaktiverer du waveform udgangen og forbinder hhv. ground-ground og output til GPIO19 på ASE fHat.

![Analog Discovery](../images/ase_rpi_fhat_analog_discovery_wfm.jpg)

Start waveform generatoren og test driveren ved at læse fra driver noden med cat . Når det er verificeret at der er hul igennem med den gamle driver, kan vi fortsætte.

e) Opdatér read til at returnere værdier fra både ISR og read() til måling af tider
Tilføj to globale int variabler, isr_gpio_value og proc_gpio_value. isr_gpio_value skal tildeles en værdi vha. gpio_get_value() i ISR og tilsvarende for proc_gpio_value i driverens read funktion.
Readfunktionen opdateres til at returnere en streng bestående af læseværdier fra read() og ISR. Formattering kan laves såldes:

len = sprintf(buf, "%i %i", isr_gpio_value, proc_gpio_value);

Se figuren nedenfor illusterer hvordan det skal virke (Bemærk: vi kommer ikke til at bruge proc_cnt og isr_cnt i denne omgang).

![Sequence Diagram](../images/Hal_ex_irq_isr_seq_diagram.png)

Du kan nu teste det indsatte module vha. den oprettede node. I første omgang ved at catte noden (her kaldet gpio19) ex:

```
cat /dev/gpio19
  1 1
  1 1
```

Dette skulle gerne spytte de to værdier ud.

Vi vil nu prøve at undersøge hvor lang tid det tager at vække read funktionen. Hypotesen er, at hvis vi påtrykker en puls som er smallere (i tid), end den tid som det tager at aktivere ISR funktionen, vække read funktionen og siden læse tilstanden af gpio porten, så vil porten læses som '0', selvom det oprindelig var en positivtgående puls vi triggede på. Hvis pulsen omvendt har en bredde længere end tiden det tager fra aktivering af interrupt, indtil læsning i read, så vil den læses som '1'. Dvs. ved at undersøge hvad værdi som porten læses som ved en given pulsbredde, vil være udtryk for tiden som det tager at aktivere ISR, vågne op og siden læse gpio porten. Det antages at læsning af gpio port med gpio_get_value() går meget hurtigt i forhold til skedulering, da det alene er en læsning fra et register. Registerlæsning ~ ex 10 clock-cycles @ 800MHz = 13 ns. << skedulering (ms). Læser vi desuden gpio værdien i ISR, kan vi også få en idé om opvågningstiden for ISR rutinen.

f) Undersøg wake-up tiderne ved at teste med en variabel pulsbredde fra Analog Discovery. Vælg Waveform Generator, square, og start med en frekvens på 5 Hz og 50% duty-cycle. Pulsbredden justeres ved at justere duty-cycle. (Obs! Du kan tjekke pulsbredden vha. Oscilloscope indgangen på Analog Discovery, hvis du forbinder den til Waveform Generators udgang).
Opret en node og test ved at catte noden. Prøv nu at justere pulsbredden på Analog Discovery's waveform generator indtil gpio værdierne begynder at afvige. På den måde kan du ud fra pulsbredden estimere hhv. interrupt og proces wake-up tider.

```
cat /dev/gpio19
  1 1
  1 0
  0 0
```

Hvor lang tid tager det fra vi har læst gpio i interrupt, indtil read er vågnet og gpio er læst i read?
Hvor lang tid er ISR om at vågne op?

g) (frivillig) Prøv at anvende et threaded interrupt, hvor du læser proc_gpio i bottom half af interruptet. Dvs du skal gøre følgende (se evt slide omkring Threaded interrupts):

    Oprette en bottom-half (process) interrupt handler, hvori proc_gpio sættes og wake_up kaldes.
    Ændre request_irq() til request_threaded_irq() og angive den nye process isr
    Rette returværdi i top-half isr

Hvad bliver wake-up tiden i process irq? Er det hurtigere end da vi læste i read()?

# Øvelse: Hot-pluggable Device Driver (Raspberry Pi)

Med udgangspunkt i kodeskabelonen til en platform driver, `plat_drv`, skal du undersøge hvordan hot-plugging virker i Linux kontekst og bagefter udvide skabelonen til en GPIO driver.

## Formål

Formålet med denne øvelse er at opnå erfaring med at anvende Linux' device model. Herunder at opnå forståelse for anvendelsen af Hot- og Cold-plugging og hvordan dette implementeres i en device driver.

Følgende Rubric beskriver forskellig opfyldelse af øvelsens mål:

| Mål                               | Mangelfuld opfyldelse af mål                                                                                                                                        | Delvis opfyldelse af mål 2                                                                                                                                                                                            | Fuld opfyldelse af mål                                                                                                                                                                                                                                                               |
| --------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Undersøge Hot-plugging            | <ul><li> Driverens init/probe er implementeret </li><li> Driver kan indsættes og device treee loades.</li><li>Der ses IKKE en besked fra probe() i dmesg.</li></ul> | <ul><li> Driverens init/probe er implementeret </li><li> Driver kan indsættes og device treee loades </li><li> Der ses en besked fra probe() i dmesg. </li><li> Der er vist output fra test</li></ul>                 | <ul><li> Driverens init/probe er implementeret </li><li> Driver kan indsættes og device treee loades </li><li> Der ses en besked fra probe() i dmesg. </li><li> Der er vist output fra test </li><li> Driver/device binding er diskuteret </li></ul>                                 |
| Implementere gpio hot-plug driver | <ul><li> Der er implementeret basal oprettelse og konfiguration af gpios i probe </li><li> Device oprettes i probe().</li></ul>                                     | <ul><li> Der er implementeret basal oprettelse og konfiguration af gpios i probe </li><li> Device oprettes i probe(). </li><li> Read og write funktioner er opdateret til at kunne tilgå de korrekte gpios.</li></ul> | <ul><li> Der er implementeret basal oprettelse og konfiguration af gpios i probe </li><li> Device oprettes i probe(). </li><li> Read og write funktioner er opdateret til at kunne tilgå de korrekte gpios. </li><li> Der er lavet korrekt cleanup i hhv. exit og remove. </li></ul> |
| Anvende Device Tree               | <ul><li> Overlay (.dts) er opdateret med GPIOs                                                                                                                      | <ul><li> Overlay er opdateret med GPIOs </li><li> overlay data bliver læst i probe() </li></ul>                                                                                                                       | <ul><li> Overlay er opdateret med GPIOs </li><li> overlay data bliver læst i probe() </li><li> gpios oprettes korrekt i /dev </li><li> gpios fungerer korrekt </li><li> tests er dokumentet </li></ul>                                                                               |

## Godkendelse

Besvarelser skal indeholde relevante kodeudsnit som der henvises til i forklarende tekst, tests og diskussion af resultater heraf. Komplet kode inklusiv Makefiles lægges i repository.

## Forberedelse

- Du skal have et funktionelt VM-Ware Image.
- Du skal have en fungerende GPIO device driver (den fra øvelse 4).
- Have set videoer på Black Board som gennemgår platform driver og device tree.
- Du skal have en Raspberry Pi Zero, samt en ASE fHAT med komponenter monteret som vist på figuren herunder:

![ASE fHAT Board](../images/ase_fhat_board_irq.png)

## Delopgave 1: Hot-Plugging af Platform Driver og Device

Målet med denne delopgave er at du opnår forståelse af principperne bag Linux' device model, i dette tilfælde hvordan hot-plug virker og hvilke funktioner som kaldes hvor og hvornår.

a) Kopier din GPIO driver fra øvelse 4 til en ny folder, kopiér desuden Makefile og `plat_drv_overlay.dts` fra repositoriet hertil. Omdøb din driver .c fil til `plat_drv.c`. Byg koden vha. den tilhørende Makefile. Bemærk at resultatet er to filer: kernemodulet `plat_drv.ko` og et device tree blob overlay, `plat_drv.dtbo`. Den sidste beskriver hardwaren og er noget vi vil dykke ned i senere.

b) Tilføj hot-plugging funktionalitet ved at oprette `probe()` og `remove()` funktionerne i driveren (frie funktioner ligesom `init()`, `read()` mm.), lav en enkelt `printk()` i hver af dem ("hello from probe\n"...). Tilføj hot-plugging identificering ved at oprette `of_device_id` og `platform_driver` struct'ene. Sæt `compatible = "au-ece, plat_drv"`, som er navnet brugt i device tree filen, `plat_drv-overlay.dts`. Registrer platform driveren i init (som det sidste) vha. `platform_driver_register()`. Byg og kopiér .ko og .dtbo filer til target.

_Hvilken headerfil skal du inkludere for at benytte "platform_driver_register": [platform_driver_register](https://elixir.bootlin.com/linux/latest/A/ident/platform_driver_register)?_

c) Du skal nu registrere device't i Linux og indsætte driveren. Du registrerer device't ved at indlæse .dtbo filen på følgende vis:

```sh
dtoverlay -d . plat_drv.dtbo
```

Dette svarer til når man laver hot-plugging med USB. Det overordnede RaspberryPi ZW device tree udvides herved til også at indeholde vores device. `-d .` gør at den loader .dtbo filen fra vores lokale sti, ellers ville den forsøge at loade den fra `/boot/overlays`. Du skal loade driveren på sædvanlig vis.

_Spørgsmål: Er driveren tildelt major nummer (se `/proc/devices`), er der lavet en Sys FS klasse (se `/sys/class`)? Hvilke metoder er blevet kaldt i device driveren (`plat_drv.c`)? Hvad sker der hvis vi fjerner overlayet igen (`dtoverlay -R plat_drv`)? Vis resultatet af `dmesg` og forklar sammenhængen til det observerede med koden i `plat_drv.c` og sekvensdiagrammet i lektionens slides. Benyt gerne dit eget sekvensdiagram med rigtige funktionsnavne._

d) I `probe()` skal nu oprettes et device vha `device_create()`. I `remove()` tilføjes `device_destroy()`. Byg og test på target.

_Spørgsmål: Er der oprettet Sys FS devices (se `/sys/class/<my_class>/`)? Er der oprettet noder (`/dev`)? Og virker read/write metoderne?_

## Delopgave 2: Konstruktion af egen hot-pluggable GPIO device driver

Nu hvor du har prøvet at anvende den device modellens overordnede mekanik, er det på tide at du anvender den til at forbedre din GPIO driver. Din driver skal automatisk oprette 2 noder under `/dev/` når device tree filen loades og fjerne dem igen, når den fjernes.

Før vi går løs kan man med fordel lave en lille struct i driveren til at gemme info om GPIO port numre og retninger, samt lave et array af denne som vi senere kan bruge til at mappe minor numre og GPIOs, ex: GPIO12 er input og skal styres vha minor 0:

```c
struct gpio_dev {
  int no;   // GPIO number
  int dir; // 0: in, 1: out
};
static struct gpio_dev gpio_devs[2] = {{12,0}, {21, 1}};
static int gpios_len = 2;
```

Vi har tidligere anvendt `request_gpio()`, `gpio_direction...()` i `init()`. Vi ønsker at allokere GPIO'er dynamisk og bruge GPIO numre og retninger givet i device tree. `request_gpio()` mm skal derfor flyttes til `probe()`, da denne kaldes når der er detekteret et gyldigt device (ex. `plat_drv` i device tree).

a) Flyt `request_gpio()`, `gpio_direction...()` kaldende til `probe()`. Loop igennem `gpios_devs` arrayet og brug informationerne til at sætte parametre (GPIO nummer) og funktionskald (`gpio_direction_in` / `out`). Lav et kald til `device_create()` for hver GPIO og brug loop variablen til at sætte minor numret med. Dvs hver GPIO får hver sit minor nummer:

```c
[request_gpio() -loop antal]
[gpio_set_direction() -loop antal]
[opret devices med device_create -loop antal]
```

Byg og test på target.

_Spørgsmål: Oprettes der Sys FS devices? Oprettes der devices i `/dev/`? Hvilke GPIO'er virker read/write funktionerne på?_

b) Vi skal have gjort read/write funktionerne generelle, så de duer på de GPIO'er vi har i vores struct. Dvs. opdatér read/write funktioner til at anvende `gpio_devs` struct'en. Husk at vi har adgang til minor nummeret i read/write vha:

```c
size_t plat_drv_read(struct file *filep, char __user *ubuf,
                      size_t count, loff_t *f_pos)
{
  int minor;
  minor = iminor(filep->f_inode);

  [gpio_get_value(gpio_devs[.......) ]
}
```

Byg og test at det virker på de GPIO'er som du har angivet i `gpios_devs`. Dokumentér med output af `dmesg` og resultater af dine tests og forklar hvordan de afspejler din implementering.

## Delopgave 3: Anvendelse af Device Tree i driver

Vi vil nu udvide vores device tree fil, `.dts`, til at indeholde en beskrivelse af hvilke GPIO'er vores driver skal bruge. Man kan på denne måde ændre GPIO'erne uden at skulle ændre driveren efterfølgende. Du bør have set lektionen og eksempler på how device tree overlays virker. Der er nogle WIKI sider og værktøjer til at oprette Device Tree Overlays som du kan bruge som hjælp.

a) Opdater `plat_drv-overlay.dts` til at inkludere GPIO pins. Brug de korrekte gpio numre og hhv '0' for input og '1' for output. I nedenstående eksempel er anvendt GPIO 55 som input og GPIO 56 som output:

```dts
/dts-v1/;
/plugin/;
/ {
    compatible = "brcm,bcm2835";

    fragment@0 {
        target-path = "/"; /* Add new virtual device to base path */
        __overlay__ {
            mydevt: mydevt {/* devtree label:instance name */
                /* Label to match in driver */
                compatible = "my_device_name";

                /* Configure GPIO module */
                /* <ressource pinno dir pinno dir pinno dir ....*/
                gpios = <&gpio 55 0>, <&gpio 56 1>;

                /* Custom Property */
                /* <32-bit int value*/
                mydevt-custom = <0x12345678>;
                status = "okay"; /* Device is enabled */
            };
        };
    };
};
```

Dts filen bygges sammen med kernemodulet, når du bruger Makefile'n med `make`. Bemærk at source filen `plat_drv-overlay.dts` bygges til filen `plat_drv.dtbo`. Omdøbningen sker i Makefile'n og skyldes navnekonventioner som anvendes ifb. med Device Tree Overlays.

b) Anvend Device Tree parametre i device driveren

Anvend `of` (Open Firmware) funktionerne i `probe()` til at udlæse GPIO numre og retning (in/out) fra Device Tree. Værdierne kan med fordel tilskrives elementerne i `gpio_devs` struct'en fra tidligere og efterfølgende bruges til initialisering af GPIO'er og oprettelse af devices (som I sådan set allerede er implementeret...).

Vigtige funktioner fra [of_gpio.h](http://elixir.free-electrons.com/linux/v4.0/source/include/linux/of_gpio.h):

```c
 static inline int of_gpio_count(struct device_node *np)
 static inline int of_get_gpio(struct device_node *np, int index)
 static inline int of_get_gpio_flags(struct device_node *np, int index,
                      enum of_gpio_flags *flags)
```

`of_get_gpio()` returnerer GPIO nummer eller en værdi <0 ved fejl. `of_get_gpio_flags()` returnerer flagets værdi by reference via flags parameteren. Returværdien er det tilhørende GPIO nummer eller en værdi <0 ved fejl.

I driveren skal du opdatere `probe()` noget i stil med:

```c
#include <linux/err.h>
#include <linux/of_gpio.h>
#include <linux/platform_device.h>

/* GPIO Device Data Struct */
struct gpio_dev {
  int gpio;
  int flag;
};

static struct gpio_dev gpio_devs[255];
int gpio_devs_cnt = 0;

static int plat_drv_probe(struct platform_device *pdev)
{
  int err = 0;
  struct device *dev = &pdev->dev; // Device ptr derived from current platform_device
  struct device_node *np = dev->of_node; // Device tree node ptr
  enum of_gpio_flags flag;
  int gpios_in_dt = 0;

  /* Retrieve number of GPIOs */
  [hent antal af gpioer (of_gpio_count)]

  /* Loop through gpios in Device Tree */
  for (int i=0;i<gpios_in_dt ; i++) {

    /* hent gpio nummer (of_get_gpio) og skriv i struct */

    /* Hent gpio flag, dvs retning (of_get_gpio_flags)  og skriv i struct */
  }

  for (int i=0;i<gpios_in_dt ; i++) {

    /* request_gpio[gpio_devs[i].no]  */

    /* Sæt gpio direction iht .dir i struct */

    /* Opret devices med device_create() */

    printk(KERN_ALERT "Using GPIO[%i], flag:%i on major:%i, minor:%i\n",
             gpio_devs[gpio_devs_cnt].gpio, gpio_devs[gpio_devs_cnt].flag,
             MY_DRIVERS_major_number, gt_devs_cnt);
   }
}
```

_Du kan finde inspiration i `spi-oc-tiny.c` driveren som findes på jeres VMware image under `/home/stud/sources/rpi-5.4.51/drivers/spi/spi-oc-tiny.c`. Se funktionerne `tiny_spi_probe()` og `tiny_spi_of_probe()` (som kaldes i `tiny_spi_probe()`)_

## Delopgave 4: Automatisk load af Device Tree Overlay og driver modul

For at loade device og driver automatisk under boot skal du gøre følgende 5 ting (ex: `mit_modul.ko`, `mit_overlay.dtbo`):

1. Kopiere `mit_modul.ko` til `/lib/modules/<linux-version>/`
2. Registrere modulet med `depmod -a`
3. Tilføje en entry i `/etc/modules_load.d/` for at loade det automatisk under boot
4. Kopiere `mit_overlay.dtbo` til `/boot/overlays/`
5. Lave entry i `/boot/config.txt` for at loade device definition under boot

1) Kernemoduler ligger som standard under `/lib/modules/den_version_af_linux_som_er_i_brug/` (Version kan udlæses med `uname -r`). Vil du gøre dit kernemodul tilgængelig for systemet, skal du kopiere din .ko fil hertil.

2) Kør `depmod -a`, som undersøger modul dependencies og laver et load script. Modulet vil herefter kunne loades med `modprobe mit_modul`.

3) Linux skanner filerne under `/etc/modules-load.d/` under upstart for at finde ud af hvilke moduler som skal loades. Du skal derfor oprette en fil her, som indeholder navnet på modulet som skal loades:

```sh
echo mit_modul /etc/modules-load.d/mit_modul.conf
```

4. Kopiér dit overlay, `mit_overlay.dtbo`, til `/boot/overlays` på din Rpi. Dette er default søgesti for overlays.

5. Lav en backup af din `config.txt` fil ved at kopiere den til et nyt navn:

```sh
cp /boot/config.txt /boot/config.txt.backup
```

6. Lav en entry i `/boot/config.txt` med navnet på overlayet, herved vil Linux søge efter det i `/boot/overlays` og indlæse det under boot. Brug en editor, f.eks. `nano` til at editere `/boot/config.txt`:

```sh
## Angiv overlay (uden .dtbo og ingen mellemrum omkring =)
dtoverlay=mit_overlay
```

_BRUG IKKE `echo bla bla /boot/config.txt` da det overskriver den eksisterende `config.txt` og din RPI vil ikke længere kunne boote. Du skal I så fald have hentet en `config.txt` ned fra en medstuderende og kopieret den ind på kortet eller kopiere din backup over i din `config.txt`. Du skal i begge tilfælde sætte dit SD kort i en kortlæser og mounte dem i Linux. `config.txt` ligger på FAT partitionen._

Nu er både kernemodul og device tree blob registreret og disse vil automatisk blive loadet når Linux booter. Hvis der er match i "compatible" navnene i hhv. device tree og driver, vil der blive oprettet devices under `/dev/` og entries i sysfs. Du kan checke om dit overlay bruger det rigtige navn ved at udlæse `/sys/firmware/devicetree/instans_navn_i_mit_overlay/compatible`.

Læs mere om loading/unloading af moduler på [ArchLinux-Kernel Modules](https://wiki.archlinux.org/index.php/kernel_modules).

# Øvelse: Linux Device Driver with SPI (Rpi)

Med udgangspunkt i kodeskabelonen til en SPI driver, spi_drv, skal du undersøge hvordan man implementerer en device driver til et device som anvender en bus.

## Formål

Formålet med denne øvelse er at få erfaring med at anvende bus interfaces i en device driver. Øvelsen tager udgangspunkt i et SPI bus interface og øvelsen vil sammenkæde viden omkring SPI interface, Linux Device Model og Device Trees.

Du forventes med øvelsen at opfylde følgende mål:

| **Mål**                      | **Delmål 1**                             | **Delmål 2**                                         | **Delmål 3**                                    |
| ---------------------------- | ---------------------------------------- | ---------------------------------------------------- | ----------------------------------------------- |
| Lave .dts                    | Finde bus no og chip select i diagrammer | Finde SPI mode mm i datablad                         | Implementere dts og loade det automatisk på Rpi |
| Opdatering af initialisering | Review af init()/exit()/probe()/remove() | Opdatering af probe til at understøtte flere kanaler | Teste probe/tildeling af SPI nummer på Rpi      |
| Lave SPI kommunikation       | Undersøge devicet's protokol             | Implementere SPI læse-/skrive funktioner             | Teste på Rpi                                    |

## Godkendelse

I bestemmer selv om I vil aflevere øvelse 7 eller 8. Deadline er som angivet i lektionsplanen på Brightspace. Såfremt I afleverer denne, skal Besvarelsen indeholde relevante kodeudsnit som der henvises til i forklarende tekst, tests og diskussion af resultater heraf. Komplet kode inklusiv Makefiles lægges i repository.

## Forberedelse

- Du skal have et funktionelt VM-Ware Image, Rpi Zero og ASE fHAT.
- Skim [Wikipedea's afsnit om SPI](http://en.wikipedia.org/wiki/Serial_Peripheral_Interface_Bus)
- Skim SPI dokumentationen i kernel tree ([spi-summary](http://www.kernel.org/doc/Documentation/spi/spi-summary))
- Skim spi.h igennem ([spi.h](http://git.kernel.org/?p=linux/kernel/git/stable/linux-stable.git;a=blob;f=include/linux/spi/spi.h))
- Skim diagrammet til [ASE fHAT](../images/ase_fhat_schematic.pdf) og [Raspberry Pi Zero W](../images/RPI-ZERO-V1_3_reduced.pdf)
- Skim [MCP4802 Datablad](http://ww1.microchip.com/downloads/en/DeviceDoc/20002249B.pdf)
- Skim [MCP3202 Datablad](http://ww1.microchip.com/downloads/en/DeviceDoc/21034F.pdf)
- Skim [BCM2835 Peripherals](https://www.raspberrypi.org/app/uploads/2012/02/BCM2835-ARM-Peripherals.pdf) (RPI processor datablad) 102-104 om pinmux og evt. 160 ff. om SPI

## Øvelsen

Med udgangspunkt i en SPI driver template skal der implementeres en ny driver til et SPI device. Dette device kan være A/D konverteren MCP3202 (U3), D/A konverteren MCP4802 (U1) eller en PSOC med et SPI interface kørende.

Opgaverne er følgende:

- [Øvelse: Linux Device Driver with SPI (Rpi)](#øvelse-linux-device-driver-with-spi-rpi)
  - [Formål](#formål)
  - [Godkendelse](#godkendelse)
  - [Forberedelse](#forberedelse)
  - [Øvelsen](#øvelsen)
    - [Hardware](#hardware)
    - [Implementering af Device Tree Overlay](#implementering-af-device-tree-overlay)
  - [Implementering af Device Driver](#implementering-af-device-driver)
    - [Kodereview af template SPI driver](#kodereview-af-template-spi-driver)
    - [Tilpasning af probe() / remove()](#tilpasning-af-probe--remove)
    - [Implementering af Read/Write](#implementering-af-readwrite)
    - [Test](#test)

### Hardware

Alt efter hvilket device du ønsker at kommunikere med skal du have monteret nogle komponenter inden du sætter strøm på systemet.

![ASE fHAT Board with SPI](../images/ase_fhat_board_ex_spi.png)

- A/D Converter (MCP3202): U3 + R6 + C1 + C2 +J4 +J5
- D/A Converter (MCP4802): U1 + R7 + C3 + C4 +J4 +J5
- PSoC: Kun J4. PSoC kan kun bruge SS1. Læs mere om Rpi-PSoC forbindelse på [Rpizw Wikien](https://redweb.ece.au.dk/devs/projects/raspberry-zero/wiki/ASE_Extension_Module) og i denne [guide](https://redweb.ece.au.dk/devs/projects/raspberry-zero/wiki/Connecting_RpiASE_fHat_to_PSoC)

Pins TÅLER MAX 3.3V!!! PSoC er som default 5V!!

## Implementering af Device Tree Overlay

Der foreligger allerede en device tree source fil, _spi_drv-overlay.dts_ som I kan tage udgangspunkt i.

Ud fra diagrammer og datablade skal du udlede følgende information for det device som du skal lave en driver til:

- SPI bus nummer (Diagram evt. støttet af datablad, i RPi tilfælde er dokumentationen mindre god, men kun bus 0 er tilgængelig på HAT)
- SPI chip-select (ss) nummer (diagram)
- SPI max frekvens (datablad)
- SPI Clock Mode (datablad). Bemærk at `spi-cpha` og `spi-cpol` i .dts er flag. Hvis udkommenteret betyder det at de er sat til nul.

For at finde bus nummeret skal du følge SPI signalerne fra [ASE-fHat diagrammet](../images/ase_fhat_schematic.pdf) ned på [Raspberry Pi Zero W diagrammet](../images/RPI-ZERO-V1_3_reduced.pdf), finde gpio nummeret (andre steder ville man bruge pin nummeret) og slå op i [BCM2835 Reference Manual](https://www.raspberrypi.org/app/uploads/2012/02/BCM2835-ARM-Peripherals.pdf) side 102 og finde GPIO pin'ens alternative funktioner, her finder du bus nummeret som et postfix på formen SPI<bus nummer>\_CE<chip select nummer>\_N.

Værdier og hvordan du har fundet dem, skal selvfølgelig med i din besvarelse.

- For MCP4802 skal du være opmærksom på følgende: De analoge udgange opdateres ved at en GPIO pin på kredsen går fra høj->lav->høj. Denne pin er forbundet til en GPIO på ASE-Fhat printet (se diagram og datablad) og denne GPIO bør derfor også angives i DT

- For MCP3202 skal du være opmærksom på følgende: Intet udover at den bruger SPI.

- For PSoC skal du være opmærksom på følgende: Hvis du gerne vil kunne interrupte RPI fra PSoC, så skal du angive et GPIO input som du vil bruge til interrupt på Rpi.

Opdatér .dts filen med de rigtige parametre for dit device

## Implementering af Device Driver

Implementeringen tager udgangspunkt i en kodetemplate som minder utrolig meget om platform driver koden I har arbejdet med. Fremgangsmåden er beskrevet trinvist for at hjælpe lidt på vej.

### Kodereview af template SPI driver

Gå koden i spi_drv.c igennem og observér forskelle og ligheder i forhold til jeres platform driver, plat_drv.c. Det kan være en idé at åbne dem begge i en editor (Code, Emacs mm) og lave en diff, så træder forskellene frem.

Læg mærke til at driveren registreres som en spi_driver i stedet for en platform_driver i init funktionen. Spi driver struct'en indeholder også et .bus element i modsætning til platform driveren.

Som med platform driveren, er det i `probe()` at vi laver den første initialisering af det fysiske interface og opretter vores device. I tilfældet SPI, bruger SPI subsystemet data fra Device Tree'et direkte til at initialisere SPI controlleren i Rpi. Det eneste som skal gøres i probe er at angive størrelsen af bits_per_word, som er 8 for vores BCM2835 processor.

Læg mærke til `MySpi` struct'en, i forhold til vores GPIO driver gemmer vi en reference til vores spi device, i stedet for et gpio nummer.

### Tilpasning af probe() / remove()

`probe()``understøtter pt. kun én kanal per SPI device, men vores devices understøtter flere kanaler, eks. ADC har to input kanaler, hvor input vælges vha. den SPI kommando som sendes til ADC'en.

Udvid `probe()` og `remove()` til at understøtte minimum to kanaler. Dvs. det første spi device, kanal 0 får minor 0, mens det første spi device kanal 1 får minor 1, det evt andet spi device kanal 0 får minor 2 osv...

Byg driver og .dtbo fil, kopier til target, insmod og load overlay (se note nedenfor!!) og verificér i dmesg at tingene ser fornuftige ud, inden du fortsætter til implementeringen af read/write funktionerne.

Husk at vise/kommentere resultater i din besvarelse.

_Note!_ _I Rpizw's device tree er spi allerede allokeret til driveren spidev. Med den nuværende kerneversion, kan man i et overlay desværre ikke slette et allerede oprettet device og spidev understøtter ikke "status=disabled", hvilket ellers er standardmåden at disable en enhed på._
Vores workaround er følgende:

1. Blacklist spidev driveren, dvs. den må ikke loades automatisk med modprobe under opstart. Dette gøres ved at oprette en konfigurationsfil, /etc/modprobe.d/blacklist.conf, på Rpi med indholdet "blacklist spidev" (uden ").
2. I stedet for at loade vore devicetree overlay (.dtbo) med dtoverlay funktionen, skal det loades under boot. På Rpi, kopierer du din .dtbo fil til /boot/overlays/ og tilføjer en teksten "dtoverlay=navnet_på_mit_overlay" sidst i filen /boot/config.txt. Overlay loades herved automatisk under boot.
   Læs mere om dette i delopgave 4 i [hotplug øvelsen](../exercise-6/README.md).
3. Reboot Rpi og check at der ligger et device, spi_drv@0, under /sys/firmware/devicetree/base/soc/spi\@7e204000. Dvs. at vores spi_drv nu er et device under spi controlleren.

_For MCP4802 (D/A) skal du være opmærksom på følgende:_

- Bruger en GPIO til at opdatere udgange, gpio skal requestes og direction sættes i Probe(). Hent GPIO nummer fra Device Tree eller skriv det direkte ind i driveren.
- Kredsen har to udgange, dvs. vil man anvende dem begge skal man oprette to devices i probe, ex. dac0 + dac1, så man kan tilgå dem individuelt. Gem spi_device pointer og kanal nummer i den globale struct (spi_devs), så kan vi bruge værdierne når vi skal implementere write()/read()
  Eksempel på mcp4802dev:

```c++
struct mcp4802dev {
  struct spi_device *spi; // Pointer to SPI device
  int channel;            // Channel, ex. dac ch 0
  int lddacs_gpio;        // Gpio number to load dac output
};
```

_For MCP3202 skal du være opmærksom på følgende:_

- Kredsen har to udgange, dvs. vil man anvende dem begge skal man oprette to devices i probe, ex. adc0 + adc1, så man kan tilgå dem individuelt. Gem spi_device pointer og kanal nummer i den globale struct (spi_devs), så kan vi bruge værdierne når vi skal implementere write()/read()
  Eksempel på mcp3202dev:

```c++
struct mcp3202dev {
  struct spi_device *spi; // Pointer to SPI device
  int channel;            // Channel, ex. adc ch 0
};
```

_For PSoC skal du være opmærksom på følgende:_

- Hvis dit PSoC design indeholder flere subdevices, ex. temperatur, led, sw, så skal du oprette et device med device_create () for hvert sub device. Lav en fornuftig navngivning af dine devices ex "psoc%d-sw", "psoc%d-led" osv. Man kan herved tilgå dem individuelt som ex /dev/psoc0-sw. Gem spi_device pointer og kanal nummer (subdevice addr) i den globale struct (spi_devs), så kan vi bruge værdierne når vi skal implementere write()/read()
  Eksempel på psocdev:

```c++
struct psocdev {
  struct spi_device *spi; // Pointer to SPI device
  int subdevice;          // Subdevice, number to be sent to PSOC to access ex. adc, temperature, button etc.
};
```

### Implementering af Read/Write

Når du skal læse/skrive fra/til dit device skal du studere det pågældende device's datablad for hvordan dets protokol er. Der er typisk vist et timing diagram, hvor det er vist hvad de enkelte bits i beskeden udgør:

![](../images/psoc_spi_read_waveform.png)

Vær opmærksom på at SPI controlleren i chippen på Raspberry Pi kun kan lave beskeder i chunks af 8-bit, dvs. skal du overføre et skævt antal, f.eks. 12 bits, så skal du konsultere databladet til device'et om du skal sende 4 tomme bits først, ellers sidst i beskeden (2x8-bit = 16-bit).

Du skal altså opdatere de eksisterende read/write funktioner i templaten til at anvende en funktion (som du laver) der læser eller skriver vha. spi, i stedet for gpio_get/set_value() som vi gjorde det i platform driveren.

Fremgangsmåden er således i stil med (Se slides og dokumentation):

```C
 int my_spi_func(struct spi_device sdev, int channel, int *result)
  /* Prepare tx data */
 buf = 1 << 8 | 21;

  /* Configure tx/rx buffers */
  t[n].tx/rx_buf = xxx; // Remember to use pointers!
  spi_message_add_tail(&t[0], &m);

  /* Transmit SPI Data (blocking) */
  err = spi_sync(m.spi, &m); // Remember to check for spi errors!

  /* Decode Result (if read) */
  *result = rx_data/2 ;
```

_For MCP4802 (D/A) skal du være opmærksom på følgende:_

- MCP4802 har ingen SDO/MISO pin, dvs. man kan ikke læse fra den og read() i driveren behøves derfor ikke implementeret.
- Man vælger mellem de analoge udgange på MCP4802 ved at sætte en bit i beskeden som sendes. Bit'en kan sættes udfra værdien af 'channel' i mcp4802dev stuct'en som du har tildelt i probe().
- GPIO'en som er forbundet til MCP4802 skal være default høj, men gå lav->høj når de analoge outputs skal opdateres.Du skal derfor bruge gpio_set_value() efter du har send SPI beskeden.
- Det være smart om de værdier som anvendes i user space svarer til spændinger. _echo 1000 > /dev/dac0_
  giver et output på 1000 mV. Du skal derfor omregne værdien fra user-space inden den sendes til MCP4802

_For MCP3202 (A/D) skal du være opmærksom på følgende:_

- I driverens read funktion skal du returnere værdien af en læsning fra en ADC kanal. For at læse værdien fra en ADC kanal skal der først sendes en besked til MCP3202 om kanalnummer mm., inden en værdi returneres, dvs. overførslen er to-vejs, se databladet (afsnit 6.1)
- Kanalen kan sættes udfra værdien af 'channel' i mcp3202dev stuct'en som du har tildelt i probe().
- Du kan bruge driverens write funktion til at sætte egenskaber for ADC'en, dvs. parametre som skal sendes med ved læsning af ADC, disse kunne føjes til spi_devs struct'en. Dette er dog frivilligt.
- Værdien returneret til user-space skal svare til den målte spænding i millivolt og I skal derfor omregne måleværdien inden den kopieres til user-space.

_For PSoC skal du være opmærksom på følgende:_

- Hvis du bruger [PSoC Template Projektet](https://redweb.ece.au.dk/devs/projects/raspberry-zero/wiki/Connecting_RpiASE_fHat_to_PSoC) så bruger det en protokol hvor man sender nummeret på det sub-device man vil tilgå og om det er en read- eller write dataoverførsel. Data følger herefter. Se protokollen via førnævnte link.
- Hvis man skal læse et sub-device, skal man først sende kommandoen om at man vil læse fra device'et, dernæst vente 20 us (udelay) og så udlæse dataene. Pausen skyldes at koden i PSoC'ens interrupt service rutine først skal lægge data i PSoC'ens SPI TX buffer.
- Sub-device kan sættes udfra værdien af 'subdevice' i psocdev stuct'en som du har tildelt i probe().
- Hvis I bruger jeres egen protokol, så skal I selvfølgelig efterleve den i driveren

### Test

Når du skal udvikle og teste en SPI driver er det nødvendigt at sætte en debugger på SPI forbindelsen, for at verificere at tingene ser rigtige ud, her er det mega heldigt at I har Analog Discovery:

![](../images/ase_rpi_fhat_analog_discovery.jpg)

Hvis man vælger "Logic" i Waveforms programmet, kan man fortælle at nogle givne ledninger er forbundet til et SPI interface, beskederne på SPI vil herefter blive dekodet. Du skal også bruge det til at verificere at clock polaritet og fase er rigtig, som angivet i data bladet.

![](../images/analog_discovery_spi_capture.png)

Hvis du implementerer driveren til DAC'en kan du verificere den analoge spænding ved at måle den med scopet:

![](../images/analog_discovery_analog_scope.png)

Laver du A/D konverter driveren, kan du bruge waveform generatoren i Analog Discovery til at lave en analog spænding og sætte den på ADC'en input og verificere resultatet. Tjek først spændingen med Analog Discovery multimeter/oscilloscope inden du slutter den til A/D inputtet. Spænding må ikke ligge udenfor 0-3.3 volt.

Husk dokumentere jeres tests med screen dumps fra Analog Discovery værktøjet, et billede af test setup vil også være godt.

# Øvelse: LDD med Timers og Attributes (Rpi)

## Formål

Formålet med denne øvelse opnår erfaring med at SysFS attributter og
timers ved er at opdatere en GPIO device driver således at den anvender
SysFS attributter til at styre en ny attribute som lader en GPIO toggle.

Du forventes med øvelsen at opfylde følgende mål:
| **Mål** | **Delmål 1** | **Delmål 2** | **Delmål 3** |
|------------------------------|-------------------------------------|----------------------------------------|----------------------------------|
| Oprettelse af attributter | show/store funktioner | Deklarere dem og gøre dem til default attributter | Bygge til- og teste på target |
| Lave attribute funktionalitet | Implementer timer state | Implementer timer delay | Test med printk |
| Lave timer funktion | Implementer timer callback | Test på board | Mål timers præcision og jitter |

## Godkendelse

I bestemmer selv om I vil aflevere øvelse 7 eller 8. Deadline er som
angivet i lektionsplanen på Brightspace.\
Såfremt I aflverer denne, skal Besvarelsen indeholde relevante
kodeudsnit som der henvises til i forklarende tekst, tests og diskussion
af resultater heraf. Komplet kode inklusiv Makefiles lægges i
repository.

## Forberedelse

- Du skal have læst de relevante sider i kapitel 4+5 i Essential Linux
  Device Drivers
- Se på source koden i
  \~/sources/rpi-5.4.83/drivers/leds/led-class.c + leds-gpio.c
- Se detaljer omkring "Reading/Writing Attribute Data" i sysfs.txt. Se
  linje 165 og frem og eksempler linje 229 og frem
- Kopier platform driveren fra øvelse 6 til en ny folder, vi arbejder
  videre med denne.
- Du skal have en Raspberry Pi Zero, samt en ASE fHAT med komponenter
  monteret som vist på figuren herunder:

![](../images/ase_fhat_board_irq.png)

## Øvelsen

Du skal i øvelsen udvide din platform driver til at kunne toggle GPIO
output portene ved hjælp af en timer funktion. Toggle delay og hvorvidt
gpio'en toggler (toggle_state) skal kunne sættes og udlæses ved hjælp af
attributes.

Den overordnede struktur for driveren er således (**eksempel!!!
attribute, _my_led_state_**):

```C
#include <linux/gpio.h>
...
static ssize_t my_led_state_show(struct device *dev, struct device_attribute *attr, char *buf)
{ ... }

static ssize_t my_led_state_store(struct device *dev, struct device_attribute *attr, const char *buf, size_t size)
{  ... }
...
DEVICE_ATTR_RW(my_led_state);
static struct attribute *my_led_attrs[] = { ... }
ATTRIBUTE_GROUPS(my_led); // Creates led_groups
...
static int __init my_platform_driver_init(void)
{
    ...
    gpio_class = class_create(THIS_MODULE, "my_led");
    gpio_class->dev_groups = my_led_groups;

    err = platform_driver_register(&my_led_platform_driver);
    ...
}
```

Bemærk ifht. eksemplerne i bogen, at funktionen _class_device_create()_
er erstattet med _device_create()_ og I derfor ikke skal spekulere på
manipulation af kobjekter. **Bemærk desuden at _my_led_state_ kun er et
eksempel, ikke navnet på den attribute som I selv skal lave!!**

### Oprettelse af device attributer

Opret show + store funktioner for attributten _gpio_toggle_state_.
Funktionerne har prototyperne:

```C
/* Global Variable */
static int toggle_state;
/* Sysfs "read" method prototype */
static ssize_t gpio_toggle_state_show(struct device *dev, struct device_attribute *attr, char *buf)
/* Sysfs "write" method prototype */
static ssize_t gpio_toggle_state_store(struct device *dev, struct device_attribute *attr, const char *buf, size_t size)
```

Indtil videre skal _\_store()_ funktionen alene gemme en værdi i en global variabel, mens _\_show()_ udlæser den. Lav gerne en printk i de to funktioner til debugging, husk "\\n" i tekststrengen.

Attributten skal nu deklareres og angives som en default attribute for vores class. Benyt nedenstående makroer og struct til at deklarere variable.

Fra _device.h_
[\~/sources/rpi-5.4.83/include/linux/device.h](http://git.kernel.org/?p=linux/kernel/git/stable/linux-stable.git;a=blob;f=include/linux/device.h);
):

```C
// Example of creating an r/w attribute "state" for the class "led" and adding it to the attribute group "led"

DEVICE_ATTR_RW(led_state); // Creates dev_attr_led_state

static struct attribute *led_attrs[] = {
    &dev_attr_led_state.attr,
    // More attributes could go in here,
    NULL,
    } ;

ATTRIBUTE_GROUPS(led); // Creates led_groups
```

Herefter skal du i driverens init funktion sætte dev_groups på den nyoprettede class pointer. Det er vigtigt at du gør det umiddelbart efter _class_create()_ og inden _platform_register()_. Kaldet til _platform_register()_ gør at _probe()_ kaldes med det samme, hvis der allerede er loaded et device tree.

Metoden der er brugt i bogen giver mulighed for at lave specifikke attributter for et givet device. Det står så derfor i kontrast til det I laver her, hvor alle "devices" af samme "class" har/får samme adfærd ("nedarver").

#### Test af atttributter på target

Indsæt modul og load devicetree. Bemærk at der i /sys/ under dit/dine nye device(s) er kommet en attribute. Skriv og læs på attributten og check i dmesg at det lykkedes.

### Implementering af toggler attributter

Vi går nu videre med den allerede oprettet attribute og udvider med funktionalitet og en attribute mere:

- gpio_toggle_state - Sætter/udlæser vores GPIO toggle tilstand (1=toggling / 0=not toggling), dvs den skal oprette/starte- og slette timeren alt efter om værdien er 0 eller 1.
- gpio_toggle_delay - Sætter/udlæser 1/toggle hastigheden, dvs den skal sætte en variabel som indeholder hvor meget der skal lægges til expiration time i timerfunktionen.

Der skal bruges følgende funktioner (fra linux/timer.h):

- "timer_setup(timer, callback, flags)":https://elixir.bootlin.com/linux/latest/source/include/linux/timer.h#L126\
- "extern int mod_timer(struct timer_list \*timer, unsigned long expires)":https://elixir.bootlin.com/linux/latest/source/include/linux/timer.h#L157\
- "int del_timer_sync(struct timer_list \*timer)": https://elixir.bootlin.com/linux/latest/source/include/linux/timer.h#L183

og en timer callback funktion på formen:

- `static void my_timer_callback(struct timer_list *t)`

Toggler funktionaliten skal implementeres vha en kernel timer. Se bog og slides for detaljer herom. N.B. For at stoppe/slette en timer kaldes `del_timer_sync(&timer)`.

Det er muligt at lave lave toggler funktionaliteten på to måder:

- Toggler funktionaliteten virker på alle gpio'er på en gang.
- Toggler funktionaliteten virker individuelt på de enkelte gpio'er, dvs. delay og state gælder per gpio. (Mere avanceret)

I begge tilfælde har attributterne tilhørende show og store metoder. Det følgende er lidt hjælp til implementeringen af disse.

#### Show metoden

Denne metode skal blot udlæse tilstanden af den pågældende attribute. Bruger man globale variabler for hhv. _state_ og _delay_ kan man nøjes med at læse værdien af den respektive tilbage og se bort fra det følgende omkring drvdata.

Har man _state_ og _delay_ for hver GPIO skal man have disse data tilknyttet det pågældende device (som er oprettet med device \_create...). Hvert oprettet device er af typen struct device, og denne indeholder et element, drvdata af typen void\*.\ Når man opretter et device med device_create kan man initialisere denne pointer med data som tilhører det pågældende device. Disse data kan siden tilgås i hhv. show()/store() funktionerne vha funktionen _dev_get_data()_. I det følgende eksempel initialiseres drvdata med en pointer til en int med gpio nummeret og siden hentes gpio nummeret ud i _show()_ funktionen:

```C
static int gpio_no = 164; /* Global variabel with GPIO number */

chr_drv_probe(..) {
    ...
    device_create(my_class, NULL, MKDEV(MAJOR(devno), its_minor),
                &gpio_no, "mygpiodevicelabel");
...}

static ssize_t gpio_toggle_delay_show(struct device *dev, struct device_attribute *attr, char *buf) {

    int *gpio_in_use_ptr = dev_get_drvdata(dev);
    printk("Show accessed for device with GPIO %i\n", *gpio_in_use_ptr);
    ...
}
```

Skal man bruge dette i vores sammenhæng, ville det være smart at kunne få adgang til det rigtige element i vores gpio_devs struct.... ;-)

Desuden kunne det være smart at udvide vores gpio_devs struct til også at indeholde togge_state, toggle_delay og evt. en struct timer. Herved kan vi gemme toggle data for hver gpio port.

#### Store metoden

Store metoden er meget lig det vi kender fra _write_ metoden. Vi behøver dog ikke kopiere fra user space. Ex:

```C
static ssize_t gpio_toggle_delay_store(struct device *dev,
        struct device_attribute *attr, const char *buf, size_t size)
{
    ssize_t ret = -EINVAL;
    unsigned long value;

    // Force zero termination to end of string
    buf[size-1] = 0;

    // Convert string to unsigned long (s to ul)
    if((ret = kstrtoul(buf, 10, &value))<0)
    return ret;

    /* Do something with value */

    ret = size; // Always read the full content of buf
    return ret;
}
```

I den simple udgave skal vi blot opdatere de respektive globale variable (hhv. toggle_delay og toggle_state) og ved _toggle_state_store()_ skal vi desuden initialisere timeren og starte den, hvis værdien er '1' eller delete den (del_timer_sync()), hvis værdien er '0'.

I udgaven med en timer per gpio, er det data'ene som kan tilgås via dev_get_drvdata() som skal opdateres, og timeren som skal startes. Bemærk!! timerens .data element kan indeholde en unsigned long int. Dette er netop hvad en pointer fylder, og man kan derfor angive en pointer, som kan derefereres og anvendes i timerens callback (handler) funktion.

**NOTE!** Det er vigtigt at få lukket timeren ned hvis modulet tages ud eller device tree unloades. Timeren lukkes ned med hjælp af _del_timer_sync()_ find funktionsprototypen mm. med en søgning på <https://elixir.bootlin.com> bemærk at der i resultatet både hvor funktionen er defineret som prototype, hvor den er defineret som funktion (kode) og hvor man kan finde dokumentation. Hvad gør funktionen ud over at deaktivere timeren?

### Implementering af Timerfunktion

I alle tilfælde opdateres expiration time og gpio toggles.

I den simple udgave kan man vælge enten blot at toggle en enkelt gpio eller alle gpioer. Man kan også løbe alle gpio'er igennem og kun toggle de, for hvem "toggle_state" er sat til '1'.

Benytter man en timer per gpio, skal man bruge _from_timer()_ til at få en pointer til det device som skal toggles ud fra timer callback funktionens parameter `struct timer_list *t`.
`

### Test af Toggler på Target

Gør som i tidligere tests. Anvend echo til at sætte et toggle delay og starte din toggler på target. Check om LED'en lyser. Lyser den svagt kan det være fordi at den blinker hurtigt. Anvend evt printk/dmesg beskeder til at debugge og verificere dit arbejde. Check desuden gerne med Analog Discovery hvor godt toggle frekvensen passer, hvor meget [jitter](https://e2e.ti.com/blogs_/archives/b/energyzarr/archive/2012/10/26/the-truth-about-jitter) der er på den og om den ændrer sig over tid.
