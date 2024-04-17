# The SPIRITUS Beacon

Security, Protection, Identification, Rescue, Information, Tracking, & Unit Support

## Introduction

SPIRITUS is a beacon message format used by humanitarian groups for the purposes of coordinating humanitarian & disaster response in conflict zones and unstable regions. The contents of the beacon message contain information that the receiving device can use to identify the beacon owner and provide accurate coordinates. SPIRITUS is specified as part of the larger [SANCTUS](./sanctus.md) framework.

## SPIRITUS Beacon Message Format (SBM):

### Field Descriptions

| Field       | Description                                       | Bits    | Type        |
| ----------- | ------------------------------------------------- | ------- | ----------- |
| OID         | Organization Identifier                           | 32      | BIN         |
| BID         | Beacon Identifier                                 | 16      | BIN         |
| GPS         | Geo-spatial Coordinate Data                       | 64      | FLOAT,FLOAT |
| STATUS      | Status Code (see below)                           | 14      | NUM         |
| TYPE        | Beacon Type (see table)                           | 4       | BIN         |
| PGP KEY     | Beacon PGP Public Key                             | 2048    | ASCII       |
| FINGERPRINT | Agency Fingerprint (used for key validation)      | 256     | HEX         |
| OSIG        | Beacon Key Signature (signed by organization key) | 320     | ASCII       |
| BSIG        | Message Signature (signed by beacon key)          | 320     | ASCII       |
| INFO SIZE   | Size of INFO Field (number of bytes)              | 16      | NUM         |
| INFO        | Free Field for Additional Info                    | 0-65536 | ASCII       |
| CRC         | Error Correction Bits                             | 17      | BIN         |

#### OID - Organization Identifier

The OID is a 32-bit value used to identify a given agency, in general agencies should coordinate with one another to ensure each uses a separate OID to improve the efficiency of authenticating SPIRITUS beacon messages.

#### BID - Beacon Identifer

The BID is a 16-bit value used to identify individual beacons issued by an agency, this is provided as a convenience measure and may be used for asset tracking & data analytics purposes. 

#### GPS - Geospatial Coordinates

GPS coordinates are encoded as a pair of float values (32-bit) with the first 32 bits representing the latitude and the next 32 bits representing the longitude. This ensures an accuracy of up to 1.1cm, which should be more than sufficient for target discrimination purposes.

#### STATUS - Status Codes

Status codes are encoded as a special numeric type of 14 bits, ranging from 0000-9999. Status codes should be readily decoded into an unsigned integer type by padding them with two 0's and reading them as a uint16_t type:Organization

```c
// Big Endian Systems:
uint16_t mask = 0x3FF; // Binary: 0011 1111 1111 1111
// Extract 14 bit value
uint16_t status = status_raw & mask;
// Conversion For Little Endian Systems:
status = (status >> 8) | (status << 8);
```

A table of STATUS codes can be found [here](##STATUS CODES)

#### TYPE - Beacon Type Code

Beacon Types are provided to allow for additional insight into the use of a given beacon. These are 4-bit, binary values that should be used with an appropriate lookup table. Beacon type codes are listed [here](##Beacon Type Codes)

#### PGP - Encryption Key

This field stores the ASCII-encoded PGP public key for the beacon. This key is used to sign beacon messages, and is used to secure the protocol by validating the messages received were not tampered with or spoofed in transit. Each beacon should have its' own private/public keys used for this purpose. 

#### FINGERPRINT - Organization Public Key Fingerprint

This field stores the ASCII-encoded fingerprint of the organization key and is used to search the key database if an OID collision occurs, or if the OID is unknown.

#### OSIG - Organization Signature

This field stores the ASCII-encoded signature of the beacon's PGP public key. This is used to validate that a beacon belongs to an organization and can be trusted.

#### BSIG - Beacon Signature

This field stores the ASCII-encoded signature of the beacon message, and is used to validate that the beacon message was created by the PGP key owner. 

#### INFO - Info Size & Info Fields

Info Size is a 16-bit numeric field used to denote the number of bytes used by the INFO field. This allows for encoding up to 65KB of ASCII data in the Info field.

#### CRC - Cyclic Redundancy Check

There are 17 bits provided for cyclic redundancy checking. 

### Endianness

SPIRITS Beacons use big-endian (left to right) encoding. 

## Status Codes

### 0000-0999: Operational Status

| Value     | Name      | Description                                                  |
| --------- | --------- | ------------------------------------------------------------ |
| 0000      | OK        | Indicates beacon is functioning normally without issues.     |
| 0100      | ALERT     | Indicates a non-critical issue or condition that requires attention but is not an immediate threat. |
| 0100-0199 | RESERVED  | Reserved for granular alert status codes.                    |
| 0200      | STANDBY   | Indicate a beacon device is operational but inactive or in standby mode. |
| 0201      | EMERGENCY | Indicates a critical emergency situation requiring immediate assistance. |
| 0202-0299 | RESERVED  | Reserved for granular emergency status codes.                |
| 0300-0999 | RESERVED  | Reserved for future use                                      |

### 1000-1999: Equipment Status.

| Value     | Name        | Description                                                  |
| --------- | ----------- | ------------------------------------------------------------ |
| 1000      | LOW BATTERY | Used to notify nearby receivers that beacon may cease operation due to power loss. |
| 1001      | MALFUNCTION | Used to indicate a malfunction with the beacon device.       |
| 1001-1999 | RESERVED    | Reserved for future use.                                     |

### 2000-2999: Communication Status

| Value     | Name      | Description                                    |
| --------- | --------- | ---------------------------------------------- |
| 2000      | CALL BACK | Prompt contact requested, see info for details |
| 2001-2999 | RESERVED  | Reserved for future use.                       |

### 3000-3999: Operational Conditions

| Value     | Name     | Description                                                  |
| --------- | -------- | ------------------------------------------------------------ |
| 3000      | EVAC     | Used to notify others that you are currently evacuating the area/region. |
| 3001      | MEDIC    | Used to indicate you are currently rendering medical aid.    |
| 3002-3999 | RESERVED | Reserved for future use.                                     |

### 4000-4999: Environmental Conditions

| Value     | Name           | Description                          |
| --------- | -------------- | ------------------------------------ |
| 4000      | SEVERE WEATHER | Indicates severe weather in region.  |
| 4001-4999 | RESERVED       | Reserved for granular weather types. |

### 5000-9999: Unallocated

## Beacon Type Codes

| Value (Binary) | Name             | Use                                          |
| -------------- | ---------------- | -------------------------------------------- |
| 0000           | RESERVED         | Do Not Use                                   |
| 0001           | Individual       | Used for individually issued beacons.        |
| 0010           | Medical          | Carried by medical personnel                 |
| 0011           | Equipment        | Used for tracking equipment & supplies       |
| 0100           | Ground Vehicle   | Installed on motorcycles, cars, trucks, etc. |
| 0101           | Aerial Vehicle   | Installed on aircraft, drones, etc.          |
| 0110           | Maritime Vehicle | Installed on boats, ships, etc.              |
| 0111-1111      | RESERVED         | Do Not Use (Reserved for future use)         |

