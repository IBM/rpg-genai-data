# Generate RPGLE

In this scenario the question is in format of text requesting the generation of new RPG code and the output is the generated RPG.

All of the following data is found in the `prototyping` directory.

- data/
  - prototype/
    - compiler_tests
      - attribution.txt
      - get_src_strmf_name_qbnrmodi/
        - input.txt
        - output.rpgle
        - metadata.txt

First we need a directory for the organization.  In this cases these are questions derived from the RPG compiler tests and contributed by Barbara Morris.  We will give it the unique id `compiler_tests` and that becomes the second level directory.
It contains an [`attribution.txt`](attribution.md) of

```yaml
title : ILE RPG Compiler Test Cases
link : https://github.com/edmundreinhardt/rpg-genai-data/tree/main/src/001compiler_tests/
contact : Barbara Morris
email : bmorris@ca.ibm.com
license : Apache 2.0
organization : IBM
version : https://github.com/edmundreinhardt/rpg-genai-data/commit/d41c5d45a58653d7d12958be6c2b739cb5d7e902
```

Now if I had not placed the source in a github repository, we could drop the link and commit/tag version as long we have a  contact to track down the source.  The version in this example is sufficient for the contact to identify the origin of the source.  For example:

```yaml
title : ILE RPG Compiler Test Cases
contact : Barbara Morris
email : bmorris@ca.ibm.com
license : Apache 2.0
organization : IBM
version : V7R4M0 test bucket 
```

The specific id of the particular question is `get_src_strmf_name_qbnrmodi`
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
language: rpg4ff
scope: proc
use: train
```

This produces the jsonl for training.

```json
{"id": "get_src_strmf_name_qbnrmodi", "input_data": "Generate an ILE RPG subprocedure that will return the contents of a stream file into a varying string of length 1000, given the library and module name that the stream file was compiled into.", "output": "**free\n...", "task": "prototyping", "difficulty": 3}
```
