# udev_esc_capslock_swap
1. install evtest:
   sudo apt install evtest
2. find out the modalias of the keyboard you are trying to change:
    1. sudo evtest
    2. note the /dev/input/eventX of your keyboard
    3. cat /sys/class/input/event17/device/modalias
    4. note the input:b\*v\*p\*e\*-\* up to the '-'
3. sudo -E hx /etc/udev/hwdb.d/90-keyboard-swap.hwdb
```
evdev:input:b0003v0951p16D2e0111*
 KEYBOARD_KEY_70029=capslock
 KEYBOARD_KEY_70039=esc
```
1. you get the "70029" etc. by sudo evtest and pressing the key:
    [Event: time 1768295550.679510, type 4 (EV_MSC), code 4 (MSC_SCAN), value 70039
    Event: time 1768295550.679510, type 1 (EV_KEY), code 1 (KEY_ESC), value 0
 
    you want the value behing (MSC_SCAN)
3. the value behind the '=' is from /usr/include/linux/input-event-codes.h 
4. IT SEEMS ORDER MATTERS?? map to capslock first!
4. sudo systemd-hwdb update
5. sudo udevadm trigger
6. enjoy
7. you can put multiple keyboards in one file, just put another block with different modalias (maybe even different key values)
