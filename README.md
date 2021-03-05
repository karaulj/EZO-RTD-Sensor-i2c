# EZO-RTD-Sensor-i2c
A simple C library for reading data from Atlas Scientific's EZO RTD sensor with the I2C protocol.

[Atlas Scientific's EZO RTD sensor](https://atlas-scientific.com/embedded-solutions/ezo-rtd-temperature-circuit/) is a temperature sensor accessible through serial or I2C. They provide code for issuing commands/taking readings via Python or Arduino/C++ but none for C so I thought I'd share a helper library I developed.

## Usage
Only the "Read" command is implemented. You can use the sample code from Atlas Scientific for other commands.

MCU I2C implementations differ so no MCU-specific functions are used. Here's a sample snippet using STM32's HAL library in blocking mode (tested on the NUCLEO-L432KC board):
```
// request reading (left shift I2C address by 1 for STM32)
HAL_I2C_Master_Transmit(&hi2c1, RTD_I2C_ADDR_DEFAULT<<1, (uint8_t*)RTD_I2C_Cmd_Read, sizeof(RTD_I2C_Cmd_Read), 10);

// delay for EZO RTD processing
HAL_Delay(RTD_I2C_LONG_DELAY_MS);

// get response
HAL_I2C_Master_Receive(&hi2c1, RTD_I2C_ADDR_DEFAULT<<1, &RTD_I2C_resBuf[0], RTD_I2C_BUFFER_MAX_SZ, 10);

// optionally convert reading to float/double
double retVal = getRTDReading(RTD_I2C_resBuf);
```

## Dependencies
This library uses stdlib and stdint for converting the ASCII reading to a double and using fixed-width integer types, respectively.
