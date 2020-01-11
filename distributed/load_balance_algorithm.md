# load balance algorithm

- Round robin – A batch of servers are programmed to handle load in a rotating sequential manner. The algorithm assumes that each device is able to process the same number of requests and isn’t able to account for active connections.
- Weighted round robin – Servers are rated based on the relative amount of requests each is able to process. Those having higher capacities are sent more requests.
- Least connections – Requests are sent to the server having the fewest number of active connections, assuming all connections generate an equal amount of server load.
- Weighted least connections – Servers are rated based on their processing capabilities. Load is distributed according to both the relative capacity of the servers and the number of active connections on each one.
- Source IP hash – Combines the source and destination IP address in a request to generate a hash key, which is then designated to a specific server. This lets a dropped connection be returned to the same server originally handling it.
- consitent_hasing

## references

1. [Load Balancing Algorithms](https://www.imperva.com/learn/availability/load-balancing-algorithms/)