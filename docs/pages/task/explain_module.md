# Explanation format for an RPG module

The following explanation applies to RPG source files modules that will be bound into programs or service programs.
The [`scope`](/pages/metadata#scope) in `metadata.txt` will be `module` in this case.

# Important:

- All the free-form snippets must be indented by at least 7 spaces before the code.
- All fixed-form snippets must have exactly 5 spaces before the specification type.
- If the rpgle to start the snippet is indented, the 5 or 7 spaces must start at the same indentation as the rpgle line.

### sum_output

The sum_output part offers a business summary in a couple of sentences, explaining the overall purpose of the module

### api_output

The `api_output` part provides information to the caller about how to call the exported procedures in this module and what the effects of calling it would be. It includes a high-level summary of the module's purpose and then for each exported procedure, an api-level description of it.  

**Important**: If a module exports `multiple procedures`, each procedure must be documented `separately` using the standard format below.

1. #### Purpose
Provide a short, high-level summary describing the overall purpose of the `module`.
This summary should focus on what the module is designed to achieve functionally, without going into the individual procedure details.

2. #### Global Dependencies
List all global dependencies required by the module, including any files, data areas, data queues, or message files that the module interacts with directly. Exclude any system APIs.

3. #### Exported Procedures
Each exported procedure should be documented using the following structure:

#### Procedure: `<procedure_name>`

  ##### Purpose
  Provide a short, high-level summary describing the purpose of the procedure. This should be a concise statement that captures the main functionality or goal of the procedure.

  ##### Parameters
  Refer to the [How to explain parameters](/pages/task/explain_parameters.md) for detailed instructions for the Type and possible Attributes columns for the parameters 

  ##### Return Value
  Refer to the [How to explain Return Value](/pages/task/explain_parameters.md) for detailed instructions for the Type and possible Attributes columns for the return value.

  ##### Usage Example:
  Provide a code snippet that demonstrates how to call the procedure, including parameter setup and the call itself. This should be a complete and realistic example that users can follow as a reference.

  This section should illustrate all relevant aspects of calling the procedure, including:

 1. The `/COPY` Statement
   - Show how the caller includes the prototype using a `/COPY` directive. 
   - This is always applicable for RPGLE examples with `scope=module`, as the caller is expected to reside in a different module.

  2. Parameter Setup
    - Define and initialize all input, output parameters and any return values required for the call.
    - Include declarations for any necessary constants, variables, or data structures.
    - Define data structures using LIKEDS with the name of the data structure defined in the /COPY file

  3. The Procedure Call
    - If the procedure returns a value, show how the caller captures and uses that return value.
    - If parameters are modified, show how the caller accesses the updated values.

  example:
  ```rpgle
         /COPY QRPGLESRC,MYPROTOTYPE

         DCL-S customerId   CHAR(10)   INZ('CUST001');
         DCL-S orderTotal   PACKED(9:2);
         DCL-S statusCode   CHAR(2);

         CalculateOrderTotal(customerId : orderTotal : statusCode);

         // After call, orderTotal and statusCode will have updated values
  ```

Repeat the above `procedure section` for `each additional exported procedure` in the module.

### how_output

The how_output part explains how the module works internally, covering the full execution flow and logic used at each step. It includes details on control specifications, file declarations, global variables and brief descriptions of each procedure defined in the module.

1. #### Purpose
Provide a short, high-level summary describing the overall purpose of the `module`.

2. #### Control Specifications 
Any key compiler options and runtime control settings included in the module. Examples include `DftActGrp`, `ActGrp`, `BNDDIR`, and other relevant control specifications. Explain these settings and any keywords used.

3. #### File Declarations
List all declared files from the F-specs with relevant keywords. This includes display files, physical,logical files, printer files, and special files. Explain how each file is declared and any keywords used.

  Provide in table format with columns for file name, type (data/device/display), and description of how the module interacts with each file.
  Example:
  | File Name       | Type     | Used         | Description                                      |
  |-----------------|----------|--------------|--------------------------------------------------|
  | `CUSTOMER`      | Data     | Input        | Reads customer records for processing            |
  | `ORDER_DISPLAY` | Display  | Input/Output | Displays order details to the user               |
  | `PRINTER_FILE`  | Printer  | Output       | Sends formatted reports to the printer           |
  | `ORDERS`        | Data     | Input/Update | Reads and updates order information              |

4. #### Global Definitions

The explanation of global definitions can be found [here](/pages/task/explain_global_definitions.md).

5. #### Procedures 
Each procedure defined in the module should be explained in detail, including its name, the parameters it accepts and what value it returns, if any. In addition, provide a clear description of the procedure’s functionality, outlining its main logic and how it contributes to the overall purpose of the module.

6. #### Possible probelms with code 
Identify potential issues that could arise during the execution of the program. Mention any problems that should be noted when discussing the final statement that ends the program.

7. #### Possible improvements with code
Identify areas where the code could be enhanced for better performance, readability, or functionality. This may include suggestions for error handling, optimization techniques, or alternative approaches to achieve the same result.

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
