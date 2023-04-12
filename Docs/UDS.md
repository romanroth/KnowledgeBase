# UDS

> ISO Standard: [ISO 14229](https://www.iso.org/search.html?q=ISO%2014229)

- Great Article to learn about UDS: [https://www.csselectronics.com/pages/uds-protocol-tutorial-unified-diagnostic-services](https://www.csselectronics.com/pages/uds-protocol-tutorial-unified-diagnostic-services)
- [https://uds.readthedocs.io/en/latest/index.html](https://uds.readthedocs.io/en/latest/index.html)

## UDS Service Identifiers (SIDs)

> 💡 TIP: To find the Response SID to a Request SID just add `0x40` to it.

### Diagnostic and Communication Management

| Request SID | Response SID | Service                        | Details                                                                  |
| :---------: | :----------: | :----------------------------- | :----------------------------------------------------------------------- |
|   `0x10`    |    `0x50`    | **Diagnostic Session Control** | *Control which UDS services are available.*                              |
|   `0x11`    |    `0x51`    | **ECU Reset**                  | *Reset the ECU ("hard reset", "key off", "soft reset").*                 |
|   `0x27`    |    `0x67`    | **Security Access**            | *Enable use of security-critical services via authentication.*           |
|   `0x28`    |    `0x68`    | **Communication Control**      | *Turn sending/receiving of messages on/off in the ECU.*                  |
|   `0x29`    |    `0x69`    | **Authentication**             | *Enable more advanced authentication (vs. 0x27) via PKI based exchange.* |
|   `0x3E`    |    `0x7E`    | **Tester Present**             | *Send a "heartbeat" periodically to remain in the current session.*      |
|   `0x83`    |    `0xC3`    | **Access Timing Parameters**   | *View/modify timing parameters used in client/server communication.*     |
|   `0x84`    |    `0xC4`    | **Secured Data Transmission**  | *Send endcrypted data via ISO 15764 (Extended Data Link Security).*      |
|   `0x85`    |    `0xC5`    | **Control DTC Settings**       | *Enable/disable detection of errors (e.g. used during diagnostics).*     |
|   `0x86`    |    `0xC6`    | **Response On Event**          | *Requests that an ECU processes a service request if an event happens.*  |
|   `0x87`    |    `0xC7`    | **Link Control**               | *Set the baud rate for diagnostic access.*                               |

### Data Transmission

| Request SID | Response SID | Service                                | Details                                                                   |
| :---------: | :----------: | :------------------------------------- | :------------------------------------------------------------------------ |
|   `0x22`    |    `0x62`    | **Read Data By Identifier**            | *Read data from targeted ECU (e.g. VIN, sensor data etc.).*               |
|   `0x23`    |    `0x63`    | **Read Memory By Address**             | *Read data from physical memory (e.g. to understand software behaviour).* |
|   `0x24`    |    `0x64`    | **Read Scaling Data By Identifier**    | *Read information about how to scale data identifiers.*                   |
|   `0x2A`    |    `0x6A`    | **Read Data By Identifier Periodic**   | *Request ECU to broadcast sensor data at slow/medium/fast/stop rate.*     |
|   `0x2C`    |    `0x6C`    | **Dynamically Define Data Identifier** | *Define data parameter for use in 0x22 or 0x2A dynamically.*              |
|   `0x2E`    |    `0x6E`    | **Write Data By Identifier**           | *Program specific variables determined by data parameters.*               |
|   `0x3D`    |    `0x7D`    | **Write Memory By Address**            | *Write information to the ECU's memory.*                                  |

### Stored Data Transmission

| Request SID | Response SID | Service                          | Details                                     |
| :---------: | :----------: | :------------------------------- | :------------------------------------------ |
|   `0x14`    |    `0x54`    | **Clear Diagnostic Information** | *Delete stored DTCs.*                       |
|   `0x19`    |    `0x59`    | **Read DTC Information**         | *Read stored DTCs and related information.* |

### InputOutput Control

| Request SID | Response SID | Service                               | Details                                                |
| :---------: | :----------: | :------------------------------------ | :----------------------------------------------------- |
|   `0x14`    |    `0x54`    | **Input Outpu Control By Identifier** | *Gain control over ECU analog/digital inputs/outputs.* |

### Routine

| Request SID | Response SID | Service             | Details                                                                |
| :---------: | :----------: | :------------------ | :--------------------------------------------------------------------- |
|   `0x31`    |    `0x71`    | **Routine Control** | *Initiate/stop routines (e.g. self-testing, erasing of flash memory).* |

### Upload Download

| Request SID | Response SID | Service                   | Details                                                                   |
| :---------: | :----------: | :------------------------ | :------------------------------------------------------------------------ |
|   `0x34`    |    `0x74`    | **Request Download**      | *Start request to add software/data to ECU (including location/size).*    |
|   `0x35`    |    `0x75`    | **Request Upload**        | *Start request to read software/data from ECU (including location/size).* |
|   `0x36`    |    `0x76`    | **Transfer Data**         | *Perform actual transfer of data following use of 0x74/0x75.*             |
|   `0x37`    |    `0x77`    | **Request Transfer Exit** | *Stop the transfer of data.*                                              |
|   `0x38`    |    `0x78`    | **Request File Transfer** | *Perform a file download/upload to/from the ECU.*                         |

### Negative Response

| Request SID | Response SID | Service               | Details                                                                |
| :---------: | :----------: | :-------------------- | :--------------------------------------------------------------------- |
|             |    `0x7F`    | **Negative Response** | *Sent with a Negative Response Code when a request cannot be handled.* |

## UDS Example

In this example we want to read the ECU Identification from the ECU (also called "manufacturer spare part number").

For this we use the UDS service `Read Data by Identifier` (`0x22`) with the Data Identifier (DID) of `0xF187`.

The ECU CAN ID in this example is `0x200` and the Tester CAN ID is `0x100`.

Also no padding is used in this example.

```bash
CANID   Data                      Explanation
--------------------------------------------------
0x100   03 22 F1 87               # Request length 3 (0x03), SID 0x22, DID 0xF187.
0x200   10 15 62 F1 87 4D 59 5F   # Positive Response (RSID 0x62 DID 0xF187). Multiframe length 21 bytes (0x15). Start of data (ASCII): MY_
0x100   30 00 00                  # Flow Control Tester
0x200   21 45 43 55 5F 49 44 45   # Consecutive Frame with data (ASCII): ECU_IDE
0x200   22 4E 54 49 46 49 43 41   # Consecutive Frame with data (ASCII): NTIFICA
0x200   23 54 49 4F 4E            # Consecutive Frame with data (ASCII): TION
```

Here we can see that the ECU returns "MY_ECU_IDENTIFICATION" as its ECU identification.



