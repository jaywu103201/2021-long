# 32位元

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

## 產生intel 語法的組合語言
```
 gcc -S -masm=intel 1.c -o func1_intel.s
 
 
 
 ksu@KSU-Ubuntu-1604-32:~$    gcc -S -masm=intel 1.c -o func1_intel.s
ksu@KSU-Ubuntu-1604-32:~$ cat func1_intel.s 
	.file	"1.c"
	.intel_syntax noprefix
	.section	.rodata
.LC0:
	.string	"Hello CTFer"
	.text
	.globl	main
	.type	main, @function
main:
.LFB0:
	.cfi_startproc
	lea	ecx, [esp+4]
	.cfi_def_cfa 1, 0
	and	esp, -16
	push	DWORD PTR [ecx-4]
	push	ebp
	.cfi_escape 0x10,0x5,0x2,0x75,0
	mov	ebp, esp
	push	ecx
	.cfi_escape 0xf,0x3,0x75,0x7c,0x6
	sub	esp, 4
	sub	esp, 12
	push	OFFSET FLAT:.LC0
	call	puts
	add	esp, 16
	mov	eax, 0
	mov	ecx, DWORD PTR [ebp-4]
	.cfi_def_cfa 1, 0
	leave
	.cfi_restore 5
	lea	esp, [ecx-4]
	.cfi_def_cfa 4, 4
	ret
	.cfi_endproc
.LFE0:
	.size	main, .-main
	.ident	"GCC: (Ubuntu 5.4.0-6ubuntu1~16.04.12) 5.4.0 20160609"
	.section	.note.GNU-stack,"",@progbits

```



## gcc -S -masm=intel func1.c -o func1_intel.s -fno-asynchronous-unwind-tables
```

ksu@KSU-Ubuntu-1604-32:~$ gcc -S -masm=intel 1.c -o func1_intel.s -fno-asynchronous-unwind-tables
ksu@KSU-Ubuntu-1604-32:~$ cat func1_intel.s 
	.file	"1.c"
	.intel_syntax noprefix
	.section	.rodata
.LC0:
	.string	"Hello CTFer"
	.text
	.globl	main
	.type	main, @function
main:
	lea	ecx, [esp+4]
	and	esp, -16
	push	DWORD PTR [ecx-4]
	push	ebp
	mov	ebp, esp
	push	ecx
	sub	esp, 4
	sub	esp, 12
	push	OFFSET FLAT:.LC0
	call	puts
	add	esp, 16
	mov	eax, 0
	mov	ecx, DWORD PTR [ebp-4]
	leave
	lea	esp, [ecx-4]
	ret
	.size	main, .-main
	.ident	"GCC: (Ubuntu 5.4.0-6ubuntu1~16.04.12) 5.4.0 20160609"
	.section	.note.GNU-stack,"",@progbits

```


# 64位元

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


```
ksu@KSU-Ubuntu-1804-64:~$ cat 1.s
	.file	"1.c"
	.text
	.section	.rodata
.LC0:
	.string	"Hello CTFer"
	.text
	.globl	main
	.type	main, @function
main:
.LFB0:
	.cfi_startproc
	pushq	%rbp
	.cfi_def_cfa_offset 16
	.cfi_offset 6, -16
	movq	%rsp, %rbp
	.cfi_def_cfa_register 6
	leaq	.LC0(%rip), %rdi
	call	puts@PLT
	movl	$0, %eax
	popq	%rbp
	.cfi_def_cfa 7, 8
	ret
	.cfi_endproc
.LFE0:
	.size	main, .-main
	.ident	"GCC: (Ubuntu 7.5.0-3ubuntu1~18.04) 7.5.0"
	.section	.note.GNU-stack,"",@progbits

```

```
ksu@KSU-Ubuntu-1804-64:~$ gcc -S -masm=intel 1.c -o func1_intel.s
ksu@KSU-Ubuntu-1804-64:~$ cat func1_intel.s 
	.file	"1.c"
	.intel_syntax noprefix
	.text
	.section	.rodata
.LC0:
	.string	"Hello CTFer"
	.text
	.globl	main
	.type	main, @function
main:
.LFB0:
	.cfi_startproc
	push	rbp
	.cfi_def_cfa_offset 16
	.cfi_offset 6, -16
	mov	rbp, rsp
	.cfi_def_cfa_register 6
	lea	rdi, .LC0[rip]
	call	puts@PLT
	mov	eax, 0
	pop	rbp
	.cfi_def_cfa 7, 8
	ret
	.cfi_endproc
.LFE0:
	.size	main, .-main
	.ident	"GCC: (Ubuntu 7.5.0-3ubuntu1~18.04) 7.5.0"
	.section	.note.GNU-stack,"",@progbits

```

##  objdump -S 1

