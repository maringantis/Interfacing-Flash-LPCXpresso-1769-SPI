# Interfacing-Flash-LPCXpresso-1769-SPI
Interfacing Flash Memory With LPCXpresso 1769 Using SPI

Pseudo Code

#include "mbed.h"
Serial pc (13,14);
DigitalOut pin_out(19); /*MAX 485 select*/
DigitalOut _ncs(8);
SPI _spi(5,6,7);
int main()
{
_ncs = 1;
_spi.format(8,0);
_spi.frequency(10000000);
_ncs = 0;
_spi.write(0x9f);
int ID = _spi.write(0x00) << 8; // write dummy word to
get upper byte
ID |= _spi.write(0x00); // write dummy word to get
lower byte
_ncs = 1;
pin_out=1;
pc.printf("ID register is 0x%X\n",ID);
wait(1.0);
pin_out=1;
pc.printf("Writing to SRAM buffer #1\n");
_ncs = 0;
_spi.write(0x84); // Buffer 1 write command
_spi.write(0x0); // address 0
_spi.write(0x0); // address 0
_spi.write(0x0); // address 0
// Write 8 bytes of data
for (int i = 0 ; i < 8 ; i++){
_spi.write(i);
}
_ncs = 1;
wait(1.0);
pin_out=1;
pc.printf("Reading from SRAM buffer #1\n");
_ncs = 0;
_spi.write(0xd4); // Buffer 1 write command
_spi.write(0x0); // address 0
_spi.write(0x0); // address 0
_spi.write(0x0); // address 0
_spi.write(0x0); // dont care byte
pin_out=1;
// read 8 bytes of data
for(int i = 0 ; i < 8 ; i++){
pc.printf("Data at %d is
%d\r\n",i,_spi.write(0));
}
_ncs = 1;
}
