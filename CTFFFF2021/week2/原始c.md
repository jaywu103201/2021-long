## 原始c語言
```
// helloCTFer.c  1.c
#include <stdio.h>

int main()
{
   printf("Hello CTFer\n");
   return 0;
}
```

## 1.產生預設的AT&T 語法的組合語言
```
[32bit與64bit組合語言不一樣!!]

gcc -S 1.c -o 1.s


```

## gcc -S 1.C -o 1.s
```
ksu@KSU-Ubuntu-1604-32:~$ gedit 1.c
ksu@KSU-Ubuntu-1604-32:~$ gcc -S 1.C -o 1.s
gcc: error: 1.C: No such file or directory
gcc: fatal error: no input files
compilation terminated.
ksu@KSU-Ubuntu-1604-32:~$ gcc -S 1.c -o 1.s
ksu@KSU-Ubuntu-1604-32:~$
```


## cat 1.s
```
ksu@KSU-Ubuntu-1604-32:~$ gcc -S 1.c -o 1.s
ksu@KSU-Ubuntu-1604-32:~$ cat 1.s
	.file	"1.c"
	.section	.rodata
.LC0:
	.string	"Hello CTFer"
	.text
	.globl	main
	.type	main, @function
main:
.LFB0:
	.cfi_startproc
	leal	4(%esp), %ecx
	.cfi_def_cfa 1, 0
	andl	$-16, %esp
	pushl	-4(%ecx)
	pushl	%ebp
	.cfi_escape 0x10,0x5,0x2,0x75,0
	movl	%esp, %ebp
	pushl	%ecx
	.cfi_escape 0xf,0x3,0x75,0x7c,0x6
	subl	$4, %esp
	subl	$12, %esp
	pushl	$.LC0
	call	puts
	addl	$16, %esp
	movl	$0, %eax
	movl	-4(%ebp), %ecx
	.cfi_def_cfa 1, 0
	leave
	.cfi_restore 5
	leal	-4(%ecx), %esp
	.cfi_def_cfa 4, 4
	ret
	.cfi_endproc
.LFE0:
	.size	main, .-main
	.ident	"GCC: (Ubuntu 5.4.0-6ubuntu1~16.04.12) 5.4.0 20160609"
	.section	.note.GNU-stack,"",@progbits

```

##
```

```
