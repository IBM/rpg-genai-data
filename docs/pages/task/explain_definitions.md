#### How to explain data types

When variables, parameters, and subfields are defined, a table is used with various columns including one or two columns for the data type.

The data type columns are `Data Type` and `Attributes`.

- The data type is described using the free-format rpgle syntax. 
- If there are other keywords such as DIM or CCSID that affect the data type, they go into the Attributes column. If there is a DIM keyword, there would also be something in the Description column to say that it is an array.
  - Don't include the Attributes column if it would always be blank for all the rows
  - For fixed-form definitions, some keywords do not go into the Attributes column but instead affect what goes into the Data Type column. See the `Fixed-form keywords that affect the data type` section below for the keywords that affect the free-form data type

##### Data Type

- char(10)
- varchar(10)
- date(*iso)
- packed(5:2), 3 bytes
- ...

##### For numeric types other than float, the number of bytes is also added

For example
- int(10), 4 bytes
- uns(5), 2 bytes
- packed(8:2), 5 bytes
- zoned(7:2), 7 bytes
- bindec(1:0), 2 bytes

##### Algorithms to determine the number of bytes for numeric definitions

| Types | Algorithm |
|------|-----------|
| int and uns | 3 digits = 1 byte, 5 digits = 2 bytes, 10 digits = 4 bytes, 20 digits = 8 bytes |
| packed | If the number of digits is odd: (digits + 1) / 2. If the number of digits is even: (digits + 2) / 2
| zoned | The number of bytes is equal to the number of digits |
| bindec | For 1 to 4 digits, the number of bytes is 2. For 5 - 9 digits, the number of bytes is 4. For 18 digits, the number of bytes is 8. |

##### Fixed-form keywords that affect the data type

**Note** : These fixed-form-only keywords **do not** go into the `Attributes` column.

| Fixed-form type | Keyword | Free-form type |
|------|-------|------|
| 10A | VARYING | varchar(10) |
| 10C | VARYING | varucs2(10) |
| 10G | VARYING | vargraph(10) |
| * |   | pointer |
| * | PROCPTR | pointer(*proc) |
| O | CLASS(*java ...) | object(*java ... ) |
| D | DATFMT(fmt) | date(fmt) |
| T | TIMFMT(fmt) | time(fmt) |
