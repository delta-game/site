# DELTΔ: The Build Process

DELTΔ is in the perpetual prototype stage. Changes are sporadic, often dramatic, and may more often than not break things. Until we have a launcher that does automatic versioning based on a given commit -- proceed with caution and make sure you're backing your database files up manually. I tried to warn ya!

## Prerequisites

Before you start, you need to make sure that you have the following prerequisites installed:

- GoLang 1.19 or later (you can download it [here](https://golang.org/dl/)). For Linux **[1]** 
- The defacto package-manager for your Operating-System.

On Windows, Chocolatey is required to use the `depends` command. You can download it from [here](https://chocolatey.org/install). On MacOSX, Homebrew is required for the same reason. You can download it [here](https://brew.sh/). On Linux, your distribution's package manager is required to install the necessary packages. Right now, we officially support Apt via Debian and Ubuntu, Pacman via Arch, and Dnf via Fedora.

Linux is the sole development platform that I use and while MacOSX is Unix-like enough where things should probably 'just work' and has been confirmed early versions currently do -- I have no idea about Windows. So if this guide doesn't work, it's probably worth setting up WSL2 (Windows Subsystem for Linux 2) using the Debian image, which can be downloaded from the Microsoft Store, and trying that. You can learn more about setting up WSL2: [here](https://learn.microsoft.com/en-us/windows/wsl/install), and download the Debian image: [here](https://www.microsoft.com/store/apps/9MSVKQC78PK6).

**[1]** On Linux, check your package-manager for a "go" or "golang" package. It should be availble on all supported distros.

## Step 1: Download The Latest Commit

First, we need to download the latest commit of DELTΔ from our GitHub repository.

1. Go to the DELTΔ repository on GitHub at [https://github.com/delta-game/delta](https://github.com/delta-game/delta).

2. Click on the green "Code" button on that page and select the "Download Zip" option.

3. Extract the downloaded ZIP file to a directory on your computer.

4. Open your system's terminal application and navigate to the directory where you extracted the DELTΔ files using the `cd` command. For example, if you extracted the files to `/home/<username>/Downloads/delta`, you would run the following cmd:

    ```bash
    # Replace the <username> w/ ... well ya know.
    cd /home/<username>/Downloads/delta-main #On Linux.

    cd /Users/<username>/Downloads/delta-main #On MacOSX

    cd C:\Users\<username>\Downloads\delta-main #On Win10+ 
    ```


## Step 2: Setting Up Magefiles

To build DELTΔ, we first need to set up Magefiles.

1. We should already have GoLang installed and our terminal app opened. Now we need to run the following command, which will download and install the `magefile` executable to our `$GOPATH`:

    ```bash 
    go install github.com/magefile/mage@latest
    ```
2. To view all user facing targets, run the following command:

    ```bash
    mage help
    ```


## Step 3: Grabbing All Dependencies

1. This will check if all the dependencies are installed and should install any missing dependencies.

    ```bash
    mage depends
    ```

    - If this fails for whatever reason, we can manually list out the command we use and can either try running it ourselves in a different terminal (possibly one with admin privs) or searching for these packages in your package-manager manually
        ```bash
        mage pkglist #this may-or-may-not be implemented atm...
        ```

## Step 4: Building And Running The Binary

1. To build DELTΔ, we really just need one command:

    ```bash
     mage rerun
     ```
        This should (re)buid and run the binary. Get it? "Rerun".

2. We could even do these steps seperately as the following:

    ```bash
    mage build
    
    mage run
    ```

## Step 5: Optional / Miscellaneous Commands
And there we have it! We should have a fully functional build of DELTΔ  ... well assuming it's not broke upstream. lol There's a few other odds-and-ends we can do. These are all completely optional, but I think still nice to know about.
1. Intall it to a local directory. This will assure it will be installed in the top-level of your user directory under .DELTΔ/ that will (eventually) contain a vers/ directory of each commit you built and then 'installed' on your machine. 
    ```bash
    mage install # fyi ... this currently doesn't implement a /vers directory with commit-hashes for subdirectory names lol
    ```
2. Clean the local build. Let's assume we want to start from scratch and try to rebuild everything.
    ```bash
    mage clean
    ````
        If you grabbed the actual git repo, and not just downloaded the zip; This pairs very well with `mage update` that should pull the latest commit. rn: it assumes we have git installed locally, but I'm hoping to move to go-get and ship it as part of the magefile proper.

3. Completely nuke your .DELTA dir. Please make sure you back up things you care about like select database files.
    ```bash
    mage remove
    ```

> Note: We're planning to add a video to this page that walks people through this process sooner-than-later; I promise you it's really that not hard to go it your own, but a lack of a single executable / binary -- I get is a bit intimidating. We do also, "eventually" plan to have a [proper launcher](https://github.com/delta-game/loft) to make this a no-brain(er) activity.
