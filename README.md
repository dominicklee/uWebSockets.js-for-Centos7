# uWebSockets.js for Centos7
Compiled uWebSockets.js binaries and instructions for those running on Centos7

## Overview ##
This repository is made for all the WHM admins and cPanel fans out there who want to installed uWebSockets.

Getting uWS to run on Centos 7 is **not easy** even for seasoned cPanel users. Although Centos 7 is a matured and robust OS normally used for running WHM and cPanel, it often contains dated dependencies that are just not quite enough to allow a proper install of uWS. I went through this struggle and would like to give some tips on how it can be done. Before we begin, I would like to say that **you must be a system admin with WHM and terminal access** in order for this tutorial to work.

uWebSockets is a powerful implementation of websockets for Node.js back-ends. I am very grateful to the author for the ever-improving code. Unfortunately, the binaries provided by default do not suffice and I have faced similar issues like these before:

[Can't run uWebSockets on CentOS 7 with glibc-2.17](https://stackoverflow.com/questions/59216325/cant-run-uwebsockets-on-centos-7-with-glibc-2-17 "Can't run uWebSockets on CentOS 7 with glibc-2.17")<br />
[How enable c99 mode in gcc with terminal](https://stackoverflow.com/questions/25566597/how-enable-c99-mode-in-gcc-with-terminal "How enable c99 mode in gcc with terminal")<br />
[GLIBC_2.18 not found (uws_linux_x64_72.node)](https://github.com/uNetworking/uWebSockets.js/issues/218 "GLIBC_2.18 not found (uws_linux_x64_72.node)")

In other words, you happily install uWS and when you run your first program, nothing works out and you may end up getting stuck for days. When I solved this issue, figuring it out was not easy but I am here to make this process easier for myself and others in the future.

**Disclaimer:** You understand and accept all risks as your own when performing any procedure in this document.  You acknowledge that you have backups of your system in case of any complications. The author shall not be responsible for any loss/damage due to this repository.

## Requirements ##
Before you follow the Installation Guide, you will obviously need an installation of NodeJS and NPM. Don't have those? Follow these instructions.

1. Run this to install NodeJS and NPM on CentOS running WHM: `yum install ea-nodejs10`
2. Run the following line-by-line to globally and locally create the symlink as needed:
```
ln -s /opt/cpanel/ea-nodejs10/bin/npm /usr/local/sbin/npm
ln -s /opt/cpanel/ea-nodejs10/bin/node /usr/local/sbin/node
ln -s /opt/cpanel/ea-nodejs10/bin/npm /usr/sbin/npm
ln -s /opt/cpanel/ea-nodejs10/bin/node /usr/sbin/node
```
3. That's all! You can check that you have NodeJS by running `node -v` and check NPM by running `npm -v` to see the version installed.

## Installation Guide ##

For convenience, I have compiled a binary of uWebsockets.js (at the moment v18) in this repository that will work on Centos 7, if that is all you want. For those who want to build their own binary and install, follow these instructions:

1. Open your terminal in Linux. CD into a new folder of your preference.
2. Clone the repo with submodules: `git clone --recursive https://github.com/uNetworking/uWebSockets.js.git`
3. CD into the folder: `cd uWebSockets.js`
4. Using your favorite text editor, open **build.c** and make the following change:
```
// Change from
build("clang", "clang++", "-static-libstdc++ -static-libgcc -s", OS, "x64");
// To this
build("gcc", "g++", "-static-libstdc++ -static-libgcc -s", OS, "x64");
```
5. Save it. Run the `make` command. If no errors, you should find the compiled binary in `uWebSockets.js/dist/uws.js`
6. **See any errors? No problem.** It probably means you need to update your GCC compiler. Its possibly a bit outdated. Lets update it. Run this: `sudo yum install centos-release-scl`
7. On RHEL, enable RHSCL repository for you system: `sudo yum-config-manager --enable rhel-server-rhscl-7-rpms`
8. Install the collection: `sudo yum install devtoolset-7`
9. Enable the software collections: `scl enable devtoolset-7 bash`
10. Now, you should be set. Check your GCC version by typing: `gcc --version`
11. Try **Step 5** again and you should be good to go!
12. For those of you who jumped all the steps and just downloaded my binary (you're welcome), all you need to do is slap that into the `node_modules` folder and start coding away.

## References ##
[uWebSockets Official Repository](https://github.com/uNetworking/uWebSockets.js "uWebSockets Official Repository")<br />
[Introduction to uWebSockets](https://edisonchee.com/writing/intro-to-%C2%B5websockets.js/ "Introduction to uWebSockets")<br />
[Install Dev Toolset 7 (for newer GCC compiler)](https://www.softwarecollections.org/en/scls/rhscl/devtoolset-7/ "Install Dev Toolset 7 (for newer GCC compiler)")