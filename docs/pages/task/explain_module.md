# Explanation format for an RPG module

The following explanation applies to RPG source files modules that will be bound into programs or service programs.
The [`scope`](/pages/metadata#scope) in `metadata.txt` will be `module` in this case.

### sum_output

The sum_output part offers a business summary in a couple of sentences, explaining the overall purpose of the module

### api_output

The `api_output` part provides information to the caller about how to call the exported procedures in this module
and what the effects of calling it would be.
It includes a high-level summary of the module's purpose and then for each exported procedure, an api-level description of it.  Note that for modules the caller will not care about any side effects within the module, but only on the external objects such as databases.

`Important`: If a module exports `multiple procedures`, each procedure must be documented `separately` using the standard format below.

1. #### Purpose
Provide a short, high-level summary describing the overall purpose of the `module`.
This summary should focus on what the module is designed to achieve functionally, without going into the individual procedure details.

2. #### Exported Procedures
Each exported procedure should be documented using the following structure:

#### Procedure: `<procedure_name>`

  ##### Description
  A clear and concise explanation of what the procedure does.

  ##### Parameters
  List all parameters with their directions and brief explanations:

  Example:
  | Parameter Name | Data Type | Description |
  |----------------|------|--------|
  | `pOrderId`     | `char(10)` | The unique identifier for the order being processed |

  ##### Return Value
  List the return value with its direction and brief explanation:

  Example:
  Data Type | Description |
  |------------|-------------|
  | `char(10)`      | The status code indicating success or failure of the procedure |

  #### Inputs/Outputs 
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

  #### Dependencies 
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

  ##### Limitations & Assumptions
    List any known assumptions or restrictions:
    -  Maximum limits
    -  Required preconditions
    -  Unsupported input types

  ##### Usage Example:
  Provide a code snippet that demonstrates how to call the procedure or program, including parameter setup and the call itself. This should be a complete and realistic example that users can follow as a reference.

  This section should illustrate all relevant aspects of calling the procedure, including:

  1. The `/COPY` Statement
      - Show how the caller includes the prototype using a `/COPY` directive (if applicable).

  2. Parameter Setup
    - Define and initialize all input and output parameters required for the call.
    - Include declarations for any necessary constants, variables, or data structures.

  3. The Procedure Call
    - Demonstrate the call using:
      - `CALLP` if using a procedure with a prototype.
    - If the procedure returns a value, show how the caller captures and uses that return value.
    - If parameters are modified, show how the caller accesses the updated values.

  4. When No Follow-Up Coding is Required
    - If the procedure performs an action with side effects (e.g., writing to a file, logging), and there is no need for the caller to process return values or modified parameters, the example may omit further usage.
    - In such cases, document only the call and any required setup.

  example:
  ```rpgle
      /COPY QRPGLESRC,MYPROTOTYPE

      DCL-S customerId   CHAR(10)   INZ('CUST001');
      DCL-S orderTotal   PACKED(9:2);
      DCL-S statusCode   CHAR(2);

      CALLP CalculateOrderTotal(customerId : orderTotal : statusCode);

      // After call, orderTotal and statusCode will have updated values
  ```

Repeat the above `procedure section` for `each additional exported procedure` in the module.

### how_output

The how_output part explains how the module works internally, covering the full execution flow and logic used at each step. It includes details on control specifications, file declarations, global variables, constants, indicators, field mapping, subroutines, error handling, and the logic of each procedure.

`Important`: Each procedure must be documented separately.
1. #### Purpose
Provide a short, high-level summary describing the overall purpose of the `module`.

2. #### Control Specifications 
Any key compiler options and runtime control settings included in the module. Examples include `DftActGrp`, `ActGrp`, `BNDDIR`, and other relevant control specifications. Explain these settings and any keywords used.

3. #### File Declarations
List all declared files from the F-specs with relevant keywords. This includes display files, physical,logical files, printer files, and special files. Explain how each file is declared and any keywords used.

  Provide in table format with columns for file name, type (data/device/display), and description of how the program interacts with each file.
  Example:
  | File Name       | Type     | Used | Description |
  |-----------------|----------|------|-------------|
  | `CUSTOMER`      | Data     | Input  | Reads customer records for processing |
  | `ORDER_DISPLAY` | Display  | Input/Output | Displays order details to the user |
  | `PRINTER_FILE`  | Printer  | Output | Sends reports to the printer |

3. #### Global Components
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

5. #### Procedures 
Provide a step-by-step description of the logic for each procedure.

  - `Definitions`: This section covers all declarations and setup required before the main logic of the procedure begins. It includes the definition of local variables, data structures, and any constants or initial values needed to support the procedure’s execution
  - `Main Logic`: Explain the core logic of the procedure, detailing the steps taken to achieve the procedure's purpose.
  - `Possible Problems with this code`: Identify potential issues that could arise during the execution of the procedure.
  - `Possible improvements to this code`: Identify areas where the code could be enhanced for better performance, readability, or functionality. This may include suggestions for error handling, optimization techniques, or alternative approaches to achieve the same result.

  `Important`
    - A module has multiple procedures with extensive lines of code, a brief outline of each procedure would be sufficient without any code snippets. However, if a module has only one or two procedures, it would be beneficial to provide a detailed explanation of each line.
    - Repeat the above `procedure section` for `each additional procedure` in the module, ensuring that any reusable components or variables are mentioned within the context of each procedure.

## sum_output

The sum_output part offers a business summary in a couple of sentences, explaining the overall purpose of the module.

## metadata.txt

The `metadata.txt` file describes important attributes of the training data.  Its structure looks like:

```yaml
difficulty: <rating>
language: <language>
scope: <scope>
use: <usage>
```

Full description of all the metadata variants can be found [here](/pages/metadata.md).

The sample explanation module can be found [here](/pages/task/sample_module.md).