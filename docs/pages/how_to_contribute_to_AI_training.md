# How to contribute to training the new RPG Generative AI

## Training Data

Unfortunately we are unable to use plain RPG source to finetune the AI directly. We hope to be able to use plain source in later stages.

What would be most useful therefore is to contribute question and answer pairs.

The format used to train the AI is JSONL.  This is the file format where each line is a separate JSON with the format:

```json
{"id": "<UNIQUE_ID>", "input":”<QUESTION>”, "context":"<CONTEXT>", ”output”:”<ANSWER>”, "task":"<TASK>", "difficulty" : "<DIFFICULTY>"}
```

Where

- *UNIQUE_ID*
unique alphanumeric string with no blanks
- *QUESTION*
the request being made to the LLM
- *CONTEXT*
files and definitions that the question is dependent on
- *ANSWER*
the response that the LLM *should* provide
- *TASK_TYPE*
What type of task is being attempted: explain/prototyping/fixed-to-free
- *DIFFICULTY*
Difficulty of question on a scale from 0 to 5 where 5 is the most difficult and 0 is trival

These JSONL files will be found in the [AIforIBMi repo](https://github.com/AIforIBMi/rpg-genai-data) under
```/data/<task_name>/<your_directory>```.
Please add to this directory a file `attribution.txt` with content of the form:

```yaml
title : ILE RPG Compiler Test Cases
link : https://github.com/edmundreinhardt/rpg-genai-data/tree/main/src/001compiler_tests/
creator : Barbara Morris
license : Apache 2.0
organization : IBM
version : https://github.com/edmundreinhardt/rpg-genai-data/commit/d41c5d45a58653d7d12958be6c2b739cb5d7e902
```

Since RPG source in `jsonl` will be difficult to read as it all has to be on one line, we have an easier format to submit your training data.  Use the following data structure:

- data/
  - <task_name>/
    - <your_org>/
      - any_id1
        - input.txt
        - i1_file1.rpgle
        - c1_MYDSPF.DSPF
        - c2_MYTABLE.TABLE
        - output.txt
        - o1_somefile.rpgle
        - metadata.txt

The above directory structure will automatically be transformed by IBM into a `jsonl` line of the form:

```json
{"id": "any_id1", "input":”<input.txt contents>\n\nfile1.rpgle\n<i1_file1.rpgle contents>”, "context”: "MYDSPF.DSPF<c1_MYDSPF.DSPF contents>\n\nMYTABLE.TABLE\n<i3_MYTABLE.TABLE contents>", ”output”: ”<output.txt contents>\n\nsomefile.rpgle\no1_somefile.rpgle contents”, "task":"<task_name>", "difficulty":0}
```

Note that the `i<number>_` prefix is used to impose an order in which the input files show up in the question.  The `c<number>_` prefix similarly orders the files that are needed to give the context for the question. Finally the `o<number>_` prefix similarly orders the output files that are embedded in the answer.  The `output.txt` could also be `output.rpgle` if the output only contains code.  The `metadata.txt` in this case contains the difficulty level.

```yaml
 difficulty: 0
```

### Use cases and examples

The scenarios we are initially focused on are:

#### 1) EXPLAIN

In this scenario the question is in format of RPG code and the output is an explanation of that code.

Since the task is to explain some RPG, all of the following data is found in the `data/explain` directory.
For this example we will have a training pair submitted by IBM, so the data will be in the `data/explain/IBM` directory.

![alt text](../media/explain_structure.png)
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

#### 2. Generate RPGLE

In this scenario the question is in format of text requesting the generation of new RPG code and the output is the generated RPG.

All of the following data is found in the `prototyping` directory.

![alt text](../media/proto_structure.png)

First we need a directory for the organization.  In this cases these are questions derived from the RPG compiler tests and contributed by Barbara Morris.  We will give it the unique id `001compiler_tests` and that becomes the second level directory.
It contains an `attribution.txt` of

```yaml
title : ILE RPG Compiler Test Cases
link : https://github.com/edmundreinhardt/rpg-genai-data/tree/main/src/001compiler_tests/
contact : Barbara Morris
email : bmorris@ca.ibm.com
license : Apache 2.0
organization : IBM
version : https://github.com/edmundreinhardt/rpg-genai-data/commit/d41c5d45a58653d7d12958be6c2b739cb5d7e902
```

Now if I had not place the source in a github repository, we could drop the link as long we can contact the own to track down the source.  For example:

```yaml
title : ILE RPG Compiler Test Cases
contact : Barbara Morris
email : bmorris@ca.ibm.com
license : Apache 2.0
organization : IBM
version : V7R4M0 test bucket 
```

The specific id of the particular question is `001_get_src_strmf_name_qbnrmodi` 
The `input.txt` contains the question

```text
Generate an ILE RPG subprocedure that will return the name of a stream file into a varying string of length 1000, given the library and module name that the stream file was compiled into.
```

We use the file extension '.rpgle' on `output.rpgle` since it will just be containing code.
It contains the generated function.

```rpgle
**free
//
// Generate an ILE RPG subprocedure that will return the name of a stream file 
// that a given module was compiled into.
// Uses the @link (https://www.ibm.com/docs/en/i/7.2?topic=ssw_ibm_i_72/apis/qbnrmodi.html)[QBNRMODI] system API.
// @param module - char(10) input - name of the module whose source we are retrieving 
// @param library - char(10) input - name of the library into which the module was compiled 
// @returns varchar(1000) the fully qualified name of the stream file from which the module was compiled
//   or the empty string if the module was not compile from a stream file
//
dcl-proc getStmfSourceName;
   dcl-pi *n varchar(1000);
      module char(10) const;
      library char(10) const;
   end-pi;
   dcl-c MODI0100_FORMAT 'MODI0100'; // Basic module information.
   // prototype for Retrieve Module Information API
   dcl-pr QBNRMODI extpgm;
      rcvr likeds(rcvr);
      rcvr_len int(10) const;
      format char(8) const;
      qualname likeds(qualobj) const;
      errcode likeds(errcode);
   end-pr;
   // Data structure to received information from the API
   dcl-ds rcvr len(5000) qualified;
      offset_to_srcstmf int(10) pos(289);
      length_of_srcstmf int(10) pos(293);
      ccsid_of_srcstmf int(10) pos(297);
   end-ds;
   // API Error Feedback structure
   dcl-ds errcode qualified;
      bytes_provided int(10) inz(0); // default to give exception
      bytes_available int(10);
      *n char(1);
      msgid char(7);
      otherData char(1000);
   end-ds;
   // Input qualified module name
   dcl-ds qualobj qualified;
      obj char(10) inz;
      lib char(10) inz('QTEMP');
   end-ds;
   // the stream file name to be returned
   dcl-s srcstmf varchar(1000);
   // Marshal input parameters to API call
   qualobj.obj = module;
   qualobj.lib = library;
   errcode.bytes_provided = 0;
   // Call API to retrieve information about the module
   qbnrmodi (rcvr : %size(rcvr) : MODI0100_FORMAT : qualobj : errcode);
   // Move the name of the source stmf into the return value
   srcstmf = %subst(rcvr
                  : rcvr.offset_to_srcstmf + 1
                  : rcvr.length_of_srcstmf);
   return srcstmf;
end-proc;
```

The difficulty is set via `metadata.txt`

```yaml
difficulty: 3
```

This produces the jsonl for training.

```json
{"file_id": "001retrieve_streamf_contents", "input": "Generate an ILE RPG subprocedure that will return the contents of a stream file into a varying string of length 1000, given the library and module name that the stream file was compiled into.", "output": "**free\n...", "task": "prototyping", "difficulty": 3}
```
