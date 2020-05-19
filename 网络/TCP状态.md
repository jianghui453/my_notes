# tcp status

- CLOSED: There is no connection.
- LISTEN: The local end-point is waiting for a connection request from a remote end-point i.e. a passive open was performed.
- ESTABLISHED: The third step of the three-way connection handshake was performed. The connection is open.
- FIN-WAIT-1: The first step of an active close (four-way handshake) was performed. The local end-point has sent a connection termination request to the remote end-point.
- CLOSE-WAIT: The local end-point has received a connection termination request and acknowledged it e.g. a passive close has been performed and the local end-point needs to perform an active close to leave this state.
- FIN-WAIT-2: The remote end-point has sent an acknowledgement for the previously sent connection termination request. The local end-point waits for an active connection termination request from the remote end-point.
- LAST-ACK: The local end-point has performed a passive close and has initiated an active close by sending a connection termination request to the remote end-point.
- CLOSING: The local end-point is waiting for an acknowledgement for a connection termination request before going to the TIME-WAIT state.
- TIME-WAIT: The local end-point waits for twice the maximum segment lifetime (MSL) to pass before going to CLOSED to be sure that the remote end-point received the acknowledgement.

## 引用

1. [TCP states](https://community.apigee.com/articles/7970/tcp-states-explained.html)