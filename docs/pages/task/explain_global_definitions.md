#### Global Definitions
This section includes all global elements used in the module, such as variables, data structures, constants. It should be organized into the following subsections for clarity

See [How to explain data types](/pages/explain_definitions.md) for detailed instructions for the Type and possible Attributes columns for variable, subfields, and parameters.

##### Variables 
This section documents all variables used in the program, module, or procedure. It should be described in tabular format with columns for Name, Data Type, Description, and Attributes.
Only include the Attributes column if necessary.

Example: Variables
| Name           | Data Type               | Description                                 | Attributes         |
|----------------|------------------------ |---------------------------------------------|--------------------|
| `customerId`   | char(10)                | The unique identifier for the customer      |                    |
| `monthlyOrders`| int(10), 4 bytes       | Array of the number of orders for each month| DIM(12)            |

##### Data Structures

This section documents all data structures used in the module, program or procedure, including their purposes, and any notable characteristics such as whether the structure is a `Program Status Data Structure`, or a `qualified data structure`. The subfields of each data structure should be described in a table format with columns for `Subfield Name`, `Type`, `Description`, and possibly `Attributes`. 

Qualified data structures should have the subfields listed in the fully-qualified form. For example, if qualified data structure `DS` has a subfield `SUBF1`, then the subfield should be listed as `ds.subf1`.

See [How to explain data structures](/pages/explain_data_structure.md) for examples of data structure explanations.

##### Constants
This section documents all constants used in the module, including their values and purposes.
Example: 
| Constant Name       | Value | Description                              |
|---------------------|-------|------------------------------------------|
| `MAX_ORDERS`        | 9999  | Maximum number of orders allowed         |
| `DEFAULT_CURRENCY`  | 'USD' | Default currency for orders              |

##### Data structure subfields without explicit data types

In some cases, subfields in a data structure do not have an explicit data type specified. These cases are handled as follows:

1. **Externally described subfields:**  
   If a subfield does not have a data type and is not the target of any `OVERLAY`, its definition comes from an externally described file (such as a display or database file). In this case, indicate that the data type depends on the external definition, for example:  
   `Depends on external definition (e.g., AGR11002FM.DSPF)`

   **Example:**
   ```rpgle
   // From display file AGR11002FM.DSPF
   A          R SCR01S
   A                                      SFL
   A            PGMSEG         1   H
   A            SRVLVL         4   H
   A            PRCDATE        6Y 0O  4  3EDTCDE(Y)
   A            PRICE          9Y 4O  4 13EDTCDE(2 $)

   // From RPGLE code
   DPriceDS          DS                  Inz
   DPPrice                         10
   D Price                               Overlay(PPrice:1)
   D PGMSeg                              Overlay(PPrice:*Next)
   ```
   | Name     | Data Type            | Description                                                      | Attributes             |
   |----------|----------------------|------------------------------------------------------------------|------------------------|
   | `PPrice` | char(10)             | Combined field storing price and program code                    |                        |
   | `Price`  | packed(6:0), 4 bytes | Holds the value of price. The definition comes from externally-described display file `AGR11002FM.DSPF` | Overlay(PPrice:1)      |
   | `PGMSeg` | packed(9:0), 5 bytes | Stores the program segment code. The definition comes from externally-described display file `AGR11002FM.DSPF` | Overlay(PPrice:*Next)  |

2. **Overlay subfields:**  
   If a subfield is the target of one or more `OVERLAY` subfields, treat the untyped subfield as a character field (`char(n)`), where the length is determined by the combined length of the overlaying subfields.

   **Example:**
   ```rpgle
   DWorkDS           DS
   D INPrnt
   D CheckCode                      2S 0 Overlay(INPrnt)
   D otherSubf                      5P 3 Overlay(INPrnt:*NEXT)
   ```
   | Name        | Data Type                       | Description                         | Attributes        |
   |-------------|---------------------------------|-------------------------------------|-------------------|
   | `INPrnt`    | char(5), derived from overlays  | Input value for deriving check code |                   |
   | `CheckCode` | zoned(2:0)                      | Determines item type                | Overlay(INPrnt)   |
   | `otherSubf` | packed(5:3), 3 bytes            | Example overlay field               | Overlay(INPrnt)   |

3. **Special system data structures (PSDS/INFDS):**  
   For special subfields in Program Status Data Structures (PSDS) or File Information Data Structures (INFDS), the data type is determined by the compiler and documented in the IBM manuals.

   **INFDS Example:**
   ```rpgle
   FInvMast   IF   E           K Disk    UsrOpn Infds(infds2)
   DINFDS2           DS
   D  STATUS           *STATUS
   ```
   | Name     | Data Type    | Description                        | Attributes |
   |----------|--------------|------------------------------------|------------|
   | `STATUS` | zoned(5:0)   | File status code (INFDS subfield)  | *STATUS    |

   **PSDS Example:**
   ```rpgle
   D                SDS
   D @PgmName          *PROC
   // or in free-form
   dcl-ds *n psds;
      @PGMName *PROC;
   end-ds;
   ```
   | Name      | Data Type  | Description                      | Attributes |
   |-----------|------------|----------------------------------|------------|
   | `@PGMName`| char(10)   | Program name (PSDS subfield)     | *PROC      |

If the external definition cannot be resolved, indicate this in the documentation, for example:  
`Depends on external definition (field FOO)`

Refer to IBM documentation for details on PSDS and INFDS subfield types:
- [File Feedback Information (INFDS)](https://www.ibm.com/docs/en/i/7.6.0?topic=structure-file-feedback-information#flfeed)
- [Program Status Data Structure (PSDS)](https://www.ibm.com/docs/en/i/7.6.0?topic=exceptionerrors-program-status-data-structure#psdsdt9)