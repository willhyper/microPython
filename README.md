# MicronPython on ESP8266

# prerequisite 1 driver
install driver
Silicon Labs VCP Driver.pkg

# prerequisite 2 an environment
condo create -n microPython python=3.6
. activate microPython
pip install -r requirements.txt
pip install --upgrade micropython-iot-software/python-tools/*whl

# prerequisite 3 bin, the controller firmware
# erase and burn the fw. this will put boot.py under root
esptool.py --port /dev/tty.SLAB_USBtoUART erase_flash
esptool.py --port /dev/tty.SLAB_USBtoUART --baud 460800 write_flash --verify -fm dio 0 micropython-iot-software/esp8266-20170612-v1.9.1.bin

# connect controller via USB, you can start talking to it
export AMPY_PORT=/dev/tty.SLAB_USBtoUART

# assert boot.py is there
ampy ls

# put python files to micro controller
ampy put micropython-iot-software/main.py main.py

# content of main.py
print(‘mission completed!’)

# run 
ampy run main.py

# alternative 1
mpfshell
open tty.SLAB_USBtoUART
repl
[entered python interactive]
print(‘mission completed!’)

# alternative 2
screen /dev/tty.SLAB_USBtoUART 115200
[entered python interactive]
print(‘mission completed!’)
[Exit screen, using: Control-A + k + y]


# edit main.py
from tsl2591 import Tsl2591
tsl = Tsl2591('lux-1')
v = tsl.sample()
print(v)

# put tsl2591 interface
ampy put tsl2591.py

# run main.py
ampy run main.py

# tip
main.py is invoked whenever controller is reset.
to reset:
soft: ampy reset
hard: press the reset button on controller

