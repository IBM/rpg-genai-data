example Copybook `getactinfcpy.rpgleinc` for procedure declarations: `GetAccNo` and `GetAccDesc`

```rpgle
// Procedure declaration for GetAccNo
dcl-pr GetAccNo char(10);
    P_UserId char(10) const;   
end-pr;

// Procedure declaration for GetAccDesc
dcl-pr GetAccDesc char(50);
    P_AccNo char(10) const;    
end-pr;
```


## how_output
The copybook `getactinfcpy` contains the following declarations:

1. Procedure Declaration for `GetAccNo`:
    - `dcl-pr GetAccNo char(10);`
        - This declares the `GetAccNo` procedure, which returns a 10-character account number.
    - `P_UserId char(10) const;`
        - This is a constant input parameter for the `GetAccNo` procedure. It represents the user ID for which the corresponding account number is retrieved.
    - `end-pr;`
        - Marks the end of the `GetAccNo` procedure declaration.

2. Procedure Declaration for `GetAccDesc`:
    - `dcl-pr GetAccDesc char(50);`
        - This declares the `GetAccDesc` procedure, which returns a 50-character account description.
    - `P_AccNo char(10) const;`
        - This is a constant input parameter for the `GetAccDesc` procedure. It represents the account number for which the corresponding account description is retrieved.
    - `end-pr;`
        - Marks the end of the `GetAccDesc` procedure declaration.

## sum_output

### Summary
The `getactinfcpy`serves as a reusable piece of code that contains procedure declarations for two key functions, `GetAccNo` and `GetAccDesc`. It allows these procedure definitions to be used across different RPGLE programs without having to redefine them each time, making the code more modular, efficient, and maintainable. The procedures are designed to handle account retrieval based on user IDs and account numbers, respectively.


## metadata

```yaml
difficulty: 2
language: rpg4ff
scope: copybook
use: eval
```

Full description of all the metadata variants can be found [here](/pages/metadata.md).