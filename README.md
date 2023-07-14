# Passwordless-VPN

This repository demonstrates, in a straightforward manner, how you can configure your workspace to avoid entering a password each time you connect to the VPN server.

## Motivation

It was just an ordinary day, and I found myself contemplating the fact that I was exhausted from constantly switching between servers on my Cisco AnyConnect. The repetitive process of entering login credentials and passwords while jumping from one VPN to another was draining my energy, and I made up my mind to put an end to this torture.

Here's a guide on setting up your workspace to minimize the effort required to connect to a VPN server. By following these steps, you will be able to connect to the server with just a single word typed in the terminal, without the need to enter any login or password information.

Please note that this instruction is far from optimal and has several drawbacks. Therefore, any comments aimed at improving my approach are welcome. Additionally, please be aware that using this method will expose your password in a script, so it is crucial to consider additional protective measures :)

As I am using macOS, the instructions will be provided using macOS commands. However, they are quite similar to most Linux systems

## Install OpenConnect

We will begin by installing the OpenConnect VPN client (https://www.infradead.org/openconnect/), which supports several protocols, including the one used by Cisco AnyConnect. Here we will install it using Homebrew (https://brew.sh)

```bash
brew install openconnect
```
From here, the connection should be as straightforward as it seems: just connect to your desired server using:

```bash
openconnect server_adress.org
```
where server_address.org is the VPN address of your desired server.

However, you are most likely going to face some sort of error that will contain messages like "Operation not permitted." So, we have to resolve it first.

## Sudo

Every time we encounter a situation where we lack the necessary permissions to perform a certain action on our system, the first idea that comes to mind is to utilize the "sudo" command. This command grants us the ability to execute any command with superuser privileges.

Indeed, if we type

```bash
sudo openconnect server_adress.org
```
it will ask for the password of your system. After typing it, we will be able to enter the username and password of the VPN server, and voila, we are connected! :)


But it doesn't make our lives much easier. Right now, we have to do even more work rather than making a simple VPN connection with other utilities, as we are obligated to enter an additional password for the system because of "sudo".

## Breaking through "three walls."

The path to our VPN connection, via a single simple command, lies behind "three walls": your system password, VPN username, and VPN server password.

Let's begin with the system password. There is a way that allows you to avoid entering your system password when using "sudo." Additionally, we will introduce some neatness to our method by configuring it to not require a password for "sudo" specifically when used with the openconnect command.


Without any tedious theory or unnecessary explanations, to achieve this, you need to open the "etc/sudoers" file and append the following line at the end:

`"user" ALL = (root) NOPASSWD: "path_to_the_openconnect"`

Here, "user" represents the username of your system, and "path_to_the_openconnect" indicates the installation path of openconnect. If you installed it via brew, the path would be: /opt/homebrew/bin/openconnect.

Keep in mind that in order to make changes to the "/etc/sudoers" file, you need to have the appropriate permissions. Therefore, use the "sudo" command before your preferred text editor.

Reload your terminal. If you have done it correctly, you won't be required to enter your system password anymore when you type "sudo openconnect."

VPN usernames and server passwords are now easier to bypass. The "openconnect" command provides additional options that can be useful for our purposes. For example, the "-u" flag allows you to specify the username within the command, and the "--passwd-on-stdin" flag enables you to enter the password from the standard input.

Therefore the following command:

```bash
echo 'pass' | sudo openconnect server_adress.org -u vpn_username --passwd-on-stdin
```

Will automatically use the username "vpn_username" and retrieve the password "pass" from the standard input.

Try executing this command with your VPN username and VPN server password to see if it works.

Finally, you can create an alias in your .bashrc or .zshrc file, depending on the environment you are using:

`alias vpn_connect="echo 'pass' | sudo openconnect server_adress.org -u vpn_username --passwd-on-stdin"`

Now you will be able to start your VPN connection to the server simply by typing "vpn_connect" in the terminal. Enjoy! ;)
