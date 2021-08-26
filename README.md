# Online-Code-Judge-with-FTP
### Some resources to get started with socket prgramming
* Socket Programming in C/C++ : https://www.geeksforgeeks.org/socket-programming-cc/
* Tutorial : https://www.youtube.com/watch?v=LtXEMwSG5-8
* Handling multiple clients : https://www.geeksforgeeks.org/socket-programming-in-cc-handling-multiple-clients-on-server-without-multi-threading/
* What is FTP ?  : https://www.geeksforgeeks.org/file-transfer-protocol-ftp-in-application-layer/

### Description

Designed a **OJ**( Online judger) that takes codes , compile it, check for compilation errors, execute your code by providing some secret inputs and collect your code's output, check for  errors , and finally match your output with its hidden test outputs ans displays the result.

This repo contanis implementation of Console based Online Judge in which there will be a server and multiple clients communicating with the server.Each client can send a c/c++ file to the server through a command (CODEJUD file.cpp) and the server will reply to the client whether the given code is successful or giving error(like compilation,runtime,TLE etc) at any point of time during execution of the code. If execution is successful then the server will also check and reply to the client about the acceptance of the c/c++ file.

The above functions on file transfer protocol(FTP) on top of the TCP sockets.There will be a server and multiple clients communicating with the server. Each client process will open a new connection with the server.Code uses "select" system call to handle multiple client requests.

The code also supports the follwing commands of FTP:

* RETR : This command causes the remote host to initiate a data connection and to send the requested file over the  data connection.
* STOR : This command causes to store a file into the current directory of the remote host.
* LIST : Sends a request to display the list of all files present in the directory.
* QUIT : This command terminates a USER and if file transfer is not in progress , the server closes the control connection.
* DELE : This command deletes a file in the current directory of server.
* CODEJUD : This command will take a c/c++ file from client and server will compile , execute and match the output with given test cases and will notify back to client about any error or correctness of c/c++ file.

### Possible error case considered :

#### CODEJUD - 

1. Overwritten already present code (c or c++) file because it could hinder with input and testcase format files.
2. If some error in storing the file then loop breaks.
3. Object file names as filename_clientNo so no client would overwite others object file.
4. If error in compilation then loop breaks , deleting code( c or c++) file.
5. Created temporary file for storing each testcase input line and passing for input redirection.                
6. Passed timeout value for running to avoid falling in infinite loops.
7. Kept flag for timeout/runtime error .
8. Output redirection done in append mode.
9. Output filename named as output_filename_CNo.txt to avois hinderence with other client output file for same code filename.
10. Handled case for code requiring input file and code not requiring it.
11. If either timeout or runtime error ocours then loop breaks , deleting the code file, object file.
12. Due to formatting issue of \r\n and for simplicity ,Matched output and testcase file using linux command "awk" and rediredted output to an temporary file in which every line contains boolean value for matched(1) and not matched(0) testcase.
13. Checked for any zero and if there exists then all testcases not passed .
14. Removed code file, object file and all temporary files so that it did'nt hinder with other client's files.

#### RETR-  

1. When already same named file exists then saved by appending one("1") at the end much like the file downloading program does. 
2. Error when a file does'nt exist .(prompt to enter new command)
3. Error while transfering file.        (sendfile return -1)
4. Error while saving in client side. (write returns -1)
5. Open file in O_RDONLY mode at server side
6. Open with O_CREAT | O_EXCL | O_WRONLY mode at client side to create a file.

#### STOR-       

1. When already same named file exists then saved by appending one("1") at the end much like the file         downloading program does. 
2. Error when a file does'nt exist .(prompt to enter new command)
3. Error while transfering file.        (sendfile return -1)
4. Error while saving in server side. (write returns -1)
5. Open file in O_RDONLY mode at client side
6. Open with O_CREAT | O_EXCL | O_WRONLY mode at server side to create a file

#### LIST-
1. In case of 'ls' command, it would create a temporary file and then transfers it to client . So used readdir from <dirent.h> to get all files names using infinite while loop and append in string.

#### QUIT- 
1. Simply checked for any file transfer and close the connection with the client.

#### DELE- 
1. Open file in O_RDONLY to check does file exists.
2. Used the system call to remove the desired file
3. Prompt at client side to confirm deletion .
_____________________________________________________________________

### Instructions to run the code
To judge any file using "CODEJUD" command there must be these files at server side(the current directory where code is run) to match output for that program :
1. input_testcaseno.txt (from where the server takes data for code).
2. testcae_testcaseno.txt ( to match the output file with this).

At client side there must be a files(saved) , named as testcaseno.<c/cpp> . 
Sample commands :
RETR <filename>
STOR <filename>
LIST
QUIT
DELE <filename>
CODEJUD <filename>.<c/c++>

#### Command to run the code (on linux kernal):
Note: Open atleast two terminal window with current directory as server and one as client. There must only one server running on a PORT but there can be multiple clients requesting a server.

* Compilation command for server :  gcc server.c -o server -lm
<br />Running command for server :   ./server PORT  ( ex ./server 1001)

* Compilation command for client :  gcc client.c -o client 
<br />Running command for client :   ./client 127.0.0.1 PORT
