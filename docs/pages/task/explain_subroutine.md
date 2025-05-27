# Explain subset of RPG source file

To simulate selection subroutine from a source file, the following syntax is used to avoid duplicating the entire program source and context.
This corresponds to the metatdata.txt [`scope`](/pages/metadata.txt###scope) of  `subr`.

The directory structure for subroutine look like:

- procedure\
  - output
    - sum_output.md
    - api_output.md
    - how_output.md
  - metadata.txt

Note: Input and Context folder are not required for subroutine.  This is because the `source`, `start` and `end` attributes will define exactly which source and context is referenced.

## Output

The Output Section is designed to provide a comprehensive explanation of the RPGLE procedure oe subroutine behavior, structure, and logic. It is divided into three main parts: `api_output`, `how_output`, and `sum_output`. Each part serves a specific purpose and includes detailed information to ensure clarity and consistency.

## api_output

The `api_output` part provides a high-level summary of the subroutine purpose and behavior, detailing the parameters passed to and from the procedure, expected inputs and outputs, dependencies, side effects or limitations, and optional usage examples.

1. #### Purpose
Provide a short insight into the purpose of the code for the subroutine. This should focus on the business logic or functional role of the code.

2. #### Variable Input/Output 
List all input and output variables used by the code, specifying whether each one is an input, output, or both.
    Example:
    | Parameter Name | Direction | Type  | Description |
    |----------------|-----------|-------|-------------|
    | `pOrderId`     | Input     | `char(10)` | The unique identifier for the order being processed |
    | `pStatus`      | Output    | `char(10)` | The status code indicating success or failure of the procedure |

3. #### File Inputs/Outputs 
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
  - Example: The caller cannot update records it is only input. This means the subroutine is designed to read data but not modify it.
      
6. #### Usage Example
Provide a code snippet showing how the subroutine is called within the program. 
```rpgle
     D InputVar        S             10A   INZ('Test')
     D Indicator       S              N     INZ(*OFF)

     // Call the subroutine
     C                   EXSR      MySubroutine

     // After call, Indicator or InputVar might be updated
```

## how_output
The `how_output` section explains how the specific subroutine works internally, covering the full execution flow and logic used at each step. It includes details on variables, constants, indicators, field mappings, subroutine calls, and error handling related to that unit.

1. #### Purpose
Provide a short insight into the purpose of the code for the subroutine. This should focus on the business logic or functional role of the code.

2. #### Global Components 
This section lists the global elements used by the subroutine, such as indicators.

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

3. #### Main Execution Flow
Describe the subroutine logic in ordered steps. This includes a detailed explanation of each line, describing what it does and how it works

4. #### Possible Problems with this Code
Identify potential issues that could arise during the execution of the subroutine. Mention any problems that should be noted when discussing the final statement that ends the subroutine.

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