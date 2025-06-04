# Explain subset of RPG source file

To simulate selection procedure from a source file, the following syntax is used to avoid duplicating the entire procedure source and context.
This corresponds to the metatdata.txt [`scope`](/pages/metadata.txt###scope) of  `proc`.

# Imporant:

- All the free-form snippets must be indented by at least 7 spaces before the code.
- All fixed-form snippets must have exactly 5 spaces before the specification type.
- If the rpgle to start the snippet is indented, the 5 or 7 spaces must start at the same indentation as the rpgle line.

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

The `api_output` part provides a high-level summary of the procedure  purpose and behavior, detailing the parameters passed to and from the procedure, expected inputs and outputs, dependencies, limitations, and optional usage examples.

1. #### Purpose
Provide a short insight into the purpose of the code for the procedure. 

2. #### Parameters
List all entry parameters accepted by the code, specifying whether each one is an input.

See [`How to explain parameters and return value`]((/pages/explain_parameters.md))

3. #### Return Values

See [`How to explain parameters and return value`]((/pages/explain_parameters.md))

4. #### Inputs and Outputs 
  
##### Global variables used

See the Variables section in [`Global definitions`]((/pages/explain_global_definitions.md))

Describe global variables used in the procedure, including their types and purposes. This includes any global variables are using `EXPORT` or `IMPORT` keywords. 

Example:  
| Name           | Data Type            | Usage        | Description                                 | Attributes |
|----------------|--------------------- |-------------|---------------------------------------------|------------|
| `customerId`   | char(10)             | Input       | The unique identifier for the customer      |            |
| `orderTotal`   | packed(9:2), 5 bytes | Input, Output      | The total amount of the order calculated    | INZ(0)     |
| `currencyCode` | char(3)              | Input       | The currency in which the order is placed   | CONST      |

##### File I/O
Describe how the procedure reads from and writes to:
- Data files (e.g., physical/logical files)
- Device files (e.g., printers, displays)
- Display files (for user interaction)
- IFS files

Provide in table format with columns for file name, type (data/device/display), and description of how the procedure interacts with each file.
Example:
| File Name       | Type     | Used          | Description                                      |
|-----------------|----------|---------------|--------------------------------------------------|
| `CUSTOMER`      | Data     | Input         | Reads customer records for processing            |
| `ORDER_DISPLAY` | Display  | Input/Output  | Displays order details to the user               |
| `PRINTER_FILE`  | Printer  | Output        | Sends reports to the printer                     |
| `ORDERS`        | Data     | Input/Update  | Reads and updates order information              |
| `LOG_FILE`      | IFS      | Output        | Logs program execution details to the IFS        |

5. #### Dependencies 
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
 
6. #### Limitations & Assumptions 
Mention any assumptions the code makes, or scenarios where it may fail or behave incorrectly.
  - Example: The caller cannot update records it is only input. This means the procedure  is designed to read data but not modify it.
      
7. #### Usage Example 
This should show how to call the procedure, including any necessary setup for parameters, along with an example or description of how the caller would handle the return value or any output parameters.

  1. Parameter Setup
    - Define and initialize all input, output parameters and any return values required for the call.
    - Include declarations for any necessary constants, variables, or data structures.

  2. The Procedure Call
    - If the procedure returns a value, show how the caller captures and uses that return value.
    - If parameters are modified, show how the caller accesses the updated values.

  3. When No Follow-Up Coding is Required
    - If the procedure performs an action with side effects (e.g., writing to a file, logging), and there is no need for the caller to process return values or modified parameters, the example may omit further usage.
    - In such cases, document only the call and any required setup.
    
```rpgle
      
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

The explanation of global definitions can be found [here](/pages/task/explain_global_definitions.md).

4. #### Main Execution Flow 
This section explains the core logic of the procedure, outlining the key steps taken to fulfill its purpose. It should also include a list of any subroutines or internal procedures used, along with a brief description of each subroutine.

5. #### Possible Problems with this Code
Identify potential issues that could arise during the execution of the procedure. Mention any problems that should be noted when discussing the final statement that ends the procedure .

6. #### Possible Improvements to this Code
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
