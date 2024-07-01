
![smoochieeBLEheaderIMG](https://github.com/smoochiee/Ble-jammer/assets/30816448/8c160045-5e4c-4ad4-ae91-471beade94ba)


 
 **WARNING!!! JAMMING IS ILLEGAL**
 ---


# WHAT DOES IT DO?

**IT CREATES NOISE SIGNAL TO JAM BLUETOOTH AND WIFI USING NRF24L01 AND ESP32 IN RANGE 2.4GHZ DEVICES , EFFECTS MAY VARY DEPENDS ON DEVICE BLUETOOTH VERSIONS **


`NOT TESTED ON 2.4GHZ DRONE I DONT HAVE ONE BUT IT CAN JAM ONE..... MAYBE?` 
---


---
## REQUIRED DEVICE AND MODULE:
1. `1pc OR 2pcs NRF24`
- ![nrf24l01](https://github.com/smoochiee/Ble-jammer/assets/30816448/c370e76d-d518-409a-ba87-cb087183958c)

2. `1pc ESP32` NOTE... YOU CAN USE A `NANO OR MICRO` BUT ESP32 ARE CHEAP ALMOST SAME PRICE AS `ARDUINOS`...LOL
- ![esp32](https://github.com/smoochiee/Ble-jammer/assets/30816448/840c5d23-a6cd-4c9b-90b5-911c3d29d8e4)


3. `10UF-100UF CAPACITOR DEPEND ON YOUR NRF` 
- ![capacitor](https://github.com/smoochiee/Ble-jammer/assets/30816448/7165fcb1-bb05-4aa4-8dee-5640118b2673)

4.` 1 DIP SWITCH` (OPTIONAL IF YOU WANT TO USE DIFFERENT PATTERN OF CHANNEL HOPPING)
- ![DIP](https://github.com/smoochiee/Ble-jammer/assets/30816448/cec3ed8d-6db0-4c9a-9d20-de72abbf9c58)


---


 ## PINS TO ATTACH NRF24L01 TO ESP32

### FOR DUAL/TWO NRF24L01 
+ ` HSPI= SCK = 14, MISO = 12, MOSI = 13, CS = 15 , CE = 16`
+ ` VSPI= SCK = 18, MISO =19, MOSI = 23 ,CS =21 ,CE = 22`

### FOR SINGLE/ONE NRF24L01 YOU CAN CHOOSE BETWEEN HSPI OR VSPI 
 - `VSPI= SCK = 18, MISO =19, MOSI = 23 ,CS =21 ,CE = 22`
- `HSPI= SCK = 14, MISO = 12, MOSI = 13, CS = 15 , CE = 16` 

### SWITCH PIN (OPTIONAL)
- `PIN 33 OF YOUR ESP32 `
---
## WIRING DIAGRAM


### NRF24L01 PINS
- ![NRF24L01 PIN](https://github.com/smoochiee/Ble-jammer/assets/30816448/e41779d4-565a-4044-9b88-c0acbea0e93e)

### ESP32 PINS WHERE YOU ATTACH YOUR `NRF24` AND `SWITCH `
- ![32](https://github.com/smoochiee/Ble-jammer/assets/30816448/ba7c2991-1542-4296-8b8c-8011c2bc8e77)




### HOW YOU ATTACH CAPACITOR LOOK OUT FOR POLAROITY SIGNS `- +`
- ![NRF24 CAPACITOR](https://github.com/smoochiee/Ble-jammer/assets/30816448/bf24b643-7b70-4d73-962a-38fa0bffe30f)


--- 

## UPLOADING CODE TO ESP32

- ` WEBFLASHER `INSTALLER FOR EASY UPLOAD CODE ON ESP32 NO NEED TO DOWNLOAD INO FILE (USE CHROME OR MICROSOFT BROWSER)

 [WEBFLASHER](https://smoochiee.github.io/Ble-jammer/flash1) HERE....SELECT WHAT TO UPLOAD `VSPI,HSPI OR DUAL` AND CHOOSE RIGHT `COM PORT`


---


## DEMO


+ **TESTED 10 METERS ON BLUETOOTH 5.0 AND WORKS GREAT**
+ **TESTED 10 METERS ON NEWER BLUETOOTH 5.3 WORKING DEPENDS BETWEEN THE GAP OF BLUETOOTH DEVICE AND PHONE**
+ + **IF YOU WANT BETTER PERFORMANCE YOU CAN ADD 2.4GHZ RF AMPLIFIER** 

- https://github.com/smoochiee/Ble-jammer/assets/30816448/16f0c1f8-6f23-4cca-a352-f6ff64fe5a90

---

## COMPILING USING IDE (IF YOUR NOT USING WEBFLASHER)

* DOWNLOAD LIBRARIES YOU NEED AND INSTALL [RF24 LIBRARY](https://github.com/nRF24/RF24) AND [EZBUTTON LIBRARY](https://arduinogetstarted.com/tutorials/arduino-button-library)
* DOWLOAD CODE [INO FILE](https://github.com/smoochiee/Ble-jammer/tree/main/INO)

---


## SOME NOTES

* NRF24 CAN DO 125 CHANNELS YOU CAN EDIT IT IN CODE
- BLUETOOTH CLASSIC 80 CHANNELS
+ BLE USES 40 CHANNELS
* WIFI USES 1-14 CHANNELS
* DRONE 2.4GHZ 1-125 SO YOU NEED TO EDIT CODE TO HOPE 125 CHANNELS
* YOU CAN EDIT PAYLOAD SIZE DIRECTLY ON RF24 LIBRARY  `RF24.CPP LINE # 1972`
```
/****************************************************************************/
void RF24::startConstCarrier(rf24_pa_dbm_e level, uint8_t channel)
{
    stopListening();
    write_register(RF_SETUP, read_register(RF_SETUP) | _BV(CONT_WAVE) | _BV(PLL_LOCK));
    if (isPVariant()) {
        setAutoAck(0);
        setRetries(0, 0);
        uint8_t dummy_buf[5];
        for (uint8_t i = 0; i < 5; ++i)
            dummy_buf[i] = 0x00;

        // use write_register() instead of openWritingPipe() to bypass
        // truncation of the address with the current RF24::addr_width value
        write_register(TX_ADDR, reinterpret_cast<uint8_t*>(&dummy_buf), 5);
        flush_tx(); // so we can write to top level

        // use write_register() instead of write_payload() to bypass
        // truncation of the payload with the current RF24::payload_size value
        write_register(W_TX_PAYLOAD, reinterpret_cast<const uint8_t*>(&dummy_buf), 5);

        disableCRC();
    }
    setPALevel(level);
    setChannel(channel);
    IF_SERIAL_DEBUG(printf_P(PSTR("RF_SETUP=%02x\r\n"), read_register(RF_SETUP)));
    ce(HIGH);
    if (isPVariant()) {
        delay(1); // datasheet says 1 ms is ok in this instance
        ce(LOW);
        reUseTX();
    }
}

/****************************************************************************/

```

---
