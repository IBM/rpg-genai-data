#### Golbal Definitions
This section includes all global elements used in the module, such as variables, data structures, constants. It should be organized into the following subsections for clarity

##### Variables 
This section documents all variables used in the program, module, or procedure. It should be described in tabular format with columns for Name, Data Type, Description, and Attributes.

Example: Variables
| Name           | Data Type               | Description                                 | Attributes         |
|----------------|------------------------ |---------------------------------------------|--------------------|
| `customerId`   | char(10)                | The unique identifier for the customer      |                    |
| `orderTotal`   | packed(9:2), 5 bytes    | The total amount of the order calculated    | INZ(0)             |
| `currencyCode` | char(3)                 | The currency in which the order is placed   | CONST              |
| `taxRate`      | zoned(5:2), 5 bytes     | The applicable tax rate for the order       | INZ(5.00)          |
| `orderDate`    | date(*ISO)              | The date the order was placed               |                    |
| `status`       | char(1)                 | Status of the order                         | INZ('A')           |
| `totals`       | packed(7:2)             | Array of totals                             | DIM(12)            |
| `config`       | char(20)                | Configuration value                         |                    |
| `notes`        | varchar(100)            | Optional notes                              | VARYING            |
| `discount`     | packed(5:2)             | Discount applied to the order               | INZ(0)             |
| `customerId`   | bindec(4:2), 2 bytes    | The unique identifier for the customer      |                    |

##### Data Structures

This section documents all data structures used in the module, program or procedure, including their subfields, purposes, and any notable characteristics such as whether the structure is `Program Status`, or `Qualified Data Structures`. Each data structure should be described in a table format with columns for Subfield Name, Type, and Description.

##### Genral Data Structures

Its general data structures that are used in the module, program or procedure.  Each data structure should be described with its subfields and their types.

Example: `OrderHeader`  
| Name           | Data Type               | Description                                  | Attributes |
|----------------|------------------------ |----------------------------------------------|------------|
| `OrderID`      | char(10)                | Unique identifier for the order              |            |
| `OrderDate`    | date(*ISO)              | Date when the order was placed               |            |
| `TotalAmount`  | packed(9:2), 5 bytes    | Total amount for the order                   |            |
| `Quantities`   | int(5), 4 bytes         | Quantity of each item in the order           | DIM(50)    |

##### Program Status Data Structure
The Program Status Data Structure (PSDS) is a special data structure in RPG that provides information about the program's execution status, such as the program name, user profile, job information, error messages, and other runtime details. It is automatically updated by the system and can be used for error handling, auditing, and diagnostics.

Example: 

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


##### Qualified Data Structures

This section documents all qualified data structures used in the module, program, or procedure.

The following qualified data structure is an array with 10 elements, used to store order header information. Each element contains details about an order, including items, requirements, and address information.

```rpgle
dcl-ds order qualified;
   num_items int(10);
   dcl-ds items dim(100);
      id char(10);
      price packed(7 : 2);
      quantity int(10);
      num_requirements int(10);
      dcl-ds requirements dim(10);
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
- Data structure `order`

| Name         | Data Type            | Description                                   |
|--------------|----------------------|-----------------------------------------------|
| `num_items`  | int(10), 4 bytes     | The number of elements in the `items` array   |
| `items`      | Data structure       | Array of items, see below                     |
| `address`    | Data structure       | The address for the order, see below          |

- Sub data structure `items`
  | Name               | Data Type             | Description                                         |
  |--------------------|---------------------- |-----------------------------------------------------|
  | `id`               | char(10)              | The identifier                                      |
  | `price`            | packed(7:2)           | The price                                           |
  | `quantity`         | int(10), 4 bytes      | The number of items                                 |
  | `num_requirements` | int(10), 4 bytes      | The number of requirements for this item            |
  | `requirements`     | Data structure        | Array of requirements, see below                    |

  - Sub data structure `requirements`
  | Name       | Data Type      | Description                                   |
  |------------|---------------|-----------------------------------------------|
  | `type`     | char(10)      | The requirement type                          |
  | `detail`   | varchar(100)  | Detailed information about the requirement     |

- Sub data structure `address` 
  | Name      | Data Type     | Description                                 |
  |-----------|--------------|---------------------------------------------|
  | `street`  | varchar(30)  | The street, presumably including the number |
  | `city`    | varchar(30)  | The city                                   |

##### Constants
This section documents all constants used in the module, including their values and purposes.
Example: 
| Constant Name       | Value | Description                              |
|---------------------|-------|------------------------------------------|
| `MAX_ORDERS`        | 9999  | Maximum number of orders allowed         |
| `DEFAULT_CURRENCY`  | 'USD' | Default currency for orders              |