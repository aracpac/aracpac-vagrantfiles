## Turbocharged Vagrantfiles with Linux, Mac, and Windows Support
The `Vagrantfiles` in this repository have been optimized for the 
[AracPac Vagrant boxes](https://app.vagrantup.com/aracpac/). Important configuration details like the box IP, primary hostname and aliases, and local and remote share points can be 
configured by modifying variables at the top of each `Vagrantfile`. Some features of these `Vagrantfiles` are:
* ✅ Linux / Mac / Windows host OS support for all functionality 
* ✅ Reverse NFS mounts automated by triggers
* ✅ Automatic shell scripts generated to enable and configure NFS support on Windows
* ✅ Various commented performance tweaks

## Beginner's Guide
### Software
First, you will first need to download and install [vagrant](https://www.vagrantup.com/docs/installation/) and
[virtualbox](https://www.virtualbox.org/wiki/Downloads).

### Plugins
Once you've installed vagrant, you will also need to install the
[Vagrant Host Manager](https://github.com/devopsgroup-io/vagrant-hostmanager) plugin, which you can do from the command
line after vagrant is installed:

```
vagrant plugin install vagrant-hostmanager
```

### Setting up your local development folder
1. Inside your *local* user directory, create a `dev` directory, and within that, a `boxes` and `src` directory:
```
.
└── dev
    ├── boxes  <-- used to store vagrant instances
    └── src    <-- used to store git source code
```
2. Navigate to the `src` directory using a command shell and clone this codebase by issuing the command below. This will
   download all of the source code from this repository to your computer. You will be prompted for your GitHub username
   and password.
```
git clone https://github.com/aracpac/aracpac-vagrantfiles
```

3. You will now have an `aracpac-vagrantfiles` directory within the `src` directory. Inside it, there will be two 
   directories,`mac` and `windows`, each of which will have `Vagrantfile`s within them. A `Vagrantfile` is a small 
   ruby script that instructs the `vagrant` software on configuring a virtual machine instance. In this case, the 
   `Vagrantfile` will configure a `virtualbox` instance, or *box*.
```
.
└── dev
    ├── boxes
    └── src
        └── aracpac-vagrantfiles
            ├── README.md
            ├── centos8-stream             <-- Vagrant configuration files to use for a CentOS virtual machine
                └── Vagrantfile
            └── ubuntu20                   <-- Vagrant configuration files to use for a Ubuntu virtual machine
                └── Vagrantfile
```

4. Now navigate to the `boxes` folder using your command shell, create a new directory called `aracpac`, and copy one of 
   the `Vagrantfile`s into this directory. **NOTE: you will need to create a new empty directory with a unique 
   `Vagrantfile` for every box you wish to create. Be sure to update the `# local ip for the box` and `# domain for the 
   box` configuration variables at the top of each `Vagrantfile`.**

5. Navigate to the `aracpac` directory using a command shell and issue `vagrant up`. Vagrant should now download
   the box and set up a local environment for you to use. **NOTE FOR WINDOWS USERS**: the first time you run 
   `vagrant up`, the script may end with an error if you do not have [Services for Network File System](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/services-for-network-file-system-command-reference)
   installed. This is expected and addressed in the next step.

6. **ONE TIME STEP FOR WINDOWS USERS**: After running `vagrant up`, you will find a new file in the `aracpac` directory 
   called `nfs_fix.bat`. Run this file from the command shell, and when prompted, enter administrator user credentials,
   then issue `vagrant up` againt after the script runs. The script will:
> * enable *Services for Network File System*
> * map the Windows anonymous user's `uid` and `gid` to `1000`, which is the `uid` of the `vagrant` user in the box
> * set the default windows filemode to 775, which ensures files and directories created on the nfs share are group writable

## Next steps
Once vagrant is ready, you can:
* ssh into the box by issuing the `vagrant ssh` command
* access the box through your web browser at https://dev.local/
* access the files in the box's webroot: on windows, via the `X:` drive, and on Linux and Mac, via the `./www` folder 
that is created in your project directory