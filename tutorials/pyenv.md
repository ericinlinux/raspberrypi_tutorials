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

