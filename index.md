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
