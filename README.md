# WSL2 documentation

# About: Local development environment with Windows + WSL2

## Install on Powershell as Administrator (Reboot needed)
```powershell
PS C:\> wsl --install
```
## Define Version 2 of WSL as Default
```powershell
PS C:\> wsl --set-default-version 2
```
### Comparing WSL 1 and WSL 2 (documentation)
https://learn.microsoft.com/en-us/windows/wsl/compare-versions#whats-new-in-wsl-2

## Listing Distros
```powershell
PS C:\> wsl --list
PS C:\> wsl --list --running      ## Only running Distros
PS C:\> wsl --list --verbose      ## Verbose mode
```
## Converting a GNU/Linux Distro from WSL1 to WSL2
```powershell
PS C:\> wsl --set-version <distro-name> 2
```

## Running commands
```powershell
PS C:\> wsl ls ~
PS C:\> wsl cat /etc/os-release
```

## Running commands on a particular distro
```
> wsl -d <distro-name> ls ~
> wsl -d <distro-name> cat /etc/os-release
```

## Running commands on a particular distro with a specific user
```powershell
PS C:\> wsl whoami
PS C:\> wsl -u root whoami
```

## Stop a particular distro
```powershell
PS C:\> wsl --terminate <distro-name>
```

## Shutdown WSL and stop all distros
```powershell
PS C:\> wsl --shutdown
```

### WSL Configuration files
https://learn.microsoft.com/pt-br/windows/wsl/wsl-config

[ .wslconfig ]
[ wsl.conf ]

## Windows Terminal Shorcuts

### Windows Terminal JSON file settings
settings.json

### Create a new instance
CTRL + SHIFT + SPACEBAR

### Show the profile list to pick one
CTRL + SHIFT + T 

### Switch open instances
CTRL + TAB

### Launch the profile 1
CTRL + SHIFT + 1

### Launch the profile 2
CTRL + SHIFT + 2

### Switch to instance 1
CTRL + ALT + 1

### Switch to instance 2
CTRL + ALT + 2

### Close current instance
CTRL + SHIFT + W

### Open Windows Terminal Settings
CTRL + ,

### Split current pane horizontally
ALT + SHIFT + -

### Split current pane vertically
ALT + SHIFT + +

### Create a new instance of the profile from the current pane
ALT + SHIFT + D

### Open Palette with multiples options
CTRL + SHIFT + P

### Move between panes
ALT + cursor Up
ALT + cursor Down
ALT + cursor Left
ALT + cursor Right

### Resizing panes
ALT + SHIFT + cursor Up
ALT + SHIFT + cursor Down
ALT + SHIFT + cursor Left
ALT + SHIFT + cursor Right

## Access Linux Distro filesystem from Windows Explorer
```
\\wsl$
```
## Access Linux Distro filesystem from Powershell
```powershell
PS C:\> Get-Content '\\wsl$\Ubuntu\home\matheus\hello.txt'
```

## Running Linux applicaions from Windows
$env:SystemRoot = [ C:\Windows ]

```powershell
PS C:\> Get-Childitem $env:SystemRoot
PS C:\>  Get-Childitem $env:SystemRoot | ForEach-Object { $_.Name.Substring(0,1).ToUpper() }
PS C:\>  Get-Childitem $env:SystemRoot | ForEach-Object { $_.Name.Substring(0,1).ToUpper() } | wsl sort | wsl uniq -c
PS C:\>  Get-Childitem $env:SystemRoot | ForEach-Object { $_.Name.Substring(0,1).ToUpper() } | wsl bash -c "sort | uniq -c"
PS C:\>  wsl ls /usr/bin
PS C:\>  wsl bash -c "ls /usr/bin | cut -c1"
PS C:\>  wsl bash -c "ls /usr/bin | cut -c1-1" | Group-Object
```
```
## Accessing Windows files from Linux (WSL2)

NOTE: "Although the Linux side treats the file system as case-sensitive, the underlying Windows file system is still case-insensitive and it is important to keep this in mind."
```bash
$ cat /mnt/c/Users/mathe/lab/wsl2-book/hello.txt
```

## Accessing Windows applicaions from Linux (WSL2)
```bash
$ /mnt/c/Windows/System32/calc.exe
$ notepad.exe "c:\\Users\\mathe\\lab\\wsl2-book\\hello.txt"
```

## WSL will map your Windows environment variable PATH to the Linux PATH on all your WSL distros.
```bash
$ echo $PATH
```

## Retrive values from Windows Registry from Linux
```bash
$ powershell.exe -C "Get-ItemProperty -Path Registry::HKEY_LOCAL_MACHINE\HARDWARE\DESCRIPTION\System"
```

## 
```bash
echo "Matheus" | powershell.exe -noprofile -c 'Write-Host "Hello $input"'
```

## Using symlinks to make Windows path easier to access
```bash
$ ln -s /mnt/c/<custom-path>/ .
$ ln -s /mnt/c/Users/mathe/Downloads/ .
$ ln -s /mnt/c/Users/mathe/lab/ .
$ ln -s /mnt/c/Users/mathe/lab/vagrant/ .
```

## Start a default application from WSL distro with wsl utility
```bash
$ touch /home/matheus/teste.txt
$ sudo apt install wslu
$ wslview teste.txt 
$ wslview https://wsl.tips
```

## Translating PATHs from Linux format to Windows format
```bash
$ wslpath -w ~/teste.txt
\\wsl.localhost\Ubuntu\home\matheus\teste.txt

$ wslpath -w /mnt/c/Windows
C:\Windows
```

## Translating PATHs from Windows format to Linux format (single quotes needed)
```bash
$ wslpath -u '\\wsl.localhost\Ubuntu\home\matheus\teste.txt'
/home/matheus/teste.txt

