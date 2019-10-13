# Preparing a raspberry pi to rock

This is a random tutorial about the tools you need to install and manage when working with a raspberry pi.

## Installing NOOBS

1. Format the SD card using [SD Formatter](https://www.sdcard.org/downloads/formatter/index.html). If the card is bigger than 32BG it should be formated in FAT32 (msdos).

2. Download, unzip and copy NOOBS into the SD card.


## Pyenv

Pyenv is a tool that permits you to run different versions of python on the same machine. The main idea is that you configure a global python to run as default, and if you need a specific version for a project, you can also do it.

The following instructions were extracted from the [blog of Felipe Toscano](https://felipetoscano.com.br/gerenciando-versoes-python-com-pyenv-no-ubuntu/) (in PT-BR).

1 - First thing: install the dependencies

```
sudo apt-get install -y make build-essential libssl-dev && sudo apt-get install -y zlib1g-dev libbz2-dev libreadline-dev && sudo apt-get install -y libsqlite3-dev wget curl llvm libncurses5-dev libffi-dev git
```

2 - Download the git repository containing pyenv

```
git clone https://github.com/pyenv/pyenv.git ~/.pyenv
```

3 - Add the path for the pyenv so your terminal can find it easily.

```
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bashrc
echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bashrc
```

4 - Add *pyenv init* on your shell to activate shims and autofill. 

```
echo -e 'if command -v pyenv 1>/dev/null 2>&1; then\n  eval "$(pyenv init -)"\nfi' >> ~/.bashrc
```

5 - To make it work.

```
source ~/.bashrc
```


6 - Test if everything is ok.

```
pyenv
```

### Commands 

You can check the versions of python in your system.

```
pyenv versions

* system (set by /home/felipe/.pyenv/version)
```

You can list all the versions available for download.

```
pyenv install -l
```

And of course you can install new versions of python.

```
pyenv install 3.6.2
```

After installing you still need to change the global python.

```
pyenv global 3.6.2
```

If you want to use some specific version of python in a project, use the local command.

```
pyenv local 2.7.1
```

## MongoBB

Inspired by [this guy](https://thedatafrog.com/mongodb-remote-raspberry-pi/)

1 - Install mongodb.

```
sudo apt install mongodb
```

2 - Put the server to run.

```
sudo systemctl enable mongodb
```

3 - Test it.

```
mongo
```

```
> use test
switched to db test
> db.col.insert({a:1})
> db.col.insert({a:2})
> db.col.find()
{ "_id" : ObjectId("5ce6ad86dace0cba25fa3458"), "a" : 1 }
{ "_id" : ObjectId("5ce6ad8bdace0cba25fa3459"), "a" : 2 }
```

## Openning MongoDB to the network

1 - Install *lsof* to watch the doors of the pi.

```
sudo apt install lsof
```

Then do

```
sudo lsof -i -P -n | grep LISTEN
```

You might see the 2 doors for the mongodb open only for local connections (127.0.0.1) at the ports 27017 and 27017. Check if the ssh is also open in your pi for external connections.

To avoid using iptables, install *ufw* to manage the firewall and open the doors. Be careful as the data base is not secured!

```
sudo apt install ufw
```

Open the doors! (including the ssh here)

```
sudo ufw allow ssh
sudo ufw allow 27017
sudo ufw allow 28017
```

Enable *ufw* to apply the rules and run. Use status command to check if it is running.

```
sudo ufw enable
sudo ufw status
```

Now fix the connection in the mongodb configuration file.

```
sudo nano /etc/mongodb.conf 
```

Comment the following line:

```
# bind_ip = 127.0.0.1
```

Reinstall the bd server.

```
sudo systemctl restart mongodb
```

To test it, go to your browser (in another computer) and check the following page:

```
http://192.168.0.108:28017/
```

If you want to test the remote access use the terminal:

```
mongo 192.168.0.108
```