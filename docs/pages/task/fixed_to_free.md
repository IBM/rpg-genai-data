# Convert Fixed Format RPG to Fully Free RPG

This is not something we expect to use the AI for as there are excellent deterministic tools to do so.  But we feel that this is an excellent way to train the AI.

## Format

* fixed-to-free
  * {unique contributor id}
    * attribution.txt
    * {unique test id}
      * i1_input.rpgle - Old fixed format RPG
      * output.rpgle - New free format RPG
      * metadata.md - difficulty, language, task

The `i1_input.rpgle` contains the old fixed format RPG to be transformed.

The `output.rpgle` has the equivale fully free format RPG.

and `metadata.txt` has

```yaml
difficulty: 3
language: rpg4fx
scope: file
use: train
```

Where

* `difficulty` - the difficulty of the explanation as rated up to 5.  Since this is trivial we rate it as 0
* `language` - the language variant which for this task will always be rpg4fx
* `scope` - the scope of the language being explained in this case `file` i.e. the source file, see full description of metadata for other scopes to transform
* `use` - `train` means the data is used for training the LLM, the alternative would be `eval` if this was being used to evaluate an LLM

Full description of all the metadata variants can be found [here](/pages/metadata.md).
