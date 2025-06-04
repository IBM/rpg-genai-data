# Generate Unit Tests for Existing RPG

Given existing RPG, generate unit tests for it.
This will be a high value scenario for the RPG AI.  Since very few IBM i shops have unit tests, this should help them get to better quality and have more confidence to modernize their code bases.

There are a number of questions for us to consider in this scenario.

1. What test framework if any should we target.
2. What scope of testing should we target.  i.e. subprocedure, program, subroutine or  service program

Initially we need to focus on unit tests for a given subprocedure or program

## Format

These should be stored under the task type directory `unittest`.
The directory structure will be

```
{unique_test_id}
  ├── input/    # Canonical solution or problem statement
  ├── output/   # Generated code or solution
  └── test/     # Unit tests used to evaluate the generated code
```

## input.txt  
  This is the reference input used to generate the solution.

## output 
  Contains the `generated code` or `solution`.  
  This is the model's output based on the input.

## test 
  Contains the `unit tests` used to evaluate the generated code.  
  These tests are the `primary method of evaluation`.

The metadata will have `metadata.txt` has

```yaml
difficulty: 3
language: rpg4ff
scope: proc
use: train
framework: rpgunit
```

Where

* `difficulty` - the difficulty of the explanation as rated up to 5. 
* `language` - the language of the snippet of code being explained in this case `rpg4ff` which is RPG IV fully free
* `scope` - the scope of the language being tested in this case `proc` as we are generating a unit test for the given exported procedure.  Alternatively `file` can be used to test the entire program or module. i.e. every exported procedure in a module, or the entire program in the case of a program.
* `use` - `train` means the data is used for training the LLM, the alternative would be `eval` if this was being used to evaluate an LLM
* `framework` - which unit test framework is being targeted.  Supported frameworks are `rpgunit`, `ibmiunit` and `mdtest`

A full description of the metadata can be found [here](/pages/metadata.md).

The sample explanation unit test can be found [here](/pages/task/sample_unittest.md)