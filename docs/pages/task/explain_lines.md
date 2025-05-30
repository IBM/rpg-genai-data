# Explanation Format for RPG Lines - Explanation

The following explanation applies to specific lines from RPG source files that are included or referenced in other programs.  
The [`scope`](/pages/metadata#scope) in `metadata.txt` will be `Line` in this case.

# Imporant:

- All the free-form snippets must be indented by at least 7 spaces before the code.
- All fixed-form snippets must have exactly 5 spaces before the specification type.
- If the rpgle to start the snippet is indented, the 5 or 7 spaces must start at the same indentation as the rpgle line.

Note: Input and Context folder are not required for explanations of lines.  This is because the `source`, `start` and `end` attributes will define exactly which source and context is referenced.

## api_output

For the `Line` scope, an API-level description is not applicable because lines of code cannot be called like a standalone procedure or program. So, the `api_output` section should not exist.

## how_output

The `how_output` explains what each of the selected lines does in a simple and technical way. It includes logic or any specific purpose the lines serve.


## sum_output

The `sum_output` gives a short business-level summary of what the selected lines do. It helps explain the purpose of the code in simple terms.

## metadata.txt

The `metadata.txt` file describes important attributes of the training data.

Structure look like:

```yaml
source: <source_member_name>
start: <start_line_number>
end: <end_line_number>
difficulty: <rating>
language: <language>
scope: <scope>
use: <usage>
```

Full description of all the metadata variants can be found [here](/pages/metadata.md).

The sample explanation lines can be found [here](/pages/task/sample_lines.md).