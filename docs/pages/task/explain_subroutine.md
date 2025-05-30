# Explain subset of RPG source file

To simulate selection subroutine from a source file, the following syntax is used to avoid duplicating the entire program source and context.
This corresponds to the metatdata.txt [`scope`](/pages/metadata.txt###scope) of  `subr`.

# Imporant:

- All the free-form snippets must be indented by at least 7 spaces before the code.
- All fixed-form snippets must have exactly 5 spaces before the specification type.
- If the rpgle to start the snippet is indented, the 5 or 7 spaces must start at the same indentation as the rpgle line.

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

The `api_output` part provides a high-level summary of the subroutine purpose and behavior, detailing the parameters passed to and from the procedure, expected inputs and outputs, dependencies, limitations, and optional usage examples.

1. #### Purpose
Provide a short insight into the purpose of the code for the subroutine. This should focus on the business logic or functional role of the code.

2. #### Variable I/O
List all input and output variables used by the code, specifying whether each one is an input, output, or both.
Example:  
| Name           | Data Type            | Usage        | Description                                 | Attributes |
|----------------|--------------------- |-------------|---------------------------------------------|------------|
| `customerId`   | char(10)             | Input       | The unique identifier for the customer      |            |
| `orderTotal`   | packed(9:2), 5 bytes | Output      | The total amount of the order calculated    | INZ(0)     |
| `currencyCode` | char(3)              | Input       | The currency in which the order is placed   | CONST      |

3. #### File I/O 
Describe how the subroutine reads from and writes to:
  - Data files (e.g., physical/logical files)
  - Device files (e.g., printers, displays)
  - Display files (for user interaction)

  Provide in table format with columns for file name, type (data/device/display), and description of how the subroutine interacts with each file.
  Example:
  | File Name       | Type          | Usage         | Description                                  |
  |-----------------|---------------|---------------|----------------------------------------------|
  | `CUSTOMER`      | Data file     | Input         | Reads customer records for processing        |
  | `ORDER_DISPLAY` | Display file  | Input/Output  | Displays order details to the user           |
  | `PRINTER_FILE`  | Printer file  | Output        | Sends reports to the printer                 |
  | `ORDERS`        | Data file     | Input/Update  | Reads and updates order information          |

4. #### Dependencies 
This section lists all external dependencies required by the program 
- Include: Only programs, service programs, data areas, data queues, and message files that the program calls or interacts with directly.
- Exclude: System APIs and any files already described in the File I/O section.

Example: 
| Object Name           | Object Type        | Description                                                                 |
|-----------------------|-------------------|-----------------------------------------------------------------------------|
| `LIB1/VALIDATION_PGM` | Called Program    | Validates input fields such as customer ID, order quantity, and product codes before processing. |
| `LIB2/ORDER_SRVPGM`   | Service Program   | Provides business logic for order calculations and status updates.           |
| `LIB1/DATA_AREA_1`    | Data Area         | Stores runtime configuration values such as environment flags or thresholds. |
| `LIB1/MSG_QUEUE_1`    | Data Queue        | Used to send and receive asynchronous messages between batch and interactive jobs. |
| `SALESERRS`           | Message File      | Used to retrieve error and validation messages. Assigned to `@MSGFILE`.     |

6. #### Usage Example
Provide a code snippet showing how the subroutine is called within the program. 
Example:
```rpgle
       InputVar = 'abc';
       EXSR MySubroutine;       
       If OutputVar > 5;      
``` 

## how_output
The `how_output` section explains how the specific subroutine works internally, covering the full execution flow and logic used at each step. It includes details on variables, constants, indicators, field mappings, subroutine calls, and error handling related to that unit.

1. #### Purpose
Provide a short insight into the purpose of the code for the subroutine. This should focus on the business logic or functional role of the code.

2. #### Global Definitions 

The explanation of global definitions can be found [here](/pages/task/explain_global_definitions.md).

3. #### Main Execution Flow
Describe the subroutine logic in ordered steps. This includes a detailed explanation of each line, describing what it does and how it works.
It should also include a list of any subroutines called within the subroutine, along with their purpose.

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