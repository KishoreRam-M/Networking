**Active FTP** vs **Passive FTP**:

### Active FTP:
1. **Control Connection**: The client connects to the server (this is like making a phone call to the server).
2. **Data Connection**: When the client wants to transfer a file, it tells the server which port it is listening to on its side (using the **PORT command**).
   - The server then **connects back** to the client on that port to start transferring the file.
   
   **Problem**: If the client is behind a firewall or a NAT (Network Address Translation) device, it may block incoming connections. This means the server might not be able to connect back to the client, causing issues.

### Passive FTP:
1. **Control Connection**: The client connects to the server (just like Active FTP).
2. **Data Connection**: Instead of the server connecting back to the client, the client tells the server to open a **data port** (using the **PASV command**).
   - The server then gives the client an IP address and port number to connect to.
   - The client **then** connects to this port on the server to transfer the file.
   
   **Benefit**: This is more **firewall-friendly** because the client is the one opening both connections. It doesn't require the server to connect back to the client, reducing issues with firewalls blocking incoming connections.

### Key Difference:
- **Active FTP**: The server connects to the client for file transfers (but can be blocked by firewalls).
- **Passive FTP**: The client connects to the server for file transfers (firewall-friendly).

In short:
- **Active FTP** has the **server** initiate the connection.
- **Passive FTP** has the **client** initiate both connections.
