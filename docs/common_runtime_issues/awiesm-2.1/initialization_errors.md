# Initialization Errors

These category of errors happen immediately at the model startup, before anything has been calculated.

## Error while loading shared libraries 

In this case, some module files are loaded differently between compiletime and
runtime, leading to an inconsistent environment. The `echam6` binary here
complains, with an error message that may take the following form:

````{error}
```
77: /work/ollie/pgierz/SciComp/Basic_Runs/experiments/pre-industrial/run_20000101-20000131/work/./echam6: error while loading shared libraries: libsupport.so.0: cannot open shared object file: No such file or directory
```
````

Note that in the example above, the `77:` denotes the CPU number which is
giving the error message, but this message will pop up for all CPUs running
ECHAM tasks.

