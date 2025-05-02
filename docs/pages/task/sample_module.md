 For example the `getactinf.rpgle` contains the code to be explained. 

```rpgle
**free
ctl-opt nomain;

//--------------------------------------------------------
// File Declaration
//--------------------------------------------------------
dcl-f AccPf usage(*input) keyed;

//--------------------------------------------------------
// Procedure: GetAccNo
// Purpose : Returns Account Number for given User ID
//--------------------------------------------------------
dcl-proc GetAccNo;
   dcl-pi GetAccNo char(10);
     P_UserId char(10) const;
   end-pi;

   dcl-s FoundAccNo char(10) inz('');

   setll *loval AccPf;
   read AccPf;
   dow not %eof(AccPf);
     if CustId = P_UserId;
       FoundAccNo = AccNo;
       leave;
     endif;
     read AccPf;
   enddo;

   return FoundAccNo;
end-proc;

//--------------------------------------------------------
// Procedure: GetAccDesc
// Purpose : Returns Account Description for given User ID
//--------------------------------------------------------
dcl-proc GetAccDesc;
   dcl-pi GetAccDesc char(50);
     P_UserId char(10) const;
   end-pi;

   dcl-s FoundAccDesc char(50) inz('');

   setll *loval AccPf;
   read AccPf;
   dow not %eof(AccPf);
     if CustId = P_UserId;
       FoundAccDesc = AccDesc;
       leave;
     endif;
     read AccPf;
   enddo;

   return FoundAccDesc;
end-proc;
``` 


This example indicates that the `AccMgmt.rpgle` relies on the `accpf.pf` file. If there were additional files like display files or copybooks, they would be included in the context section as well.

Therefore the `context` directory would look like:

```text
 - context
    - accpf.pf 
```

`api_output.md` has the content for `getactinf.rpgle`:

### 1. Purpose

The `GetActInf` module provides reusable subprocedures to retrieve account-related information from the `AccPf` physical file based on a given customer ID (`CustId`). Specifically, it allows external programs to obtain:

- The account number (`AccNo`)
-  The account description (`AccDesc`)

### 2. Exported Procedures

#### 2.1 Procedure: `GetAccNo`

#### Description
  Retrieves the account number (`AccNo`) associated with a given customer ID (`CustId`) from the physical file `AccPf`. The procedure reads the file sequentially and returns the first matched account number.

#### Parameters 
| Parameter Name | Direction | Data Type  | Description                                     |
| -------------- | --------- | ---------- | ----------------------------------------------- |
| `P_UserId`     | Input     | `char(10)` | Customer ID to search in the `AccPf` file       |
| -Return Value- | Output    | `char(10)` | The account number associated with the given ID |

#### Dependencies
  - File: `AccPf`
    This is a physical file that must contain at least the following fields:
    - `CustId`: Customer ID (`char(10)`)
    - `AccNo`: Account Number (`char(10)`)

#### Limitations & Assumptions
  - The procedure assumes that `CustId` values are unique. only the first match will be returned.
  - If the input `P_UserId` is blank or not found in the file, the returned account number will be blank.
  - No validations are performed on input or output values.
  - It assumes `AccPf` is not being exclusively locked or blocked by another job at the time of read.

#### Outcomes
- `Successful Retrieval`:
  If a record exists in `AccPf` where `CustId` matches `P_UserId`, the procedure returns the corresponding `AccNo`.

- `Blank or Invalid User`:
  If `P_UserId` is blank or no matching record exists:
  - The return value will be blank.
  - If not properly handled, this could result in unintended usage of stale or empty values in the calling program.

#### Usage Example
```rpgle
dcl-s userid char(10);
dcl-s account_number char(10);

userid = 'TEST01';
account_number = GetAccNo(userid);
```
- If a matching record is found:
  `account_number` will contain the correct account number, such as `'9876543210'`.
- If no match is found:
  `account_number` will remain blank. The caller must ensure it is not reused across calls without resetting.

#### 2.2 Procedure: `GetAccDesc`

#### Description
  Retrieves the account description (`AccDesc`) associated with a given customer ID (`CustId`) from the physical file `AccPf`. The procedure performs a sequential read and returns the description for the first matched customer record.

