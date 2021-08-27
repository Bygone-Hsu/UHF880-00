# UHF880-00 Command Set

## Index of Contents

1. [History](#1-history)
2. [Symbols](#2-symbols)
3. [GNetPlus Protocol](GNetPlus%20Protocol.md)
    1. [Basic Package](GNetPlus%20Protocol.md#basic-package)
    2. [CRC16 Calculation](GNetPlus%20Protocol.md#crc16-calculation)
    3. [GNetPlus Implement](GNetPlus%20Protocol.md#gnetplus-implement)
4. [Commands](#4-commands)
    1. [Get Version](#4-1-get-version-command)
    2. [Set Working Mode](#4-2-set-working-mode)
5. [Error Code](#5-error-code)



## 1. History

### 2021/08/27

*   Initial Version



## 2\. Symbols

Symbols to this command set are as follows

### Data Types

The RF385-00 commands digital data order uses **Big Endian**

| Type | Bytes | Value Range     | Description              |
| :--: | :---: | :-------------- | ------------------------ |
|  s8  |   1   | -128 \~ 127     | 8 bit signed integer     |
| s16  |   2   | -32768 \~ 32767 | 16 bit signed integer    |
|  u8  |   1   | 0 \~ 255        | 8 bit unsigned integer   |
| u16  |   2   | 0 \~ 65535      | 16 bit  unsigned integer |



## 4\. Commands

RF385-00 commands is base on [GNetPlus Protocol](GNetPlus%20Protocol.md)

## *Request Code List*

| Name | Code | Description |
| :---: | :---: | ------- |
| [Get Version](#4-1-get-version-command) | `10h` | Get Firmware / Hardware version |
| [Set Working Mode](#4-2-set-working-mode) | `12h` | Set Working Mode |
| [Read EEPROM](#4-3-read-eeprom) | 24h | Read data from EEPROM |
| [Write EEPROM](#4-4-write-eeprom) | 22h | Write data to EEPROM |



## 4-1\. Get Version Command

Request Code is 10h

### Get Firmware/Hardware Version

* Parameter

| Offset | Bytes | Type | Name | Description |
| :----: | :---: | :---: | ---- | ----------- |
| 0 | 1 | u8 | Index | Version Index<br>0: Firmware Version<br>1: Hardware Version |

* Response
    * NAK: Response an [error code](#4\.-error-code)
    * ACK: Success with response data
        * Response Data

| Offset | Bytes | Type | Name | Description |
| :----: | :---: | :--: | ------- | ---------------------------- |
| 0 | N | u8 | Version | Firmware or Hardware Version |

#### Example

For the NAK response of the example command, please refer to [GNetPlus's example](GNetPlus%20Protocol.md#response-a-nak-package)

* Get Firmware Version

<br />`[Send 7 Bytes] Get Version (10h: Code: Command)`

| `Offset` | `00` | `01` | `02` | `03` | `04` | `05` | `06` | `07` | `08` | `09` | `0A` | `0B` | `0C` | `0D` | `0E` | `0F` | <div style='min-width:8em' align='center'>`ASCII`</div> |
| :------: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---- |
| `00` | `01` | `00` | `10` | `01` | `00` | `71` | `00` |  |  |  |  |  |  |  |  |  | `.....q.` |

| `Offset` | `Bytes` | <div style='min-width:12em' align='center'>`Data`</div> | <div style='min-width:32em' align='center'>`Description`</div> |
| :------: | :---: | :--- | :---- |
| `00` | `1` | ` 01` | `SOH (Start of Heading)` |
| `01` | `1` | ` 00` | `00h: Device Address: Broadcast (Any Device)` |
| `02` | `1` | ` 10` | `10h: Code: Get Version` |
| `03` | `1` | ` 01` | `Data Length` |
| `04` | `1` | ` 00` | `0: Version Index: Firmware Version` |
| `05` | `2` | ` 71 00` | `CRC16` |

<br />`[Receive 43 Bytes] Get Version Response (06h: Code: ACK)`

| `Offset` | `00` | `01` | `02` | `03` | `04` | `05` | `06` | `07` | `08` | `09` | `0A` | `0B` | `0C` | `0D` | `0E` | `0F` | <div style='min-width:8em' align='center'>`ASCII`</div> |
| :------: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---- |
| `00` | `01` | `FF` | `06` | `25` | `52` | `46` | `33` | `38` | `35` | `2D` | `30` | `30` | `20` | `52` | `4F` | `4D` | `...%RF385-00 ROM` |
| `10` | `2D` | `54` | `31` | `37` | `38` | `34` | `20` | `56` | `31` | `2E` | `30` | `30` | `52` | `33` | `20` | `28` | `-T1784 V1.00R3 (` |
| `20` | `32` | `30` | `30` | `33` | `31` | `31` | `30` | `29` | `00` | `27` | `C0` |  |  |  |  |  | `2003110).'.` |

| `Offset` | `Bytes` | <div style='min-width:12em' align='center'>`Data`</div> | <div style='min-width:32em' align='center'>`Description`</div> |
| :------: | :---: | :--- | :---- |
| `00` | `1` | ` 01` | `SOH (Start of Heading)` |
| `01` | `1` | ` FF` | `Device Address` |
| `02` | `1` | ` 06` | `06h: Code: ACK` |
| `03` | `1` | ` 25` | `Data Length` |
| `04` | `37` | ` 52 46 33 38 35 2D 30 30` <br /> ` 20 52 4F 4D 2D 54 31 37` <br /> ` 38 34 20 56 31 2E 30 30` <br /> ` 52 33 20 28 32 30 30 33` <br /> ` 31 31 30 29 00` | `Firmware Version:`<br />`RF385-00 ROM-T1784 V1.00R3 (2003110)` |
| `29` | `2` | ` 27 C0` | `CRC16` |



## 4-2\. Set Working Mode

Request Code is 12h

### Enter Auto / Command Mode

* Parameter

| Offset | Bytes | Type | Name | Description                                                  |
| :----: | :---: | :--: | ---- | ------------------------------------------------------------ |
|   0    |   1   |  u8  | Mode | New [Working Mode](#working-mode)<br />0: Auto<br />1: Command |

* Response
    * NAK: Response an [error code](#4\.-error-code). 
    * ACK: Success with response data
        * Response Data

| Offset | Bytes | Type | Name | Description |
| :----: | :---: | :--: | ------- | ---------------------------- |
| 0 | 1 | u8 | Mode | Current [Working Mode](#working-mode)<br />0: Auto<br />1: Command |

#### Working Mode

| Value | Description                                                  |
| ----- | ------------------------------------------------------------ |
| 0     | Auto Mode:<br />Automatic inventory tag and output tag data  |
| 1     | Command Mode:<br />Waiting for the command without inventory Tag |



#### Example

For the NAK response of the example command, please refer to [GNetPlus's example](GNetPlus%20Protocol.md#response-a-nak-package)

* Enter Auto Mode
<br />`[Send 7 Bytes] Enter Auto Mode (10h: Code: Command)`

| `Offset` | `00` | `01` | `02` | `03` | `04` | `05` | `06` | `07` | `08` | `09` | `0A` | `0B` | `0C` | `0D` | `0E` | `0F` | <div style='min-width:8em' align='center'>`ASCII`</div> |
| :------: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---- |
| `00` | `01` | `00` | `10` | `01` | `00` | `71` | `00` |  |  |  |  |  |  |  |  |  | `.....q.` |

| `Offset` | `Bytes` | <div style='min-width:12em' align='center'>`Data`</div> | <div style='min-width:32em' align='center'>`Description`</div> |
| :------: | :---: | :--- | :---- |
| `00` | `1` | ` 01` | `SOH (Start of Heading)` |
| `01` | `1` | ` 00` | `Device Address` |
| `02` | `1` | ` 10` | `00h: Code: Set Working Mode` |
| `03` | `1` | ` 01` | `Data Length` |
| `04` | `1` | ` 00` | `00h: [Working Mode](#working-mode): Auto Mode` |
| `05` | `2` | ` 71 00` | `CRC16` |

<br />`[Receive 7 Bytes] Enter Auto Mode Response (06h: Code: ACK)`

| `Offset` | `00` | `01` | `02` | `03` | `04` | `05` | `06` | `07` | `08` | `09` | `0A` | `0B` | `0C` | `0D` | `0E` | `0F` | <div style='min-width:8em' align='center'>`ASCII`</div> |
| :------: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---- |
| `00` | `01` | `FF` | `06` | `01` | `00` | `A1` | `D1` |  |  |  |  |  |  |  |  |  | `.......` |

| `Offset` | `Bytes` | <div style='min-width:12em' align='center'>`Data`</div> | <div style='min-width:32em' align='center'>`Description`</div> |
| :------: | :---: | :--- | :---- |
| `00` | `1` | ` 01` | `SOH (Start of Heading)` |
| `01` | `1` | ` FF` | `Device Address` |
| `02` | `1` | ` 06` | `06h: Code: ACK` |
| `03` | `1` | ` 01` | `Data Length` |
| `04` | `1` | ` 00` | `00h: [Working Mode](#working-mode): Auto Mode` |
| `05` | `2` | ` A1 D1` | `CRC16` |

* Enter Command Mode
<br />`[Send 7 Bytes] Enter Command Mode (12h: Code: Command)`

| `Offset` | `00` | `01` | `02` | `03` | `04` | `05` | `06` | `07` | `08` | `09` | `0A` | `0B` | `0C` | `0D` | `0E` | `0F` | <div style='min-width:8em' align='center'>`ASCII`</div> |
| :------: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---- |
| `00` | `01` | `00` | `12` | `01` | `01` | `71` | `60` |  |  |  |  |  |  |  |  |  | <code>.....q\`</code> |

| `Offset` | `Bytes` | <div style='min-width:12em' align='center'>`Data`</div> | <div style='min-width:32em' align='center'>`Description`</div> |
| :------: | :---: | :--- | :---- |
| `00` | `1` | ` 01` | `SOH (Start of Heading)` |
| `01` | `1` | ` 00` | `Device Address` |
| `02` | `1` | ` 12` | `00h: Code: Change Working Mode` |
| `03` | `1` | ` 01` | `Data Length` |
| `04` | `1` | ` 01` | `01h: Working Mode: Command Mode` |
| `05` | `2` | ` 71 60` | `CRC16` |

<br />`[Receive 7 Bytes] Enter Command Mode Response (06h: Code: ACK)`

| `Offset` | `00` | `01` | `02` | `03` | `04` | `05` | `06` | `07` | `08` | `09` | `0A` | `0B` | `0C` | `0D` | `0E` | `0F` | <div style='min-width:8em' align='center'>`ASCII`</div> |
| :------: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---- |
| `00` | `01` | `FF` | `06` | `01` | `00` | `A1` | `D1` |  |  |  |  |  |  |  |  |  | `.......` |

| `Offset` | `Bytes` | <div style='min-width:12em' align='center'>`Data`</div> | <div style='min-width:32em' align='center'>`Description`</div> |
| :------: | :---: | :--- | :---- |
| `00` | `1` | ` 01` | `SOH (Start of Heading)` |
| `01` | `1` | ` FF` | `Device Address` |
| `02` | `1` | ` 06` | `06h: Code: ACK` |
| `03` | `1` | ` 01` | `Data Length` |
| `04` | `1` | ` 00` | `01h: Working Mode: Command Mode` |
| `05` | `2` | ` A1 D1` | `CRC16` |



## 5\. Error Code

| Value | Description |
| :---: | ----------- |
| E0h | Access Denied |
| E4h | Illegal Query Code |
| E6h | Overrun, Out of record count |
| E7h | CRC Error |
| ECh | Query Number no support      |
| EDh | Out Of Memory Range |
| EEh | Address Number out of range |
| EFh | Unknown                      |
