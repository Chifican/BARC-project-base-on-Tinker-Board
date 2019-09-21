import cv2
import numpy as np
import time
import RPi.GPIO as GPIO
from matplotlib import pyplot as plt
global p
pwm = 15

GPIO.setmode(GPIO.BOARD)
GPIO.setup(12, GPIO.OUT)
p = GPIO.PWM(12, 100)  		# channel=12 frequency=100Hz
p.start(15)


def servo(key):			#use pwm output to control the servo
    global pwm

    while key == 'S':		#go straight
        if pwm != 15:
            while (pwm != 15):
                if (pwm > 15):
                    pwm = pwm - 0.5
                    p.ChangeDutyCycle(pwm)
                    print(pwm)
                else:
                    pwm = pwm + 0.5
                    p.ChangeDutyCycle(pwm)
                    print(pwm)
            break
            
        else :
            p.ChangeDutyCycle(15)
            print(pwm)
        break
    
    while key == 'L' :		#go left
        if pwm >= 15 :
            while (pwm != 14.5):
                pwm = pwm - 0.5
                p.ChangeDutyCycle(pwm)
                print (pwm)
                break
            break
        elif(pwm > 10):
            pwm = pwm - 0.5
            p.ChangeDutyCycle(pwm)
            print (pwm)
        else:
            p.ChangeDutyCycle(10)
            print (pwm)
        break
    
    while key == 'R' :		#go right
        if pwm <= 15 :
            while (pwm != 15.5):
                pwm = pwm + 0.5
                p.ChangeDutyCycle(pwm)
                print (pwm)
                break
            break
        elif (pwm < 20):
            pwm = pwm + 0.5
            p.ChangeDutyCycle(pwm)
            print (pwm)
        else:
            p.ChangeDutyCycle(20)
            print (pwm)
        break
              
    while key == 'F' :		#default
        p.ChangeDutyCycle(15)
        print (pwm)
        break
    
    


def drct(x1,x2,y1,y2):		#determine the direction of servo by detecting image
    #x_lower = 70
    #x_upper = 90
    x = 80
    y = x2 - x1
    z = x1 - x2
    global key
    if  (x1 >= 70 and x1 <= 90) and (x2 >= 70 and x2 <= 90) :
      key = 'S'
    else:
      if (x2 >= 0 and x2 <= 80) and y1 ==0 and y2 == 119: 
        if (abs (z) <= 10) :  
          key = 'L'
        elif (z > 10) : 
          key = 'R'
        else : 
          key = 'L'
      elif (x2 > 80  and x2 < 160) and y1 ==0 and y2 == 119:
        if(abs (z) <= 10) :
          key = 'R' 
        elif(z > 10) :
          key = 'R'
        else : 
          key = 'L'
      elif (y2 != 119): 
        if(x1 >= 0 and x1 <= 80) :
          key = 'L'
        elif (x1 > 80 and x1 < 160 ) :
          key = 'R'
        else :
          key = 'F'
      elif (y1 != 0): 
        if(x2 >= 0 and x2 <= 80 ) :
          key = 'R'
        elif (x2 > 80 and x2 < 160 ) :
          key = 'L'
        else:
          key = 'F'
      else:
        key = 'F'
    return key


    
cap = cv2.VideoCapture(4)

while(cap.isOpened()):
    ret, frame = cap.read()		 # Capture frame-by-frame	
    frame= cv2.resize(frame,(160,120))		#resize the image for faster processing speed
    hsv = cv2.cvtColor(frame, cv2.COLOR_BGR2HSV)	#transform RGB to HSV 
    lower_blue=np.array([100,43,46])		#the threshold of blue
    upper_blue=np.array([124,255,255])
    mask=cv2.inRange(hsv,lower_blue,upper_blue)		#extract blue pixels	
    #median = cv2.medianBlur(mask,5)		#medium filtering
    kernel = np.ones((5,5),np.uint8)		#erode
    mask= cv2.erode(mask,kernel,iterations = 1)	#erode
    n=0
    i=0
    for b in range(119):			#detect the points
        for a in range(159):
            if mask[b,a]!=0:
                i=i+a
                n=n+1
        if i!=0:
            x1=i/n
            y1=b
            break
    n=0
    i=0
    for b in range(119,1,-1):
        for a in range(159):
            if mask[b,a]!=0:
                i=i+a
                n=n+1
        if i!=0:
            x2=i/n
            y2=b
            break
    
    try:
        x1
    except NameError:				#default
        x1_exists = False
    else:
        x1_exists = True


    if x1_exists == True:

        #cv2.line(mask,(int(x1),int(y1)),(int(x2),int(y2)),(255,0,0),3)
        print(x1,x2,y1,y2)
        key = drct(x1,x2,y1,y2)
        print(key)
        servo(key)
