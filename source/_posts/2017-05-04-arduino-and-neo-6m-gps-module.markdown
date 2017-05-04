---
layout: post
title: "Arduino and NEO-6M GPS Module"
date: 2017-05-04 07:32:17 +0700
comments: true
categories: [arduino,c]
---

{% img left /images/logo/arduino.png %}
Couple days ayo, I met a friend of mine when I was at university. He plays extensively with [Arduino](http://www.arduino.org), [Raspberry Pi](https://www.raspberrypi.org), [Orange Pi](http://www.orangepi.org) and other IoT stuff. He showed me how interesting IoT is, include wiring modules, and surely its programming. I remember that a few months ago, I got Arduino kit with GPS module from another friend of mine. The items were idle since I have other things to do in my work. Yesterday, I just have a free time to play with the Arduino kit. And I never play or explore Arduino before.

On the first time exploring Arduino, the hardware I use are :

1. <a href="http://www.arduino.org/products/boards/arduino-uno">Arduino Uno</a>
2. <a href="https://www.aliexpress.com/store/product/GPS-Receiver-U-blox-NEO-6M-Module-with-Ceramic-Antenna-TTL-Interface-for-raspberry-pi-2/1266255_32365271431.html">GPS Module NEO-6M-0-001</a>

To get working Arduino with GPS module, I use [TinyGPS](https://github.com/mikalhart/TinyGPS) library. TinyGPS is designed to provide most of the NMEA GPS functionality. The detail description about TinyGPS can found [here](http://arduiniana.org/libraries/tinygps/). TinyGPS is additional library for Arduino. So we need to install it before include it to our project. The steps of installation additional Arduino libraries can be found [here](https://www.arduino.cc/en/Guide/Libraries)

The table below shows wiring between Arduino and NEO-6M-0-001 GPS module

| NEO-6M-0-001 GPS     | Arduino Uno      | Cable
| -------------------- | ---------------- |-------
| Vcc                  | Power 3.3 Volt   | Black
| GND                  | GND              | White
| TXD                  | RX pin 4         | Gray
| RXD                  | TX pin 3         | Magenta

<p/>

{% img center /images/post/2017-05-04-neo-6m-0-0-001.png %}

{% img center /images/post/2017-05-04-arduino-uno.png %}

{% img center /images/post/2017-05-04-arduino-uno-gps.png %}

To simplify our testing, I grab sample source code provided by [TinyGPS](https://github.com/mikalhart/TinyGPS). On This sample, I Set the data rate in bits per second (baud) for serial data transmission to 9600.

``` cpp

#include <SoftwareSerial.h>

#include <TinyGPS.h>

/* This sample code demonstrates the normal use of a TinyGPS object.
   It requires the use of SoftwareSerial, and assumes that you have a
   4800-baud serial GPS device hooked up on pins 4(rx) and 3(tx).
*/

TinyGPS gps;
SoftwareSerial ss(4, 3);

static void smartdelay(unsigned long ms);
static void print_float(float val, float invalid, int len, int prec);
static void print_int(unsigned long val, unsigned long invalid, int len);
static void print_date(TinyGPS &gps);
static void print_str(const char *str, int len);

void setup()
{
  //Serial.begin(115200);
  Serial.begin(9600);

  Serial.print("Testing TinyGPS library v. "); Serial.println(TinyGPS::library_version());
  Serial.println("by Mikal Hart");
  Serial.println();
  Serial.println("Sats HDOP Latitude  Longitude  Fix  Date       Time     Date Alt    Course Speed Card  Distance Course Card  Chars Sentences Checksum");
  Serial.println("          (deg)     (deg)      Age                      Age  (m)    --- from GPS ----  ---- to London  ----  RX    RX        Fail");
  Serial.println("-------------------------------------------------------------------------------------------------------------------------------------");

  //ss.begin(4800);
  ss.begin(9600);
  delay(1000);
}

void loop()
{
  float flat, flon;
  unsigned long age, date, time, chars = 0;
  unsigned short sentences = 0, failed = 0;
  static const double LONDON_LAT = 51.508131, LONDON_LON = -0.128002;

  print_int(gps.satellites(), TinyGPS::GPS_INVALID_SATELLITES, 5);
  print_int(gps.hdop(), TinyGPS::GPS_INVALID_HDOP, 5);
  gps.f_get_position(&flat, &flon, &age);
  print_float(flat, TinyGPS::GPS_INVALID_F_ANGLE, 10, 6);
  print_float(flon, TinyGPS::GPS_INVALID_F_ANGLE, 11, 6);
  print_int(age, TinyGPS::GPS_INVALID_AGE, 5);
  print_date(gps);
  print_float(gps.f_altitude(), TinyGPS::GPS_INVALID_F_ALTITUDE, 7, 2);
  print_float(gps.f_course(), TinyGPS::GPS_INVALID_F_ANGLE, 7, 2);
  print_float(gps.f_speed_kmph(), TinyGPS::GPS_INVALID_F_SPEED, 6, 2);
  print_str(gps.f_course() == TinyGPS::GPS_INVALID_F_ANGLE ? "*** " : TinyGPS::cardinal(gps.f_course()), 6);
  print_int(flat == TinyGPS::GPS_INVALID_F_ANGLE ? 0xFFFFFFFF : (unsigned long)TinyGPS::distance_between(flat, flon, LONDON_LAT, LONDON_LON) / 1000, 0xFFFFFFFF, 9);
  print_float(flat == TinyGPS::GPS_INVALID_F_ANGLE ? TinyGPS::GPS_INVALID_F_ANGLE : TinyGPS::course_to(flat, flon, LONDON_LAT, LONDON_LON), TinyGPS::GPS_INVALID_F_ANGLE, 7, 2);
  print_str(flat == TinyGPS::GPS_INVALID_F_ANGLE ? "*** " : TinyGPS::cardinal(TinyGPS::course_to(flat, flon, LONDON_LAT, LONDON_LON)), 6);

  gps.stats(&chars, &sentences, &failed);
  print_int(chars, 0xFFFFFFFF, 6);
  print_int(sentences, 0xFFFFFFFF, 10);
  print_int(failed, 0xFFFFFFFF, 9);
  Serial.println();

  smartdelay(1000);
}

static void smartdelay(unsigned long ms)
{
  unsigned long start = millis();
  do
  {
    while (ss.available())
      gps.encode(ss.read());
  } while (millis() - start < ms);
}

static void print_float(float val, float invalid, int len, int prec)
{
  if (val == invalid)
  {
    while (len-- > 1)
      Serial.print('*');
    Serial.print(' ');
  }
  else
  {
    Serial.print(val, prec);
    int vi = abs((int)val);
    int flen = prec + (val < 0.0 ? 2 : 1); // . and -
    flen += vi >= 1000 ? 4 : vi >= 100 ? 3 : vi >= 10 ? 2 : 1;
    for (int i=flen; i<len; ++i)
      Serial.print(' ');
  }
  smartdelay(0);
}

static void print_int(unsigned long val, unsigned long invalid, int len)
{
  char sz[32];
  if (val == invalid)
    strcpy(sz, "*******");
  else
    sprintf(sz, "%ld", val);
  sz[len] = 0;
  for (int i=strlen(sz); i<len; ++i)
    sz[i] = ' ';
  if (len > 0)
    sz[len-1] = ' ';
  Serial.print(sz);
  smartdelay(0);
}

static void print_date(TinyGPS &gps)
{
  int year;
  byte month, day, hour, minute, second, hundredths;
  unsigned long age;
  gps.crack_datetime(&year, &month, &day, &hour, &minute, &second, &hundredths, &age);
  if (age == TinyGPS::GPS_INVALID_AGE)
    Serial.print("********** ******** ");
  else
  {
    char sz[32];
    sprintf(sz, "%02d/%02d/%02d %02d:%02d:%02d ",
        month, day, year, hour, minute, second);
    Serial.print(sz);
  }
  print_int(age, TinyGPS::GPS_INVALID_AGE, 5);
  smartdelay(0);
}

static void print_str(const char *str, int len)
{
  int slen = strlen(str);
  for (int i=0; i<len; ++i)
    Serial.print(i<slen ? str[i] : ' ');
  smartdelay(0);
}

```

The output of this testing on Serial monitor showed as follow.

{% img center /images/post/2017-05-04-output.png %}

If you do not get similar output as above (get <code>*</code> on table output) it means your arduino fails get data from GPS module. Please ensure your GPS led is blinking which indicate it receives data from the GPS satellites.

The other thing that you should ensure is you have the right wiring. RX of the Arduino (pin 4, according to the <code>SoftwareSerial</code> statement) goes to the TX of the GPS. Arduino pin 3 (ss TX) goes to the GPS RX.

To validate the accuracy of GPS output (Latitude, Longitude) showed on Arduino Serial Monitor you can check it on google map.

## References
<ul>
<li><a href="https://www.u-blox.com/sites/default/files/products/documents/NEO-6_DataSheet_(GPS.G6-HW-09005).pdf">NEO-6 u-blox 6 GPS Modules Data Sheet</a></li>
<li><a href="http://www.ayomaonline.com/iot/gy-gps6mv2-neo6mv2-neo-6m-gps-module-with-arduino-usb-ttl/">GY-GPS6MV2 – NEO6MV2 (NEO 6M) GPS MODULE WITH ARDUINO / USB TTL</a></li>
<li><a href="https://arduino.stackexchange.com/questions/24235/arduino-softwareserial-cant-get-data-from-neo-6m-gps-module">Arduino SoftwareSerial - can't get data from NEO 6M-GPS module</a></li>
<ul>
