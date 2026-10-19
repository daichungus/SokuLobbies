# Running SokuLobbies on Linux

## Original text by [Ham](https://github.com/MrsHammies), edited by Daichungus

This tutorial will guide you through running your own lobby
for the SokuLobbies mod on Linux.

Some basic level of Linux knowledge is expected.

The Windows-version source code can be found in the
[original SokuLobbies repo](https://github.com/PinkySmile/SokuLobbies).

Alternatively, you can clone the `linux` branch of
[daichungus' fork](https://github.com/daichungus/SokuLobbies/tree/linux)
to avoid modifying the `CMakeLists.txt` file yourself.

## Installing required software

- A C++ Compiler

  Self explanatory, as the code is written in C++.
  Ham and daichungus both personally used GCC.

- CMake

  The minimum required version of CMake is `3.15`.

- SFML

### Debian/Ubuntu

```sh
sudo apt-get install g++ cmake libsfml-dev
```

### Fedora

```sh
sudo dnf install gcc-c++ cmake SFML-devel
```

## Opening a port

Since we're hosting a server, we need to open a port. These tutorials should have you covered:

- [Debian](https://bobcares.com/blog/debian-open-port-8080/)
- [Fedora](https://jfearn.fedorapeople.org/fdocs/en-US/Fedora/20/html/Security_Guide/sec-Open_Ports_in_the_firewall-CLI.html)

Some notables:

- Some Linux systems use `ss` instead of `netstat`
- Some Linux systems don't have `iptables` as a service,
  so restarting and saving a service won't work
  - But don't worry, it only means that that step is not necessary

## Compiling SokuLobbiesServer

Now that we meet the requirements, we can start compiling.
For this step, we'd usually use Pinky's **CMakeLists** provided in the original repo.

However, it needs the following modification to work on most Linux distributions.

Wrap the last three lines of `CMakeLists.txt` with the following `if()` statement.

```cpp
if (MSVC)
SET(CMAKE_EXE_LINKER_FLAGS_RELEASE "${CMAKE_EXE_LINKER_FLAGS_RELEASE} /DEBUG /OPT:REF /OPT:ICF")
SET(CMAKE_MODULE_LINKER_FLAGS_RELEASE "${CMAKE_MODULE_LINKER_FLAGS_RELEASE} /DEBUG /OPT:REF /OPT:ICF")
SET(CMAKE_SHARED_LINKER_FLAGS_RELEASE "${CMAKE_SHARED_LINKER_FLAGS_RELEASE} /DEBUG /OPT:REF /OPT:ICF")
endif()
```

This will allow the program to be compiled on Linux systems.

Next, create a `build/` directory.

```sh
mkdir build
cd build/
```

Then, generate the CMake files for compiling.

```sh
cmake .. -DCMAKE_BUILD_TYPE=Release -DMAIN_SERVER_HOST=pinkysmile.fr
```

Once it finishes, we finally build our files

```sh
cmake --build . --target SokuLobbiesServer
```

Our server is finally compiled, yay! We can run it within the directory with:

```sh
./SokuLobbiesServer <port> <max players> <name of lobby> [password (blank if none)]
```

If everything is right, it should have an output similar to the snippet below

```sh
you@example:~/SokuLobbies/build$ ./SokuLobbiesServer 10800 42 test P@s5M0Rd
Password is 'P@s5M0Rd'
Listening on port 10800
> Registering lobby 'test' to the server: 42 max slots
Main server is pinkysmile.fr:5254
```

Any new connections or disconnections will be logged as well.

Use `/help` for a full list of admin commands.

**NOTE:** If you want to run the program from anywhere
on your system, copy or move the binary to a directory
included in your shell's `PATH` environment variable,
such as `/usr/local/bin`.

```sh
sudo cp SokuLobbiesServer /usr/local/bin
```

Substitute `/usr/local/bin` with `$HOME/.local/bin`
or similar if you have a preferred install location.

## Keep the Lobby running

If it's your first time running a server,
you might be wondering how to keep it
running once you close your terminal session.

One method is a Linux utility called **`screen`**.

You can find [a tutorial here](https://linuxize.com/post/how-to-use-linux-screen/)
