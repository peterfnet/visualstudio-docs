---
title: "Commenting Code in a Legacy Language Service"
ms.custom: na
ms.date: "10/14/2016"
ms.prod: "visual-studio-dev14"
ms.reviewer: na
ms.suite: na
ms.technology: 
  - "vs-ide-sdk"
ms.tgt_pltfrm: na
ms.topic: "article"
helpviewer_keywords: 
  - "comments, supporting in language services [managed package framework]"
  - "language services [managed package framework], commenting code"
ms.assetid: 9600d6f0-e2b6-4fe0-b935-fb32affb97a4
caps.latest.revision: 14
ms.author: "gregvanl"
manager: "ghogen"
translation.priority.mt: 
  - "cs-cz"
  - "de-de"
  - "es-es"
  - "fr-fr"
  - "it-it"
  - "ja-jp"
  - "ko-kr"
  - "pl-pl"
  - "pt-br"
  - "ru-ru"
  - "tr-tr"
  - "zh-cn"
  - "zh-tw"
---
# Commenting Code in a Legacy Language Service
Programming languages typically provide a means to annotate or comment the code. A comment is a section of text that provides additional information about the code but is ignored during compilation or interpretation.  
  
 The managed package framework (MPF) classes provide support for commenting and uncommenting selected text.  
  
## Comment Styles  
 There are two general styles of comment:  
  
1.  Line comments, where the comment is on a single line.  
  
2.  Block comments, where the comment may include multiple lines.  
  
 Line comments typically have a starting character (or characters), while block comments have both start and end characters. For example, in C#, a line comment starts with //, and a block comment starts with /* and ends with \*/.  
  
 When the user selects the command **Comment Selection** from the **Edit** -> **Advanced** menu, the command is routed to the \<xref:Microsoft.VisualStudio.Package.Source.CommentSpan*> method on the \<xref:Microsoft.VisualStudio.Package.Source> class. When the user selects the command **Uncomment Selection**, the command is routed to the \<xref:Microsoft.VisualStudio.Package.Source.UncommentSpan*> method.  
  
## Supporting Code Comments  
 You can have your language service support code comments by means of the `EnableCommenting` named parameter of the \<xref:Microsoft.VisualStudio.Shell.ProvideLanguageServiceAttribute> . This sets the \<xref:Microsoft.VisualStudio.Package.LanguagePreferences.EnableCommenting*> property of the \<xref:Microsoft.VisualStudio.Package.LanguagePreferences> class. For more information about setting language servicce features, see [Registering a Legacy Language Service](../extensibility/registering-a-legacy-language-service1.md)).  
  
 You must also override the \<xref:Microsoft.VisualStudio.Package.Source.GetCommentFormat*> method to return a \<xref:Microsoft.VisualStudio.Package.CommentInfo> structure with the comment characters for your language. C#-style line comment characters are the default.  
  
### Example  
 Here is an example implementation of the \<xref:Microsoft.VisualStudio.Package.Source.GetCommentFormat*> method.  
  
```c#  
using Microsoft.VisualStudio.Package;  
  
namespace MyLanguagePackage  
{  
    class MySource : Source  
    {  
        public override CommentInfo GetCommentFormat() {  
            CommentInfo info = new CommentInfo();  
            info.LineStart       = "//";  
            info.BlockStart      = "/*";  
            info.BlockEnd        = "*/";  
            info.UseLineComments = true;  
            return info;  
        }  
    }  
}  
```  
  
## See Also  
 [Legacy Language Service Features](../extensibility/legacy-language-service-features1.md)   
 [Registering a Legacy Language Service](../extensibility/registering-a-legacy-language-service1.md)