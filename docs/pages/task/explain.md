# Explain existing RPG in Natural Language

A common problem for IBM i customers is coming across old code that is not easy to understand. The goal of this task is to train the AI to be able to produce a natural language description of some RPG.
In this scenario the question is in format of RPG code and the output is an explanation of that code.

Since the task is to "explain" some RPG, all of the following data is found in the `data/explain` directory.
For this example we will have a training pair submitted by IBM, so the data will be in the `data/explain/IBM` directory.

The expected explanations are found in the `output` subdirectory and the `metadata.txt` file describes
the attributes of this scenario.

See detailed explanations in the respective linke for explaining an [entire source file](/pages/task/explain_compilable_source.md) or a [specific subset of the source file like a procedure, subroutine](/pages/task/explain_source_references.md) or a [selected lines](/pages/task/explain_lines.md)

In either case when the data is commited to Github, an action is automatically performed that summarizes the training pairs into `jsonl` lines in a file in `data/explain/<contributor>` and then rolls that up into `data/explain` and finally for all training types in `data`.
The pairs can either be used to train the LLM or to evaluate how well the LLM is doing at generating explanations.  The `use` attribute of the metadata.txt determines what the data will be used for.
For `use: train` the data will be stored in `train_rpgle_to_text.md` and for `use: eval` the data will be stored in `eval_rpgle_to_text.md`.

In either case the JSONL will contain data, where each line is of the format:

```json
{
 "id":"unique_directory_name_depth",
 "input_data":"rpg source being explained",
 "context":"surrounding context needed to understand the code being explained",
 "output":"explanation of the code in markdown format",
 "metadata":"attributes of this explanation"
}
```

## Notes

- Use `rpgle` for ILE RPG code blocks and `rpg` for OPM RPG code blocks.
- Use `###` for first-level headings and `####` for sub-level headings.
- Avoid using `**` for bold text use backticks ` ` to highlight quotations from the RPG source, such as variables and RPG syntax.
- Use the term `fully-free` instead of `full free format`.
- Use `ILE RPG` instead of `RPGLE`.
- Do not expand `RPG` as `Report Program Generator`. Always refer to it as `RPG`.
- Data types should follow the format:  
  - `Character, length 10`  
  - `Packed numeric, length 7, with 2 decimals`
- All `free-for` code snippets must be indented by `at least 7 spaces`. 
- All `fixed-for` code snippets must be indented by `at least 5 spaces`.
- If the \`\`\`rpgle to start the snippet is indented, the 5 or 7 spaces must start at the same indentation as the \`\`\`rpgle line.
