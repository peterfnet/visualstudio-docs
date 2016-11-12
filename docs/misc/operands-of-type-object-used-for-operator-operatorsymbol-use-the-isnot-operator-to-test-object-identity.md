---
title: "Operands of type Object used for operator &#39;&lt;operatorsymbol&gt;&#39;; use the &#39;IsNot&#39; operator to test object identity | Microsoft Docs"
ms.custom: ""
ms.date: "2015-07-20"
ms.prod: "visual-studio-dev14"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "devlang-visual-basic"
ms.tgt_pltfrm: ""
ms.topic: "article"
f1_keywords: 
  - "vbc42032"
  - "bc42032"
helpviewer_keywords: 
  - "BC42032"
ms.assetid: f43b163b-f8f6-489d-ba9e-df6398ccc553
caps.latest.revision: 8
author: "stevehoag"
ms.author: "shoag"
manager: "wpickett"
translation.priority.ht: 
  - "de-de"
  - "es-es"
  - "fr-fr"
  - "it-it"
  - "ja-jp"
  - "ko-kr"
  - "ru-ru"
  - "zh-cn"
  - "zh-tw"
translation.priority.mt: 
  - "cs-cz"
  - "pl-pl"
  - "pt-br"
  - "tr-tr"
---
# Operands of type Object used for operator &#39;&lt;operatorsymbol&gt;&#39;; use the &#39;IsNot&#39; operator to test object identity
An expression uses the `<>` operator with one or both operands of the [Object Data Type](/dotnet/visual-basic/language-reference/data-types/object-data-type).  
  
 You should use the `Is` or `IsNot` operator to determine whether two object references refer to the same object instance. See "Comparing Objects" in [Comparison Operators in Visual Basic](/dotnet/visual-basic/programming-guide/language-features/operators-and-expressions/comparison-operators).  
  
 When a variable or expression evaluates to `Object`, the compiler must perform *late binding*, which causes extra operations at run time. It also exposes your application to potential run-time errors. For example, if you assign a <xref:System.Windows.Forms.Form> to an `Object` variable and then try to use it with the `<>` operator, the runtime throws an <xref:System.InvalidCastException> because Visual Basic cannot convert a <xref:System.Windows.Forms.Form> object to a data type suitable for value comparison. Even if both operands evaluate to type <xref:System.Windows.Forms.Form>, the operation fails because `<>` is not defined for <xref:System.Windows.Forms.Form> operands.  
  
 By default, this message is a warning. For information on hiding warnings or treating warnings as errors, see [Configuring Warnings in Visual Basic](../ide/configuring-warnings-in-visual-basic.md).  
  
 **Error ID:** BC42032  
  
### To correct this error  
  
-   If you want to determine whether two object references refer to the same object instance, use the `Is` or `IsNot` operator.  
  
## See Also  
 [Comparison Operators in Visual Basic](/dotnet/visual-basic/programming-guide/language-features/operators-and-expressions/comparison-operators)   
 [IsNot Operator](/dotnet/visual-basic/language-reference/operators/isnot-operator)   
 [How to: Determine Whether Two Objects Are Related](../Topic/How%20to:%20Determine%20Whether%20Two%20Objects%20Are%20Related%20\(Visual%20Basic\).md)   
 [How to: Determine Whether Two Objects Are Identical](../Topic/How%20to:%20Determine%20Whether%20Two%20Objects%20Are%20Identical%20\(Visual%20Basic\).md)