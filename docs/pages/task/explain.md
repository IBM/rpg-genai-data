# EXPLAIN

In this scenario the question is in format of RPG code and the output is an explanation of that code.

Since the task is to explain some RPG, all of the following data is found in the `data/explain` directory.
For this example we will have a training pair submitted by IBM, so the data will be in the `data/explain/IBM` directory.

![alt text](media/explain_structure.png)
The main file used to do the training is `train_text_to_rpgle.jsonl`

```json
{"file_id": "helloworld", "input":"Explain the following ILE RPG code\n\nhelloworld.rpgle\n**free;\ndsply ‘hello world’;\n
return;\n","output":"The fully free-form ILE RPG program displays the message ‘hello world’ to the user."}
```

If you used the directory method it would look like

- helloworld\
  - input.txt
  - i1_helloworld.rpgle
  - output.txt
  - metatdata.txt

where input.txt has the content:

```text
Explain the following ILE RPG code
```

and `i1_helloworld.rpgle` has the content:

```rpgle
**free
dsply 'hello world';
return;
```

and output.txt has the content:

```text
The fully free-form ILE RPG program displays the message 'hello world' to the user.
```

and `metadata.txt` has

```yaml
difficulty: 0
```
