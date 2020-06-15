# Dev env setup note

###### tags: `zdev`, `devenv`, `DevOps`, `Python`, `MacOS`

> This note is yours, feel free to play around.  :video_game: 
> Type on the left :arrow_left: and see the rendered result on the right. :arrow_right: 

- Auto-generated Table of Content
[ToC]

## Getting Xcode
Apple now provide the Xcode suite as a free download from the App Store. To install Xcode Command Line Tools, install Xcode from the App Store, then open a Terminal window and enter the following command:
```shell=
xcode-select --install 
````

## Setting Up Homebrew
Follow the instructions on the site [Homebrew](http://brew.sh/) provides a package management system for macOS.

You should also amend your PATH, so that the versions of tools that are installed with Homebrew take precedence over others. To do this, edit the file `.bashrc/.zshrc` in your home directory to include this line:
```shell=zsh
export PATH="/usr/local/bin:/usr/local/sbin:~/bin:$PATH"
```
You need to close all terminal windows for this change to take effect.

To check that Homebrew is installed correctly, run this command in a terminal window:
```shell=
brew doctor
```

To update the index of available packages, run this command in a terminal window:
```zsh
brew update
```

Once you have set up Homebrew, use the brew install command to add command-line software to your Mac, and brew cask install to add graphical software. For example, this command installs the Slack app:
```shell=
brew cask install slack
```

## Installing the Git Version Control System
The Xcode Command Line Tools include a copy of [Git](http://www.git-scm.com/), which is now the standard for Open Source development, but this will be out of date.

To install a newer version of Git than Apple provide, use Homebrew. Enter this command in a terminal window:
```shell=
brew install git
```

If you do not use Homebrew, go to the Web site and follow the link for Other Download Options to obtain a macOS disk image. Open your downloaded copy of the disk image and run the enclosed installer in the usual way, then dismount the disk image.

Always set your details before you create or clone repositories on a new system. This requires two commands in a terminal window:
```shell=
git config --global user.name "Your Name"
git config --global user.email "you@your-domain.com"
```

The global option means that the setting will apply to every repository that you work with in the current user account.

To enable colors in the output, which can be very helpful, enter this command:
```shell=
git config --global color.ui auto
```

## Text Editors
Installations of macOS include older command-line versions of both [Emacs](http://www.gnu.org/software/emacs/) and [vim](http://www.vim.org/), as well as TextEdit, a desktop text editor. TextEdit is designed for light-weight word processing, and has no support for programming. Add the code editors or IDEs that you would prefer to use.

If you do not have a preferred editor, consider using a version of [Visual Studio Code](https://code.visualstudio.com/). Read the next section for more details.

To work with a modern Vim editor, install [Neovim](https://neovim.io/).

## Visual Studio Code
[Visual Studio Code](https://code.visualstudio.com/) is a powerful desktop editor for programming, with built-in support for version control and debugging. The large range of extensions for Visual Studio Code enable it to work with every popular programming language and framework. It is available free of charge.

The Microsoft releases of Visual Studio Code are proprietary software with telemetry enabled by default. To avoid these issues, use the packages that are provided by the [vscodium](https://github.com/VSCodium/vscodium) project instead.

Once you have installed Visual Studio Code or VSCodium, read [this article](https://www.stuartellis.name/articles/visual-studio-code/) for more information about using the editor.

## Neovim
If you would like a modern Vim editor with a good default configuration, [set up Neovim](https://www.stuartellis.name/articles/neovim-setup/).

## Setting The EDITOR Environment Variable
Whichever text editor you choose, remember to set the EDITOR environment variable in your ~/.bashrc file, so that this editor is automatically invoked by command-line tools like your version control system. For example, put this line in your profile to make Neovim (nvim) the favored text editor:
```shell=
export EDITOR="nvim"
```

## Setting Up A Directory Structure for Projects
To keep your projects tidy, I would recommend following the [Go developer conventions](http://golang.org/doc/code.html). These guidelines may seem slightly fussy, but they pay off when you have many projects, some of which are on different version control hosts.

First create a top-level directory with a short, generic name like code. By default Go uses a directory called go, but you can change that when you set up a Go installation.

In this directory, create an src sub-directory. For each repository host, create a subdirectory in src that matches your username. Check out projects in the directory. The final directory structure looks like this:
```shell=zsh
code/
  src/
    bitbucket.org/
      my-bitbucket-username/
        a-project/
    gitlab.com/
      my-gitlab-username/
        another-project/
