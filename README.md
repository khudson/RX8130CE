# Epson Seiko RX8130CE Real-Time Clock Arduino Library

Epson Seiko RX8130CE Real-Time Clock/calendar with 4 byte RAM

## Examples

initialize 

```C++
#include <Arduino.h>
#include <RX8130CE.h>
#include <Wire.h>
#include <time.h>

RX8130CE rtc;

void setup() {
  Serial.begin(115200);

  Wire.begin();
  while(!rtc.begin(&Wire)) {
    Serial.println("RX8130CE: Failed to initialize");
    delay(1000);
  }
}
```

Set time

```C++
struct tm t;
  t.tm_hour = 23;
  t.tm_min = 59;
  t.tm_sec = 50;
  t.tm_mday = 30;
  t.tm_mon = 3 - 1; // 0 - 11
  t.tm_year = 2023 - 1900; // since 1900
  if (rtc.setTime(t)) {
    Serial.println("RX8130CE: time set");
  } else {
    Serial.println("RX8130CE: Failed to set time");
  }
```

Get time

```C++
  // Get time from RTC
  struct tm t;
  if (rtc.getTime(&t)) {
    Serial.printf(
      "Datetime: %02d:%02d:%02d %d/%d/%d\n",
      t.tm_hour, t.tm_min, t.tm_sec,
      t.tm_mday, t.tm_mon + 1, t.tm_year + 1900
    );
  } else {
    Serial.println("RX8130CE: Failed to get time");
  }
```

Full test


```C++
#include <Arduino.h>
#include <ArtronShop_RX8130CE.h>
#include <Wire.h>
#include <time.h>

RX8130CE rtc;

void setup() {
  Serial.begin(115200);

  Wire.begin();
  while(!rtc.begin(&Wire)) {
    Serial.println("RX8130CE: failed to initialize");
    delay(1000);
  }

  // Test set time to RTC
  struct tm t;
  t.tm_hour = 23;
  t.tm_min = 59;
  t.tm_sec = 50;
  t.tm_mday = 28;
  t.tm_mon = 2 - 1; // 0 - 11
  t.tm_year = 2024 - 1900; // since 1900
  if (!rtc.setTime(t)) {
    Serial.println("RX8130CE: Failed to set time");
  }
}

void loop() {
  // Test get time from RTC
  struct tm t;
  if (rtc.getTime(&t)) {
    Serial.printf(
      "Datetime: %02d:%02d:%02d %d/%d/%d\n",
      t.tm_hour, t.tm_min, t.tm_sec,
      t.tm_mday, t.tm_mon + 1, t.tm_year + 1900
    );
  } else {
    Serial.println("RX8130CE: Failed to get time");
  }
  delay(1000);
}
```
