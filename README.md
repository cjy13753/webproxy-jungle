# webserver-lab 정리
## Web server가 뭐야?

## static content와 dynamic content는 어떻게 다른거지?
serving static content: url에 위치해 있는 disk file을 통쨰로 클라이언트에 제공
serving dynamic content: url에 위치한 executable file을 실행하고 output을 클라이언트에 제공

## Dynamic content를 serve할 때, client와 weber server process와 child process가 어떻게 program arguments와 output 등을 서로에게 전달하는가?
client -> web server: url
web server process -> child process: envrionment variable
child process -> client -> standard output

## CGI program이 뭐야?

## URL과 web content 사이의 관계에 대해서 설명할 줄 알기

## HTTP Request의 request headers 중에서 Host header가 중요한 이유를 proxy chain 연결 지어서 설명하기

## Web server를 구현하는 데 있어 Socket interface를 이해하는 게 중요한 이유
client가 server에게 request를 날리고, server가 client에게 response를 반환하는 작업을 하려면 client와 server가 네트워크 상에서 connect된 상태여야 한다. client와 server 사이의 connection을 만들기 위해 굉장히 복잡한 기술들이 필요한데, 이걸 쉽게 하기 위해 우리가 가져다 쓰는 게 socket interface이다. socket interface에 대한 implementation은 대부분의 operating system에 존재한다.

## Socket에 어떤 정보가 담기지? 어떻게 socket을 통해서 client와 server가 정보를 주고 받지? 질    문이 잘못된건가?

## IP protocol에 version이 있다는 말이 뭘 의미하는 거지? 교과서 섹션 11.4.7을 보면 getaddrinfo와 getnameinfo 함수들이 sockets interface와 함께 쓰일 때 `they allow us to write network programs that are independent of any particular version of the IP protocol`이라고 함.


## getaddrinfo 함수는 possible socket addresses를 어디서 가져오는건데? 그리고 왜 여러 개를 주렁주렁 가져와야 하는 구조인거지?
getaddrinfo의 중요한 기능 중 하나는 hostname을 던져주면, domain name system을 이용하여, hostname에 매핑이 되는 ip address(들)을 받고, 이 정보와 함께 socket type, socket domain, socket protocol 정보들까지 한 번에 제공한다. 이 정보들을 가지고 socket 함수를 호출하면, hardcoding 없이 protocol independent하게 네트워크 프로그램을 작성할 수 있다. `they allow us to write network programs that are independent of any particular version of the IP protocol(CSAPP3rd 11.4.7)`

## linked list에 double pointer를 쓰면 어떻게 작동을 한다는 건가?(getaddrinfo 함수의 인자값에 struct addrinfo **result 들어가는데 궁금해서)
getaddrinfo 함수에서 왜 struct addrinfo **result를 인자로 받는 거지? double pointer말고 그냥 pointer면 안 되나?
링크드리스트 관련 operation 시 double pointer를 쓰면 수월해지는 게 많다.
장점: head를 별도로 반환 안 해줘도 함수 안에서 head가 가리키는 node를 바꿔줄 수 있고, 추가로 boundary NULL check이 훨씬 편해진다.
[내 유튜브 설명](https://www.youtube.com/watch?v=8May6WNIZk0)
``` c
LLNode *insert_with_singlepointer(LLNode *head, LLNode *node) {
    if (head == NULL) {
        head = node;
        return head;
    }
    LLNode *prev = NULL;
    LLNode *curr = head;
    while (curr && curr->data < node -> data) {
        prev = curr;
        curr = curr->next;
    }
    if (prev == NULL) {
        node->next = head;
        head = node;
    } else {
        prev->next = node;
        node->next = curr;
    }
    return head;
}

void insert_with_doublepointer(LLNode **head, LLNode *node) {
    LLNode **curr = head;
    while (*curr && (*curr)->data < node->data)
    {
        curr = &((*curr)->next);
    }
    node->next = *curr;
    *curr = node;
}
```

## server-side에서 connfd와 listenfd를 별도로 두는 이유는?

## clientfd와 connfd를 rio와 associate 해서 쓰는 이유는?

## tiny clienterror 함수에서 \r\n을 매번 붙여주는 이유는?
HTTP standard에서는 라인 끝나는 걸 표시하는 게 리눅스처럼 line feed(\n)만 붙이는 게 아니라, carriage return과 line feed를 함께(\r\n) 붙인다고 함

## tiny clienterror 함수에서 reponse header의 경우 왜 한 줄마다 rio_writen을 할까?

## tiny serve_static에서 Mmap을 쓰는 이유는?