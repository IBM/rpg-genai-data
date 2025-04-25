# Explain existing RPG in Natural Language

A common problem for IBM i customers is coming across old code that is not easy to understand. The goal of this task is to train the AI to be able to produce a natural language description of some RPG.
In this scenario the question is in format of RPG code and the output is an explanation of that code.

Since the task is to "explain" some RPG, all of the following data is found in the `data/explain` directory.
For this example we will have a training pair submitted by IBM, so the data will be in the `data/explain/IBM` directory.

The directory structure for Procedure or subroutine look like:

- procedure\
  - output
    - sum_output.md
    - api_output.md
    - how_output.md
  - metadata.txt

Note: Input and Context folder are not required for Procedure or subroutine.

The `Updatefieldsvalidation` procedure contains the code to be explained. 

```rpgle 
            Dcl-Proc Updatefieldsvalidation;

                Dcl-S Validmail Varchar(100);
                Dcl-S Adharidlen Zoned(2);
                Dcl-S Contactlen Zoned(10);
                Dcl-S Result1 Zoned(2);
                Dcl-S Result2 Zoned(2);
                Dcl-S Regex Varchar(50);
                Dcl-S C_String Char(53);
                Dcl-S Count Zoned(5);

                Result1 = %Scan( '0' : %Char(Uorgadhrid) : 1 );
                Result2 = %Scan( '0' : %Char(Uorgmobno) : 1 );
                Adharidlen = %Len(%Trim(%Char(Uorgadhrid)));
                Contactlen = %Len(%Trim(%Char(Uorgmobno)));
                Validmail = %Trim(Uorgmail);

                Regex = '^(?:\W+\.?)*\W+@(?:\W+\.)*\W+$';
              C_String = 'Abcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyz';

                Exec Sql
                   Set :Count = Regexp_Count(:Validmail,:Regex);
                Select;
                   When Uorgadhrid = 0 ;
                        Errmsg = 'Aadhar Id Field Cannot Be Blank';
                        Madharid = *On;
                        Return;
                   When Adharidlen < 12;
                        Errmsg = 'Aadhar Id Length Should Be Of 12 Digits';
                        Madharid = *On;
                        Return;
                   When Uorgname = *Blanks;
                        Errmsg = 'Event Organizer Field Cannot Be Blank';
                        Dorgname = *On;
                        Return;
                   When %Check(C_String:Uorgname) <> 0;
                        Errmsg = 'Only Characters Allowed';
                        Dorgname = *On;
                        Return;
                   When Uorggender = *Blanks;
                        Errmsg = 'Gender Feild Cannot Be Blank';
                        Morggender = *On;
                        Return;
                   When Uorgstate  = *Blanks;
                        Errmsg = 'State Feild Cannot Be Blank';
                        Morgstate = *On;
                        Return;
                   When Uorgaddr  = *Blanks;
                        Errmsg  = 'Address Field Cannot Be Blank';
                        Morgaddress = *On;
                        Return;
                   When Uorgmobno = 0;
                        Errmsg  = 'Contact Number Field Cannot Be Blank';
                        Morgmobnumber =  *On;
                        Return;
                   When Result2 = 1 ;
                         Errmsg = 'Mobile Number Should Not Strat With Zero';
                         Morgmobnumber = *On;
                         Return;
                   When Contactlen < 10;
                       Errmsg  = 'Contact Number Length Should Be Of 10 Digits';
                        Morgmobnumber =  *On;
                        Return;
                   When Uorgmail = *Blank;
                        Errmsg = 'Email Field Can Not Be Blank';
                        Dorgmail = *On;
                        Return;
                   When Count <> 1;
                         Errmsg = 'Invalid Email';
                         Dorgmail = *On;
                         Return;
                Endsl;
            End-Proc;
```

### Output

The Output Section is designed to provide a comprehensive explanation of the RPGLE procedure oe subroutine behavior, structure, and logic. It is divided into three main parts: `api_output`, `how_output`, and `sum_output`. Each part serves a specific purpose and includes detailed information to ensure clarity and consistency.

### api_output 
The `api_output` part provides a high-level summary of the procedure or subroutine purpose and behavior, detailing the parameters passed to and from the procedure, expected inputs and outputs, dependencies, side effects or limitations, and optional usage examples.

1. `Purpose`: Provide a short insight into the purpose of the code for the procedure or subroutine. This should focus on the business logic or functional role of the code.

