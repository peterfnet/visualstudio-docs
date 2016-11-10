---
title: "Error Handling and Return Values"
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
  - "errors [Visual Studio SDK], handling"
  - "error handling"
  - "return values"
ms.assetid: b2d9079d-39a6-438a-8010-290056694b5c
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
# Error Handling and Return Values
VSPackages and COM use the same architecture for errors. The `SetErrorInfo` and `GetErrorInfo` functions are part of the Win32 application programming interface (API). Any VSPackage in the integrated development environment (IDE) can call these global Win32 APIs to record rich error information when receiving an error notification. The [!INCLUDE[vsipsdk](../extensibility/includes/vsipsdk_md.md)] provides interop assemblies to manage error information.  
  
## Interop Methods  
 As a convenience, the IDE provides a method, \<xref:Microsoft.VisualStudio.Shell.Interop.IVsUIShell.SetErrorInfo*>, to use instead of calling the Win32 APIs. In managed code use \<xref:Microsoft.VisualStudio.Shell.Interop.IVsUIShell.SetErrorInfo*>. When an error `HRESULT` arrives at the level where the error message should be displayed (this is often the object implementing an \<xref:Microsoft.VisualStudio.OLE.Interop.IOleCommandTarget> command handler), the IDE uses another method, \<xref:Microsoft.VisualStudio.Shell.Interop.IVsUIShell.ReportErrorInfo*>, to display the appropriate message box. In managed code use the \<xref:Microsoft.VisualStudio.Shell.Interop.IVsUIShell.ReportErrorInfo*> method.  
  
 As a VSPackage implementer, your COM objects normally implement `ISupportErrorInfo`. The `ISupportErrorInfo` interface ensures that rich error information can move vertically up the call chain. Objects that might be used across processes or across threads must support `ISupportErrorInfo` to ensure that the rich error information is properly marshaled back to the caller.  
  
 All objects that are related to VSPackages and that are involved in extending the IDE, including editor factories, editors, hierarchies, and offered services, should support rich error information. While the IDE does not require these VSPackage objects to implement `ISupportErrorInfo`, it is always encouraged.  
  
 The IDE is responsible for reporting error information and displaying it to a user of [!INCLUDE[vsprvs](../codequality/includes/vsprvs_md.md)] whenever an `HRESULT` is propagated to the IDE. The IDE is also the mechanism for creating `ErrorInfo` objects.  
  
## General Guidelines  
 You can use the \<xref:Microsoft.VisualStudio.Shell.Interop.IVsUIShell.SetErrorInfo*> and \<xref:Microsoft.VisualStudio.Shell.Interop.IVsUIShell.ReportErrorInfo*> methods to set and report errors that are internal to your VSPackage implementation as well. However, as a general rule, follow these guidelines for handling error messages in your VSPackage:  
  
-   Implement `ISupportErrorInfo` in your VSPackage COM objects.  
  
-   Create an error reporting mechanism that calls the \<xref:Microsoft.VisualStudio.Shell.Interop.IVsUIShell.SetErrorInfo*> method in objects that implement \<xref:Microsoft.VisualStudio.OLE.Interop.IOleCommandTarget>.  
  
-   Let the IDE display errors to users through the \<xref:Microsoft.VisualStudio.Shell.Interop.IVsUIShell.ReportErrorInfo*> method.  
  
## Error Information in the IDE  
 The following rules indicate how to handle error information in the [!INCLUDE[vsprvs](../codequality/includes/vsprvs_md.md)] IDE:  
  
-   As a defensive strategy to guarantee that stale error info is not reported to users, functions that call the \<xref:Microsoft.VisualStudio.Shell.Interop.IVsUIShell.ReportErrorInfo*> method should first call the \<xref:Microsoft.VisualStudio.Shell.Interop.IVsUIShell.SetErrorInfo*> method. Pass in `null` to clear cached error messages before calling anything that might set new error information.  
  
-   Functions that do not directly report error messages are only allowed to call the \<xref:Microsoft.VisualStudio.Shell.Interop.IVsUIShell.SetErrorInfo*> method if they are returning an error `HRESULT`. It is permissible to clear the `ErrorInfo` on the entry to a function or when returning \<xref:Microsoft.VisualStudio.VSConstants.S_OK>. The only exception to this rule is when a call returns an error `HRESULT` from which the receiving party can explicitly recover or safely ignore.  
  
-   Any party that explicitly ignores an error `HRESULT` must call the \<xref:Microsoft.VisualStudio.Shell.Interop.IVsUIShell.SetErrorInfo*> method with \<xref:Microsoft.VisualStudio.VSConstants.S_OK>. Otherwise, the `ErrorInfo` object might be accidentally used when another party generates an error without providing their own `ErrorInfo`.  
  
-   All methods that originate an error `HRESULT` are encouraged to call the \<xref:Microsoft.VisualStudio.Shell.Interop.IVsUIShell.SetErrorInfo*> method to provide rich error information. If the returned `HRESULT` is a special `FACILITY_ITF` error, then the method is required to provide a proper `ErrorInfo`object. If the returned error is a standard system error (for example, \<xref:Microsoft.VisualStudio.VSConstants.E_OUTOFMEMORY>, \<xref:Microsoft.VisualStudio.VSConstants.E_ABORT>, \<xref:Microsoft.VisualStudio.VSConstants.E_INVALIDARG>, \<xref:Microsoft.VisualStudio.VSConstants.E_UNEXPECTED>, and so on.) it is acceptable to return the error code without explicitly calling the \<xref:Microsoft.VisualStudio.Shell.Interop.IVsUIShell.SetErrorInfo*> method. As a defensive coding strategy, when originating an error `HRESULT` (including system errors), always call the \<xref:Microsoft.VisualStudio.Shell.Interop.IVsUIShell.SetErrorInfo*> method, either with `ErrorInfo` describing the failure in greater detail, or `null`.  
  
-   All functions that return an error originated by another call must pass on the information that was received from the failing call in the `HRESULT` without modifying the `ErrorInfo` object.  
  
## See Also  
 \<xref:Microsoft.VisualStudio.OLE.Interop.IOleCommandTarget>   
 [SetErrorInfo (Component Automation)](assetId:///8eaacfac-fc37-4eaa-870b-10b99d598d66)   
 [GetErrorInfo](assetId:///03317526-8c4f-4173-bc10-110c8112676a)   
 [ISupportErrorInfo Interface](assetId:///42d33066-36b4-4a5b-aa5d-46682e560f32)