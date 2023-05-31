---
title: "mrgsolve用户指南第二章：模型描述"
date: 2023-05-31T19:41:32+08:00
draft: false
typora-root-url: ../../../../static/

---

原文链接：[https://mrgsolve.org/user-guide/specification.html#seCLcode-blocks](https://mrgsolve.org/user-guide/specification.html#seCLcode-blocks)

原文作者：[Kyle Baron](https://github.com/kylebaron)

译者说：当希望将定量药理学**模型**以**可访问可交互**的方式面向非专业用户**公开发布**时，可以**免费商业使用**的非线性混合效应程序包就变得非常必要，因此本文翻译了[mrgsolve](https://mrgsolve.org/)——"一个可以实现非线性混合效应模型并可基于模型进行模拟的，基于R语言的，采用GPL开源协议的R添加包"的用户指南的第二章的内容，以使读者可以学习和了解此程序包的基本语法规则，以便未来需要时可以基于此程序包公开发布定量药理学模型的结果，提高模型的影响力。由于"定量药理学"相关中文的资料非常少，也希望此文能够拓展一些中文中关于"定量药理学"内容。本文所翻译第二章主要介绍了mrgsolve模型描述文件的语法，这些语法是非常重要的基础内容，其他更多的关于mrgsolve的使用说明可参见其[用户指南](https://mrgsolve.org/user-guide/)以及其[代码仓库](https://github.com/metrumresearchgroup/mrgsolve)。

标题：mrgsolve用户指南第二章：模型描述

# 2 模型指定

本章详细介绍了mrgsolve模型描述文件(model specification)【译者注：类似于NONMEM的控制文件，Phoenix的PML语言】的格式。

## 2.1如何编写一个模型

有两种方法可以编写模型：

1. 描述模型的代码储存在一个**单独的文件**，并将其载入到你的 R 脚本中
2. 描述模型的代码作为字符串文本**内联**(inline)在你的R 脚本中

我们建议对任何重要的建模工作使用方法1(单独的文件)。方法2对于快速编写模型代码非常方便，您还将看到我们在演示如何使用mrgsolve时经常使用该方法。

### 2.1.1 单独的文件

对于大多数应用，您需要将模型代码放在一个独立的文件中。此文件可以具有任何扩展名，但是当您使用 .cpp 扩展名时，存在一些特殊行为。我们将在这里向您展示"任意扩展名"或".cpp扩展名"这两者。

#### 2.1.1.1 使用".cpp"文件扩展名

打开文本编辑器，将模型代码键入到名称为"\<model-name\>.cpp"的文件中。此文件名格式标识模型的"名称"( "\<model-name\>.cpp" 字符中的"\<model-name\>"字符串可以为任意符合名称要求的字符串，这些字符在此处被视为是此模型的名称)。注意：整个文件将被读取和解析，因此其中的所有内容都必须是有效的"mrgsolve"模型描述元素。

使用 "mread()" 函数读取和解析此文件。对于保存在 "mymodel.cpp" (在当前工作目录中的"mymodel.cpp"文件) 中的名为 "mymodel" 的模型，发出以下命令：

```R
mod <- mread("mymodel")
```

"mread()" 返回一个模型对象，您可以使用该对象进行模拟。

#### 2.1.1.2 使用任意文件扩展名

您可以使用任意文件扩展名来保存"mrgsolve"模型。与其使用".cpp"，不如使用".mod"。使用".cpp"以外的扩展名时，传入完整的文件名，包括文件名和扩展名。如果你有模型代码保存在"mymodel.mod"中，那么你应使用以下代码

```R
mod <- mread("mymodel.mod")
```

以加载该模型。

#### 2.1.1.3 模型项目目录

"mread()"的第二个参数是"project"。当存在第二个参数时，"mrgsolve"将在第二个参数对应的目录中查找模型文件(模型文件名作为第一个参数传递给"mread()")。

如果你的"myproject.mod"文件位于"models"目录中，那么你可以使用以下代码

```R
mod <- mread("mymodel.mod", project = "models")
```

或者，您可以将整个路径传递到模型文件

### 2.1.2 内联/代码

通常，直接在"R"脚本中编写模型更方便。该模型可能如下所示：

```R
code <- '
$PARAM CL = 1, VC = 20
$PKModel ncmt=1
'
```

在这里，我们创建了一个长度为 1 的字符向量，并将其保存到名为"code"的"R"对象中。此对象的名称无关紧要。但是"code"将作为模型描述传递到"mrgsolve"中。当"mrgsolve"获得这样的模型以及模型的"名称"时，mrgsolve会将代码写入一个名为\<model-name\>.cpp的文件，并立即将其读回，就像您将代码键入该文件一样([第2.1.1节](https://mrgsolve.org/user-guide/specification.html#seCLspeCLseparate-file))。

要解析和加载此模型，请使用"mcode()"命令：

```R
mod <- mcode("mymodel", code)
```

"mcode()" 是 "mread()" 的方便包装器。"mcode"将代码写入"tempdir()"中的"mymodel.cpp"，将其读回，编译并加载。

"mcode"调用等效于：

```R
mod <- mread("mymodel", tempdir(), code)
```

有关帮助，请参阅?mread，在加载"mrgsolve"后?mread会包含进"R"的帮助系统中。

### 2.1.3 注释

您可以使用两个前斜杠注释掉模型描述文件中的代码//

```R
$MAIN

double a = 1.234;
// double b = 5.678;
```

此注释序列风格是C++代码中使用的，但您可以在 mrgsolve 模型描述文件中的任何位置使用它。

## 2.2 代码块

### 2.2.1 关于代码块

块标识符

不同类型的代码被组织在模型描述文件中，并用块标识符分隔。在mrgsolve中，有两种方法可以形成块标识符。在第一种类型中，在块名称的开头放置一个美元符号

```R
$BLOCKNAME
<block-code>
```

例如，一个参数块将是

```R
$PARAM
CL = 1
```

第二种写法是用括号

```R
[ BLOCKNAME ] 
<block-code>
```

美元符号和括号之间没有功能上的区别。当模型描述代码保存到扩展名为". cpp"的文件中时，代码编辑器可能会对代码的格式或样式做出某些假设。在这种情况下，使用括号很可能会更好地与编辑器配合使用。

块标识符不区分大小写，因此下述两种写法等价并都也可以工作

```R
$param 
CL = 1
```

```R
[ param ] 
CL = 1
```

用户可以自由地在块标识符的同一行中包含块代码，但必须在标识符后包含空格。例如，解析器将可以识别"`$PARAM CL = 1`"，但不能识别"`$PARAMCL=1`"作为参数。

**块语法**：不同的块可能需要不同的语法。例如，用"`$PARAM`"编写的代码将由"R"解析器解析，通常需要遵守"R"语法要求。另一方面，"`$MAIN`"、"`$ODE`"和"`$TABLE`"中的代码将用于创建C++中的函数，因此需要是有效的C++代码，包括每一行末端都需要有";"符号。

**块选项**：可以在某些代码块上指定选项，以指示如何在模拟中解析或使用代码。。

块选项可以是布尔值，在这种情况下，您可以指示选项名称，后面没有任何内容。例如，要指示 OMEGA 矩阵的"block = TRUE"，请写入

```R
$OMEGA @block 
1 2 3
```

这里的"@block"表示"block = TRUE"。从 mrgsolve 1.0.0 开始，可以通过在"@"和选项名称之间写入"!"符号来否定块选项。请求非块(对角线)OMEGA矩阵

```R
$OMEGA @!block 
1 2 3
```

但是请注意，对角线是OMEGA的默认配置，所以从来没有必要写这个。

其他非布尔值的块选项，应先声明选项的名称，然后给该名称分配值。例如，如果我们想为来自OMEGA块的ETA分配标签，我们会写

```R
$OMEGA @labels ECL EVC
1 2
```

这实质上是将标签设置为等于向量c("ECL", "EVC")。

我们注意到两个特定的布尔选项("@object"和"@as_object")，它们在多个块中具有相似的功能。

### 2.2.2程序化或批量初始化

下面描述了使用R代码将块初始化为R对象的语法。例如，当您需要初始化50x50 OMEGA矩阵或一系列系统命名的参数时，这会很有帮助。我们将描述在选定的块上可用的"@object"和"@as_object"选项。

"@object"选项允许您命名在"`$ENV`"中定义的对象以用于实例化块数据。例如，要使用"@object"选项指定一系列参数，您可以编写

```R
$ENV
params <- list(CL = 1, V = 20, KA = 1.2)

$PARAM @object params
```

这告诉mrgsolve有一个名为"params"的对象，它在"`$ENV`"环境中，mrgsolve将使用它来定义名称和值。这是一个简单的示例，用于演示如何定义一系列简单的参数。但是，此功能的预期用途是允许在大型模型中高效创建一系列系统命名的大型参数。

"@as_object"选项是一个布尔选项，它告诉 mrgsolve 块代码实际上将返回对象(而不是要求 mrgsolve 在"`$ENV`"中查找对象)。上述块的等效的模型描述为：

```R
$PARAM @as_object
list(CL = 1, V = 20, KA = 1.2)
```

以下块包含"@object"和"@as_object"**选项**：

- "`$PARAM`"

- "`$THETA`"

- "`$CMT`"

- "`$INT`"

- "`$OMEGA`"

- "`$SIGMA`"

有关调用此语法时应返回的特定对象类型的更多详细信息，请参阅特定的块文档。

### 2.2.3 "`$PROB`"

**语法**：文本

**允许多个**：是

**选项**："@annotated"、"@covariates"

使用此块对模型进行注释。这里输入的文本没有限制。mrgsolve不会以任何方式常规处理文本，除非将模型渲染为文档。通常，我们在这里以markdown格式编写文本，以便在模型文档中很好地呈现。但这是完全可选的。

有关示例，请参见第 11.1 节中的带注释的模型。

### 2.2.4"`$PARAM`"

**语法**：R

**允许多个**：是

**选项**："@annotated"、"@covariates"、"@object"、"@as_object"

在当前模型中定义参数列表。参数是与可在整个模型中使用的值相关联的名称。必须为每个参数名称给出一个值。参数的名称(和编号)必须在编译模型时设置，但参数值可以在不重新编译模型的情况下更新。

"@covariates"选项允许您将参数集合标记为"协变量"。它不会以任何方式更改模型或模拟工作流的功能，但允许您从模型对象中获取协变量名称列表。

示例：

```R
[ PARAM ] CL = 1, VC = 20, KA = 1.2
KM = 25, VMAX = 400, FLAG = 1, WT = 80
SEX = 0, N = sqrt(25)
```

请注意，参数列表中的"值"将由R解释器计算，计算将发生在由"`$ENV`"块定义的环境中。例如

```R
$ENV NWT = 1.2

$PARAM 
WT = 80 * 2.2
MASS = 600 * MWT
```

带注释的示例：

```R
[ PARAM ] @annotated
CL :  1 : Clearance (L/hr)
VC : 20 : Volume of distribution (L)
KA: 1.2 : Absorption rate constant (1/hr)
```

有关使用"@covariates"的示例，请参阅"popex"模型

. Model file: popex.cpp 

. 

. $PARAM

. TVKA = 0.5, TVCL = 1, TVV = 24

. 

. $PARAM

. @covariates

. WT = 70

然后

```R
mod <- modlib("popex")
```

```R
as.list(mod)$covariates
```

. [1] "WT"

注意事项：

- 允许多个块

- 值由"R"解释器计算

"@object"和"@as_object"选项应该命名或返回命名的参数列表。

另见：[第2.2.22节](https://mrgsolve.org/user-guide/specification.html#seCLblock-theta)和[第2.2.5节](https://mrgsolve.org/user-guide/specification.html#seCLblock-fixed)。

加载"mrgsolve"后请参阅?param在"R"帮助系统。

### 2.2.5 "`$FIXED`"

**语法**：R

**允许多个**：是

**选项**："@annotated"、"@object"、"@as_object"

与"`$PARAM`"一样，"`$FIXED`"用于指定"名称=值"对。但是，与"`$PARAM`"不同，与"`$FIXED`"中的名称关联的值无法更新。

默认情况下，"`$FIXED`"中的名称通过C++预处理器#define语句与其值相关联。

通常，"`$FIXED`"仅在存在大量参数(100 或 200)。当其中一些参数永远不需要更新时，您可以将它们移动到"`$FIXED`"块中，以获得适度的仿真效率提升。

查询参数时，"`$FIXED`"中的项目不会显示。

示例：

```R
[ PARAM ] CL = 2, VC = 20

[ FIXED ]
g = 9.8
```

带注释的示例：

```R
$FIXED @annotated
g : 9.8 : Acceleration due to gravity (m/s^2)
```

另见：第2.2.4节和第2.2.22节。

注意事项：

- 允许多个块

- 值由"R"解释器计算

### 2.2.6"`$CMT`"和"`$INIT`"

**语法**：文本

**允许多个**：是

**选项**："@annotated"、"@object"、"@as_object"

声明模型中所有房室的名称。

- 对于"`$CMT`"，给出房室的名称；初始值假定为0

- 对于"`$INIT`"，给出所有房室的名称和初始值

请注意，"`$CMT`"和"`$INIT`"都声明房室，因此任何房室名称都应在"`$CMT`"或"`$INIT`"中声明，但绝不能在两者中同时声明。

示例：

```R
[ CMT ] GUT CENT RESPONSE
```

```R
[ INIT ] GUT = 0, CENT = 0, RESPONSE = 25
```

带注释的示例：

```R
[ CMT ] @annotated
GUT   : Dosing compartment (mg)
CENT   : Central PK compartment (mg)
RESPONSE : Response
```

```R
$INIT @annotated
GUT   :  0 : Dosing compartment (mg)
CENT   :  0 : Central PK compartment (mg)
RESPONSE : 25 : Response
```

"@object"和"@as_object"选项在"`$INIT`"中使用时应命名或返回房室和初始值的命名列表，在"`$CMT`"中使用时应返回房室名称的字符向量。

加载"mrgsolve"之后请参阅?init在"R"帮助系统。

### 2.2.7"`$MAIN`"

**语法**：C++

**允许多个**：否

此代码块有两个主要目的：

- 在参数、随机、效应和其他衍生变量之间衍生的新代数关系

- 设置模型房室的初始条件

对于熟悉NONMEM的用户来说，"`$MAIN`"类似于"`$PK`"。 【译者注：对于熟悉Phonenix NLME的PML语言的用户来说，"`$MAIN`"类似于PML的"stparm()"语句：用于指定参数的统计分布与协变量关系；部分时候"`$MAIN`"语句可以起到PML的"sequence()"语句部分作用，用于为一些变量设置初始时刻的值或条件。对与mrgsolve来说，普通的代数方程可以写在多出，但是处于运算量考虑建议仅将与会随时间变化的代数方程写入`$ODE`块，而不会随时间变化的写入`$MAIN`块，从这个概念上外推，我们可以将所有想写的代数方程统一写在`$ODE`块即可，慢点就慢点】。

"`$MAIN`"被包装到一个C++函数中，并由"mrgsolve"编译/加载。

对于数据集中的每条记录，"MAIN"函数在系统从当前时间推进到下一次之前被调用。"`$MAIN`"在开始问题之前("NEWIND==0")和模拟每个个体之前("NEWIND==1")也被调用了几次。最后，每次使用"init()"查询模型初始条件时都会调用"`$MAIN`"。

新变量可以在"`$MAIN`"中声明。详情见第2.5节。

示例：

```R
[ CMT ] CENT RESP

[ PARAM ] KIN = 100, KOUT = 2, CL = 1, VC = 20

[ MAIN ]

RESP_0 = KIN/KOUT;

double ke = CL/VC;
```

### 2.2.8"`$PK`"

这是"`$MAIN`"的别名。

### 2.2.9"`$ODE`"

**语法**：C++

**允许多个**：无

**选项**："@参数"

使用"`$ODE`"定义模型微分方程。对于所有房室，将微分方程的值分配给"dxdt_CMT"，其中"CMT"是房室的名称。"dxdt_"方程可以是模型参数(通过"`$PARAM`")、任何房室("CMT")的当前值或任何用户衍生出的变量的函数。

对于熟悉Phonenix NLME的PML语言的用户来说，"`$ODE`"类似于"deriv()"语句, dxdt_GUT = -KA*GUT等价于deriv(GUT=-KA*GUT)。

示例：

```R
[ CMT ] GUT CENT

[ ODE ]
dxdt_GUT = -KA*GUT;
dxdt_CENT = KA*GUT - KE*CENT;
```

重要的是要确保为"`$CMT`"或"`$INIT`"中列出的每个房室定义了一个"dxdt_"表达式，即使将它定义为dxdt_CMT = 0;

"`$ODE`"函数在模拟运行期间被重复调用。因此，明智的做法是在"`$ODE`"之外进行尽可能多的计算，通常是在"`$MAIN`"中。但是请记住，任何依赖于房室中的数量并有助于确定模型中的"dxdt_"表达式的计算都必须用"`$ODE`"编写。

新变量可以在"`$ODE`"中声明。详情见第2.5节。

示例：

```R
$CMT CENT RESP
$PARAM VC = 100, KE = 0.2, KOUT = 2, KIN = 100
$ODE
double CP = CENT/VC;
double INH = CP/(IMAX+CP)

dxdt_CENT = -KE*CENT;
dxdt_RESP = KIN*(1 - INH) - RESP*KOUT;
```

注意事项：

- "`$ODE`"是用C++语法编写的；每行必须以;

- 一个模型中可能只有一个"`$ODE`"块

### 2.2.10"`$DES`"

这是"`$ODE`"的别名。

### 2.2.11"`$TABLE`"

**语法**：C++

**允许多个**：否

在系统前进到下一个时间后，使用"`$TABLE`"与参数、房室值和其他用户定义的变量进行交互。

示例：

```R
[ TABLE ]
double CP = CENT/VC;
```

注意"mrgsolve"以前有一个"table()"宏，用于将衍生值插入模拟输出。此宏已被弃用。将衍生值插入模拟输出的唯一方法是通过"`$CAPTURE`"。

注意当变量被标记为捕获时(参见第2.2.15节)，这些变量的值保存在"`$TABLE`"函数的末尾。这个过程由"mrgsolve"自动执行，因此不需要用户干预。

2.2.12"`$ERROR`"

这是"`$TABLE`"的别名。

### 2.2.13 "`$PREAMBLE`"

**语法**：C++

**允许多个**：否

这是第四个C++代码块。它在两种不同的设置中调用一次(第 7 章)：

在开始模拟运行之前

在计算初始条件时调用"`$MAIN`"之前

"`$PREAMBLE`"是一个允许您设置C++环境的函数。它仅在模拟运行期间调用一次(就在开始时)。此块中的代码通常用于配置或初始化在"`$GLOBAL`"中声明C++变量或数据结构。

示例：

```R
[ PLUGIN ] Rcpp

[ GLOBAL ]
namespace{
 Rcpp::NumericVector x;
}
[ PREAMBLE ]
x.push_back(1);
x.push_back(2);
x.push_back(3);
[ MAIN ]
<some code that uses x vector>
```

在这个例子中，我们希望使用一个数字向量"x"并在"`$GLOBAL`"中声明它，以便我们可以在代码中的其他任何地方使用它(声明也是在未命名的命名空间中进行的，以确保变量是模型文件的本地变量)。然后，在"`$PREAMBLE`"中，我们将 3 个数字放入向量中，并在"`$MAIN`"中使用"x"。由于"`$MAIN`"、"`$TABLE`"和(尤其是)"`$ODE`"在模拟运行过程中被反复调用，因此我们将"x"的初始化放在"`$PREAMBLE`"中，以确保"x"的初始化只发生一次。

注意事项：

- "`$PREAMBLE`"是用C++语法写的;每行都必须以;结尾

- 模型中可能只有一个"`$PREAMBLE`"块

- 与"`$MAIN`"、"`$ODE`"和"`$TABLE`"一样，在"`$PREAMBLE`"中初始化的"double"、"int"和"bool"变量实际上是为全局初始化的(在模型文件中)

另见：第7章和第2.2.21节。

### 2.2.14 "`$PRED`"

**语法**：C++

**允许多个**：否

使用"`$PRED`"编写没有微分方程的模型。在此块中，为衍生参数、响应和任何其他衍生输出量编写所有代数表达式。

示例：

```R
[ PARAM ] TVE0 = 100, AUC50 = 100, IMAX = 40, AUC = 0

[ PRED ]
double E0 = EVE0*exp(ETA(1));

double RESP = E0 - IMAX*AUC/(AUC50+AUC);
```

在此示例中，整个模型编写在"`$PRED`"块中。使用"`$PRED`"时包含以下块是错误的："`$MAIN`"、"`$TABLE`"、"`$PKMODEL`"、"`$ODE`"、"`$CMT`"、"`$INIT`"。

有关与"`$PRED`"块一起使用的数据集的更多信息，请参见第 3.7 节。

### 2.2.15 "`$CAPTURE`"

**语法**：文本

**允许多个**：是

**选项**："@annotated"、"@etas"

这是一个块，用于标识应在模拟输出中捕获的变量。"@etas"选项是 1.0.8 版的新功能。

示例：

```R
[ PARAM ] A = 1, B = 2

[ MAIN ]
double C = 3;
bool yes = true;

[ CAPTURE ] A B C yes
```

此构造将在模拟输出中生成四个额外的列，名称为"A"、"B"、"C"和"yes"。

用户还可以通过提供"newname = oldname"规范来重命名捕获的变量。

```R
$PARAM WT = 70, THETA1 = 2.2

$MAIN
double CL = THETA1*pow(WT/70,0.75)*exp(ETA(1));

$OMEGA 1

$CAPTURE WEIGHT = WT TVCL = THETA2 CL ETA(1)
```

在此示例中，捕获(CAPTURE)的数据项的名称将为"WEIGHT，TVCL，CL，ETA_1"。

用户可以使用"CAPTURE"类型在"`$MAIN`"和"`$TABLE`"中声明变量。"CAPTURE"类型实际上是"双精度"，但使用该类型将发出"mrgsolve"信号以自动捕获该值。示例：

```R
$PARAM VC = 300

$CMT CENT

$TABLE
capture DV = (CENT/VC);
```

由于我们对"DV"使用了类型"捕获"，"DV"将在模拟数据中显示为一列。

带注释的示例：

```R
$MAIN
double CLi = TVCL*exp(ECL);

$TABLE
double DV = (CENT/VC)*exp(PROP);

$CAPTURE @annotated
CLi : Individual clearance (L/hr)
DV : Plasma concentration (mcg/ml)
```

使用"@etas"选项指定一个表达式，该表达式的计算结果为整数，这些整数标识要捕获的"ETA"数字。例如

```R
$OMEGA 0.1 0.2 0.3

$CAPTURE @etas 1:LAST
CL DV
```

将所有三个"ETA"捕获到模拟输出中。NONMEM风格的名称将被使用(例如"ETA1")。这与捕获"ETA(1)"时生成的名称形成对比;在这种情况下，括号将被删除并替换为"_"，因此输出名称为"ETA_1"。mrgsolve将提供"LAST"(和"last")，它将评估到问题中"ETA"的最大数量(在本例中为3)。您可以在"@etas"中编写任何有效的表达式。计算表达式时，将在通过"`$ENV`"块创建的模型环境中对其进行计算(第 2.2.26 节)。

> 小提示
>
> 除了在`$CAPTURE`块中列出用于输出的模型变量外，用户还可以通过mread()的CAPTURE参数"动态"捕获模型变量。这些可能是模型参数(列在`$PARAM`中)或用户定义的模型变量。
>
> 要获取可用用户定义变量的列表，您可以使用
>
> ```R
> mod <- house()
> as.list(mod)$cpp_variables
> ```

### 2.2.16"`$OMEGA`"


**语法**：文本

**允许多个**：是

**选项**："@annotated"、"@block"、"@correlation"、"@labels"、"@name"、"@object"、"@as_object"

有关此块的选项的更多详细信息，请参见?modMATRIX。

使用此块输入从多元正态分布中提取的个体级随机效应的方差/协方差矩阵。假设所有随机效应的平均值为0。如果使用"@correlation"选项，则块矩阵的非对角线元素假定为相关系数(见下文)。

默认情况下，假设为**对角**(diagonal)矩阵。所以：

```R
$OMEGA
1 2 3
```

将生成一个3x3的omega矩阵。

可以使用"block=TRUE"输入**块**(block)矩阵。所以：

```R
$OMEGA @block
0.1 0.02 0.3
```

将生成协方差为0.02的2x2矩阵。

一个2x2矩阵，其中非对角线元素是相关性，而不是协方差，可以这样指定：

```R
$OMEGA @correlation
0.1 0.67 0.3
```

这里，相关性为0.67。"mrgsolve"将计算协方差并替换这些值。矩阵将与这些协方差一起存储和使用，而不是相关性。

可以为每个矩阵分配一个名称：

```R
$OMEGA @name PK @block
0.2 0.02 0.3

$OMEGA @name PD
0.1 0.2 0.3 0.5
```

区分多个"`$OMEGA`"块并方便以后更新。前面示例中的模型将有两个"`$OMEGA`"矩阵：2x2和4x4。

可以分配标签作为不同"ETA"的别名

```R
$OMEGA @block @labels ETA_CL ETA_V
0.1 0.05 0.2
```

标签的数量应与矩阵中的行数(或列数)相匹配。

带注释的示例(对角矩阵)：

```R
$OMEGA @annotated
ECL: 0.09 : ETA on clearance
EVC: 0.19 : ETA on volume
EKA: 0.45 : ETA on absorption rate constant
```

带注释的示例(块矩阵)：

```R
$OMEGA @annotated @block
ECL: 0.09 : ETA on clearance
EVC: 0.001 0.19 : ETA on volume
EKA: 0.001 0.001 0.45 : ETA on absorption rate constant
```

"@object"和"@as_object"选项应该命名或返回一个平方数矩阵。如果矩阵中包含"行名"，那么它们将用于形成已实现ETA的标签。例如，我们可以初始化一个非常大的"`$OMEGA`"矩阵

```R
$OMEGA @as_object
n <- 20
lbl <- paste0("ETA_", LETTERS[1:n])
matrix(0, 20, 20, dimnames = list(lbl, lbl))
```

注意事项：这只初始化矩阵；加载模型后，您(可能)需要用有意义的值更新它(第5章)。

### 2.2.17 "`$SIGMA`"

**语法**：文本

**允许多个**：是

**选项**："@annotated"、"@block"、"@correlation"、"@labels"、"@name"、"@object"、"@as_object"

有关此块选项的更多详细信息，请参阅。?modMATRIX

使用此块输入从多元正态分布中提取的个体内随机效应的方差/协方差矩阵。假设所有随机效应的平均值为0。如果使用"@ correlation"选项，则块矩阵的非对角线元素假定为相关系数(见下文)。

"@object"和"@as_object"选项应该命名或返回一个平方数矩阵。如果矩阵中包含"行名"，那么它们将用于形成实现的EPS值的标签。

"`$SIGMA`"块的功能类似于"`$OMEGA`"块。有关详细信息，请参阅"`$OMEGA`"。

### 2.2.18"`$SET`"

**语法**：R

**允许多个**：否

使用此代码块为模拟设置不同的选项。使用"name=value"格式，其中"value"由"R"解释器计算。

大多数可以在"`$SET`"中输入的选项都被传递给"update"。

示例：

```R
[ SET ] end = 240, delta = 0.5, req = "RESP"
```

在这里，我们将模拟"结束"时间设置为240，将两个相邻时间点之间的时间差设置为0.25时间单位，并仅请求模拟输出中的"RESP"房室。

### 2.2.19 "`$GLOBAL`"

**语法**：C++

**允许多个**：否

"`$GLOBAL`"块用于编写"`$MAIN`"、"`$ODE`"和"`$TABLE`"之外的C++代码。

对于什么样的C++代码可以进入"`$GLOBAL`"没有人为的限制。

但是，有两种更常见的用途：

- 编写#define预处理器语句

- 定义全局变量，通常是 "double"、"bool"、"int" 以外的变量(参见第 2.5 节)

预处理器指令：预处理器#define指令是C++预处理器在编译代码之前所做的直接替换。

示例：

```R
[ GLOBA ]
#define CP (CENT/VC)
```

当包含此预处理器指令时，无论预处理器在哪里找到"CP"标记，它都将替换(CENT/VC)。"CENT"和"VC"都必须定义，"CENT"和"VC"的比率将根据当前值计算。请注意，我们在周围包含了括号(CENT/VC)。

这确保了在任何其他涉及"CP"的操作之前，首先获取两者之间的比率。

声明全局变量有时，您可能希望使用全局变量并更好地控制它们的声明方式。

```R
$GLOBAL
bool cure = false;
```

使用此构造，布尔变量"cure"在编译模型时被声明和定义。

批量声明

如果要声明大量变量，可以在"`$GLOBAL`"中批量声明。例如，我们知道我们需要一长串"双精度"变量进行协变量建模，我们可以像这样一次声明它们：

```R
[ globa ] 
double TVCL, TVV2, TVQ = 0, TVV3 = 0;

[ main ] 

TVCL = THETA1 * pow(WT / 70.0, 0.75);
TVV2 = THETA2 * WT / 70.0;
TVQ = THETA3 * pow(WT / 70.0, 0.75);
TVV3 = THETA4 * WT / 70.0;
```

这不是一个很长的列表，但让我们假装它是为了说明如何做到这一点。你也可以声明"int"，"bool"和其他类似的类型。我已经初始化了最后两个("TVVQ"和"TVV3")来说明如何做到这一点。这样做是一种很好的做法，但只要在使用之前将所有内容初始化为某些内容，就没有必要了。

### 2.2.20"`$PKMODEL`"

**语法**：R

**允许多个**：否

这个代码块实现了一个一室或两室的PK模型，其中系统是由代数方程计算的，而不是ODE。"mrgsolve"处理计算，如果"`$PKMODEL`"和"`$ODE`"块都包含在同一个模型描述文件中，则会产生错误。

这是一个仅限选项的块。用户必须指定要在模型中使用的房室数量(1或2)以及是否包括接受药量的吸收房室。有关此块的更多详细信息，请参见?PKMODEL，包括必须在模型描述文件中定义的符号的具体要求。

"`$CMT`"或"`$INIT`"块也必须包含在适当数量的房室中。但是，房室名称可以由用户确定。

示例：

```R
[ CMT ] GUT CENT PERIPH

[ PKModel ] ncmt=2, depot=TRUE
```

从版本"0.8.2"开始，我们可以在"`$PKMODEL`"块中指定房室：

```R
$PKModel cmt="GUT CENT PERIPH", depot = TRUE
```

使用"depot=TRUE"指定三个房室意味着"ncmt=2"。请注意，当在"`$PKMODEL`"中指定"cmt"时，单独的"`$CMT`"块不在适用。

### 2.2.21 "`$PLUGIN`"

**语法**：文本

**允许多个**：否

PLUGIN(插件)是一种向"mrgsolve"模型添加扩展的方法。插件可以将您的模型链接到外部库(如"boost"或"Rcpp")，也可以开放对"mrgsolve"本身提供的其他功能的访问。

插件将在第9章中详细列出和讨论。。

用法

要调用插件，请在代码块中列出插件名称。要在模型中请求"Rcpp"标头，请调用

```R
$PLUGIN Rcpp
```

可用插件

以下插件提供了其他特定于 mrgsolve 的功能

- "mrgx"：额外的C++函数(见下文)

- "tad"：在模型中跟踪提供给药后的时间

- "N_CMT"：获取房室的编号

以下插件为您提供不同的型号描述体验

- "autodec"：mrgsolve将在您的模型中查找"赋值"语句，并自动将其中的变量声明为"双精度"

- nm-vars：NONMEM外观和感觉的房室模型;使用"F1"、"A(1)"和"DADT(1)"而不是"F_GUT"、"GUT"和"dxdt_GUT"

以下插件允许您自定义模型的编译方式

- "CXX11"：使用标准编译模型;这将添加编译器标志C++11-std=c++11

以下插件将允许您链接到外部库

- "Rcpp"：在您的模型中包含"Rcpp"标头

- "BH"：在模型中包含"boost"标头

- "RcppArmadillo"：在模型中包括"Armadillo"标头

请注意，"Rcpp"，"RcppArmadillo"和"BH"仅允许您链接到这些标头。要利用这一点，您需要知道如何使用"Rcpp"，"boost"等。对于"BH"插件，不包含任何标头;您必须在"`$GLOBAL`"中包含要使用的正确标头。

**mrgx**：这是我们提供的函数的常规集合。

"mrgx"提供的功能：

- T get\<T\>(std::string \<pkgname\>, std::string \<objectname\>)

   - 这会从任何包中获取任何Rcpp可表示类型("T")的对象

- T get\<T\>(std::string \<objectname\>)

   - 这将从". GlobalEnv"获取任何Rcpp可表示类型("T")的对象

- T get\<T\>(std::string \<objectname\>, databox& self)

   - 这将从"`$ENV`"获取任何 Rcpp 可表示类型 ("T") 的对象

- double rnorm(double mean, double sd, double min, double max)

   - 模拟介于"最小值"和"最大值"之间的正态分布中的一个变量

- double rlognorm(double mean, double sd, double min, double max)

   - 与mrgx::rnorm类似，但模拟后将模拟值传递给exp

- Rcpp::Function mt_fun()

   - 返回mrgsolve: mt_fun;这通常在$GLOBAL中声明R函数时使用

   - 示例：Rcpp::Function print = mrgx::mt_fun();

**重要提示**：所有这些函数都在"mrgx"命名空间中。因此，为了调用这些函数，您必须在函数名称的前面包含命名空间标识符。例如，不要使用 "rnorm(50,20,40,140)";而应使用mrgx::rnorm(50,20,40,140)。

#### 2.2.21.1 一些示例

从"`$ENV`"获取数字向量

```R
[ PLUGIN ] Rcpp mrgx

[ ENV ]

x <- c(1,2,3,4,5)

[ GLOBA ]
Rcpp::NumericVector x;

[ PREAMBLE ]
x = mrgx::get<Rcpp::NumericVector>("x", self);
```

从package:base中获取"print"函数

```R
$PLUGIN Rcpp mrgx

$GLOBAL
Rcpp::Function print = mrgx::mt_fun();

$PREAMBLE
print = mrgx::get<Rcpp::Function>("base", "print");

$MAIN
print(self.rown);
```

请注意，我们在"`$GLOBAL`"中声明"print"并使用"mt_fun()"占位符。

**模拟截断的正态变量**：这将模拟均值为 80、标准差为 20 且大于 40 且小于 140 的体重。

```R
$PLUGIN Rcpp mrgx

$MAIN
if(NEWIND <=1) {
 double WT = mrgx::rnorm(80,20,40,140);
}
```

另请参阅：[第 2.2.13 节](https://mrgsolve.org/user-guide/specification.html#seCLblock-preamble)。

### 2.2.22 "`$THETA`"

**语法**：文本

**允许多个**：是

**选项**："@annotated"、"@name"、"@object"、"@as_object"

使用此代码块作为添加到参数列表的有效方法，其中名称由前缀和数字确定。默认情况下，前缀为"THETA"，数字按顺序对输入值进行编号。

例如：

```R
[ THETA ]
0.1 0.2 0.3
```

相当于

```R
$PARAM THETA1 = 0.1, THETA2 = 0.2, THETA3 = 0.3
```

带注释的示例：

```R
$THETA @annotated
0.1 : Typical value of clearance (L/hr)
0.2 : Typical value of volume (L)
0.3 : Typical value of ka (1/hr)
```

要更改前缀，请使用"@name"选项

```R
$THETA @name theta
0.1 0.2 0.3
```

将等同于

```R
[ PARAM ] theta1 = 0.1, theta2 = 0.2, theta3 = 0.3
```

"@object"和"@as_object"选项应命名或返回参数值的未命名向量(名称将被忽略)。

另请参阅：[第 2.2.4 节](https://mrgsolve.org/user-guide/specification.html#seCLblock-param)。

### 2.2.23 "`$NMXML`"

**语法**：R

**允许多个**：是

"`$NMXML`"块允许您读取 NONMEM 运行的结果并将其合并到您的"mrgsolve"模型中。例如

```R
$NMXM
run = 1001
project = "../model/nonmem"
root = "cppfile"
```

在 NONMEM 运行中，"THETA"将被导入到您的参数列表中(参见[第 2.2.4 节](https://mrgsolve.org/user-guide/specification.html#seCLblock-param)和[第 1.1 节](https://mrgsolve.org/user-guide/components.html#seCLcomponent-param))，"OMEGA"将被捕获为"`$OMEGA`"块[(第 2.2.16 节](https://mrgsolve.org/user-guide/specification.html#seCLblock-omega))，"SIGMA"将被捕获为"`$SIGMA`"块([第 2.2.17 节](https://mrgsolve.org/user-guide/specification.html#seCLblock-sigma))。 用户可以选择省略其中任何一个导入。

"`$NMXML`"包含一个"project"参数和一个"run"参数。默认情况下，估计值是从文件project/run/run.xml中读取的。也就是说，假设在"project"目录中有一个名为"run"的目录，"`$NMXML`"将在其中找到"run.xml"。您的NONMEM运行目录可能无法以与此默认值兼容的方式组织。在这种情况下，您需要提供"file"参数，它应该是"run.xml"文件的路径，可以作为完整路径，也可以作为相对于当前工作目录的路径。

获取模型对象后，可以通过强制模型对象转换为列表并查找"nm_import"来检索构成导入参数源的"xml"文件的路径：

```R
mod <- modlib("1005", compile = FALSE)

as.list(mod)$nm_import
```

有关"`$NMXML`"的参数/选项的帮助，请参阅?nmxml帮助主题。

一个示例

在"mrgsolve"包中嵌入了一个NONMEM运行

```R
path <- file.path(path.package("mrgsolve"),"nonmem")
list.files(path, recursive=TRUE)
```

. [1] "1005/1005.cat"  "1005/1005.coi"  "1005/1005.cor"  "1005/1005.cov"  

. [5] "1005/1005.cpu"  "1005/1005.ctl"  "1005/1005.ext"  "1005/1005.grd"  

. [9] "1005/1005.lst"  "1005/1005.phi"  "1005/1005.shk"  "1005/1005.shm"  

. [13] "1005/1005.tab"  "1005/1005.xml"  "1005/1005par.tab" "1005/INTER"   

. [17] "2005/2005.ext"

我们可以创建一个"mrgsolve"控制流，它将使用"`$NMXML`"代码块从该运行中导入"THETA"，"OMEGA"和"SIGMA"。

```R
// inline/nmxml-1.cpp

$NMXML
run = 1005
project = path
root = "cppfile"
olabels = c("ECL", "EVC", "EKA")
slabels = c("PROP", "ADD")

$MAIN
double CL = THETA1*exp(ECL);
double V2 = THETA2*exp(EVC);
double KA = THETA3*exp(EKA);
double Q = THETA4;
double V3 = THETA5;

$PKModel ncmt=2, depot=TRUE

$CMT GUT CENT PERIPH

$TABLE
double CP = (CENT/V2)*(1+PROP) + ADD/5;

$CAPTURE CP

$SET delta=4, end=96
```

注意：为了使用此代码，我们需要安装"xml2"软件包。

```R
mod <- mread("inline/nmxml-1.cpp", compile = FALSE)

mod
```

. 

. 

. --------------- source: nmxml-1.cpp ---------------

. 

.  project: /Users/kyleb/git...-guide/inline

.  shared object: nmxml-1.cpp-so-1058b51a4e6d6 \<not loaded\>

. 

.  time:     start: 0 end: 96 delta: 4

.         add: \<none\>

. 

.  compartments: GUT CENT PERIPH [3]

.  parameters:  THETA1 THETA2 THETA3 THETA4 THETA5

.         THETA6 THETA7 [7]

.  captures:   CP [1]

.  omega:     3x3 

.  sigma:     2x2 

. 

.  solver:    atol: 1e-08 rtol: 1e-08 maxsteps: 20k

. ------------------------------------------------------

```R
param(mod)
```

. 

. Model parameters (N=7):

. name  value . name  value

. THETA1 9.51  | THETA5 113 

. THETA2 22.8  | THETA6 1.02 

. THETA3 0.0714 | THETA7 1.19 

. THETA4 3.47  | .   .

```R
revar(mod)
```

. $omega

. $...

.       [,1]    [,2]    [,3]

. ECL:  0.21387884 0.12077020 -0.01162777

. EVC:  0.12077020 0.09451047 -0.03720637

. EKA: -0.01162777 -0.03720637 0.04656315

. 

. 

. $sigma

. $...

.       [,1]   [,2]

. PROP: 0.04917071 0.0000000

. ADD:  0.00000000 0.2017688

**root参数**：请使用root = "cppfile"参数。

从 mrgsolve "0.11.0" 开始，我们在 "`$NMXML`" 中添加了一个名为 "root" 的参数，它告诉 mrgsolve 它应该从中读取"xml"文件的位置。默认行为是目录。在这种情况下，mrgsolve 假定可以相对于"工作"目录找到"xml"文件。"root"可以取的唯一其他值是"cppfile"。当root = "cppfile"，那么mrgsolve将在相对于模型源代码文件所在的目录中查找"xml"文件。请参阅[第2.2.24节](https://mrgsolve.org/user-guide/specification.html#seCLblock-nmext)以获取更多讨论和示例。

从版本 1.0.8 开始，当使用 "run" + "project" 参数找到 NONMEM 运行时，您可以传递run = "@cppstem"，mrgsolve将查找与当前 mrgsolve 文件的名称匹配的 NONMEM 运行编号。例如，如果您正在编写一个模型以匹配 NONMEM run 101.ctl，则调用您的 mrgsolve 模型文件"101.cpp"或"101.mod";只需匹配文件名称即可。然后编写如下代码

在名为"101.cpp"文件中

```R
$NMXM
run = "@cppstem"
project = <nonmem-run-directory>
```

mrgsolve 将检查 mrgsolve 模型文件的名称，并在如下路径下查找NONMEM的.xml文件

```R
<nonmem-run-directory>/101/101.xml
```

请参阅?nmxml帮助主题，了解有关可以传递给"`$NMXML`"的参数的更多信息。

另请参阅：[第 2.2.24 节](https://mrgsolve.org/user-guide/specification.html#seCLblock-nmext)。

### 2.2.24"`$NMEXT`"

**语法**：R

**允许多个**：是

与"`$NMXML`"一样，"`$NMEXT`"允许从您的NONMEM中导入"`$THETA`"、"`$OMEGA`"和"`$SIGMA`"运行到您的mrg求解模型中，但估计值是从". ext"文件输出中读取的。当从基于采样的方法(主要是"METHOD=BAYES")加载时，"`$NMEXT`"能够比"`$NMXML`"更快地导入NONMEM估计。

我们可以使用以下代码加载与上面的"`$NMXML`"示例相同的模式：

```R
// inline/nmext-1.cpp
 
$NMEXT
run = 1005
project = file.path(path.package("mrgsolve"),"nonmem")
root = "cppfile"

olabels = c("ECL", "EVC", "EKA")
slabels = c("PROP", "ADD")

$MAIN
double CL = THETA1*exp(ECL);
double V2 = THETA2*exp(EVC);
double KA = THETA3*exp(EKA);
double Q = THETA4;
double V3 = THETA5;

$PKModel ncmt=2, depot=TRUE

$CMT GUT CENT PERIPH

$TABLE
double CP = (CENT/V2)*(1+PROP) + ADD/5;

$CAPTURE CP

$SET delta = 4, end = 96
```

```R
mod <- mread("inline/nmext-1.cpp", quiet=TRUE)

param(mod)
```

. 

. Model parameters (N=7):

. name  value . name  value

. THETA1 9.51  | THETA5 113 

. THETA2 22.8  | THETA6 1.02 

. THETA3 0.0714 | THETA7 1.19 

. THETA4 3.47  | .   .

一旦获得模型对象，就可以通过强制模型对象列出并查找"nm_import"来检索构成导入参数源的"ext"文件的路径。有关等效功能的示例，请参见[第2.2.23节](https://mrgsolve.org/user-guide/specification.html#seCLblock-nmxml)中的"`$NMXML`"主题。

**重要提示**：虽然"`$NMEXT`"的工作方式与"`$NMXML`"非常相似，但两者之间有一个关键区别：当使用"`$NMEXT`"时，您将始终获得一个"`$OMEGA`"矩阵和一个"`$SIGMA`"矩阵，而不管NONMEM控制流中使用的块结构如何。当使用"`$NMXML`"时，您在mrg求解模型中获得与NONMEM模型相同的块结构。例如：在NONMEM控制流中，"`$OMEGA`"是一个2x2矩阵和一个3x3矩阵。使用"`$NMEXT`"导入这些估计值将为您提供一个5x5矩阵，而使用"`$NMXML`"导入估计值将为您提供一个2x2和一个3x3矩阵的列表。

**root参数**：请使用root = "cppfile"参数。

从mrg求解"0.11.0"开始，我们在"`$NMEXT`"中添加了一个名为"root"的参数，它告诉mrg求解应该从哪里读取"ext"文件。默认行为是"working"目录。在这种情况下，mrgsolve假设可以找到相对于"工作"目录的"ext"文件。"root"可以取的唯一其他值是"cppfile"。当root = "cppfile"，那么mrgsolve将在**相对于模型源代码文件所在的目录**中查找"ext"文件。

我们建议用户开始使用"root"参数并将其设置为"cppfile"。这最终将成为默认设置。例如：

```R
$NMEXT
run = 1005
project = "../../model/pk"
root = "cppfile"
```

这告诉mrgsolve找到NONMEM run的两个父级目录(../../)，然后进入Model --> pk --> 1005相对于mrgsolve模型文件所在的位置。这与之前预期的路径应该相对于当前工作目录的行为相反。

我们在这里还注意到，当用户通过绝对路径时，相对路径根本不重要。用户可以自由地使用路径工具来生成项目的绝对路径。例如，here::here()函数可以这样使用

```R
$NMEXT
run = 1005
project = here::here("model/pk")
```

在适当的上下文中使用时，"here()"将生成从项目根目录到nonmem项目目录的绝对路径。请参考here::here()留档以正确使用此函数。

从版本1.0.8开始，当使用"run"+"project"参数找到NONMEM run时，您可以传递run = "@cppstem"，mrgsolve将查找与当前mrgsolve文件的名称匹配的NONMEM run号。请参阅[第2.2.23节](https://mrgsolve.org/user-guide/specification.html#seCLblock-nmxml)中的详细信息和示例。

有关可以传递给"`$NMEXT`"块的参数，请参阅?nmextR帮助主题。值得注意的是，用户可以在ext中选择要读取的函数file.By默认情况下，mrgsolve将尝试加载data.table并使用"fread"函数。如果无法加载data.table，则mrgsolve将使用utils::read.table。

另请参阅：[第2.2.23节](https://mrgsolve.org/user-guide/specification.html#seCLblock-nmxml)。

### 2.2.25 "`$INCLUDE`"

**语法**：文本

**允许多个**：否

要在模型中包含您自己的头文件，请使用"`$INCLUDE`"

```R
$INCLUDE
mystuff.h
otherstuff.h
```

或者

```R
$INCLUDE
mystuff.h, otherstuff.h
```

"mrgsolve"将在编译的C++代码中插入正确的#include预处理器指令。

要求

- "`$INCLUDE`"中列出的所有头文件都假定(预期)位于"project"目录下；不要对位于任何其他位置的头文件使用"`$INCLUDE`"

- 如果头文件不存在，则会生成错误

- 如果在文件名中找到任何引号，则会生成错误(不要在文件名周围使用引号；"mrgsolve"会处理这一点)

- 如果头文件不以". h"结尾，则会发出警告

- 当头文件更改(MD5校验和更改)时，当调用"mread()"或"mcode()"(但不调用"mread_cache()"或"mcode_cache()")时，模型将被强制重建(重新编译)；此功能仅适用于"`$INCLUDE`"中列出的头文件(见下文)

- 不要使用"`$INCLUDE`"来包含"Rcpp"、"boop"、"RcppArmadillo"或"RcppEigen"标头；改用适当的"`$PLUGIN`"

对于不符合上面列出的要求的应用程序，用户始终可以在"`$GLOBAL`"中的模型中包含头文件，如下所示：

```R
$GLOBAL
#include "/Users/me/libs/mystuff.h"
```

但是这样做的时候要小心：如果对"mysats. h"有更改，但对模型描述的任何其他部分没有更改，那么在调用"mread"时模型可能没有完全编译。在这种情况下，总是使用"preleans=TRUE"参数来"mread"，以强制在调用"mread"时构建模型。

### 2.2.26"`$ENV`"

**语法**：R

**允许多个**：否

此块都是"R"代码(就像您在独立的"R"脚本中编码一样。编译模型时，代码会被解析并评估到新环境中。

示例：

```R
$ENV

Sigma <- cmat(1,0.6,2)

mu <- c(2,4)

cama <- function(mod) {
 mod %>%
  ev(amt=100, ii=12, addl=10) %>% 
  mrgsim(obsonly=TRUE,end=120)
}
```

"`$ENV`"中的对象可以用于不同的C++函数(参见[第2.2.21节](https://mrgsolve.org/user-guide/specification.html#seCLblock-plugin))或模拟过程的其他部分。

解析模型描述文件的不同部分时，"`$ENV`"中的对象也将可用。例如，我们可以在上面的"`$ENV`"块示例中使用"Sigma"对象初始化一个"`$OMEGA`"块：

```R
$OMEGA @object Sigma
```

另一个例子是提供常量或函数来简化"`$PARAM`"块中的代码

```R
$ENV
rescale <- 1/1000 

convert <- function(x) x * 5 / 166.2

$PARAM
p = 500 * rescale

q = convert(2.51)
```

您还可以使您的模型描述文件依赖于R包；例如，如果您想使用"here"包来呈现NONMEM模型的路径

```R
$ENV
if(!requireNamespace("here")) {
 stop("the `here` package is required to load this model.") 
}

$NMXML
run = 101
project = here::here("model/nonmem")
```

可以使用"env_get()"函数访问模型环境：

```R
env_get(mod)
```

或直接在"envir"插槽中

```R
mod@envir
```

另请参见"env_ls()"列出模型环境中变量的名称，"env_update()"改变模型环境中对象的值，或"env_eval"重新计算模型环境代码。

## 2.3变量和宏

本节描述了一些可在模型描述文件中使用的宏和内部变量。在下一节中，我们采用"CMT"代表模型中的房室的约定。

**重要提示：**

从使用示例中应该清楚的看出，哪些变量可以由用户设置，哪些变量需要读取或检查。**所有内部变量都由mrgsolve预先定义和初始化。用户永远不应该尝试声明内部变量；这总是会导致编译时错误。**

### 2.3.1"ID"

当前个体标识符。"ID"是"self.id"的别名。

### 2.3.2"TIME"

给出当前数据集记录中的时间。这通常仅用于"`$MAIN`"或"`$TABLE`"。"TIME"是"self. time"的别名。与"SOLVERTIME"形成对比。

### 2.3.3"SOLVERTIME"

给出求解器当前时间步长的时间。这只能在"`$ODE`"中使用。与"TIME"形成对比。

从1.0.0版本开始，当您调用nm-vars插件([第9.2节](https://mrgsolve.org/user-guide/plugins.html#seCLplugin-nm-vars))时，变量T在$ODE中作为SOLVERTIME的同义词提供。

### 2.3.4"T"

仅在调用"nm-vars"插件时提供；请参阅"SOLVERTIME"。

### 2.3.5"EVID"

"EVID"是一个事件ID指示器。"mrgsolve"识别以下事件ID：

- 0=观察记录

- 1=推注或输注剂量

- 2=其他类型的事件，求解器重置

- 3=系统复位

- 4=系统复位和剂量

- 8=替换

"EVID"是"self. evid"的别名。

### 2.3.6"CMT"

"CMT"是当前的房室编号。在这种情况下，"CMT"是字面意思(不是房室名称的替代品)。例如：

```R
$CMT GUT CENT PERIPH

$MAIN

if(CMT==2) {
 // ....
}
```

在该示例中，"GUT"、"CENT"和"PERIPH"是各自房室中的量，而"CMT"是指数据记录/数据集中的"CMT"值。

"CMT"是"self. cmt"的别名。

### 2.3.7"AMT"

"AMT"是剂量药量的当前值。

"AMT"是"self. amt"的别名。

### 2.3.8"NEWIND"

"NEWIND"是一个新个体指示器，采用以下值：

- 0表示数据集的第一个事件记录

- 1对于后续个体的第一个事件记录

- 2个体的后续事件记录

示例：

```R
[ GLOBA ]
int counter = 0;

[ MAIN ]
if(NEWIND <=1) {
 counter = 0;
}
```

"NEWIND"是"self. newind"的别名。

### 2.3.9"SS_ADVANCE"

这是一个"bool"数据项(要么"true"要么"false")，它总是"false"，除非"mrgsolve"当前正在将系统推进到稳态([第8章](https://mrgsolve.org/user-guide/steady-state.html))；那么它将是"true"。此变量仅在"`$ODE`"(或"`$DES`")块中可用。

当系统进入稳态时，使用此变量修改模型微分方程的计算。一个用例是，当您有一个累积房室来计算"AUC"，但您想在系统向稳态工作时停止累积

```R
$ODE
dxdt_CENT = -(CL/V) * CENT;
dxdt_AUC = CENT/V;
if(SS_ADVANCE) dxdt_AUC = 0;
```

### 2.3.10"smite()"

"simeta()"函数可用于重新模拟ETA值。例如，

```R
$MAIN
simeta();
```

将重新模拟所有"ETA(n)"。

### 2.3.11"simeps()"

"simeps()"的工作方式类似于"simeta()"，但所有"EPS(n)"值都被重新模拟，而不是ETA值。

### 2.3.12" self "

"self"是传递给模型函数的对象，其中包含可以调用的数据项和函数。它是一个"struct"(参见[此处](https://github.com/metrumresearchgroup/mrgsolve/blob/master/inst/base/mrgsolv.h)的源代码)。以下部分记录了部分成员列表。

"self"函数包括

- self.tad()

- self.mtime()

- self.mevent()

- self.stop()

-  self.stop_id()

-  self. stop_id_cf()

"self"数据成员包括

- self.id(ID)

- self.amt(AMT)

- self.cmt(CMT)

- self.evid(EVID)

- self.time(TIME)

- self.newind(NEWIND)

- self.nid

- self.idn

- self.rown

- self.nrow

- self. envir(见下面的注释)

提供此信息是为了透明，并非详尽无遗。我们通过具有更简单名称的预处理器指令为这些数据项提供接口(例如"EVID"将转换为"self.evid")，并鼓励用户使用更简单的API名称。

### 2.3.13 "self.time"

当前数据集"TIME"。

### 2.3.14 "self.cmt"

当前房室编号，无论它在数据集中是作为"cmt"还是"CMT"给出。"self.cmt"没有别名。

示例：

```R
$TABLE

double DV = CENT/VC + EPS(1);

if(self.cmt==3) DV = RESPOSE + EPS(2);
```

### 2.3.15 "self.amt"

当前的"amt"值，无论它在数据集中是作为"amt"还是"AMT"给出。"self.amt"没有别名。

```R
[ PREAMBLE ]
double last_dose = 0;

[ MAIN ]

if(EVID==1) {
 last_dose = self.amt; 
}
```

### 2.3.16 "self.nid"

数据集中的ID数。

### 2.3.17 "self.idn"

当前的ID号。数字从0开始，增加1到"self. nid-1"。因此，如果要测试输出数据集中的最后一个ID，您将编写：

```R
[ table ]

if(self.idn == (self.nid-1)) {
 // do something ....
}
```

### 2.3.18"self. nrow"

输出数据集中的总行数。

### 2.3.19 "self.rown"

当前行号。数字从0开始，增加1到"self.rod-1"。因此，如果要测试输出数据集中的最后一行，您将编写：

```R
[ table ]

if(self.rown == (self.nrow-1)) {
 // do something ....
}
```

### 2.3.20 "self.envir"

这一项是一个空指针，只能在调用"Rcpp"插件时转换为Rcpp::Environment(参见[第9.5节](https://mrgsolve.org/user-guide/plugins.html#seCLplugin-rcpp))。

示例：

```R
[ plugin ] Rcpp mrgx

[ preamble ] 
Rcpp::Environment env = mrgx::get_envir(self);
```

### 2.3.21 "self.tad()" 

这是一个计算并返回最近一次剂量事件记录(EVID等于1或4的任何记录)之后的时间的函数。当在个体内的第一次剂量记录之前调用"self.tad()"时将返回"-1"(NEWIND <= 1)。

每次调用"`$MAIN`"时，都应在"`$MAIN`"中调用此函数。

示例：

```R
$MAIN

double TAD = self.tad();
```

不要让这个计算依赖于任何测试或条件；每次调用"`$MAIN`"时都必须调用它才能看到每一次剂量。

### 2.3.22 self.mtime(\<time\>) 

这是一个创建建模偶数时间的函数。唯一的参数是当前个体未来的数字时间，它指示何时应该在模拟中包含不连续性。

当调用"self. mtime()"时，一个新的记录被添加到当前个人的记录集中，"EVID"设置为2。这意味着当系统前进到这个记录(这次)时，微分方程求解器将重置并重新启动，从而产生不连续性。该函数返回这个事件的时间，以便用户可以在后续代码中使用它。

比如说，

```R
$PARAM change_point = 5.13;

$MAIN

double KA = 1.1;

double mt1 = self.mtime(change_point);

if(TIME >= mt1) KA = 2.1; 
```

### 2.3.23 self.mevent(\<time\>, \<evid\>)

与"self. mtime()"([第2.3.22节](https://mrgsolve.org/user-guide/specification.html#seCLself.mtime))相关，除了您可以为此干预设置特定的"EVID"，并且您可以通过"EVID"而不是时间跟踪更改时间。如果您想在未来锚定多个事件并确保能够区分它们，您可以使用它。示例：

```R
[ main ] 
self.mevent(change_point1, 33);
self.mevent(change_point2, 34);
```

现在，您可以查找"EVID==33"以知道您已经点击了"change_point1"或"EVID==34"以指示您已经点击了"change_point2"。请注意" self.mevent() "不返回任何值。您将通过选中"EVID"来知道您已经点击了更改时间。

### 2.3.24 "self.stop()"

这个"self"函数可以从"`$PREAMBLE`"、"`$MAIN`"和"`$TABLE`"调用。调用此函数时，整个问题将在处理下一个模拟记录时停止。

当发生了非常糟糕的事情并且您只想以错误停止模拟时，可能会调用此函数。

### 2.3.25" self.stop_id()"

可以从"`$PREAMBLE`"、"`$MAIN`"和"`$TABLE`"调用此"self"函数。调用此函数时，停止当前个体的处理，并为剩余房室'填写缺失值("NA_real")并捕获输出。

当当前个体达到某种条件，表明其余输出无关紧要或该特定个体有问题时，可以调用此项。

另请参见"self.stop_id()"和"self. stop()"。

### 2.3.26"self. stop_id_cf()"

可以从"`$PREAMBLE`"、"`$MAIN`"和"`$TABLE`"调用此"self"函数。调用此函数时，停止当前个体的处理，并将当前值结转("cf", carried forward)用于该个体的剩余输出记录。

当当前个体达到某种条件，表明其余输出无关紧要或该特定个体有问题时，可以调用此项。

另请参见"self.stop_id_cf()"和"self. stop()"。

### 2.3.27"THETA(n)"

"THETA(n)"在NONMEM控制流中无处不在，表示估计的固定效应参数。mrgsolve没有为"THETA(n)"附加任何特殊含义，但从1.0.0版开始，它将"THETA(n)"翻译为"THETAn"。这在使用"`$NMXML`"或"`$NMEXT`"导入NONMEM估计时很有用，其中"THETA"作为参数导入，名称为"THETA1"、"THETA2"、"THETA3"等(参见[第2.2.23节](https://mrgsolve.org/user-guide/specification.html#seCLblock-nmxml)和[第2.2.24节](https://mrgsolve.org/user-guide/specification.html#seCLblock-nmext))。"THETA(n)"只是一种面向NONMEM的风格，用于指代"THETAn"。

### 2.3.28"预计到达时间(n)"

"ETA(n)"是从模型"OMEGA"矩阵中提取的"个体"水平的变量的值。"ETA(1)"到"ETA(25)"的默认值为零，因此即使没有提供适当的"OMEGA"矩阵，它们也可以在模型中使用。

例如：

```R
$OMEGA
1 2 3

$MAIN
double CL = TVCL*exp(ETA(1));
double VC = TVVC*exp(ETA(2));
double KA = TVKA*exp(ETA(3));
```

这里，我们有一个3x3的"OMEGA"矩阵。"ETA(1)"、"ETA(2)"和"ETA(3)"将填充从该矩阵中提取的变量。"ETA(4)"到"ETA(25)"将填充为零。

### 2.3.29"EPS(n)"

"EPS(n)"保存从"SIGMA"中提取"观测"水平的随机变量的当前值。基本设置与"ETA(n)"中详述的相同。

示例：

```R
[ CMT ] CENT

[ PARAM ] CL=1, VC=20

[ SIGMA ] 
25 0.0025

[ TABLE ]
double DV = (CENT/VC)*(1+EPS(2)) + EPS(1);
```

### 2.3.30"SIGMA(n)"

从版本1.0.8开始，用户可以通过SIGMA()宏以只读的方式访问∑的对角线上的元素。【译者注：SIGMA 矩阵是一个方差协方差矩阵，其中的对角线元素是方差值，具体来说是EPS的方差】。示例：

```R
$SIGMA 0.1 12

$TABLE
double STD = sqrt(SIGMA(1) + pow(F,2)*SIGMA(2));
```

### 2.3.31 table(\name\>)

此宏已被弃用。用户不应使用如下的代码：

```R
[ TABLE ]
table(CP) = CENT/VC;
```

而是应写为如下的代码：

```R
$TABLE 
double CP = CENT/VC;

$CAPTURE CP
```

见：[第2.2.11节](https://mrgsolve.org/user-guide/specification.html#seCLblock-table)和[第2.2.15节](https://mrgsolve.org/user-guide/specification.html#seCLblock-capture)。

### 2.3.32"F_CMT"

对于"CMT"房室，设置该房室的生物利用度分数。

示例：

```R
$MAIN
F_CENT = 0.7;
```

### 2.3.33"ALAG_CMT"

对于"CMT"房室，设置剂量进入该房室的滞后时间。

示例：

```R
$MAIN
ALAG_GUT = 0.25;
```

### 2.3.34"R_CMT"

对于"CMT"房室，设置该房室的输液速率。只有当数据集或事件对象中的"速率"设置为"-1"时，才能通过"R_CMT"设置输液速率。

示例：

```R
$MAIN
R_CENT = 100;
```

### 2.3.35"Rn"

仅在调用"nm-vars"插件时可用；请参阅"R_CMT"。

### 2.3.36"D_CMT"

对于"CMT"房室，设置该房室的输液持续时间。

只有当数据集或事件对象中的"速率"设置为"-2"时，才通过"D_CMT"设置输注持续时间。

示例：

```R
$MAIN
D_CENT = 2;
```

### 2.3.37类NONMEM语法

有一种类似NONMEM的语法允许您编写"F1"而不是"F_GUT"、"D2"而不是"D_CENT"以及NONMEM控制流中常用的其他变量。要使这种语法可用，必须调用"nm-vars"插件。

有关详细信息，请参阅"nm-vars"插件的[第9.2节](https://mrgsolve.org/user-guide/plugins.html#seCLplugin-nm-vars)。

## 2.4保留字

保留字不能用作模型中参数、房室或其他衍生变量的名称。**请注意**，其中一些词是"保留"供您在数据集中使用的。

```R
ID
amt
cmt
ii
ss
evid
addl
rate
time
SOLVERTIME
table
ETA
EPS
AMT
CMT
ID
TIME
EVID
simeps
self
simeta
NEWIND
DONE
CFONSTOP
DXDTZERO
CFONSTOP
INITSOLV
_F
_R
_ALAG
SETINIT
report
_VARS_
VARS
SS_ADVANCE
AMT
CMT
II
SS
ADDL
RATE
THETA
pred_CL
pred_VC
pred_V
pred_V2
pred_KA
pred_Q
pred_VP
pred_V3
double
int
bool
capture
```

其他保留字取决于模型中的房室名称。例如，如果您在模型中有一个名为"CENT"的房室，那么以下内容将被保留

```R
F_CENT
R_CENT
D_CENT 
ALAG_CENT
N_CENT
```

## 2.5衍生新变量

新的C++变量可以在"`$GLOBAL`"、"`$PREAMBLE`"、"`$MAIN`"、"`$ODE`"和"`$TABLE`"中衍生。因为这些是C++变量，所以必须声明正在使用的变量类型。对于绝大多数应用程序，使用"双精度"类型(双精度数值)。

```R
$MAIN

double CLi = TVCL*exp(ETA(1));
```

我们希望"CLi"是一个数值，所以我们使用"双精度"。要初始化"布尔"变量(真/假)，请编写

```R
$MAIN
bool cure = false;
```

### 2.5.1对"双精度double"、"整数int"、"布尔bool"的特殊处理

当double、int和bool类型的变量在`$PREAMBLE`、`$MAIN`、`$ODE`、`$TABLE`中声明和初始化时，mrgsolve将检测这些声明，并修改代码，以便变量在`$GLOBAL`中实际声明一次，而不是在`$MAIN`、`$ODE`或`$TABLE`中声明一次。这样做是为了在一个代码块(例如"`$MAIN`")中声明的变量可以在另一个代码块(例如"`$TABLE`")中读取和修改。

例如，在以下代码中：

```R
$MAIN
double CLi = TVCL*exp(ETA(1));
```

在"`$MAIN`"块中创建了一个双精度数字变量("CLi")。当mrgsolve解析模型文件时，此代码被翻译为

```R
$GLOBAL
namespace {
 double CLi;
}

$MAIN
CLi = TVCL*exp(ETA(1));
```

也就是说，"CLi"在未命名的命名空间的"`$GLOBAL`"中声明，因此像这样的变量仅是模型文件中的全局变量。

这样，我们仍然可以读取"`$TABLE`"中的"CLi"变量：

```R
$MAIN
double CLi = TVCL*exp(ETA(1));
double VCi = TVVC*exp(ETA(2));

$TABLE
double KEi = CLi/VCi;

$CAPTURE KEi
```

要声明特定代码块的本地变量：

```R
$MAIN

localdouble CLi = TVCL*exp(ETA(1));
```

"localDouble"类型仍然只是一个双精度变量。不同之处在于它受到保护，免受此重新声明过程的影响，并且该变量将是(在本例中)"`$MAIN`"块的本地变量。

### 2.5.2全局使用其他类型

正如我们在上一节中指出的，"Double"、"int"和"bool"以一种特殊的方式处理，因此它们默认是文件的全局变量。很多时候，我们希望以全局方式使用其他变量类型。每当您希望跨函数(例如"`$MAIN`"、"`$TABLE`"等)访问数据结构时，它们应该在"`$GLOBAL`"中声明，可以选择在未命名的命名空间中声明。

例如：

```R
[ GLOBAL ]
std::vector<double> myvec;
```

或者

```R
[ GLOBAL ]
namespace {
 std::vector<double> myvec;
}
```

如果该对象在开始问题之前需要一些配置，请使用"`$PREAMBLE`"来完成这项工作

```R
[ GLOBAL ]
std::vector<double> myvec;

[ PREAMBLE ] 
myvec.assign(3,1);
```

## 2.6随机数生成

用户可以使用与R中通常使用的函数类似的函数(例如"rnorm()"和"runif()")在模型文件中模拟随机数。此功能由"Rcpp"提供，因此需要使用"Rcpp"插件(参见[第9章](https://mrgsolve.org/user-guide/plugins.html)和[第9.5节](https://mrgsolve.org/user-guide/plugins.html#seCLplugin-rcpp))。

"Rcpp"在"R"命名空间中提供了这些函数，因此您必须在函数调用前加上R::。

例如，从Uniform(0,1)中抽取

```R
[ plugin ] Rcpp
 
[ error ] 
double draw = R::runif(0,1);
```

请注意，此处"0"用作"min"，"1"用作"max"；

我们没有在这里传递"n"，并且"Draw"是一个数字(不是您从中获得的向量(R控制台上的"runif(100,0,1)")。所以一般来说，这些函数的工作方式与它们的"R"对应物一样，但没有"n"参数。

另一个示例显示如何从概率为0.5的二项式分布中绘制

```R
[ plugin ] Rcpp

[ error ] 
double draw = R::rbinom(1, 0.5);
```

这里，"1"用作"size"(不是"n")，"0.5"用作"prob"。

其他有用的函数可以是R::rnorm()或R::rlnorm()，但是您可以通过这个"R"命名空间调用任何"r"函数以及相应的"dpq"函数。

文档，包括要调用的函数和参数可以在Rcpp API文档中找到

[http://dirk.eddelbuettel.com/code/rcpp/html/namespaceR.html](http://dirk.eddelbuettel.com/code/rcpp/html/namespaceR.html)

## 2.7示例

以下部分展示了示例模型描述。目的是展示不同的块、宏和变量如何协同工作以形成功能模型。给出一些模型纯粹是为了说明目的，在应用中可能不是特别有用。

### 2.7.1简单PK模型

注意事项：

- 基本PK参数在"`$PARAM`"中声明；每个参数都需要赋值

- 在"`$CMT`"中声明了两个房室"GUT"和"CENT"；使用"`$CMT`"假定两个房室都以0质量开头

- 因为我们将"GUT"和"CENT"声明为房室，所以我们在"`$ODE`"中为两者编写"dxdt_"方程

- 在"`$ODE`"中，我们指的是参数(CL/VC/KA)和每个房室在任何特定时间的数量("GUT"和"CENT")

- "`$ODE`"应该是C++代码；每行以;结尾

- 我们在"`$TABLE`"中派生了一个名为"CP"的变量，它的类型为"capture捕获"；mrgsolve将在"`$CAPTURE`"块列表中输入"CP"名称

```R
$PARAM CL = 1, VC = 30, KA = 1.3

$CMT GUT CENT

$ODE

dxdt_GUT = -KA*GUT;
dxdt_CENT = KA*GUT - (CL/VC)*CENT;

$TABLE
capture CP = CENT/VC;
```

这个模型也可以不用微分方程来写

```R
[ PARAM ] CL = 1, VC = 30, KA = 1.3

[ PKModel ] cmt = "CMT GUT CENT", depot = TRUE

$TABLE
capture CP = CENT/VC;
```

### 2.7.2PK/PD模型

注意事项：

- 我们在"`$GLOBAL`"中使用预处理器#define指令；在模型中找到"CP"标记的任何地方，都会插入表达式(CENT/VC)…带括号…

- 我们将"RESP"房室的初始值写入"`$MAIN`"中，作为一个包含有两个参数的函数KIN/KOUT

- 在"`$ODE`"中声明并使用了一个新变量-"INH"

- 由于"CP"定义为CENT/VC，我们可以在"`$CAPTURE`"中"捕获"该名称/值

- "`$MAIN`"和"`$ODE`"都是C++代码块；不要忘记在每个语句的末尾添加;

```R
$PARAM CL = 1, VC = 30, KA = 1.3
KIN = 100, KOUT = 2, IC50 = 2

$GLOBAL
#define CP (CENT/VC)

$CMT GUT CENT RESP

$MAIN
RESP_0 = KIN/KOUT;

$ODE

double INH = CP/(IC50+CP);

dxdt_GUT = -KA*GUT;
dxdt_CENT = KA*GUT - (CL/VC)*CENT;
dxdt_RESP = KIN*(1-INH) - KOUT*RESP;

$CAPTURE CP
```

### 2.7.3具有协变量和IOV的群体PK模型

注意事项：

- 使用"`$SET`"将模拟时间网格设置为从0到240每次0.1

- 有两个"`$OMEGA`"矩阵；我们将它们命名为"IIV"和"IOV"

- IIV"etas"标记为ECL/EVC/EKA；这些是ETA(1)/ETA(2)/ETA(3)的别名。"IOV"矩阵未标记；为此，我们必须参考ETA(4)/ETA(5)

- 因为"ETA(1)"和"ETA(2)"被标记，所以我们可以将它们"捕获"为"ECL"和"EVC"

- 我们为两个"`$OMEGA`"矩阵添加了零；在我们填充这些矩阵之前，所有的eta都将为零([第5.3节](https://mrgsolve.org/user-guide/matrix.html#seCLmatrix-update))

```R
$PARAM TVCL = 1.3, TVVC=28, TVKA=0.6, WT=70, OCC=1

$SET delta=0.1, end=240

$CMT GUT CENT

$MAIN

double IOV = IOV1
if(OCC==2) IOV = IOV2;

double CLi = exp(log(TVCL) + 0.75*log(WT/70) + ECL + IOV);
double VCi = exp(log(TVVC) + EVC);
double KAi = exp(log(TVKA) + EKA);

$OMEGA @name IIV @labels ECL EVC EKA
0 0 0
$OMEGA @name IOV @labels IOV1 IOV2
0 0

$SIGMA 0

$ODE
dxdt_GUT = -KAi*GUT;
dxdt_CENT = KAi*GUT - (CLi/VCi)*CENT;

$TABLE
capture CP = CENT/VCi;

$CAPTURE IOV ECL EVC
```