2. `Inputs`: Provide all input parameters or external inputs used by the procedure or procedure. Keep it short and make sure to list the field names, if any. This section can be divided into subsections
  - `Entry parameters`: Entry parameters. Mention the length if it is referring to a keyword.
  - `Input files`: List the files that are input for the procedure or procedure.
  - `Input fields`: List the fields that are input for the procedure or subroutine.

3. `Outputs`: Provide all the return values and output fields such as display fields, host variables, and overall outputs of the procedure or subroutine execution. Make sure to list the field names, if any. This section can also be divided into subsections
  - `Return values`: Describe the return values from the procedure or subroutine, including data types and meanings.
  - `Output fields`: List the fields that are output from the procedure or subroutine.
  - `Output files`: List the files that are output for the pprocedure or subroutine.

4. `Dependencies`: This section outlines all components that the procedure or subroutine depends on in order to run successfully. Dependencies include physical and logical files used for data access, external procedures for business logic, and display files for user interface interactions.

5. `Side Effects`: Mention any indirect or non-obvious effects such as data area updates, logs, or changes to global state. 
  - Example: This includes any global variables whose values are changed within the procedure.

6. `Limitations & Assumptions`: Mention any assumptions the code makes, or scenarios where it may fail or behave incorrectly.
  - Example: The caller cannot update records it is only input. This means the procedure or subroutine is designed to read data but not modify it.
      
7. `Outcomes`: List possible major outcomes or scenarios resulting from the code execution. This section can include an optional usage example to illustrate the expected outcome.

`api_output.md` has the content for `Updatefieldsvalidation`:

```text
### 1. Purpose
The colunm-limited SQLRPGLE `Updatefieldsvalidation` subprocedure validates various fields related to an event organizer's details. It ensures that the data entered meets specific criteria and standards.

### 2. Inputs

#### 2.1 Input Fields 
  - `Uorgadhrid`: Aadhar ID of the event organizer.
  - `Uorgmobno`: Contact number of the event organizer.
  - `Uorgmail`: Email address of the event organizer.
  - `Uorgname`: Name of the event organizer.
  - `Uorggender`: Gender of the event organizer.
  - `Uorgstate`: State of the event organizer.
  - `Uorgaddr`: Address of the event organizer.

#### 3. Outputs

#### 3.1 Return values:
        - Errmsg: Error message indicating the validation failure.
#### 3.2 Output fields:
        - Various indicators (Madharid, Dorgname, Morggender, Morgstate, Morgaddress, Morgmobnumber, Dorgmail) set to *On to highlight the field with validation issues

#### 4. Limitations
  - The subprocedure does not continue checking after any validation fails, requiring all fields to meet the specified criteria before proceeding.

#### 5. Usage Example
    - When the `Updatefieldsvalidation` subprocedure is called, it performs a series of checks on the input fields. If any field fails validation, an appropriate error message is set, and the corresponding indicator is turned on. Then it does not continue checking after any validation fails, requiring all fields to meet the specified criteria before proceeding.
    - For example, if the email format is invalid, the `Errmsg` will be set to "Invalid Email," and the `Dorgmail` indicator will be turned on.
    - The caller should not proceed with the update if `Errmsg` is not blank, as this indicates that the procedure found a problem with one of the fields.
```

### how_output
The `how_output` section explains how the specific procedure or subroutine works internally, covering the full execution flow and logic used at each step. It includes details on variables, constants, indicators, field mappings, subroutine calls, and error handling related to that unit.

1. `Declaration`: Explain all declarations used in the procedure. This includes:
   -`Variable`: List all variables used in the procedure or subroutine and explain their purpose.
   -`Constant`: List constants if any used.
   -`Keywords`: Explain any keywords used in variable declarations, such as `LIKE`, `INZ`, `N` data type, etc.

2. `Field Mapping`: If the procedure or subroutine relates to any display file, create a clear table or list to show how physical/logical file fields are mapped to display fields. This includes field names and display fields.

3. `Indicators`: If the procedure or subroutine uses indicators, explain their purpose and how they are set or cleared during execution in table format.

4. `Main Execution Flow`: Describe high-level procedure or subroutine logic in ordered steps. This includes initialization, opening files, loading initial data, validating and updating the database, error handling, and exiting the procedure.
    `Subroutine`: If the procedure contains subroutines, list and explain each subroutine and its purpose. 

5. `Error Handling`: Mention all error-handling techniques used. 

