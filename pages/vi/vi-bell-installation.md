# BeLL Installation

We will need community [BeLL](https://github.com/open-learning-exchange/BeLL-Apps) (Basic e-Learning Library) and its dependencies on our system to complete the next steps. Please follow the directions for your OS below.

## Prerequisites

#### Staying Organized

We recommend you designate a new directory for your work at OLE. This put all of your OLE related repositories in one place and enables you to be organized and efficient.

To do this, you could make a new folder directly through your OS GUI. Or you could open `Terminal`(macOS), `cmd`(Windows), or `shell`(Linux) and use the following commands: (Note the commands should be identical on all three operating systems)

1. `cd Desktop`
1. `mkdir OLE`

#### Virtualization

Before installing Vagrant on any platform, it is necessary to check if VT-x/AMD-V instruction set is enabled on your processor by checking the BIOS. This is a requirement for installing Vagrant on any platform since Vagrant is a type of virtualization software that utilizes VirtualBox. Most recent CPUs have this feature enabled already. If later you are having trouble running Vagrant, it may just be the case that VT-x/AMD-V is not enabled on your system.

If so, here are instructions to enable virtualization on [Windows](https://www.howtogeek.com/213795/how-to-enable-intel-vt-x-in-your-computers-bios-or-uefi-firmware/) | [Ubuntu](http://askubuntu.com/questions/256792/how-do-i-enable-hardware-virtualization-technology-vt-x-for-use-in-virtualbox) | [Macintosh](http://kb.parallels.com/en/5653)

---

## Windows

We wrote two different scripts to install the community BeLL and its dependencies on your computer.

They are equivalent, so if you run Windows 8.1 or above, you can use either of the two. If you like, you can also try both and provide us with feedback on which one worked better for you. It is not required to try both, but we would be grateful if you decide to do so.   

To run the script, copy and paste one of the lines below in a [Command prompt](http://www.howtogeek.com/235101/10-ways-to-open-the-command-prompt-in-windows-10/) opened as administrator.

##### Windows 8.1 and Above

```bat
@powershell -Command "(New-Object Net.WebClient).DownloadFile('https://raw.githubusercontent.com/dogi/ole--vagrant-vi/master/windows/install.ps1', 'install.ps1')" && @powershell -NoProfile -ExecutionPolicy Bypass -Command ".\install.ps1"
```
##### Windows 7 and Above

```bat
powershell -Command "(New-Object Net.WebClient).DownloadFile('https://raw.githubusercontent.com/dogi/ole--vagrant-vi/master/windows/install.bat', 'install.bat')" && start install.bat && exit
```
To run your community BeLL at the end of the installation, please find the MyBeLL icon on your desktop and double click on it. It will open a Firefox browser and take you directly to your community BeLL web page.

Note: If Firefox displays ```Unable to connect``` error when the MyBell icon is clicked, visit [Vagrant instructions](#!pages/vi/vi-vagrant.md) for more information on ```vagrant up```.

#### Dependencies

These programs will be automatically installed on your computer:

- **Chocolatey**
[Chocolatey](https://chocolatey.org/) is a package manager for Windows that we use to install/uninstall all the other programs in a simple and reliable way.
- **Bonjour**
[Bonjour](https://support.apple.com/kb/DL999?locale=en_US) is used to implement zero-configuration networking on your computer.
- **Git**
[Git](https://git-scm.com) is an open source version control system that we use for communication and management for our software. More specifically, we use gitter.im for communication and github.com for software management.
- **VirtualBox**
[Virtualbox](https://www.virtualbox.org) allows you to install a software virtualization package as an application on your OS.

  **Note:** if you already have VirtualBox installed on your computer and have existing VMs on virtualbox already, running the command above will re-intall VirtualBox and won't affect/wipe out your existing VMs; it will only add the a VM called "vi" to the ones you already have.
- **Vagrant**  
[Vagrant](https://www.vagrantup.com) is an open source tool for building development environments.

- **Firefox**
[Firefox](https://www.mozilla.org/en-US/firefox/new/) is a popular browser, which is guaranteed to work nicely with your community BeLL.

You now have a working [community BeLL](http://127.0.0.1:5985/apps/_design/bell/MyApp/index.html) on your OS. If you run into a problem, head over to [Troubleshooting section](#Troubleshooting).
It is advisable to use Firefox to access your community BeLL. If you don't have it already, you may want to download it.

---

## macOS or Ubuntu

Open your `Terminal`

#### For macOS

We assume that [brew](http://brew.sh/) is already installed:

```bash
brew install git
brew cask install vagrant
brew cask install virtualbox
```

#### For Ubuntu

```bash
sudo apt-get install git
sudo apt-get install virtualbox
sudo apt-get install vagrant
```

#### For macOS and Ubuntu – Install a Community BeLL

Make sure you've `cd` to the designate OLE directory you created earlier.

```bash
git clone https://github.com/dogi/ole--vagrant-vi.git
cd ole--vagrant-vi
vagrant up
```

You now have a working [community BeLL](http://127.0.0.1:5985/apps/_design/bell/MyApp/index.html) on your OS.
It is advisable to use Firefox to access your community BeLL. If you don't have it already, you may want to download it.

## Troubleshooting

1. On macOS, when you run `vagrant up`, you may experience an error such as the following: "vi: Box 'ole/jessie64' could not be found. Attempting to find and install...". A simple solution is to use this command `sudo rm /opt/vagrant/embedded/bin/curl`, This will remove the old version of curl in Vagrant and `vagrant up` should now work as usual. For more information, visit [this Stack Overflow question](http://stackoverflow.com/questions/23874260/error-when-trying-vagrant-up)

2. On Windows, when you run `vagrant up` from command prompt, you might get the following error :
"The executable `curl` Vagrant is trying to run was not found in the `%PATH%` variable. This is an error. Please verify this software is installed and on the path." A simple solution is to add Cygwin bin folder to path variable or use Git Bash rather than command prompt to run `vagrant up`. For more information, visit [this GitHub issue](https://github.com/hashicorp/vagrant/issues/6788)

3. On Ubuntu, you might get this error when you run `vagrant up`:

   > Stderr: VBoxManage: error: The virtual machine 'ud381_default_1463617458900_49294' has terminated unexpectedly during startup with exit code 1 (0x1) VBoxManage: error: Details: code NS_ERROR_FAILURE (0x80004005), component MachineWrap, interface IMachine

   This is caused when VirtualBox get a minor version Update. (i.e. 5.0.x -> 5.1.x or 5.1.x -> 5.2.x). There are some old unused modules, who are not compatible with the newer version, remain installed which causes the above problem and prevent VirtualBox from starting. A system restart also does not solve the problem.

   To solve it first remove the unused packages using `sudo apt-get autoremove`. Then reconfigure VirtualBox to install updated modules using `sudo /sbin/vboxconfig`

4. If you see "no_db_found" when trying to access <http://127.0.0.1:5985/apps/_design/bell/MyApp/index.html>. At this early stage, the simple solution would be using `vagrant destroy` to delete the current machine, then use `vagrant up` to rebuild it.

5. If the command `vagrant up` is not working, try to install [Virtual Box version 5.1](https://www.virtualbox.org/wiki/Download_Old_Builds_5_1).

## Next Steps

Now that you have installed your community Bell, head over to [BeLL Configurations](#!./pages/vi/vi-configurations.md) to register your community with the nation.
