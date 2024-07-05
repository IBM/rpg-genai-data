# Generate Unit Tests for exist RPG

Given existing RPG, generate unit tests for it.
This will be a high value scenario for the RPG AI.  Since very few IBM i shops have unit tests, this should help them get to better quality and have more confidence to modernize their code bases.

There are a number of questions for us to consider in this scenario.

1. What test frameword if any should we target.
2. What scope of testing should we target.  i.e. subprocedure, program, subroutine or  service program

Initially we need to focus on unit tests for a given subprocedure or program

These should be stored under the task type directory `unittest`.
The directory structure will be

* unittest
  * {contributor_id}
    * attribution.txt - describe contributor
    * {unique_test_id}
      * input.txt
      * c1_tested.rpgle
      * output.rpgle
      * metadata.txt - difficulty

The `input.txt` will have the query to generate unit tests and for what scope and for what framework.

The `c1_rpgsource.rpgle` file should have the context of the code we want unit tested.  Additional SQL and DDS files to provide field definitions can be suppllied as `c2_???.table` etc.

Finally the `output.rpgle` can have the generated unit test.