#### Parameters
| Parameter Name | Direction | Data Type  | Description                                                |
| -------------- | --------- | ---------- | ---------------------------------------------------------- |
| `P_UserId`     | Input     | `char(10)` | Customer ID to search for in the `AccPf` file              |
| -Return Value- | Output    | `char(50)` | The account description associated with the input `CustId` |

#### Dependencies
   File: `AccPf`
    This file must be defined in the main program or in an included copybook. It should contain at least:
    - `CustId`: Customer ID (`char(10)`)
    - `AccDesc`: Account Description (`char(50)`)

#### Limitations & Assumptions
  - Only the first matching `CustId` will be considered.
  - If the input `P_UserId` is not found or is blank, the return value will be blank.
  - No trimming or validation of returned description text is performed.
  - The procedure assumes `AccPf` is unlocked and available for read access during execution.

#### Outcomes

- `Successful Retrieval`:
  When a record with `CustId` matching `P_UserId` exists in `AccPf`, the procedure returns the corresponding `AccDesc`.

- `Blank or Invalid User`:
  - If `P_UserId` is blank or unmatched:
    - The returned value will be blank.
    - Calling code must ensure the returned value is not reused inappropriately in future calls.

#### Usage Example

```rpgle
dcl-s userid char(10);
dcl-s account_desc char(50);

userid = 'TEST01';
account_desc = GetAccDesc(userid);
```

- If a match is found:
  `account_desc` will contain something like `'Premium Savings Account - Tier 1'`.
- If no match is found:
  `account_desc` will be blank. The calling logic should handle such cases to avoid misinterpretation.


`how_output.md` has the content for `getactinf.rpgle`:

### Purpose
The RPGLE program provides reusable subprocedures to retrieve account-related information from the `AccPf` physical file based on a given customer ID (`CustId`). Specifically, it allows external programs to obtain:

- The account number (`AccNo`)
- The account description (`AccDesc`)

### 2. Control Specifications
Control specifications in RPGLE are used to define the overall behavior and environment settings for the program.
- `nomain`: Indicates that this module does not have a main procedure. It is typically used for service programs or modules that only contain procedures.

### 3. File Specifications

`dcl-f AccPf usage(-input) keyed;`
- `AccPf`: The name of the physical file.
  - Attributes:
    - `I`: Input file, meaning the file can be read.
    - `F`: Fully procedural file, meaning all I/O operations are manually controlled in the program.
    - `E`: Externally described file, meaning the file's structure is defined outside the program.
    - `K`: Keyed access, meaning the file is accessed using a key field.
    - `Disk`: Indicates that this is a disk file used for storing data on disk.

### Procedure: GetAccNo

Purpose: This procedure returns the account number (`AccNo`) for a given user ID (`P_UserId`).

#### 1. Initialization

#### Input Parameter:
- `P_UserId` (Type: `char(10)`, Constant): This is the user ID passed as an input parameter to the procedure. It is used to search for the corresponding account number in the `AccPf` file.

#### Variable
Local Variable: `FoundAccNo` (Type: `char(10)`): This variable holds the account number of the user once it is found. It is initialized to an empty string (`inz('')`).

#### 2. Main Logic
- The procedure begins by positioning the file pointer to the first record in the `AccPf` file using `setll -loval AccPf`, ensuring that the search starts from the beginning of the file.
- The first record is read from the file with `read AccPf`, bringing the record into memory for processing.
- The procedure enters a `dow not %eof(AccPf)` loop that continues as long as there are records to process.
    - Inside the loop, the `if CustId = P_UserId` condition is checked to see if the `CustId` from the current record matches the `P_UserId` parameter passed to the procedure.
    - If a match is found:
        - The account number (`AccNo`) from the current record is stored in the `FoundAccNo` variable.
        - The loop is exited immediately using the `leave` statement, as the account number has been found.
    - If the `CustId` does not match, the next record is read from the file with `read AccPf`, and the loop continues to the next record.
- The loop continues processing records until either a match is found or the end of the file (`%eof(AccPf)`) is reached.

#### 3. Return Value
  - The procedure returns the value of `FoundAccNo`, which will either contain the account number of the user corresponding to `P_UserId` or remain empty (`''`) if no match was found.

