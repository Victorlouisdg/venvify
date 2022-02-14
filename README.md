# venvify
Python package to turn Python installations into venv-like environments that you can activate.

Say you have the following python installation:

```
python_env
|- bin
|   |- python3.9
|- include
|- lib
```

Then you can venvify it by running the following script:
```
venvify ~/python_env
```

The result:
```
python_env
|- bin
|   |- python3.9
|   |- python (symlink)
|   |- python3 (symlink)
|   |- activate
|   |- activate.sh
|   |- etc.
|- include
|- lib
```
Now you can source the `activate` file to use the environment as you would use a venv:
```console
user@laptop: source python_env/bin/activate
(python_env) user@laptop: python -m pip install <some_package>
``` 

To undo the venvifying, simply remove the `activate` scripts and the symlinks from the `bin` directory.

## Installation
All the logic of the package can be found in a single script `venvify.py`.
The only dependency is the [venv](https://docs.python.org/3/library/venv.html) Python standard library package.
`venv` should be shipped with Python 3, but can be installed with `sudo apt install python3-venv`.

So if you don't want to pip install, you can clone this repo and simply run `venvify.py` with any Python interpreter.

However if you don't mind you can:
```
pip install venvify
```
And then run the `venvify` command.

## Use case: Blender Python environment
I made this package because, when developing python scripts for Blender, you often need to use its Python interpreter.
After venvifying the Blender python installation and activating it, you can simply use it as `python` e.g. to install 
additional packages: `python -m pip install blenderproc` will install BlenderProc into Blender's Python installation.

Furthermore, some package like BlenderProc come with command line entry points e.g.
`blenderproc run <script.py>` etc. When the blender python env is activated, these commands are easily avialable just
like in a real venv.

Example of how to venvify Blender's Python:
```
cd ~/blender-3.0.0-linux-x64/3.0/python/bin
./python3.9 -m ensurepip
./pip3 install venvify
./venvify .. --env_name blender
source activate
```
