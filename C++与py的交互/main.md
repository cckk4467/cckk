
#

include那些就不用写了吧，先写些基础理论概念qwq
首先
>Most Python/C API functions have one or more arguments as well as a return value of type PyObject* ”
除了个别特殊情况，在C代码里py类型的存在形式就是PyObject*

PyObject* 有两个属性：type(类型)、reference(引用)

>type:"An object’s type determines what kind of object it is"

>e.g. integer类型，list类型，dict类型等等

>e.g.如用PyList_Check(PyObject*  a)若为true则a为list类型

>reference: "it counts how many different places there are that have a reference to an object."


https://docs.python.org/3/c-api/intro.html#objects-types-and-reference-counts