```

## Creating SSH Keys
You will frequently use SSH to access Git repositories or remote UNIX systems. macOS includes the standard OpenSSH suite of tools.

OpenSSH stores your SSH keys in a .ssh directory. To create this directory, run these commands in a terminal window:
```shell=
mkdir $HOME/.ssh
chmod 0700 $HOME/.ssh
```
To create an SSH key, run the ssh-keygen command in a terminal window. For example:
```shell=
ssh-keygen -t rsa -b 4096 -C "Me MyName (MyDevice) <me@mydomain.com>"
```

> [color=#FF0000]Use 4096-bit RSA keys for all systems. The older DSA standard only supports 1024-bit keys, which are now too small to be considered secure.
> 

# Programming Languages
## JavaScript Development: Node.js

Homebrew provides separate packages for each version of [Node.js](https://nodejs.org/). To ensure that you are using the version of Node.js that you expect, specify the version when you install it. For example, enter this command in a Terminal window to install the Node.js 12, the current LTS release:
```shell=
brew install node@12
```

Add the `bin/ directory` for this Node.js installation to your PATH:
```zsh
/usr/local/opt/node@12/bin
```

If you need yarn, enter this command in a Terminal window to install it:
```shell=bash
brew install yarn
```

## Go Development
Use Homebrew to install [Go](https://golang.org/):
```shell=
brew install golang
```

This provides the standard command-line tools for Go.

The current version of Go includes support for dependency management with [modules](https://blog.golang.org/using-go-modules). Use modules for new projects. Some existing projects still use [dep](https://golang.github.io/dep/), or an older tool.

## Setting a GOPATH
Current versions of Go do not require a GOPATH environment variable, but you should set it to ensure that third-party tools and Terminal auto-completion work correctly.

Set a GOPATH environment variable in your `~/.bashrc` file:
```shell=
export GOPATH="$HOME/go"
```

Then, add this to your PATH:
```shell=
$GOPATH/bin
```

Close the Terminal and open it again for the changes to take effect.

## Java Development: AdoptOpenJDK
Which Version of Java?
Many vendors provide a JDK. To avoid potential licensing and support issues, use the JDK that is provided by the [AdoptOpenJDK project](https://adoptopenjdk.net/). The versions of Java on the OpenJDK Website are for testers, and the Oracle JDK is a proprietary product that requires license fees.

Use the LTS version of the OpenJDK, unless you need features that are in the latest releases.

Once you have installed a JDK, get the [Apache Maven](https://maven.apache.org/) build tool. This is provided by the Maven project itself, and is not part of the OpenJDK.

Use [jEnv](https://www.jenv.be/) if you need to run multiple JDKs, such as different versions of the same JDK.

## Setting up Java with Homebrew
Run these commands in a terminal window:
```shell=
brew tap adoptopenjdk/openjdk
brew cask install adoptopenjdk11
```

This installs version 11 of the OpenJDK, from the AdoptOpenJDK project.
Run this command in a terminal window to install Maven:
```shell=
brew install maven
```

## Setting up jEnv
Run this command in a terminal window to install jEnv:
```shell=
brew install jenv
```

Next, add this to your PATH:
```shell=
$HOME/.jenv/bin
```

Add this to your `~/.bashrc file`:
```shell=
eval "$(jenv init -)"
```

Open a new terminal window, and run this command:
```shell=
jenv enable-plugin export
```

This enables jEnv to manage the JAVA_HOME environment variable.

To avoid inconsistent behaviour, close all the terminal windows that you currently have open. The jEnv utility will work correctly in new terminal windows.

Lastly, run this command to register your current JDK with jEnv:
```shell=
jenv add $(/usr/libexec/java_home)
```

To see a list of the available commands, type jenv in a terminal window:
```shell=
jenv
```

## Manual Set up of AdoptOpenJDK
To manually install a copy of the JDK:

1. Download the version of the JDK that you need from AdoptOpenJDK
2. Unzip the download
3. Copy the JDK directory to /usr/local/lib
4. Edit your ~/.bashrc file to set environment variables. For example, to use jdk-11.0.3+7 as the Java version:
```java=
JAVA_HOME=/usr/local/lib/jdk-11.0.3+7/Contents/Home
PATH=$PATH:/usr/local/lib/jdk-11.0.3+7/Contents/Home/bin
```

To manually install a copy of [Apache Maven](https://maven.apache.org/):

1. Download the latest version of Maven
2. Unzip the download
3. Copy the Maven directory to /usr/local/lib/
4. Add /usr/local/lib/MAVEN-DIRECTORY to your PATH environment variable

Replace MAVEN-DIRECTORY with the name of the directory that Maven uses, such as apache-maven-3.6.0.

Maven is written in Java, which means that the project provides one package, which works on any operating system that has a supported version of Java.

## Python Development: pipenv
Unfortunately, macOS includes a copy of Python 2, so you will need to install Python 3 yourself.

To maintain current and clean Python environments, you should also use [pipenv](https://docs.pipenv.org/). This builds on two features of Python: the [virtual environments](https://docs.python.org/3/tutorial/venv.html) and the [pip](https://pip.pypa.io/en/stable/) utility.

Enter this command to install Python 3 and pipenv using Homebrew:
```shell=
brew install python3 pipenv
```

Use pipenv to manage your Python projects. The pipenv tool itself will automatically work with the copy of Python 3 from Homebrew.

To use the Python 3 interpreter outside of projects that are managed by pipenv, specify python3 on the command-line and in your scripts, rather than python:
```shell=
python3 --version
```

If you need to run the pip utility, rather than setting up a development environment with pipenv, always use the command pip3:
```
pip3 --version
```

The [Python Guide tutorial](http://docs.python-guide.org/en/latest/dev/virtualenvs/) shows you how to work with pipenv.


*[HTML]: Hyper Text Markup Language
*[W3C]:  World Wide Web Consortium
The HTML specification
is maintained by the W3C.

## Reference
* https://www.stuartellis.name/articles/mac-setup/

:::info
:bulb: **Hint:** You can also apply styling from the toolbar at the top :arrow_upper_left: of the editing area.

![](https://i.imgur.com/Cnle9f9.png)
:::

> Drag-n-drop image from your file system to the editor to paste it!
:::info
:pushpin: Want to learn more? ➜ [HackMD Tutorials](https://hackmd.io/c/tutorials) 
:::

---

