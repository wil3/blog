Using Tensorflow AoT Compilation without Bazel

Tensorflow uses Bazel for building, if you are trying to use Tensorflow as a
library there's little documentation on how to do this. [Tensorflow ahead-of-time
compilation](https://www.tensorflow.org/performance/xla/tfcompile) allows our graphs to be compiled into C++ code which is helpful for
installing in environments where resources are limited (e.g., embedded or
mobile). 

In this post we'll walk through how to compile a graph into C++ and include it
in a project built using make. This involves the following steps,

1) Produce a frozen graph 
2) (Optional) Optimize graph
3) Convert graph to C++ code using tfcompile
4) Build our executable 

tensorflow/compiler/tf2xla


# 1 Produce a frozen graph 
# 2 (Optional) Optimize graph
# 3 Convert graph to C++ code using tfcompile
# 4 Build our executable 

Originally through brute trial and error trying to get my graph to compile it appears the generated C++ code depends on the following objects, 
tensorflow/compiler/tf2xla/xla_compiled_cpu_function.o
tensorflow/compiler/aot/runtime.o 
tensorflow/compiler/xla/executable_run_options.o

For a quick hack, if your target platform is the same as the one you compiled
Tensorflow you can just build Tensorflow from source and link to the following
objects, 

bazel-bin/tensorflow/compiler/tf2xla/_objs/xla_compiled_cpu_function/tensorflow/compiler/tf2xla/xla_compiled_cpu_function.o 
bazel-bin/tensorflow/compiler/aot/_objs/runtime/tensorflow/compiler/aot/runtime.o
bazel-bin/tensorflow/compiler/xla/_objs/executable_run_options/tensorflow/compiler/xla/executable_run_options.o

However this isn't going to work if our target platform is different, for
example if we are compiling for arm. So lets look more closely how these objects
are compiled.

Lets start by look at the BUILD file under [tensorflow/compiler/tf2xla/](https://github.com/tensorflow/tensorflow/blob/master/tensorflow/compiler/tf2xla/BUILD#L94-L106) where we find the 
xla_compiled_cpu_function
