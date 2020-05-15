# Chapter 9 構造化言語とオープンシステム
# 9.2
## 9.2.4 システムプログラム ソケット通信の例
C言語では、TCP/IPを使用したソケット通信をサポートしている。
### ソケット方式について
  ```
  socket()->bind()->listen()->accept()------->read()--------->write()       サーバ側
				       |        |　　　　　　　	|
			       接続確立|	|データ要求	|データ応答
				       |	|		|
		    　　　socket()->connect()->write()-------->read()       クライアント側
  ```
  
ソケット通信では、クライアント/サーバ側の通信方式が代表的で、
1. クライアントはサーバに対して、任意のタイミングでリクエストを送り(クライアント側`write()`)
1. サーバではクライアントから任意のタイミングで送られてくるリクエストを待ち受ける

この通信方式はネットワークを使用するアプリケーションでの基本パターン

ソケットを使用した通信方式には、当初バークレイ系のUnixから実装された
- バークレイソケット：Unix <-こっちが主流
- TLI(Transport Layer interface)：SystemV系



## バークレイソケットを使用したプログラム例

```c:client.c
#include <sys/types.h>
#include <sys/socket.h>
#include <stdio.h>
#include <netinet/in.h>
#include <arpa/inet.h>
#include <unistd.h>
#include <stdlib.h>

int main(int argc, char **argv) {
  int sockfd, len, res;
  struct sockaddr_in addr;
  char *send = *(++argv); char recv[80];
  sockfd = socket(AF_INET, SOCK_STREAM, 0);//ソケットシステムコール
  addr.sin_family = AF_INET;
  addr.sin_addr.s_addr = inet_addr("127.0.0.1");
  addr.sin_port = 9374;
  len = sizeof(addr);
  res = connect(sockfd, (struct sockaddr *)&addr, len);
  if (res == -1) {
    printf("client error");
    exit(1);
  }
  write(sockfd, send, 80);
  read(sockfd, recv, 80);
  printf("サーバからの応答 = %s\n", recv);
  close(sockfd);
  exit(0);
  return 0;
}
```

### `socket()`システムコール
クライアントからサーバにアクセスする場合、最初にsocketシステムコールでソケットを生成する  

### `connect()`システムコール
`socket()`コールで通信方式を指定したあと、クライアントは`connect`コールでサーバへの接続確立のリクエストを送信する。

### `write()`システムコール
`connect`コールの戻り値からサーバとの接続が角煮されるとサーバへのデータ送信が可能になり、`write()`コールでサーバへデータを送信する

### `read()`システムコール
`write`コールはサーバへのリクエスト送信となり、サーバからのレスポンスを`read`コールで受信する

```c:server.c
#include <sys/types.h>
#include <sys/socket.h>
#include <stdio.h>
#include <netinet/in.h>
#include <arpa/inet.h>
#include <unistd.h>
#include <string.h>
int main(){
  int server_sockfd, client_sockfd;
  int server_len, client_len;
  struct sockaddr_in server_address;
  struct sockaddr_in client_address;
  server_sockfd = socket(AF_INET, SOCK_STREAM, 0);
  server_address.sin_family = AF_INET;
  server_address.sin_addr.s_addr = inet_addr("127.0.0.1");
  server_address.sin_port = 9374;
  server_len = sizeof(server_address);
  bind(server_sockfd, (struct sockaddr *) &server_address, server_len);
  listen(server_sockfd, 5);

  for(;;) {
    char msg[80]; printf("server waiting\n");
    client_sockfd = accept(server_sockfd, (struct sockaddr *)&client_address, &client_len);
    read(client_sockfd, msg, 80);
    strcat(msg, " Yes We can!\n");
    write(client_sockfd, msg, 80);
    close(client_sockfd);
  }
}
```
