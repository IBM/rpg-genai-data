#### How to explain parameters and return value

See [How to explain data types](/pages/explain_definitions.md) for detailed instructions for the Type and possible Attributes columns for the parameters and return value.

#### Parameters

The parameters are described in a table with columns for the parameter number, `Data Type`, `Usage`, `Description`, and possibly `Attributes`.

The column for the parameter number does not have a heading.

If it is an unnamed parameter in a prototype, put `not named` in the name column. Also, add a point in the "Possible Improvements to this Code" that it's a good idea to give names to the parameters in a prototype

Example:
```rpgle
     D MyProc          pi              n
     D   CustomerId                  10a   const options(*omit)
     D   info                              likeds(info_t) const
     D   OrderCount                  10i 0 options(*nopass)
```

|   | Name          | Usage        | Data Type      | Description                                         | Attributes                |
|---|---------------|--------------|----------------|-----------------------------------------------------|---------------------------|
| 1 | `CustomerID`  | Input        | char(10)       | Unique identifier for the customer                  | *OMIT                     |
| 2 | `info`        | Input        | Data structure, LIKEDS(info_t) | Data structure array containing order details       | DIM(10) CONST             |
|   | `info.number` |              | int(10)        | Number of orders processed in the current session   |                           |
|   | `info.name`   |              | varchar(20)       | Name of the customer associated with the order   |                           |
| 3 | `OrderCount`  | Output       | int(5)         | Total number of orders processed                    | *NOPASS                   |

For this example, assume that the following data structure is defined in an include file in the context.

```rpgle
     D info_t          ds                  template qualified
     D   number                      10i 0
     D   name                        20a   varying
```

#### Return value

The return value has columsn for `Data Type`, `Description`, and posslby `Attributes`

| Data Type      | Description                                         |
|----------------|-----------------------------------------------------|
| ind       | It returns `*ON` to indicate success and `*OFF` to indicate failure|
