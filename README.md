# sdcc-msx
Fork of sdcc that provides msx specific extensions (mz80)

* Added page command line options
    ```
    -bo  <num>             use code page <num>
    -ba  <num>             use data page <num>
    ```
* Added pragmas CODE_PAGE and DATA_PAGE to select code and data pages inside a module
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
       -b _CODE_PAGE_1=0x1A000 -b _CODE_PAGE_2=0x2A000 ... -B _DATA_PAGE_3=0x38000 ... etc
       ```
   * The linker will resolve banked calls based on the segment each symbol belongs to based on bo, ba and pragma bank

   Note that as bank number comes from the high byte of the 24bit address of the page, is only possible
   to use one type of page (either CODE or DATA) per 0x10000 address range, even if they are not overlapping.

   * __sdcc_banked_call must be implemented in an non-banked area (HOME or CODE) and is up to the user

## todo

* Add new sub ports instead of using mz80
	* msx
	* msxtR
* Add support for R800 MUL* instructions
* Add some reference implementation of __sdcc_banked_call in a device library
* Add makebin version to extract pages from ihx and assemble a functioning ROM
