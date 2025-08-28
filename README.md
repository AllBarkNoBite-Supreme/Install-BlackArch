# HOW TO INSTALL BLACKARCH


### INSTALLING BLACKARCH USING THE SLIM OS ISO FILE


## REQUIREMENTS

**Stable** *Internet Connection*

**2 Drives**. *One 8GB minimum. Second 200GB minimum*

## PURPOSE


During my install I found that there was very little support; *atleast in text form*, for anyone wanting to switch
to blackarch linux. Troubleshooting the problems with the install itself can be somewhat of a nightmare if you
are not methodological about how you approach the solve. I've documented the steps that I followed (*atleast the 
steps that will lead to a meaningful outcome*) to obtain a fully functional blackarch driven system. 

## CREATING A BOOTABLE DRIVE


You must have the blackarch SLIM ISO downloaded, if not, you can get it from this [direct download link](https://ftp.halifax.rwth-aachen.de/blackarch/iso/blackarch-linux-slim-2023.05.01-x86_64.iso)

To create a bootable drive, you can use **a)** *dd in the linux shell* or **b)** *balena etcher: a GUI available on all 
major platforms.*

To flash the ISO to your USB stick run this command (using dd):
`dd if=/path/to/image.iso of=/dev/sdX status=progress oflag=sync bs=16M` where X is replaced by the letter that
suffixes your desired drive, and bs(buffer size) is set to 16MB instead of the *ridiculous* 512B. Wait for the 
program to finish and eject your drive.

