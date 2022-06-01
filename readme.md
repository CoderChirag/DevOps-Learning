# Windows Tools

#### Install chocolatey from the instructions given in the link below.

https://chocolatey.org/docs/installation

#### Run all the below commands on Powershell (Open Powershell as Admin)

> choco install virtualbox

> choco install vagrant

> choco install git

> choco install jdk8

> choco install maven

> choco install awscli

> choco install intellijidea-ultimate

> choco install sublimetext3.app

# MacOS Tools

#### Install brew from the instructions given in the link below.

https://brew.sh/

#### After installing homebrew

Create a file in users home directory with name .curlrc with content “-k”
(-k without quotes and give a new line character after -k.)

**Steps:**

1. Open Terminal
2. echo -k > ~/.curlrc
3. cat ~/.curlrc

#### Run all the below commands in Terminal

> brew install --cask virtualbox

> brew install --cask vagrant

> brew install --cask vagrant-manager

> brew install git

> brew install openjdk@8

> brew install openjdk@8

> brew install maven

> brew install --cask intellij-idea

> brew install --cask intellij-idea-ce

> brew install --cask sublime-text

> brew install awscli

# Ubuntu 20 Tools

#### Install Virtualbox

> $ sudo apt update

> $ sudo apt install virtualbox

#### Install Vagrant

> $ curl -O https://releases.hashicorp.com/vagrant/2.2.9/vagrant_2.2.9_x86_64.deb

> $ sudo apt install ./vagrant_2.2.9_x86_64.deb

#### Install Git

> $ apt install git

#### Install jdk8

> $ sudo apt-get install openjdk-8-jdk

#### Install Maven

> $ sudo apt-get install maven

#### Install awscli

> $ sudo apt-get install awscli

#### Install Intellij community

> $ sudo snap install intellij-idea-community --classic

#### Install Sublime Text

> $ sudo apt update

> $ sudo apt install dirmngr gnupg apt-transport-https ca-certificates software-properties-common

> $ curl -fsSL https://download.sublimetext.com/sublimehq-pub.gpg | sudo apt-key add -

> $ sudo add-apt-repository "deb https://download.sublimetext.com/ apt/stable/"

> $ sudo apt install sublime-text
