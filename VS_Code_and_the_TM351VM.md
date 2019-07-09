# Accessing the TM351 VM Jupyter Environment from Microsoft VS Code

The Microsoft VS Code Editor is an integrated development environment that can be run locally as a desktop electron app or accessed as a web application via a web browser.

VS Code supports Jupyter notebooks and connections to Jupyter servers and can be used to edit notebooks and code files executued against a remote Jupyter kernel (docs: [Working with Jupyter Notebooks in Visual Studio Code](https://code.visualstudio.com/docs/python/jupyter-support)).

At the current time, running VS Code against the TM351 VM Jupyter server requires some minor set-up tweaks:

### In VS Code:

- install the Python extension

### In the VM Vagrantfile:

Just before` #---- END PORT FORWARDING ----` line, open up a convenience port and then `vagrant reload` to kickk the VM into accepting that port:

```
## Convenience port
config.vm.network :forwarded_port, guest: 35199, host: 35199, auto_correct: true
```

### In the VM:

Launch a terminal: from the notebook homepage, select `New > terminal`.

Start a notebook server:

Here's what I've tried so far (which is trying to get the notebook server as wide open and  as little security minded as possible!)

```
cd ~
jupyter notebook --port 35199 --ip 0.0.0.0 --no-browser --NotebookApp.allow_origin='*' --NotebookApp.allow_remote_access=True --NotebookApp.token='letmein'
```

*(Actually, it probably makes more sense to cd ~/notebooks before starting the server? We could probably alternativel tweak the default notebook service definition to allow the * origin and remote access and then we could just connect via the original default server on port 8888.)*

### In VS Code:

Select the command palette (cogwheel, bottom left corner), then:

```
Python: Specify Jupyter server URI
```

The details for the URI to connect to a running Jupyter server are:

```
http://localhost:35199?token=letmein
```

Then from the `Command Palette`:

```
Python Show Python Interactive Window
```

If that doesn't work, go to:

```Files > Preferences > Settings```

and change the `Data Science : Jupyter Server URI` from "local" to the correct URI.

## Running a Remote Jupyter Server on Digital Ocean

As well as connecting to a TM351 VM Jupytr server from VS Code, you can also connect to remote Jupyter servers running in the cloud.

For example, here are some instructions for [Connecting to a Remote Jupyter Notebook Server Running on Digital Ocean from Microsoft VS Code](https://blog.ouseful.info/2019/02/11/connecting-to-a-remote-jupyter-notebook-server-running-on-digital-ocean-from-microsoft-vs-code/).
