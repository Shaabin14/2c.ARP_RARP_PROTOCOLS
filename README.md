# 2c.SIMULATING ARP /RARP PROTOCOLS
## AIM
To write a python program for simulating ARP protocols using TCP.
## ALGORITHM:
## Client:
1. Start the program
2. Using socket connection is established between client and server.
3. Get the IP address to be converted into MAC address.
4. Send this IP address to server.
5. Server returns the MAC address to client.
## Server:
1. Start the program
2. Accept the socket which is created by the client.
3. Server maintains the table in which IP and corresponding MAC addresses are
stored.
4. Read the IP address which is send by the client.
5. Map the IP address with its MAC address and return the MAC address to client.
P
## PROGRAM - ARP
server.py
```python
import socket
s = socket.socket()
s.bind(('localhost', 8000))
s.listen(5)
print("Server is listening...")

c, addr = s.accept()
print(f"Connection established with {addr}")

address = {
    "165.165.80.80": "6A:08:AA:C2",
    "165.165.79.1": "8A:BC:E3:FA"
}

while True:
    ip = c.recv(1024).decode()

    if not ip:  
        break

    print(f"Received IP: {ip}")

    mac = address.get(ip, "MAC address not found")
    c.send(mac.encode())

c.close()
```

client.py
```python
import socket
c = socket.socket()
c.connect(('localhost', 8000))

while True:
    ip = input("Enter IP address to find MAC (or type 'exit' to quit): ")

    if ip.lower() == "exit":  
        break

    c.send(ip.encode())
    mac = c.recv(1024).decode()
    print(f"MAC Address for {ip}: {mac}")
c.close()

```

## OUTPUT - ARP

<img width="1052" height="219" alt="Screenshot 2026-02-04 141956" src="https://github.com/user-attachments/assets/eecdf810-409f-4b83-be66-359928d4821d" />

<img width="1051" height="201" alt="Screenshot 2026-02-04 142020" src="https://github.com/user-attachments/assets/db742f1c-27b2-4c7c-a14b-2ae9a6c66298" />



## PROGRAM - RARP
server.py
```python
import socket

s = socket.socket()
s.bind(('localhost', 8000))
s.listen(5)
print("Server is listening for RARP requests...")

c, addr = s.accept()
print(f"Connection established with {addr}")

rarp_table = {
    "6A:08:AA:C2": "165.165.80.80",
    "8A:BC:E3:FA": "165.165.79.1"
}

while True:
    mac = c.recv(1024).decode()
    if not mac:
        break

    print(f"Received MAC: {mac}")
    ip = rarp_table.get(mac, "IP address not found")
    c.send(ip.encode())

c.close()
s.close()

```
client.py
```python
import socket
c = socket.socket()
c.connect(('localhost', 8000))

while True:
    mac = input("Enter MAC address to find IP (or type 'exit' to quit): ")
    if mac.lower() == "exit":  
        break
    c.send(mac.encode())
    ip = c.recv(1024).decode()
    print(f"IP Address for {mac}: {ip}")
c.close()
```
## OUPUT -RARP

<img width="1057" height="193" alt="Screenshot 2026-02-04 142324" src="https://github.com/user-attachments/assets/2b683499-7d3d-49e6-a10b-755a3a76683f" />

<img width="1058" height="213" alt="Screenshot 2026-02-04 142348" src="https://github.com/user-attachments/assets/b96602c0-24b8-4d3b-a81b-14b34d4a3850" />

## RESULT
Thus, the python program for simulating ARP protocols using TCP was successfully 
executed.
