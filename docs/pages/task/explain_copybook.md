# Explanation format for an RPG copybook (include file)

The following explanation applies to RPG source files modules that will be copied or included into other source.
The [`scope`](/pages/metadata#scope) in `metadata.txt` will be `COPYBOOK` in this case.

## context
The `Context` section is not required for the copybook because copybooks in RPGLE generally consist of reusable code snippets, such as procedure declarations (dcl-pr), variable definitions, constants, and data structure declarations, which are included in the main program or subroutines. These code snippets themselves do not depend on external files or resources, but instead serve as building blocks to be referenced and used by other RPGLE programs or modules

## api_output
The `api_output` section is not applicable for this copybook. Copybooks are not directly callable entities like APIs. Instead, they are included in programs to define procedures that can be called within those programs. 

## how_output

The how_output part explains the details of the copybook covering all of the declarations in it.

## sum_output

The sum_output part offers a business summary in a couple of sentences, explaining the overall purpose of the copybook

## metadata.txt

The `metadata.txt` file describes important attributes of the training data.

Structure look like:

```yaml
difficulty: <rating>
language: <language>
scope: <scope>
use: <usage>
```

Full description of all the metadata variants can be found [here](/pages/metadata.md).

The sample explanation copybook can be found [here](/pages/task/sample_copybook.md).