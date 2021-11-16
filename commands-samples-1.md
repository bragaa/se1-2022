
```bash

dmesg | grep -i sata | grep 'link up'

grep -Fx -f file1 file2

grep -a -B 5 -A 5 'libc' /bin/ls > results.txt

grep -RnisI '#include <stdlib.h>' *

find . -type f -print0 | xargs -0 grep -i '#include *<stdlib.h>'

find . -type f -exec grep -i <pattern> \;

grep -nisI <pattern> * .[!.]*

locate -0 -bi *java* | xargs -0 mv -t ~/'Library/Books/Java Entreprie Edition'

mkdir /home/foo/doc/bar && cd $_

md () { mkdir -p "$@" && cd "$@"; }

function mcd() { [ -n "$1" ] && mkdir -p "$@" && cd "$1"; }

rmdir -p a/b/c

curl -A "Mozilla" "http://translate.google.com/translate_tts?tl=en&q=hello+world" > hello.mp3

sudo -u apache find . -not -perm /o+r
o=0; git log --oneline | while read l; do printf "%+9s %s\n" "HEAD~${o}" "$l"; o=$(($o+1)); done | less

ps aux | sort -nk +4 | tail

ps axo %mem,pid,euser,cmd | sort -nr | head -n 10

top -b -o +%MEM |head -17

kill -9 `ps -xaw -o state -o ppid | grep Z | grep -v PID | awk '{print $2}'`

kill -9 `ps xawo state=,pid=|sed -n 's/Z //p'`

ps axo state,ppid | awk '!/PPID/$1~"Z"{print $2}' | xargs -r kill -9

dpkg -S /usr/bin/ls

dpkg -l 'linux-*' | sed '/^ii/!d;/'"$(uname -r | sed "s/\(.*\)-\([^0-9]\+\)/\1/")"'/d;s/^[^ ]* [^ ]* \([^ ]*\).*/\1/;/[0-9]/!d' | xargs sudo apt-get -y purge

cat /var/lib/dpkg/info/*.list > /tmp/listing ; ls /proc/*/exe |xargs -l readlink | grep -xvFf /tmp/listing; rm /tmp/listin

ssh remotehost 'dpkg --get-selections' | dpkg --set-selections && dselect install

apt-get install `ssh root@somehost "dpkg -l | grep ii" | awk '{print $2}'`

dpkg -l 'linux-*' | sed '/^ii/!d;/'"$(uname -r | sed "s/\(.*\)-\([^0-9]\+\)/\1/")"'/d;s/^[^ ]* [^ ]* \([^ ]*\).*/\1/;/[0-9]/!d' | xargs sudo apt-get -y purge
```

```bash
for f in *;do case "$(echo $f|sed "s/.*\.\([a-z\.]*\)/\1/g")" in zip)unzip -qqo $f&&rm $f;;tar.gz|tar.bz2)tar xf $f&&rm $f;;rar)unrar e -o+ -r -y $f&&rm $f;;7z)7z e -qqo $f;;esac;done
```

```bash
du -b --max-depth 1 | sort -nr | perl -pe 's{([0-9]+)}{sprintf "%.1f%s", $1>=2**30? ($1/2**30, "G"): $1>=2**20? ($1/2**20, "M"): $1>=2**10? ($1/2**10, "K"): ($1, "")}e'
```

```bash
alias head='head -n $((${LINES:-`tput lines 2>/dev/null||echo -n 12`} - 2))'
```

```bash
#!/bin/bash
# Counting the number of lines in a list of files
# function version

count_lines () {
  local f=$1  
  # this is the return value, i.e. non local
  l=`wc -l $f | sed 's/^\([0-9]*\).*$/\1/'`
}

if [ $# -lt 1 ]
then
  echo "Usage: $0 file ..."
  exit 1
fi

echo "$0 counts the lines of code" 
l=0
n=0
s=0
while [ "$*" != ""  ]
do
        count_lines $1
        echo "$1: $l"
        n=$[ $n + 1 ]
        s=$[ $s + $l ]
	shift
done

echo "$n files in total, with $s lines in total"
```

```bash
sed -i 8d ~/.ssh/known_hosts
```

```bash
echo "${1}" | egrep '^[[:digit:]]*$' ; if [ "$?" -eq 0 ] ; then sed -i "${1}"d $HOME/.ssh/known_hosts ; else printf "\tYou must enter a number!\n\n" ; exit 1 ; fi
```

```bash
sed -n 5p source.c

sed -i source.c -re '1,3d'

cd $(dirname $(find ~ -name emails.txt))

cd $(find ~ -name emails.txt -exec dirname {} \; -quit)

sort -n <( for i in $(find . -maxdepth 1 -mindepth 1 -type d); do echo $(find $i | wc -l) ": $i"; done;)

tail -F file

passwd -d $USER

whereis thiscommand

aptitude remove $(dpkg -l|egrep '^ii linux-(im|he)'|awk '{print $2}'|grep -v `uname -r`)
grep -n apache /var/log/messages | cut -f1 -d: | tail -1 | 

xargs -I num tail -n +10 /var/log/messages

partprobe

blockdev --getbsz /dev/sda1

sfdisk

cfdisk

hdparm -f /dev/sda1

fdisk -l /dev/sda

blkid /dev/sda1

blkid -L /boot

blkid -U 652b786e-b87f-49d2-af23-8087ced0c667

findfs UUID=652b786e-b87f-49d2-af23-8087ced0c667

findfs LABEL=/boot

e2label /dev/sda1

findmnt

mkfs -t ext4 /dev/sda1

mke2fs -j /dev/sda1

mkfs.ext3 /dev/sda1

mkfs -t msdos /dev/sda

mount

cat /proc/mounts

cat /etc/mtab

sudo dd if=/dev/mem | cat | strings
```