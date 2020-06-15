- Table of Content
[ToC]
###### tags: `zdev`, `devenv`, `DevOps`, `Python`, `MacOS`
# Development Environments
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

### Git Ignore
Create the file `~/.gitignore` as shown below
```
# Folder view configuration files
.DS_Store
Desktop.ini

# Thumbnail cache files
._*
Thumbs.db

# Files that might appear on external disks
.Spotlight-V100
.Trashes

# Compiled Python files
*.pyc

# Compiled C++ files
*.out

# Application specific files
venv
node_modules
.sass-cache
```
followed by running the command below, in terminal:

```shell=
git config --global core.excludesfile ~/.gitignore
```
to not track files that are almost always ignored in all Git repositories.

Or simply download [macOS specific .gitignore](https://github.com/github/gitignore/blob/master/Global/macOS.gitignore) maintained by GitHub itself and put contents of it to ~/.gitignore.

Note: You can also download it using curl

```shell=
curl https://raw.githubusercontent.com/github/gitignore/master/Global/macOS.gitignore -o ~/.gitignore
```

### Git Config
Create `.gitconfig` file under your home dir. 
```
[push]
        default = matching
[core]
        trustctime = false
        editor = vim
        filemode = false
        excludesfile = /Users/zac/.gitignore
[color]
        ui = auto
[credential]
        helper = cache --timeout=3600
[merge]
        tool = vimdiff
[mergetool]
        keeptemporaries = false
        keepbackups = false
        prompt = false
        trustexitcode = false
[alias]
        last = log -1 --stat
        cp = cherry-pick
        co = checkout
        cl = clone
        ci = commit
        st = status -sb
        br = branch
        unstage = reset HEAD --
        dc = diff --cached
        lg = log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %Cblue<%an>%Creset' --abbrev-commit --date=relative --all
```

## Terminal
### iTerm
相信很多使用 MacOS 的人都會用 [iTerm](https://www.iterm2.com/) 這套 Termaial 工具 (我自己也是)，不過比較遺憾的地方在於，他並不能夠跨平台使用，他只存活於 MacOS 內而已

### Hyper
所以在這邊要推薦另外一個跨平台 Terminal 叫做 [Hyper](https://hyper.is/)，因為其使用 Electron 打包，所以可以在 MacOS, Windows 與 Linux 平台上面使用，所以假如日常工作環境可能會涵括到多重平台，又想要達成使用體驗一致性的話，推薦可以試試看 Hyper

## Shell
[Linux command and shell](https://nborrington.com/2015/09/21/linux-command-shells/)

![Shell](https://miro.medium.com/max/1400/1*b-wYTV-o-sphPqYQqHdDHQ.png "shell pic")

### Zsh (Z Shell)
[Zsh](https://zh.wikipedia.org/wiki/Z_shell) 是基於 Sh 之上，做出了大量的改進，並且同時加入了Bash、ksh 及 tcsh的某些功能。

### Fish (Friendly Interactive Shell)
*[Fish]: Friendly Interactive Shell
[Fish](https://en.wikipedia.org/wiki/Friendly_interactive_shell) 希望成為一個比其他 shell 互動性更強且體驗更佳的 shell，而他既不衍生於 Sh (Bourne Shell)也不衍生於 C Shell，所以按照人類社會的知識來看，他跟上面那堆並沒有親屬關係，可能可以算是外星人吧XD
就如上面所提到的，看來就是要在 Zsh 跟 Fish 中間選擇一套了，而因為自己的工作屬性關係 (SRE/DevOps)，所以雖然是要在本地端使用，但還是希望一些開發的腳本或是指令在 Server 端運作的時候不會遇到不預期的錯誤，所以選擇了跟 Bash/Sh 基因比較相近的 Zsh，講古好像講太久了，先喝口水來動手操作一下 (底下安裝指令使用 MacOS 示範，其他平台的安裝可以參考線上文件)

```
# 檢查是否已經有安裝 Zsh (已經裝了的話，就可以跳過下一個安裝步驟)
~$ zsh --version
zsh 5.7.1 (x86_64-apple-darwin19.0)
# 假如電腦內沒有安裝 Zsh，可以利用下面的指令安裝
~$ brew install zsh
```

## Configuration Framework
而 Zsh 之所以會強大，除了他從兄弟姐妹們那裡作出很多地改善之外，更厲害的地方在於它強大的社群與生態系，而為了要更方便地使用這些 Zsh Theme, Prompt, Plugins…等，便有了 Zsh 的組態管理工具

### Oh My Zsh Framework
[Oh My Zsh](https://github.com/ohmyzsh/ohmyzsh) 應該算是老牌的 Zsh 組態管理框架，我一開始也是使用這套，用了好長一段時間也一直都很滿意，不過後來發現他的啟動速度好像被另外一套 Zim 比下去，所以這篇文章使用的 Framework 會是 Zim

### Zim (Zsh IMproved FrameWork)
*[Zim]: Zsh IMproved FrameWork
Zim 的啟動速度比 Oh My Zsh 快上一倍左右，常言道時間就是金錢，所以除非 Zim 有什麼地方無法滿足需求，不然建議可以使用看看
```
# 安裝 Zim
~$ curl -fsSL https://raw.githubusercontent.com/zimfw/install/master/install.zsh | zsh
) Git is installed.
) Using Zsh version 5.7.1
) ZIM_HOME not set, using the default one.
) Zsh is your default shell.
) Downloaded the Zim script to /Users/smalltown/.zim/zimfw.zsh
) Copied Zim template to /Users/smalltown/.zimrc
) Copied Zim template to /Users/smalltown/.zshrc
) Copied Zim template to /Users/smalltown/.zshenv
) Copied Zim template to /Users/smalltown/.zlogin
) Installed modules.
All done. Enjoy your Zsh IMproved! Restart your terminal for changes to take effect.
# 將 Zsh 設定成預設的 Shell (假如上面有提示 Zsh 還不是預設 Shell 的話)
~$ chsh -s $(which zsh)
```
把 Terminal 重新開啟之後，便安裝完成啦！接著我們便可以利用它來安裝其他好用的工具

### Use Zim install Terminal Theme
而在 Theme 的選擇方面有兩個滿多人用的分別是 [Powerlevel10k](https://github.com/romkatv/powerlevel10k) 以及 [Spaceship](https://github.com/denysdovhan/spaceship-prompt)，先澄清一下，看起來酷不酷炫並不是我考量的要點XD 我注重的是執行速度，命令自動完成，常用命令提示，跟 Prompt 資訊的完整性…等，其實 Prompt 就是那個 PS1 環境變數，預設顯示的資訊其實滿陽春的，通常就只有 1) 使用者名稱 2) Hostname 3) 當前資料夾，對於這個資訊爆炸的時代來說，其實很不夠用，例如，我想要知道目前的 Git 相關資訊 (目前位於哪一個 Branch, Commit 的狀況…等)，或是想要知道 Kubernetes Context 目前使用得是哪一個，傳統的 PS1 的資訊其實完全不夠用，**在 Prompt 顯示的資訊豐富程度來說，我覺得 Spaceship 很棒，而 Powerlevel10k 也不錯，不過在兩者的回饋速度來說，Powerlevel10k 略勝一籌，因此在經過一段時間的評估之後，我選擇了 Powerlevel10k (底下有實際比較影片供參考)**
```
# 首先將 zmodule romkatv/powerlevel10k 加到 ~/.zimrc 中，
~$ vim ~/.zimrc
...
zmodule romkatv/powerlevel10k
...
# 存檔完之後執行安裝指令
~$ zimfw install
) powerlevel10k: Installed
Done with install. Restart your terminal for any changes to take effect.
```
按照指示重開完 Terminal 之後便會進入 Powerlevel10k 的設定精靈頁面，在這邊會有滿多的步驟需要設定的，感覺有點像是在填問卷一樣，他會問使用者很多的問題，用以完成最適合自己使用習慣的環境
```
This is Powerlevel10k configuration wizard. You are seeing it because you
 haven't defined any Powerlevel10k configuration options. It will ask you a few
                      questions and configure your prompt.
Install Meslo Nerd Font?
(y)  Yes (recommended).
(n)  No. Use the current font.
(q)  Quit and do nothing.
```

#### Customize prompt
Edit `~/.p10k.zsh` then `source ~/.p10k.zsh` to take effect.

Powerlevel10k doesn't recognize Pure configuration parameters, so you'll need to use `POWERLEVEL9K_COMMAND_EXECUTION_TIME_THRESHOLD=3` instead of `PURE_CMD_MAX_EXEC_TIME=3`, etc. All relevant parameters are in `~/.p10k.zsh`. This file has plenty of comments to help you navigate through it.

## Text Editors
Installations of macOS include older command-line versions of both [Emacs](http://www.gnu.org/software/emacs/) and [vim](http://www.vim.org/), as well as TextEdit, a desktop text editor. TextEdit is designed for light-weight word processing, and has no support for programming. Add the code editors or IDEs that you would prefer to use.

If you do not have a preferred editor, consider using a version of [Visual Studio Code](https://code.visualstudio.com/). Read the next section for more details.

To work with a modern Vim editor, install [Neovim](https://neovim.io/).

### Visual Studio Code
[Visual Studio Code](https://code.visualstudio.com/) is a powerful desktop editor for programming, with built-in support for version control and debugging. The large range of extensions for Visual Studio Code enable it to work with every popular programming language and framework. It is available free of charge.

The Microsoft releases of Visual Studio Code are proprietary software with telemetry enabled by default. To avoid these issues, use the packages that are provided by the [vscodium](https://github.com/VSCodium/vscodium) project instead.

Once you have installed Visual Studio Code or VSCodium, read [this article](https://www.stuartellis.name/articles/visual-studio-code/) for more information about using the editor.

### Neovim
If you would like a modern Vim editor with a good default configuration, [set up Neovim](https://www.stuartellis.name/articles/neovim-setup/).

### Setting The EDITOR Environment Variable
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

## Tmux (Terminal Multiplexer)
*[Tmux]: Terminal Multiplexer
[Tmux](https://github.com/tmux/tmux)
### Installation
**Mac OS**
```shell=zsh
# Install brew first then install tmux
ruby -e "$(curl -fsSL https://raw.github.com/mxcl/homebrew/go)"
brew update
brew install tmux
```
**Debian / Ubuntu**
```shell=
sudo apt-get install tmux
```
**RedHat / CentOS**
```
yum install tmux
```
安裝完跑 tmux，最下面有出現一條綠色的狀態列就是成功了

### session
每次下 tmux 指令時都會開啟一個新的 session，每個 session 各自獨立
### window
window 就是下圖整個終端機畫面，一個 session 裡面可以有多個 window
### pane
如下圖，一個 window 可以切成多個小區塊，每個區塊就是一個 pane，通常會用來同時觀察多個程式

### [Tmux Cheatsheet](https://gist.github.com/MohamedAlaa/2961058)

### TPM
[tpm](https://github.com/tmux-plugins/tpm) 是 tmux 的套件管理工具，英文是 Tmux Plugin Manager。tmux 之於 tpm 就如同是 nodejs 之於 npm.

回過頭來，我們想要用 tpm 來安裝一些好用的套件，先安裝好 tpm。 首先 git clone tpm 至本機。
```shell=
git clone https://github.com/tmux-plugins/tpm ~/.tmux/plugins/tpm
```

接著修改 ~/.tmux.conf 檔案的內容，將下列內容複製貼上至檔案中。
```shell=
# List of plugins
set -g @plugin 'tmux-plugins/tpm'
set -g @plugin 'tmux-plugins/tmux-sensible'
# Other examples:
# set -g @plugin 'github_username/plugin_name'
# set -g @plugin 'git@github.com/user/plugin'
# set -g @plugin 'git@bitbucket.com/user/plugin'
# Initialize TMUX plugin manager (keep this line at the very bottom of tmux.conf)
run -b '~/.tmux/plugins/tpm/tpm'
```
安裝完成後在終端機中輸入下列指令或是重開終端機即可載入完成。
```shell=
tmux source ~/.tmux.conf
```

### 使用 tpm 安裝 tmux 套件
安裝 tpm 套件可能跟你想的不太一樣，直接修改 `~/.tmux.conf` 檔案的內容，並且重啟。例如你想要安裝 tmux-copycat。
```shell=
vim ~/.tmux.conf
```
加入這一行
```shell=
set -g @plugin 'tmux-plugins/tmux-copycat'
```
接著，重開 tmux session 或是在 tmux 中輸入 prefix (ctrl+b) + I 即可。

### Remap prefix
ctrl+b 有點遠，修改 prefix 為 ctrl + a
tmux 大部分的指令是由組合鍵 prefix + 某某鍵所構成。 prefix 預設是 ctrl + b，外國鄉民大多是將 prefix 修改成 ctrl + a ，你也可以設成你喜歡的樣子，設定方法很簡單。在 `.tmux.con`f 中加上以下內容即可將 prefix 變成ctrl + a。
```shell=
# remap prefix from 'C-b' to 'C-a'
unbind C-b
set-option -g prefix C-a
bind-key C-a send-prefix
```

### 介紹幾個好用的套件
#### [tmux-yank](https://github.com/tmux-plugins/tmux-yank)
讓你的 tmux 可以用系統的剪貼簿，支援 osx, linux, WSL(Windows Subsystem for Linux) … 等等的環境。

#### [tmux-pain-control](https://github.com/tmux-plugins/tmux-pain-control)
這個套件有三大功能，第一是切割視窗，第二是在視窗中跳躍，第三是縮放視窗大小。
讓切割視窗變得更加簡單

這兩個是 tmux 預設的切割視窗快捷鍵
* prefix + "：進行水平分割
* prefix + %：進行垂直分割

在使用tmux-pain-control 之後：
* prefix + |：進行水平分割
* prefix + -：進行垂直分割

用 vim 的方向鍵跳轉視窗
* prefix + h：往左跳
* prefix + j：往上跳
* prefix + k：往下跳
* prefix + l：往右跳

用 vim 的方向鍵縮放視窗
* prefix + shift + h：視窗邊界往左移
* prefix + shift + j：視窗邊界往上移
* prefix + shift + k：視窗邊界往下移
* prefix + shift + l：視窗邊界往右移

#### [tmux copycat](https://github.com/tmux-plugins/tmux-copycat)
tmux copycat 可以讓你不使用滑鼠就能夠複製文字。這個套件非常好用，礙於時間不夠無法錄製完整操作。殘念。

#### [tmux-resurrect](https://github.com/tmux-plugins/tmux-resurrect)
讓系統重啟的時候保持 tmux 的設定。

#### [tmux-open](https://github.com/tmux-plugins/tmux-open)
能夠讓你快速的打開你選取到的文字對應的超連結或是檔案。

#### [tmux-prefix-highlight](https://github.com/tmux-plugins/tmux-prefix-highlight)
讓你知道你有沒有成功觸發 prefix，如果按成功了，在下方顯示列會有成功觸發的圖案。

#### [tmux-continuum](https://github.com/tmux-plugins/tmux-continuum)
持續幫你儲存 tmux 的設定。

#### [tmux-sensible](https://github.com/tmux-plugins/tmux-sensible)
讓 tmux 比較合乎邏輯，不然 tmux 有些預設的設定實在是有點難用，這是必備的 tmux 套件。

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

### Setting a GOPATH
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

## Java Development
### AdoptOpenJDK
Which Version of Java?
Many vendors provide a JDK. To avoid potential licensing and support issues, use the JDK that is provided by the [AdoptOpenJDK project](https://adoptopenjdk.net/). The versions of Java on the OpenJDK Website are for testers, and the Oracle JDK is a proprietary product that requires license fees.

Use the LTS version of the OpenJDK, unless you need features that are in the latest releases.

Once you have installed a JDK, get the [Apache Maven](https://maven.apache.org/) build tool. This is provided by the Maven project itself, and is not part of the OpenJDK.

Use [jEnv](https://www.jenv.be/) if you need to run multiple JDKs, such as different versions of the same JDK.

### Setting up Java with Homebrew
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

### Setting up jEnv
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

### Manual Set up of AdoptOpenJDK
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

:::info
:bulb: **Reference:** 
* https://www.stuartellis.name/articles/mac-setup/
* https://medium.com/starbugs/%E6%89%93%E9%80%A0-10x-engineer-zsh-shell-97e40db76391
* https://medium.com/starbugs/tpm-%E5%A5%97%E4%BB%B6%E7%AE%A1%E7%90%86%E5%B7%A5%E5%85%B7-%E8%AE%93%E4%BD%A0%E7%9A%84-tmux-%E6%9B%B4%E5%A5%BD%E7%94%A8-95ecd924c9d
* http://sourabhbajaj.com/mac-setup/
:::

:::info
:pushpin: Want to learn more? ➜ [HackMD Tutorials](https://hackmd.io/c/tutorials) 
:::
---