6. `Possible Problems with this Code`: Identify potential issues that could arise during the execution of the procedure or subroutine. Mention any problems that should be noted when discussing the final statement that ends the procedure or subroutine.

7. `Possible Improvements to this Code`: Identify areas where the code could be enhanced for better performance, readability, or functionality. This may include suggestions for error handling, optimization techniques, or alternative approaches to achieve the same result.
 
`how_output.md` has the content for `Updatefieldsvalidation`:

```text
#### 1. Purpose
- The column-limited SQLRPGLE `Updatefieldsvalidation` subprocedure is designed to validate various fields related to an event organizer. It ensures that the data entered meets specific criteria before proceeding further. This subprocedure performs checks on the Aadhar ID, contact number, email address, name, gender, state, and address fields. If any field fails validation, an appropriate error message is set, and the corresponding indicator is turned on.

#### 2. Declaration Section
The subprocedure begins with the declaration of several standalone variables that are used for validation purposes.

- `Dcl-S Validmail Varchar(100)`
   - `Validmail`: A character variable with a maximum length of 100 characters.
   - `S`: Indicates that this is a stand-alone variable.
   - `Varchar(100)`: Specifies that the variable is a variable-length character field with a maximum length of 100 characters.
   - It is used to store the email address for validation.

- `Dcl-S Adharidlen Zoned(2)`
   - `Adharidlen`: A numeric variable with a length of 2 digits and no decimal places.
   - `S`: Indicates that this is a stand-alone variable.
   - `Zoned(2)`: Specifies that the variable is numeric with a length of 2 digits and no decimal places.
   - It is used to store the length of the Aadhar ID after trimming any leading or trailing spaces.

- `Dcl-S Contactlen Zoned(10)`
   - `Contactlen`: A numeric variable with a length of 10 digits and no decimal places.
   - `S`: Indicates that this is a stand-alone variable.
   - `Zoned(10)`: Specifies that the variable is numeric with a length of 10 digits and no decimal places.
   - It is used to store the length of the contact number after trimming any leading or trailing spaces.

- `Dcl-S Result1 Zoned(2)`
   - `Result1`: A numeric variable with a length of 2 digits and no decimal places.
   - `S`: Indicates that this is a stand-alone variable.
   - `Zoned(2)`: Specifies that the variable is numeric with a length of 2 digits and no decimal places.
   - It is used to store the result of scanning the Aadhar ID for the presence of the digit '0'.

- `Dcl-S Result2 Zoned(2)`
   - `Result2`: A numeric variable with a length of 2 digits and no decimal places.
   - `S`: Indicates that this is a stand-alone variable.
   - `Zoned(2)`: Specifies that the variable is numeric with a length of 2 digits and no decimal places.
   - It is used to store the result of scanning the contact number for the presence of the digit '0'.

- `Dcl-S Regex Varchar(50)`
   - `Regex`: A character variable with a maximum length of 50 characters.
   - `S`: Indicates that this is a stand-alone variable.
   - `Varchar(50)`: Specifies that the variable is a variable-length character field with a maximum length of 50 characters.
   - It is used to store the regular expression pattern for validating the email format.

- `Dcl-S C_String Char(53)`
   - `C_String`: A character variable with a fixed length of 53 characters.
   - `S`: Indicates that this is a stand-alone variable.
   - `Char(53)`: Specifies that the variable is a fixed-length character field with a length of 53 characters.
   -  It is used to store a string of alphabetical characters for checking the validity of the event organizer's name.
     
- `Dcl-S Count Zoned(5)`
   - `Count`: A numeric variable with a length of 5 digits and no decimal places.
   - `S`: Indicates that this is a stand-alone variable.
   - `Zoned(5)`: Specifies that the variable is numeric with a length of 5 digits and no decimal places.
   - It is used to store the result of the regular expression count for validating the email format.

#### 3. Initialization and Validation Logic
The subprocedure initializes the variables and performs validation checks.

-  `Result1 = %Scan('0' : %Char(Uorgadhrid) : 1);`
   - Scans the `Uorgadhrid` numeric variable (Aadhar ID) for the digit '0'.
   - `%Scan` searches for a specified substring within a string.
   - `%Char(Uorgadhrid)` converts the `Uorgadhrid` numeric variable to a character string.
   - `'0'` is the substring to search for.
   - `1` is the starting position for the scan.
   - `Result1` will be set to the position of the first occurrence of '0' in the `Uorgadhrid` string. If '0' is not found, `Result1` will be set to 0.

-  `Result2 = %Scan('0' : %Char(Uorgmobno) : 1);`
   - Scans the `Uorgmobno` numeric variable (contact number) for the digit '0'.
   - `%Scan` searches for the specified substring within the string.
   - `%Char(Uorgmobno)` converts the `Uorgmobno` numeric variable to a character string.
   - `'0'` is the substring to search for.
   - `1` is the starting position for the scan.
   - `Result2` will be set to the position of the first occurrence of '0' in the `Uorgmobno` string. If '0' is not found, `Result2` will be set to 0.

-  `Adharidlen = %Len(%Trim(%Char(Uorgadhrid)));`
   - Calculates the length of the trimmed `Uorgadhrid` numeric variable (Aadhar ID).
   - `%Len` returns the length of a string.
   - `%Trim(%Char(Uorgadhrid))` converts the `Uorgadhrid` numeric variable to a character string and trims any leading or trailing spaces.
   - `Adharidlen` will be set to the length of the trimmed `Uorgadhrid` string.

-  `Contactlen = %Len(%Trim(%Char(Uorgmobno)));`
   - Calculates the length of the trimmed `Uorgmobno` numeric variable (contact number).
   - `%Len` returns the length of a string.
   - `%Trim(%Char(Uorgmobno))` converts the `Uorgmobno` numeric variable to a character string and trims any leading or trailing spaces.
   - `Contactlen` will be set to the length of the trimmed `Uorgmobno` string.

-  `Validmail = %Trim(Uorgmail);`
   - Trims any leading or trailing spaces from the `Uorgmail` character variable (email address).
   - `%Trim` removes leading and trailing spaces from a string.
   - `Validmail` will be set to the trimmed `Uorgmail` string.

-  `Regex = '^(?:\W+\.?)*\W+@(?:\W+\.)*\W+$';`
   - Defines the regular expression pattern used for email validation.
   - Regular expressions match patterns within strings.
   - `^(?:\W+\.?)*\W+@(?:\W+\.)*\W+$` is the pattern to match a valid email address format.
   - `Regex` will be set to the specified pattern.

-  `C_String = 'Abcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyz';`
   - Defines a string of characters used for validation.
   - The string contains a predefined set of alphanumeric characters.
   - `'Abcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyz'` is the string of characters.
   - `C_String` will be set to the specified string.

#### 4. SQL Statement for Regular Expression Count
```rpgle for ILE RPG
       Exec Sql
       Set :Count = Regexp_Count(:Validmail, :Regex);
