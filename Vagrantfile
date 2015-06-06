# -*- mode: ruby -*-
# vi: set ft=ruby :

$clitools = <<SCRIPT 
#!/bin/bash

# install Xcode Command Line Tools
git --help >/dev/null 2>&1
if [ $? -gt 0 ]; then
  # create the placeholder file that's checked by CLI updates' .dist code 
  # in Apple's SUS catalog
  touch /tmp/.com.apple.dt.CommandLineTools.installondemand.in-progress
  # find the CLI Tools update
  PROD=$(softwareupdate -l | grep "\*.*Command Line" | head -n 1 | awk -F"*" '{print $2}' | sed -e 's/^ *//' | tr -d '\n')
  # install it
  softwareupdate -i "$PROD" -v
  rm /tmp/.com.apple.dt.CommandLineTools.installondemand.in-progress
fi
SCRIPT

$script = <<SCRIPT
# install homebrew
which brew >/dev/null
if [ $? -gt 0 ]; then
  ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
  brew doctor
fi

# install caskroom.io
if [ ! -d /usr/local/Library/Taps/caskroom/homebrew-cask ]; then
  brew tap caskroom/cask
  brew install brew-cask
fi

brew tap caskroom/versions

# german timezone
sudo systemsetup -settimezone Europe/Berlin
# german language
sudo languagesetup -langspec de
# read keyboard layout
# defaults read /Library/Preferences/com.apple.HIToolbox.plist AppleEnabledInputSources

sudo systemsetup -setsleep Never

brew install nvm

export NVM_DIR=~/.nvm
source $(brew --prefix nvm)/nvm.sh

cat >~/.bash_profile <<EOF
export NVM_DIR=~/.nvm
source $(brew --prefix nvm)/nvm.sh
EOF

nvm install 0.12
nvm use 0.12
nvm alias default 0.12

git clone https://github.com/kitematic/kitematic
cd kitematic
npm install
SCRIPT

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.define :"kitematic-box" do |box|
    # box.vm.box = "osx1010-desktop"
    box.vm.box = "osx109"
    box.ssh.forward_agent = true
    box.ssh.insert_key = false
    box.vm.hostname = "kitematic"

    box.vm.provision "shell", inline: $clitools
    box.vm.provision "shell", inline: $script, privileged: false

    ["vmware_fusion", "vmware_workstation"].each do |provider|
      box.vm.provider provider do |v, override|
        v.gui = true
        v.vmx["memsize"] = "6144"
        v.vmx["numvcpus"] = "4"
        v.vmx["vhv.enable"] = "TRUE"
      end
    end
  end
end
