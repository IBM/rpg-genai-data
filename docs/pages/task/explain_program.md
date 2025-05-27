# Explanation format for an RPG program

The following explanation applies to both modern linear-main programs as well as legacy cycle-main programs.
The [`scope`](/pages/metadata#scope) in `metadata.txt` will be `program-linear` or `program-cycle` in this case.

### api_output

The `api_output` part provides information to the caller of a program about how to call the program and what the effects of calling it would be. It includes
a high-level summary of the program's purpose and behavior, detailing the parameters passed to and from the program, dependencies, side effects or limitations, and optional usage examples.

1. #### Purpose 
Provide a short insight into the purpose of the code for the program. This should focus on the business logic or functional role of the code.

2. #### Parameters
List all entry parameters accepted by the program, specifying whether each one is an input, output, or both. Provide in table format with columns for parameter name, direction (input/output), and description. 

  Example:
  | Parameter Name | Direction | Type  | Description |
  |----------------|-----------|-------|------|
  | `CustomerID`   | Input     | 10 Length Character | Unique identifier for the customer |
  | `OrderCount`   | Output    | 5 numeric no decimals  | Total number of orders processed |

3. #### Inputs/Outputs 
Describe how the program reads from and writes to:
  - Data files (e.g., physical/logical files)
  - Device files (e.g., printers, displays)
  - Display files (for user interaction)

  Provide in table format with columns for file name, type (data/device/display), and description of how the program interacts with each file.
  Example:
  | File Name       | Type     | Used | Description |
  |-----------------|----------|------|-------------|
  | `CUSTOMER`      | Data     | Input  | Reads customer records for processing |
  | `ORDER_DISPLAY` | Display  | Input/Output | Displays order details to the user |
  | `PRINTER_FILE`  | Printer  | Output | Sends reports to the printer |

4. #### Dependencies 
List all external components the program relies on to function correctly. This includes:
  - Called programs
  - Service programs (SRVPGMs)
  - APIs or external libraries
  - Data areas
  - Data queues
  - Any other external modules or systems
   
  Provide in table format with columns for component name, type (program/service/API/data area), and description of its role.
  Example:
  | Component Name  | Type     | Description |
  |-----------------|----------|-------------|
  | `CALC_SERVICE`   | Service  | Performs complex calculations for order processing |
  | `DATA_AREA_1`    | Data Area| Stores configuration settings for the program |
  | `ORDER_API`      | API      | Provides access to order management functions |

5. #### Limitations & Assumptions 
Mention any assumptions the code makes, or scenarios where it may fail or behave incorrectly.
  - Example: The load all subfile can load only up to 9999 records.

6. #### Usage Example: 
Provide a code snippet that demonstrates how to call the program. 

  1. Parameter Setup
    - Define and initialize all input/output parameters required for the call.
    - Include any relevant data structures, constants, or variables.

  2. The Program Call
    - Use `CALL` to invoke a program object.
    - Ensure proper handling of parameters (if applicable) according to how the target program expects them.
  Example:
  ```clle
    CALL  AGR02001
  ```

### how_output

The how_output part explains how the program works internally, covering the full execution flow and logic used at each step. It includes details on control specifications, file declarations, global variables, constants, indicators, field mapping, subroutines, error handling, and the main program logic.

1. #### Purpose
Provide a short insight into the purpose of the code for the program. This should focus on the business logic or functional role of the code.

2. #### Control Specifications 
Any key compiler options and runtime control settings included in the program. Examples include `DftActGrp`, `ActGrp`, `BNDDIR`, and other relevant control specifications. Explain these settings and any keywords used.

3. #### File Declarations 
List all global files from the F-specs with relevant keywords. This includes display files, physical,logical files, printer files, and special files. Explain how each file is declared and any keywords used.

provide in table format with columns for file name, type (data/display/printer), and description of how the program interacts with each file.
Example:
| File Name       | Type     | Used | Description |
|-----------------|----------|------|-------------|
| `CUSTOMER`      | Data     | Input  | Reads customer records for processing |
| `ORDER_DISPLAY` | Display  | Input/Output | Displays order details to the user |
| `PRINTER_FILE`  | Printer  | Output | Sends reports to the printer |

4. #### Global Components
This section includes all global elements used in the program, such as variables, data structures, arrays, constants, and special keywords (e.g., LIKE, LIKEDS, CONST, etc.) used in declarations. It should be organized into the following subsections for clarity

  ##### Variables 
  This section documents all variables used in the program, including their types and purposes.

  Example: Variables
  | Variable Name | Type    | Description |
  |----------------|--------|-------------|
  | `CustomerID`   | 10 Length Character   | Unique identifier for the customer |
  | `OrderCount`   | 5 numeric no decimals | Total number of orders processed |

  - At end of this you can explain any specific keywords used in the variable declarations, such as `LIKE`, `INZ`, or `N` data type.

  ##### Data Structures
  This section documents all data structures used in the program, including their subfields and purposes.

  Provide the subfields in a table format with columns for subfield name, type, and description.
  Example: Data Structures
  Subfield Name | Type    | Description |
  |----------------|--------|-------------|
  | `OrderDetail`   | 10 Length Character   | Details of the order |
  | `OrderAmount`   | 7 numeric 2 decimals | Amount of the order |

  - At end of this you can explain any specific keywords used in the variable declarations, such as `LIKE`, `INZ`, or `N` data type.

  ##### Arrays 
  This section documents all arrays used in the program, including their dimensions and purposes.

  Example: Arrays
  | Array Name | Dimensions | Description |
  |-------------|------------|-------------|
  | `OrderArray` | 1000 elements | Array to hold order details |

  - At end of this you can explain any specific keywords used in the array declarations, such as `DIM`, `LIKE`, or `INZ`.

  ##### Indicators
  This section documents all indicators used for display/printer control, based on system conventions.

  `Include:`
  -  `*INxx` Indicators: Standard RPG indicators (e.g., `*IN03`) or fixed-format numeric indicators controlling logic or UI.
  -  Indicators in `INDDS` Structures: Subfields defined inside indicator data structures linked to files using `INDARA`. These control function keys, display flags, etc.
  -  Indicators in Externally Described DS (without `INDARA`): Subfields like `IN01`, `IN02` representing indicators tied to display/printer files.

  `Exclude:`
  -  Indicators defined as normal variables (e.g., `DCL-S flag IND`) used within program logic but not tied to display/printer files.

  Example: Indicators
  | Indicator Name | Description | Purpose |
  |----------------|-------------|---------|
  | `*IN03`        | Display file input | Indicates if the user has pressed the Enter key on the display file |
  | `*IN04`        | Function key F4 | Indicates if the user has pressed the F4 key on the display file |

  ##### Function Keys 
  This section documents all function keys used in the program

  Example: Function Keys
  | Function Key Name | Description | Purpose |
  |-------------------|-------------|---------|
  | `F3`              | Exit        | Exits the program  |
  | `F4`              | Add         | Opens the add record screen |

  ##### Constants
  This section documents all constants used in the program, including their values and purposes.
  Example: Constants
  | Constant Name | Value | Description |
  |----------------|-------|-------------|
  | `MAX_ORDERS`   | 9999  | Maximum number of orders allowed |
  | `DEFAULT_CURRENCY` | 'USD' | Default currency for orders |

5. #### Main Execution Flow 
Describe program logic in ordered steps. This includes initialization, opening files, loading initial data, displaying subfile, handling user actions (Add/Edit/Delete/Search), validating and updating database, printing report (if required), and exiting the program. Provide code snippets for better readability.

  ##### Subroutines 
  If the program contains subroutines, list and explain each subroutine in detail.

6. #### Possible Problems with this code
Identify potential issues that could arise during the execution of the program. Mention any problems that should be noted when discussing the final statement that ends the program.

7. #### Possible improvements to this code 
Identify areas where the code could be enhanced for better performance, readability, or functionality. This may include suggestions for error handling, optimization techniques, or alternative approaches to achieve the same result.

### sum_output

The sum_output part offers a business summary in a couple of sentences, explaining the overall purpose of the program.

## metadata.txt

The `metadata.txt` file describes important attributes of the training data.  Its structure looks like:

```yaml
difficulty: <rating>
language: <language>
scope: <scope>
use: <usage>
```

Full description of all the metadata variants can be found [here](/pages/metadata.md).

The sample explanation program can be found [here](/pages/task/sample_program.md).