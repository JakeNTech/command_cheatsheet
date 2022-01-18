# Linux Command Cheatsheet
> Jake's Special List

A list of commands to help with using Linux.
- [Linux Command Cheatsheet](#linux-command-cheatsheet)
	- [File Viewing / Editing](#file-viewing--editing)
		- [Cat](#cat)
		- [BatCat (Viewing)](#batcat-viewing)
		- [VIM (Viewing/Editing)](#vim-viewingediting)
			- [Edit files](#edit-files)
			- [Finished Editing a file](#finished-editing-a-file)
			- [Close and Save a file](#close-and-save-a-file)
		- [Force Quit, without saving](#force-quit-without-saving)
			- [Show Line Numbers](#show-line-numbers)
		- [Visual Studio Code](#visual-studio-code)
	- [File Analysis](#file-analysis)
		- [Binwalk](#binwalk)
		- [LNK File](#lnk-file)
		- [Foremost](#foremost)
		- [PDF](#pdf)
		- [Office Documents with VBA Code](#office-documents-with-vba-code)
		- [Anything with Metadata](#anything-with-metadata)
		- [Ghex](#ghex)
		- [Hashes](#hashes)
	- [EXE/DLL Analysis](#exedll-analysis)
		- [Cutter Dissasembler](#cutter-dissasembler)
		- [Strings](#strings)
	- [Memory Forensics](#memory-forensics)
	- [Networking stuff](#networking-stuff)
- [Useful Scripts](#useful-scripts)
	- [Update System Script](#update-system-script)
	- [Install the tools script](#install-the-tools-script)

## File Viewing / Editing
Want to open a file and show the contents on the command line? I bet you do!

### Cat
As simple as that, cat a file to the terminal:
```shell
cat ./<file>
```

### BatCat (Viewing)
Just a fancy way of viewing a file, a nice replacement for `cat` as it adds easier scrolling, line numbers and Syntax highlighting
```shell
batcat ./<file>
```

### VIM (Viewing/Editing)
Hmm VIM FOR THE WIN!
```shell
vim ./<file>
```

#### Edit files
By defualt VIM opens files without the ability to edit the text in them all we need to do is, press the "i" key:\
<kbd>i</kdb>

#### Finished Editing a file
Done making a change to a file:\
<kbd>Ctrl</kbd>+<kbd>C</kbd>

#### Close and Save a file
<kbd>Shift</kbd>+<kbd>;</kbd> and then can type <kbd>w</kbd>+<kbd>q</kbd>

(w = Write, q = Quit)
### Force Quit, without saving
<kbd>Shift</kbd>+<kbd>;</kbd> and then type `q!`

This forces the document to close witout making changes. 

#### Show Line Numbers
<kbd>Shift</kbd>+<kbd>;</kbd> Then Type `set number`

### Visual Studio Code
Want to use a proper GUI editor? No drama.
```shell
code ./<file/direcotry>
```

## File Analysis
Got a file...I'll tell you about it

### Binwalk
Works with any file!
```shell
binwalk ./<file>
```
This goes through the file looking for file [headers](https://en.wikipedia.org/wiki/List_of_file_signatures) and tells you what kind of file it is we are dealing with 
```shell
binwalk ./<file> -e
```
If you see more then one header this will extract other files from within the file. When looking at a PDF you will see alot of Zlib files, a fun quirk with the format

### LNK File
Got a Windows Shortcut but dunno what it's for? This will tell ya.
```shell
lnkinfo ./<file>
```

### Foremost
This is mostly the same as binwalk but can be a bit more agressive, this really is only for file extraction:
```shell
foremost -i ./<file> -o ./<output_directory> -v
```
Then pop a `cd` into the output direcotry and you should have your files. This is a tool very good for disk images rather then files. Files you best just stick with Binwalk.

### PDF 
Some PDF's have JavaScript in them that run stuff, this will cause other process to be spawned and will cause a detection match:
```shell
pdfinfo -js ./file > java.js
```
This will pull the JS out of the file for you to see what it does.

### Office Documents with VBA Code
Office Document with VBA Code? Never!
```shell
olevba ./File > output.file
```
This pulls VBA from a file, without you having to go in and do it. This will dump it all into the output file, you can then see what it does.

### Anything with Metadata
Want to see if it's got metadata?
```shell
exiftool ./<file>
```

### Ghex
Proberbly won't ever need to see the HEX of a file, but it's a cool thing to play with (And used in CTF Challanges 😉)
```shell
ghex ./<file> &
```

### Hashes
Need to grab the hash for a file?
```shell
sha256sum ./<file>
sha1sum ./<file>
md5sum ./<file>
```

## EXE/DLL Analysis
I've put this seperate from File analysis because it can be done in two ways, with a tool or just with strings

### Cutter Dissasembler
Cutter is, basically, an opensource version of IDA Pro; It can pull version infomation and will, as the name says, disassemble a DLL/EXE file. It can, in some cases, try to guess the origional code which is a handy feature.
```shell
/opt/Cutter-v2.0.3-x64.Linux.AppImage ./<file>
```
This one's GUI based so no faffing arround with more commands!

### Strings
As the name suggests this one just dumps the strings out onto the comamnd line, there are some options that can make this more viewable though:
```shell
strings ./<file> | less
```
This allows you to scroll through the output of strings. You could also save the output to a file, but there isn't really much point as you can get what you want from the command above and run:
```shell
strings ./file > output.txt
```

## Memory Forensics
If i'm being honest Volatility is a bit of a pain to install, the help documentation is very...well helpful but it still deserves a mention here
## Networking stuff
You thought you could escape the netowrking?
1. TCPDump
2. TShark
3. Wireshark
4. ip
5. Burpsuite

# Useful Scripts
## Update System Script
If your going to use the script to grab all the above there is a simple way to update:
```shell
/opt/update.sh
```
THIS WILL REBOOT THE MACHINE...Because why not!
## Install the tools script
```bash
# Updates APT
sudo apt update
# Binwalk
sudo apt install binwalk
# lnkinfo
sudo apt install lnkinfo
# OLETools
sudo -H pip3 install -U https://github.com/decalage2/oletools/archive/master.zip
# Ghex
sudo apt install ghex
# batcat
sudo apt install bat
# Cutter
cd /opt
sudo wget https://github.com/rizinorg/cutter/releases/download/v2.0.5/Cutter-v2.0.5-x64.Linux.appimage
sudo chmod +x Cutter-v2.0.5-x64.Linux.appimage
cd ~
# Exiftool
sudo apt install exiftool
# Update Script
echo "echo 'Updating!'" > ./temp.txt
echo "sudo apt update -y" >> ./temp.txt
echo "sudo apt full-upgrage -y" >> ./temp.txt
echo "sudo apt autoremove -y"
echo "reboot" >> ./temp.txt
sudo mv ./temp.txt /opt/update.sh
sudo chmod +x /opt/update
# Microsoft Visual Studio Code (Install via package manager)
wget -qO- https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > packages.microsoft.gpg
sudo install -o root -g root -m 644 packages.microsoft.gpg /etc/apt/trusted.gpg.d/
sudo sh -c 'echo "deb [arch=amd64,arm64,armhf signed-by=/etc/apt/trusted.gpg.d/packages.microsoft.gpg] https://packages.microsoft.com/repos/code stable main" > /etc/apt/sources.list.d/vscode.list'
rm -f packages.microsoft.gpg
sudo apt install apt-transport-https
sudo apt update
sudo apt install code
```