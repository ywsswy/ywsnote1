#include <iostream>
#include <WinSock2.h>
using namespace std;
#pragma comment(lib,"ws2_32.lib")
void msg_display(char *buf)
{
    char msg[100];
    strcpy(msg,buf);
    cout<<"**************** Received : ****************\n";
    cout<<msg<<endl ;
}
 
int main()
{
    int sock, msgsock, length, receive_len ;
    sockaddr_in sock_server,sock_client ;
    char buf[0x600];
 
    WSADATA wsa ;
    WSAStartup(WINSOCK_VERSION, &wsa) ;
    sock = socket(AF_INET,SOCK_STREAM,0); 
    sock_server.sin_family=AF_INET;
    sock_server.sin_port=htons(1325);
    sock_server.sin_addr.S_un.S_addr=INADDR_ANY;
    if (bind(sock,(struct sockaddr *)&sock_server,sizeof(sock_server)))
    {
        cout<<"Binding stream socket error!\n" ;
    }
	
	cout<<"exploit target server 1.0\n" ;
    listen(sock,4);
    length=sizeof(struct sockaddr) ;
    do
    {
        msgsock=accept(sock,(struct sockaddr *)&sock_client,(int *)&length);
        if (msgsock == -1)
        {
            cout<<"accept error!\n" ;
            break;
        }
        else
            do
            {
                memset(buf,0,sizeof(buf));
                if ((receive_len = recv(msgsock,buf,sizeof(buf),0))<0)
                {
                    cout<<"reading stream message error!\n" ;
                    receive_len = 0;
                }
				else
					msg_display(buf);
            } while (receive_len);
            closesocket(msgsock) ;
    } while (1);
    WSACleanup() ;
	return 0;
}
