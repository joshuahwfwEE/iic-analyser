# iic-analyser
the iic analyser can capture 2 wire sda and scl signal between iic communication and interpret the bus  

xlinx axi iic:  

Initialization
    Set the RX_FIFO depth to maximum by setting RX_FIFO_PIRQ = 0x _ _
    Reset the TX_FIFO with 0x_ _  
    Enable the AXI IIC, remove the TX_FIFO reset, and disable the general call.  
    
Read Request Sequence:    
Read Bytes from an IIC Slave Device Addressed as 0x_ _  

First, a write access is necessary to set the slave device address, then a repeated start follows with the read accesses:  

    Check that all FIFOs are empty and that the bus is not busy by reading the Status register.  
    Write 0x_ _ _ to the TX_FIFO (set start bit, device address to 0x__, write access).  
    Write 0x__ to the TX_FIFO (slave address for data).  
    Write 0x___ to the TX_FIFO (set start bit for repeated start, device address 0x_ _, read access).  
    Write 0x___ to the TX_FIFO (set stop bit, four bytes to be received by the AXI IIC).  
    Wait for RX_FIFO not empty.  
  
  
a) Read the RX_FIFO byte.  
b) If the last byte is read, exit; otherwise, continue checking RX_FIFO not empty.  
  
   
Write Request sequence:    
Write Bytes to an IIC Slave Device Addressed as 0x_ _  
  
Place the data at slave device address 0x__:  
    Check that all FIFOs are empty and that the bus is not busy by reading the SR.  
    Write 0x___ to the TX_FIFO (set the start bit, the device address, write access).  
    Write 0x__ to the TX_FIFO (slave address for data).  
    Write 0x__ to the TX_FIFO (byte 1).  
    Write 0x__ to the TX_FIFO (byte 2).  
    Write 0x__ to the TX_FIFO (stop bit, byte x).  





