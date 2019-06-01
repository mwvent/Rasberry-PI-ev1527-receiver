# Rasberry PI ev1527 Receiver

I made this because none of the other libraries I found online actually worked with my ebay PIR sensor / ebay 433mhz receiver. I hope this helps anybody else pulling their hair out because the commonly availible libraries are not working for them.

The protocol is simple enougth but all timings have to be dealt as approximate with variations of around 30-50μs.

Starts with a low pulse of around 9230μs

Followed by 12 tetrabits of around 2400μs each containing either two short pulses ( <500μs ) = 0, two long pulses ( >500μs ) = 1, one short & one long = F, one long & one short = X. Great guide here https://tinkerman.cat/post/decoding-433mhz-rf-data-from-wireless-switches-the-data

This library just gets pulse edge times right now and doesnt take advantage of using the tetrabit length to correct errors caused by too many edges in a time period.

Needs pigpio deamon running and python libs installed http://abyz.me.uk/rpi/pigpio/ - it is very CPU intensive and innacurate to do this without using pigpio. An update to this library is needed to handle pigpio errors though.

Simple usage by example code below - just import, create an object ev1527.Ev1527_receiver( pinNumberOfReceiver ) and call add_callback on the object to point at a callback function that receives the codes.


```
import ev1527
import time

def print_code ( code ) :
	print( code )

rx = ev1527.Ev1527_receiver( 27 )
rx.add_callback ( print_code )

while True :
	time.sleep(100)
```

