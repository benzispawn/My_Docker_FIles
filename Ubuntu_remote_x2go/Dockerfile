#Modificated by Raphael Benzi
# Get the distro:
FROM ubuntu:18.04
ENV DEBIAN_FRONTEND=noninteractive

COPY sources.list /etc/apt/sources.lists
RUN apt-get clean && apt-get update && apt-get install -y wget gnupg2
RUN wget -q https://dl.google.com/linux/linux_signing_key.pub -O- | apt-key add -

# Do some general updates.
RUN apt-get clean && rm -rf /var/lib/apt/lists/* &&  apt-get clean &&  apt-get update -y &&  apt-get upgrade -y
RUN apt-get install -y software-properties-common 
RUN add-apt-repository universe
RUN apt-get install -y cups curl sudo libgconf2-4 iputils-ping libxss1 wget xdg-utils libpango1.0-0 fonts-liberation
RUN apt-get update -y && apt-get install -y software-properties-common && apt-get install -y locales 

# Let's add a user, so we have a chance to, well, actually use the machine.
ENV USER='benzi'
ENV PASSWORD='0ok9ij8uh7yg'
RUN groupadd dev \
    && useradd -r -g dev -d /home/$USER -s /bin/bash -c "$USER" $USER \
    && adduser $USER sudo \
    && mkdir /home/$USER \
    && chown -R $USER:dev /home/$USER \
    && echo $USER':'$PASSWORD | chpasswd

ENV USER2='user'
ENV PASSWORD2='user'
RUN useradd -r -g dev -d /home/$USER2 -s /bin/bash -c "$USER2" $USER2 \
    && adduser $USER2 sudo \
    && mkdir /home/$USER2 \
    && chown -R $USER2:dev /home/$USER2 \
    && echo $USER2':'$PASSWORD2 | chpasswd
# Let's add a super user, too. Same password
ENV SUDOUSER='benzi'
ENV PASSWORD='0ok9ij8uh7yg'
RUN groupadd $SUDOUSER \
    #&& useradd -r -g $SUDOUSER -d /home/$SUDOUSER -s /bin/bash -c "$SUDOUSER" $SUDOUSER \
    && adduser $SUDOUSER sudo \
    #&& mkdir /home/$SUDOUSER \
    && chown -R $SUDOUSER:root /home/$SUDOUSER \
    && echo $SUDOUSER':'$PASSWORD | chpasswd

# Configure timezone and locale to en_US. __Change locale and timezone to whatever you want.__
 ENV LANG="en_US.UTF-8"
 ENV LANGUAGE=en_US
 RUN locale-gen en_US.UTF-8 && locale-gen en_US
 RUN echo "Europe/Copenhagen" > /etc/timezone && \
     apt-get install -y locales && \
     sed -i -e "s/# $LANG.*/$LANG.UTF-8 UTF-8/" /etc/locale.gen && \
     dpkg-reconfigure --frontend=noninteractive locales && \
     update-locale LANG=$LANG

# Create an keyboard-layout file, so we won't have to set it every time the machine starts. Just replace XKBLAYOUT="dk" with your layout, ex. "us".
# RUN printf '# Consult the keyboard(5) manual page.\nXKBMODEL="pc105"\nXKBLAYOUT="dk"\nXKBVARIANT=""\nXKBOPTIONS=""\nBACKSPACE="guess"\n'"" > /etc/default/keyboard
# - doesn't work :(
# set danish keyboard layout
RUN apt-get install setxkbmap
RUN setxkbmap -model abnt2 -layout br
#RUN echo 'setxkbmap -model abnt2 -layout br' >> /etc/X11/xinit/xinitrc

# Install some much needed programs - nano, midnight commander, guake terminal
RUN apt-get install vim -y
RUN apt-get install mc -y
RUN apt-get install guake -y

# Use the Xfce desktop. Because it's nice to look at, in my opinion.
RUN apt-get update -y && \
    apt-get install -y xfce4
