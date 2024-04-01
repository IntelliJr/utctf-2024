## Prompt 
Hey, my digital cat managed to get into my server and I can't get him out.
The only thing running on the server is a website a colleague of mine made.
Can you find a way to use the website to check if my cat's okay? He'll likely be in the user's home directory.
You'll know he's fine if you find a "flag.txt" file.

http://betta.utctf.live:5422

## Approach
At least for me, this problem wasn't easy. It took me quite some time to get it right. 
We're required to upload a zip file and the webpage would display the contents of each file in the unzipped folder.

![image](https://github.com/IntelliJr/utctf-2024/assets/66992622/76f0f9b3-4fa4-46e5-ae6a-30d23e9c7b1b)

After spending several fruitless hours on the internet, I found [this video](https://www.youtube.com/watch?v=bRZhOB8T9Ig&ab_channel=EffortlessSecurity) demonstrating a zip path traversal attack in which one can upload a zip file containing a symbolic link that points to a file outside the 
unzipping directory. If the server doesn't perform necessary validations during the extraction process, sensitive files could be read/overwritten. 

I executed the following commands to inspect the `/etc/passwd` file on the server as proof of concept

```bash
bham@sysprog:~$ mkdir attack 
bham@sysprog:~$ cd attack
bham@sysprog:~/attack$ ln -s /etc/passwd etc
bham@sysprog:~/attack$ zip -r --symlinks attack.zip etc
```
Uploading the zip file to the webpage ... and: 

![image](https://github.com/IntelliJr/utctf-2024/assets/66992622/e5155222-37f4-4256-b00a-e57dcf6566bc)

it works!

On the last line we see a user called `copenhagen`.

I then created a symlink to `/home/copenhagen/flag.txt` and repeated the other steps as shown above. 

Moment of truth..

![image](https://github.com/IntelliJr/utctf-2024/assets/66992622/5017a216-100c-42b2-a634-d02a5dffb855)

Which is the flag we're looking for!


PS: [This article](https://blog.ostorlab.co/zip-packages-exploitation.html#:~:text=Common%20ZIP%20vulnerabilities%3A&text=ZIP%20symlink%20path%20traversal%3A%20ZIP,might%20lead%20to%20code%20execution.) sums up many zip vulnerabilites. 


