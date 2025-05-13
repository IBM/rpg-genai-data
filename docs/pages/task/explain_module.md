# Explanation format for an RPG module

The following explanation applies to RPG source files modules that will be bound into programs or service programs.
The [`scope`](/pages/metadata#scope) in `metadata.txt` will be `module` in this case.

## sum_output

The sum_output part offers a business summary in a couple of sentences, explaining the overall purpose of the module

## api_output

The `api_output` part provides information to the caller about how to call the exported procedures in this module
and what the effects of calling it would be.
It includes a high-level summary of the module's purpose and then for each exported procedure, an api-level description of it.  Note that for modules the caller will not care about any side effects within the module, but only on the external objects such as databases.

`Important`: If a module exports `multiple procedures`, each procedure must be documented `separately` using the standard format below.

 1. `Purpose`

Provide a short, high-level summary describing the overall purpose of the `module`.
This summary should focus on what the module is designed to achieve functionally, without going into the individual procedure details.

 2. `Exported Procedures`

Each exported procedure should be documented using the following structure:

### Procedure: `<procedure_name>`

  -  `Description`
    A clear and concise explanation of what the procedure does.

  -  `Parameters`
    List all parameters with their directions and brief explanations:
    -  `<parameter_name>` (`input`) – Description

  - `Return Value`
    List the return value with its direction and brief explanation:
    -  `-Return Value-` (`output`) – Description

  -  `Dependencies`
    Mention any external components required by this procedure to work:

    -  Physical or logical files
    -  Copybooks
    -  Data areas
    -  External APIs or modules

  -  `Side Effects`
    Describe any indirect or hidden impacts the procedure has on:

    -  Global variables
    -  Shared resources
    -  Logs or external states

  -  `Limitations & Assumptions`
    List any known assumptions or restrictions:

    -  Maximum limits
    -  Required preconditions
    -  Unsupported input types

  -  `Outcomes`
    Define the expected outcomes of the procedure’s execution.
    Optionally, include a short description of a typical usage scenario.

Repeat the above `procedure section` for `each additional exported procedure` in the module.

## how_output

The how_output part explains how the module works internally, covering the full execution flow and logic used at each step. It includes details on control specifications, file declarations, global variables, constants, indicators, field mapping, subroutines, error handling, and the logic of each procedure.

`Important`: Each procedure must be documented separately.

1. `Control Specifications`: Any key compiler options and runtime control settings included in the module. Examples include `DftActGrp`, `ActGrp`, `BNDDIR`, and other relevant control specifications. Explain these settings and any keywords used.

2. `File Declarations`: List all declared files from the F-specs with relevant keywords. This includes display files, physical,logical files, printer files, and special files. Explain how each file is declared and any keywords used.

3. `Global Components`
The Global Variables section includes all variables, prototypes for external program calls, data structures, arrays, constants, and keywords used in the module. Detailed explanations will be provided later in the logic sections.

  - `Prototype & Interface Definition`: Describe internal procedure prototypes and their usage. This includes procedure names, input/output parameters, data types, and return types.
  - `Prototype for External Program Call`: Mention prototypes for external RPG/CL/API calls. This includes program name, parameter interface, data types, and expected results.
  - `Data Structures and Arrays`: Describe all declared data structures and arrays used in the module, including their purpose and how they are utilized.
    - `File Information Data Structure (INFDS)`: Mention any INFDS declared in the program and what file-level metadata they capture (e.g., file status, error codes, record numbers).
    - `Indicator Data Structure (INDDS)`: Explain the usage of indicator-based data structures, if any, especially those mapped to display files or used for control logic.
    - `Arrays`: Describe any arrays used, whether single or multi-dimensional, compile-time or run-time, and their role.
    - `General Data Structures`: Include details of data structures. Mention key attributes or keywords applied (e.g., `inz`, `overlay`, `dim`, `based`, etc.).
  - `Variables`: Explain the variables used in the module. This includes:
    - `Global Variables`: List all global variables used in the module and explain their purpose.
    - `Constants`: List constants declared using `DCL-C`, including status flags, limits, messages, and system values.
    - `Keywords`: Explain any keywords used in variable declarations, such as `LIKE`, `INZ`, `N` data type, etc.

4. `Procedures`

  Provide a step-by-step description of the logic for each procedure.

  - `Initialization`: Describe the initial setup required for the procedure. Include all local variables and parameters.
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