### 4. Possible Problems with this Code
- If the procedure is called multiple times without reinitializing the `FoundAccNo` variable, it may retain the previous value, leading to incorrect results in subsequent calls.
- The program does not validate whether the `P_UserId` parameter is blank or invalid before attempting to search the file. An empty or invalid user ID could cause the procedure to search unnecessarily or return incorrect data.
- The loop exits as soon as a match is found, but there is no handling for scenarios where `CustId` is not found in the file. The procedure will return an empty string (`FoundAccNo`) without any indication that no match was found, which may be misleading.

### 5. Possible Improvements to this Code
- Add validation to check if the `P_UserId` parameter is blank or invalid before performing the file search. If `P_UserId` is invalid, return an appropriate error or indication.
- Consider optimizing the search by using a keyed read operation (`read(e) AccPf`) if `AccPf` is a keyed file and the `CustId` is a key field. This would significantly reduce search time compared to sequentially reading all records.
- If no matching `CustId` is found, return a meaningful result or error message instead of an empty string. This would provide clarity that the user ID was not found in the records.


### Procedure: GetAccDesc
Purpose: This procedure returns the account description (`AccDesc`) for a given user ID (`P_UserId`).

#### 1. Initialization

#### Input Parameter:
- `P_UserId` (Type: `char(10)`, Constant): This is the user ID passed as an input parameter to the procedure. It is used to search for the corresponding account description in the `AccPf` file.

#### Variable:
-  Local Variable: `FoundAccDesc` (Type: `char(50)`): This variable holds the account description of the user once it is found. It is initialized to an empty string (`inz('')`).

#### 2. Main Logic
- The procedure begins by positioning the file pointer to the first record in the `AccPf` file using `setll -loval AccPf`, ensuring that the search starts from the beginning of the file.
- The first record is read from the file with `read AccPf`, bringing the record into memory for processing.
- The procedure enters a `dow not %eof(AccPf)` loop that continues as long as there are records to process.
  - Inside the loop, the `if CustId = P_UserId` condition is checked to see if the `CustId` from the current record matches the `P_UserId` parameter passed to the procedure.
  - If a match is found:
    - The account description (`AccDesc`) from the current record is stored in the `FoundAccDesc` variable.
    - The loop is exited immediately using the `leave` statement, as the account description has been found.
  - If the `CustId` does not match, the next record is read from the file with `read AccPf`, and the loop continues to the next record.
- The loop continues processing records until either a match is found or the end of the file (`%eof(AccPf)`) is reached.

#### 3. Return Value
- The procedure returns the value of `FoundAccDesc`, which will either contain the account description of the user corresponding to `P_UserId` or remain empty (`''`) if no match was found.

### 4. Possible Problems with this Code
- If the procedure is called multiple times without reinitializing the `FoundAccDesc` variable, it may retain the previous value, leading to incorrect results in subsequent calls.
- The program does not validate whether the `P_UserId` parameter is blank or invalid before attempting to search the file. An empty or invalid user ID could cause the procedure to search unnecessarily or return incorrect data.
- The procedure will return an empty string (`FoundAccDesc`) if no match is found, but there is no indication that no match was found. This might be misleading, as users would not know whether the user ID was not found or if there was an error.

### 5. Possible Improvements to this Code
- Implement a check to verify if `P_UserId` is blank or invalid before performing the file search. If `P_UserId` is invalid, return an appropriate error or indication.
- If `AccPf` is a keyed file, consider using a keyed read operation (`read(e) AccPf`) to directly access the record with the matching `CustId`, improving the performance by reducing search time.
- If no matching `CustId` is found, return a meaningful result or error message instead of just an empty string. This would help clarify that the user ID was not found in the records, providing more informative feedback.


### Summary

The `GetAccDesc` procedure is designed to retrieve the account description (`P_AccDesc`) associated with a given user ID (`P_UserId`) from a file (`AccPf`). It iterates through the records in the file, checking each record to see if the `CustId` matches the provided `P_UserId`. If a match is found, the corresponding account description (`AccDesc`) is assigned to `P_AccDesc` and returned. If no match is found, an empty string is returned.


`metadata.txt` has the content from `getactinf.rpgle`:

```yaml
difficulty: 2
language: rpg4ff
scope: module
use: eval
```

Full description of all the metadata variants can be found [here](/pages/metadata.md).