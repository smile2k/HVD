
LINK
https://yinyangit.wordpress.com/2011/06/22/socket-communication-with-tcp-client-server/

https://learn.microsoft.com/en-us/dotnet/fundamentals/networking/sockets/tcp-classes
---

The [Socket](https://learn.microsoft.com/en-us/dotnet/api/system.net.sockets.socket) class is highly recommended for advanced users, instead of `TcpClient` and `TcpListener`.

To work with Transmission Control Protocol (TCP), you have two options: either use [Socket](https://learn.microsoft.com/en-us/dotnet/api/system.net.sockets.socket) for maximum control and performance, or use the [TcpClient](https://learn.microsoft.com/en-us/dotnet/api/system.net.sockets.tcpclient) and [TcpListener](https://learn.microsoft.com/en-us/dotnet/api/system.net.sockets.tcplistener) helper classes. [TcpClient](https://learn.microsoft.com/en-us/dotnet/api/system.net.sockets.tcpclient) and [TcpListener](https://learn.microsoft.com/en-us/dotnet/api/system.net.sockets.tcplistener) are built on top of the [System.Net.Sockets.Socket](https://learn.microsoft.com/en-us/dotnet/api/system.net.sockets.socket) class and take care of the details of transferring data for ease of use.


## Use `TcpClient` and `TcpListener`

## Create an IP endpoint
The `IPEndPoint` is constructed with an [IPAddress](https://learn.microsoft.com/en-us/dotnet/api/system.net.ipaddress) and its corresponding port number.

## Create a `TcpClient`
When sending and receiving messages, the [Encoding](https://learn.microsoft.com/en-us/dotnet/api/system.text.encoding) should be known ahead of time to both server and client. For example, if the server communicates using [ASCIIEncoding](https://learn.microsoft.com/en-us/dotnet/api/system.text.asciiencoding) but the client attempts to use [UTF8Encoding](https://learn.microsoft.com/en-us/dotnet/api/system.text.utf8encoding), the messages will be malformed.

### Client Code
```CSharp
using System;
using System.Net;
using System.Net.Sockets;
using System.Text;
using System.Threading;

class Client

{
    static TcpClient client;
    static NetworkStream stream;

    static void Main()
    {
        // Connect to server
        client = new TcpClient("127.0.0.1", 12345); // Wait until the TCP connection is established or if an error occurs.
        Console.WriteLine("Connected to server.");

        // Get client stream
        stream = client.GetStream(); // Uses a NetworkStream to read data from the remote host.

        // Start separate thread to handle incoming messages from server
        Thread receiveThread = new Thread(ReceiveMessage);
        receiveThread.Start();

        // Main loop to send messages to server
        while (true)
        {
            Console.Write("Client: ");
            string messageToSend = Console.ReadLine();
            SendMessage(messageToSend);
        }
    }

    static void ReceiveMessage()
    {
        // Receive messages from server
        while (true)
        {
            byte[] receiveData = new byte[1024];
            int bytesReceived = stream.Read(receiveData, 0, receiveData.Length);
            string receivedMessage = Encoding.ASCII.GetString(receiveData, 0, bytesReceived);
            Console.WriteLine("\n" + "Server: " + receivedMessage);

            // Print message to terminal right after receiving
            Console.Write("Client: ");
        }
    }

    static void SendMessage(string message)
    {
        // Send message to server
        byte[] sendData = Encoding.ASCII.GetBytes(message);
        stream.Write(sendData, 0, sendData.Length);
    }
}
```



## Create a `TcpListener`

### Server code
```CSharp
using System;
using System.Net;
using System.Net.Sockets;
using System.Text;
using System.Threading.Tasks;

class TCPServer
{
    static async Task Main()
    {
        // Define the port number
        int port = 8080;

        // Create a TCP listener and start listening on any available IP address
        TcpListener listener = new TcpListener(IPAddress.Any, port);
        listener.Start();
        Console.WriteLine("Server is listening on port " + port);

        try
        {
            // Accept incoming client connections
            TcpClient client = await listener.AcceptTcpClientAsync();
            Console.WriteLine("Client connected.");

            // Get the network stream for reading and writing
            NetworkStream stream = client.GetStream();

            // Start receiving messages from the client asynchronously
            _ = ReceiveMessagesAsync(stream);

            // Start sending messages to the client
            while (true)
            {
                Console.Write("Enter message to send to client: ");
                string messageToSend = Console.ReadLine();

                byte[] messageBytes = Encoding.ASCII.GetBytes(messageToSend);
                await stream.WriteAsync(messageBytes, 0, messageBytes.Length);
            }
        }
        finally
        {
            // Stop listening and close the listener
            listener.Stop();
        }
    }

    static async Task ReceiveMessagesAsync(NetworkStream stream)
    {
        byte[] buffer = new byte[1024];
        while (true)
        {
            int bytesRead = await stream.ReadAsync(buffer, 0, buffer.Length);
            string receivedData = Encoding.ASCII.GetString(buffer, 0, bytesRead);
            Console.WriteLine("\n" + "Received from client: " + receivedData);
        }
    }
}

```