```

ksu@KSU-Ubuntu-1604-32:~$ gcc 1.c -o 1 -g
ksu@KSU-Ubuntu-1604-32:~$ objdump -S 1

1:     file format elf32-i386


Disassembly of section .init:

080482a8 <_init>:
 80482a8:	53                   	push   %ebx
 80482a9:	83 ec 08             	sub    $0x8,%esp
 80482ac:	e8 8f 00 00 00       	call   8048340 <__x86.get_pc_thunk.bx>
 80482b1:	81 c3 4f 1d 00 00    	add    $0x1d4f,%ebx
 80482b7:	8b 83 fc ff ff ff    	mov    -0x4(%ebx),%eax
 80482bd:	85 c0                	test   %eax,%eax
 80482bf:	74 05                	je     80482c6 <_init+0x1e>
 80482c1:	e8 3a 00 00 00       	call   8048300 <__libc_start_main@plt+0x10>
 80482c6:	83 c4 08             	add    $0x8,%esp
 80482c9:	5b                   	pop    %ebx
 80482ca:	c3                   	ret    

Disassembly of section .plt:

080482d0 <puts@plt-0x10>:
 80482d0:	ff 35 04 a0 04 08    	pushl  0x804a004
 80482d6:	ff 25 08 a0 04 08    	jmp    *0x804a008
 80482dc:	00 00                	add    %al,(%eax)
	...

080482e0 <puts@plt>:
 80482e0:	ff 25 0c a0 04 08    	jmp    *0x804a00c
 80482e6:	68 00 00 00 00       	push   $0x0
 80482eb:	e9 e0 ff ff ff       	jmp    80482d0 <_init+0x28>

080482f0 <__libc_start_main@plt>:
 80482f0:	ff 25 10 a0 04 08    	jmp    *0x804a010
 80482f6:	68 08 00 00 00       	push   $0x8
 80482fb:	e9 d0 ff ff ff       	jmp    80482d0 <_init+0x28>

Disassembly of section .plt.got:

08048300 <.plt.got>:
 8048300:	ff 25 fc 9f 04 08    	jmp    *0x8049ffc
 8048306:	66 90                	xchg   %ax,%ax

Disassembly of section .text:

08048310 <_start>:
 8048310:	31 ed                	xor    %ebp,%ebp
 8048312:	5e                   	pop    %esi
 8048313:	89 e1                	mov    %esp,%ecx
 8048315:	83 e4 f0             	and    $0xfffffff0,%esp
 8048318:	50                   	push   %eax
 8048319:	54                   	push   %esp
 804831a:	52                   	push   %edx
 804831b:	68 a0 84 04 08       	push   $0x80484a0
 8048320:	68 40 84 04 08       	push   $0x8048440
 8048325:	51                   	push   %ecx
 8048326:	56                   	push   %esi
 8048327:	68 0b 84 04 08       	push   $0x804840b
 804832c:	e8 bf ff ff ff       	call   80482f0 <__libc_start_main@plt>
 8048331:	f4                   	hlt    
 8048332:	66 90                	xchg   %ax,%ax
 8048334:	66 90                	xchg   %ax,%ax
 8048336:	66 90                	xchg   %ax,%ax
 8048338:	66 90                	xchg   %ax,%ax
 804833a:	66 90                	xchg   %ax,%ax
 804833c:	66 90                	xchg   %ax,%ax
 804833e:	66 90                	xchg   %ax,%ax

08048340 <__x86.get_pc_thunk.bx>:
 8048340:	8b 1c 24             	mov    (%esp),%ebx
 8048343:	c3                   	ret    
 8048344:	66 90                	xchg   %ax,%ax
 8048346:	66 90                	xchg   %ax,%ax
 8048348:	66 90                	xchg   %ax,%ax
 804834a:	66 90                	xchg   %ax,%ax
 804834c:	66 90                	xchg   %ax,%ax
 804834e:	66 90                	xchg   %ax,%ax

08048350 <deregister_tm_clones>:
 8048350:	b8 1f a0 04 08       	mov    $0x804a01f,%eax
 8048355:	2d 1c a0 04 08       	sub    $0x804a01c,%eax
 804835a:	83 f8 06             	cmp    $0x6,%eax
 804835d:	76 1a                	jbe    8048379 <deregister_tm_clones+0x29>
 804835f:	b8 00 00 00 00       	mov    $0x0,%eax
 8048364:	85 c0                	test   %eax,%eax
 8048366:	74 11                	je     8048379 <deregister_tm_clones+0x29>
 8048368:	55                   	push   %ebp
 8048369:	89 e5                	mov    %esp,%ebp
 804836b:	83 ec 14             	sub    $0x14,%esp
 804836e:	68 1c a0 04 08       	push   $0x804a01c
 8048373:	ff d0                	call   *%eax
 8048375:	83 c4 10             	add    $0x10,%esp
 8048378:	c9                   	leave  
 8048379:	f3 c3                	repz ret 
 804837b:	90                   	nop
 804837c:	8d 74 26 00          	lea    0x0(%esi,%eiz,1),%esi

08048380 <register_tm_clones>:
 8048380:	b8 1c a0 04 08       	mov    $0x804a01c,%eax
 8048385:	2d 1c a0 04 08       	sub    $0x804a01c,%eax
 804838a:	c1 f8 02             	sar    $0x2,%eax
 804838d:	89 c2                	mov    %eax,%edx
 804838f:	c1 ea 1f             	shr    $0x1f,%edx
 8048392:	01 d0                	add    %edx,%eax
 8048394:	d1 f8                	sar    %eax
 8048396:	74 1b                	je     80483b3 <register_tm_clones+0x33>
 8048398:	ba 00 00 00 00       	mov    $0x0,%edx
 804839d:	85 d2                	test   %edx,%edx
 804839f:	74 12                	je     80483b3 <register_tm_clones+0x33>
 80483a1:	55                   	push   %ebp
 80483a2:	89 e5                	mov    %esp,%ebp
 80483a4:	83 ec 10             	sub    $0x10,%esp
 80483a7:	50                   	push   %eax
 80483a8:	68 1c a0 04 08       	push   $0x804a01c
 80483ad:	ff d2                	call   *%edx
 80483af:	83 c4 10             	add    $0x10,%esp
 80483b2:	c9                   	leave  
 80483b3:	f3 c3                	repz ret 
 80483b5:	8d 74 26 00          	lea    0x0(%esi,%eiz,1),%esi
 80483b9:	8d bc 27 00 00 00 00 	lea    0x0(%edi,%eiz,1),%edi

080483c0 <__do_global_dtors_aux>:
 80483c0:	80 3d 1c a0 04 08 00 	cmpb   $0x0,0x804a01c
 80483c7:	75 13                	jne    80483dc <__do_global_dtors_aux+0x1c>
 80483c9:	55                   	push   %ebp
 80483ca:	89 e5                	mov    %esp,%ebp
 80483cc:	83 ec 08             	sub    $0x8,%esp
 80483cf:	e8 7c ff ff ff       	call   8048350 <deregister_tm_clones>
 80483d4:	c6 05 1c a0 04 08 01 	movb   $0x1,0x804a01c
 80483db:	c9                   	leave  
 80483dc:	f3 c3                	repz ret 
 80483de:	66 90                	xchg   %ax,%ax

080483e0 <frame_dummy>:
 80483e0:	b8 10 9f 04 08       	mov    $0x8049f10,%eax
 80483e5:	8b 10                	mov    (%eax),%edx
 80483e7:	85 d2                	test   %edx,%edx
 80483e9:	75 05                	jne    80483f0 <frame_dummy+0x10>
 80483eb:	eb 93                	jmp    8048380 <register_tm_clones>
 80483ed:	8d 76 00             	lea    0x0(%esi),%esi
 80483f0:	ba 00 00 00 00       	mov    $0x0,%edx
 80483f5:	85 d2                	test   %edx,%edx
 80483f7:	74 f2                	je     80483eb <frame_dummy+0xb>
 80483f9:	55                   	push   %ebp
 80483fa:	89 e5                	mov    %esp,%ebp
 80483fc:	83 ec 14             	sub    $0x14,%esp
 80483ff:	50                   	push   %eax
 8048400:	ff d2                	call   *%edx
 8048402:	83 c4 10             	add    $0x10,%esp
 8048405:	c9                   	leave  
 8048406:	e9 75 ff ff ff       	jmp    8048380 <register_tm_clones>

0804840b <main>:
// helloCTFer.c  1.c
#include <stdio.h>

int main()
{
 804840b:	8d 4c 24 04          	lea    0x4(%esp),%ecx
 804840f:	83 e4 f0             	and    $0xfffffff0,%esp
 8048412:	ff 71 fc             	pushl  -0x4(%ecx)
 8048415:	55                   	push   %ebp
 8048416:	89 e5                	mov    %esp,%ebp
 8048418:	51                   	push   %ecx
 8048419:	83 ec 04             	sub    $0x4,%esp
   printf("Hello CTFer\n");
 804841c:	83 ec 0c             	sub    $0xc,%esp
 804841f:	68 c0 84 04 08       	push   $0x80484c0
 8048424:	e8 b7 fe ff ff       	call   80482e0 <puts@plt>
 8048429:	83 c4 10             	add    $0x10,%esp
   return 0;
 804842c:	b8 00 00 00 00       	mov    $0x0,%eax
}
 8048431:	8b 4d fc             	mov    -0x4(%ebp),%ecx
 8048434:	c9                   	leave  
 8048435:	8d 61 fc             	lea    -0x4(%ecx),%esp
 8048438:	c3                   	ret    
 8048439:	66 90                	xchg   %ax,%ax
 804843b:	66 90                	xchg   %ax,%ax
 804843d:	66 90                	xchg   %ax,%ax
 804843f:	90                   	nop

08048440 <__libc_csu_init>:
 8048440:	55                   	push   %ebp
 8048441:	57                   	push   %edi
 8048442:	56                   	push   %esi
 8048443:	53                   	push   %ebx
 8048444:	e8 f7 fe ff ff       	call   8048340 <__x86.get_pc_thunk.bx>
 8048449:	81 c3 b7 1b 00 00    	add    $0x1bb7,%ebx
 804844f:	83 ec 0c             	sub    $0xc,%esp
 8048452:	8b 6c 24 20          	mov    0x20(%esp),%ebp
 8048456:	8d b3 0c ff ff ff    	lea    -0xf4(%ebx),%esi
 804845c:	e8 47 fe ff ff       	call   80482a8 <_init>
 8048461:	8d 83 08 ff ff ff    	lea    -0xf8(%ebx),%eax
 8048467:	29 c6                	sub    %eax,%esi
 8048469:	c1 fe 02             	sar    $0x2,%esi
 804846c:	85 f6                	test   %esi,%esi
 804846e:	74 25                	je     8048495 <__libc_csu_init+0x55>
 8048470:	31 ff                	xor    %edi,%edi
 8048472:	8d b6 00 00 00 00    	lea    0x0(%esi),%esi
 8048478:	83 ec 04             	sub    $0x4,%esp
 804847b:	ff 74 24 2c          	pushl  0x2c(%esp)
 804847f:	ff 74 24 2c          	pushl  0x2c(%esp)
 8048483:	55                   	push   %ebp
 8048484:	ff 94 bb 08 ff ff ff 	call   *-0xf8(%ebx,%edi,4)
 804848b:	83 c7 01             	add    $0x1,%edi
 804848e:	83 c4 10             	add    $0x10,%esp
 8048491:	39 f7                	cmp    %esi,%edi
 8048493:	75 e3                	jne    8048478 <__libc_csu_init+0x38>
 8048495:	83 c4 0c             	add    $0xc,%esp
 8048498:	5b                   	pop    %ebx
 8048499:	5e                   	pop    %esi
 804849a:	5f                   	pop    %edi
 804849b:	5d                   	pop    %ebp
 804849c:	c3                   	ret    
 804849d:	8d 76 00             	lea    0x0(%esi),%esi

080484a0 <__libc_csu_fini>:
 80484a0:	f3 c3                	repz ret 

Disassembly of section .fini:

080484a4 <_fini>:
 80484a4:	53                   	push   %ebx
 80484a5:	83 ec 08             	sub    $0x8,%esp
 80484a8:	e8 93 fe ff ff       	call   8048340 <__x86.get_pc_thunk.bx>
 80484ad:	81 c3 53 1b 00 00    	add    $0x1b53,%ebx
 80484b3:	83 c4 08             	add    $0x8,%esp
 80484b6:	5b                   	pop    %ebx
 80484b7:	c3                   	ret    

```

