<h1 align="center">SDI-12</h1>

<h3 align="center">Teensy 3.x/LC SDI-12 Library v3.0.1</h3>

<b>[SDI12 Specification]</b>
> SDI12 is a single wire serial protocol that uses inverted 5V logic levels, specifically (1200 baud, 7E1) for bi-directional data flow with one Master and many Slaves. This library sets up the Freescale Cortex one-wire protocol for each of its 3 Hardware Serial ports TX pin along with critical timing - Break and Mark signals to wake the sensor bus. This makes effectively 3 separate SDI12 buses that can be used or not. Since SDI12 is Master-Slave, many different types of sensor can share the same bus through the use of unique address for each sensor.<br>


<b>Level Shifting:</b>
>Since Teensy (3.x/LC) are 3.3V micrcontrollers it is actually out of SDI12 specification:<br>
1. Spacing (3.5V to 5V)<br>
2. Marking (-0.5V to 1V)<br>

>For proper level shifting I found that you can use the Adafruit's Bi-directional Logic Level Converter [TXB0104]. I2C level shifters do not work because of their 'open-drain' architecture the [TXB0104] uses 'push-pull'.
<br>


<b>Sensor Tested:</b>
>1. [Decagon 5TE]
>2. [Decagon CTD]
>3. [Keller DigiLevel]
>4. [Vaisala WXT520]

<b>Hookup - No Level Shifter:</b>
>1. Teensy 3.0 are not 5V tolerant, put a resistor inlined with the data line.<br>
>2. Teensy 3.1 is 5V tolerant so direct connection can be done.<br>
>3. While SDI12 specification states 12V is used for power, many sensors use a range of values (5-17V).<br>

Connect the sensor to Teensy using Teensy's Vin (5V) for sensor power.<br>
```
   *-----------------5V DC---------------------------------*               
   |                                                       |               
   |                                   TEENSY 3.0/LC       |               
   |                                  _______________      |               
   | *----------------GND----------->|GND |_____| Vin|>----*               
   | |                               |0  -----   AGND|                     
   | | *--Signal---/\/\/\/\--------<>|1/TX1   |  3.3V|                     
  _|_|_|__            10K            |2 |,,,,,|    23|                     
 /        \                          |3  -----     22|                     
| DECAGON  |                         |4 |'|        21|                     
|   5TE    |                         |5   ------   20|                     
|          |                         |6 |::::::::| 19|                     
|          |                         |7 |::::::::| 18|                     
|          |                         |8 |::::::::| 17|                     
|          |                         |9   ------   16|                     
 \________/                          |10    ---    15|                     
  || || ||                           |11   |(`)|   14|                     
  || || ||                           |12    ---    13|                     
  || || ||                            ---------------                       
  || || ||                                                                 
  || || ||                                                                      
  || || ||                                                                 
  \/ \/ \/                                                                 


   *-----------------5V DC---------------------------------*               
   |                                                       |                
   |                                    TEENSY 3.1         |               
   |                                  _______________      |               
   | *----------------GND----------->|GND |_____| Vin|>----*               
   | |                               |0  -----   AGND|                     
   | | *-------------Signal--------<>|1/TX1   |  3.3V|                     
  _|_|_|__                           |2 |,,,,,|    23|                     
 /        \                          |3  -----     22|                     
