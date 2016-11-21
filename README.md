# TIL
A place where I can dump things I learnt

## Symlink
Symbolic link to another file\s\s
`ln -s /path/to/file /path/to/symlink`

## NPM init
Initiate a new npm project without having to specify anything\s\s
`npm init -y`

## Remove all files in a directory except something
Single file `ls | grep -v <file> | xargs -L 1 rm -rf`\s\s
Multiple files `ls | grep -v -e <first file> -e <second file> | xargs -L 1 rm -rf`

## Move all file into a folder expect something
`ls | grep -v -e <file> -e <folder> | while read x; do mv $x ./<folder>/$x; done`\s\s
A strange artifact is created with OpenVZ, so lets remove it `rm -rf .cpt_hardlink_dir_*`
