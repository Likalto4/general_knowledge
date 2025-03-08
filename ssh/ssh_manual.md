# Introduction to SSH

The connection to our UdG servers is done through the SSH protocol. This protocol is secure and allows you to connect to the server and execute commands on it.
There are many ways to connect to a server via SSH, but the two most common methods we use in the lab are: using a terminal or using VSCode with the Remote-SSH extension.

First of all, be sure that SSH is installed in your machine.
You can check this by typing:
```bash
ssh -V
```

This will show you the version of the SSH client installed in your machine. If you see a version number, then SSH is installed.

If you see an error message, then you need to install SSH. To do this follow the instructions for your operating system. For Ubuntu:
```bash
sudo apt-get install openssh-client
```

Notice that you only need to install the SSH client, not the server, as we are going to connect to a server, not create one.

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

To exit, press that same blue button and select `Disconnect from Host`. In theory, you can close the VSCode window and you will still be connected to the server, but it is better to disconnect before closing the window.

### Access to the server without password

If you are tired of typing the password every time you connect to the server, you can set up an SSH key. This is a more secure way to connect to the server, as you don't need to type the password every time you connect. You can follow [this nice tutorial](https://www.youtube.com/watch?v=PDVnUErS_us) to do it.

Essentially, you have to do the following:

1. Generate an SSH key pair in your local machine:
```bash
ssh-keygen -t ed25519
```
You could select between rsa, ed25519 or ecdsa. Search more about the differences between them to choose the one that fits you better. In general, for modern systems, ed25519 is the best choice.

This command will generate two files: `id_ed25519` and `id_ed25519.pub`. The first one is the private key and the second one is the public key.

2. Copy the public key to the server. For this, open the public key file and copy its content.

Then, connect to the server and run the following command:
```bash
echo "public_key_content" >> ~/.ssh/authorized_keys
```
Where `public_key_content` is the content of the public key file. This will add the public key to the authorized keys file in the server. You can also manually open the authorized keys file and paste the public key there using nano or vim.

Note: If the authorized keys file does not exist, you can create it by running:
```bash
touch ~/.ssh/authorized_keys
```

3. In the VScode SSH configuration file, add the following line to the configuration of your server:
```bash
IdentityFile ~/.ssh/id_ed25519
```
This will tell VSCode to use the private key to connect to the server.

Now, when you connect to the server, you will not be asked for the password. You will be connected directly!


# Best practices

You are now connected to the server, but there are some best practices you should follow to avoid problems and to keep the server running smoothly.

1. Check your resources:

Remember there are <b>other users</b> connected to the server. Be respectful and don't do anything that could affect them. For example, don't run heavy processes that could slow down the server. The best way to know the memory and CPU usage is by running:
```bash
htop
```

This will show you the processes running on the server and the resources they are using, organized by user name, PID (tag), CPU usage, memory usage, etc. You can also see the overall memory and CPU usage at the top of the screen.

For GPU usage, you can run:
```bash
nvidia-smi
```

This will show you the GPU usage, memory usage, temperature, etc. Also, if you ever wonder the Nvidia driver, GPU model and cuda version you can find this information in the first lines of the output.

Finally, for disk usage, you can run:
```bash
df -h
```

This will show you the disk usage of the server in general.

If you want to know the disk usage of your specific user directory
```bash
du -sh /home/username
```
    
Where `username` is your username.

2. Killing processes:

Sometimes, some processes can get stuck and use a lot of resources. If you see that the server is slow, you can kill a process by running:
```bash
kill -9 PID
```

Where `PID` is the process ID. You can find the PID by running `htop` or `nvidia-smi`. Be careful when killing processes, as you could kill a process that is important for the server. Only kill processes that you know are not important.

4. Specify the GPU in your code
When using the GPU, always specify the GPU you want to use in your code. This is important because if you don't specify the GPU, your code could use all the GPUs on the server, affecting other users. To specify the GPU in your code, you can use the following code:

