# Setup a New Developer Computer
This script will help with the quick setup and installation of tools and applications for new developers. 
This script works on both Intel and M1/M2/M3 Macs. 

You can run this script multiple times without issue. You can also run it on a partially set-up computer and it will only install what is missing.

The script will create/modify `.bash_profile` and `.zprofile` with path and autocomplete sources. If you do run it on an already set-up computer, please check these files for any duplicated paths/imports/etc.

<br>

## Quick Install Instructions

Paste the command below in a Mac OS Terminal:
```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/ECOSurv/new-computer-setup/main/setup-new-computer.sh)"
```

## Manual Install Instructions

* Download the script `setup-new-computer.sh` to your home folder
* Open Terminal and navigate to where you saved it
* Make the script executable:
   ```sh
   chmod +x ./setup-new-computer.sh
   ```
* Run the script:
   ```sh
   ./setup-new-computer.sh
   ```

* Some installs will need your password
* You will be prompted to fill out your git email and name. Use the email and name you use for Github


<br><br>


## Post Installation Instructions
After you have run the script, please complete the following steps to finish setting up your computers:

   
1. **Github Command-line SSH Authentication**\
   Git is now configured to use SSH by default for github urls. You will need to generate and add an SSH key to your Github account or you will run into errors. Do the following to authorize Github on your computer:
   - [Generate an SSH key for your new computer][generate key]
   - [Add the SSH public key to your Github account][add to github]
     
   [generate key]: https://help.github.com/en/github/authenticating-to-github/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent
   [add to github]: https://help.github.com/en/github/authenticating-to-github/adding-a-new-ssh-key-to-your-github-account
  

<br><br>


## Post Installation Tips

**Fix ZSH Errors**\
If you are using ZSH as your shell (default in newer Mac OS versions) you may get this error after running the setup script:
 
> zsh compinit: insecure directories, run compaudit for list.\
> Ignore insecure directories and continue [y] or abort compinit [n]?

You can fix this by running the following command in your terminal:
```sh
compaudit | xargs chmod g-w
```

<br>

**Setting Up Pycharm with Python 2.7**\
As Mac OS has recently removed the bundled copy of Python 2.7

<br>

  
**Keeping your tools up-to-date**\
Homebrew can keep your command-line tools and languages up-to-date.
```sh
# List what needs to be updated
brew update
brew outdated
 
# Upgrade a specific app/formula (example: git)
brew upgrade git

# Upgrade everything
brew upgrade
  
# List previous versions installed (example: git)
brew switch git list
 
# Roll back to a currently installed previous version (example: git 2.25.0)
brew switch git 2.25.0
```


<br>
<br>

---
## What's Installed

### Shell Profile Setup (Bash and Zsh)
<details>
  <summary>.bash_profile</summary>
  The following will be added to your ~/.bash_profile
	
   ```sh
# --------------------------------------------------------------------
# Begin Bash autogenerated content from setup-new-computer.sh   $VERSION
# --------------------------------------------------------------------

# Supress "Bash no longer supported" message
export BASH_SILENCE_DEPRECATION_WARNING=1

# Start Homebrew
if [[ "$(uname -p)" == "arm" ]]; then
    # Apple Silicon M1/M2 Macs
    eval "$(/opt/homebrew/bin/brew shellenv)"
else
    # Intel Macs
    eval "$(/usr/local/bin/brew shellenv)"
fi

# Bash Autocompletion
if type brew &>/dev/null; then
  HOMEBREW_PREFIX="$(brew --prefix)"
  if [[ -r "${HOMEBREW_PREFIX}/etc/profile.d/bash_completion.sh" ]]; then
    source "${HOMEBREW_PREFIX}/etc/profile.d/bash_completion.sh"
  else
    for COMPLETION in "${HOMEBREW_PREFIX}/etc/bash_completion.d/"*; do
      [[ -r "$COMPLETION" ]] && source "$COMPLETION"
    done
  fi
fi

# Google Cloud SDK
[ -e "$(brew --prefix)/Caskroom/google-cloud-sdk/latest/google-cloud-sdk/path.bash.inc" ] && 
	source "$(brew --prefix)/Caskroom/google-cloud-sdk/latest/google-cloud-sdk/path.bash.inc"
[ -e "$(brew --prefix)/Caskroom/google-cloud-sdk/latest/google-cloud-sdk/completion.bash.inc" ] && 
	source "$(brew --prefix)/Caskroom/google-cloud-sdk/latest/google-cloud-sdk/completion.bash.inc"

# NVM
# This needs to be after "Setting up Path for Homebrew" to override Homebrew Node
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && . "$NVM_DIR/nvm.sh"  # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && . "$NVM_DIR/bash_completion"  # This loads nvm bash_completion

# Node
# Increases the default memory limit for Node, so larger Anglar prjects can be built
export NODE_OPTIONS=--max_old_space_size=12000

# Update Node to selected version and reinstall previous packages
node-upgrade() {
    new_version=${1:?"Please specify a version to upgrade to. Example: node-upgrade 18"}
    nvm install "$new_version" --reinstall-packages-from=current
    nvm alias default "$new_version"
    # nvm uninstall "$prev_ver"
    nvm cache clear
}

# --------------------------------------------------------------------
# End autogenerated content from setup-new-computer.sh   $VERSION
# --------------------------------------------------------------------
   ```
