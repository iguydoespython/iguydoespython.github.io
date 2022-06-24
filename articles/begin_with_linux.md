# Begin with Linux

Linux dominates the open source software space. 
If you intend to work with open source software it is best to begin with an understanding of Linux. 
Once you've become familiar with Linux, working in a shell on IBM i becomes trivial.

## Hardware
Do yourself a favor and install Linux on an old desktop or laptop. While this will be quite frustrating in the beginning, it will pay large dividends down the road.
And, you should be able to do this for little to no money involved. 
Check online resources, Facebook Marketplace, Craigslist or eBay. Woot often has good deals on refurbished desktops and laptops.
You should be able to find a good computer for not a lot of cash.

You won't need a lot of power for this system as it will be a simple development machine. 
Focus on something with a solid state drive and 8-16gig of RAM (alternatively, something where you can upgrade the RAM). 
Windows requires a lot more horsepower to run than Linux. 
Don't feel like you need the latest and greatest specs to get started. 
This is a system you're going to be learning on, not a high-end development machine.

## Which Linux Distro to choose
I recommend using [Pop!_OS](https://pop.system76.com). 
Pop!_OS is built on top of Ubuntu and uses a modified Gnome desktop.
It comes with a streamlined, easy to use installer that will quickly walk you through installing Linux on your hardware. 

[LearnLinuxTV](https://www.learnLinux.tv/) has a [Full Beginners Guide to Pop!_OS](https://www.youtube.com/watch?v=4mySqL4bCSw) available on YouTube. 
It covers installation and the provides an overview of using the OS.

## Package Managers
Packages managers on Linux are used to install applications on your system.
Instead of searching the web for an installation program to download, package managers provide a central repository from which you can install well known packages.
Windows also has a few package managers available, most notably, [Chocolatey](https://chocolatey.org/) and 
[Ninite](https://ninite.com/).
On Linux, package managers are not an afterthought. They are included in your distribution.

Pop!_OS uses the APT package manager.
Again, LearnLinuxTV has a couple good videos where you can learn more about package managers and the APT package manager in particular.

[Linux Command for Beginners 11 - Intro to Package management on Debian-based Distributions](https://www.youtube.com/watch?v=yxc2ntmH9xY)

[Linux Crash Course - The apt Command](https://www.youtube.com/watch?v=1kicKTbK768)

## Further Learning
There are lots of great resources on YouTube for learning about Linux.
LearnLinuxTV is probably my favorite.
But, others include:

[Awesome Open Source](https://www.youtube.com/c/AwesomeOpenSource)

[DistroTube](https://www.youtube.com/c/DistroTube)

[Engineer Man](https://www.youtube.com/c/EngineerMan)

[The Linux Experiment](https://www.youtube.com/c/TheLinuxExperiment)


I'm sure there are many other good channels out there, these are just the few I've found and regularly watch.

## The Terminal is your Friend
Windows is famous for being a point and click user interface. While Linux allows the same, the terminal is where it's at.
Unfortunately, terminal commands in Linux don't follow the convenient IBM i command naming conventions.
Instead, you have to learn and remember each command on their own.

[DistroTube](https://www.youtube.com/c/DistroTube) has a [Beginner's Guide to the Linux Terminal](https://www.youtube.com/watch?v=s3ii48qYBxA&t=27s) video on YouTube.

## Integrated Development Environment (IDE)
Since you're going to be coding on this system we'll need to have an IDE available.
While I use [PyCharm](https://www.jetbrains.com/pycharm/) for Python development, I'd recommend [Visual Studio Code](https://code.visualstudio.com/) for the IBM i developer (along with [Code for IBM i](https://code.ileditor.dev/pro) of course!).

## Wrap Up
Learning Linux on another platform will make using open source on IBM i considerably easier.
You can get started for very little money and not have to worry about access to an IBM i.

Once you have become comfortable with Linux, transitioning to IBM i is trivial. 
The IBM engineers have done a great job of making open source packages available on IBM i via the YUM package manager.

Happy Coding!


