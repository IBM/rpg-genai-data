# Explanation format for an RPG program

The following explanation applies to both modern linear-main programs as well as legacy cycle-main programs.
The [`scope`](/pages/metadata#scope) in `metadata.txt` will be `program-linear` or `program-cycle` in this case.

# Important:

- All the free-form snippets must be indented by at least 7 spaces before the code.
- All fixed-form snippets must have exactly 5 spaces before the specification type.
- If the rpgle to start the snippet is indented, the 5 or 7 spaces must start at the same indentation as the rpgle line.

### api_output

The `api_output` part provides information to the caller of a program about how to call the program and what the effects of calling it would be. It includes a high-level summary of the program's purpose and behavior, detailing the parameters passed to and from the program, dependencies,limitations, and optional usage examples.

1. #### Purpose 
Provide a short insight into the purpose of the code for the program. This should focus on the business logic or functional role of the code.

2. #### Parameters
List all entry parameters accepted by the program, specifying whether each one is an input, output, or both. Provide in table format with columns for Name, Usage, Type, Description and attributes. 

See [`How to explain parameters and return value`]((/pages/explain_parameters.md))

3. #### File I/O 
Describe how the program reads from and writes to:
- Data files (e.g., physical/logical files)
- Device files (e.g., printers, displays)
- Display files (for user interaction)
- IFS files 

Provide in table format with columns for file name, type (data/device/display), and description of how the program interacts with each file.
Example:
| File Name       | Type          | Usage         | Description                                  |
|-----------------|---------------|---------------|----------------------------------------------|
| `CUSTOMER`      | Data file     | Input         | Reads customer records for processing        |
| `ORDER_DISPLAY` | Display file  | Input/Output  | Displays order details to the user           |
| `PRINTER_FILE`  | Printer file  | Output        | Sends reports to the printer                 |
| `ORDERS`        | Data file     | Input/Update  | Reads and updates order information          |
| `LOG_FILE`      | IFS file      | Output        | Logs program execution details to the IFS    |

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

5. #### Limitations & Assumptions 
Mention any assumptions the code makes, or scenarios where it may fail or behave incorrectly.
  - Example: The load all subfile can load only up to 9999 records.
  - Example: The caller cannot update records it is only input. This means the program is designed to read data but not modify it.

6. #### Usage Example: 
Provide a code snippet that demonstrates how to call the program. 

1. Parameter Setup
  - Define and initialize all input/output parameters required for the call.
  - Include any relevant data structures, constants, or variables.

2. The program call.
- If there is an RPG prototype for the program, use an rpgle snippet with the /COPY for the prototype, and any definitions needed for parameters. Do not use the CALL or CALLP opcode.
```rpgle
        /copy qrpglesrc,prototype
          
      mypgm (parameters);
```
  - Otherwise, use a clle snippet to represent a call from the command line.
```clle 
        call mypgm parm(parameters)
```

### how_output

The how_output part explains how the program works internally, covering the full execution flow and logic used at each step. It includes details on control specifications, file declarations, global variables, constants, indicators, field mapping, subroutines, error handling, and the main program logic.

1. #### Purpose
Provide a short insight into the purpose of the code for the program. This should focus on the business logic or functional role of the code.

2. #### Control Specifications 
Any key compiler options and runtime control settings included in the program. Examples include `DftActGrp`, `ActGrp`, `BNDDIR`, and other relevant control specifications. Explain these settings and any keywords used.

3. #### File Declarations 
List all global files from the F-specs with relevant keywords. This includes display files, physical,logical files, printer files, and special files. Explain how each file is declared and any keywords used.

Provide in table format with columns for file name, type (data/display/printer), and description of how the program interacts with each file.
Example:
| File Name       | Type     | Used | Description |
|-----------------|----------|------|-------------|
| `CUSTOMER`      | Data     | Input  | Reads customer records for processing |
| `ORDER_DISPLAY` | Display  | Input/Output | Displays order details to the user |
| `PRINTER_FILE`  | Printer  | Output | Sends reports to the printer |

4. #### Global Definitions

The explanation of global definitions can be found [here](/pages/task/explain_global_definitions.md).

5. #### Main Execution Flow 
This section describes the main execution flow of the program in detail, explaining the logic line by line.

  ##### Subroutines 
  If the program contains subroutines or sub procedures, list and explain each subroutine and sub procedures in detail.

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