</details>

<details>
  <summary>.zprofile</summary>
  The following will be added to your ~/.zprofile
	
   ```sh
# --------------------------------------------------------------------
# Begin ZSH autogenerated content from setup-new-computer.sh   $VERSION
# --------------------------------------------------------------------

# Start Homebrew
if [[ "$(uname -p)" == "arm" ]]; then
    # Apple Silicon M1/M2 Macs
    eval "$(/opt/homebrew/bin/brew shellenv)"
else
    # Intel Macs
    eval "$(/usr/local/bin/brew shellenv)"
fi

# Brew Autocompletion
if type brew &>/dev/null; then
    fpath+=$(brew --prefix)/share/zsh/site-functions
fi

# Zsh Autocompletion
# Note: must run after Brew Autocompletion
autoload -U +X compinit && compinit
autoload -U +X bashcompinit && bashcompinit
fpath=(/usr/local/share/zsh-completions $fpath)

# --------------------------------------------------------------------
# End autogenerated content from setup-new-computer.sh   $VERSION
# --------------------------------------------------------------------
   ```
</details>


### Command-line tools and languages
<details>
  <summary>Xcode CLI Development Tools</summary>
  
   ```sh
   xcode-select --install
   ```
</details>


<details>
  <summary>Homebrew (brew)</summary>
  
   ```sh
   /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
   
   # Fix brew insecure directories warning (zsh)
   chmod go-w "$(brew --prefix)/share"
   ```
</details>





<details>
  <summary>Zsh Completions</summary>
  
   ```sh
   brew install zsh-completions
   ```
</details>


