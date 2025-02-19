# Introduction

The connection to our UdG servers is done through the SSH protocol. This protocol is secure and allows you to connect to the server and execute commands on it.
There are many ways to connect to a server via SSH, but the two most common methods we use in the lab are: using a terminal or using VSCode with the Remote-SSH extension.

## Using a terminal
This is the most basic connection you can make to the server.
The command is:
```bash
ssh username@server -p port
```

Where `username` is your username, `server` is the server address and `port` is the port number.

e.g.
```bash
ssh ricardo@padel.udg.edu -p 1234
```

After executing the command, you will be asked for the password. The first time you connect to the server, you will be asked to accept the server's fingerprint. Type `yes` and press `Enter`.

And that's it! You are now connected to the server. All commands that you run now in the command line are going to be run on the server, os be careful!

To exit the server, type `exit` and press `Enter`.

## Using VSCode

Using the command line is a good way to start, specially to be sure that the SSH connection can be established. But if you are going to work on the server for a long time, it is better to use VSCode with the Remote-SSH extension. The advantages of using VSCode SSH connection are mainly that you will be working like if you were on your local machine, but with the power of the server.

1. Install the Remote-SSH extension in VSCode.
    - Open the Extensions view by clicking on the square icon in the sidebar.
    - Search for Remote-SSH.
    - Click on Install.
2. Open the Command Palette (Ctrl+Shift+P) and type `Remote-SSH: Connect to Host...`, and select it. Then click on `Add New SSH Host...`.
3. Enter the server address, username and port. (same as in the terminal)
```bash
username@server -p port
```
4. Choose the SSH configuration file to save this connection (~/.ssh/config is the default). This will be the file where the connection information will be saved. You can always change and edit this file later.

5. After saving the configuration, you will be asked for the password. Type it and press `Enter`. The first time you access you may be asked to accept the server's fingerprint. Type `yes` and press `Enter`.

That's it! You are now connected to the server using VSCode. You can now open a terminal (ctrl+shift+`) in VSCode and run commands on the server, or open a file and edit it directly on the server.

To know whether you are connected to the server or not, look at the bottom left corner of the VSCode window. You will see the name of the server you are connected to.

To exit, press that same blue button and select `Disconnect from Host`.