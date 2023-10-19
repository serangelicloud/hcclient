<div align="left">
<h1> hcclient <br /> <a target="_blank" href="https://github.com/AnnikaV9/carrotsh/blob/master/LICENSE" title="License"><img src="https://img.shields.io/static/v1?label=License&message=The%20Unlicense&color=blue&style=flat-square"></a></h1>
A terminal client for connecting to <a href="https://hack.chat">hack.chat</a>

<br />
<br />

<img src="https://github.com/AnnikaV9/hcclient/assets/68383195/bdd7ff1c-55c0-4585-abab-1506bd6949f2" width="100%"></div>

<br />

## Table of Contents
- [Introduction](#introduction)
- [Features](#features)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Usage](#usage)
- [Colors](#colors)
- [Configuration](#configuration)
- [Known Issues](#issues)
- [Contributing](#contributing)

<br />

## Introduction <a name="introduction"></a>
*"hack.chat is a minimal, distraction-free, accountless, logless, disappearing chat service which is easily deployable as your own service."* - [hack.chat](https://github.com/hack-chat/main)

hcclient is a configurable terminal client for connecting to [hack.chat](https://hack.chat).

**Note:** &nbsp;As this client is written to be compatible with the official live instance running at https://hack.chat, compatibility with your own self-hosted instance or other alternate instances is not guaranteed.

<br />

## Features <a name="features"></a>
- **Cross-platform:** &nbsp;No platform specific modules used, should work fine on most systems.
- **Color theming:** &nbsp;Configured with command line flags, colors provided by termcolor.
- **Send/Receive raw packets:** &nbsp;Send json packets without parsing with `/raw`, display received packets as json with `--no-parse`.
- **Suggestions:** &nbsp;Starting your message with `@` will bring up a menu with a list of online users. Cycle through them with arrow keys or continue typing to filter the suggestions even more. Outside of the menu, arrow keys cycle through message history.
- **Overwrite prevention:** &nbsp;Text that you have typed but not sent is stored and refilled to a new prompt whenever a message gets received. You can type long messages as slow as you want, and received messages won't overwrite your progress.
- **Desktop notifications:** &nbsp;Receive notifications whenever someone mentions you or sends you a whisper. (Not supported in container mode)
- **Moderator mode:** &nbsp;Enabled with `--is-mod`, gives you a bunch of `/` commands for moderator actions. Moderator commands are not documented in `/help`, check the source code for the list of available ones and their parameters.

<br />

## Prerequisites <a name="prerequisites"></a>
Either [Docker](https://docs.docker.com/engine/) or [Podman](https://github.com/containers/podman) is recommended. The container image is built with hcclient's dependencies.  
<br />
You *can* run the client directly without using a container, on non-linux systems however:
- This requires python >= 3.10 and pip. <br />
- You'll have to install pip dependencies locally or in a virtual environment.<br />
- The `tput` command (provided by ncurses) is an optionally dependency that allows saving and restoring terminal contents.

For linux based systems, statically and dynamically linked x86-64 binaries are provided. Only `tput` is an external dependency.

<br />

## Installation <a name="installation"></a>

On Linux based systems:
```
# Download the latest binary
wget -O hcclient https://github.com/AnnikaV9/hcclient/releases/download/v1.4.2/hcclient-1.4.2-linux-x86-64

# Or the statically linked binary if the above one doesn't work
wget -O hcclient https://github.com/AnnikaV9/hcclient/releases/download/v1.4.2/hcclient-1.4.2-linux-x86-64-static

# Make the binary executable
chmod +x hcclient

# Move it to somewhere in PATH
mv hcclient ~/.local/bin/

# Run hcclient
hcclient --help
```
On other platforms:
```
# Download the latest source release
wget https://github.com/AnnikaV9/hcclient/archive/refs/tags/v1.4.2.tar.gz

# Extract the archive
tar xvf v1.4.2.tar.gz

# Change the working directory
cd hcclient-1.4.2

# Install the dependencies
pip install -r requirements.txt

# Run hcclient
python hcclient --help
```
As a container:
```
# Download the latest image
wget https://github.com/AnnikaV9/hcclient/releases/download/v1.4.2/hcclient-1.4.2-image.tar.xz

# Install the image
docker/podman load -i hcclient-1.4.2-image.tar.xz

# Run hcclient
docker/podman run --rm -it hcclient --help
```
<br />

## Usage <a name="usage"></a>
```
$ hcclient --help

usage:  [-h] -c CHANNEL -n NICKNAME [-t TRIP_PASSWORD] [-w WEBSOCKET_ADDRESS]
        [--no-parse] [--no-clear] [--is-mod] [--no-icon]
        [--message-color MESSAGE_COLOR] [--whisper-color WHISPER_COLOR]
        [--emote-color EMOTE_COLOR] [--nickname-color NICKNAME_COLOR]
        [--warning-color WARNING_COLOR] [--server-color SERVER_COLOR]
        [--client-color CLIENT_COLOR] [--timestamp-color TIMESTAMP_COLOR]
        [--mod-nickname-color MOD_NICKNAME_COLOR]
        [--admin-nickname-color ADMIN_NICKNAME_COLOR]

Terminal client for connecting to hack.chat servers. Colors are provided by
termcolor.

options:
  -h, --help            show this help message and exit

required arguments:
  -c CHANNEL, --channel CHANNEL
                        specify the channel to join
  -n NICKNAME, --nickname NICKNAME
                        specify the nickname to use

optional arguments:
  -t TRIP_PASSWORD, --trip-password TRIP_PASSWORD
                        specify a tripcode password to use when joining
  -w WEBSOCKET_ADDRESS, --websocket-address WEBSOCKET_ADDRESS
                        specify the websocket address to connect to (default:
                        wss://hack-chat/chat-ws)
  --no-parse            log received packets without parsing
  --clear               enables clearing of the terminal
  --is-mod              enables moderator commands
  --no-icon             disables moderator/admin icon
  --no-notify           disables desktop notifications
  --message-color MESSAGE_COLOR
                        sets the message color (default: white)
  --whisper-color WHISPER_COLOR
                        sets the whisper color (default: green)
  --emote-color EMOTE_COLOR
                        sets the emote color (default: green)
  --nickname-color NICKNAME_COLOR
                        sets the nickname color (default: white)
  --warning-color WARNING_COLOR
                        sets the warning color (default: yellow)
  --server-color SERVER_COLOR
                        sets the server color (default: green)
  --client-color CLIENT_COLOR
                        sets the client color (default: green)
  --timestamp-color TIMESTAMP_COLOR
                        sets the timestamp color (default: white)
  --mod-nickname-color MOD_NICKNAME_COLOR
                        sets the moderator nickname color (default: cyan)
  --admin-nickname-color ADMIN_NICKNAME_COLOR
                        sets the admin nickname color (default: red)
  --version             displays the version and exits
```

<br />

## Colors <a name="colors"></a>
The default color scheme can be overidden by using arguments such as `--message-color` or `--timestamp-color`. More options can be viewed with `--help`. Colors are provided by [termcolor](https://pypi.org/project/termcolor/). Valid colors are:
- grey
- red
- green
- yellow
- blue
- magenta
- cyan
- white

<br />

## Configuration <a name="configuration"></a>
hcclient does not save to or read from any configuration file. It might be troublesome to have to type long passwords and colors every time you wish to connect. You can create a shell script to act as a profile and call hcclient with your preferred arguments. For example:

*profile1.sh*
```bash
#!/bin/bash
hcclient --trip-password mypassword \
         --server-color red \
         "$@"
```
or
```bash
#!/bin/bash
hcclient --trip-password mypassword \
         --timestamp-color green \
         --no-clear \
         "$@"
```
The profile can then be used like:
```
sh profile1.sh -c mychannel -n mynick
```

<br />

## Known Issues <a name="issues"></a>

- Not compatible with hack.chat's new `updateMessage` implementation. You won't be able to see the output of any bots that use `updateMessage` to display delayed/streamed output.

- Some terminal emulators will have locked scrolling when hcclient is run with `--clear`. This is an issue with how the terminal emulators interact with the alternate screen `tput smcup` invokes. There's no fix for this at the moment, so run hcclient without `--clear` if you want to be able to scroll.

<br />

## Contributing <a name="contributing"></a>

All contributions are welcome! :D

**Credits to everyone [here](https://github.com/AnnikaV9/hcclient/graphs/contributors)**