<details>
  <summary>Git</summary>
  
   ```sh
   brew install git
   ```

   [git](https://git-scm.com/)
</details>

 
 
 ### Languages

<details>
  <summary>Ruby</summary>
  
```sh
brew install ruby
```

[ruby](https://www.ruby-lang.org/en/) 
</details>
	   


### Applications

<details>
  <summary>Google Chrome</summary>
  
```sh
brew install --cask google-chrome
```
</details>


<details>
  <summary>Docker for Mac</summary>

```sh
brew install --cask docker
```
</details>


<details>
  <summary>Postman</summary>

```sh
brew install --cask postman
```
</details>


### Optional IDEs and Tools

<details>
  <summary>Visual Studio Code</summary>
  
```sh
brew install --appdir="/Applications" --cask visual-studio-code
```
</details>

<details>
  <summary>PhpStorm</summary>
  
```sh
brew install --appdir="/Applications" --cask phpstorm
```
</details>


<details>
  <summary>Pycharm</summary>
  
```sh
brew install --appdir="/Applications" --cask pycharm
```
</details>


<details>
  <summary>Postman</summary>
  
```sh
brew install --appdir="/Applications" --cask Postman
```
</details>

<details>
  <summary>Insomnia</summary>
  
```sh
brew install --appdir="/Applications" --cask mnnsomnia
```
</details>

<details>
  <summary>iTerm2</summary>
  
```sh
brew install --appdir="/Applications" --cask iterm2
```
</details>

<details>
  <summary>Keybase</summary>
  
```sh
brew install --appdir="/Applications" --cask keybase
```
</details>

<details>
  <summary>Sequel Ace</summary>
  
```sh
brew install --appdir="/Applications" --cask sequel-ace
```
</details>


### System Tweaks

<details>
  <summary>General: Expand save and print panel by default</summary>
  
```sh
defaults write NSGlobalDomain NSNavPanelExpandedStateForSaveMode -bool true
defaults write NSGlobalDomain NSNavPanelExpandedStateForSaveMode2 -bool true
defaults write NSGlobalDomain PMPrintingExpandedStateForPrint -bool true
defaults write NSGlobalDomain PMPrintingExpandedStateForPrint2 -bool true
```
</details>

<details>
  <summary>General: Save to disk (not to iCloud) by default</summary>
  
```sh
defaults write NSGlobalDomain NSDocumentSaveNewDocumentsToCloud -bool false
```
</details>

<details>
  <summary>General: Avoid creating .DS_Store files on network volumes</summary>
  
```sh
defaults write com.apple.desktopservices DSDontWriteNetworkStores -bool true
```
</details>

<details>
  <summary>Typing: Disable smart quotes and dashes as they cause problems when typing code</summary>
  
```sh
defaults write NSGlobalDomain NSAutomaticQuoteSubstitutionEnabled -bool false
defaults write NSGlobalDomain NSAutomaticDashSubstitutionEnabled -bool false
```
</details>

<details>
  <summary>Typing: Disable press-and-hold for keys in favor of key repeat</summary>
  
```sh
defaults write NSGlobalDomain ApplePressAndHoldEnabled -bool false
```
</details>

<details>
  <summary>Finder: Show status bar and path bar</summary>
  
```sh
defaults write com.apple.finder ShowStatusBar -bool true
defaults write com.apple.finder ShowPathbar -bool true	
```
</details>

<details>
  <summary>Finder: Disable the warning when changing a file extension</summary>
  
```sh
defaults write com.apple.finder FXEnableExtensionChangeWarning -bool false
```
</details>

<details>
  <summary>Finder: Show the ~/Library folder</summary>
  
```sh
chflags nohidden ~/Library
```
</details>

<details>
  <summary>Safari: Enable Safari’s Developer Settings</summary>
  
```sh
defaults write com.apple.Safari IncludeInternalDebugMenu -bool true
defaults write com.apple.Safari IncludeDevelopMenu -bool true
defaults write com.apple.Safari WebKitDeveloperExtrasEnabledPreferenceKey -bool true
defaults write com.apple.Safari com.apple.Safari.ContentPageGroupIdentifier.WebKit2DeveloperExtrasEnabled -bool true
defaults write NSGlobalDomain WebKitDeveloperExtras -bool true
```
</details>

<details>
  <summary>Chrome: Disable the all too sensitive backswipe on Trackpads and Magic Mice</summary>

```sh
# Note: The chrome defaults can cause your Chrome browser to display a message stating
# that Chrome is "Managed by your organization" when it isn't
# 
# To view policies that are affecting this message, view the following pages:
# chrome://policy and chrome://management/
# 
# To quickly remove Chrome default overrides, run the following commands:
# defaults delete com.google.Chrome
# defaults delete com.google.Chrome.canary
#
defaults write com.google.Chrome AppleEnableSwipeNavigateWithScrolls -bool false
defaults write com.google.Chrome.canary AppleEnableSwipeNavigateWithScrolls -bool false
defaults write com.google.Chrome AppleEnableMouseSwipeNavigateWithScrolls -bool false
defaults write com.google.Chrome.canary AppleEnableMouseSwipeNavigateWithScrolls -bool false	
```
</details>

<details>
  <summary>Chrome: Use the system print dialog and expand dialog by default</summary>
  
```sh
# Note: The chrome defaults can cause your Chrome browser to display a message stating
# that Chrome is "Managed by your organization" when it isn't
# 
# To view policies that are affecting this message, view the following pages:
# chrome://policy and chrome://management/
# 
# To quickly remove Chrome default overrides, run the following commands:
# defaults delete com.google.Chrome
# defaults delete com.google.Chrome.canary
#
defaults write com.google.Chrome DisablePrintPreview -bool true
defaults write com.google.Chrome.canary DisablePrintPreview -bool true
defaults write com.google.Chrome PMPrintingExpandedStateForPrint2 -bool true
defaults write com.google.Chrome.canary PMPrintingExpandedStateForPrint2 -bool true
```
</details>



### Set up Git

<details>
  <summary>Configure git to always ssh when dealing with https github repos</summary>
  
```sh
git config --global url."git@github.com:".insteadOf https://github.com/
# you can remove this change by editing your ~/.gitconfig file
```
</details>


<details>
  <summary>Set Git to store credentials in Keychain</summary>
  
```sh
git config --global credential.helper osxkeychain
```
</details>


<details>
  <summary>Set git display name and email</summary>
  
```sh
if [ -n "$(git config --global user.email)" ]; then
  echo "✔ Git email is set to $(git config --global user.email)"
else
  read -p 'What is your Git email address?: ' gitEmail
  git config --global user.email "$gitEmail"
fi

if [ -n "$(git config --global user.name)" ]; then
  echo "✔ Git display name is set to $(git config --global user.name)"
else
  read -p 'What is your Git display name (Firstname Lastname)?: ' gitName
  git config --global user.name "$gitName"
fi
```
</details>


<br>
	   
---   

<br>
