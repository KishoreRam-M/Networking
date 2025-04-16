### 1. **FTP Client**
   - **Role**: The FTP client is the software that the user interacts with to connect to the FTP server. It can be a command-line client, a graphical user interface (GUI) client, or a web-based FTP client.
   - **Responsibilities**:
     - Initiates connections to the server (on port 21 for control connection).
     - Sends FTP commands (e.g., USER, PASS, RETR, STOR) to the server to perform actions like logging in, retrieving files, or uploading files.
     - Receives and processes responses from the server.
     - Manages local file operations, such as saving files after downloading or organizing files before uploading.

### 2. **FTP Server**
   - **Role**: The FTP server is the software running on the remote machine where files are stored and managed. It listens for incoming client connections and responds to FTP commands.
   - **Responsibilities**:
     - Listens on port 21 (for control connection) and processes client requests.
     - Responds to commands from the client (e.g., authentication, file retrieval, file upload).
     - Sends back appropriate responses to each client command (such as success or error codes).
     - Handles file transfers over the data connection (opened dynamically for each file transfer).

### 3. **Control Connection**
   - **Role**: This is the primary connection between the FTP client and server, used for sending commands and receiving responses.
   - **Responsibilities**:
     - Established on port 21 (by default) between the client and server.
     - Keeps the connection open throughout the FTP session.
     - Used to send commands like **USER**, **PASS**, **LIST**, **RETR**, **STOR**, and **QUIT**.
     - Sends responses to the client about the status of each command.

### 4. **Data Connection**
   - **Role**: This is a secondary connection used to transfer actual data (files or directory listings) between the FTP client and server.
   - **Responsibilities**:
     - This connection is established after certain FTP commands are issued, such as when transferring a file (RETR to download, STOR to upload) or listing directories (LIST command).
     - It can be established either by the server (in Active FTP) or by the client (in Passive FTP).
     - The data connection is temporary, created only for the duration of the file transfer or directory listing and is closed once the transfer is complete.

### 5. **FTP Commands**
   - **Role**: FTP commands are sent from the client to the server to perform various file-related operations.
   - **Responsibilities**:
     - **USER** and **PASS**: Used to authenticate and log in the client.
     - **LIST**: Request a list of files and directories from the server.
     - **RETR**: Retrieve (download) a file from the server.
     - **STOR**: Store (upload) a file to the server.
     - **CWD**: Change the working directory on the server.
     - **QUIT**: Close the session.

### 6. **FTP Responses**
   - **Role**: These are the responses sent by the FTP server to the client to indicate the outcome of each command.
   - **Responsibilities**:
     - Response codes are three-digit numbers (e.g., 220 for a successful welcome message, 230 for a successful login).
     - The responses help the client understand if a command was successful, if there was an error, or if further action is required (e.g., provide a password after a successful username).
     - Common response codes:
       - **220**: Service ready (welcome message).
       - **331**: Username okay, need password.
       - **230**: User logged in.
       - **150**: File status okay (ready to transfer).
       - **226**: File transfer complete.
       - **425**: Can't open data connection.
       - **530**: Not logged in (authentication failure).

### 7. **Authentication (USER & PASS)**
   - **Role**: These components are responsible for authenticating the client with the server before any file operations can take place.
   - **Responsibilities**:
     - **USER**: The client sends the **USER** command with the username.
     - **PASS**: The server requests the **PASS** command to supply the password.
     - After successful authentication, the server responds with a confirmation, and the client can proceed with file operations.

### 8. **FTP Modes: Active vs Passive**
   - **Role**: Defines how the data connection is established between the client and server.
   
   - **Active Mode (PORT)**:
     - The client sends a PORT command to tell the server the port it is listening to for the data connection.
     - The server then initiates the data connection from its default data port (port 20) back to the client's port.

   - **Passive Mode (PASV)**:
     - The client sends a PASV command to the server.
     - The server then provides the client with an IP address and port to connect to, and the client establishes the data connection.

   - **Key Difference**: In Active FTP, the server connects back to the client, while in Passive FTP, the client makes both connections (control and data), which is often more firewall-friendly.

### Summary:
- The **FTP client** is the user interface to interact with files.
- The **FTP server** hosts and manages the files.
- The **control connection** handles commands and responses.
- The **data connection** transfers actual file data.
- **FTP commands** manage login, file retrieval, uploads, etc.
- **Authentication** ensures secure client login before operations.
- **Active and Passive FTP modes** handle how the data connection is established.

