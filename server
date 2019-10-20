//TCP server
//winsock libraries
#include <WinSock2.h>
#include <string>
#include <iostream>

std::string getErrorCode(int err)
{
    switch (err)
    {
    case WSANOTINITIALISED:
        return "WinSock was not properly initialized.";
    case WSAENETDOWN:
        return "Winsock Subsystem has failed.";
    case WSAEFAULT:
        return "Buffers provided are either to small or not in user address space.";
    case WSAEINTR:
        return "Blocking call was canceled."; //needs to be caught and avoided from error list
    case WSAEINPROGRESS:
        return "Call is still in Progress.";
    case WSAEINVAL:
        return "Socket not bound or invalid flag provided or length was negative.";
    case WSAEISCONN:
        return "Operation not possible with a connected socket.";
    case WSAENETRESET:
        return "Datagram socket TTL expired.";
    case WSAENOTSOCK:
        return "Handle is not a socket.";
    case WSAEOPNOTSUPP:
        return "Either unidirectional communication is not supported or out-of-band communication not possible.";
    case WSAESHUTDOWN:
        return "Socket already terminated.";
    case WSAEWOULDBLOCK:
        return "Receiving would block the socket which is illegal for a nonblocking socket.";
    case WSAEMSGSIZE:
        return "The buffer was too small to accomodate the message.";
    case WSAETIMEDOUT:
        return "Connection timed out.";
    case WSAECONNRESET:
        return "Connection reset by remote.";
    default:
        return "Error could not be identified.";
    }
}

int main()
{
    WSAData winSockData;

    //Start Winsock API
    if(WSAStartup(MAKEWORD(2, 1), &winSockData)!=0)
        std::cout<<getErrorCode(GetLastError())<<std::endl;

    //Adress will hold the adress info. Data type: SOCKADDR_IN structure.
    SOCKADDR_IN adress;
    int adressSize = sizeof(adress);

    //Set the IP, port and version of the adress (family).
    adress.sin_addr.s_addr = inet_addr("127.0.0.1");
    adress.sin_family = AF_INET;
    adress.sin_port = htons(8080);

    //Declaring 2 needed sockets.
    SOCKET sock_Listen, sock_Connect;

    //Setting the sock_Listen socket to an IPV4 type socket.
    sock_Listen = socket(AF_INET, SOCK_STREAM, 0);
    if(sock_Listen == 0)
        std::cout<<getErrorCode(GetLastError())<<std::endl;

    //Binding the adress to sock_Listen.
    if(::bind(sock_Listen, (SOCKADDR*)&adress, sizeof(adress))==-1)
        std::cout<<getErrorCode(GetLastError())<<std::endl;

    //Setting the sock_Listen socket to listen for a connetion for the maximum number of tries (SOMAXCONN)
    if(::listen(sock_Listen, SOMAXCONN)==-1)
        std::cout<<getErrorCode(GetLastError())<<std::endl;

    //Setting the sock_Connect socket to an IPV4 type socket.
    sock_Connect = socket(AF_INET, SOCK_STREAM, 0);
    if(sock_Connect==0)
        std::cout<<getErrorCode(GetLastError())<<std::endl;

    while(true)
    {
        std::cout<<"\n\tServer is waiting for a connection...";
        //The connection socket accepts a message from the client.
        sock_Connect = ::accept(sock_Listen, (SOCKADDR*)&adress, &adressSize);
        if(sock_Connect==0)
            std::cout<<getErrorCode(GetLastError())<<std::endl;
        if(sock_Connect)
        {
            //If a connection is established, print it to the server and send a message to the client.
            std::cout<<"Established connection with client. \n";
            if(::send(sock_Connect, "Successfully connected to server!", 46, 0)==-1)
                std::cout<<getErrorCode(GetLastError())<<std::endl;
            //Close open sockets and WinSockAPi DLL
            closesocket(sock_Connect);
            closesocket(sock_Listen);
            WSACleanup();
            break;
        }
    }
    return 0;
}
