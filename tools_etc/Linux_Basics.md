# Linux Basics

```uname snor```

 ## 1. Directories
 - Get your current working directory: ```pwd```
 - Check the files in your current working directory: ```ls```
  - ```ls``` and ```pwd``` are applications and they can take parameters, just as Python functions do. Some parameters are options and some are arguments. Here are some options:
    - ```ls -a```
    - ```ls -l```
    - ```ls -al```
    - ```ls -la```
    - ```ls -l -a```
    - ```ls -lah```
 - The ```cd``` command takes arguments. Change your working directory:
  - ```pwd```
  - ```cd /home/my_user_name```
  - ```pwd```
 - A little more ```cd```
  - Move up with ```cd ..```
  - Stay here with ```cd .```
 - Make a new directory:
  - ```cd /home/my_user_name```
  - ```mkdir workspace```
  - ```cd workspace```
 - Try:
   - ```cd ~```
   - ```pwd```
   - ```cd ~/workspace```
   - ```pwd```
 - Create a file:
  - ```touch ~/junk.txt```
 - The ```mv``` command takes at least 2 arguments. Move the file to your workspace:
  - ``` mv junk.txt ~/workspace ```
 - Changing the filename is the same as renaming:
  - ```mv junk.txt README.txt```

  ## 2. Files
  #### (let's start with echoes so we don't have to talk about vim)
  - To read a text file, use:
    - ```cat README.txt```
    - (there's nothing there yet!)
  - Try ```echo "Hi, Linux!"```
  - Now add some text to your README.txt with:
   - ```echo "Hello" > README.txt```
  - Try reading again:
    - ```cat README.txt```
  - Now, add a line:
    -```echo "Hello, again!" >> README.txt```
  - OK, we're done here:
    - ```echo "Goodbye, cruel bash!" > README.txt```
    - ``` cat README.txt```
    - ```rm README.txt```  __careful with this one!__ __there is no undo__

  ## 3. Some More Commands
  - When you don't know something:
    - ```man ls```
    - ```man git```
    - ```man curl```
  - Read Google's homepage:
    - ``` curl http://www.google.com ```
  - Let's steal Google's homepage, using both arguments and options:
    - ```curl http://www.google.com -o i_stole_google_arrest_me.html```
  - Now, read it to make sure:
    - ```cat i_stole_google_arrest_me.html```
  - Oh my....that's a lot of text. Let's try printing it with less:
    - ```less i_stole_google_arrest_me.html```
  - But what if we want to read Google.com without stealing it? If only we could combine commands...
    - ```curl http://www.google.com | less```
    - The pipe operator lets us send the output of one application to the input of another

  ## 4. Processes
  - How much memory am I using?
      - ```free```
      - ```free -g```
  - What's going on?
      - ```man ps```
      - ```ps```
      - ```ps -aux```
      - ```echo "too much text again"```
      - For a summary dashboard: ```top```
  - Try this:
      - ```echo 'spy on gian'```
      -  ```ps -aux | grep my_user_name```
      - "grep" basically means "find" or "search"
    - ```ps -aux | grep my_user_name | grep ssh```
    - ```ps -aux | grep ssh```
    - ```pgrep -l ssh -u my_user_name```


 ## 5. Hold on...what's with this "ssh" thing?
  - "ssh" stands for "secure shell"
    - it lets one computer securely connect to another
  - Start by getting your ec2 IP address:
  - ```ifconfig``` or ```uname -n```
  - ```ssh -R 52698:localhost:52698 an_ip_address```
    - The ```-R 52698:localhost:52698``` option enables port forwarding (useful for connecting text editors, e.g.)
    - Specs for bouncing off of bastion (a proxy server) are in your ```/.ssh/config``` file (if you've created it):

    ```
    Host 1ip.23addr.ess24
        ProxyCommand ssh my_user_name@proxy.proxy.proxy.com -W %h:%p
    ```

  - Try:
    - ```ls ~/.ssh```
    - or ```cd ~/.ssh```
    - and then ```ls```
    - Do you have any files in there? If so, __don't touch them__
    - These are your ssh keys. Think of them like a handshake between your machine and EC2 or Github (or any other server)
      - ```cd ~/.ssh```
      - we can look without touching: ```cat id_rsa```
        - ```cat id_rsa.pub```
        - Your private keys (i.e., files without the .pub extension) should never, ever be shared with anyone
        - When a service like Github wants your ssh key, it means the public key
    - To create a new key pair (a private and a public ssh key): ```ssh-keygen```
  - You can use ssh for many things, among them:
    - run jupyter from your EC2 machine
    - use a text editor to edit files on your EC2 machine
    - the ```scp``` command is part of the ssh suite. From local:
      - ```touch junk.txt```
      - ```echo "I'm from local. Remember me?" > junk.txt```
      - ```cat junk.txt```
      - ```scp ./junk.txt my.ec2.addr.ess:/home/my_user_name/```
    - From EC2:
      -```cd ~``` or ```cd```
      -```cat junk.txt```
      -OK, done with this one now:
        - ```rm junk.txt```

## 6. Do I have to type ```ssh -R 52698:localhost:52698 ip.add.ress``` every time?
  - Just give it an alias:
    - ```alias junk="echo "NO\!""```
    - ```junk```
  - You can put alias assignments (among other things) in your ```~/.bash_profile``` so you don't have to re-enter them every time you open a shell
  - ```less ~/.bash_profile```
  - Looks like it's time for vim:
  ```vim ~/.bash_profile```
