# Online-Code-Judge-with-FTP
### Some resources to get started with socket prgramming
* Socket Programming in C/C++ : https://www.geeksforgeeks.org/socket-programming-cc/
* Tutorial : https://www.youtube.com/watch?v=LtXEMwSG5-8
* Handling multiple clients : https://www.geeksforgeeks.org/socket-programming-in-cc-handling-multiple-clients-on-server-without-multi-threading/

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
