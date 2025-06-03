#### Global Definitions
This section includes all global elements used in the module, such as variables, data structures, constants. It should be organized into the following subsections for clarity

See [How to explain data types](/pages/explain_definitions.md) for detailed instructions for the Type and possible Attributes columns for variable, subfields, and parameters.

##### Variables 
This section documents all variables used in the program, module, or procedure. It should be described in tabular format with columns for Name, Data Type, Description, and Attributes.
Only include the Attributes column if necessary.

Example: Variables
| Name           | Data Type               | Description                                 | Attributes         |
|----------------|------------------------ |---------------------------------------------|--------------------|
| `customerId`   | char(10)                | The unique identifier for the customer      |                    |

##### Data Structures

This section documents all data structures used in the module, program or procedure, including their purposes, and any notable characteristics such as whether the structure is a `Program Status Data Structure`, or a `qualified data structure. The subfields of each data structure should be described in a table format with columns for `Subfield Name`, `Type`, `Description`, and possibly `Attributes`. 

Qualified data structures should have the subfields listed in the fully-qualified form. For example, if qualified data structure `DS` has a subfield `SUBF1`, then the subfield should be listed as `ds.subf1`.

Here are some examples of data structures

##### Data structure `OrderHeader`  

| Subfield mame           | Data Type               | Description                                  | Attributes |
|----------------|------------------------ |----------------------------------------------|------------|
| `OrderID`      | char(10)                | Unique identifier for the order              |            |
| `OrderDate`    | date(*ISO)              | Date when the order was placed               |            |
| `TotalAmount`  | packed(9:2), 5 bytes    | Total amount for the order                   |            |
| `Quantities`   | int(5), 4 bytes         | Quantity of each item in the order           | DIM(50)    |

##### Program Status Data Structure `myPsds`
The Program Status Data Structure (PSDS) is a special data structure in RPG that provides information about the program's execution status, such as the program name, user profile, job information, error messages, and other runtime details. It is automatically updated by the system and can be used for error handling, auditing, and diagnostics.

| Name            | Data Type     | Description                                         |
|-----------------|--------------|-----------------------------------------------------|
| `PgmName`       | char(10)     | Name of the program currently running               |
| `UserProfile`   | char(10)     | User profile under which the job is running         |
| `JobName`       | char(10)     | Name of the job                                     |
| `JobNumber`     | char(6)      | Number of the job                                   |
| `JobUser`       | char(10)     | User name associated with the job                   |
| `Date`          | char(6)      | Date the job started (in job date format)           |
| `Time`          | char(6)      | Time the job started (in job time format)           |
| `LastError`     | char(7)      | Last error message ID encountered by the program    |
| `StatusCode`    | char(5)      | Status code of the last operation                   |
| `FileName`      | char(10)     | Name of the file in error (if applicable)           |
| `ExceptionId`   | char(4)      | Exception/error code                                |


##### Qualified data structure `myQualifiedDs`

The following qualified data structure is an array with 10 elements, used to store order header information. Each element contains details about an order, including items, requirements, and address information.

```rpgle
dcl-ds order qualified dim(10);
   num_items int(10);
   dcl-ds items dim(100);
      id char(10);
      price packed(7 : 2);
      quantity int(10);
      num_requirements int(10);
      dcl-ds requirements dim(3);
         type char(10);
         detail varchar(100);
      end-ds;
   end-ds;
   dcl-ds address;
      street varchar(30);
      city varchar(30);
   end-ds;
end-ds;
```
###### Subfields

| Subfield Name         | Data Type            | Description                          | Attributes        |
|--------------|----------------------|-----------------------------------------------|--------------|
| `num_items`  | int(10), 4 bytes     | The number of elements in the `items` array   | |
| `items`      | Data structure       | Array of items                     | DIM(10) |
| `items.id`               | char(10)              | The identifier                                   |   |
| `items.price`            | packed(7:2)           | The price                                        |   |
| `items.quantity`         | int(10), 4 bytes      | The number of items                              |   |
| `items.num_requirements` | int(10), 4 bytes      | The number of requirements for this item         |   |
| `items.requirements`     | Data structure        | Array of requirements                   | DIM(3) |
| `items.requirements.type`     | char(10)      | The requirement type                          | |
| `items.requirements.detail`   | varchar(100)  | Detailed information about the requirement     | |
| `address`    | Data structure       | The address for the order, see below          | |
| `items.address.street`  | varchar(30)  | The street, presumably including the number | |
| `items.address.city`    | varchar(30)  | The city                                   | |

##### Constants
This section documents all constants used in the module, including their values and purposes.
Example: 
| Constant Name       | Value | Description                              |
|---------------------|-------|------------------------------------------|
| `MAX_ORDERS`        | 9999  | Maximum number of orders allowed         |
| `DEFAULT_CURRENCY`  | 'USD' | Default currency for orders              |
