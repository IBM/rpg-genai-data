# Explain subset of RPG source file

To simulate selection procedure from a source file, the following syntax is used to avoid duplicating the entire procedure source and context.
This corresponds to the metatdata.txt [`scope`](/pages/metadata.txt###scope) of  `proc`.

The directory structure for procedure look like:

- procedure\
  - output
    - sum_output.md
    - api_output.md
    - how_output.md
  - metadata.txt

Note: Input and Context folder are not required for procedure . This is because the `source`, `start` and `end` attributes will define exactly which source and context is referenced.

## Output

The Output Section is designed to provide a comprehensive explanation of the RPGLE procedure  behavior, structure, and logic. It is divided into three main parts: `api_output`, `how_output`, and `sum_output`. Each part serves a specific purpose and includes detailed information to ensure clarity and consistency.

## api_output

The `api_output` part provides a high-level summary of the procedure  purpose and behavior, detailing the parameters passed to and from the procedure, expected inputs and outputs, dependencies, side effects or limitations, and optional usage examples.

1. #### Purpose
Provide a short insight into the purpose of the code for the procedure. 

2. #### Parameters
List all entry parameters accepted by the code, specifying whether each one is an input.
  Example:
  | Parameter Name | Data Type           |Usage      | Description                                                  |
  |----------------|---------------------|------------|--------------------------------------------------------------|
  | `pOrderId`     | Character, length 10 | Input only | The unique identifier for the order being processed          |

3. #### Return Values
List the return value accepted by the procedure, specifying whether it is an output
  Example:
  | Data Type           | Description                                                             |
  |---------------------|-------------------------------------------------------------------------|
  | Character, length 10 | The status code indicating success or failure of the procedure          |

4. #### Inputs/Outputs 
  
##### Variable
Describe varables used in the procedure, including their types and purposes. This includes any global variables are using `EXPORT` or `IMPORT` keywords.
Example: 
| Variable Name | Type                          | Description                                              |
|----------------|-------------------------------|----------------------------------------------------------|
| `CustomerID`   | Character, length 10           | Unique identifier for the customer                       |
| `OrderCount`   | Numeric, length 5, with 0 decimals | Total number of orders processed (no decimals)           |

##### Files
Describe how the program reads from and writes to:
- Data files (e.g., physical/logical files)
- Device files (e.g., printers, displays)
- Display files (for user interaction)

Provide in table format with columns for file name, type (data/device/display), and description of how the program interacts with each file.
  Example:
| File Name       | Type     | Used          | Description                                      |
|-----------------|----------|---------------|--------------------------------------------------|
| `CUSTOMER`      | Data     | Input         | Reads customer records for processing            |
| `ORDER_DISPLAY` | Display  | Input/Output  | Displays order details to the user               |
| `PRINTER_FILE`  | Printer  | Output        | Sends reports to the printer                     |
| `ORDERS`        | Data     | Input/Update  | Reads and updates order information              |

5. #### Dependencies 
 
##### Programs and Services
These are external programs or API that the module calls or interacts with to perform specific tasks.
Example:
| Component Name        | Type              | Description                                                                 |
|-----------------------|-------------------|-----------------------------------------------------------------------------|
| `LIB1/ORDER_API`      | API               | Offers a standardized interface to access and manage order-related data.    |
| `LIB1/VALIDATION_PGM` | Called Program    | Validates input fields such as customer ID, order quantity, and product codes before processing. |

##### Data and Messaging Components
These include data areas, data queues, and other that store or transmit data used by the program.
Example:
| Component Name        | Type       | Description                                                                 |
|-----------------------|------------|-----------------------------------------------------------------------------|
| `LIB1/DATA_AREA_1`    | Data Area  | Stores runtime configuration values such as environment flags or thresholds. |
| `LIB1/MSG_QUEUE_1`    | Data Queue | Used to send and receive asynchronous messages between batch and interactive jobs. |
| `LIB1/CONFIG_SYS`     | Data Area  | Holds system-wide settings like default language, currency, or region.      |
| `*LIBL/SALESERRS`     | Message file  | Used to retrieve error and validation messages. Assigned to `@MSGFILE`.     |

6. #### Side Effects 
Mention side effects which are not covered by the inputs and outputs, such as:
  - Data area updates
  - Global variable modifications
  - Example: The procedure updates a global data area to track the last processed order ID.
  - Example: The procedure modifies a global variable to store the last processed order count.

7. #### Limitations & Assumptions 
Mention any assumptions the code makes, or scenarios where it may fail or behave incorrectly.
  - Example: The caller cannot update records it is only input. This means the procedure  is designed to read data but not modify it.
      
8. #### Usage Example 
This should show how to call the procedure, including any necessary setup for parameters, along with an example or description of how the caller would handle the return value or any output parameters.

 1. The `/COPY` Statement
   - Show how the caller includes the prototype using a `/COPY` directive. 
   - This is always applicable for RPGLE examples with `scope=module`, as the caller is expected to reside in a different module.

  2. Parameter Setup
    - Define and initialize all input, output parameters and any return values required for the call.
    - Include declarations for any necessary constants, variables, or data structures.

  3. The Procedure Call
    - If the procedure returns a value, show how the caller captures and uses that return value.
    - If parameters are modified, show how the caller accesses the updated values.

  4. When No Follow-Up Coding is Required
    - If the procedure performs an action with side effects (e.g., writing to a file, logging), and there is no need for the caller to process return values or modified parameters, the example may omit further usage.
    - In such cases, document only the call and any required setup.
    
```rpgle
       /COPY QRPGLESRC,MYPROTOTYPE

       DCL-S inputValue   PACKED(5:0) INZ(10);
       DCL-S resultValue  PACKED(7:2);

       // Call the procedure and capture the return value
       resultValue = CalculateTax(inputValue);

       // The resultValue now holds the tax amount returned by the procedure
```

## how_output
The `how_output` section explains how the specific procedure  works internally, covering the full execution flow and logic used at each step. It includes details on variables, constants, indicators, field mapping calls, and error handling related to that unit.

1. #### Purpose
Provide a short insight into the purpose of the code for the procedure. 

3. #### Global Components
This section includes all global elements used in the program, such as variables, data structures, arrays, constants, and special keywords (e.g., LIKE, LIKEDS, CONST, etc.) used in declarations. It should be organized into the following subsections for clarity

  ##### Variables 
  This section documents all variables used in the program, including their types and purposes.

  Example: Variables
  | Variable Name | Type                          | Description                                              |
  |----------------|-------------------------------|----------------------------------------------------------|
  | `CustomerID`   | Character, length 10           | Unique identifier for the customer                       |
  | `OrderCount`   | Numeric, length 5, with 0 decimals | Total number of orders processed (no decimals)           |

  - At end of this you can explain any specific keywords used in the variable declarations, such as `LIKE`, `INZ`, or `N` data type.

  ##### Data Structures

  This section documents all data structures used in the program, including their subfields, purposes, and any notable characteristics such as whether the structure is `qualified`, an `array`, or has other special attributes.

  Each data structure is presented with a table of its subfields, including:

  Example: `OrderHeader` 
  | Subfield Name | Type                                 | Description                                                  |
  |----------------|--------------------------------------|--------------------------------------------------------------|
  | `OrderID`      | Character, length 10                 | Unique identifier for the order                              |
  | `OrderDate`    | Date, *ISO format                    | Date when the order was placed                               |
  | `TotalAmount`  | Packed numeric, length 9, 2 decimals | Total amount for the order                                   |
  | `Quantities`   | Numeric array, length 5, dimension 50| Quantity of each item in the order                           |

  - At end of this you can explain any specific keywords used in the data structure declarations, such as `LIKEDS`, `QUALIFIED`, or `INZ`.

  ##### Arrays 
  This section documents all arrays used in the program, including their dimensions and purposes.
  | Array Name     | Data Type           | Dimensions & Description                          |
  |----------------|---------------------|---------------------------------------------------|
  | `MeetingDates` | Date, *ISO format   | Array with 10 elements; stores available meeting dates |

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
  | Indicator Name | Description         | Purpose                                                                 |
  |----------------|---------------------|-------------------------------------------------------------------------|
  | `*IN03`        | Display file input  | Indicates if the user has pressed the Enter key on the display file     |
  | `*IN04`        | Function key F4     | Indicates if the user has pressed the F4 key on the display file        |

  ##### Function Keys 
  This section documents all function keys used in the program

  Example: Function Keys
  | Function Key Name | Description | Purpose                              |
  |-------------------|-------------|--------------------------------------|
  | `F3`              | Exit        | Exits the program                    |
  | `F4`              | Add         | Opens the add record screen          |

  ##### Constants
  This section documents all constants used in the program, including their values and purposes.
  Example: Constants
  | Constant Name       | Value | Description                              |
  |---------------------|-------|------------------------------------------|
  | `MAX_ORDERS`        | 9999  | Maximum number of orders allowed         |
  | `DEFAULT_CURRENCY`  | 'USD' | Default currency for orders              |

3. #### Main Execution Flow 
This section explains the core logic of the procedure, outlining the key steps taken to fulfill its purpose. It should also include a list of any subroutines or internal procedures used, along with a brief description of each subroutine.

4. #### Possible Problems with this Code
Identify potential issues that could arise during the execution of the procedure. Mention any problems that should be noted when discussing the final statement that ends the procedure .

5. #### Possible Improvements to this Code
Identify areas where the code could be enhanced for better performance, readability, or functionality. This may include suggestions for error handling, optimization techniques, or alternative approaches to achieve the same result.

## sum_output

The sum_output part offers a business summary in a couple of sentences, explaining the overall purpose of the procedure

## metadata.txt

The `metadata.txt` file describes important attributes of the training data.

Structure look like:

```yaml
source: <source_member_name>
start: <start_line_number>
end: <end_line_number>
difficulty: <rating>
language: <language>
scope: <scope>
use: <usage>
```

Full description of all the metadata variants can be found [here](/pages/metadata.md).

The sample explanation source reference can be found [here](/pages/task/sample_source_references.md).