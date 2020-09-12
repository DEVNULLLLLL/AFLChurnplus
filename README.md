# source
Based on [american fuzzy lop](https://github.com/google/AFL), which is originally developed by Michal Zalewski <lcamtuf@google.com>.

# this project

fuzzing with change-burst info.

## version
Linux 18.04, 64-bit system. 

LLVM 7.0.1


## Install

   
### install aflchurn
We have two schemes of burst, one is the age of lines and the other is the number of changes of lines. 
We can choose one of the schemes or both of them.

- `export BURST_COMMAND_AGE=1` enables the age of lines using git command.
- `export BURST_COMMAND_CHURN=1` enables the number of changes of lines using git command.

Install

    cd /path/to/root/afl/dir
    make clean all
    cd llvm_mode
    make clean all

`LIBGIT_INC` and `LIBGIT_LIB` are used in llvm_mode/Makefile.


### About configure

    export BURST_COMMAND_AGE=1
    export BURST_COMMAND_CHURN=1
    CC=/path/to/aflchurn/afl-clang-fast ./configure [...options...]
    make

Be sure to also include CXX set to afl-clang-fast++ for C++ code.

### configure the time period to record churns

    export BURST_SINCE_MONTHS=num_months

e.g., `export BURST_SINCE_MONTHS=6` indicates recording changes in the recent 6 months

## run fuzzing

    afl-fuzz -i <input_dir> -o <out_dir> -p anneal -- <file_path> [...parameters...]

### option -p
power schedule

    -p anneal
    -p average

### option -b
Choose "age", "churn" or both. Default: both

    -b age
    -b churn