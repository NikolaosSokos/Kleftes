# PHOTO WHEN CONNECTED INTO YOUR PC
Take a photo from your camera when someone gets into your PC
Code :
from email import encoders
from email.message import Message
from email.mime.audio import MIMEAudio
from email.mime.base import MIMEBase
from email.mime.image import MIMEImage
from email.mime.multipart import MIMEMultipart
from email.mime.text import MIMEText
import cv2
import datetime
now = datetime.datetime.now()
msg = '\n W ' + str(datetime.datetime.now())
camera_port = 0
ramp_frames = 30
camera = cv2.VideoCapture(camera_port)
def get_image():
 retval, im = camera.read()
 return im
for i in range(ramp_frames):
 temp = get_image()
print("Taking image...")
camera_capture = get_image()
file = "******"#Path where the photo taken will be saved
cv2.imwrite(file, camera_capture)
del(camera)

# Define these once; use them twice!
strFrom = '******@gmail.com'#Account That Sends Mail
strTo = '*****@gmail.com'#Receiver

# Create the root message and fill in the from, to, and subject headers
msgRoot = MIMEMultipart('related')
msgRoot['Subject'] = 'test message'
msgRoot['From'] = strFrom
msgRoot['To'] = strTo
msgRoot.preamble = 'This is a multi-part message in MIME format.'

# Encapsulate the plain and HTML versions of the message body in an
# 'alternative' part, so message agents can decide which they want to display.
msgAlternative = MIMEMultipart('alternative')
msgRoot.attach(msgAlternative)

msgText = MIMEText('This is the alternative plain text message.')
msgAlternative.attach(msgText)

# We reference the image in the IMG SRC attribute by the ID we give it below
msgText = MIMEText('Kleftis' + msg)
msgAlternative.attach(msgText)

# This example assumes the image is in the current directory
fp = open('path', 'rb')#path again 
msgImage = MIMEImage(fp.read())
fp.close()

# Define the image's ID as referenced above
msgImage.add_header('Content-ID', '<image1>')
msgRoot.attach(msgImage)

# Send the email (this example assumes SMTP authentication is required)
import smtplib
username = '*******@gmail.com'#Your Gmail Here
password = '*****'#Your Mail Password Here
smtp = smtplib.SMTP()
server = smtplib.SMTP('smtp.gmail.com:587')
server.starttls()
server.login(username,password)
server.sendmail(strFrom, strTo, msgRoot.as_string())
server.quit()

