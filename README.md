# TOF10120_M5STACK
# Code to configure a TOF10120 sensor with M5STACK and UIFlow. Since this will create code in micropython I think it is quite convenient for learning purposes
# 2021 Fernando Doutel

from m5stack import *
from m5ui import *
from uiflow import *
import wifiCfg
import ntptime
import i2c_bus
import unit

setScreenColor(0x222222)
color0 = unit.get(unit.COLOR, unit.PORTA)


distmm = None

wifiCfg.doConnect('Name', 'Password')
label2 = M5TextBox(92, 43, "D", lcd.FONT_DejaVu56, 0xFFFFFF, rotate=0)
time = M5TextBox(34, 152, "00:00:00", lcd.FONT_DejaVu56, 0xFFFFFF, rotate=0)




ntp = ntptime.client(host='de.pool.ntp.org', timezone=1)
speaker.tone(800, 200)
i2c0 = i2c_bus.easyI2C((21, 22), 0x52, freq=4000)
while True:
  i2c0.write_u8(0x52, 0x00)
  distmm = (i2c0.read_data(1, i2c_bus.INT16BE))[-1]
  label2.setText(str((str(distmm) + str('mm'))))
  label0.setText(str(ntp.formatTime(':')))
  wait_ms(2)
