---
title: Ubuntu debian package develop
date: "2022-02-13T21:28:37.121Z"
template: "post"
draft: false
slug: "ubuntu-develop"
category: "ubuntu develop"
tags:
  - "ubuntu develop"
description: ""
socialImage: "/media/river.jpg"
---

# ubuntu debian package develop steps

- Get the source code of that Debian package.
- Add an executable bash script called “testing.sh”
    - Executing testing.sh will echo “this is a test from Nigel Wang ” on the console.
- Please echo string “this is a test from Nigel Wang” during the Debian package installation time.
- Host all of your change to a public git repository (e.g. launchpad git, Github or GitLab)
- Dput the source change to your ppa.
- Install your package from your ppa.
- Past the message printed by the installation.
    - The string “this is a test from Nigel Wang” is expected.
- Past the executing result of  `dpkg -S testing.sh; testing.sh`
    - The string “this is a test from {your name}” is expected.
    - Record things that you experienced for this task on this mail as a document.
- Image the steps is to guide people who did not experience this task to finish this task.



## deb script
- preinst
- postinst: when user run dpkg -i ****.deb, the process will run DEBIAN/postinst
- prerm 
- postrm


## Personal Package Archives (PPA) 
allow you to upload Ubuntu source packages to be built and published as an apt repository by Launchpad. You can find out more about PPAs and how to use them in our help page.

## Target
- Modify debian source package code, and publish it to ppa.
    - Download source code.
    ```c
    pull-lp-source hello// hello is package name
    ```
    - Run initization​ script when run dpkg -i **.deb. Below scripts all is in debian/*
    
        - postinst (script): It can run commands when run dpkg -i ***
        ```c
        #!/usr/bin
        echo "hello?"
        ```
        - install : move testing.sh to usr/bin/*
        ```c
        testing.sh usr/bin
        ```
        - Run "dpkg-source --commit" in source code location, then run "debuild -us -uc". Above command is add commit and build code to .deb file.
        - 
          -us Do not sign the source package.
           -uc Do not sign the .changes file.
        - If above steps all is finish, if you run dpkg -i **.deb will print below picture.
        ![](https://i.imgur.com/dQOjhMn.png)
        
    - The step describes how to publish cheange to ppa.
        -  Need to create a ppa repository.
        ![](https://i.imgur.com/XImm842.png)
        -  Need to generate pgp key in your local.
        ```c
        gpg --gen-key // generate key command
        ```
        - You follow the step, you can saw below message box. You must remember the pass phrase.
        ![](https://i.imgur.com/gpjEanU.png)
        - You can check by below command, if your setting is success, you can watch your pub key and private key.
        ```c
        gpg --list-keys
        ```
        ![](https://i.imgur.com/xOcY6pJ.png)


        -  Need to publish key to ubuntu key-server.
        ```c
        gpg --keyserver keyserver.ubuntu.com --send-keys <pub key>
        ```
        - Paste you key in openpgp page. 
            - [openpgplink](https://launchpad.net/people/+me/+editpgpkeys)
            - ![](https://i.imgur.com/ge9rln6.png)
        -  Finally, the server will send a encrypty email for you, you need to use open gpg decrypt the message.
            - You need to create a file, and paste encrypy message from email. The content is from "-----BEGIN PGP MESSAGE-----" to "-----END PGP MESSAGE-----"
            - Run below command
            
            ```c
            gpg --decrypt <key file>
            ```
            - You will saw the message box.
             ![](https://i.imgur.com/jvKkGLT.png)
            - You need to go to the below link check your final step.
            ![](https://i.imgur.com/e0ml1Oe.png)
            - finally you can saw below message what the mean is you link ppa successfully.
            ![](https://i.imgur.com/GP7MPAQ.png)
    - Publish file to ppa server.
        - Build changes file and  package.
            ```c
            debuild -S -d -us -uc
            ```
        - Commit your changes.
        ```c
        dch -i
        ```
        - Sign your changes file.
        ```c 
        debsign -k <gpg key id> ./******.changes
        ```
        
        - Publish command
        ```c
        dput ppa:nigelwang/hello1225 <source.changes>    
        ```
        - If above step is success, you will receive a email.
        ![](https://i.imgur.com/ElsdFqv.png)
        - Otherwise you will see below fail email. The reason is your package doesn't sign successfully.
        ![](https://i.imgur.com/l0SFlf5.png)
        - If above step all is normal, you can see your package in your ppa.
        ![](https://i.imgur.com/9JUQ9yF.png)
