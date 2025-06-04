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
The copybook `getactinfcpy` contains the following declarations:

1. Prototype for procedure `GetAccNo`:
   ```rpgle
          // Procedure declaration for GetAccNo
          dcl-pr GetAccNo char(10);
              P_UserId char(10) const;   
          end-pr;
   ```
    - `dcl-pr` declares the prototype for procedure `GetAccNo`, which returns a 10-character account number.
    - `P_UserId` is a constant input parameter for the `GetAccNo` procedure. It represents the user ID for which the corresponding account number is retrieved.

2. Prototype for program `GETACCDESC`:
   ```rpgle
          // Prototype declaration for program GetAccDesc
          dcl-pr GetAccDesc extpgm;
              P_AccNo char(10) const;   
              P_AccDesc char(50); 
          end-pr;
   ```
    - `dcl-pr` declares the `GetAccDesc` prototype for program `GETACCDESC`.
    - `P_AccNo` is a constant input parameter for the `GetAccDesc` procedure. It represents the account number for which the corresponding account description is retrieved.
    - `P_AccDesc' is 50-character output variable that receives the account description.

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