The usage of [balena etcher](https://etcher.balena.io/#download-etcher) is very straight forward, it will automatically detect available drives and also prompt you
for the location of you ISO image. 

**RUFUS** may also be used, but do so in dd mode and also note *it is considerably slower than balena etcher for 
this particular ISO.* I do not understand why.

## BOOTING INTO THE USB


You will need to get into your **boot manager** and manually select the Installer USB, especially if your system
is not preconfigured to boot from *external storage media*. Your default login credentials are;
*Login:* **liveuser**
*Password:* **blackarch**

At this stage I strongly advise **against** connecting the device to the internet as this will prompt the system
to update itself, which will further complicate the installation process. Therefore we will not be updating
packages just yet.

Press the installer icon and follow the prompts. The software used here is **calamares**, which is widely used
to install various distributions of linux such as *Debian*. If you are struggling with the partitioning please refer
to this [Debian Calamares install tutorial](https://c-nergy.be/blog/?p=19377) 

Once the installation has completed, shutdown the machine, remove the Installer USB and *boot onto the drive you just 
installed blackarch onto.* We are not done yet.

## CONFIGURING THE OS


Do *not* connect your device to the internet just yet.

Use the *credentials* you entered into the calamares installer to login into your blackarch OS.
Once you have logged in successfully, open the **terminal**. 
*Everything looks perfectly fine!* **Why bother!?**
I hate to be that guy, but unfortunately **this system is severely broken**. This guide will assist you in 
rectifying these issues.

Firstly we must make sure that the *community* is commented out in the *pacman.conf* file located at 
**/etc/pacman.conf** - This is necessary because it is no longer supported and will disrupt any attempts at 
updating the system. To do this we run `sudo nano /etc/pacman.conf` which will open the configuration file.
Make sure the *community* header is commented out as follows

```
#[community]
#Include = /etc/pacman.d/mirrorlist
```


*The hashtag makes it a comment.
Press `CTRL + X` to exit. Make sure to *save* the changes as this is **very important**

The next thing we have to do is set the system time. We can do this by running the `timedatectl` command.
Enter the appropriate time in this format: `sudo timedatectl set-time "2025-12-25 12:12:20"`. This is *important*
because it ensures packages can be downloaded correctly.

Now we are **removing packages that will cause update errors**; **calamares** and **jdk-openjdk.**
To do this run the commands: 
`sudo pacman -Rns calameres`
`sudo pacman -Rns jdk-openjdk`

Another package that will cause installation conflicts is the **python-typing_extensions**, package however getting
rid of this is *rather difficult* so we will allow pacman to do it for us.
Run the command `sudo pacman -Syy` to refresh pacman.

# UPDATING DATABASES AND MIRRORS


This is **arguably the most important** step of this entire configuration process. 

You can finally connect your device to the internet!

First we will configure the *blackarch-mirrorlist*, which you can download from this [direct download link](https://www.blackarch.org/blackarch-mirrorlist)

Once it is downloaded, open it using *Mousepad* and **copy** all its contents. 
You will now need to open the *blackarch-mirrorlist* located at **/etc/pacman.d/blackarch-mirrorlist**
Do this using this command `sudo nano /etc/pacman.d/blackarch-mirrorlist`
To **delete all the mirrors** in the file press these keys `ALT + \` and `ALT + T`
Then **paste** the new data using `CTRL + SHIFT +V`. Make sure to *uncomment one server that is closest 
to your region*. **This is important.**
Press `CTRL + X` and make sure to **save the file** before exiting. You may also copy the blackarch mirrorlist I've appended to the bottom of this paragraph, but do well to keep it up-to-date.






```
# Australia
#Server = http://au.mirrors.cicku.me/blackarch/$repo/os/$arch
#Server = https://au.mirrors.cicku.me/blackarch/$repo/os/$arch
#Server = http://blackarch.mirror.digitalpacific.com.au/$repo/os/$arch

# Austria
#Server = http://mirror.easyname.at/blackarch/$repo/os/$arch
#Server = ftp://mirror.easyname.at/blackarch/$repo/os/$arch

# Canada
#Server = http://ca.mirrors.cicku.me/blackarch/$repo/os/$arch
#Server = https://ca.mirrors.cicku.me/blackarch/$repo/os/$arch

# China
#Server = https://mirrors.hust.edu.cn/blackarch/$repo/os/$arch
#Server = https://mirror.sjtu.edu.cn/blackarch/$repo/os/$arch
#Server = https://mirrors.nju.edu.cn/blackarch/$repo/os/$arch
#Server = http://mirrors.nju.edu.cn/blackarch/$repo/os/$arch
#Server = https://mirrors.tuna.tsinghua.edu.cn/blackarch/$repo/os/$arch
#Server = https://mirrors.ustc.edu.cn/blackarch/$repo/os/$arch
#Server = http://mirrors.aliyun.com/blackarch/$repo/os/$arch
#Server = https://mirrors.aliyun.com/blackarch/$repo/os/$arch

# Denmark
#Server = https://mirrors.dotsrc.org/blackarch/$repo/os/$arch
#Server = http://mirrors.dotsrc.org/blackarch/$repo/os/$arch
#Server = ftp://mirrors.dotsrc.org/blackarch/$repo/os/$arch

# Ecuador
#Server = http://mirror.cedia.org.ec/blackarch/$repo/os/$arch
#Server = https://mirror.cedia.org.ec/blackarch/$repo/os/$arch

# France
#Server = http://blackarch.leneveu.fr/blackarch/$repo/os/$arch
#Server = http://mirror.cyberbits.eu/blackarch/$repo/os/$arch
#Server = https://mirror.cyberbits.eu/blackarch/$repo/os/$arch

# Germany
#Server = https://www.blackarch.org/blackarch/$repo/os/$arch
#Server = http://www.blackarch.org/blackarch/$repo/os/$arch
#Server = http://de.mirrors.cicku.me/blackarch/$repo/os/$arch
#Server = https://de.mirrors.cicku.me/blackarch/$repo/os/$arch
#Server = http://ftp.halifax.rwth-aachen.de/blackarch/$repo/os/$arch
Server = https://ftp.halifax.rwth-aachen.de/blackarch/$repo/os/$arch
#Server = ftp://ftp.halifax.rwth-aachen.de/blackarch/$repo/os/$arch
#Server = http://blackarch.unixpeople.org/$repo/os/$arch
#Server = https://blackarch.unixpeople.org/$repo/os/$arch

# Greece
#Server = http://ftp.cc.uoc.gr/mirrors/linux/blackarch/$repo/os/$arch
#Server = ftp://ftp.cc.uoc.gr/mirrors/linux/blackarch/$repo/os/$arch

# Great Britain
#Server = http://www.mirrorservice.org/sites/blackarch.org/blackarch/$repo/os/$arch
#Server = https://www.mirrorservice.org/sites/blackarch.org/blackarch/$repo/os/$arch
#Server = http://mirrors.gethosted.online/blackarch/blackarch/$repo/os/$arch
#Server = https://mirrors.gethosted.online/blackarch/blackarch/$repo/os/$arch

# India
#Server = http://in.mirrors.cicku.me/blackarch/$repo/os/$arch
#Server = https://in.mirrors.cicku.me/blackarch/$repo/os/$arch
#Server = https://mirror.maa.albony.in/blackarch/$repo/os/$arch

# Iran
#Server = http://mirror.blackrepo.com/$repo/os/$arch

# Italy
#Server = http://blackarch.mirror.garr.it/mirrors/blackarch/$repo/os/$arch

# Japan
#Server = http://www.miraa.jp/blackarch/$repo/$os/$arch
#Server = https://www.miraa.jp/blackarch/$repo/$os/$arch
#Server = http://jp.mirrors.cicku.me/blackarch/$repo/os/$arch
#Server = https://jp.mirrors.cicku.me/blackarch/$repo/os/$arch
#Server = http://www.ftp.ne.jp/Linux/packages/blackarch/$repo/os/$arch
#Server = https://www.ftp.ne.jp/Linux/packages/blackarch/$repo/os/$arch
#Server = http://ftp.kddilabs.jp/Linux/packages/blackarch/$repo/os/$arch
#Server = http://www.miraa.jp/blackarch/$repo/os/$arch
#Server = https://www.miraa.jp/blackarch/$repo/os/$arch
#Server = https://ftp.kddilabs.jp/Linux/packages/blackarch/$repo/os/$arch

# Korea
#Server = https://deny.krfoss.org/blackarch/$repo/os/$arch
#Server = http://kr.mirrors.cicku.me/blackarch/$repo/os/$arch
#Server = https://kr.mirrors.cicku.me/blackarch/$repo/os/$arch

# Netherlands
#Server = http://mirror.serverion.com/blackarch/$repo/os/$arch
#Server = https://mirror.serverion.com/blackarch/$repo/os/$arch

# Poland
#Server = http://ftp.icm.edu.pl/pub/Linux/dist/blackarch/$repo/os/$arch
#Server = https://ftp.icm.edu.pl/pub/Linux/dist/blackarch/$repo/os/$arch
#Server = ftp://ftp.icm.edu.pl/pub/Linux/dist/blackarch/$repo/os/$arch
#Server = gopher://ftp.icm.edu.pl/1/pub/Linux/dist/blackarch/$repo/os/$arch

# Portugal
#Server = http://eu.mirrors.cicku.me/blackarch/$repo/os/$arch
#Server = https://eu.mirrors.cicku.me/blackarch/$repo/os/$arch

# Romania
#Server = http://mirrors.hostico.ro/blackarch/$repo/os/$arch
#Server = https://mirrors.hostico.ro/blackarch/$repo/os/$arch

# Russia
#Server = http://repository.su/blackarch/$repo/os/$arch
#Server = https://repository.su/blackarch/$repo/os/$arch
#Server = http://mirror.yandex.ru/mirrors/blackarch/$repo/os/$arch
#Server = ftp://mirror.yandex.ru/mirrors/blackarch/$repo/os/$arch

# Singapore
#Server = http://sg.mirrors.cicku.me/blackarch/$repo/os/$arch
#Server = https://sg.mirrors.cicku.me/blackarch/$repo/os/$arch
#Server = http://download.nus.edu.sg/mirror/blackarch/$repo/os/$arch
#Server = https://download.nus.edu.sg/mirror/blackarch/$repo/os/$arch

# Sweden
#Server = http://mirror.zetup.net/blackarch/$repo/os/$arch
#Server = https://mirror.zetup.net/blackarch/$repo/os/$arch

# Switzerland
#Server = http://mirror.easyname.ch/blackarch/$repo/os/$arch
#Server = ftp://mirror.easyname.ch/blackarch/$repo/os/$arch
#Server = https://mirror.tillo.ch/ftp/blackarch/$repo/os/$arch
#Server = http://mirror.tillo.ch/ftp/blackarch/$repo/os/$arch
#Server = ftpes://mirror.tillo.ch/blackarch/$repo/os/$arch
#Server = ftp://mirror.tillo.ch/blackarch/$repo/os/$arch

# Turkey
#Server = http://ftp.linux.org.tr/blackarch/$repo/os/$arch
#Server = https://ftp.linux.org.tr/blackarch/$repo/os/$arch
#Server = ftp://ftp.linux.org.tr/blackarch/$repo/os/$arch

# Taiwan
#Server = http://blackarch.cs.nycu.edu.tw/$repo/os/$arch
#Server = https://blackarch.cs.nycu.edu.tw/$repo/os/$arch
#Server = http://mirror.archlinux.tw/BlackArch/$repo/os/$arch
#Server = https://mirror.archlinux.tw/BlackArch/$repo/os/$arch

# UK
#Server = http://mirrors.cicku.me/blackarch/$repo/os/$arch
#Server = https://mirrors.cicku.me/blackarch/$repo/os/$arch
#Server = http://mirrors.gethosted.online/blackarch/blackarch/$repo/os/$arch
#Server = https://mirrors.gethosted.online/blackarch/blackarch/$repo/os/$arch

# US
#Server = http://us.mirrors.cicku.me/blackarch/$repo/os/$arch
#Server = https://us.mirrors.cicku.me/blackarch/$repo/os/$arch
#Server = http://mirror.math.princeton.edu/pub/blackarch/$repo/os/$arch
#Server = http://distro.ibiblio.org/blackarch/$repo/os/$arch
#Server = ftp://distro.ibiblio.org/blackarch/$repo/os/$arch
#Server = https://mirror.team-cymru.com/blackarch/$repo/os/$arch
#Server = ftp://mirror.team-cymru.com/blackarch/$repo/os/$arch
#Server = https://mirrors.ocf.berkeley.edu/blackarch/$repo/os/$arch
#Server = http://mirrors.ocf.berkeley.edu/blackarch/$repo/os/$arch
#Server = https://ftp2.osuosl.org/pub/blackarch/$repo/os/$arch
#Server = http://ftp2.osuosl.org/pub/blackarch/$repo/os/$arch

```


Now we update the *pacman mirrorlist*. To do do this you must find the best mirrors for your region.
You can do that by using the [archlinux mirror generator](https://archlinux.org/mirrorlist/)
Copy the mirrors show in the website. Then open the pacman mirrorlist file located at 
**/etc/pacman.d/mirrorlist** using the command `sudo nano /etc/pacman.d/mirrorlist`
Press `CTRL + \` and `CTRL + T` to delete everything in the file.
Press `CTRL + SHIFT + V` to paste the new mirrors. Select a mirror you like. 
Press `CTRL + X` to exit. Remember to save the file as this is very important.
Run the command `sudo pacman -Syy` to refresh pacman.

After updating the mirrorlist files it is now necessary to update the *keyring*. Another **very very important
step.** To do this run the following command `sudo pacman -S archlinux-keyring`. You may also do the same for 
the blackarch keyring and even go as far as manually trusting the keys, however this will not be necessary,
atleast in my humble opinion. Run the command `sudo pacman -Syy` to refresh pacman.

## DEPENDENCY HELL


This is the part where things get very *interesting, frustrating and annoying*. **I want you to remember that
we have already updated the keyring.** So it will be a *tedious and wasteful* activity to **refresh or repopulate**
the keyring at this stage. What is of great interest however, is the fact that **some mirror servers are partially 
complete.** Meaning that **some packages will fail to download properly if you connect to a partially complete server.**
Going forward I advise that you ensure *package caching* is enabled on your system. It is enabled by *default*.
This will ensure that **downloaded packages are not deleted.** 

Further more, if pacman produces an error saying that a package is **corrupted or was signed using an invalid 
PGP key**, **it is most likely that your pacman mirror server is unreliable**. However I advise that you allow the 
download to **keep going until failure to ensure packages are cached and do not need to be redownloaded**... *This 
saves time.* If errors persist, *switch to a reliable mirror server* from the **list generated by the archlinux 
generator**. All of those mirrors **should be** perfectly functional, but in some instances **may be defunct.**
Do this until all packages download properly. **Once all the packages are properly downloaded, the installation
will begin automatically.** Allow it to go through **patiently, you will see a few warnings, but don't be alarmed.**

**REBOOT THE MACHINE ONCE THE INSTALLATION FINISHES.**

The reason why I am convinced the mirrors are culprit is because of the nature of the command I used to fully upgrade 
the system `sudo pacman -Syu --ignore python-typing_extensions --overwrite '*'`
`--overwrite '*'` will forcibly resolve any install issues. This ensures that you don't have to deal with dependency
issues manually. A process that is **nightmarish** *and will sink several hours or days to resolve if other "methods"
are used*. **I speak from experience.**

Moving away from my rant, the above command **should work in theory** and fully upgrade the system.However, if this guide
does not help you in resolving your install issues, please visit the official [blackarch reddit community](https://www.reddit.com/r/BlackArchOfficial/) and post
your issue there and also take [arch wiki](https://wiki.archlinux.org/title/Main_page) into consideration

**Whenever** pacman asks you to remove a conflicting package, your answer should always be **yes**. *Or else it will terminate
the update.*

**Wishing you the best, fastest install know to man. Not all must suffer. Hehe.**


# FINISHING THOUGHTS


If you wish to **install the full blackarch toolkit** consider running this command `sudo pacman -S blackarch`
or you can refer to the [pdf guide](https://blackarch.org/blackarch-guide-en.pdf) to see how to install groups. P.s **blackman has not been maintained in a long time**
use it at your own risk.
Another short rant... Forcibly removing conflicting packages is not a crime unless the packages in question are
**dbus** or **systemd**...

