# Synchronous-FIFO-with-Write-and-Read-logic

FULL & EMPTY FLAG GENERATION<br/>
FIFO full and almost full flags are generated by Write Control Logic whereas empty and almost empty flags are generated by Read Control  Logic.
<br/>FIFO almost full and almost empty flags are generated to intimate thesource and the requestor about impending full or empty conditions. <br/>The almost full and almost empty levels are parameterized.
<br/>It is important to note that read and write pointers point to the same memory location at both full and empty conditions. 
<br/>Therefore, in order todifferentiate between the two one extra bit is added to read and write pointers. 
<br/>For example if a FIFO has depth of 256 then to span it completely8-bits will be needed. 
<br/>Therefore with one extra bit read and write pointers will be of 9-bits. When their lower 8-bits point to same memory location their MSBs are used to ascertain whether it is a full condition or emptycondition. 
<br/>**In empty conditions the MSBs are equal whereas in full condition MSBs are different.** 
<br/>The verilog code shown below depicts the same:
<br/>**assign fifo_full = ( (write_ptr[7 : 0] == read_addr[7 : 0]) &&(write_ptr[8] ^ read_ptr[8]) );**

<br/>Following piece of verilog code shows logic almost full generation:
<br/>// Generating fifo almost full status
```
always @*
 begin 
   if ( write_ptr[8] == read_ptr[8] )
    fifo_afull = ((write_ptr[7:0] - read_ptr[7:0]) >=(DEPTH - AFULL));
  else
    fifo_afull = ((read_ptr[8:0] - write_ptr[8:0]) <= AFULL);
 end
 ```
<br/>Here **AFUL**L signifies the almost full-level. User can set it to 4, 8 or whatever value depending on how soon it wants to intimate the source aboutimpending full condition. (AFULL=4)means almost full flag will getasserted when at most 4 locations are left for new data.

<br/>Following piece of verilog code shows logic almost empty generation:
<br/>//assigning fifo almost empty
```
always @*
 begin
  if (read_ptr[8] == write_ptr[8])
   fifo_aempty = ( (write_ptr[7:0] - read_ptr[7:0]) <= AEMPTY );
  else
   fifo_aempty = ((read_ptr[8:0] - write_addr[8:0])>= (DEPTH - AEMPTY));
 end
 ```
<br/>Here **AEMPTY** signifies the almost empty-level. User can set it to 4, 8 or whatever value depending on how soon it wants to intimate the requestor about impending empty condition. (AEMPTY=4) means almost empty flagwill get asserted when at most 4 data locations are left to be read.
