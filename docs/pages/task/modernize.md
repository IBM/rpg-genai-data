# Generate a more modern version of RPG

This is the most valuable type of training.  There are many different types of modernization.  

- Converting from subroutines to subprocedures
- Converting from monolithic programs to modules and service programs
- Converting from record level access to embedded SQL
- Converting from program described files to externally described files
- Converting from RPG III to RPG IV, even just the tricky part about program described data layouts
- Converting from RPG II to RPG IV

## Format

- modernize
  - {unique contributor id}
    - attribution.txt
    - {unique test id}
      - i1_old.rpgle - the RPG you want to modernize
      - c1_db.rpgle - any files needed for context to this question
      - output.rpgle - Provide modernized code
      - metadata.md - difficulty, language, transform, task

The `i1_rpgsource.rpgle` file should have the code we want modernized.  

Additional SQL and DDS files to provide field definitions can be suppllied as `c1_???.table` etc.

Finally the `output.rpgle` can have the modernized version of the RPG.

Full description of the metadata can be found [here](/pages/metadata.md).
