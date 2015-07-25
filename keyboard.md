# Keyboard programming

A keyboard firmware can be divided in three parts

1. *Scan Module* Deals with collecting the state of all the keyboard switches
2. *Translation Module* Handles the layout mapping from scan codes to output codes
3. *Output Module* Takes output codes and sends the correct signals to the host computer

## Scan Module

The scan module is responsible for collecting the state of every single keyboard switch. Most micro controllers have single processing core and that means that a micro controller can only read a single switch at a time. That means that they are problems when actually multiple keys are pressed at the same time. Rather than sending each key press to the host, the microcontroller  caches the state of each key until all the keys are pressed. It is easy to keep track of as every key has its unique scan code.

## Translation module

Now that every unique scan code and therefore the state of the keyboard is known, we need to convert each scan code to something the host can understand. On most modern system that means translating the scan codes to USB codes, which are defined in the [USB HID Keyboard spec](http://www.usb.org/developers/hidpage/HID1_11.pdf).

## Output module

Basically all of the USB codes that are currently pressed are send to the host in a single message.

## Resources

- [Introduction to programming](https://www1.massdrop.com/article/introduction-to-keyboard-programming)
