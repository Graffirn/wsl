# Setup WSL

## 1. __Update packages__

```
sudo apt-get upgrade -y && sudo apt-get update -y
sudo apt-get install git
```

## 2. __Install zsh and oh-my-zsh__

Install zsh and set zsh as default shell
```
sudo apt install zsh -y
chsh -s $(which zsh)
```
Reopen shell, create empty file .zshrc

Install oh-my-zsh and its plugins
```
sh -c "$(wget https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh -O -)"

git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
```

Set ZSH_THEME="powerlevel10k/powerlevel10k" in ~/.zshrc

Add plugin to .zshrc
```
plugins=(git zsh-autosuggestions web-search zsh-syntax-highlighting)
```

Remove backgorund color of dircolor
```
dircolors -p | sed 's/;42/;01/' > ~/.dircolors
```
Add following lines to ~/.zshrc
```
if [ -x /usr/bin/dircolors ]; then
    test -r ~/.dircolors && eval "$(dircolors -b ~/.dircolors)" || eval "$(dircolors -b)"
    alias ls='ls --color=auto'
    #alias dir='dir --color=auto'
    #alias vdir='vdir --color=auto'

    alias grep='grep --color=auto'
    alias fgrep='fgrep --color=auto'
    alias egrep='egrep --color=auto'
fi
``` 

## 3. __Copy ssh keys__

Copy Windows ssh key to wsl
```
cp -r /mnt/c/Users/<username>/.ssh ~/.ssh
```
Fix permission & known_hosts
```
chmod 600 ~/.ssh/id_rsa
rm known_hosts
```

## 4. __Python__

Place the following line to ~/.zshrc
```
alias python=python3

```
Install pip
```
sudo apt install python3-pip
```
Install pipenv
```
pip install pipenv
```
Install pyenv
```
curl -L https://github.com/pyenv/pyenv-installer/raw/master/bin/pyenv-installer | bash

```