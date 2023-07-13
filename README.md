# Passwordless-VPN

This repository demonstrates, in a straightforward manner, how you can configure your workspace to avoid entering a password each time you connect to the VPN server.

## Motivation

It was just an ordinary day, and I found myself contemplating the fact that I was exhausted from constantly switching between servers on my Cisco AnyConnect. The repetitive process of entering login credentials and passwords while jumping from one VPN to another was draining my energy, and I made up my mind to put an end to this torture.

Here's a guide on setting up your workspace to minimize the effort required to connect to a VPN server. By following these steps, you will be able to connect to the server with just a single word typed in the terminal, without the need to enter any login or password information.

Please note that this instruction is far from optimal and has several drawbacks. Therefore, any comments aimed at improving my approach are welcome. Additionally, please be aware that using this method will expose your password in a script, so it is crucial to consider additional protective measures :)

As I am using macOS, the instructions will be provided using macOS commands. However, they are quite similar to most Linux systems

## Setup


We will begin by installing the OpenConnect VPN client, which supports several protocols, including the one used by Cisco AnyConnect.

```bash
brew install openconnect
```
