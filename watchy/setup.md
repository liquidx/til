# Setting up

The setup instructions are at [watchy.sqfmi.com](https://watchy.sqfmi.com/docs/getting-started). 

Rough instructions because the ones above are a little out of date.

1. Install the Arduino IDE (v2.0) as of writing.
2. Open the Board Manager (Board icon on the left bar of the editor)
  (a) Install the esp32 by Espressif Systems (v2.05 as of writing)
3. Open the Library Manager (Library icon in the editor) 
  (a) Install the Watchy by SQFMI (v1.4.3)
  (b) Update the dependencies to the latest versions
      - Adafruit GFX Library (v1.11.3) 
      - Arduino_JSON (v0.2.0)
      - DS3232RTC (v2.0.1)
      - NTPClient (v3.2.1)
      - Rtc_Pcf8563 (v1.0.3)
      - GxEPD2 (v1.4.9)
      - WiFiManager (v2.0.14-beta)
4. At the top of the editor, click on "Select Board"
  (a) Choose "Select other board and port..."
  (b) Select Watchy as the board.
  (c) Select Port /dev/cu.usbserial-* 
5. Open up a Watchy Example
  (a) File > Examples > Others > Watchy > Watchfaces
  
# Issues

1. There are two different Arduino's, 1.8 and 2.0. I'm used to the 1.8 interface, and seems that a lot of the 
   instructions are written for the older UI. But the 2.0 UI is more responsive.
2. I got a compilation error when trying to do the build out of the box. In `~/Documents/Arduino/libraries/Watchy/src/Watchy.cpp`
   It seemed like an issue with the JSON parsing being ambigious whether it is a `char*` or a `String` type. 
   I couldn't quite figure out how to fix this so I commented out that line. Later on, after I updated the libraries
   a few times, the problem went away, so I couldn't tell if I screwed up something or not.
   
   ```c++
        JSONVar responseObject = JSON.parse(payload);
        currentWeather.temperature = int(responseObject["main"]["temp"]);
        currentWeather.weatherConditionCode =
            int(responseObject["weather"][0]["id"]);
        //currentWeather.weatherDescription =
        //    responseObject["weather"][0]["main"];
   ```
      
