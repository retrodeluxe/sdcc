# sdcc-msx
Fork of sdcc that provides msx specific extensions (mz80)

* Added bank command line options
   
    ```
    -bo  <num>             use code bank <num>
    -ba  <num>             use data bank <num>
    ```
* Added pragma "bank" to select code bank
* Added support for "--model-large"
   * This will make functions banked unless otherwise specified with __nonbanked
   * Every banked call is replaced with 
   
       ```  
       call __sdcc_banked_call
       dw function_addr (LSB, MSB)
       ds bank (HIB)
       ```
       
   * Base bank 24bit addresses need to be provided to the linker
   
       ```
       -b _CODE_1=0x1A000 -b _CODE_2=0x2A000 ... -B _DATA_1=0x1800 ... etc
       ```
   * The linker will resolve banked calls based on the segment each symbol belongs to based on bo, ba and pragma bank
   * __sdcc_banked_call must be implemented in an non-banked area (HOME or CODE) and is up to the user
