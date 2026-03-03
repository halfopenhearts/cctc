NETCAT Relay Demos Listener - Listener

On Blue_Host-1 Relay: $ mknod mypipe p $ nc -lvp 1111 < mypipe | nc -lvp 3333 > mypipe

On Internet_Host (send): $ nc 172.16.82.106 1111 < secret.txt On Blue_Priv_Host-1 (receive):

$ nc 192.168.1.1 3333 > newsecret.txt

NETCAT Relay Demos Client - Client

On Internet_Host (send):

$ nc -lvp 1111 < secret.txt On Blue_Priv_Host-1 (receive):

$ nc -lvp 3333 > newsecret.txt On Blue_Host-1 Relay:

$ mknod mypipe p $ nc 10.10.0.40 1111 < mypipe | nc 192.168.1.10 3333 > mypipe

NETCAT Relay Demos Client - Listener

On Internet_Host (send):

$ nc -lvp 1111 < secret.txt On Blue_Priv_Host-1 (receive):

$ nc 192.168.1.1 3333 > newsecret.txt On Blue_Host-1 Relay:

$ mknod mypipe p $ nc 10.10.0.40 1111 < mypipe | nc -lvp 3333 > mypipe

NETCAT Relay Demos Listener - Client

On Internet_Host (send):

$ nc 172.16.82.106 1111 < secret.txt On Blue_Priv_Host-1 (receive):

$ nc -lvp 3333 > newsecret.txt On Blue_Host-1 Relay:

$ mknod mypipe p $ nc -lvp 1111 < mypipe | nc 192.168.1.10 3333 > mypipe