$ wslpath -u 'C:\Windows'
/mnt/c/Windows
```

## Combining wslview with wslpath
```bash
$ wslview $(wslpath -w /home/matheus/teste.txt)
```

## Wrap all that into a function for ease of use
```bash
$ wslvieww() { wslview $(wslpath -w "$1"); };
```

## Calling this function
```bash
$ wslview /home/matheus/teste.txt
```

## Easy way to mapping Windows PATHs on WSL2
```bash
$ WIN_PROFILE=$(cmd.exe /C echo %USERPROFILE% 2>/dev/null)
$ WIN_PROFILE_MNT=$(wslpath -u ${WIN_PROFILE/[$'\r\n']})
$ ln -s $WIN_PROFILE_MNT/Downloads ~/Downloads
```

## Windows OpenSSH Authentication Agent
Windows key + R
Type it "services.msc" (quotes not needed)
Find OpenSSH Authentication Agent, open its Properties (right-click on it).
Make sure your settings look like this:
```
Startup Type: Automatic
Service Status is Running (click Start to do this =)
```

## Generate SSH key pair (If you don't already have one)
```bash
$ ssh-keygen -t rsa -b 4096
```

## Include your SSH private key on Windows OpenSSH Authentication Agent
```bash
$ ssh-add ~/.ssh/id_rsa
```

## If you have configured your SSH keys to be user to authenticate on Github, you can test it.
```bash
# On WLS2 Linux distro
$ ssh -T git@github.com
The authenticity of host 'github.com (20.201.28.151)' can't be established.
ED25519 key fingerprint is SHA256:+FiY3wmmM6TuWWhbpZisF/zDAPL0zPMSvHdkr4UvCOqU.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'github.com' (ED25519) to the list of known hosts.
Hi matheuscarino! You've successfully authenticated, but GitHub does not provide shell access.
```
```powershell
# On Powershell
PS C:\> ssh -T git@github.com
The authenticity of host 'github.com (20.201.28.151)' can't be established.
ED25519 key fingerprint is SHA256:+FiY3wmmM6TuWWhbpZisF/zDAPL0zPMSvHdkr4UvCOqU.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'github.com' (ED25519) to the list of known hosts.
Hi matheuscarino! You've successfully authenticated, but GitHub does not provide shell access.
```

## Rename Windows Terminal Tab on WSL distro
```bash
$ function set-prompt() { echo -ne '\033]0;' $@ '\a'; }
$ set-prompt "Novo Título"
```

## Rename Windows Terminal Tab on Powershell
```powershell
PS C:\> function Set-Prompt {

    param (

        # Specifies a path to one or more locations.

        [Parameter(Mandatory=$true,

                   ValueFromPipeline=$true)]

        [ValidateNotNull()]

        [string]

        $PromptText

    )

    $Host.UI.RawUI.WindowTitle = $PromptText

}
> Set-Prompt "Novo Título 2"
```

## Open Windows Terminal from command line with a custom tab title
```powershell
PS C:\> wt.exe -p "PowerShell" --title "This one is PowerShell"`; new-tab -p "Ubuntu-20.04" --title "WSL here!"
```

## Windows Terminal 
*****new-tab
*****split-pane

## Windows Terminal Custom profile (automatically connects via SSH)
```
            {
                "guid": "{9b0583cb-f2ef-4c16-bcb5-9111cdd626f3}",
                "hidden": false,
                "name": "Custom SSH Connection",
                "commandline": "wsl bash -c \"ssh <USER>@<FQDN-or-IPADDRESS>\"",
                "colorScheme": "Ubuntu-sl",
                "background": "#801720"
            },
```

## Install Docker Desktop for Windows
https://docs.docker.com/desktop/install/windows-install/
```bash
$ docker info
```

## Run a simple webserver in a container
```bash
$ docker run -d --name docker-nginx -p 8080:80 nginx
```

-d          tells Docker to run this container detached from our terminal (to run it in the background);
--name      specific the name for the container rather than generating a random one;
-p          map ports on the host to ports inside the running container;
nginx       the name of the container image to run (if no version is specified, the latest will be used);

Access your browser http://localhost:8080

## Listing Containers with status RUNNING
```bash
$ docker container ls
```

## Listing ALL Containers (running or not)
```bash
$ docker container ls -a
```

## Listing Container images
```bash
$ docker image ls
```

## Stop Container
```bash
$ docker stop docker-nginx
```

## Remove Container
```bash
$ docker rm docker-nginx
```

## Enabling bash completion for Kubernetes (restart shell needed)
```bash
$ echo 'source <(kubectl completion bash)' >>~/.bashrc
```

## Removing a WSL distro
```powershell
PS C:\> wsl --unregister <distro-name>
```

## 
```

```

## 
```

```

## 
```

```

## 
```

```

## 
```

```



## EXTRA DOCUMENTATION

Universal naming convention (UNC) paths
https://learn.microsoft.com/en-us/dotnet/standard/io/file-path-formats

Powershell Execution Policies
https://learn.microsoft.com/pt-pt/powershell/module/microsoft.powershell.core/about/about_execution_policies

WSL2-Linux-Kernel
https://github.com/microsoft/WSL2-Linux-Kernel

Connecting to GitHub with SSH
https://docs.github.com/pt/authentication/connecting-to-github-with-ssh

Testing your SSH connection
https://docs.github.com/pt/authentication/connecting-to-github-with-ssh/testing-your-ssh-connection

Install Docker Desktop on Windows
https://docs.docker.com/desktop/install/windows-install/

