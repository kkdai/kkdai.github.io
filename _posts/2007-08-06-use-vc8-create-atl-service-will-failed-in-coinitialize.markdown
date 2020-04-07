---
layout: post
layout: post
author: kkdai
comments: true
date: 2007-08-06 13:54:07+00:00
slug: use-vc8-create-atl-service-will-failed-in-coinitialize
title: Use VC8 create ATL Service will failed in CoInitialize
wordpress_id: 726
categories:
- NET程式設計
- VC++程式設計
---




![](http://msdn.microsoft.com/vstudio/images/VS2005_logo_product_home.gif)




Use VC8 (Visual Studio 2005) to create project is more easy than VC6(VC98), but however we may not CoInitialize ATL service as well. It always happen failed since you use your API to CoInitialize this service. But it iswork if you use VC6 to create the ATL service. The root cause is the sercurity issue as follow:




[http://msdn2.microsoft.com/en-us/library/xe2dxx5c(VS.80).aspx](http://msdn2.microsoft.com/en-us/library/xe2dxx5c(VS.80).aspx)  
  




    
    
    <span style="color:rgb(0,0,255);">class</span> <span style="color:rgb(0,0,0);">CATL_SERVERModule</span> : <span style="color:rgb(0,0,255);">public</span> <span style="color:rgb(0,0,0);">CAtlServiceModuleT</span>< <span style="color:rgb(0,0,0);">CATL_SERVERModule</span>, <span style="color:rgb(0,0,0);">IDS_SERVICENAME</span> >
    {
    <span style="color:rgb(0,0,255);">public</span> :
        <span style="color:rgb(0,0,0);">DECLARE_LIBID</span>(<span style="color:rgb(0,0,0);">LIBID_ATL_SERVERLib</span>)
        <span style="color:rgb(0,0,0);">DECLARE_REGISTRY_APPID_RESOURCEID</span>(<span style="color:rgb(0,0,0);">IDR_ATL_SERVER</span>, <span style="color:rgb(128,0,0);">"{C08654FF-A0F1-4CFD-8A09-93D4768BCF14}"</span>)
        <span style="color:rgb(0,0,0);">HRESULT</span> <span style="color:rgb(0,0,0);">InitializeSecurity</span>() <span style="color:rgb(0,0,255);">throw</span>()
        {
            <span style="color:rgb(0,128,0);">// TODO : Call CoInitializeSecurity and provide the appropriate security settings for
    </span>        <span style="color:rgb(0,128,0);">// your service
    </span>        <span style="color:rgb(0,128,0);">// Suggested - PKT Level Authentication,
    </span>        <span style="color:rgb(0,128,0);">// Impersonation Level of RPC_C_IMP_LEVEL_IDENTIFY
    </span>        <span style="color:rgb(0,128,0);">// and an appropiate Non NULL Security Descriptor.
    </span>        <span style="color:rgb(0,0,255);">return</span> <span style="color:rgb(0,0,0);">S_OK</span>;
        }
    };
    




The problem is VC8 move the the security implement the base-class and you at-least implement one security check in this section of code. The easy way to implement it is copy code from VC6 as follow:



    
    
            <span style="color:rgb(0,128,0);">// TODO : Call CoInitializeSecurity and provide the appropriate security settings for
    </span>        <span style="color:rgb(0,128,0);">// your service
    </span>        <span style="color:rgb(0,128,0);">// Suggested - PKT Level Authentication,
    </span>        <span style="color:rgb(0,128,0);">// Impersonation Level of RPC_C_IMP_LEVEL_IDENTIFY
    </span>        <span style="color:rgb(0,128,0);">// and an appropiate Non NULL Security Descriptor.
    
    </span>            <span style="color:rgb(0,128,0);">// This provides a NULL DACL which will allow access to everyone.
    </span>        <span style="color:rgb(0,0,0);">CSecurityDescriptor</span> <span style="color:rgb(0,0,0);">sd</span>;
            <span style="color:rgb(0,0,0);">sd</span>.<span style="color:rgb(0,0,0);">InitializeFromThreadToken</span>();
            <span style="color:rgb(0,0,0);">hr</span> = <span style="color:rgb(0,0,0);">CoInitializeSecurity</span>(<span style="color:rgb(0,0,0);">sd</span>, -1, <span style="color:rgb(0,0,0);">NULL</span>, <span style="color:rgb(0,0,0);">NULL</span>,
                 <span style="color:rgb(0,0,0);">RPC_C_AUTHN_LEVEL_PKT</span>, <span style="color:rgb(0,0,0);">RPC_C_IMP_LEVEL_IMPERSONATE</span>, <span style="color:rgb(0,0,0);">NULL</span>, <span style="color:rgb(0,0,0);">EOAC_NONE</span>, <span style="color:rgb(0,0,0);">NULL</span>);
    
            <span style="color:rgb(0,0,255);">return</span> <span style="color:rgb(0,0,0);">S_OK</span>;
    


  


Powered by [Zoundry](http://www.zoundry.com)
