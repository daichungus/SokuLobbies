# Running Sokulobbies on Linux
## Original text by [Ham](https://github.com/MrsHammies), edited by Daichungus
This tutorial will guide you through running your own SokuLobby on Linux, as the current build is made for Windows. Some level of Linux knowledge is expected.
The short explanation is that we're compiling the code again using cmake and g++. The source code can be found in the [SokuLobbies repo](https://github.com/PinkySmile/SokuLobbies)

## What do you need to install
### A C++ Compiler
Self explanatory, the code is c++. I (Ham) personally used `g++`
#### Debian/Ubuntu:
```
sudo apt-get install g++
```
#### Fedora:
```
sudo dnf install gcc-c++
```

### CMake
Minimum required version of CMake is `3.15`, which is now supported by the latest versions of Debian.
#### Debian/Ubuntu:
```
sudo apt install cmake
```
#### Fedora:
```
sudo dnf install cmake
```
### SFML
I'm (Ham is) actually unsure if it's required since the libraries that Sokulobbies snatches from it are already included in the src folder, but Pinky said it was required.
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

## Actually compiling Sokulobbies code
Now that we meet the requirements, we can start compiling.
For this step, we'd use Pinky's **CMakeLists** provided in the original repo. Sadly, it needs some modifications to work in Linux. These are:

 - Actually listing the libraries we're retrieving from `SFML`
 - Setting a new `CMAKE_CXX_FLAGS`
If you don't understand what that means it's ok, just replace the original code in `CMakeLists.txt` for [this](https://pastebin.com/z54fVKcz), or use this repo's `CMakeLists.txt`

With everything set, we head to the src folder (should be in the root folder of SokuLobbies) and prepare our build

```
cmake .. -DCMAKE_BUILD_TYPE=Release -DMAIN_SERVER_HOST=pinkysmile.fr
```

Once it finishes, we finally build our files

```
cmake --build . --target SokuLobbiesServer
```
    
Our server is finally compiled, yay! We can run it with

```
./SokuLobbiesServer <port> <N°of players> <Name of Lobby> <Password (blank if none)>
```

If everything is right, it should look like ![this](https://i.imgur.com/CnoN6qr.png)

## Keep the Lobby running
If it's your first time running a server, you might be wondering how to keep it running once you close your session. This is achieved by using a Linux utility called **`screen`**. You can find a tutorial [here](https://linuxize.com/post/how-to-use-linux-screen/)
