---
layout: post
layout: post
author: kkdai
comments: true
date: 2008-03-06 14:10:58+00:00
slug: simple-console-tool-to-show-all-file-version-of-current-folder-%e9%a1%af%e7%a4%ba%e6%aa%94%e6%a1%88%e7%89%88%e6%9c%ac%e7%9a%84%e6%b8%ac%e8%a9%a6%e7%a8%8b%e5%bc%8f
title: Simple console tool to show all file version of current folder 顯示檔案版本的測試程式
wordpress_id: 824
categories:
- 我的生活
---

[![properties.gif](http://farm4.static.flickr.com/3203/2314184072_9d96c2d6f3.jpg)](http://www.flickr.com/photos/27643002@N00/2314184072/)

 

[![result.JPG](http://farm4.static.flickr.com/3027/2313375799_13443e0d40.jpg)](http://www.flickr.com/photos/27643002@N00/2313375799/)

 

This is a simple console problem to display all file version and time for current folder. The full code as follow:


<!-- more -->


    <span class="rem">// PrintF.cpp : Defines the entry point for the console application.</span>
    <span class="rem">//</span>
    
    #include <span class="str">"stdafx.h"</span>
    #include <stdio.h>
    #include <afx.h>
    #include <shlwapi.h>
    
    <span class="kwrd">enum</span> PrintParam
    {
        Print_Version = 1,
        Print_Time = 2,
    };
    
    WORD GetFileVersion(VS_FIXEDFILEINFO FileInfo, <span class="kwrd">int</span> nIndex);
    <span class="kwrd">void</span> EmitErrorMsg (HRESULT hr);
    <span class="kwrd">void</span> GetFileTimeString(<span class="kwrd">char</span> *fname, CString &outStr);
    <span class="kwrd">void</span> GetFileVersionString(<span class="kwrd">char</span> *fname, CString &outStr);
    HRESULT GetFileVersion (<span class="kwrd">char</span> *filename, VS_FIXEDFILEINFO *vsf);
    HRESULT GetFileDate (<span class="kwrd">char</span> *filename, FILETIME *pft);
    HRESULT LastError();
    BOOL StartPrint(PrintParam nPrintType = Print_Version);


​    
    <span class="kwrd">int</span> main(<span class="kwrd">int</span> argc, <span class="kwrd">char</span>* argv[])
    {
    <span class="rem">//</span>
    <span class="rem">//    v Print file version</span>
    <span class="rem">//    t Print file create time</span>
    <span class="rem">//  null print file version as default.</span>
    <span class="rem">//</span>
        <span class="kwrd">if</span> (argc != 2)
        {
            StartPrint(Print_Version);
        }
        <span class="kwrd">else</span>
        {
            <span class="kwrd">char</span> chAction = argv[1][0];
            <span class="kwrd">switch</span>(chAction)
            {
            <span class="kwrd">case</span> <span class="str">'v'</span>:
            <span class="kwrd">case</span> <span class="str">'V'</span>:
                StartPrint(Print_Version);
                <span class="kwrd">break</span>;
            <span class="kwrd">case</span> <span class="str">'t'</span>:
            <span class="kwrd">case</span> <span class="str">'T'</span>:
                StartPrint(Print_Time);
                <span class="kwrd">break</span>;
    
            <span class="kwrd">default</span>:
                <span class="kwrd">break</span>;
            }
        }
        <span class="kwrd">return</span> 0;
    }
    
    <span class="kwrd">void</span> GetFileTimeString(<span class="kwrd">char</span> *fname, CString &outStr)
    {
        FILETIME t;
        GetFileDate(fname, &t);
    
        FILETIME lft;
        FILETIME *ft = &lft;
        FileTimeToLocalFileTime(&t,ft);
        <span class="rem">//outStr.Format("%08x %08x",ft->dwHighDateTime,ft->dwLowDateTime); </span>
        {
            SYSTEMTIME stCreate;
            BOOL bret = FileTimeToSystemTime(ft,&stCreate);
            outStr.Format(<span class="str">"%02d/%02d/%d  %02d:%02d:%02d"</span>,
                stCreate.wMonth, stCreate.wDay, stCreate.wYear,
                stCreate.wHour, stCreate.wMinute, stCreate.wSecond);
        }
    }
    
    <span class="kwrd">void</span> GetFileVersionString(<span class="kwrd">char</span> *fname, CString &outStr)
    {
        VS_FIXEDFILEINFO FileInfo;
    
        <span class="kwrd">if</span> (SUCCEEDED(GetFileVersion(fname, &FileInfo)))
        {
            outStr.Format(<span class="str">"%d.%d.%d.%d"</span>, GetFileVersion(FileInfo, 3), GetFileVersion(FileInfo, 2), GetFileVersion(FileInfo, 1), GetFileVersion(FileInfo, 0));
        }
        <span class="kwrd">else</span>
            outStr = <span class="str">"No info"</span>;
    }
    
    WORD GetFileVersion(VS_FIXEDFILEINFO FileInfo, <span class="kwrd">int</span> nIndex)
    {
        <span class="kwrd">if</span> (nIndex == 0)
            <span class="kwrd">return</span> (WORD)(FileInfo.dwFileVersionLS & 0x0000FFFF);
        <span class="kwrd">else</span> <span class="kwrd">if</span> (nIndex == 1)
            <span class="kwrd">return</span> (WORD)((FileInfo.dwFileVersionLS & 0xFFFF0000) >> 16);
        <span class="kwrd">else</span> <span class="kwrd">if</span> (nIndex == 2)
            <span class="kwrd">return</span> (WORD)(FileInfo.dwFileVersionMS & 0x0000FFFF);
        <span class="kwrd">else</span> <span class="kwrd">if</span> (nIndex == 3)
            <span class="kwrd">return</span> (WORD)((FileInfo.dwFileVersionMS & 0xFFFF0000) >> 16);
        <span class="kwrd">else</span>
            <span class="kwrd">return</span> 0;
    }
    
    HRESULT GetFileDate (<span class="kwrd">char</span> *filename, FILETIME *pft) {
        <span class="rem">// we are interested only in the create time</span>
        <span class="rem">// this is the equiv of "modified time" in the </span>
        <span class="rem">// Windows Explorer properties dialog</span>
        FILETIME ct,lat;
        HANDLE hFile = CreateFile(filename,GENERIC_READ,FILE_SHARE_READ | FILE_SHARE_WRITE,0,OPEN_EXISTING,0,0);
        <span class="kwrd">if</span> (hFile == INVALID_HANDLE_VALUE)
            <span class="kwrd">return</span> E_FAIL;<span class="rem">//LastError();</span>
        BOOL bret = GetFileTime(hFile,&ct,&lat,pft);
        <span class="kwrd">if</span> (bret == 0)
            <span class="kwrd">return</span> E_FAIL;<span class="rem">//LastError();</span>
        <span class="kwrd">return</span> S_OK;
    }
    
    <span class="rem">// This function gets the file version info structure</span>
    HRESULT GetFileVersion (<span class="kwrd">char</span> *filename, VS_FIXEDFILEINFO *pvsf) {
        DWORD dwHandle;
        DWORD cchver = GetFileVersionInfoSize(filename,&dwHandle);
        <span class="kwrd">if</span> (cchver == 0)
            <span class="kwrd">return</span> E_FAIL;<span class="rem">//LastError();</span>
        <span class="kwrd">char</span>* pver = <span class="kwrd">new</span> <span class="kwrd">char</span>[cchver];
        BOOL bret = GetFileVersionInfo(filename,dwHandle,cchver,pver);
        <span class="kwrd">if</span> (!bret)
            <span class="kwrd">return</span> E_FAIL;<span class="rem">//LastError();</span>
        UINT uLen;
        <span class="kwrd">void</span> *pbuf;
        bret = VerQueryValue(pver,<span class="str">"\",&pbuf,&uLen);
        if (!bret)
            return E_FAIL;//LastError();
        memcpy(pvsf,pbuf,sizeof(VS_FIXEDFILEINFO));
        delete[] pver;
        return S_OK;
    }
    
    HRESULT LastError () {
        HRESULT hr = HRESULT_FROM_WIN32(GetLastError());
        if (SUCCEEDED(hr))
            return E_FAIL;
        return hr;
    }
    
    // This little function emits an error message based on WIN32 error messages
    void EmitErrorMsg (HRESULT hr) {
        char szMsg[1024];
        FormatMessage(
            FORMAT_MESSAGE_FROM_SYSTEM,
            NULL,
            hr,
            MAKELANGID(LANG_NEUTRAL, SUBLANG_DEFAULT),
            szMsg,
            1024,
            NULL
            );
        printf("</span>%sn<span class="str">",szMsg);
    }


​    
    BOOL StartPrint(PrintParam nPrintType)
    {
        char szIniPATH[1024];
        GetModuleFileName(NULL, szIniPATH, 1024);
        PathRemoveFileSpec(szIniPATH);
        PathAddBackslash(szIniPATH);
    
        CFileFind finder;
        CString folder = szIniPATH;
        folder += "</span>*.*<span class="str">";
    
        BOOL bWorking = finder.FindFile(folder);
        if (!bWorking) //Folder not found...
        {
            return FALSE;
        }
    
        BOOL bBuildFound = FALSE;
        while (bWorking)
        {
            bWorking = finder.FindNextFile();
    
            if (finder.IsDots())
                continue;
    
            if (!finder.IsDirectory())
            {
                CString strFileVersion;
                CString filePath = finder.GetFilePath();
                CString filename = finder.GetFileName();
    
                if (nPrintType == Print_Version)
                    GetFileVersionString(filePath.GetBuffer(), strFileVersion);
                else if (nPrintType == Print_Time)
                    GetFileTimeString(filePath.GetBuffer(), strFileVersion);
    
                printf("</span>%s, %sn", filename, strFileVersion);
            }
        }
    
        <span class="kwrd">return</span> TRUE;
    }




.csharpcode, .csharpcode pre
{
​	font-size: small;
​	color: black;
​	font-family: consolas, "Courier New", courier, monospace;
​	background-color: #ffffff;
​	/*white-space: pre;*/
}
.csharpcode pre { margin: 0em; }
.csharpcode .rem { color: #008000; }
.csharpcode .kwrd { color: #0000ff; }
.csharpcode .str { color: #006080; }
.csharpcode .op { color: #0000c0; }
.csharpcode .preproc { color: #cc6633; }
.csharpcode .asp { background-color: #ffff00; }
.csharpcode .html { color: #800000; }
.csharpcode .attr { color: #ff0000; }
.csharpcode .alt
{
​	background-color: #f4f4f4;
​	width: 100%;
​	margin: 0em;
}
.csharpcode .lnum { color: #606060; }
