**Description**
This PC is attacked. The flag is stolen.
Flag: `SharifCTF{522bab2661c00e672cf1af399d6055cd}`


-----


**Files**
[Mini2.7z](https://mega.nz/#!AtgWjCxQ!C106i55-eVvkdqI0xDvGhWLEUFEnBcV6xQHjRWkDuDk)


-----


**Solution**
Mini.vmdk was inside the 7z archive - a Linux Mate VM disk.
Let's load it in VirtualBox - why not?
First of all we have to get into the boot menu GRUB and edit the boot options.

-----


![](https://raw.githubusercontent.com/YuryDo/writeups/master/SharifCTF8/Forensic200/1.png)


-----


We see that we can easily change users creds. First we have to check who was active and to look if there are any `cron/startup - surprises` in the system.


-----


![](https://raw.githubusercontent.com/YuryDo/writeups/master/SharifCTF8/Forensic200/2.png)


-----


Everything seems fine, let's see what the user was doing. And who stole our flag...
First of all the idea was to check all the network processes:


-----


![](https://raw.githubusercontent.com/YuryDo/writeups/master/SharifCTF8/Forensic200/3.png)


-----


Nothing of really interesting. After digging the user directory (building some boring timelines) and checking the apps, we ran into a firefox profile. We have opened firefox and have found a suspicious extension named "firecopy 2.3":


-----


![](https://raw.githubusercontent.com/YuryDo/writeups/master/SharifCTF8/Forensic200/4.png)


-----


So let's look closer at the extension, that is located in the .mozilla profile directory - /home/user/.mozilla/firefox/ih8wwh9c.default/extensions/{5ca6c28f-6283-42bd-912f-e6b776f83c39}.xpi. We can untar and read all the contents:


-----


![](https://raw.githubusercontent.com/YuryDo/writeups/master/SharifCTF8/Forensic200/5.png)
![](https://raw.githubusercontent.com/YuryDo/writeups/master/SharifCTF8/Forensic200/6.png)


-----


So it was now obvious, that something was stolen from the user session in firefox. We see a token, that can be also used for requests. Could it be our flag?


-----


![](https://raw.githubusercontent.com/YuryDo/writeups/master/SharifCTF8/Forensic200/7.png)


-----

Hm.. The git is empty. But what about the gist?


-----


![](https://raw.githubusercontent.com/YuryDo/writeups/master/SharifCTF8/Forensic200/8.png)


-----

Yay, we are near!.. But we got stuck, no trace of flags. Something was broken..
Everything was fixed after the news were updated with post:
> Attention
> check your findings again. The issue with "Stolen Flag" is solved.


-----


Now the url isn't active, but the flag was just [there](https://gist.github.com/clipboardstolenthings/).
And still a request from api.github.com is working:


-----


`curl -i -H 'Authorization: token 50e833d4ef0d228525f0bef6db857b35f53cd6d6' "https://api.github.com/users/clipboardstolenthings/gists"`


-----


**That's All Folks!**