# There's also the MATE desktop-enviroment. Bit more light-weight.
#RUN apt-get update -y && \
#   apt-get install -y mate-desktop-environment-extras    
#RUN echo 'setxkbmap -model abnt2 -layout br' >> /etc/X11/xinit/xinitrc

# Install git
RUN apt-get install -y git
# Install Python.
# Whoa, waitaminute - Ubuntu 17.10 already comes with Python 3.6 as default. Just run python3 to invoke it.

# Install miniconda, for Python virtual environmentaziation
# RUN curl -fSL "https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh" -o Miniconda3-latest-Linux-x86_64.sh
# RUN bash Miniconda3-latest-Linux-x86_64.sh -b
# Enable conda
# RUN echo ". /home/theadmin/miniconda3/etc/profile.d/conda.sh" >> ~/.bashrc
# RUN conda create -n <name_of_env> python=3.6 -y

# Install Firefox
RUN apt-get install firefox -y

# Install Postman to 'opt' dir
RUN wget https://dl.pstmn.io/download/latest/linux64 -O postman.tar.gz
RUN tar -xzf postman.tar.gz -C /opt
RUN rm postman.tar.gz

# Install Visual Studio Code
#ENV VSCODEPATH="https://go.microsoft.com/fwlink/?LinkID=760868"
#RUN curl -fSL "${VSCODEPATH}" -o vscode.deb && dpkg -i vscode.deb
RUN wget -q https://packages.microsoft.com/keys/microsoft.asc -O- | apt-key add -
RUN add-apt-repository "deb [arch=amd64] https://packages.microsoft.com/repos/vscode stable main"
RUN apt update && apt install -y code

# To make it easier to automate and configure VS Code, it is possible to list, install, 
# and uninstall extensions from the command line. When identifying an extension, provide 
# the full name of the form publisher.extension, for example donjayamanne.python.
USER $USER
WORKDIR /home/$USER

# Enable viewing git log, file history, compare branches and commits - https://marketplace.visualstudio.com/items?itemName=donjayamanne.githistory
RUN code --install-extension donjayamanne.githistory
# Install Ms' python - linting, debugging, intellisense, etc.
RUN code --install-extension ms-python.python
# Install code outline provider - better code visualization in the explorer pane
#RUN code --install-extension patrys.vscode-code-outline

#Install X2GO
RUN add-apt-repository ppa:x2go/stable
RUN apt-get update && apt-get install x2goserver x2goserver-xsession

# Annnnnd back to root for the remainder of this session.
USER root

# Install nomachine, the remote-desktop server that enables us to remote into the container image.
# You don't have to rely on my choice of NoMachine - just go to their website and get a different one, if you want.
#ENV NOMACHINE_MAJOR_VERSION "6.7"
#ENV NOMACHINE_PACKAGE_NAME nomachine_6.7.6_11_amd64.deb
#ENV NOMACHINE_MD5 4d94be64f52b4b5d726da7d2610feeaa

#RUN curl -fSL "http://download.nomachine.com/download/${NOMACHINE_MAJOR_VERSION}/Linux/${NOMACHINE_PACKAGE_NAME}" -o nomachine.deb \
#&& echo "${NOMACHINE_MD5} *nomachine.deb" | md5sum -c - \
#&& dpkg -i nomachine.deb


# Create an executable file that starts the NoMachine remote desktop server.
# A unix executable .sh-file must start with #!/bin/bash. '\n' means 'newline'.
# Note how the file ends with a /bin/bash-command. That's deliberate, it allows
# us do - don't ask me how - keep the container running when we use it later.
#RUN printf '#!/bin/bash\n/etc/NX/nxserver --startup\n/bin/bash'"" > /etc/NX/nxserverStart.sh
# Now make the executable _actually_ executable ...
#RUN chmod +x /etc/NX/nxserverStart.sh
# ... and start the nomachine-remote server when the container runs, and ...
#CMD ["/etc/NX/nxserverStart.sh"]
#... happy developing! Use a NoMachine-client program to log into the server.
# PS: remember to run the container with the -d and -t arguments. 
# Check the readme.md file, https://github.com/harleydk/linuxRemoteDocker/blob/master/README.md