## objdump -S -M intel 1
```
ksu@KSU-Ubuntu-1604-32:~$ objdump -S -M intel 1

1:     file format elf32-i386


Disassembly of section .init:

080482a8 <_init>:
 80482a8:	53                   	push   ebx
 80482a9:	83 ec 08             	sub    esp,0x8
 80482ac:	e8 8f 00 00 00       	call   8048340 <__x86.get_pc_thunk.bx>
 80482b1:	81 c3 4f 1d 00 00    	add    ebx,0x1d4f
 80482b7:	8b 83 fc ff ff ff    	mov    eax,DWORD PTR [ebx-0x4]
 80482bd:	85 c0                	test   eax,eax
 80482bf:	74 05                	je     80482c6 <_init+0x1e>
 80482c1:	e8 3a 00 00 00       	call   8048300 <__libc_start_main@plt+0x10>
 80482c6:	83 c4 08             	add    esp,0x8
 80482c9:	5b                   	pop    ebx
 80482ca:	c3                   	ret    

Disassembly of section .plt:

080482d0 <puts@plt-0x10>:
 80482d0:	ff 35 04 a0 04 08    	push   DWORD PTR ds:0x804a004
 80482d6:	ff 25 08 a0 04 08    	jmp    DWORD PTR ds:0x804a008
 80482dc:	00 00                	add    BYTE PTR [eax],al
	...

080482e0 <puts@plt>:
 80482e0:	ff 25 0c a0 04 08    	jmp    DWORD PTR ds:0x804a00c
 80482e6:	68 00 00 00 00       	push   0x0
 80482eb:	e9 e0 ff ff ff       	jmp    80482d0 <_init+0x28>

080482f0 <__libc_start_main@plt>:
 80482f0:	ff 25 10 a0 04 08    	jmp    DWORD PTR ds:0x804a010
 80482f6:	68 08 00 00 00       	push   0x8
 80482fb:	e9 d0 ff ff ff       	jmp    80482d0 <_init+0x28>

Disassembly of section .plt.got:

08048300 <.plt.got>:
 8048300:	ff 25 fc 9f 04 08    	jmp    DWORD PTR ds:0x8049ffc
 8048306:	66 90                	xchg   ax,ax

Disassembly of section .text:

08048310 <_start>:
 8048310:	31 ed                	xor    ebp,ebp
 8048312:	5e                   	pop    esi
 8048313:	89 e1                	mov    ecx,esp
 8048315:	83 e4 f0             	and    esp,0xfffffff0
 8048318:	50                   	push   eax
 8048319:	54                   	push   esp
 804831a:	52                   	push   edx
 804831b:	68 a0 84 04 08       	push   0x80484a0
 8048320:	68 40 84 04 08       	push   0x8048440
 8048325:	51                   	push   ecx
 8048326:	56                   	push   esi
 8048327:	68 0b 84 04 08       	push   0x804840b
 804832c:	e8 bf ff ff ff       	call   80482f0 <__libc_start_main@plt>
 8048331:	f4                   	hlt    
 8048332:	66 90                	xchg   ax,ax
 8048334:	66 90                	xchg   ax,ax
 8048336:	66 90                	xchg   ax,ax
 8048338:	66 90                	xchg   ax,ax
 804833a:	66 90                	xchg   ax,ax
 804833c:	66 90                	xchg   ax,ax
 804833e:	66 90                	xchg   ax,ax

08048340 <__x86.get_pc_thunk.bx>:
 8048340:	8b 1c 24             	mov    ebx,DWORD PTR [esp]
 8048343:	c3                   	ret    
 8048344:	66 90                	xchg   ax,ax
 8048346:	66 90                	xchg   ax,ax
 8048348:	66 90                	xchg   ax,ax
 804834a:	66 90                	xchg   ax,ax
 804834c:	66 90                	xchg   ax,ax
 804834e:	66 90                	xchg   ax,ax

08048350 <deregister_tm_clones>:
 8048350:	b8 1f a0 04 08       	mov    eax,0x804a01f
 8048355:	2d 1c a0 04 08       	sub    eax,0x804a01c
 804835a:	83 f8 06             	cmp    eax,0x6
 804835d:	76 1a                	jbe    8048379 <deregister_tm_clones+0x29>
 804835f:	b8 00 00 00 00       	mov    eax,0x0
 8048364:	85 c0                	test   eax,eax
 8048366:	74 11                	je     8048379 <deregister_tm_clones+0x29>
 8048368:	55                   	push   ebp
 8048369:	89 e5                	mov    ebp,esp
 804836b:	83 ec 14             	sub    esp,0x14
 804836e:	68 1c a0 04 08       	push   0x804a01c
 8048373:	ff d0                	call   eax
 8048375:	83 c4 10             	add    esp,0x10
 8048378:	c9                   	leave  
 8048379:	f3 c3                	repz ret 
 804837b:	90                   	nop
 804837c:	8d 74 26 00          	lea    esi,[esi+eiz*1+0x0]

08048380 <register_tm_clones>:
 8048380:	b8 1c a0 04 08       	mov    eax,0x804a01c
 8048385:	2d 1c a0 04 08       	sub    eax,0x804a01c
 804838a:	c1 f8 02             	sar    eax,0x2
 804838d:	89 c2                	mov    edx,eax
 804838f:	c1 ea 1f             	shr    edx,0x1f
 8048392:	01 d0                	add    eax,edx
 8048394:	d1 f8                	sar    eax,1
 8048396:	74 1b                	je     80483b3 <register_tm_clones+0x33>
 8048398:	ba 00 00 00 00       	mov    edx,0x0
 804839d:	85 d2                	test   edx,edx
 804839f:	74 12                	je     80483b3 <register_tm_clones+0x33>
 80483a1:	55                   	push   ebp
 80483a2:	89 e5                	mov    ebp,esp
 80483a4:	83 ec 10             	sub    esp,0x10
 80483a7:	50                   	push   eax
 80483a8:	68 1c a0 04 08       	push   0x804a01c
 80483ad:	ff d2                	call   edx
 80483af:	83 c4 10             	add    esp,0x10
 80483b2:	c9                   	leave  
 80483b3:	f3 c3                	repz ret 
 80483b5:	8d 74 26 00          	lea    esi,[esi+eiz*1+0x0]
 80483b9:	8d bc 27 00 00 00 00 	lea    edi,[edi+eiz*1+0x0]

080483c0 <__do_global_dtors_aux>:
 80483c0:	80 3d 1c a0 04 08 00 	cmp    BYTE PTR ds:0x804a01c,0x0
 80483c7:	75 13                	jne    80483dc <__do_global_dtors_aux+0x1c>
 80483c9:	55                   	push   ebp
 80483ca:	89 e5                	mov    ebp,esp
 80483cc:	83 ec 08             	sub    esp,0x8
 80483cf:	e8 7c ff ff ff       	call   8048350 <deregister_tm_clones>
 80483d4:	c6 05 1c a0 04 08 01 	mov    BYTE PTR ds:0x804a01c,0x1
 80483db:	c9                   	leave  
 80483dc:	f3 c3                	repz ret 
 80483de:	66 90                	xchg   ax,ax

080483e0 <frame_dummy>:
 80483e0:	b8 10 9f 04 08       	mov    eax,0x8049f10
 80483e5:	8b 10                	mov    edx,DWORD PTR [eax]
 80483e7:	85 d2                	test   edx,edx
 80483e9:	75 05                	jne    80483f0 <frame_dummy+0x10>
 80483eb:	eb 93                	jmp    8048380 <register_tm_clones>
 80483ed:	8d 76 00             	lea    esi,[esi+0x0]
 80483f0:	ba 00 00 00 00       	mov    edx,0x0
 80483f5:	85 d2                	test   edx,edx
 80483f7:	74 f2                	je     80483eb <frame_dummy+0xb>
 80483f9:	55                   	push   ebp
 80483fa:	89 e5                	mov    ebp,esp
 80483fc:	83 ec 14             	sub    esp,0x14
 80483ff:	50                   	push   eax
 8048400:	ff d2                	call   edx
 8048402:	83 c4 10             	add    esp,0x10
 8048405:	c9                   	leave  
 8048406:	e9 75 ff ff ff       	jmp    8048380 <register_tm_clones>

0804840b <main>:
// helloCTFer.c  1.c
#include <stdio.h>

int main()
{
 804840b:	8d 4c 24 04          	lea    ecx,[esp+0x4]
 804840f:	83 e4 f0             	and    esp,0xfffffff0
 8048412:	ff 71 fc             	push   DWORD PTR [ecx-0x4]
 8048415:	55                   	push   ebp
 8048416:	89 e5                	mov    ebp,esp
 8048418:	51                   	push   ecx
 8048419:	83 ec 04             	sub    esp,0x4
   printf("Hello CTFer\n");
 804841c:	83 ec 0c             	sub    esp,0xc
 804841f:	68 c0 84 04 08       	push   0x80484c0
 8048424:	e8 b7 fe ff ff       	call   80482e0 <puts@plt>
 8048429:	83 c4 10             	add    esp,0x10
   return 0;
 804842c:	b8 00 00 00 00       	mov    eax,0x0
}
 8048431:	8b 4d fc             	mov    ecx,DWORD PTR [ebp-0x4]
 8048434:	c9                   	leave  
 8048435:	8d 61 fc             	lea    esp,[ecx-0x4]
 8048438:	c3                   	ret    
 8048439:	66 90                	xchg   ax,ax
 804843b:	66 90                	xchg   ax,ax
 804843d:	66 90                	xchg   ax,ax
 804843f:	90                   	nop

08048440 <__libc_csu_init>:
 8048440:	55                   	push   ebp
 8048441:	57                   	push   edi
 8048442:	56                   	push   esi
 8048443:	53                   	push   ebx
 8048444:	e8 f7 fe ff ff       	call   8048340 <__x86.get_pc_thunk.bx>
 8048449:	81 c3 b7 1b 00 00    	add    ebx,0x1bb7
 804844f:	83 ec 0c             	sub    esp,0xc
 8048452:	8b 6c 24 20          	mov    ebp,DWORD PTR [esp+0x20]
 8048456:	8d b3 0c ff ff ff    	lea    esi,[ebx-0xf4]
 804845c:	e8 47 fe ff ff       	call   80482a8 <_init>
 8048461:	8d 83 08 ff ff ff    	lea    eax,[ebx-0xf8]
 8048467:	29 c6                	sub    esi,eax
 8048469:	c1 fe 02             	sar    esi,0x2
 804846c:	85 f6                	test   esi,esi
 804846e:	74 25                	je     8048495 <__libc_csu_init+0x55>
 8048470:	31 ff                	xor    edi,edi
 8048472:	8d b6 00 00 00 00    	lea    esi,[esi+0x0]
 8048478:	83 ec 04             	sub    esp,0x4
 804847b:	ff 74 24 2c          	push   DWORD PTR [esp+0x2c]
 804847f:	ff 74 24 2c          	push   DWORD PTR [esp+0x2c]
 8048483:	55                   	push   ebp
 8048484:	ff 94 bb 08 ff ff ff 	call   DWORD PTR [ebx+edi*4-0xf8]
 804848b:	83 c7 01             	add    edi,0x1
 804848e:	83 c4 10             	add    esp,0x10
 8048491:	39 f7                	cmp    edi,esi
 8048493:	75 e3                	jne    8048478 <__libc_csu_init+0x38>
 8048495:	83 c4 0c             	add    esp,0xc
 8048498:	5b                   	pop    ebx
 8048499:	5e                   	pop    esi
 804849a:	5f                   	pop    edi
 804849b:	5d                   	pop    ebp
 804849c:	c3                   	ret    
 804849d:	8d 76 00             	lea    esi,[esi+0x0]

080484a0 <__libc_csu_fini>:
 80484a0:	f3 c3                	repz ret 

Disassembly of section .fini:

080484a4 <_fini>:
 80484a4:	53                   	push   ebx
 80484a5:	83 ec 08             	sub    esp,0x8
 80484a8:	e8 93 fe ff ff       	call   8048340 <__x86.get_pc_thunk.bx>
 80484ad:	81 c3 53 1b 00 00    	add    ebx,0x1b53
 80484b3:	83 c4 08             	add    esp,0x8
 80484b6:	5b                   	pop    ebx
 80484b7:	c3                   	ret  

```

