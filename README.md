Client.py
# Import socket module
import socket
import RPi.GPIO as GPIO
from time import sleep
GPIO.setwarnings(False)
GPIO.setmode(GPIO.BOARD)
GPIO.setup(8,GPIO.OUT)
GPIO.output(8,GPIO.LOW)
# Create a socket object
s = socket.socket()
# Define the port on which you want to connect
port = 8787
# connect to the server on local computer
s.connect(('192.168.21.232', port))
# receive data from the server and decoding to get the string.
print (s.recv(1024).decode())
while True:
if s.recv(1024).decode()=='1':
print('intruder detected')
74
GPIO.output(8,GPIO.HIGH)
sleep(10)
else:
GPIO.output(8,GPIO.LOW)
print('intruder not detected')
# close the connection
s.close()
Server.py
import socket
import RPi.GPIO as GPIO
GPIO.setwarnings(False)
GPIO.setmode(GPIO.BOARD)
GPIO.setup(8,GPIO.IN)
# next create a socket object
s = socket.socket()
print ("Socket successfully created")
# reserve a port on your computer in our
# case it is 12345 but it can be anything
port = 8787
# Next bind to the port
# we have not typed any ip in the ip field
# instead we have inputted an empty string
# this makes the server listen to requests
# coming from other computers on the network
s.bind(('', port))
print ("socket binded to %s" %(port))
# put the socket into listening mode
s.listen(5)
print ("socket is listening")
# a forever loop until we interrupt it or
# an error occurs
75
while True:
# Establish connection with client.
c, addr = s.accept()
print ('Got connection from', addr )
# send a thank you message to the client. encoding to send byte type.
c.send('Thank you for connecting'.encode())
while True:
if GPIO.input(7):
c.send("1".encode())
print("INtruder DEtected")
else:
c.send("0".encode())
print("No INtruder")
