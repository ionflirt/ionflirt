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

# separate into chunks of 50 emails in order to avoid sending limits
for b in range(20):
    
    # open the email server connection
    server = smtp.SMTP(host=smtp_host, port=smtp_port)
    server.starttls()
    server.login(user=user, password=password)

    # create and send the message
    for i in range(50):
        # check to see if this is the final message, which has a slightly different range
        if msg_count == 1000:
            start_pos = (len(word_list)-final_msg_size)
            msg_text = ' '.join(word_list[start_pos:])
        else:
            start_pos = msg_count * msg_size
            msg_text = ' '.join(word_list[start_pos:start_pos + msg_size])

        # create the email message headers and set the payload
        msg = EmailMessage()
        msg['From'] = fr_address
        msg['To'] = to_address
        msg['Subject'] = subject + str(msg_count+1)
        msg.set_payload(msg_text)

        msg_count += 1
        
        # open the email server and send the message
        server.send_message(msg)

        ''' delay each email by 1/2 second to space out the distribution
            this 1/2 second delay may cause the emails to be delivered out-of-order
            when some are slightly larger than others.
        '''
        time.sleep(0.5)
      
    # delay each batch by 60 seconds to avoid sending limits
    time.sleep(60)
    
