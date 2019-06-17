## Description
This is a basic UNIX shell implementation that is running on a Linux sub-system in Windows, written in C++. Here is how it works...

### Enter the Shell

![shell-1](./images/shell-1.PNG)

I can run ls and view the contents of this folder. 

![shell-1](./images/shell-2.PNG)

### Make a directory

![shell-1](./images/shell-3.PNG)

### Change Directory

![shell-1](./images/shell-4.PNG)

### Make a new file

![shell-1](./images/shell-5.PNG)

### Input Redirection

![shell-1](./images/shell-6.PNG)

### Piping
I made a small text file called "animals.txt" that I will use to demonstrate piping. 

![shell-1](./images/shell-7.PNG)

### Single Pipe

![shell-1](./images/shell-8.PNG)

### Double Pipe

![shell-1](./images/shell-9.PNG)

### Triple Pipe

![shell-1](./images/shell-10.PNG)

## Implemention 

### Display Directory and Process Input

The program loops indefinitely, reads in user input, and processes that input. The following code demonstrates this process in the main function. The current working directory is obtained using the getwd() function, and displayed to the user. The program then waits for the user to input a command and then processes it in the processCommand() function. 

```
while(1) {
  cout << username;
  char buffer[PATH_MAX];
  vector<char> current_path;
  
  getwd(buffer);
  
  int i = 0;
  while(buffer[i] != 0) {
    current_path.push_back(buffer[i]);
    i++
  }
  
  cout << current_path << "$ ";
  
  getline(cin, command);
  processCommand(command);
}
```
### Process Input

In the Process Command function, I declare a stringstream and read the user input delimiting at every pipe operator ('|'). Each command is then placed in a vector. More than one command in the vector is an indicator that the user is piping.  

```
stringstream command_string(cmd);
string temp_arg;
vector <string> command_vec;

while(getline(command_string, temp_arg, '|')) {
  command_vec.push_back(temp_arg);
  command_string.ignore(1); 
 }
 
 executeCommands(command_vec);

```

### Execute Commands

I support background process calling in my implementation so I need to check if any background processes are complete so that I can acknowledge them, effectively killing their zombie status. 

```
pid_t pidd = 0;
pid_t result = waitpid(pidd, NULL, WNOHANG);
```

The next lines check if the program needs to pipe (I explain how pipeCommands() works further down the page).

```
if (command_list.size() > 1){
        pipeCommands(command_list);
        return true;
}
```
The program then checks for a series of commands, cd (change directory), &(background process), and </> (input/output redirection). Each of these scenarios are handled differently and each go to their own functions (which I will not describe here). 

### Fork and Execute

Here is how the most basic commands are executed. The program forks and the child process executes the command while the parent waits. Since the parent is waiting for the child, the user will not be able to perform any commands until the process has executed (not the case with background process execution). Also, the reason we don't execute the command in the parent is because we would lose the program since execvp() terminates the program on completion.

```
int pidd = fork();
if (pidd == 0) {
  execvp(arglist[0], arglist);
  cout << "error: no command '" << arglist[0] << "' found" << endl;
  exit(0);
} else {
  wait(&pidd);
}
```

### Piping 

Here is my solution for any number of pipes. 

