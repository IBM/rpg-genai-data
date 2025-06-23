**example** Copybook `getactinfcpy.rpgleinc` with prototypes `GetAccNo` and `GetAccDesc`

```rpgle
       // Prototype for procedure GetAccNo
       dcl-pr GetAccNo char(10);
           P_UserId char(10) const;   
       end-pr;

       // Prototype for program GETACCDESC
       dcl-pr GetAccDesc extpgm;
           P_AccNo char(10) const;
           P_AccDesc char(50); 
       end-pr;
```

## how_output

### Purpose
This code defines two procedure prototypes using free-form ILE RPG syntax. These prototypes specify the interface for a procedure and an external program that can be called from within an RPGLE program.

### Procedures

#### GetAccNo
This is a procedure prototype for a local procedure named `GetAccNo`.

##### Parameters:

| Name     | Usage | Data Type | Description                    | Attributes |
|----------|-------|-----------|--------------------------------|------------|
| P_UserId | Input | char(10)  | 10-character user identifier   | CONST      |

##### Return value:

| Data Type | Description                  |
|-----------|------------------------------|
| char(10)  | Returns a 10-character value |

#### GetAccDesc
This is a prototype for an external program named `GETACCDESC`.

##### Parameters:

| Name      | Usage | Data Type | Description                     | Attributes |
|-----------|-------|-----------|---------------------------------|------------|
| P_AccNo   | Input | char(10)  | 10-character account number     | CONST      |
| P_AccDesc | Output| char(50)  | 50-character account description|            |

##### Return value:

- None (as it is an external program call, it returns values via parameters)

## sum_output

### Summary
The `getactinfcpy`serves as a reusable piece of code that contains prototypes for two key functions, `GetAccNo` and `GetAccDesc`. It allows these prototypes to be used across different RPGLE programs without having to redefine them each time, making the code more modular, efficient, and maintainable. The procedures are designed to handle account retrieval based on user IDs and account numbers, respectively.

## metadata

```yaml
difficulty: 2
language: rpg4ff
scope: copybook
use: eval
```

Full description of all the metadata variants can be found [here](/pages/metadata.md).
