---
title: "Оператор If (Visual Basic) | Microsoft Docs"
ms.date: "2015-07-20"
ms.prod: ".net"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "devlang-visual-basic"
ms.topic: "article"
f1_keywords: 
  - "vb.IfOperator"
  - "IfOperator"
dev_langs: 
  - "VB"
helpviewer_keywords: 
  - "условное выполнение"
  - "условный оператор [Visual Basic]"
  - "if - выражения [Visual Basic]"
  - "If - оператор [Visual Basic]"
  - "тернарные операторы"
ms.assetid: dd56c9df-7cd4-442c-9ba6-20c70ee44c8f
caps.latest.revision: 19
author: "stevehoag"
ms.author: "shoag"
caps.handback.revision: 19
---
# Оператор If (Visual Basic)
[!INCLUDE[vs2017banner](../../../visual-basic/includes/vs2017banner.md)]

Использует сокращенные вычисления, в зависимости от результата которых возвращает одно из двух значений.  Оператор `If` может быть вызван с тремя или с двумя аргументами.  
  
## Синтаксис  
  
```  
If( [argument1,] argument2, argument3 )  
```  
  
## Оператор If с тремя аргументами  
 При вызове `If` с тремя аргументами первый аргумент должен иметь значение, которое может быть приведено к `Boolean`.  Значение `Boolean` определит, какой из двух других аргументов будет вычислен и возвращен.  Следующий список относится только к вызову оператора `If` с тремя аргументами.  
  
## Части  
  
|||  
|-|-|  
|Термин|Определение|  
|`argument1`|Обязательный.  `Boolean`.  . Определяет, какой из других аргументов будет вычислен и возвращен.|  
|`argument2`|Обязательный.  `Object`.  Вычисляется и возвращается, если `argument1` принимает значение `True`.|  
|`argument3`|Обязательный.  `Object`.  Вычисляется и возвращается, если `argument1` результатом которого является `False` или если `argument1` переменной, [Nullable](../../../visual-basic/programming-guide/language-features/data-types/nullable-value-types.md)`Boolean`, результатом которого является значение [Nothing](../../../visual-basic/language-reference/nothing.md).|  
  
 Оператор `If` с тремя аргументами работает подобно функции `IIf` за исключением того, что он использует сокращенные вычисления.  Функция `IIf` всегда вычисляет все три своих аргумента, а оператор `If` с тремя аргументами вычисляет только два из них.  Вычисляется первый аргумент `If` и результат имеет тип `Boolean`, а значение `True` или `False`.  Если значение равно `True`, то вычисляется и возвращается значение `argument2`, а `argument3` не вычисляется.  Если значение `Boolean` равно `False`, то вычисляется и возвращается значение `argument3`, а `argument2` не вычисляется.  Следующие примеры иллюстрируют использование `If` с тремя аргументами:  
  
 [!code-vb[VbVbalrOperators#100](../../../visual-basic/language-reference/operators/codesnippet/visualbasic/if-operator_1.vb)]  
  
 В следующем примере показано значение сокращенного вычисления.  В примере показано две попытки деления переменной `number` на переменную `divisor`, за исключением случая, когда `divisor` равно нулю.  В этом случае должен быть возвращен 0 и не должно производиться деление, поскольку это вызвало бы ошибку во время выполнения.  Так как выражение `If` использует сокращенные вычисления, оно вычисляет или второй, или третий аргумент, в зависимости от значения первого аргумента.  Если первый аргумент имеет значение true, то делитель не ноль и вычисление второго аргумента и деление является безопасными.  Если первый аргумент имеет значение false, то вычисляется только третий аргумент и возвращается 0.  Таким образом, если делитель равен 0, то не производится попытка выполнить деление и ошибка не появляется.  Однако поскольку `IIf` не использует сокращенные вычисления, второй аргумент вычисляется, даже если первый аргумент равен false.  Это вызывает ошибку во время выполнения деления на ноль.  
  
 [!code-vb[VbVbalrOperators#101](../../../visual-basic/language-reference/operators/codesnippet/visualbasic/if-operator_2.vb)]  
  
## Оператор If с двумя аргументами  
 Первый аргумент `If` можно опустить.  Это позволяет вызвать оператор с использованием только двух аргументов.  Следующий список относится только к вызову оператора `If` с двумя аргументами.  
  
## Части  
  
|||  
|-|-|  
|Термин|Определение|  
|`argument2`|Обязательный.  `Object`.  Должен быть ссылкой или типом nullable.  Вычисляется и возвращается, если вычисленное значение отлично от `Nothing`.|  
|`argument3`|Обязательный.  `Object`.  Вычисляется и возвращается, если `argument2` принимает значение `Nothing`.|  
  
 Если аргумент `Boolean` опущен, то первый аргумент должен быть ссылкой или типом nullable.  Если первый аргумент имеет значение `Nothing`, то возвращается значение второго аргумента.  Во всех остальных случаях возвращается значение первого аргумента.  В следующем примере показано, как работает это вычисление.  
  
 [!code-vb[VbVbalrOperators#102](../../../visual-basic/language-reference/operators/codesnippet/visualbasic/if-operator_3.vb)]  
  
## См. также  
 <xref:Microsoft.VisualBasic.Interaction.IIf%2A>   
 [Типы значения, допускающие Null](../../../visual-basic/programming-guide/language-features/data-types/nullable-value-types.md)   
 [Nothing](../../../visual-basic/language-reference/nothing.md)