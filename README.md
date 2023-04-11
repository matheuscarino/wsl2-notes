# WSL2 documentation

## Install on Powershell as Administrator (Reboot needed)
```
> wsl --install
```
## Define Version 2 of WSL as Default
```
> wsl --set-default-version 2
```
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

### Windows Termi8nal JSON file settings
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