## objdump -S -j .text -M intel 1
```
ksu@KSU-Ubuntu-1604-32:~$ objdump -S -j .text -M intel 1

1:     file format elf32-i386


Disassembly of section .text:

08048310 <_start>:
 8048310:	31 ed                	xor    ebp,ebp
 8048312:	5e                   	pop    esi
 8048313:	89 e1                	mov    ecx,esp
 8048315:	83 e4 f0             	and    esp,0xfffffff0
 8048318:	50                   	push   eax
 8048319:	54                   	push   esp
 804831a:	52                   	push   edx
 804831b:	68 a0 84 04 08       	push   0x80484a0
 8048320:	68 40 84 04 08       	push   0x8048440
 8048325:	51                   	push   ecx
 8048326:	56                   	push   esi
 8048327:	68 0b 84 04 08       	push   0x804840b
 804832c:	e8 bf ff ff ff       	call   80482f0 <__libc_start_main@plt>
 8048331:	f4                   	hlt    
 8048332:	66 90                	xchg   ax,ax
 8048334:	66 90                	xchg   ax,ax
 8048336:	66 90                	xchg   ax,ax
 8048338:	66 90                	xchg   ax,ax
 804833a:	66 90                	xchg   ax,ax
 804833c:	66 90                	xchg   ax,ax
 804833e:	66 90                	xchg   ax,ax

08048340 <__x86.get_pc_thunk.bx>:
 8048340:	8b 1c 24             	mov    ebx,DWORD PTR [esp]
 8048343:	c3                   	ret    
 8048344:	66 90                	xchg   ax,ax
 8048346:	66 90                	xchg   ax,ax
 8048348:	66 90                	xchg   ax,ax
 804834a:	66 90                	xchg   ax,ax
 804834c:	66 90                	xchg   ax,ax
 804834e:	66 90                	xchg   ax,ax

08048350 <deregister_tm_clones>:
 8048350:	b8 1f a0 04 08       	mov    eax,0x804a01f
 8048355:	2d 1c a0 04 08       	sub    eax,0x804a01c
 804835a:	83 f8 06             	cmp    eax,0x6
 804835d:	76 1a                	jbe    8048379 <deregister_tm_clones+0x29>
 804835f:	b8 00 00 00 00       	mov    eax,0x0
 8048364:	85 c0                	test   eax,eax
 8048366:	74 11                	je     8048379 <deregister_tm_clones+0x29>
 8048368:	55                   	push   ebp
 8048369:	89 e5                	mov    ebp,esp
 804836b:	83 ec 14             	sub    esp,0x14
 804836e:	68 1c a0 04 08       	push   0x804a01c
 8048373:	ff d0                	call   eax
 8048375:	83 c4 10             	add    esp,0x10
 8048378:	c9                   	leave  
 8048379:	f3 c3                	repz ret 
 804837b:	90                   	nop
 804837c:	8d 74 26 00          	lea    esi,[esi+eiz*1+0x0]

08048380 <register_tm_clones>:
 8048380:	b8 1c a0 04 08       	mov    eax,0x804a01c
 8048385:	2d 1c a0 04 08       	sub    eax,0x804a01c
 804838a:	c1 f8 02             	sar    eax,0x2
 804838d:	89 c2                	mov    edx,eax
 804838f:	c1 ea 1f             	shr    edx,0x1f
 8048392:	01 d0                	add    eax,edx
 8048394:	d1 f8                	sar    eax,1
 8048396:	74 1b                	je     80483b3 <register_tm_clones+0x33>
 8048398:	ba 00 00 00 00       	mov    edx,0x0
 804839d:	85 d2                	test   edx,edx
 804839f:	74 12                	je     80483b3 <register_tm_clones+0x33>
 80483a1:	55                   	push   ebp
 80483a2:	89 e5                	mov    ebp,esp
 80483a4:	83 ec 10             	sub    esp,0x10
 80483a7:	50                   	push   eax
 80483a8:	68 1c a0 04 08       	push   0x804a01c
 80483ad:	ff d2                	call   edx
 80483af:	83 c4 10             	add    esp,0x10
 80483b2:	c9                   	leave  
 80483b3:	f3 c3                	repz ret 
 80483b5:	8d 74 26 00          	lea    esi,[esi+eiz*1+0x0]
 80483b9:	8d bc 27 00 00 00 00 	lea    edi,[edi+eiz*1+0x0]

080483c0 <__do_global_dtors_aux>:
 80483c0:	80 3d 1c a0 04 08 00 	cmp    BYTE PTR ds:0x804a01c,0x0
 80483c7:	75 13                	jne    80483dc <__do_global_dtors_aux+0x1c>
 80483c9:	55                   	push   ebp
 80483ca:	89 e5                	mov    ebp,esp
 80483cc:	83 ec 08             	sub    esp,0x8
 80483cf:	e8 7c ff ff ff       	call   8048350 <deregister_tm_clones>
 80483d4:	c6 05 1c a0 04 08 01 	mov    BYTE PTR ds:0x804a01c,0x1
 80483db:	c9                   	leave  
 80483dc:	f3 c3                	repz ret 
 80483de:	66 90                	xchg   ax,ax

080483e0 <frame_dummy>:
 80483e0:	b8 10 9f 04 08       	mov    eax,0x8049f10
 80483e5:	8b 10                	mov    edx,DWORD PTR [eax]
 80483e7:	85 d2                	test   edx,edx
 80483e9:	75 05                	jne    80483f0 <frame_dummy+0x10>
 80483eb:	eb 93                	jmp    8048380 <register_tm_clones>
 80483ed:	8d 76 00             	lea    esi,[esi+0x0]
 80483f0:	ba 00 00 00 00       	mov    edx,0x0
 80483f5:	85 d2                	test   edx,edx
 80483f7:	74 f2                	je     80483eb <frame_dummy+0xb>
 80483f9:	55                   	push   ebp
 80483fa:	89 e5                	mov    ebp,esp
 80483fc:	83 ec 14             	sub    esp,0x14
 80483ff:	50                   	push   eax
 8048400:	ff d2                	call   edx
 8048402:	83 c4 10             	add    esp,0x10
 8048405:	c9                   	leave  
 8048406:	e9 75 ff ff ff       	jmp    8048380 <register_tm_clones>

0804840b <main>:
// helloCTFer.c  1.c
#include <stdio.h>

int main()
{
 804840b:	8d 4c 24 04          	lea    ecx,[esp+0x4]
 804840f:	83 e4 f0             	and    esp,0xfffffff0
 8048412:	ff 71 fc             	push   DWORD PTR [ecx-0x4]
 8048415:	55                   	push   ebp
 8048416:	89 e5                	mov    ebp,esp
 8048418:	51                   	push   ecx
 8048419:	83 ec 04             	sub    esp,0x4
   printf("Hello CTFer\n");
 804841c:	83 ec 0c             	sub    esp,0xc
 804841f:	68 c0 84 04 08       	push   0x80484c0
 8048424:	e8 b7 fe ff ff       	call   80482e0 <puts@plt>
 8048429:	83 c4 10             	add    esp,0x10
   return 0;
 804842c:	b8 00 00 00 00       	mov    eax,0x0
}
 8048431:	8b 4d fc             	mov    ecx,DWORD PTR [ebp-0x4]
 8048434:	c9                   	leave  
 8048435:	8d 61 fc             	lea    esp,[ecx-0x4]
 8048438:	c3                   	ret    
 8048439:	66 90                	xchg   ax,ax
 804843b:	66 90                	xchg   ax,ax
 804843d:	66 90                	xchg   ax,ax
 804843f:	90                   	nop

08048440 <__libc_csu_init>:
 8048440:	55                   	push   ebp
 8048441:	57                   	push   edi
 8048442:	56                   	push   esi
 8048443:	53                   	push   ebx
 8048444:	e8 f7 fe ff ff       	call   8048340 <__x86.get_pc_thunk.bx>
 8048449:	81 c3 b7 1b 00 00    	add    ebx,0x1bb7
 804844f:	83 ec 0c             	sub    esp,0xc
 8048452:	8b 6c 24 20          	mov    ebp,DWORD PTR [esp+0x20]
 8048456:	8d b3 0c ff ff ff    	lea    esi,[ebx-0xf4]
 804845c:	e8 47 fe ff ff       	call   80482a8 <_init>
 8048461:	8d 83 08 ff ff ff    	lea    eax,[ebx-0xf8]
 8048467:	29 c6                	sub    esi,eax
 8048469:	c1 fe 02             	sar    esi,0x2
 804846c:	85 f6                	test   esi,esi
 804846e:	74 25                	je     8048495 <__libc_csu_init+0x55>
 8048470:	31 ff                	xor    edi,edi
 8048472:	8d b6 00 00 00 00    	lea    esi,[esi+0x0]
 8048478:	83 ec 04             	sub    esp,0x4
 804847b:	ff 74 24 2c          	push   DWORD PTR [esp+0x2c]
 804847f:	ff 74 24 2c          	push   DWORD PTR [esp+0x2c]
 8048483:	55                   	push   ebp
 8048484:	ff 94 bb 08 ff ff ff 	call   DWORD PTR [ebx+edi*4-0xf8]
 804848b:	83 c7 01             	add    edi,0x1
 804848e:	83 c4 10             	add    esp,0x10
 8048491:	39 f7                	cmp    edi,esi
 8048493:	75 e3                	jne    8048478 <__libc_csu_init+0x38>
 8048495:	83 c4 0c             	add    esp,0xc
 8048498:	5b                   	pop    ebx
 8048499:	5e                   	pop    esi
 804849a:	5f                   	pop    edi
 804849b:	5d                   	pop    ebp
 804849c:	c3                   	ret    
 804849d:	8d 76 00             	lea    esi,[esi+0x0]

080484a0 <__libc_csu_fini>:
 80484a0:	f3 c3                	repz ret 

```

