##写一个a+b py函数与C++的调用

```C++
#include <Python.h>
#include <iostream>
#pragma comment(lib, "python36.lib")
using namespace std;
int main(int argc, char *argv[])
{
	Py_SetProgramName(L"asda");
	Py_Initialize();

	//PyRun_SimpleString("import sys");
	//PyRun_SimpleString("sys.path.append('./')");
	//PyImport_Import
	PyObject *f = NULL;
	PyObject *arg = NULL;
	PyObject *pN = NULL;
	PyObject *re;

	pN = PyImport_ImportModule("Gay");
	//PyObject *e = PyBytes_FromString("Gay");
	//pN = PyImport_Import(e);
	if (!pN)
	{
		cout << "error" << endl;
	}
	if (!(f = PyObject_GetAttrString(pN, "gay")))
	{
		cout << "error2" << endl;
	}
	arg = Py_BuildValue("ii", 1, 2);

	re = PyObject_CallObject(f, arg);	
	//re = PyObject_CallFunction(f, "ii", 1, 23);
	int a=0;
	PyArg_Parse(re, "i", &a)

	Py_Finalize();
	return 0;
}

```

**Py_SetProgramName** ：我也不知道有什么用

**Py_Initialize()**：必要的初始化

**PyRun_SimpleString**：这两句是为了修改工作路径，在这里可有可无

网上很多是使用 **pN = PyImport_Import(PyString_FromString("Gay"))** 加载模块

然后我发现 **PyString_FromString** py3.x变成了 **PyBytes_FromString**

不过改了名以后我依然怎么样都加载不到模块，最后换成了一句 **PyImport_ImportModule("Gay");** 就可以了，搞不懂0.0

**PyObject_GetAttrString**从模块中获取属性，这里的属性gay是个函数:)

**Py_BuildValue** 可以很方便地把任意c类型转为py类型，类型转换有很多方法，我觉得这种最简单

**PyObject_CallObject(f, arg)** 回调Object，名字+传递参数，返回值就是py回调对象后的返回值，（但由于返回值是PyObject也就是py类型）所以最后一步还有一个转换

**PyArg_Parse** 可以理解为 **Py_BuildValue**的逆函数，也是我觉得最简单的py类型转换为c类型的方法

呐，Gay.py了解一下

```py
def gay(a,b):
	return a+b
```

####哈哈，很简单的例子