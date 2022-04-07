# used to get book data from website
import requests 
# used to calculate words per message
from math import floor 
# used to create delay in loop
import time 
# used for sending the email
import smtplib  as smtp 
# used to build the email
from email.message import EmailMessage 

book_url = 'https://www.gutenberg.org/files/2600/2600-0.txt'
r = requests.get(book_url)

book_data = r.text.encode('ascii', 'ignore').decode('ascii')

word_list = book_data.split(" ")

msg_size = floor(len(word_list) / 1000)
final_msg_size = len(word_list) - (msg_size * 999)

print(f"Words per message: {msg_size}\nFinal message size: {final_msg_size}")

user = 'kaleman682@gmail.com'
password = 'Dude2004'
fr_address = 'kaleman682@gmail.com'
to_address = 'jso476333@gmail.com'
smtp_host = 'jso476333.gmail.com' 
smtp_port = 587

subject = 'War & Peace - Part '
msg_text = ''
start_pos = 0
msg_count = 0