- This SQL statement counts the occurrences of the regular expression pattern defined in the `Regex` variable within the `Validmail` variable. The result is stored in the `Count` variable, which is used to validate the format of the email address. If the email address matches the pattern, `Count` will be set to the number of matches found. If it does not match, `Count` will be set to 0. This ensures the email address is in a valid format before proceeding with updates.

#### 5. Validation Checks
The subprocedure performs a series of validation checks using a `Select` statement.

#### 5.1 Aadhar Validation for Zeros
  - If `Uorgadhrid` is 0, it sets `Errmsg` to "Aadhar Id Field Cannot Be Blank" and turns on the `Madharid` indicator.
  - If the condition fails, then returns immediately without doing any further validation.

#### 5.2 Aadhar Validation for Length
  - If `Adharidlen` is less than 12, it sets `Errmsg` to "Aadhar Id Length Should Be Of 12 Digits" and turns on the `Madharid` indicator.
  - If the condition fails, then returns immediately without doing any further validation.

#### 5.3 Name Validation for Blanks
  - If `Uorgname` is blank, it sets `Errmsg` to "Event Organizer Field Cannot Be Blank" and turns on the `Dorgname` indicator.
  - If the condition fails, then returns immediately without doing any further validation.

#### 5.4 Name Validation for Characters
  - If `Uorgname` contains invalid characters or numeric, it sets `Errmsg` to "Only Characters Allowed" and turns on the `Dorgname` indicator.
  - If the condition fails, then returns immediately without doing any further validation.

#### 5.5 Gender Validation
  - If `Uorggender` is blank, it sets `Errmsg` to "Gender Field Cannot Be Blank" and turns on the `Morggender` indicator.
  - If the condition fails, then returns immediately without doing any further validation.

#### 5.6 State Validation
  - If `Uorgstate` is blank, it sets `Errmsg` to "State Field Cannot Be Blank" and turns on the `Morgstate` indicator.
  - If the condition fails, then returns immediately without doing any further validation.

