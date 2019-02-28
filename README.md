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
``` BASH
find . -not -path '*/\.*' | python -c "import sys as s;s.a=[];[setattr(s,'a',list(filter(lambda p: c.startswith(p+'/'),s.a)))or (s.stdout.write(' '*len(s.a)+c[len(s.a[-1])+1 if s.a else 0:])or True) and s.a.append(c[:-1]) for c in s.stdin]"
```

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

### Setup networking on Centos 7
GUI method - `nmtui`   

### Create a live USB
First find the drive   
OSX - `diskutil list`   
Linux - `lsblk`   
Now unmount the drive `umount /dev/disk`  
Now copy the image to the disk with `dd`   
`dd bs=1m if=<input image> of=<output disk>`   
To provide process while `dd` is copying, you can either press `ctrl + T` or you can use `pv` (Pipe Viewer).   
Now you can run `dd` like this: where -s specified the file size to provide a completion bar.   
`dd if=/dev/sdb | pv -s 2G | dd of=DriveCopy1.dd bs=4096`

### Listen to all traffic on a port
`tcpdump -nni <interface> port <port>`   
For example - `tcpdump -nni eth0 port 80`

### Transfer your public RSA key to a remote host
`cat ~/.ssh/id_rsa.pub | ssh <user>@<host> "cat >> ~/.ssh/authorized_keys"`   
You can also just use `ssh-copy-id <user>@<host>`, however, this isn't always installed.

### Completely cleanup all virtualbox/vagrant boxes on a host
``` BASH
VBoxManage list runningvms | cut -d \" -f2 | while read key; do echo `vboxmanage controlvm ${key} poweroff && vboxmanage unregistervm ${key} --delete`; done && vagrant global-status --prune
```   
> This only currently will work if the box is running

### Extract, Edit, and Repack an ISO in Linux
*Requirements*:
- p7zip-full
- mkisofs

*Process*:
1. Move the ISO into a new folder for extraction (I used `/tmp/iso`)
2. Extract the ISOs contents `7z x <image>.iso` 
3. Deleted the ISO `rm <image>.iso`
4. Make your changes
5. Repack the ISO `mkisofs -o <output name>.iso -b isolinux/isolinux.bin -c isolinux/boot.cat -no-emul-boot -boot-load-size 4 -boot-info-table -J -R -V "CentOS 7.0 Custom ISO" .`

### Ping IP range
`nmap -T5 -sP <ip start range>-<ip end range>`   
For example - `nmap -T5 -sP 172.20.60.0-255`

### Remove all `.DS_Store` files
Got a new git repo? Did you accidently commit all the `.DS_Store` files in your project?   
Here's a simple remedy:   
`find . -name .DS_Store -print0 | xargs -0 git rm -f --ignore-unmatch && echo ".DS_Store" >> .gitignore`   

### View Chrome Plugin Source 
1. `extension_id=<id>`   
2. `curl -L -o "$extension_id.zip" "https://clients2.google.com/service/update2/crx?response=redirect&os=mac&arch=x86-64&nacl_arch=x86-64&prod=chromecrx&prodchannel=stable&prodversion=44.0.2403.130&x=id%3D$extension_id%26uc"`    
3. `unzip -d "$extension_id-source" "$extension_id.zip"`

### jQuery in three lines
```javascript
const $ = (selector) => document.querySelector(selector)
const $$ = (selector) => document.querySelectorAll(selector)
const on = (elem, type, listener) => elem.addEventListener(type,listener)
```

### Encrypt and Decrypt text with your rsa key
```bash
# create pem public key if you don't have one
openssl rsa -in ~/.ssh/<your private rsa key> -pubout > ~/.ssh/<name>.pem
# encrypt the text
echo "text" | openssl rsautl -encrypt -pubin -inkey <public key>.pem > <cypher file>
# decrypt the cypher
openssl rsautl -decrypt -inkey ~/.ssh/<your private rsa key> -in <cypher file>
```   

### Flexbox Elipsis
```scss
.heading {
  flex: 1 1 auto;
  height: 4rem;  
  &, & > * {
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
  }
}
```   

### Format certificate file to 64 chars
`fold -w 64`

### Download video streams
First locate the `.m3u8` file in the network.   
Install `ffmpeg`   
Run `ffmpeg -protocol_whitelist file,http,https,tcp,tls,crypto -i "https//file.com/viedo.m3u8" -c copy video.mp4`

### Token Replacement
```javascript
const interpolate = (str, data) => str.replace(/\{(.*?)\}/g, (_, token) => data[token]);

interpolate(
  '{name} said {message}',
  {
    name: 'Dane',
    message: 'Hello, World!'
  }
);
```

### Merge branch with a lot of changes
When you first start a project, project stucture tends to change a lot. If you want to merge a feature branch into master, and take all of the new changes and leave the old ones behind do this. 
```
git checkout feature_branch
git merge -s ours --no-commit master
git commit -m "..."
git checkout master
git merge feature_branch
```

### Inline Markdown Images
Instead of depending on a URL to always host an image, this utility will replace all markdown images and inline them as JPEGs.   
```javascript
const sharp = require('sharp');
const fetch = require('node-fetch');

const transformImage = url => {
  return fetch(url)
    .then(response => response.buffer())
    .then(buffer => sharp(buffer)
      .resize({ 
        width: 600,
      })
      .toFormat('jpeg')
      .toBuffer()
    )
    .then(buffer => buffer.toString("base64"));
};

const replaceAsync = async str => {
  const rx = /!\[(.*?)\]\((.*?)\)/g;
  const promises = [];
  str.replace(rx, (_, name, url) => {
      const promise = transformImage(url)
        .then(data => `![${name}](data:image/jpeg;base64,${data})`);
      promises.push(promise);
  });
  const data = await Promise.all(promises);
  return str.replace(rx, () => data.shift());
};
```

### Get all font variations on a page
```javascript
const nodes = document.body.querySelectorAll('*');
let styles = [];

Array.from(nodes).forEach(node => {
  const computedStyles = window.getComputedStyle(node);
  if (/(Noir|Chronicle)/.test(computedStyles['font-family'])) {
    styles.push({
      family: computedStyles['font-family'].split('"')[1],
      weight: computedStyles['font-weight'],
      style: computedStyles['font-style'],
    });
  }
});

stylesEqual = (current, next) => (
  current.family === next.family &&
  current.weight === next.weight &&
  current.style === next.style
);

styles = styles.reduce((acc, curr) => {
  if (acc.some(next => stylesEqual(next, curr))) return acc;
  return [ ...acc, curr ];
}, []);

```