## objdump -S -j .text -M intel 1 --no-show-raw-insn  
```
ksu@KSU-Ubuntu-1604-32:~$ objdump -S -j .text -M intel 1 --no-show-raw-insn  

1:     file format elf32-i386


Disassembly of section .text:

08048310 <_start>:
 8048310:	xor    ebp,ebp
 8048312:	pop    esi
 8048313:	mov    ecx,esp
 8048315:	and    esp,0xfffffff0
 8048318:	push   eax
 8048319:	push   esp
 804831a:	push   edx
 804831b:	push   0x80484a0
 8048320:	push   0x8048440
 8048325:	push   ecx
 8048326:	push   esi
 8048327:	push   0x804840b
 804832c:	call   80482f0 <__libc_start_main@plt>
 8048331:	hlt    
 8048332:	xchg   ax,ax
 8048334:	xchg   ax,ax
 8048336:	xchg   ax,ax
 8048338:	xchg   ax,ax
 804833a:	xchg   ax,ax
 804833c:	xchg   ax,ax
 804833e:	xchg   ax,ax

08048340 <__x86.get_pc_thunk.bx>:
 8048340:	mov    ebx,DWORD PTR [esp]
 8048343:	ret    
 8048344:	xchg   ax,ax
 8048346:	xchg   ax,ax
 8048348:	xchg   ax,ax
 804834a:	xchg   ax,ax
 804834c:	xchg   ax,ax
 804834e:	xchg   ax,ax

08048350 <deregister_tm_clones>:
 8048350:	mov    eax,0x804a01f
 8048355:	sub    eax,0x804a01c
 804835a:	cmp    eax,0x6
 804835d:	jbe    8048379 <deregister_tm_clones+0x29>
 804835f:	mov    eax,0x0
 8048364:	test   eax,eax
 8048366:	je     8048379 <deregister_tm_clones+0x29>
 8048368:	push   ebp
 8048369:	mov    ebp,esp
 804836b:	sub    esp,0x14
 804836e:	push   0x804a01c
 8048373:	call   eax
 8048375:	add    esp,0x10
 8048378:	leave  
 8048379:	repz ret 
 804837b:	nop
 804837c:	lea    esi,[esi+eiz*1+0x0]

08048380 <register_tm_clones>:
 8048380:	mov    eax,0x804a01c
 8048385:	sub    eax,0x804a01c
 804838a:	sar    eax,0x2
 804838d:	mov    edx,eax
 804838f:	shr    edx,0x1f
 8048392:	add    eax,edx
 8048394:	sar    eax,1
 8048396:	je     80483b3 <register_tm_clones+0x33>
 8048398:	mov    edx,0x0
 804839d:	test   edx,edx
 804839f:	je     80483b3 <register_tm_clones+0x33>
 80483a1:	push   ebp
 80483a2:	mov    ebp,esp
 80483a4:	sub    esp,0x10
 80483a7:	push   eax
 80483a8:	push   0x804a01c
 80483ad:	call   edx
 80483af:	add    esp,0x10
 80483b2:	leave  
 80483b3:	repz ret 
 80483b5:	lea    esi,[esi+eiz*1+0x0]
 80483b9:	lea    edi,[edi+eiz*1+0x0]

080483c0 <__do_global_dtors_aux>:
 80483c0:	cmp    BYTE PTR ds:0x804a01c,0x0
 80483c7:	jne    80483dc <__do_global_dtors_aux+0x1c>
 80483c9:	push   ebp
 80483ca:	mov    ebp,esp
 80483cc:	sub    esp,0x8
 80483cf:	call   8048350 <deregister_tm_clones>
 80483d4:	mov    BYTE PTR ds:0x804a01c,0x1
 80483db:	leave  
 80483dc:	repz ret 
 80483de:	xchg   ax,ax

080483e0 <frame_dummy>:
 80483e0:	mov    eax,0x8049f10
 80483e5:	mov    edx,DWORD PTR [eax]
 80483e7:	test   edx,edx
 80483e9:	jne    80483f0 <frame_dummy+0x10>
 80483eb:	jmp    8048380 <register_tm_clones>
 80483ed:	lea    esi,[esi+0x0]
 80483f0:	mov    edx,0x0
 80483f5:	test   edx,edx
 80483f7:	je     80483eb <frame_dummy+0xb>
 80483f9:	push   ebp
 80483fa:	mov    ebp,esp
 80483fc:	sub    esp,0x14
 80483ff:	push   eax
 8048400:	call   edx
 8048402:	add    esp,0x10
 8048405:	leave  
 8048406:	jmp    8048380 <register_tm_clones>

0804840b <main>:
// helloCTFer.c  1.c
#include <stdio.h>

int main()
{
 804840b:	lea    ecx,[esp+0x4]
 804840f:	and    esp,0xfffffff0
 8048412:	push   DWORD PTR [ecx-0x4]
 8048415:	push   ebp
 8048416:	mov    ebp,esp
 8048418:	push   ecx
 8048419:	sub    esp,0x4
   printf("Hello CTFer\n");
 804841c:	sub    esp,0xc
 804841f:	push   0x80484c0
 8048424:	call   80482e0 <puts@plt>
 8048429:	add    esp,0x10
   return 0;
 804842c:	mov    eax,0x0
}
 8048431:	mov    ecx,DWORD PTR [ebp-0x4]
 8048434:	leave  
 8048435:	lea    esp,[ecx-0x4]
 8048438:	ret    
 8048439:	xchg   ax,ax
 804843b:	xchg   ax,ax
 804843d:	xchg   ax,ax
 804843f:	nop

08048440 <__libc_csu_init>:
 8048440:	push   ebp
 8048441:	push   edi
 8048442:	push   esi
 8048443:	push   ebx
 8048444:	call   8048340 <__x86.get_pc_thunk.bx>
 8048449:	add    ebx,0x1bb7
 804844f:	sub    esp,0xc
 8048452:	mov    ebp,DWORD PTR [esp+0x20]
 8048456:	lea    esi,[ebx-0xf4]
 804845c:	call   80482a8 <_init>
 8048461:	lea    eax,[ebx-0xf8]
 8048467:	sub    esi,eax
 8048469:	sar    esi,0x2
 804846c:	test   esi,esi
 804846e:	je     8048495 <__libc_csu_init+0x55>
 8048470:	xor    edi,edi
 8048472:	lea    esi,[esi+0x0]
 8048478:	sub    esp,0x4
 804847b:	push   DWORD PTR [esp+0x2c]
 804847f:	push   DWORD PTR [esp+0x2c]
 8048483:	push   ebp
 8048484:	call   DWORD PTR [ebx+edi*4-0xf8]
 804848b:	add    edi,0x1
 804848e:	add    esp,0x10
 8048491:	cmp    edi,esi
 8048493:	jne    8048478 <__libc_csu_init+0x38>
 8048495:	add    esp,0xc
 8048498:	pop    ebx
 8048499:	pop    esi
 804849a:	pop    edi
 804849b:	pop    ebp
 804849c:	ret    
 804849d:	lea    esi,[esi+0x0]

080484a0 <__libc_csu_fini>:
 80484a0:	repz ret 

```
