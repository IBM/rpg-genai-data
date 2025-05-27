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
  | Parameter Name | Data Type | Description |
  |----------------|------|--------|
  | `pOrderId`     | `char(10)` | The unique identifier for the order being processed |

3. #### Return Values
List the return value accepted by the procedure, specifying whether it is an output
  Example:
  Data Type | Description |
  |------------|-------------|
  | `char(10)`      | The status code indicating success or failure of the procedure |

4. #### Inputs/Outputs  
Describe how the procedure reads from and writes to:
  - Data files (e.g., physical/logical files)
  - Device files (e.g., printers, displays)
  - Display files (for user interaction)

  Provide in table format with columns for file name, type (data/device/display), and description of how the procedure interacts with each file.
   Example:
   | File Name       | Type     | Used | Description |
   |-----------------|----------|------|-------------|
   | `CUSTOMER`      | Data     | Input  | Reads customer records for processing |
   | `ORDER_DISPLAY` | Display  | Input/Output | Displays order details to the user |
   | `PRINTER_FILE`  | Printer  | Output | Sends reports to the printer |

5. #### Side Effects 
Mention side effects which are not covered by the inputs and outputs, such as:
  - Data area updates
  - Example: The procedure updates a global data area to track the last processed order ID.

6. #### Limitations & Assumptions 
Mention any assumptions the code makes, or scenarios where it may fail or behave incorrectly.
  - Example: The caller cannot update records it is only input. This means the procedure  is designed to read data but not modify it.
      
7. #### Usage Example 
This should show how to call the procedure, including any necessary setup for parameters, along with an example or description of how the caller would handle the return value or any output parameters.

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

2. #### Global Components 
This section includes all global elements used in the procedure, such as variables, data structures, arrays, constants, and special keywords (e.g., LIKE, LIKEDS, CONST, etc.) used in declarations. It should be organized into the following subsections for clarity

  ##### Variables 
  This section documents all variables used in the procedure, including their types and purposes.

  Example: Variables
  | Variable Name | Type    | Description |
  |----------------|--------|-------------|
  | `CustomerID`   | 10 Length Character   | Unique identifier for the customer |
  | `OrderCount`   | 5 numeric no decimals | Total number of orders processed |

  - At end of this you can explain any specific keywords used in the variable declarations, such as `LIKE`, `INZ`, or `N` data type.

  ##### Data Structures
  This section documents all data structures used in the procedure, including their subfields and purposes.

  Provide the subfields in a table format with columns for subfield name, type, and description.
  Example: Data Structures
  Subfield Name | Type    | Description |
  |----------------|--------|-------------|
  | `OrderDetail`   | 10 Length Character   | Details of the order |
  | `OrderAmount`   | 7 numeric 2 decimals | Amount of the order |

  - At end of this you can explain any specific keywords used in the variable declarations, such as `LIKE`, `INZ`, or `N` data type.

  ##### Arrays 
  This section documents all arrays used in the procedure, including their dimensions and purposes.

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
  -  Indicators defined as normal variables (e.g., `DCL-S flag IND`) used within procedure logic but not tied to display/printer files.

  Example: Indicators
  | Indicator Name | Description | Purpose |
  |----------------|-------------|---------|
  | `*IN03`        | Display file input | Indicates if the user has pressed the Enter key on the display file |
  | `*IN04`        | Function key F4 | Indicates if the user has pressed the F4 key on the display file |

  ##### Function Keys 
  This section documents all function keys used in the procedure

  Example: Function Keys
  | Function Key Name | Description | Purpose |
  |-------------------|-------------|---------|
  | `F3`              | Exit        | Exits the procedure  |
  | `F4`              | Add         | Opens the add record screen |

  ##### Constants
  This section documents all constants used in the procedure, including their values and purposes.
  Example: Constants
  | Constant Name | Value | Description |
  |----------------|-------|-------------|
  | `MAX_ORDERS`   | 9999  | Maximum number of orders allowed |
  | `DEFAULT_CURRENCY` | 'USD' | Default currency for orders |

3. #### Main Execution Flow 
Describe the procedure logic in ordered steps. This includes a detailed explanation of each line, describing what it does and how it works

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