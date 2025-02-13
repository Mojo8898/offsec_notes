# Proper System Management

No, Linux is not out to get you. It's a skill issue. It always is.

## Git Repositories

I prefer to git clone to `/opt/` as root and symbolic link to `/usr/local/bin`. See the following example.

```powershell
sudo git clone https://github.com/dirkjanm/krbrelayx.git /opt/krbrelayx
sudo ln -s /opt/krbrelayx/krbrelayx.py /usr/local/bin/krbrelayx.py
```

Although repositories such as krbrelayx require dependencies, this can be solved with the following methods.

## Python

Stop clobbering your system with python installations.

### apt

Kali now supports installing python packages globally while maintaining version differences. Say you need to install the `impacket` package, instead of installing with `pip`, use the following command. Not all python packages are supported with this method but this will work for the majority of projects and should be utilized when possible to centralize package management.

```powershell
# Install impacket
sudo apt install python3-impacket

# Install most other python3 packages
sudo apt install python3-$PACKAGE
```

### pipx

```powershell
# Install the github repository with pipx to keep it containerized
pipx install git+https://github.com/Pennyw0rth/NetExec.git

# Use the following example to run the tool with sudo
sudo $(which nxc) $PARAMS
```

### venv

```powershell
# Enter the python repository
cd /opt/pypykatz

# Create the virtual environment
python3 -m venv venv

# Enter the virtual environment
source venv/bin/activate

# Install the package
python3 setup.py install
```

**Note:** Use `ipython3` for debugging/executing blocks on the fly