| DECAGON  |                         |4 |'|        21|                     
|   5TE    |                         |5   ------   20|                     
|          |                         |6 |::::::::| 19|                     
|          |                         |7 |::::::::| 18|                     
|          |                         |8 |::::::::| 17|                     
|          |                         |9   ------   16|                     
 \________/                          |10    ---    15|                     
  || || ||                           |11   |(`)|   14|                     
  || || ||                           |12    ---    13|                     
  || || ||                            ---------------                      
  || || ||                                                                 
  || || ||                                                                 
  || || ||                                                                 
  \/ \/ \/                                                                 
```

Connect the sensor to Teensy using external Vin for sensor power.<br>
```
                External Power                                             
                 ----------                                                
   *---12-7V---<|VOUT      |                                               
   | *---GND--->|GND    GND|<--*       TEENSY 3.0/LC                       
   | |           ----------    |      _______________                      
   | |                         *---->|GND |_____| Vin|                     
   | |                               |0  -----   AGND|                     
   | | *--Signal---/\/\/\/\--------<>|1/TX1   |  3.3V|                     
  _|_|_|__            10K            |2 |,,,,,|    23|                     
 /        \                          |3  -----     22|                     
| DECAGON  |                         |4 |'|        21|                     
|   5TE    |                         |5   ------   20|                     
|          |                         |6 |::::::::| 19|                     
|          |                         |7 |::::::::| 18|                     
|          |                         |8 |::::::::| 17|                     
|          |                         |9   ------   16|                     
 \________/                          |10    ---    15|                     
  || || ||                           |11   |(`)|   14|                     
  || || ||                           |12    ---    13|                     
  || || ||                            ---------------                       
  || || ||                                                                 
  || || ||                                                                      
  || || ||                                                                 
  \/ \/ \/                                                                 


                External Power                                             
                 ----------                                                
   *---12-7V---<|VOUT      |                                               
   | *---GND--->|GND    GND|<--*         TEENSY 3.1                        
   | |           ----------    |      _______________                      
   | |                         *---->|GND |_____| Vin|                     
   | |                               |0  -----   AGND|                     
   | | *------------Signal---------<>|1/TX1   |  3.3V|                     
  _|_|_|__                           |2 |,,,,,|    23|                     
 /        \                          |3  -----     22|                     
| DECAGON  |                         |4 |'|        21|                     
|   5TE    |                         |5   ------   20|                     
|          |                         |6 |::::::::| 19|                     
|          |                         |7 |::::::::| 18|                     
|          |                         |8 |::::::::| 17|                     
|          |                         |9   ------   16|                     
 \________/                          |10    ---    15|                     
  || || ||                           |11   |(`)|   14|                     
  || || ||                           |12    ---    13|                     
  || || ||                            ---------------                       
  || || ||                                                                 
  || || ||                                                                      
  || || ||                                                                 
  \/ \/ \/                                                                 
```

Connect a sensor using level shifter and Teensy's Vin for sensor power.<br>
```
                                                     *-----5V-------*     
                                                     |              |     
*----------------------GND---------------------------^-----GND----* |     
|                                                    |            | |     
|                                                    | *-SIGNAL-* | |     
|                                                    | |        | | |     
|                                                    | |       _*_*_*__   
|           TEENSY 3.x/LC                            | |      /        \  
|          _______________                           | |     | DECAGON  | 
*-------->|GND |_____| Vin|>------5V-----------------* |     |   5TE    | 
|  *-----<|0  -----   AGND|             ----------   | |     |          | 
|  |  *-<>|1/TX1   |  3.3V|>---------->|3.3V    5V|<-* |     |          | 
|  |  |   |2 |,,,,,|    23|      *---<>|A1      B1|<>--*     |          | 
|  |  |   |3  -----     22|      |     |A2      B2|          |          | 
|  |  |   |4 |'|        21|      |     |A4      B4|          |          | 
|  |  |   |5   ------   20|      |     |A3      B3|           \________/  
|  |  |   |6 |::::::::| 19|      |  *->|OE     GND|<-----*     || || ||   
|  |  |   |7 |::::::::| 18|      |  |   ----------       |     || || ||   
|  |  |   |8 |::::::::| 17|      |  |     TXB0104        |     || || ||   
|  |  |   |9   ------   16|      |  |  bi-directional    |     || || ||   
|  |  |   |10    ---    15|      |  |  level converter   |     || || ||   
|  |  |   |11   |(`)|   14|      |  |                    |     || || ||   
|  |  |   |12    ---    13|      |  |                    |     || || ||   
|  |  |    ---------------       |  |                    |     \/ \/ \/            
|  |  *--------TEENSY-SIGNAL-----*  |                    |                
|  |                                |                    |                
|  *-----------OE CONTROL-----------*                    |                
|                                                        |                
*-----------------GND------------------------------------*                
```

Connect a sensor using level shifter using Ext Vin for sensor power.<br>
```
                             External Power                               
                              ----------                                     
                             |      VOUT|>-------7-12V--------------*     
