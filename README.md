# Running SokuLobbies on Linux
## Original text by [Ham](https://github.com/MrsHammies), edited by Daichungus
This tutorial will guide you through running your own lobby for the SokuLobbies mod on Linux.

Some basic level of Linux knowledge is expected.

The Windows-version source code can be found in the [original SokuLobbies repo](https://github.com/PinkySmile/SokuLobbies).

Alternatively, you can clone the `linux` branch of [daichungus' fork](https://github.com/daichungus/SokuLobbies/tree/linux) to avoid modifying the `CMakeLists.txt` file yourself.

## Installing required software
### A C++ Compiler
Self explanatory, as the code is written in C++. Ham and daichungus both personally used GCC. 
#### Debian/Ubuntu:
```
sudo apt-get install g++
```
#### Fedora:
```
sudo dnf install gcc-c++
```

### CMake
Minimum required version of CMake is `3.15`. [^1]
#### Debian/Ubuntu:
```
sudo apt install cmake
```
#### Fedora:
```
sudo dnf install cmake
```
### SFML [^2]
#### Debian/Ubuntu:
```
sudo apt-get install libsfml-dev
```
#### Fedora:
```
sudo dnf install SFML-devel
```
## Opening a port
We're hosting a server, so we need to open a port. These tutorials should have you covered:
- [Debian](https://bobcares.com/blog/debian-open-port-8080/)
- [Fedora](https://jfearn.fedorapeople.org/fdocs/en-US/Fedora/20/html/Security_Guide/sec-Open_Ports_in_the_firewall-CLI.html)

Some notables:
 - Some Linux systems use `ss` instead of `netstat`
 - Some Linux systems don't have `iptables` as a service, so restarting and saving a service won't work (but don't worry, it only means that that step is not necessary). 

## Compiling SokuLobbiesServer
Now that we meet the requirements, we can start compiling.
For this step, we'd usually use Pinky's **CMakeLists** provided in the original repo. 

However, it needs the following modifications to work on most Linux distributions.
1. Actually listing the libraries we're retrieving from `SFML`.
2. Adding a new CMake flag (`-pthread` to enable parallel processing).

If you are using the `linux` branch of daichungus' fork as described at the beginning, you can move onto the next step.

Otherwise, if you are using the original source code for Windows, you can:
- Directly replace it with [the modified file](https://github.com/daichungus/SokuLobbies/blob/linux/CMakeLists.txt).
- Make the [changes yourself with your favorite editor](https://github.com/daichungus/SokuLobbies/commit/c8c9760f8ac20e5bda66fe125fe38d8b022489ae).

With everything set, we head to the src folder (should be in the root folder of SokuLobbies) and prepare our build

```
cmake .. -DCMAKE_BUILD_TYPE=Release -DMAIN_SERVER_HOST=pinkysmile.fr
```

Once it finishes, we finally build our files

```
cmake --build . --target SokuLobbiesServer
```
    
Our server is finally compiled, yay! We can run [^3] it with

```
./SokuLobbiesServer <port> <max players> <name of loby> <password (blank if none)>
```

If everything is right, it should have an output similar to the screenshot below: 

![](https://i.imgur.com/CnoN6qr.png)

## Keep the Lobby running
If it's your first time running a server, you might be wondering how to keep it running once you close your terminalsession. This is achieved by using a Linux utility called **`screen`**.

You can find a tutorial [here](https://linuxize.com/post/how-to-use-linux-screen/)

[^1]: When Ham originally wrote the CMake section, Debian did not have this particular version of CMake in its repos. Hence he included extra steps for possibly compiling CMake itself (which also required the OpenSSL Library)

[^2]: Ham was actually unsure if it's required to install the packages since the libraries that SokuLobbies snatches from are already included in the `src` directory, but Pinky said it was required.

[^3]: If you want to run the program from any directory, copy the `SokuLobbiesServer` binary with `sudo` to `/usr/local/bin`, or add the directory to `$PATH`. The first method is recommended by daichungus.
