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

* unittest
  * {contributor_id}
    * attribution.txt - describe contributor
    * {unique_test_id}
      * input.txt
      * i1_tested.rpgle
      * c1_data.table
      * output.rpgle
      * metadata.txt

The `input.txt` will have the query to generate unit tests and for what scope and for what framework. (Note that we will probably be replacing with an engineered prompt over time.)

The `i1_rpgsource.rpgle` file should have the code we want unit tested.  

Additional SQL and DDS files to provide field definitions can be suppllied as `c1_???.table` etc.

Finally the `output.rpgle` can have the generated unit test.

Themetadata will have `metadata.txt` has

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
