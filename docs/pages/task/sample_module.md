 For example the `getactinf.rpgle` contains the code to be explained. 

```rpgle
**free
ctl-opt nomain;

/copy getactinfcpy

//--------------------------------------------------------
// File Declaration
//--------------------------------------------------------
dcl-f AccPf usage(*input) keyed;

//--------------------------------------------------------
// Procedure: GetAccNo
// Purpose : Returns Account Number for given User ID
//--------------------------------------------------------
dcl-proc GetAccNo  export;
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
dcl-proc GetAccDesc  export;
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

## api_output

### 1. Purpose

The `GetActInf` module provides reusable subprocedures to retrieve account-related information from the `AccPf` physical file based on a given customer ID (`CustId`). Specifically, it allows external programs to obtain:

- The account number (`AccNo`)
- The account description (`AccDesc`)

### 2. Global Dependencies

| Dependency | Type  | Description                                      |
|------------|-------|--------------------------------------------------|
| `AccPf`    | File  | Physical file containing account records         |
| `getactinfcpy` | Copybook | Contains prototypes for exported procedures |

### 3. Exported Procedures

#### Procedure: `GetAccNo`

##### Purpose
Retrieves the account number (`AccNo`) associated with a given customer ID (`CustId`) from the physical file `AccPf`. The procedure reads the file sequentially and returns the first matched account number.

##### Parameters

|   | Name       | Usage | Data Type  | Description                               | Attributes |
|---|------------|-------|------------|-------------------------------------------|------------|
| 1 | `P_UserId` | Input | char(10)   | Customer ID to search in the `AccPf` file | CONST      |

##### Return Value

| Data Type  | Description                                     | Attributes |
|------------|-------------------------------------------------|------------|
| char(10)   | The account number associated with the given ID |            |

##### Usage Example

```rpgle
       /copy getactinfcpy

       dcl-s userid char(10) inz('TEST01');
       dcl-s account_number char(10);

       account_number = GetAccNo(userid);

       // account_number will contain the account number if found, or blank if not found
```

#### Procedure: `GetAccDesc`

##### Purpose
Retrieves the account description (`AccDesc`) associated with a given customer ID (`CustId`) from the physical file `AccPf`. The procedure performs a sequential read and returns the description for the first matched customer record.

##### Parameters

|   | Name       | Usage | Data Type  | Description                               | Attributes |
|---|------------|-------|------------|-------------------------------------------|------------|
| 1 | `P_UserId` | Input | char(10)   | Customer ID to search in the `AccPf` file | CONST      |

##### Return Value

| Data Type  | Description                                         | Attributes |
|------------|-----------------------------------------------------|------------|
| char(50)   | The account description associated with the given ID|            |

##### Usage Example

```rpgle
       /copy getactinfcpy

       dcl-s userid char(10) inz('TEST01');
       dcl-s account_desc char(50);

       account_desc = GetAccDesc(userid);

       // account_desc will contain the account description if found, or blank if not found
```

## how_output

## how_output

### 1. Purpose

The RPGLE module provides reusable subprocedures to retrieve account-related information from the `AccPf` physical file based on a given customer ID (`CustId`). Specifically, it allows external programs to obtain:
- The account number (`AccNo`)
- The account description (`AccDesc`)

### 2. Control Specifications

```rpgle
       ctl-opt nomain;
```
- `nomain`: Indicates that this module does not have a main procedure. It is intended to be used as a service program or a module containing only procedures.

### 3. File Declarations

| File Name | Type | Used  | Description                                 |
|-----------|------|-------|---------------------------------------------|
| `AccPf`   | Data | Input | Reads account records for lookup operations |

```rpgle
       dcl-f AccPf usage(*input) keyed;
```
- `AccPf`: Name of the physical file containing account records.
- `usage(*input)`: The file is opened for input (read-only).
- `keyed`: The file is accessed using a key field (typically `CustId`).

### 4. Global Definitions

There are no global variables or data structures defined outside the procedures in this module.

### 5. Procedures

#### Procedure: GetAccNo

##### Purpose:
  Returns the account number (`AccNo`) for a given user ID (`P_UserId`).

##### Parameters:

  |   | Name      | Usage | Data Type | Description                               | Attributes |
  |---|-----------|-------|-----------|-------------------------------------------|------------|
  | 1 | `P_UserId`| Input | char(10)  | Customer ID to search in the `AccPf` file | CONST      |

##### Local Variables:

  | Name         | Data Type | Description                              | Attributes |
  |--------------|-----------|------------------------------------------|------------|
  | `FoundAccNo` | char(10)  | Stores the found account number, blank if not found |            |

##### Return Value:

  | Data Type | Description                                     | Attributes |
  |-----------|-------------------------------------------------|------------|
  | char(10)  | The account number associated with the given ID |            |

##### Logic: 
  - Positions to the start of the file.
  - Reads each record sequentially.
  - If `CustId` matches `P_UserId`, assigns `AccNo` to `FoundAccNo` and exits the loop.
  - Returns `FoundAccNo` (blank if not found).

#### Procedure: GetAccDesc

##### Purpose: 
  Returns the account description (`AccDesc`) for a given user ID (`P_UserId`).

##### Parameters:

  | # | Name      | Usage | Data Type | Description                               | Attributes |
  |---|-----------|-------|-----------|-------------------------------------------|------------|
  | 1 | `P_UserId`| Input | char(10)  | Customer ID to search in the `AccPf` file | CONST      |

##### Local Variables:

  | Name           | Data Type | Description                                   | Attributes |
  |----------------|-----------|-----------------------------------------------|------------|
  | `FoundAccDesc` | char(50)  | Stores the found account description, blank if not found |            |

##### Return Value:

  | Data Type | Description                                         | Attributes |
  |-----------|-----------------------------------------------------|------------|
  | char(50)  | The account description associated with the given ID|            |

##### Logic:
  - Positions to the start of the file.
  - Reads each record sequentially.
  - If `CustId` matches `P_UserId`, assigns `AccDesc` to `FoundAccDesc` and exits the loop.
  - Returns `FoundAccDesc` (blank if not found).

### 6. Possible Problems

- If the procedures are called multiple times without reinitializing the local variables, previous values may persist.
- No validation is performed on `P_UserId` (e.g., blank or invalid input).
- If no match is found, the procedures return a blank value, which may not clearly indicate "not found" to the caller.

### 7. Possible Improvements

- Validate `P_UserId` before searching.
- Use keyed read operations (`read(e) AccPf`) for better performance if `CustId` is a key.
- Return a more meaningful result or error indicator if no match is found.

## sum_output

### Summary

The `GetAccDesc` procedure is designed to retrieve the account description (`P_AccDesc`) associated with a given user ID (`P_UserId`) from a file (`AccPf`). It iterates through the records in the file, checking each record to see if the `CustId` matches the provided `P_UserId`. If a match is found, the corresponding account description (`AccDesc`) is assigned to `P_AccDesc` and returned. If no match is found, an empty string is returned.


## metadata

```yaml
difficulty: 2
language: rpg4ff
scope: module
use: eval
```

Full description of all the metadata variants can be found [here](/pages/metadata.md).