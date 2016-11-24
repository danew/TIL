# TIL
Stuff I might need again... and will probably forget

### Symlink
Symbolic link to another file  
`ln -s /path/to/file /path/to/symlink`

### NPM init
Initiate a new npm project without having to specify anything  
`npm init -y`

### Remove all files in a directory except something
Single file `ls | grep -v <file> | xargs -L 1 rm -rf`  
Multiple files `ls | grep -v -e <first file> -e <second file> | xargs -L 1 rm -rf`

### Move all file into a folder expect something
`ls | grep -v -e <file> -e <folder> | while read x; do mv $x ./<folder>/$x; done`  
A strange artifact is created with OpenVZ, so lets remove it `rm -rf .cpt_hardlink_dir_*`

### Display tree structure of directory with only python dependency
`find . -not -path '*/\.*' | python -c "import sys as s;s.a=[];[setattr(s,'a',list(filter(lambda p: c.startswith(p+'/'),s.a)))or (s.stdout.write(' '*len(s.a)+c[len(s.a[-1])+1 if s.a else 0:])or True) and s.a.append(c[:-1]) for c in s.stdin]"`

### List all enviroment variables
`printenv | less`

### Check if a Port is in use
`lsof -i <protocol>:<port>`  
For example to check what is using TCP port 80  
`lsof -i TCP:80`

### Find a Program using PID
`ps -p <PID> -o comm=`

### View a file with line numbers
Using `cat`, `cat -n <file>`  
Using `less`, `less -N <file>`

### watch
This is a really cool command, it allows you to watch a command and listen for changes.  
For example sometihng as simple as watching the response from an API end point  
`watch -d curl -s localhost:3000` where the `-s` option just removes the status bar (aka silent).  

### Find gateway and DNS
DNS - `less /etc/resolv.conf`  
GW - `less /etc/network/interfaces`

### Install specific version with PIP
`pip install <package>==<version>`  
For example - `pip install ansible==2.1.0`
