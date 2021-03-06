"cmath" --- 关于复数的数学函数
******************************

======================================================================

这一模块提供了一些关于复数的数学函数。 该模块的函数的参数为整数、浮点
数或复数。 这些函数的参数也可为一个拥有 "__complex__()" 或
"__float__()" 方法的 Python 对象，这些方法分别用于将对象转换为复数和浮
点数，这些函数作用于转换后的结果。

注解:

  在具有对于有符号零的硬件和系统级支持的平台上，涉及分歧点的函数在分歧
  点的 *两侧* 都是连续的：零的符号可用来区别分歧点的一侧和另一侧。 在
  不支持有符号零的平台上，连续性的规则见下文。


到极坐标和从极坐标的转换
========================

使用 *矩形坐标* 或 *笛卡尔坐标* 在内部存储 Python 复数 "z"。 这完全取
决于它的 *实部* "z.real" 和 *虚部* "z.imag"。 换句话说:

   z == z.real + z.imag*1j

*极坐标* 提供了另一种复数的表示方法。在极坐标中，一个复数 *z* 由模量
*r* 和相位角 *phi* 来定义。模量 *r* 是从 *z* 到坐标原点的距离，而相位
角 *phi* 是以弧度为单位的，逆时针的，从正X轴到连接原点和 *z* 的线段间
夹角的角度。

下面的函数可用于原生直角坐标与极坐标的相互转换。

cmath.phase(x)

   将 *x* 的相位 (也称为 *x* 的 *参数*) 返回为一个浮点数。"phase(x)"
   相当于 "math.atan2(x.imag, x.real)"。 结果处于 [-*π*, *π*] 之间，以
   及这个操作的分支切断处于负实轴上，从上方连续。 在支持有符号零的系统
   上（这包涵大多数当前的常用系统），这意味着结果的符号与 "x.imag" 的
   符号相同，即使 "x.imag" 的值是 0:

      >>> phase(complex(-1.0, 0.0))
      3.141592653589793
      >>> phase(complex(-1.0, -0.0))
      -3.141592653589793

注解:

  一个复数 *x* 的模数（绝对值）可以通过内置函数 "abs()" 计算。没有单独
  的 "cmath" 模块函数用于这个操作。

cmath.polar(x)

   在极坐标中返回 *x* 的表达方式。返回一个数对 "(r, phi)"，*r* 是 *x*
   的模数，*phi* 是 *x* 的相位角。 "polar(x)" 相当于 "(abs(x),
   phase(x))"。

cmath.rect(r, phi)

   通过极坐标的 *r* 和 *phi* 返回复数 *x*。相当于 "r * (math.cos(phi)
   + math.sin(phi)*1j)"。


幂函数与对数函数
================

cmath.exp(x)

   返回 *e* 的 *x* 次方，*e* 是自然对数的底数。

cmath.log(x[, base])

   返回给定 *base* 的 *x* 的对数。如果没有给定 *base*，返回 *x* 的自然
   对数。 从 0 到 -∞ 存在一个分歧点，沿负实轴之上连续。

cmath.log10(x)

   返回底数为 10 的 *x* 的对数。它具有与 "log()" 相同的分歧点。

cmath.sqrt(x)

   返回 *x* 的平方根。 它具有与 "log()" 相同的分歧点。


三角函数
========

cmath.acos(x)

   返回 *x* 的反余弦。这里有两个分歧点：一个沿着实轴从 1 向右延伸到 ∞
   ，从下面连续延伸。另外一个沿着实轴从 -1 向左延伸到 -∞，从上面连续延
   伸。

cmath.asin(x)

   返回 *x* 的反正弦。它与 "acos()" 有相同的分歧点。

cmath.atan(x)

   返回 *x* 的反正切。它具有两个分歧点：一个沿着虚轴从 "1j"  延伸到
   "∞j"，向右持续延伸。另一个是沿着虚轴从 "-1j" 延伸到 "-∞j" ，向左持
   续延伸。

cmath.cos(x)

   返回 *x* 的余弦。

cmath.sin(x)

   返回 *x* 的正弦。

cmath.tan(x)

   返回 *x* 的正切。


双曲函数
========

cmath.acosh(x)

   返回 *x* 的反双曲余弦。它有一个分歧点沿着实轴从 1 到  -∞ 向左延伸，
   从上方持续延伸。

