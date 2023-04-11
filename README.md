# WSL2 Book documentation

Git repo: https://github.com/PacktPublishing/Windows-Subsystem-for-Linux-2-WSL-2-Tips-Tricks-and-Techniques

## Install on Powershell as Administrator (Reboot needed)
```
> wsl --install
```
## Define Version 2 of WSL as Default
```
> wsl --set-default-version 2
```
### Comparing WSL 1 and WSL 2 (documentation)
https://learn.microsoft.com/en-us/windows/wsl/compare-versions#whats-new-in-wsl-2
## Listing Distros
```
> wsl --list
> wsl --list --running      ## Only running Distros
> wsl --list --verbose      ## Verbose mode
```
## Converting a GNU/Linux Distro from WSL1 to WSL2
```
> wsl --set-version <distro-name> 2
```

## Running commands
```
> wsl ls ~
> wsl cat /etc/os-release
```

## Running commands on a particular distro
```
> wsl -d <distro-name> ls ~
> wsl -d <distro-name> cat /etc/os-release
```

## Running commands on a particular distro with a specific user
```
> wsl whoami
> wsl -u root whoami
```

## Stop a particular distro
```
> wsl --terminate <distro-name>
```

## Shutdown WSL and stop all distros
```
> wsl --shutdown
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
```
> Get-Content '\\wsl$\Ubuntu\home\matheus\hello.txt'
```

## Running Linux applicaions from Windows
$env:SystemRoot = [ C:\Windows ]

```
> Get-Childitem $env:SystemRoot
> Get-Childitem $env:SystemRoot | ForEach-Object { $_.Name.Substring(0,1).ToUpper() }
> Get-Childitem $env:SystemRoot | ForEach-Object { $_.Name.Substring(0,1).ToUpper() } | wsl sort | wsl uniq -c
> Get-Childitem $env:SystemRoot | ForEach-Object { $_.Name.Substring(0,1).ToUpper() } | wsl bash -c "sort | uniq -c"
> wsl ls /usr/bin
> wsl bash -c "ls /usr/bin | cut -c1"
> wsl bash -c "ls /usr/bin | cut -c1-1" | Group-Object
```
```
## Accessing Windows files from Linux (WSL2)

NOTE: "Although the Linux side treats the file system as case-sensitive, the underlying Windows file system is still case-insensitive and it is important to keep this in mind."
```
$ cat /mnt/c/Users/mathe/lab/wsl2-book/hello.txt
```

## Accessing Windows applicaions from Linux (WSL2)
```
$ /mnt/c/Windows/System32/calc.exe
$ notepad.exe "c:\\Users\\mathe\\lab\\wsl2-book\\hello.txt"
```

## WSL will map your Windows environment variable PATH to the Linux PATH on all your WSL distros.
```
$ echo $PATH
```

## Retrive values from Windows Registry from Linux
```
$ powershell.exe -C "Get-ItemProperty -Path Registry::HKEY_LOCAL_MACHINE\HARDWARE\DESCRIPTION\System"
```

## 
```
echo "Matheus" | powershell.exe -noprofile -c 'Write-Host "Hello $input"'
```

## Using symlinks to make Windows path easier to access
```
$ ln -s /mnt/c/<custom-path>/ .
$ ln -s /mnt/c/Users/mathe/Downloads/ .
$ ln -s /mnt/c/Users/mathe/lab/ .
$ ln -s /mnt/c/Users/mathe/lab/vagrant/ .
```

## Start a default application from WSL distro with wsl utility
```
$ touch /home/matheus/teste.txt
$ sudo apt install wslu
$ wslview teste.txt 
$ wslview https://wsl.tips
```

## Translating PATHs from Linux format to Windows format
```
$ wslpath -w ~/teste.txt
\\wsl.localhost\Ubuntu\home\matheus\teste.txt

$ wslpath -w /mnt/c/Windows
C:\Windows
```

## Translating PATHs from Windows format to Linux format (single quotes needed)
```
$ wslpath -u '\\wsl.localhost\Ubuntu\home\matheus\teste.txt'
/home/matheus/teste.txt

$ wslpath -u 'C:\Windows'
/mnt/c/Windows
```

## Combining wslview with wslpath
```
$ wslview $(wslpath -w /home/matheus/teste.txt)

```

## Wrap all that into a function for ease of use
```
$ wslvieww() { wslview $(wslpath -w "$1"); };
```

## Calling this function
```
$ wslview /home/matheus/teste.txt
```

## Easy way to mapping Windows PATHs on WSL2
```
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
```
$ ssh-keygen -t rsa -b 4096
```

## Include your SSH private key on Windows OpenSSH Authentication Agent
```
$ ssh-add ~/.ssh/id_rsa
```

## If you have configured your SSH keys to be user to authenticate on Github, you can test it.
```
# On WLS2 Linux distro
$ ssh -T git@github.com
The authenticity of host 'github.com (20.201.28.151)' can't be established.
ED25519 key fingerprint is SHA256:+FiY3wmmM6TuWWhbpZisF/zDAPL0zPMSvHdkr4UvCOqU.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'github.com' (ED25519) to the list of known hosts.
Hi matheuscarino! You've successfully authenticated, but GitHub does not provide shell access.

# On Powershell
PS C:\Users\mathe> ssh -T git@github.com
The authenticity of host 'github.com (20.201.28.151)' can't be established.
ED25519 key fingerprint is SHA256:+FiY3wmmM6TuWWhbpZisF/zDAPL0zPMSvHdkr4UvCOqU.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'github.com' (ED25519) to the list of known hosts.
Hi matheuscarino! You've successfully authenticated, but GitHub does not provide shell access.
```

## Rename Windows Terminal Tab on WSL distro
```
$ function set-prompt() { echo -ne '\033]0;' $@ '\a'; }
$ set-prompt "Novo Título"
```

## Rename Windows Terminal Tab on Powershell
```
> function Set-Prompt {

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
```
> wt.exe -p "PowerShell" --title "This one is PowerShell"`; new-tab -p "Ubuntu-20.04" --title "WSL here!"
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

## 
```

```

## 
```

```

## 
```

```



##### READ MORE LATER #####
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