*-------------GND----------->|GND    GND|<--------GND-------------* |     
|                             ----------                          | |     
|                                                                 | |     
|                                                                 | |     
|                                                      *-SIGNAL-* | |     
|                                                      |        | | |     
|                                                      |       _*_*_*__   
|           TEENSY 3.x/LC                              |      /        \  
|          _______________                             |     | DECAGON  | 
*-------->|GND |_____| Vin|>------5V-----------------* |     |   5TE    | 
|  *-----<|0  -----   AGND|             ----------   | |     |          | 
|  |  *-<>|1/TX1   |  3.3V|>---------->|3.3V    5V|<-* |     |          | 
|  |  |   |2 |,,,,,|    23|      *---<>|A1      B1|<>--*     |          | 
|  |  |   |3  -----     22|      |     |A2      B2|          |          | 
|  |  |   |4 |'|        21|      |     |A4      B4|          |          | 
|  |  |   |5   ------   20|      |     |A3      B3|           \________/  
|  |  |   |6 |::::::::| 19|      |  *->|OE     GND|<-----*     || || ||   
|  |  |   |7 |::::::::| 18|      |  |   ----------       |     || || ||   
|  |  |   |8 |::::::::| 17|      |  |     TXB0104        |     || || ||   
|  |  |   |9   ------   16|      |  |  bi-directional    |     || || ||   
|  |  |   |10    ---    15|      |  |  level converter   |     || || ||   
|  |  |   |11   |(`)|   14|      |  |                    |     || || ||   
|  |  |   |12    ---    13|      |  |                    |     || || ||   
|  |  |    ---------------       |  |                    |     \/ \/ \/            
|  |  *--------TEENSY-SIGNAL-----*  |                    |                
|  |                                |                    |                
|  *-----------OE CONTROL-----------*                    |                
|                                                        |                
*-----------------GND------------------------------------*                
```
<h1 align="center">Library Usage</h1>

<h5>Constructor:</h5>
```c
SDI12( Stream *port, char address, bool crc = false ) ;
```
>1. ```Stream *port``` = One of the Hardware Serial Ports.
>2. ```char address``` = Sensor Address, must be preprogramed.
>3. ```bool crc```(optional) = Set 'true' to append CRC to sensor data.

Example:
```c
// Define a constructor for each sensor you plan to use.
// Serial Port can either be Serial1, Serial2, Serial3.
// Address have to be ascii values. (0-10),(a-z),(A-Z).
// CRC will be appended to returned data packet for each sensor.
SDI12 DECAGON_5TE_10CM( &Serial3, '1', true );
SDI12 DECAGON_5TE_20CM( &Serial3, '2', true );
SDI12 DECAGON_5TE_30CM( &Serial3, '3', true );
SDI12 DECAGON_5TE_40CM( &Serial3, '4', true );
```
<br>
<h3 align="center">Functions</h3>

<h5>isActive:</h5>
```c
//SDI12 (Acknowledge Active) "a!" command.
int isActive( int address = -1 );
```
>1. ```int address```(optional) = can use other address also.

Example:
```c
int error;

// Check if sensor is active using address defined in constructor. 
error = DECAGON_5TE_10CM.isActive( );
if( error ) Serial.println( "Sensor at address 1 Not Active" );

// Check if sensor is active using address defined in constructor. 
error = DECAGON_5TE_20CM.isActive( );
if( error ) Serial.println( "Sensor at address 2 Not Active" );

// Check if sensor is active using address defined in constructor. 
error = DECAGON_5TE_30CM.isActive( );
if( error ) Serial.println( "Sensor at address 3 Not Active" );

// Check if sensor is active using address defined in constructor. 
error = DECAGON_5TE_40CM.isActive( );
if( error ) Serial.println( "Sensor at address 4 Not Active" );

// Check if sensor is active using different address than defined 
// in constructor. This allows us to see if it is actually a 
// different address.
error = DECAGON_5TE_40CM.isActive( '5' );
if( error ) Serial.println( "Sensor at address 5 Not Active" );
```
<br>
---
<h5>identification:</h5>
```c
// SDI12 (Send Identification) "aI!" command.
int identification( volatile void *src );
```
>1. ```(char or uint8_t) *src``` = buffer array you supply to hold returned string.

Example:
```c
int error;
// Max size of return string is 35 character.
char buf[35]

// If no error then print id string. 
error = DECAGON_5TE_10CM.identification( buf );
if( !error ) Serial.println( buf );

// If no error then print id string. 
error = DECAGON_5TE_20CM.identification( buf );
if( !error ) Serial.println( buf );

// If no error then print id string.
error = DECAGON_5TE_30CM.identification( buf );
if( !error ) Serial.println( buf );

// If no error then print id string.
error = DECAGON_5TE_40CM.identification( buf );
if( !error ) Serial.println( buf );
```
<br>
---
<h5>queryAddress:</h5>
```c
// SDI12 (Address Query) "?!" command.
/*** Only ONE sensor can on the bus when using this command! ***/
int queryAddress( void );
```
Example:
```c
int address;

// Used to see what your sensor address actually is.
// Only one sensor can be connected to bus at a time. 
address = DECAGON_5TE_10CM.queryAddress( );
if(address != -1) {
Serial.print( "Sensor Address is " );
Serial.println( (char)address )
}
```
<br>
---
<h5>changeAddress:</h5>
```c
// SDI12 (Change Address) "aAb!" command.
int changeAddress( uint8_t new_address );
```
>1. ```uint8_t new_address``` = address you want to change to.

Example:
```c
int address;

// Change address defined in the constructor '1' to '5'.
// This address will be updated for any future use of this function.
address = DECAGON_5TE_10CM.changeAddress( '5' );
if( address != -1 ) {
Serial.print( "New Address is" );
Serial.println( (char)address );
} else {
Serial.println( "Address out of range or command failed" );
}
```
<br>
---
<h5>verification:</h5>
```c
// SDI12 (Start Verification) "aV!" command.
int verification( volatile void *src );
```
>1. ```(char or uint8_t) *src``` = buffer array you supply to hold returned string.

Example:
```c
int error;
// Buffer to hold returned string.
char buf[35];
// Optionally can get debug info on command return
char debug[10];

error = DECAGON_5TE_10CM.verification( debug );
if ( !error ) Serial.print( debug );

// Verification needs a return measurement command to get data
// 'returnMeasurement' function is explained below.
error = DECAGON_5TE_10CM.returnMeasurement( buf, 0 );
if ( !error ) Serial.print( buf );
```
<br>
---
<h5>measurement:</h5>
```c
// SDI12 (Start Measurement) command.
// "aM!", "aMC!" or "aM0...aM9" or "aMC0...aMC9"
int measurement( int num = -1 ) { uint8_t s[75]; measurement( s, num ); }
int measurement( volatile void *src, int num = -1 );
```
>1. ```(char or uint8_t) *src``` = buffer array you supply to hold acknowledgement string.
>2. ```int num = -1```(optional) = Additional measurements.

Example:
```c
int error;
// buffer to hold sensor measurement acknowledgment string.
char debug[10];
// Max return sensor string size is 81 characters.
char data[81];

// Function to tell sensor to make a measurement.
// optional additional measurements command.
/***error = DECAGON_5TE_10CM.measurement( debug, 0 );***/
error = DECAGON_5TE_10CM.measurement( debug );
if ( !error ) Serial.print( debug );
// 'measurement' needs a return measurement command to get data.
// 'returnMeasurement' function is explained below.
error = DECAGON_5TE_10CM.returnMeasurement( data, 0 );
if ( !error ) Serial.print( data );

// Function to tell sensor to make a measurement.
// optional additional measurements command.
/***error = DECAGON_5TE_20CM.measurement( debug, 0 );***/
error = DECAGON_5TE_20CM.measurement( debug );
if ( !error ) Serial.print( debug );
error = DECAGON_5TE_20CM.returnMeasurement( data, 0 );
if (!error) Serial.print( data );

// Function to tell sensor to make a measurement.
// optional additional measurements command.
/***error = DECAGON_5TE_30CM.measurement( debug, 0 );***/
error = DECAGON_5TE_30CM.measurement( debug );
if ( !error ) Serial.print( debug );
error = DECAGON_5TE_30CM.returnMeasurement( data, 0 );
if ( !error ) Serial.print( data );

// Function to tell sensor to make a measurement.
// optional additional measurements command.
/***error = DECAGON_5TE_40CM.measurement( debug, 0 );***/
error = DECAGON_5TE_40CM.measurement( debug );
if ( !error ) Serial.print( debug );
error = DECAGON_5TE_40CM.returnMeasurement( data, 0 );
if ( !error ) Serial.print( data );
```
<br>
---
<h5>concurrent:</h5>
```c
// SDI12 (Start Concurrent Measurement) command.
// "aC!","aCC!" or "aC0...aC9" or "aCC0...aCC9"
int  concurrent( volatile void *src, int num = -1 );
```
>1. ```(char or uint8_t) *src``` = buffer array you supply to hold acknowledgement string.
>2. ```int num = -1```(optional) = Additional measurements.

Example:
```c
// Function to tell sensor to make a measurement.
// optional additional measurements command.
/***error = DECAGON_5TE_40CM.concurrent( debug, 0 );***/
error = DECAGON_5TE_40CM.concurrent( debug );
if ( !error ) Serial.print( debug );
error = DECAGON_5TE_40CM.returnMeasurement( data, 0 );
if ( !error ) Serial.print( data );
```
<br>
---
<h5>continuous:</h5>
```c
// SDI12 (Start Continuous Measurement) command.
// "aR0!...aR9!" or "aRC0!...aRC9!"
int continuous( volatile void *src, int num = -1 );
```
>1. ```(char or uint8_t) *src``` = buffer array you supply to hold returned string.
>2. ```int num = -1```(optional) = Additional measurements.

Example:
```c
int error;
// buffer to hold sensor measurement string. 
// Max return sensor string size is 81 characters.
char data[81];

// Function to tell sensor to get a continuous measurement 
// if the sensor supports it.
// optional additional measurements command.
/***error = DECAGON_5TE_10CM.continuous( data, 0 );***/
error = DECAGON_5TE_10CM.continuous( data );
if ( !error ) Serial.print( data );
```
<br>
---
<h5>returnMeasurement:</h5>
---
```c
// SDI12 (Return Measurement) command.
// "aD!" or "aD0!...aD9!"
int returnMeasurement( volatile void *src, int num = -1 );
```
>1. ```(char or uint8_t) *src``` = buffer array you supply to hold returned string.
>2. ```int num = -1```(optional) = Additional measurements.

Example:
```c
// This function will send the send data command to get the data string.
// Example is provided with 'measurement' function above.
```
<br>
---
<h5>transparent:</h5>
```c
// SDI12 (Transparent) command. Allows extended SDI12 commands.
int transparent( const void *command, volatile void *src );
```
>1. ```const (char or uint8_t) *command``` = SDI12 command to send.
>2. ```(char or uint8_t) *src``` = buffer array you supply to hold returned string.

Example:
```c
int error;
// Command to send.
char cmd[3] = "2I!";
// buffer to hold sensor measurement string. 
// Max return sensor string size is 81 characters.
char data[81];

memset( data, 0, 81 );
// Function to tell sensor to send a transparent command.
// If using 'M' command it will handle sensor acknowledgement also.
error = DECAGON_5TE_10CM.transparent( cmd, data );
if ( !error ) Serial.print( data );
```

[SDI12 Specification]:http://www.sdi-12.org/current%20specification/SDI-12_version1_3%20January%2026,%202013.pdf
[Vaisala WXT520]:http://www.vaisala.com/en/products/multiweathersensors/Pages/WXT520.aspx
[Decagon 5TE]:http://www.decagon.com/products/soils/volumetric-water-content-sensors/5te-vwc-ec-temp/
[Decagon CTD]:http://www.decagon.com/products/hydrology/water-level-temperature-electrical-conductivity/ctd-5-10-sensor-electrical-conductivity-temperature-depth/
[Keller DigiLevel]:http://www.kelleramerica.com/blog/?tag=digilevel
[TXB0104]:http://www.adafruit.com/products/18
