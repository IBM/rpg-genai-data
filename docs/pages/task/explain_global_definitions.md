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
| `monthlyOrders`| int(10), 4 bytes       | Array of the number of orders for each month| DIM(12)            |

##### Data Structures

This section documents all data structures used in the module, program or procedure, including their purposes, and any notable characteristics such as whether the structure is a `Program Status Data Structure`, or a `qualified data structure`. The subfields of each data structure should be described in a table format with columns for `Subfield Name`, `Type`, `Description`, and possibly `Attributes`. 

Qualified data structures should have the subfields listed in the fully-qualified form. For example, if qualified data structure `DS` has a subfield `SUBF1`, then the subfield should be listed as `ds.subf1`.

See [How to explain data structures](/pages/explain_definitions.md) for examples of data structure explanations.

##### Constants
This section documents all constants used in the module, including their values and purposes.
Example: 
| Constant Name       | Value | Description                              |
|---------------------|-------|------------------------------------------|
| `MAX_ORDERS`        | 9999  | Maximum number of orders allowed         |
| `DEFAULT_CURRENCY`  | 'USD' | Default currency for orders              |
