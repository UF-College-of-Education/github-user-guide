# Using the Command Line/Terminal

The command line is a tool that lets you type commands directly to the computer without a graphic user interface (gui). On a Mac, the command line is often used through the Terminal app. Windows has also introduced a Windows Terminal app starting with Windows 11.

Other programs are available, too, and modern code editors like vscode provide a terminal directly in the app. You access this through the Terminal menu in vscode.


## Entering Terminal Commands

The command line works by entering commands that tell the machine what you want it to do. There are some commands that accessible everywhere like
	
	For the Current Directory:  ```cd``` 
    For the Listing Contents of Current Directory:  ```ls```  

Other commands are associated with a particular application with a CLI. This would include apps like Git and NPM. These kinds of commands will usually have 2 parts. The first part will be the app and the second part will be the specific action that you want it to take. For instance, you could have a command like:

```git log```


## Command Flags 

Some commands also allow you to pass in extra directions about how you want something carried out. These are called flags, and they will always be preceded by dashes. It will be one dash if it’s a one letter flag like “-a” and two dashes if the flag is a full word “--help”.

So for instance we can modify the command above with the ”--no-merges” flag or the “-p” flag. Putting this together it would look like:

```git log -—no-merges -p```


## Parameters

Certain commands also require additional information to be provided to work. This info is added as parameters usually at the end of the command. If multiple pieces of information are required these will be separated by spaces. The order of the parameters will vary depending on the app.

For example:

```git commit -m "This is the commit message"```


## Where To Run Commands

Everything you do on the command line happens in the context of a location on your machine. If you open the Terminal app, the command line will be open in the root folder of your user folder. If using the terminal in vscode, you open the folder via the file menu, and the terminal will automatically be set to that file location. 


### Basic Terminal Navigation Commands

Here are a few basic navigation commands you can use on the command line if you’re in the wrong folder.

***To enter a subfolder in the current folder:***
```cd <subfolder name here>/```

***To go to the parent folder of the current folder***
```cd ../```

To use the Git CLI, you first need to be in the correct folder on the command line. You should be in the root level of the folder where you have initiated the Git repo (we’ll explain that in a minute). 
