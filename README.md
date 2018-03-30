## bitBangedSPI  
    
----    
  
SPI class for Arduino Uno and similar.  
  
For details and documentation see:  
http://www.gammon.com.au/forum/?id=10892  
  
In particular:  
http://www.gammon.com.au/forum/?id=10892&reply=6#reply6  
  
----  

I have written a small library to implement "bit banged" SPI for situations where you might not want to use the hardware SPI. It can be downloaded from:

http://www.gammon.com.au/Arduino/bitBangedSPI.zip

Also at:

https://github.com/nickgammon/bitBangedSPI

Example code:

```C++
#include <bitBangedSPI.h>

bitBangedSPI bbSPI (5, 6, 7);  // MOSI, MISO, SCK
const byte mySS =  8;  // slave select

void setup (void)
  {
  bbSPI.begin ();
  pinMode (mySS, OUTPUT);
  }  // end of setup

void loop (void)
  {
  char c;
  
  // enable Slave Select
  digitalWrite(mySS, LOW); 
  
  // send test string
  for (const char * p = "Hello, world!" ; c = *p; p++)
    bbSPI.transfer (c);

  // disable Slave Select
  digitalWrite(mySS, HIGH);

  delay (100); 
  }  // end of loop
```

The constructor specifies the MOSI, MISO and SCK pins. It is your responsibility to manage "slave select".

Both MOSI and MISO may be bitBangedSPI::NO_PIN, in which case they are not written to/read from, in case you are working with a one-way device (eg. an output shift register).

The library uses digitalRead and digitalWrite so it is not particularly fast, but for situation like reading configuration switches this shouldn't be an issue. Timing shows that it takes around 240 uS to transfer one byte.