#### 5.7 Address Validation
  - If `Uorgaddr` is blank, it sets `Errmsg` to "Address Field Cannot Be Blank" and turns on the `Morgaddress` indicator.
  - If the condition fails, then returns immediately without doing any further validation.

#### 5.8 Mobile Number Validation for Zero
  - If `Uorgmobno` is 0, it sets `Errmsg` to "Contact Number Field Cannot Be Blank" and turns on the `Morgmobnumber` indicator.
  - If the condition fails, then returns immediately without doing any further validation.

#### 5.9 Mobile Number Validation for Starting Digit
  - If `Result2` is 1, it sets `Errmsg` to "Mobile Number Should Not Start With Zero" and turns on the `Morgmobnumber` indicator.
  - If the condition fails, then returns immediately without doing any further validation.

#### 5.10 Mobile Number Validation for Length
  - If `Contactlen` is less than 10, it sets `Errmsg` to "Contact Number Length Should Be Of 10 Digits" and turns on the `Morgmobnumber` indicator.
  - If the condition fails, then returns immediately without doing any further validation.

#### 5.11 Email Validation for Blanks
  - If `Uorgmail` is blank, it sets `Errmsg` to "Email Field Cannot Be Blank" and turns on the `Dorgmail` indicator.
  - If the condition fails, then returns immediately without doing any further validation.

#### 5.12 Email Validation for Format
  - If `Count` is not 1, it sets `Errmsg` to "Invalid Email" and turns on the `Dorgmail` indicator.
  - If the condition fails, then returns immediately without doing any further validation.

#### 6. End-Proc Statement
- If the subprocedure reaches the `End-Proc` statement, it means that no errors were found in the validation.

### 7. Possible Problems with the Procedure

- `Contactlen Zoned(10)`
  - The `Contactlen` variable is declared as `Zoned(10)`, which is excessive for storing the length of a contact number.
  - The length of a contact number is typically a small integer. Therefore, a length of 2 digits (e.g., `Zoned(2)`) would be sufficient to hold the length of a 10-digit number.

- `C_String = 'Abcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyz';`
  - The `C_String` variable includes repeated lowercase characters.
  - The first 26 characters are supposed to be uppercase A-Z. The `C_String` is only used for the first parameter of `%Check`, so there is no reason to have the b-z characters repeated.

- `Result1 = %Scan('0' : %Char(Uorgadhrid) : 1);`
  - The procedure sets the value of `Result1` but does not validate whether the Aadhar ID starts with zero.
  - While `Result1` stores the position of the first occurrence of '0' in the `Uorgadhrid` string, there is no check to ensure that the Aadhar ID does not start with zero.
``` 

### sum_output
The sum_output part offers a business summary in a couple of sentences, explaining the overall purpose of the procedure

`sum_output.md` has the content for `Updatefieldsvalidation`:

```text 
### Summary
The column-limited SQLRPGLE `Updatefieldsvalidation` subprocedure is designed to validate various fields related to an event organizer. It ensures that the data entered meets specific criteria before proceeding further. This subprocedure performs checks on the Aadhar ID, contact number, email address, name, gender, state, and address fields. If any field fails validation, an appropriate error message is set, and the corresponding indicator is turned on.
```

### metadata.txt
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

`metadata.txt` has the content from  `Updatefieldsvalidation`:

```text
source : ADMDASHRPG
start : 1860
end : 1947
difficulty : 3
use: eval
scope: proc
language: rpglf-sql
```
Main program source [ADMDASHRPG](https://github.com/AIforIBMi/rpg-genai-data/blob/11850bbd870a220d76f13394993ca41e79b3a4ab/data/explain/Programmers.io/ADMDASHRPG/input/admdashrpg.pgm.sqlrpgle)

Full description of all the metadata variants can be found [here](/pages/metadata.md).

### Notes

- Use `rpgle` for ILE RPG code blocks and `rpg` for OPM RPG code blocks.
- Use `###` for first-level headings and `####` for sub-level headings.
- Avoid using `**` for bold text use backticks ` ` to highlight quotations from the RPG source, such as variables and RPG syntax.
- Use the term `fully-free` instead of `full free format`.
- Use `ILE RPG` instead of `RPGLE`.
- Do not expand `RPG` as `Report procedure Generator`. Always refer to it as `RPG`. 