cmath.asinh(x)

   返回 *x* 的反双曲正弦。它有两个分歧点：一个沿着虚轴从 "1j" 向右持续
   延伸到 "∞j"。另一个是沿着虚轴从 "-1j" 向左持续延伸到 "-∞j"。

cmath.atanh(x)

   返回 *x* 的反双曲正切。它有两个分歧点：一个是沿着实轴从 "1" 延展到
   "∞"，从下面持续延展。另一个是沿着实轴从 "-1" 延展到 "-∞"，从上面持
   续延展。

cmath.cosh(x)

   返回 *x* 的双曲余弦值。

cmath.sinh(x)

   返回 *x* 的双曲正弦值。

cmath.tanh(x)

   返回 *x* 的双曲正切值。


分类函数
========

cmath.isfinite(x)

   如果 *x* 的实部和虚部都是有限的，则返回 "True"，否则返回 "False"。

   3.2 新版功能.

cmath.isinf(x)

   如果 *x* 的实部或者虚部是无穷大的，则返回 "True"，否则返回 "False"
   。

cmath.isnan(x)

   如果 *x* 的实部或者虚部是 NaN，则返回 "True" ，否则返回 "False"。

cmath.isclose(a, b, *, rel_tol=1e-09, abs_tol=0.0)

   若 *a* 和 *b* 的值比较接近则返回 "True"，否则返回 "False"。

   根据给定的绝对和相对容差确定两个值是否被认为是接近的。

   *rel_tol* 是相对容差 —— 它是 *a* 和 *b* 之间允许的最大差值，相对于
   *a* 或 *b* 的较大绝对值。例如，要设置5％的容差，请传递
   "rel_tol=0.05" 。默认容差为 "1e-09"，确保两个值在大约9位十进制数字
   内相同。 *rel_tol* 必须大于零。

   *abs_tol* 是最小绝对容差 —— 对于接近零的比较很有用。 *abs_tol* 必须
   至少为零。

   如果没有错误发生，结果将是： "abs(a-b) <= max(rel_tol * max(abs(a),
   abs(b)), abs_tol)" 。

   IEEE 754特殊值 "NaN" ， "inf" 和` *-inf`* 将根据IEEE规则处理。具体
   来说， "NaN" 不被认为接近任何其他值，包括 "NaN" 。 "inf" 和 "-inf"
   只被认为接近自己。

   3.5 新版功能.

   参见: **PEP 485** —— 用于测试近似相等的函数


常数
====

cmath.pi

   数学常数 *π* ，作为一个浮点数。

cmath.e

   数学常数 *e* ，作为一个浮点数。

cmath.tau

   数学常数 *τ* ，作为一个浮点数。

   3.6 新版功能.

cmath.inf

   浮点正无穷大。相当于 "float('inf')"。

   3.6 新版功能.

cmath.infj

   具有零实部和正无穷虚部的复数。相当于 "complex(0.0, float('inf'))"。

   3.6 新版功能.

cmath.nan

   浮点“非数字”（NaN）值。相当于 "float('nan')"。

   3.6 新版功能.

cmath.nanj

   具有零实部和 NaN 虚部的复数。相当于 "complex(0.0, float('nan'))"。

   3.6 新版功能.

请注意，函数的选择与模块 "math" 中的函数选择相似，但不完全相同。 拥有
两个模块的原因是因为有些用户对复数不感兴趣，甚至根本不知道它们是什么。
它们宁愿 "math.sqrt(-1)" 引发异常，也不想返回一个复数。 另请注意，被
"cmath" 定义的函数始终会返回一个复数，尽管答案可以表示为一个实数（在这
种情况下，复数的虚数部分为零）。

关于分歧点的注释：它们是沿着给定函数无法连续的曲线。它们是许多复杂函数
的必要特征。假设您需要使用复杂函数进行计算，您将了解分歧点。请参阅几乎
所有关于复杂变量的（不太基本）的书来进行启发。关于分歧点数值目的的正确
选择信息，应提供以下良好参考：

参见:

  Kahan, W: Branch cuts for complex elementary functions; or, Much ado
  about nothing's sign bit. In Iserles, A., and Powell, M. (eds.), The
  state of the art in numerical analysis. Clarendon Press (1987) pp165
  --211.
