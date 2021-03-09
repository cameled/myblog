---
title: constexpr
date: 2021-03-09 09:21:53
tags: C++
---

# const和constexpr

 C++源代码如下：

 ```C++
int print()
{
    return 0;
}

int main()
{
    // 常量定义，编译器保证a不会被修改，但a的值需要在运行时确认。
    const int a = print();
    const int b = 7 + a;

    // 常量表达式，编译器保证x不会被修改，
    // 同时如果后续有用到x，编译器会直接用x的值进行替换。
    constexpr int x = sizeof(decltype(print()));
    constexpr int y = 1 + x;

    return b + y;
}
 ```

 汇编代码如下：
 
 ```ASM
main:                                   # @main
	.cfi_startproc
# %bb.0:
	pushq	%rbp
	.cfi_def_cfa_offset 16
	.cfi_offset %rbp, -16
	movq	%rsp, %rbp
	.cfi_def_cfa_register %rbp
	subq	$32, %rsp
	movl	$0, -4(%rbp)
	callq	_Z5printv
	# a = print()，运行时确定返回值。
	movl	%eax, -8(%rbp)
	movl	-8(%rbp), %eax
	addl	$7, %eax
	# b = 7 + a。
	movl	%eax, -12(%rbp)
	# x = 4。
	movl	$4, -16(%rbp)
	# y = 5，即1+4。在编译时就以确认y的值。
	movl	$5, -20(%rbp)
	movl	-12(%rbp), %eax
	# y + b 作为返回值。
	addl	$5, %eax
	addq	$32, %rsp
	popq	%rbp
	.cfi_def_cfa %rsp, 8
	retq
 ```

 # 小结

constexpr是对const的一种补充，强制使用字面值类型(literal type)来进行初始化，同时编译器会负责检查constexpr的赋值类型是否符合要求。