```python
## extra imports to set GPU options
import torch
###################################
selected_gpu = 0 # here you select the GPU used (0, 1 or 2)
device = torch.device("cuda:" + str(selected_gpu) if
torch.cuda.is_available() else "cpu")
```
There may be other ways to specify the GPU in your code, but this is a simple way to do it.

3. General information about the server:
Fastfetch is a tool that shows you information about the server you are connected to.
```bash
fastfetch
```

# Tmux

Tmux is a terminal multiplexer. It allows you to run multiple terminals in the same window. This is useful when you want to run multiple processes at the same time, or when you want to keep a process running even if you close the terminal. This is ideal for us, as we usually run processes that may take hours to finish (like training models or processing data).

Assuming tmux is installed (already in our servers), you can start a new tmux session by running:
```bash
tmux
```
When you run this command, you will see a green bar at the bottom of the terminal. This means you are now in a tmux session. You can now run any command you want, and it will be running in this session. Even if you close the terminal, the process will keep running.

If you want to leave the tmux session but keep it running in the background you can press `Ctrl+b` and then `d`. This will detach the session and you will be back to the terminal.

If you already have a tmux session running, you can access it again by running:
```bash
tmux attach
```

Before we move to the next section, keep in mind two important features that are going to be useful when you are using tmux: the
- <u>The prefix key:</u> This is the key that you press before running a tmux command. By default, the prefix key is `Ctrl+b`. This means that if you want to run a tmux command, you have to press `Ctrl+b` and then the command.
- <u>The command key</u>: This is the key that you press to run a tmux command. By default, the command key is `:`. This means that if you want to run a tmux command, you have to press `Ctrl+b` and then `:`.

## How does tmux work?

Tmux works with the concept of sessions, windows, and panes.

<u>Sessions</u> are the highest level of organization. You can have multiple sessions running at the same time. You can create a new session by running:
```bash
tmux new -s session_name
```

Where `session_name` is the name of the session. Sessions are usually used to group processes that are related to each other. For example, if you are working on a project about "skin lession" then you can create a session called "skin_project".

To close a session, you can press `Ctrl+b` and then `:` and then type `kill-session`. To rename a session, you can press `Ctrl+b` and then `$`.

<u>Windows</u> are the second level of organization. You can have multiple windows in a session. They are like tabs in a browser. You can create a new window by pressing `Ctrl+b` and then `c`. You can switch between windows by pressing `Ctrl+b` and then the window number.

To close a window you can press `Ctrl+b` and then `&`. You could also go the window you want to close and type `exit` in the terminal. To rename a window, you can press `Ctrl+b` and then `,`.

<u>Panes</u> are the third level of organization. You can have multiple panes in a window. They are like split screens. You can create a new pane by pressing `Ctrl+b` and then `%` (for a vertical split) or `"` (for a horizontal split). You can switch between panes by pressing `Ctrl+b` and then the arrow key in the direction you want to go.

To close a pane, you can press `Ctrl+b` and then `x`. You can also go to the pane you want to close and type `exit` in the terminal.

For more tmux commands take [a look here](https://tmuxcheatsheet.com/).

## Improving tmux experience

1. Depending on the IDE you are using for coding, it may be wise to change the defualt prefix key of tmux. The reason is, `ctrl+b` is also used for other shortcuts and they may interfere. I recommend changing it to `ctrl+a`, for instance but you can choose any other key you like.

To change the prefix key, first open the tmux configuration file by running:
```bash
nano ~/.tmux.conf
```
(<i>If the file does not exist, you can create it by running `touch ~/.tmux.conf`</i>)

Then add the following line to the file:
```bash
set -g prefix C-a
```

(Optional) Unbind the default prefix key:
```bash
unbind C-b
```

After saving the file, you can reload the tmux configuration by running:
```bash
tmux source-file ~/.tmux.conf
```

2. In my opinion, the original tmux visual design is not very user-friendly. If you want to improve your tmux experience, you can take a look at [this repository](https://github.com/gpakosz/.tmux).