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
