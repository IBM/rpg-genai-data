# Example Explanation for Unit Test Case – `GetAccNo`

## Directory Structure

```
/getaccno/
├── input/
│   └── GetAccNo.rpgle
├── output/
│   └── test_getaccno.rpgle
└── test/
    └── getaccno_unittest.rpgle
```

## input/

File: `GetAccNo.rpgle`

```rpgle
dcl-proc GetAccNo;
  dcl-pi GetAccNo char(10);
    P_UserId char(10) const;
  end-pi;

  dcl-s FoundAccNo char(10);  

  setll -loval AccPf;
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
```

## output/

File: `test_getaccno.rpgle`

The generated test cases should cover valid and invalid inputs, edge cases, and typical use cases. Ensure that the test cases are diverse enough to validate the functionality of the `GetAccNo` procedure thoroughly.

```rpgle
**free

dcl-proc test_getaccno;

    // Valid test cases
    assertEqual(GetAccNo('CUST001'), 'ACC1234567');
    assertEqual(GetAccNo('CUST002'), 'ACC9876543');
    assertEqual(GetAccNo('CUST003'), 'ACC0000001');

    // Invalid test cases
    assertEqual(GetAccNo('UNKNOWN'), ''); // Should return blank if user not found
    assertEqual(GetAccNo(''), '');        // Empty input should return blank

    // Edge cases
    assertEqual(GetAccNo('CUST999999'), ''); // Very high customer ID not in file
    assertEqual(GetAccNo('CUST001 '), '');   // Input with trailing space

end-proc;
```

This RPGLE code creates a test procedure named `test_getaccno` that validates the `GetAccNo` procedure using a variety of test cases. It includes:

- `Valid inputs` to confirm that known customer IDs return the correct account numbers.
- `Invalid inputs` to ensure the procedure handles unknown IDs gracefully.
- `Edge cases` to test robustness against unusual but possible inputs, such as trailing spaces or high-value IDs.

The `assertEqual` function is used to compare the expected and actual results of the `GetAccNo` procedure calls. If any of these assertions fail, it indicates that the procedure does not behave as expected under those conditions.

## test/

File: `getaccno_unittest.rpgle`

```rpgle
**free
ctl-opt dftactgrp(*no) actgrp('caller');

// Prototype
dcl-pr GetAccNo char(10);
  P_UserId char(10) const;
end-pr;

dcl-proc main;
  dcl-s result char(10);

  // Test 1: Known customer
  result = GetAccNo('CUST001');
  if result = 'ACC1234567';
    dsply('Test 1 Passed');
  else;
    dsply('Test 1 Failed');
  endif;

  // Test 2: Unknown customer
  result = GetAccNo('UNKNOWN');
  if result = '';
    dsply('Test 2 Passed');
  else;
    dsply('Test 2 Failed');
  endif;

end-proc;
```

## metadata

```yaml
difficulty: 3
language: rpg4ff
scope: proc
use: train
framework: rpgunit
```