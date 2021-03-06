#### Dockerized Spacemacs(GUI)  

[![screenshot](https://raw.githubusercontent.com/syl20bnr/spacemacs/master/doc/img/spacemacs-python.png)](https://raw.githubusercontent.com/syl20bnr/spacemacs/master/doc/img/spacemacs-python.png)
*Press `" "`-`"T"`-`"n"` to cycle to the [next theme](https://github.com/JAremko/spacemacs/blob/master/.spacemacs#L113)*  
*[More themes](http://themegallery.robdor.com/)*

##### [Also You might want to look at my Vim+Tmux based thingy `jare/drop-in`](https://hub.docker.com/r/jare/drop-in)  
[![](http://i.imgur.com/RVTlBBO.png)](http://i.imgur.com/RVTlBBO.png)

---------------------------------------------------------------------------------
#### What's inside?

##### jare/spacemacs:base  
  - [Emacs 24](https://www.gnu.org/software/emacs/)  
  - [Spacemacs(develop)*](https://github.com/syl20bnr/spacemacs)  
  - *bash, curl, git, openssl, mosh, rsync, make, htop and fish.*     

*The image contains all Emacs packages used by Spacemacs. ([issues/6769](https://github.com/syl20bnr/spacemacs/issues/6769))

##### jare/spacemacs:latest  
  - Same as jare/spacemacs:base +
  - [Golang](https://golang.org/) + a ton of tools 
  - [Iceweasel(Firefox)](https://www.mozilla.org/en-US/firefox/new/)   
  - [Compass](http://compass-style.org/)  
  - [PhantomJS](http://phantomjs.org/)  
  - [Node.js](https://nodejs.org/) + some tools  
  - [TypeScript](http://www.typescriptlang.org/) + some tools 
  - [Slim](http://slim-lang.com/) 

You can set the user by changing those lines in the jare/spacemacs:base Dockerfile:
```
  ENV uid 1000
  ENV gid 1000
  ENV UNAME jare
```
Also edit [.spacemacs](https://github.com/JAremko/spacemacs/blob/master/.spacemacs) to add stuff that you would like to have.

#### Usage: 

 - **Create /usr/local/bin/spacemacs.bash**

```bash
#!/bin/bash

#Keep the container up-to-date
docker pull jare/spacemacs:latest
  
cd /home/jare/Documents/workspace

SPACE_HOME="$(docker inspect --format '{{.Config.Labels.HOME}}' \
jare/spacemacs:latest)"

echo '3...2...1...'

docker run -ti --rm -v $('pwd'):"${SPACE_HOME}/workspace" \
 -v /etc/localtime:/etc/localtime:ro                      \
 -v /home/jare/.ssh/id_rsa:${SPACE_HOME}/.ssh/id_rsa:ro   \
 -v /var/run/dbus:/var/run/dbus                           \
 -v /tmp/.X11-unix:/tmp/.X11-unix                         \
 -v /etc/machine-id:/etc/machine-id:ro                    \
 -e DISPLAY=$DISPLAY                                      \
 -e "GITEMAIL=w3techplayground@gmail.com"                 \
 -e "GITNAME=JAremko"                                     \
 -p 80:80 -p 8080:8080 -p 443:443 -p 3000:3000            \
 --name spacemacs jare/spacemacs:latest
```
 - **Add this line to .bashrc** `alias spacemacs='bash /usr/local/bin/spacemacs.bash'`  
 - *To use GitHub, change `GITEMAIL=w3techplayground@gmail.com`, `GITNAME=JAremko` and `home/jare/.ssh/id_rsa`*  
*If you see Emacs warnings, you can add `--privileged` but it's not necessary.*
