CC = gcc
CFLAGS = -g -O2 -Wall -I .

# This flag includes the Pthreads library on a Linux box.
# Others systems will probably require something different.
LIB = -lpthread

all: hostinfo echoclient echoserver

echoclient: echoclient.c csapp.o
	$(CC) $(CFLAGS) -o echoclient echoclient.c csapp.o $(LIB)

echoserver: echoserver.c csapp.o echo.o
	$(CC) $(CFLAGS) -o echoserver echoserver.c csapp.o echo.o $(LIB)

echo.o: echo.c
	$(CC) $(CFLAGS) -c echo.c

hostinfo: hostinfo.c csapp.o
	$(CC) $(CFLAGS) -o hostinfo hostinfo.c csapp.o $(LIB)

csapp.o: csapp.c
	$(CC) $(CFLAGS) -c csapp.c

clean:
	rm -f *.o hostinfo echoclient echoserver *~