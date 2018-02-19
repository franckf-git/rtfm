## Index

[TOC]

- [pinout](#pinout)
- [bibliotheque](#bibliotheque)
- [mode](#mode)
- [configuration des entrées-sorties](#configuration-des-entrées-sorties)
- [lire une entrée numérique](#lire-une-entrée-numérique)
- [Changer l état d une sortie numérique](#changer-l-état-d-une-sortie-numérique)
- [Connaître la configuration d une entrée-sortie numérique](#connaître-la-configuration-d-une-entrée-sortie-numérique)
- [PWM](#pwm)
- [Remettre à zéro](#remettre-à-zéro)
- [modele python](#modele-python)
- [SPI](#spi)
- [I2C](#i2c)
- [LED](#led)
- [PWM servomoteur](#pwm-servomoteur)
- [Servo](#servo)
- [Bouton](#bouton)
- [gpiozero](#gpiozero)
- [gpiozero LED](#gpiozero-led)
- [gpiozero PWMLED](#gpiozero-pwmled)
- [gpiozero RGBLED](#gpiozero-rgbled)
- [gpiozero Buzzer](#gpiozero-buzzer)
- [gpiozero Motor](#gpiozero-motor)
- [gpiozero Button](#gpiozero-button)
- [gpiozero Line Sensor](#gpiozero-line-sensor)
- [gpiozero Motion Sensor](#gpiozero-motion-sensor)
- [gpiozero Light Sensor](#gpiozero-light-sensor)
- [gpiozero Distance Sensor](#gpiozero-distance-sensor)
- [gpiozero LED bouton](#gpiozero-led-bouton)
- [7 segment display](#7-segment-display)
- [Tkinter](#tkinter)
- [Sensehat Simulator](#sensehat-simulator)
- [Sensehat text scroll](#sensehat-text-scroll)
- [Sensehat image import jpg et png 8x8](#sensehat-image-import-jpg-et-png-8x8)
- [Sensehat shake](#sensehat-shake)
- [Sensehat rotation](#sensehat-rotation)
- [Sensehat orientation](#sensehat-orientation)
- [Sensehat lettre](#sensehat-lettre)
- [Sensehat image](#sensehat-image)
- [Sensehat environnement](#sensehat-environnement)
- [Sensehat joystick simple](#sensehat-joystick-simple)
- [Sensehat joystick](#sensehat-joystick)
- [Sensehat compass](#sensehat-compass)
- [SenseHat 3d](#sensehat-3d)

_____________________________________________________________________________________
pinout
-------------------------------------------------------------------------------------
[GPIO pinout](http://fr.pinout.xyz/)
```
1 3.3v              2 5v
3 BCM 2 (SDA)       4 5v
5 BCM 3 (SCL)       6 Masse
7 BCM 4 (GPCLK0)    8 BCM 14 (TXD)
9 Masse             10 BCM 15 (RXD)
11 BCM 17           12 BCM 18 (PWM0)
13 BCM 27           14 Masse
15 BCM 22           16 BCM 23
17 3.3v             18 BCM 24
19 BCM 10 (MOSI)    20 Masse
21 BCM 9 (MISO)     22 BCM 25
23 BCM 11 (SCLK)    24 BCM 8 (CE0)
25 Masse            26 BCM 7 (CE1)
27 BCM 0 (ID_SD)    28 BCM 1 (ID_SC)
29 BCM 5            30 Masse
31 BCM 6            32 BCM 12 (PWM0)
33 BCM 13 (PWM1)    34 Masse
35 BCM 19 (MISO)    36 BCM 16
37 BCM 26           38 BCM 20 (MOSI)
39 Masse            40 BCM 21 (SCLK)

Ethernet USB

sudo apt install python3-gpiozero
pinout
,--------------------------------.
| oooooooooooooooooooo J8     +====
| 1ooooooooooooooooooo        | USB
|                             +====
|      Pi Model 3B V1.2          |
|      +----+                 +====
| |D|  |SoC |                 | USB
| |S|  |    |                 +====
| |I|  +----+                    |
|                   |C|     +======
|                   |S|     |   Net
| pwr        |HDMI| |I||A|  +======
`-| |--------|    |----|V|-------`

Revision           : a02082
SoC                : BCM2837
RAM                : 1024Mb
Storage            : MicroSD
USB ports          : 4 (excluding power)
Ethernet ports     : 1
Wi-fi              : True
Bluetooth          : True
Camera ports (CSI) : 1
Display ports (DSI): 1

   3V3  (1) (2)  5V    
 GPIO2  (3) (4)  5V    
 GPIO3  (5) (6)  GND   
 GPIO4  (7) (8)  GPIO14
   GND  (9) (10) GPIO15
GPIO17 (11) (12) GPIO18
GPIO27 (13) (14) GND   
GPIO22 (15) (16) GPIO23
   3V3 (17) (18) GPIO24
GPIO10 (19) (20) GND   
 GPIO9 (21) (22) GPIO25
GPIO11 (23) (24) GPIO8 
   GND (25) (26) GPIO7 
 GPIO0 (27) (28) GPIO1 
 GPIO5 (29) (30) GND   
 GPIO6 (31) (32) GPIO12
GPIO13 (33) (34) GND   
GPIO19 (35) (36) GPIO16
GPIO26 (37) (38) GPIO20
   GND (39) (J8) GPIO21
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
bibliotheque
-------------------------------------------------------------------------------------
```python
import RPi.GPIO as GPIO
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
mode
-------------------------------------------------------------------------------------
```python
GPIO.setmode(GPIO.BOARD) #pin
###>>>ou
GPIO.setmode(GPIO.BCM)   #broche gpio
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
configuration des entrées-sorties
-------------------------------------------------------------------------------------
```python
GPIO.setup(12, GPIO.IN)                        # broche 12 est une entree numerique
GPIO.setup(12, GPIO.OUT)                       # broche 12 est une sortie numerique
GPIO.setup(12, GPIO.OUT, initial=GPIO.HIGH)    # broche 12 est une sortie initialement a l etat haut
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
lire une entrée numérique
-------------------------------------------------------------------------------------
```python
GPIO.input(12)
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
Changer l état d une sortie numérique
-------------------------------------------------------------------------------------
```python
###>>>indiquer le numéro de la sortie, ainsi que l état désiré.
GPIO.output(12, GPIO.LOW)
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
Connaître la configuration d une entrée-sortie numérique
-------------------------------------------------------------------------------------
```python
###>>>GPIO.INPUT, GPIO.OUTPUT, GPIO.SPI, GPIO.I2C, GPIO.HARD_PWM, GPIO.SERIAL ou GPIO.UNKNOWN
state = GPIO.gpio_function(12)
print(state)
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
PWM
-------------------------------------------------------------------------------------
```python
###>>>La PWM pour Pulse Width Modulation consiste en un signal carré dont on fait varier le rapport cyclique, en d autres termes, on fait varier la durée de l état haut, par rapport à l état bas.
#                  50%    ___            ___      100% ________________________
#                        /   \          /   \         /
#                       /     \        /     \       /
#__0%__________________/       \______/       \_____/

p = GPIO.PWM(channel, frequence)
p.start(rapport_cyclique) #ici, rapport_cyclique vaut entre 0.0 et 100.0 %
p.ChangeFrequency(nouvelle_frequence)
p.ChangeDutyCycle(nouveau_rapport_cyclique)
p.stop()
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
Remettre à zéro
-------------------------------------------------------------------------------------
```python
GPIO.cleanup()
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
modele python
-------------------------------------------------------------------------------------
```python
from tkinter import *
import RPi.GPIO as GPIO
import time

GPIO.setwarnings(False)   
GPIO.setmode(GPIO.BCM)
GPIO.setup(?, GPIO.OUT)

#GPIO.cleanup()
#root.mainloop()
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
SPI
-------------------------------------------------------------------------------------
```python
###>>>Une liaison SPI (pour Serial Peripheral Interface) est un bus de données série synchrone baptisé ainsi par Motorola, qui opère en mode full-duplex. Les circuits communiquent selon un schéma maître-esclaves, où le maître contrôle la communication. Plusieurs esclaves peuvent coexister sur un même bus, dans ce cas, la sélection du destinataire se fait par une ligne dédiée entre le maître et l esclave appelée Slave Select (SS).

from __future__ import division 
import spidev 
 
def lire_analog(puce_spi = 0, entree_analog = 1): 
    liaison = spidev.SpiDev(0, puce_spi) 
    liaison.max_speed_hz = 300000 # en Hertz 

    # Initialisation des parametres de lecture
    # (cf datasheet pour les curieux) 
    if entree_analog == 0: 
        value = 128 
    else: 
        value = 160 
    to_send = [value, 0] 

    # Lecture 
    rd_octets = liaison.xfer2(to_send) 

    # La reponse arrive sur deux octets 
    msb = rd_octets[0] 
    lsb = rd_octets[1] 
    value = (msb << 8) + lsb 
    calcul = 2 * (value * 3.3) / 4096.0 
    return calcul 

if __name__ == '__main__': 
    print(lire_analog())
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
I2C
-------------------------------------------------------------------------------------
```python
###>>>I2C (signifie : Inter-Integrated Circuit, en anglais) est un bus informatique qui a émergé de la guerre des standards lancée par les acteurs du monde électronique. Conçu par Philips pour les applications de domotique et d électronique domestique, il permet de relier facilement un microprocesseur et différents circuits.

import time
from smbus import SMBus
bus= SMBus(1)   # 1 indique qu il faut utiliser le port /dev/i2c-1

while True:
    # Le composant porte l'adresse 0x48 (A0 et A1 relies à GND)
    # On va lire plusieurs octets a partir du registre 0
    data = bus.read_i2c_block_data(0x48, 0) 
    
    tempMSB = data[0]
    tempLSB = data[1]
    
    temperature=(((tempMSB << 8) + tempLSB) >>7) * 0.5
    if temperature > 128 :  # test si la temperature est negative
        # complement a 1 de la temperature
        temperature = (((((tempMSB << 8) + tempLSB) >>7 )* 0.5) -256)
            
    print(temperature)
    fichier = open ('fichier_anne_marie','a')
    fichier.write (str(temperature))
    fichier.write (',  ')
    fichier.close () 
    
    time.sleep(2)
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
LED
-------------------------------------------------------------------------------------
```python
# Import des modules
import RPi.GPIO as GPIO
import time

# Initialisation de la numerotation et des E/S
GPIO.setmode(GPIO.BOARD)
GPIO.setup(3, GPIO.OUT, initial = GPIO.HIGH)

# On fait clignoter la LED
while True:
    GPIO.output(3, not GPIO.input(3))
    time.sleep(0.5)
    GPIO.output(18, True)
    time.sleep(0.1)
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
PWM servomoteur
-------------------------------------------------------------------------------------
```python
# Import des modules
import RPi.GPIO as GPIO
import time

# Initialisation de la numerotation et des E/S
GPIO.setmode(GPIO.BOARD)
GPIO.setup(12, GPIO.OUT, initial = GPIO.LOW)

Rapport = 10.0
sens = True

p = GPIO.PWM(12, 200)
p.start(rapport) #ici, rapport_cyclique vaut entre 0.0 et 100.0

# On fait varier l intensite de la LED
while True:
    if sens and rapport < 100.0:
        rapport += 10.0
    elif sens and rapport >= 100.0:
        sens = False
    elif not sens and rapport > 10.0:
        rapport -= 10.0
    elif rapport == 10.0:
        sens = True
    p.ChangeDutyCycle(rapport)
    time.sleep(0.25)
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
Servo
-------------------------------------------------------------------------------------
```python
import RPi.GPIO as GPIO
import time
GPIO.setmode(GPIO.BOARD)
servopin=11
GPIO.setup(servopin,GPIO.OUT)
pwm=GPIO.PWM(servopin,50)
pwm.start(7)
while(1):
        for i in range(0,180):
            DC=1./18.*(i)+2
            pwm.ChangeDutyCycle(DC)
            time.sleep(.02)
        for i in range(180,0,-1):
            DC=1/18.*i+2
            pwm.ChangeDutyCycle(DC)
            time.sleep(.02)
pwm.stop
GPIO.cleanup
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
Bouton
-------------------------------------------------------------------------------------
```python
# Import des modules
import RPi.GPIO as GPIO
import time

# Initialisation de la numerotation et des E/S
GPIO.setmode(GPIO.BOARD)
GPIO.setup(12, GPIO.OUT, initial=GPIO.LOW)
GPIO.setup(19, GPIO.IN)

# Si on detecte un appui sur le bouton, on allume la LED 
# et on attend que le bouton soit relache
while True:
    state = GPIO.input(19)
    if not state:
        # on a appuye sur le bouton connecte sur la broche 19
        GPIO.output(12, GPIO.HIGH)
        while not state:
            state = GPIO.input(19)
            time.sleep(0.02)  # Pause pour ne pas saturer le processeur
        GPIO.output(12, GPIO.LOW)
    time.sleep(0.02)  # Pause pour ne pas saturer le processeur
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
gpiozero
-------------------------------------------------------------------------------------
```python
###>>>librairie simplifié BCM
###>>>https://gpiozero.readthedocs.io/en/stable/index.html
from gpiozero import *
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
gpiozero LED
-------------------------------------------------------------------------------------
```
gpiozero.LED(pin, active_high=True, initial_value=False)

Use this class to turn an LED on and off. The LED should have its longer leg (anode) connected to a GPIO pin, and the other leg connected via a limiting resistor to GND. The following example will light an LED connected to GPIO 17:

from gpiozero import LED
led = LED(17)
led.on()

#Methods:
on() Turn the device on.
off() Turn the device off.
blink(on_time=1, off_time=1, n=None, background=True) Make the device turn on and off repeatedly.
toggle() Reverse the state of the device; if on, it ll turn off , and vice versa.
is_lit Returns True if the device is currently active, and False otherwise.
pin The GPIO pin that the device is connected to.
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
gpiozero PWMLED
-------------------------------------------------------------------------------------
```
gpiozero.PWMLED(pin, active_high=True, initial_value=0, frequency=100)

This class is used to light an LED with variable brightness. As before, a resistor should be used to limit the current in the circuit. The following example will light an LED connected to pin 17 at half brightness:

from gpiozero import PWMLED
led = PWMLED(17)
led.value = 0.5

#Methods:
on() Turn the device on.
off() Turn the device off .
blink(on_time=1, off_time=1, fade_in_time=0, fade_out_time=0, n=None, background=True) Make the device turn on and off repeatedly.
pulse(fade_in_time=1, fade_out_time=1, n=None, background=True) Make the device fade in and out repeatedly.
toggle() Toggle the state of the device. If it s currently off (value is 0.0), this changes it to  fully  on (value is 1.0). If the device has a duty cycle
(value) of 0.1, this will toggle it to 0.9, and so on.
is_lit Returns True if the device is currently active, and False otherwise.
pin The GPIO pin that the device is connected to.
value The duty cycle of the PWM device, from 0.0 (off ) to 1.0 (fully on).
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
gpiozero RGBLED
-------------------------------------------------------------------------------------
```
gpiozero.RGBLED(red, green, blue, active_high=True, initial_value=(0, 0, 0), pwm=True)

This class is used to light a full-colour LED (composed of red, green, and blue LEDs). Connect its longest leg (cathode) to GND, and the other to GPIO pins via resistors (or use one on the cathode). The following code will make the LED purple:

from gpiozero import RGBLED
led = RGBLED(2, 3, 4)
led.color = (1, 0, 1)

#Methods:
on() Turn the device on: equivalent to setting the LED colour to white (1, 1, 1).
off() Turn the device off : equivalent to setting the LED colour to black (0, 0, 0).
blink(on_time=1, off_time=1, fade_in_time=0, fade_out_time=0, on_color=(1, 1, 1), off_color=(0, 0, 0), n=None, background=True) Make the device turn on and off repeatedly.
pulse(fade_in_time=1, fade_out_time=1, on_color=(1, 1, 1), off_color=(0, 0, 0), n=None, background=True) Make the device fade in and out repeatedly.
toggle() Toggle the state of the device. If it s currently off (value is (0, 0, 0)), this changes it to  fully  on (value is (1, 1, 1)). If the device has a specific colour, this method inverts it.
color Represents the color of the LED as an RGB 3-tuple of (red, green, blue), where each value is between 0 and 1 if pwm=True, and only 0 or 1 if not.For example, purple is (1, 0, 1), yellow is (1, 1, 0), and orange is (1, 0.5, 0).
is_lit Returns True if the LED is currently active (not black) and False otherwise.
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
gpiozero Buzzer
-------------------------------------------------------------------------------------
```
gpiozero.Buzzer(pin, active_high=True, initial_value=False)

This class is used to control a piezo buzzer. This example will sound a buzzer connected to GPIO pin 3:

from gpiozero import Buzzer
bz = Buzzer(3)
bz.on()

#Methods:
on() Turn the device on.
off() Turn the device off.
beep(on_time=1, off_time=1, n=None, background=True) Make the device turn on and off repeatedly.
toggle() Reverse the state of the device; if on, it ll turn off , and vice versa.
is_active Returns True if the device is currently active, and False otherwise.
pin The GPIO pin that the device is connected to.
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
gpiozero Motor
-------------------------------------------------------------------------------------
```
gpiozero.Motor(forward, backward, pwm=True)

This class will drive a generic motor connected via an H-bridge motor controller. The following example will make a motor connected to GPIO pins 17 and 18 turn  forwards :

from gpiozero import Motor
motor = Motor(17, 18)
motor.forward()

#Methods:
backward(speed=1) Drive the motor backwards. Speed can be any value between 0 and 1 (if pwm=True).
forward(speed=1) Drive the motor forwards. Speed can be any value between 0 and 1 (if pwm=True).
stop() Stop the motor.
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
gpiozero Button
-------------------------------------------------------------------------------------
```
gpiozero.Button(pin, pull_up=True, bounce_time=None)

Use this class with a simple push button or switch. The following example will print a line of text when a button connected to GPIO pin 4 is pressed:

from gpiozero import Button
button = Button(4)
button.wait_for_press()
print("The button was pressed!")

#Methods:
wait_for_press(timeout=None) Pause the script until the device is activated, or the timeout (in seconds) is reached.
wait_for_release(timeout=None) Pause the script until the device is deactivated, or the timeout (in seconds) is reached.
when_pressed The function to run when the device changes state from inactive to active.
when_released The function to run when the device changes state from active to inactive.
when_held The function to run when the device has remained active for hold_time seconds.
hold_time The length of time (in seconds) to wait after the device is activated, until executing the when_held handler. If hold_repeat is True, this is also the length of time between calls to when_held .
hold_repeat If True, when_held will be executed repeatedly with hold_time seconds between each call.
held_time The length of time (in seconds) that the device has been held for. 
is_held When True, the device has been active for at least hold_time seconds.
is_pressed Returns True if the device is currently active, and False otherwise.
pin The GPIO pin that the device is connected to.
pull_up If True, the device uses a pull up resistor to set the GPIO pin  high  by default.
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
gpiozero Line Sensor
-------------------------------------------------------------------------------------
```
gpiozero.LineSensor(pin)

This class is used to read a single pin line sensor, like the TCRT5000. The following example will print a line of text indicating when the sensor (with its output connected to GPIO pin 4) detects a line, or stops detecting one:

from gpiozero import LineSensor
from signal import pause
sensor = LineSensor(4)
sensor.when_line = lambda: print('Line detected')
sensor.when_no_line = lambda: print('No line detected')
pause()

#Methods:
wait_for_line(timeout=None) Pause the script until the device is deactivated, or the timeout (in seconds) is reached.
wait_for_no_line(timeout=None) Pause the script until the device is activated, or the timeout (in seconds) is reached.
when_line The function to run when the device changes state from active to inactive.
when_no_line The function to run when the device changes state from inactive to active.
pin The GPIO pin that the device s output is connected to.
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
gpiozero Motion Sensor
-------------------------------------------------------------------------------------
```
gpiozero.MotionSensor(pin, queue_len=1, sample_rate=10, threshold=0.5, partial=False)

As shown in chapter 6, this class is used with a passive infrared (PIR) sensor. The following example will print a line of text when motion is detected by the sensor (with its middle output leg connected to GPIO pin 4):

from gpiozero import MotionSensor
pir = MotionSensor(4)
pir.wait_for_motion()
print("Motion detected!")

#Methods:
wait_for_motion(timeout=None) Pause the script until the device is activated, or the timeout (in seconds) is reached.
wait_for_no_motion(timeout=None) Pause the script until the device is deactivated, or the timeout (in seconds) is reached.
motion_detected Returns True if the device is currently active, and False otherwise.
when_motion The function to run when the device changes state from inactive to active.
when_no_motion The function to run when the device changes state from active to inactive.
pin The GPIO pin that the device s output is connected to.
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
gpiozero Light Sensor
-------------------------------------------------------------------------------------
```
gpiozero.LightSensor(pin, queue_len=5, charge_time_limit=0.01, threshold=0.1, partial=False)

Connect one leg of the LDR to the 3V3 pin; connect one leg of a 1μF capacitor to a ground pin; connect the other leg of the LDR and the other leg of the capacitor to the same GPIO pin. This class repeatedly discharges the capacitor, then times the duration it takes to charge, which will vary according to the light falling on the LDR. The following code will print a line of text when light is detected:

from gpiozero import LightSensor
ldr = LightSensor(18)
ldr.wait_for_light()
print("Light detected!")
ou
while True:
      print(ldr.value)

#Methods:
wait_for_dark(timeout=None) Pause the script until the device is deactivated, or the timeout (in seconds) is reached.
wait_for_light(timeout=None) Pause the script until the device is activated, or the timeout (in seconds) is reached.
light_detected Returns True if the device is currently active, and False otherwise.
when_dark The function to run when the device changes state from active to inactive.
when_light The function to run when the device changes state from inactive to active.
pin The GPIO pin that the device is connected to.
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
gpiozero Distance Sensor
-------------------------------------------------------------------------------------
```
gpiozero.DistanceSensor(echo, trigger, queue_len=30, max_distance=1, threshold_distance=0.3, partial=False)

This class is used with a standard HC-SR04 ultrasonic distance sensor. Note: to avoid damaging the Pi, you ll need to use a voltage divider on the breadboard to reduce the sensor s output (ECHO pin) from 5V to 3.3V. The following example will periodically report the distance measured by the sensor in cm (with the TRIG pin connected to GPIO17, and ECHO pin to GPIO18):

from gpiozero import DistanceSensor
from time import sleep
sensor = DistanceSensor(echo=18, trigger=17)
while True:
print('Distance: ', sensor.distance * 100)
sleep(1)

#Methods:
distance Returns the current distance measured by the sensor in metres.
Note that this property will have a value between 0 and max_distance .
max_distance As specifi ed in the class constructor, the maximum distance that the sensor will measure in metres.
threshold_distance As specifi ed in the class constructor, the distance (measured in metres) that will trigger the when_in_range and when_out_of_range events when crossed.
when_in_range The function to run when the device changes state from active to inactive.
when_out_of_range The function to run when the device changes state from inactive to active.
wait_for_in_range(timeout=None) Pause the script until the device is deactivated, or the timeout is reached.
wait_for_out_of_range(timeout=None) Pause the script until the device is activated, or the timeout is reached.
echo Returns the GPIO pin that the sensor s ECHO pin is connected to.
trigger Returns the GPIO pin that the sensor s TRIG pin is connected to.
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
gpiozero LED bouton
-------------------------------------------------------------------------------------
```python
from gpiozero import LED, Button
from time import sleep
led = LED(17)
button = Button(2)
button.wait_for_press()
led.on()
sleep(3)
led.off()

from gpiozero import LED, Button
from signal import pause
led = LED(17)
button = Button(2)
button.when_pressed = led.on
button.when_released = led.off
pause()

from gpiozero import LED, Button
from time import sleep
led = LED(17)
button = Button(2)
while True:
    button.wait_for_press()
    led.toggle()
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
7 segment display
-------------------------------------------------------------------------------------
```python
import RPi.GPIO as IO            # calling for header file which helps us use GPIO s of PI
import time                      # calling for time to provide delays in program

DISPLAY = [0x3F,0x06,0x5B,0x4F,0x66,0x6D,0x7D,0x07,0x7F,0x67]            # string of characters storing PORT values for each digit.

IO.setwarnings(False)            # do not show any warnings
IO.setmode (IO.BCM)              # programming the GPIO by BCM pin numbers. (like PIN29 as GPIO5 )

IO.setup(13,IO.OUT)              # initialize GPIO Pins as outputs
IO.setup(6,IO.OUT)
IO.setup(16,IO.OUT)
IO.setup(20,IO.OUT)
IO.setup(21,IO.OUT)
IO.setup(19,IO.OUT)
IO.setup(26,IO.OUT)
IO.setup(12,IO.OUT)

def PORT(pin):                   # assigning GPIO logic by taking  pin  value
    if(pin&0x01 == 0x01):
        IO.output(13,1)          # if  bit0 of 8bit  pin  is true, pull PIN13 high
    else:
        IO.output(13,0)          # if  bit0 of 8bit  pin  is false, pull PIN13 low
    if(pin&0x02 == 0x02):
        IO.output(6,1)           # if  bit1 of 8bit  pin  is true, pull PIN6 high
    else:
        IO.output(6,0)           # if  bit1 of 8bit  pin  is false, pull PIN6 low
    if(pin&0x04 == 0x04):
        IO.output(16,1)
    else:
        IO.output(16,0)
    if(pin&0x08 == 0x08):
        IO.output(20,1)
    else:
        IO.output(20,0)   
    if(pin&0x10 == 0x10):
        IO.output(21,1)
    else:
        IO.output(21,0)
    if(pin&0x20 == 0x20):
        IO.output(19,1)
    else:
        IO.output(19,0)
    if(pin&0x40 == 0x40):
        IO.output(26,1)
    else:
        IO.output(26,0)
    if(pin&0x80 == 0x80):
        IO.output(12,1)          # if  bit7 of 8bit  pin  is true, pull PIN12 high
    else:
        IO.output(12,0)          # if  bit7 of 8bit  pin  is false, pull PIN12 low
        
while 1:
    for x in range(10):          # execute the loop ten times incrementing x value from 0 to nine
        pin = DISPLAY[x]         # assigning value to  pin  for each digit
        PORT(pin);               # showing each digit on display 
        time.sleep(1)
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
Tkinter
-------------------------------------------------------------------------------------
```python
from tkinter import *
import RPi.GPIO as GPIO
import time

GPIO.setmode(GPIO.BCM)
GPIO.setup(2,4,17,22, GPIO.OUT)
GPIO.setup(2,4,17,22, GPIO.OUT)
GPIO.setup(2,4,17,22, GPIO.OUT)
GPIO.setup(2,4,17,22, GPIO.OUT)
pwma = GPIO.PWM(2, 100)
pwmb = GPIO.PWM(4, 100)
pwmc = GPIO.PWM(17, 100)
pwmd = GPIO.PWM(22, 100)
pwma.start(5)
pwmb.start(5)
pwmc.start(5)
pwmd.start(5)

class Appa:

    def __init__(self, master):
        frame = Frame(master)
        frame.pack()
        scale = Scale(frame, from_=0, to=360, tickinterval=360,
              orient=HORIZONTAL, command=self.update, length=400, label='servo GPIO 2')
        scale.grid(row=0)


    def update(self, angle):
        duty = float(angle) / 10.0 + 2.5
        pwma.ChangeDutyCycle(duty)

class Appb:

    def __init__(self, master):
        frame = Frame(master)
        frame.pack()
        scale = Scale(frame, from_=0, to=360, tickinterval=360,
              orient=HORIZONTAL, command=self.update, length=400, label='servo GPIO 4')
        scale.grid(row=0)


    def update(self, angle):
        duty = float(angle) / 10.0 + 2.5
        pwmb.ChangeDutyCycle(duty)

class Appc:

    def __init__(self, master):
        frame = Frame(master)
        frame.pack()
        scale = Scale(frame, from_=0, to=360, tickinterval=360,
              orient=HORIZONTAL, command=self.update, length=400, label='servo GPIO 17')
        scale.grid(row=0)


    def update(self, angle):
        duty = float(angle) / 10.0 + 2.5
        pwmc.ChangeDutyCycle(duty)

class Appd:

    def __init__(self, master):
        frame = Frame(master)
        frame.pack()
        scale = Scale(frame, from_=0, to=360, tickinterval=360,
              orient=HORIZONTAL, command=self.update, length=400, label='servo GPIO 22')
        scale.grid(row=0)


    def update(self, angle):
        duty = float(angle) / 10.0 + 2.5
        pwmd.ChangeDutyCycle(duty)

root = Tk()
root.wm_title('Servo Control x 4')
appa = Appa(root)
Label(root, text='pwda').pack()
appb = Appb(root)
Label(root, text='pwdb').pack()
appc = Appc(root)
Label(root, text='pwdc').pack()
appd = Appd(root)
Label(root, text='pwdd').pack()

w = 420
h = 420
ws = root.winfo_screenwidth() 
hs = root.winfo_screenheight()
x = (ws/2) - (w/2)
y = (hs/2) - (h/2)
root.geometry('%dx%d+%d+%d' % (w, h, x, y))

root.mainloop()
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
Sensehat Simulator
-------------------------------------------------------------------------------------
```python
from sense_emu import SenseHat
from sense_emu import SenseStick
import time

hat = SenseHat()
hat.clear()


while True:
    if SenseStick.direction_any == True:
        hat.show_message(time.strftime("%H:%M"))
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
Sensehat text scroll
-------------------------------------------------------------------------------------
```python
from sense_hat import SenseHat

sense = SenseHat()
sense.set_rotation(180)
red = (255, 0, 0)
sense.show_message("One small step for Pi!", text_colour=red)
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
Sensehat image import jpg et png 8x8
-------------------------------------------------------------------------------------
```python
sense.load_image("image.png")
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
Sensehat shake
-------------------------------------------------------------------------------------
```python
while True:
    x, y, z = sense.get_accelerometer_raw().values()

    x = abs(x)
    y = abs(y)
    z = abs(z)

    if x > 1 or y > 1 or z > 1:
        sense.show_letter("!", text_colour=[255, 0, 0])
    else:
        sense.clear()
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
Sensehat rotation
-------------------------------------------------------------------------------------
```python
while True:
    x, y, z = sense.get_accelerometer_raw().values()

    x = round(x, 0)
    y = round(y, 0)

    if x == -1:
        sense.set_rotation(180)
    elif y == 1:
        sense.set_rotation(90)
    elif y == -1:
        sense.set_rotation(270)
    else:
        sense.set_rotation(0)
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
Sensehat orientation
-------------------------------------------------------------------------------------
```python
while True:
    pitch, roll, yaw = sense.get_orientation().values()
    print("Elevation/pitch=%s, Roulis/roll=%s, Embardee/yaw=%s" % (pitch,yaw,roll))
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
Sensehat lettre
-------------------------------------------------------------------------------------
```python
sense.show_letter("O",text_colour=[255, 0, 0])
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
Sensehat image
-------------------------------------------------------------------------------------
```python
sense.set_pixel(2, 2, [0, 0, 255])
sense.set_pixel(7, 7, 255, 0, 255)

B = (102, 51, 0)
b = (255, 255, 255)
steve_pixels = [
    B, B, B, B, B, b, B, B,
    B, B, B, B, b, b, B, B,
    b, b, b, b, b, b, B, B,
    B, b, b, B, B, b, B, B,
    B, B, b, B, B, b, b, B,
    B, B, b, b, b, b, b, b,
    B, B, b, b, B, B, B, B,
    B, B, b, B, B, B, B, B,
]
sense.set_pixels(steve_pixels)
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
Sensehat environnement
-------------------------------------------------------------------------------------
```python
while True:
    t = sense.get_temperature()
    p = sense.get_pressure()
    h = sense.get_humidity()

    t = round(t, 1)
    p = round(p, 1)
    h = round(h, 1)

    if t > 18.3 and t < 26.7:
        bg = [0, 100, 0]  # green
    else:
        bg = [100, 0, 0]  # red

    msg = "Temperature = {0}, Pressure = {1}, Humidity = {2}".format(t, p, h)

    sense.show_message(msg, scroll_speed=0.05, back_colour=bg)
    sense.set_rotation(90)
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
Sensehat joystick simple
-------------------------------------------------------------------------------------
```python
import pygame

from pygame.locals import *
from sense_hat import SenseHat

pygame.init()
pygame.display.set_mode((640, 480))
sense = SenseHat()
sense.clear()

running = True

while running:
    for event in pygame.event.get():
        print(event)
        if event.type == KEYDOWN:
    sense.set_pixel(x, y, 0, 0, 0)  # Noir = 0,0,0 - ce qui éteind la LED

    if event.key == K_DOWN  and y < 7:
        y = y + 1

        sense.set_pixel(x, y, 255, 255, 255)
        if event.type == QUIT:
            running = False
            print("AU REVOIR")
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
Sensehat joystick
-------------------------------------------------------------------------------------
```python
from sense_hat import SenseHat
import os
import time
import pygame
from pygame.locals import *


print("Press Escape to quit")
time.sleep(1)

pygame.init()
pygame.display.set_mode((640, 480))

sense = SenseHat()
sense.clear() 

UP_PIXELS = [[3, 0], [4, 0]]
DOWN_PIXELS = [[3, 7], [4, 7]]
LEFT_PIXELS = [[0, 3], [0, 4]]
RIGHT_PIXELS = [[7, 3], [7, 4]]
CENTRE_PIXELS = [[3, 3], [4, 3], [3, 4], [4, 4]]


def set_pixels(pixels, col):
    for p in pixels:
        sense.set_pixel(p[0], p[1], col[0], col[1], col[2])


def handle_event(event, colour):
    if event.key == pygame.K_DOWN:
        set_pixels(DOWN_PIXELS, colour)
    elif event.key == pygame.K_UP:
        set_pixels(UP_PIXELS, colour)
    elif event.key == pygame.K_LEFT:
        set_pixels(LEFT_PIXELS, colour)
    elif event.key == pygame.K_RIGHT:
        set_pixels(RIGHT_PIXELS, colour)
    elif event.key == pygame.K_RETURN:
        set_pixels(CENTRE_PIXELS, colour)


running = True

BLACK = [0, 0, 0]
WHITE = [255, 255, 255]

while running:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False
        if event.type == KEYDOWN:
            if event.key == K_ESCAPE:
                running = False
            handle_event(event, WHITE)
        if event.type == KEYUP:
handle_event(event, BLACK)
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
Sensehat compass
-------------------------------------------------------------------------------------
```python
import sys
from sense_hat import SenseHat

# To get good results with the magnetometer you must first calibrate it using
# the program in RTIMULib/Linux/RTIMULibCal
# The calibration program will produce the file RTIMULib.ini
# Copy it into the same folder as your Python code

led_loop = [4, 5, 6, 7, 15, 23, 31, 39, 47, 55, 63, 62, 61, 60, 59, 58, 57, 56, 48, 40, 32, 24, 16, 8, 0, 1, 2, 3]

sense = SenseHat()
sense.set_rotation(0)
sense.clear()

prev_x = 0
prev_y = 0

led_degree_ratio = len(led_loop) / 360.0

while True:
    dir = sense.get_compass()
    dir_inverted = 360 - dir  # So LED appears to follow North
    led_index = int(led_degree_ratio * dir_inverted)
    offset = led_loop[led_index]

    y = offset // 8           # row
    x = offset % 8            # column

    if x != prev_x or y != prev_y:
        sense.set_pixel(prev_x, prev_y, 0, 0, 0)

    sense.set_pixel(x, y, 0, 0, 255)

    prev_x = x
prev_y = y
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
SenseHat 3d
-------------------------------------------------------------------------------------
```python
from sense_hat import SenseHat
sense = SenseHat()
sense.clear()

while True:
o = sense.get_orientation()
pitch = o["pitch"]
roll = o["roll"]
yaw = o["yaw"]
pitch = round(pitch, 1) # ne garder qu un seul chiffre après le point décimal.
roll = round(roll, 1)
yaw = round(yaw, 1)
print("pitch %s roll %s yaw %s" % (pitch, roll, yaw))
```

**[`^        back to top        ^`](#)**
