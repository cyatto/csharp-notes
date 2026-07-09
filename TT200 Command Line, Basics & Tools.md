# TT200 Command Line, Basics & Tools

# 1. 0 Timekeeping

🕐 ETA: 6 hours

📆 2026.06.23

|block|section|begin|end|total|
| -------| -----------------| ----------| ----------| ----------|
|1|TT 2.1|15:32:49|16:27:27|0:54|
|2|YB200 pp. 15-21|16:45:00|16:59:18|0:14|
|3|TT 2.2|17:00:02|18:14:27|01:14:25|

---

# 2. 1 Basics and Files

- Unix Shell: why?

  - standard deployment platform for applications is Linux
  - a web application is likely deployed on a Linux server
  - most tooling and scripts have been setup for these shells whereas Windows is not always given the same amount of consideration
  - the *unix shell* is used in informatics heavily as a lot of commands and features allow for composition and piping of data between each other

‍

## 2.1 Concepts

- filesystem, directories and fiels
- pathing and naming
- commands, programs and processes
- command line arguments and flags

‍

### 2.1.1 Filesystem

- primary usecase of the shell: interact with the *filesystem:*  a representation of what is on the hard drive
- made up of *files* and *directories*
- *file:*  holds data (an image, text file, a program itself)
- *directory* (aka folder) can contain a number of files or folders
- *filesystem*: a system used to organsie both files and directories; many different kinds of filesystems but the primary interaction and model is a **Tree** structure
- *Tree structure:*  both Windows and Linux utilise this structure; everything starts at the root directory which is either `/`​ or commonly for Windows `C:\`
- *file paths*: outline where, relative to the current position or relative to the root of the filesystem, a file is located

  - *absolute path*: based on the root of the filesystem or drive
  - *relative path*: based on a position that could be anywhere on the system
  - examples:

    - `C:\Windows`​ - an absolute path; points to the `Windows` directory in the C drive
    - `/home/user/Documents`​ - an absolute path, based on the `/` root of the filesystem
    - `../Documents/notes`​ - relative path where `..`​ indicates the parent directory (folder that contains `Documents`)
    - `Videos\FilesFromDecember\log.mp4`​ - relative path, whichever directory that the user is currently in, they expect `Videos`​ to exist within it and `FilmsFromDecember` to be nested inside it
    - Note: Windows uses `\`​ for separating directories; Linux and Mac uses `/`.

‍

### 2.1.2 Pathing and Naming

- File Extension: files don't necessarily need to have an extension; they can exist without them. However, they are helpful for the operating system so it knows what application should launch with it.

‍

### 2.1.3 Knowledge Tasks

#### 2.1.3.1 1 Shells

1/ Find an example of a shell, specifically find information that will inform you what the differences are between Bash, Zsh and Powershell are.

1. [https://www.freecodecamp.org/news/linux-shells-explained/](https://www.freecodecamp.org/news/linux-shells-explained/)
2. [https://zellwk.com/blog/bash-zsh-fish/](https://zellwk.com/blog/bash-zsh-fish/)
3. [en.wikipedia.org/wiki/Comparison_of_command_shells](https://en.wikipedia.org/wiki/Comparison_of_command_shells)
4. [medium.com/@natarajanck2/exploring-command-line-shells-bash-zsh-...](https://medium.com/@natarajanck2/exploring-command-line-shells-bash-zsh-fish-and-powershell-1d87f14b9007)

‍

- A shell is a ***command interpreter***: sends typed commands to the operating system for execution and then displays the output.
- Shells give you direct, scriptable control over your system (unlike GUIs)
- **Bash** (Bourne Again Shell) - the classic Unix workhorse:

  - the default command-line shell for most Linux distros and early macOS, created in 1989
  - Main features: standard on nearly all Unix-like systems; supports shell scripting, loops, variables, and functions; rich command history and job control (`fg`​, `bg`​, etc.); powerful for automation with shell scripts (`.sh` files)
  - Where it's used: default shell on most Linux systems; available on MacOS and even Windows (via WSL); core to DevOps pipelines, server administration, and scripting.
  - How it runs: bash runs as a user-space process, interpreting text commands entered or those stored in script files; communicates directly with the OS kernel via system calls
  - Why use Bash?: when you need: maximum compatibility across Unix systems; mature scripting support; easy integration into CI/CD, Docker, and server environments.
- **Zsh** (Z Shell) - a superset of Bash - compatible with most Bash commands but more modern and feature-rich

  - Main features: advanced autocompletion (commands, files, Git branches); syntax highlighting and error detection; theme and plugin system (via Oh My Zsh or Prezto); shared command history between sessions; path explansion and inline globbing (`**/*.js`).
  - Where it's used: default on MacOS (since Catalina 10.15); popular among developers who spend lots of time in the terminal
  - How it runs: behaves like Bash at the core but adds extensions that hook into shell startup. Config files include `.zshrc` and plugin loaders.
  - Why use Zsh?: if you want fast, modern, visually appealing shell that still feels familiar to Bash users
- **Fish** (Friendly Interactive Shell) - a user-focused alternative to Bash and Zsh; designed for simplicity and discoverability rather than backward compatibility

  - Main features: smart suggestions and autosuggestions as you type; built-in syntax highlighting; web-based config via `fish-config`​; no need to remember flags - command completion helps interactively; simpler syntax than Bash (no `$`, minimal punctuation)
  - Where it's used: runs on Linux, MacOs, and Windows (via WSL or native builds). Great shell for personal use and development environments.
  - How it runs: runs like any other shell but with its own interpreter - doesn't rely on POSIX compatibilty, which makes it easier to use but less script-portable.
  - Why use Fish?: user-friendly, aesthetically pleasing, efficient for interactive work - not necessarily scripting for production servers.
- **PowerShell** - developed by Microsoft in 2006 for Windows admin - now open-source and cross-platform; unlike Unix shells that pass plain text, PowerShell uses structured  **.NET objects**, making it incredibly powerful for automation.

  - Main features: object-oriented: commands (called *cmdlets*) pass rich data structures; integrated with the Windows API, Azure, and Active Directory; supports pipelines, but instead of text, they pass objects (`|`); cross-platform (Linux, MacOS, Windows); advanced scripting language for automation and DevOps.
  - Where it's used: Windows Server and enterprise environments; cross-platform automation in Azure DevOps, Kubernetes, or cloud workflows.
  - How it runs: runs within the .NET runtime (CoreCLR), interpreting cmdlets and converting them into system or API-level calls.
  - Why use PowerShell: for Windows administration, cloud automation, or when working with APIs and data that benefit from object-based output.

‍

#### 2.1.3.2 Linux server deployment - what domains?

*2/ Given the scope of the document, consider why deploying on Linux Servers is very common. Consider what domains (like Software Development, Computer Graphics, Web Development, Informatics) linux is used in.*

- [www.logicmonitor.com/blog/why-linux-is-a-popular-choice-for-serv...](https://www.logicmonitor.com/blog/why-linux-is-a-popular-choice-for-servers)
- [www.geeksforgeeks.org/linux-unix/introduction-to-linux-operating...](https://www.geeksforgeeks.org/linux-unix/introduction-to-linux-operating-system/)
- [www.vodien.com/learn/what-is-linux-and-why-to-use-it/](https://www.vodien.com/learn/what-is-linux-and-why-to-use-it/)
- [www.inf.uni-hamburg.de/en/inst/irz/software/linux.html](https://www.inf.uni-hamburg.de/en/inst/irz/software/linux.html)

‍

Linux servers are the backbone of modern IT infrastructure:

- known for its stability and security, makes it a go-to for mission-crtiical workloads
- runs across cloud, on-premises, bare metal, and hybrid environments with no vendor lock-in
- a wide range of distributions; gives you the flexibility to match the workload
- Linux may support your infrastructure across AWS, Azure, GCP, or on-premises

‍

IT downtime costs organisations an average of $9,000 per minute, or more than 1 million per hour (when websites crash, transactions fail, or internal systems go offline) --> avoiding these losses starts with choosing the right server OS

- the OS sets the foundation for how stable, secure and cost-efficient your infrastructure will be
- proprietary platforms often come with high licensing fees and limited flexibility; Linux is open-source, customisable, and tested across enterprise, cloud and large scale production environments

‍

What is a Linux server?

- runs on the Linux OS, an open-source OS built on the Linux kernel; widely used in enterprise IT because it's stable, cost-efficient, and adaptable to different workloads.
- terminology:

  - **Linux server OS:**  the operating system itself (a distribution like Ubuntu Server or Red Hat Enterprise Linux that bundles the kernel, libraries and tools)
  - **Server hardware:**  the physical, virtual or cloud infrastructure the OS runs on (bare metal, a VMware VM, or a cloud instance on Amazon EC2)
  - **Hosted workload:**  what runs on top (a web server, database, container, or business application)
- A "Linux server" usually refers to all three working together: the Linux OS running on server hardware to host workloads.
- Linux is used by roughly 61% of websites where the operating system is known.

‍

### 2.1.4 How a Linux Server Is Structured

Linux follows a layered architecture. Each layer has a defined job, which makes the system easier to secure and monitor.

|**Layer**|**What it does**|**Examples**|
| ----------------------| --------------------------------------------------------------------| -----------------------------------------------------|
|Hardware|Provides physical or virtual compute, memory, storage, and network|Bare metal server, VMware VM, AWS or Azure instance|
|Linux kernel|Bridges hardware and software; manages resources|Process scheduling, memory allocation, I/O, drivers|
|System libraries|Reusable code that applications call into|glibc, OpenSSL, and PAM libraries|
|User-space utilities|Tools administrators run from the shell|bash, grep, ssh, systemctl|
|Daemons and services|Background processes that handle requests|sshd, nginx, postgres|
|Applications|Workloads end users consume|Web apps, databases, APIs|

‍

##### 2.1.4.0.1 Linux Server Architecture

![linuxServerArchitecture](assets/linuxServerArchitecture-20260623161703-l2aw14g.svg)

#### 2.1.4.1 Domains

- Servers: web servers, database servers, other critical infrastructure
- Cloud computing: cloud providers like AWS, Google Cloud, Microsoft Azure
- Software dev: supoprt for virtually all programming languages, wide range of dev tools, ability to run a server envrionment locally
- Desktop use: while less common than Windows or macOS, Linux is gaining traction in desktop environments; some distros offer user-friendly interfaces which makes it more accessible to everyday users; for privacy and security, Linux provides an alternative that minimises exposure to proprietary tracking and data collection practices.
- Embedded systems: e.g. smart home devices, automotive infotainment systems, industrial automation; its lightweight nature and ability to run on various hardware achitecture make it an ideal choice for these apps.
- Web hosting: Apache, MySQL, PHP (foundational to web dev); open-source nature means that hosting providers and devs can customise their environments to optimise performance and security
- Informatics: the interdisplinary science of how humans and machines interact with information;

‍

3/ Given the brief introduction of a filesystem, provide some concrete examples of a file. What are some file types you commonly open and how do you organise your files?

‍

---

‍

# 3. 2.2 Commands, Programs and Processes

‍

## 3.1 2.2.1 Commands

- Commands are programs that the shell has access to and do not require knowledge in where they live. New commands can be added or even removed from your computer (likely unadvisable!)
- *Program:*  the file that holds computer instructions native to the CPU and OS; it is a container of instructions that can be loaded and executed.
- *Process:*  an *instance of a program*; in that sense, the same program can be run multiple times and usually concurrently; each time a program is run, we are creating a process that executes the instructions held by the program.

‍

### 3.1.1 Commanding the filesystem

- `pwd` - Print Working Directory
- `cd` - Change Directory
- `ls` - List contents of a directory
- `mkdir` - Make directory
- `touch` - Create a file
- `rm` - Remove a file (and with flags, directories)
- `mv` - Move a file (or rename it)
- `cat`​ - Reads a file and outputs it to *standard output*
- `echo`​ - Print to *standard output*

‍

## 3.2 2.2.2 Tour!

- `~`​ - represents the home directory, usually where the current `user` will store their files
- `pwd` - print working directory; find out what the current path is

```terminal
cyatto@MeoBook ~ % pwd
/Users/cyatto
```

- `ls`​ - list contents of a directory; get the contents of our `home` directory:

```terminal
cyatto@MeoBook ~ % ls
Applications			Soulseek Downloads
Applications (Parallels)	Startup
Apps				Study
Backups				Study2
Desktop				Sync
Documents			Txt
Downloads			Waiting
Hoe				books
LINQPad				cyatto2
Library				dotfiles
Movies				electronics
Music				gallery-dl.conf
Parallels			gearexample.rb
Pictures			hours.db
Public				iCloud Drive (Archive)
Reading				lpm
School				make
Scripts				sinatra_test.rb
Soulseek Chat Logs		wget-log
```

- `touch` - create a file
- `rm` - remove a file

```terminal
cyatto@MeoBook ~ % touch hello.txt
cyatto@MeoBook ~ % ls
hello.txt
cyatto@MeoBook ~ % rm hello.txt
cyatto@MeoBook ~ % ls

```

‍

- ⚠️ prior to this command, we have just been using commands without inputting **Command Line Arguments**

  - we have now just specified our first argument with the `rm`​ command --> `rm hello.txt`
  - this allows us to communicate that the filename to `rm` knows what is to be removed

‍

- `mkdir` - make directory
- `mv` - move a file (or rename it)

```terminal
cyatto@MeoBook ~ % mkdir Projects
cyatto@MeoBook ~ % touch hello.txt
cyatto@MeoBook ~ % mv hello.txt Projects/hello.txt
cyatto@MeoBook ~ % ls Projects
hello.txt
```

- the file has moved from `~`​ to `~/Projects`

  - or another way we can look at this is that the file at `/home/user/hello.txt`​ is now at `/home/user/Projects/hello.txt`

‍

- `cd` - change directory

```terminal
cyatto@MeoBook ~ % cd Projects
cyatto@MeoBook Projects % pwd
/Users/cyatto/Projects
cyatto@MeoBook Projects % ls
hello.txt
```

- we have moved our current working directory (cwd) to `home/user/Projects`
- `echo` - print to standard output

```terminal
cyatto@MeoBook Projects % echo 'Woohoo, using the command line'
Woohoo, using the command line
```

- we can observe we have printed the text we inputted as an argument for `echo`
- `>` - allows us to redirect output of a program to a different destination; allows us to take the output of a program and write it to a file.

  - the program doesn't output to the shell again; run `cat hello.txt` to confirm that it wrote to the txt file

```terminal
cyatto@MeoBook Projects % echo "Woohoo, using the command line" > hello.txt
cyatto@MeoBook Projects % cat hello.txt
Woohoo, using the command line
```

‍

---

## 3.3 2.3 Terminal Shortcuts

- note down some of the shortcuts to improve your speed/efficiency using the terminal

‍

### 3.3.1 2.3.1 Shell Cursor Manipulation (Also works in text editors)

The following shortcuts will improve moving the text cursor quickly in the shell and a text editor.

- `CTRL+A` - This will reset the text cursor to the beginning of the line.
- `CTRL+E` - This will set the text cursor to the end of the line.
- `CTRL+RIGHT_ARROW` Will move to the start of the next word
- `CTRL+LEFT_ARROW` - Will move to the start of the previous word

To get access to some previously executed commands, you can use `UP_ARROW`​ which will go to the previous command. While traversing this list, you can use `DOWN_ARROW` to go to the next command.

‍

### 3.3.2 2.3.2 Shell Editing Shortcuts (should also work in text editors)

When using the following shortcuts, you should be able to edit text quickly.

- `CTRL+W` - Deletes the previous word
- `ALT+D` - Deletes the next word
- `CTRL+U` - Deletes the line
- `CTRL+SHIFT+V` - Pastes what is currently in the copy-clipboard.
- `CTRL+K` - Cuts the text (to the clipboard) until the end of the start

‍

### 3.3.3 2.3.3 Process Control Shortcuts

You will likely encounter an infinite loop when programming or you want to stop inputting text into your process. These shortcuts provide a quick solution to some of these issues.

- `CTRL+C` - Interrupts the process.
- `CTRL+D`​ - When a process is executing, it may await for input from the user however by using this shortcut it will close `stdin`.
- `CTRL+L` - It will clear the screen.
- `CTRL+Z` - It will suspend the process and send it to the background.

‍

### 3.3.4 2.3.4 Shell History

You can access to the shell history by doing either.

- `history` command within the shell
- `CTRL+R` which will prompt a history search

```terminal
cyatto@MeoBook Projects % history
 1004  ls
 1005  rm hello.txt
 1006  ls
 1007  mkdir Projects
 1008  ls
 1009  touch hello.txt
 1010  mv hello.txt Projects/hello.txt
 1011  ls Projects
 1012  cd Projects
 1013  pwd
 1014  ls
 1015  echo "Woohoo, using the command line\n"
 1016  echo "Woohoo, using the command line\n\n~\n"
 1017  echo 'Woohoo, using the command line'
 1018  echo "Woohoo, using the command line" > hello.txt
 1019  cat hello.txt
```

‍

---

## 3.4 2.4 Dotnet

- `dotnet` is going to be a common build tool used throughout this book

  - serves the purpose of building, packaging and runnings our programs that are constructed
  - this command is responsible for creating projects from templates, compiling and running your programs
  - why aren't we using Visual Studio?

    - there is a lot of value in interacting with the command line for dotnet as you may be responsible for deploying and building programs that do not have access to a GUI
    - also supports automated builds for continuous integration and delivery

‍

### 3.4.1 Concepts

- `dotnet` command, compilation and source code
- What a *Virtual Machine* is.
- Compiled and Interpreted programs

‍

### 3.4.2 2.4.1 C# Programs and `DotNet`

- While the C# default CLI program will use **Top-Level Statements** by default, it is best to be upfront and expose what is being generated for you and decompose it.
- To create a project using `dotnet`, do the following:

```terminal
cyatto@MeoBook Projects % dotnet new console -n HelloWorld --use-program-main
The template "Console App" was created successfully.

Processing post-creation actions...
Restoring /Users/cyatto/Study/prog/csharp/notes/tafe_tthom_programming1cs_book/Projects/HelloWorld/HelloWorld.csproj:
Restore succeeded.


cyatto@MeoBook Projects % ls
HelloWorld
```

- the above snippet will create a console application in a folder called `HelloWorld`​ and ensure we have a `Main` method as its entrypoint.

```terminal
cyatto@MeoBook Projects % ls HelloWorld
HelloWorld.csproj	obj
Program.cs
```

- changing directories to the project folder `HelloWorld` will reveal the following files, which are responsible for the following:

  - `.csproj` is a project file which will be used to integrate third party libraries and references
  - `obj`​ folder is responsible for metadata and caches for your project; used alongside `.csproj` files and your program
  - `Program.cs`​ - this file is our **program**; contains code that will be compiled.

‍

Let's inspect `Program.cs`:

```cs
namespace HelloWorld;

class Program
{
    static void Main(string[] args)
    {
        Console.WriteLine("Hello, World!");
    }
}
```

- (ensuring we are not using top-level statements)
- when creating a project like this, we will always have a `Main` method which is where the program starts.

  - our program is doing a simple job already: emitting `Hello, World!`​ (the standard for most templates; the relation between the project name and what is being outputted is down to convention; everyone's first program is usually something akin to `Hello World`).

‍

#### 3.4.2.1 Building our program

Steps for **compiling** and **running** our program:

- use the `build`​ subcommand for `dotnet` to achieve this.

```terminal
cyatto@MeoBook Projects % cd HelloWorld
cyatto@MeoBook HelloWorld % dotnet build
Restore complete (0.2s)
  HelloWorld net10.0 succeeded (1.1s) → bin/Debug/net10.0/HelloWorld.dll

Build succeeded in 1.5s
```

- Run `dotnet build` inside the project folder

  - Output: if the `build succeeded`, we are then able to run the program
  - However, the build could fail for a number of reasons, notably if there is syntax error in the code.

‍

Steps for **running** the program:

- use the `run` subcommand

```terminal
cyatto@MeoBook HelloWorld % dotnet run
Hello, World!
```

- we can observe the `Console.WriteLine` part in the source code allows us to print to the screen.
- Note! You can call `dotnet run` and if the build process has not been triggered, it will also recompile your program again.

‍

#### 3.4.2.2 Compilation and Virtual Machines

- C# is *compiled* and *interpreted* --> need to make the process behind compilation and execution as clear as possible:
- What occurs when building a project:

  - 1/ When running `dotnet build` => analysis of the source code, performs the necessary checks to make it suitable for translation
  - 2/ When translating the source code, it will be translating into **machine code**. That machine node is not x86 or ARM (or the target platform); it is translating into CIL (Common Intermediate Langauge).
  - 3/ When running `dotnet run`​, the **CIL** code is then interpreted by the **CLR (Common Language Runtime)**  which is a virtual machine that is then translated into machine code.
- Importantly, the *interpreting* part is actually indicating that we have a machine inside our machine. This is called a *virtual machine*.
- Compilation: taking the source code and outputting machine code, which is normally something not particularly pleasant for humans to read or write by hand...

‍

---

# 4. 2.5 NuGet

NuGet is a package manager used alongside the dotnet build tool for packaging and distributing libraries; it serves a similar purpose as other software repository systems such as `npm`​, `cargo`​ and `gradle`.

‍

## 4.1 Using `nuget`

- For the most part, `nuget`​ is not something that needs to be used directly as usage typically will be through the `dotnet` tool.

- To add a package to a dotnet project, use `dotnet add package <name>`
- Within each `dotnet`​ package, the `.csproj`​ file maintains a list of dependencies, inside an `ItemGroup`​ as a `PackageReference` entry.

‍

Available commands:

- `dotnet list package` - list the packages associated with the project
- `dotnet add package <name>`​ - add a package to a project; optionally specify the version with `--version <version>` after the package name
- `dotnet remove package <package name>` - remove the package from the project.

‍

Afterwards, you are able to call `dotnet build`​ which will automatically call `nuget restore`​ or `dotnet restore` to retrieve the necessary packages and build them.

‍

---

# 5. 2.6 Version Control (Git)

‍

`git` - this command helps power a lot of software development and collaboration.

- **Repositories:**  locations where source code is stored/committed to. There are *local* or *remote* repos that can be **push**ed to
- **Commit**: a set of changes made to a repository, which can include any files that have been added, modified, or deleted.

  - `git log` - view commit history
- **Staging:**  a state **pre-commit**; when changes (added, modified, deleted files) are made to the repo, these changes can be staged, i.e. ready for commit.

  - `git status` - check the status of staged changes
- **Remote:**  a repository that is usually on another computer or server. Services like GitHub, Codeberg and GitLab allow devs to create a repo on their service and treat that repo as a remote one. Multiple remote repos can be set up.
- **Cloning:**  creating a duplicate of a repo (typically one that is online) for local purposes.

  - `git clone <url>`​ - clone the repo and use/modify it and commit back to it (assuming write permissions are granted), or have a **fork** of it (a derivative version)

‍

## 5.1 Closing a repository

Exercise:

- To get access to the set of exercises associated with this book, clone the repo to create a local copy on your device: `git clone https://github.com/TAFE-tthom/programming1cs-git`
- The above script will clone the repo and create a folder called `programming1-cs`
- To pull any updates that may be made to the repo, use `git pull`.

‍

## 5.2 Git steps

### 5.2.1 Initialising a repo

- `git init` - initialise a repo in the current folder you are in

‍

### 5.2.2 Adding to staging

- `git status` - show the current state of changes that have been staged, tracked or not.
- `git add <name of files>`​ or `git add .` (add all files in the current and nested folders)

‍

```terminal
cyatto@MeoBook HelloWorld % git init
Initialized empty Git repository in /Users/cyatto/Study/prog/csharp/notes/tafe_tthom_programming1cs_book/Projects/HelloWorld/.git/
cyatto@MeoBook HelloWorld % git status
On branch main

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
	HelloWorld.csproj
	Program.cs
	bin/
	obj/

nothing added to commit but untracked files present (use "git add" to track)
```

‍

### 5.2.3 Commit

- create a commit with `git commit -m <message>`​ - where `message` is a string in single of double quotes outlining what the commit is doing; a convention in software dev is to associate particular commits with a label like: Fix, Chore, Feat/Feature, Revert
- e.g. `git commit -m 'FIX: scaling bus associated with the grid'`

‍

### 5.2.4 Using a remote repo (setting up an account)

- register for a GitHub account (or otherwise: Codeberg, GitLab, BitBucket)
- you can even host it yourself if you have access to a VPS
- after registering, create a repo on the website (or via CLI commands)

‍

### 5.2.5 Before adding the remote to your local

- generate an **SSH key** using `ssh-keygen` which will generate a key-pair (public key and private key)
- use the default settings, ignore giving it a passphrase when prompted
- once generated, use the `<id>.pub`​ file in `~/.ssh`
- copy the contents of the `.pub`​ file, go to GitHub > Settings > SSH and GPG keys > New SSH key > specify the title `TAFEDemo`​ and paste the contents into the textbox with the `Key` label above it > Add SSH Key

‍

### 5.2.6 Using a remote (create a repo)

- Create a new remote repo on the GitHub website (or use CLI commands)
- `git remote add origin <url>`​ - `origin` is just a name (alias) for the remote while the url is the actual location.
- once added, `push` commits to the remote repo --> synchronise changes with the remote repo

  - first time: `git push -u origin main`
  - thereafter: `git push`

‍

## 5.3 Common mistakes/issues

- Initialisation: initialising a git repo in the home drive; each project should be considered a separate repo --> ensure that `git init` is run inside the intended folder

  - To undo this mistake: identify a repo using `git status`​ which will indicate if it is a repo or not; you can then find the `.git`​ folder and delete it (use `ls -la` to list hidden files).
- Staging: staging a file but then modifying it after the fact and not re-staging the modified file; the modified file must be re-added to save those changes to the commit
- Commit: may get prompted to set a username and email, relating to authoring of the commit

  - may need to configure these with: `git config --global user.email '<your email>'`​ and `git config --global user.name '<your name>'`
- SSH Key: do not copy the contents of the private key; ensure the public key contents are added to GitHub

‍

## 5.4 Git Checklist

Make sure you have satisified the following:

1. You have created a local repository
2. You have added a file to your local repository
3. You have made a commit to your local repository
4. You have created an account on github (or another service)
5. You have created an ssh-key and added the public key to your account
6. You have set a remote to your local repository
7. You have pushed the changes and can see them on the website.

‍

---

# 6. 2.7 Review

As we end the chapter on the command line, we have started launching ourselves into gaining more control over the computer and programming. To conclude, you posess knowledge of this topic and be able to apply it.

Review the following and ensure you can produce some evidence in the form of ​**notes**​, ​**annotations**​, ​**programs**​, **attempts** or some artifact that informs you if you meet its criteria.

- What you should know and understand

  - The shell on your device
  - Filesystem pathing, files and directories
  - Filesystem structure
  - Concepts of a program and process
  - What a command is and how it relates to a program
  - What command line arguments are
  - Understand the role `git` has in this document
- What you should be able to apply

  - Open a shell
  - Be able to run commands
  - Navigate the filesystem using some basic commands
  - Be able to compile and run a simple **C#**  program
  - Pass appropriate command line arguments to commands

While it is always difficult to know the concepts completely, the more one does, the more you will gain from it.

‍
