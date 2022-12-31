# Setup WSL

## 1. __Update packages__

Update package manager
```
sudo apt-get upgrade -y && sudo apt-get update -y
sudo apt-get install git
```

Install additional libs
```
sudo apt-get install build-essential libssl-dev libffi-dev python3-dev python3-pip libsasl2-dev libldap2-dev default-libmysqlclient-dev libsqlite3-dev liblzma-dev tk-dev libreadline-dev libbz2-dev libncurses-dev
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

In order to get rid of underline of zsh-syntax-highlighting, add this to ~/.zshrc
```
ZSH_HIGHLIGHT_STYLES[path]=none
ZSH_HIGHLIGHT_STYLES[path_prefix]=none
```

## 3. __Copy ssh keys__

Copy Windows ssh key to wsl
```
cp -r /mnt/c/Users/<username>/.ssh/* ~/.ssh
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

Add pyenv to PATH
```
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.zshrc
echo 'command -v pyenv >/dev/null || export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.zshrc
echo 'eval "$(pyenv init -)"' >> ~/.zshrc
```

## 5. __Kubernetes__

Install kubectl
```
sudo curl -fsSLo /etc/apt/keyrings/kubernetes-archive-keyring.gpg https://packages.cloud.google.com/apt/doc/apt-key.gpg
echo "deb [signed-by=/etc/apt/keyrings/kubernetes-archive-keyring.gpg] https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list
sudo apt-get update
sudo apt-get install -y kubectl
```

Install k9s
```
curl -sS https://webinstall.dev/k9s | bash
source ~/.config/envman/PATH.env
```

## 6. __Docker__

Create volumn for mysql
```
sudo docker volume create mysql-data
```

Create a container run mysql
```
sudo docker run --name=mysql-docker -p 3306:3306 -v mysql-data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=admin -e MYSQL_USER=huynkq -e MYSQL_PASSWORD=123456 -d mysql/mysql-server:latest
```

By default, MySQL restricts connection other than the local machine for security reasons. To connect from the other machines, you have to change the connection restriction:
```
docker exec -it mysql-docker bash
# Connect to mysql server
mysql -u root -p

# Allow access from all hosts
UPDATE mysql.user SET host = ‘%’;
GRANT ALL PRIVILEGES ON *.* TO 'huynkq'@'%';
FLUSH PRIVILEGES;
```

Start a redis instance
```
sudo docker run --name redis-docker -p 6379:6379 -d redis
```