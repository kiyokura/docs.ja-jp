---
title: "方法: FTP を使用してファイルをアップロードする | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework"
ms.reviewer: ""
ms.suite: ""
ms.tgt_pltfrm: ""
ms.topic: "article"
dev_langs: 
  - "VB"
  - "CSharp"
  - "C++"
  - "jsharp"
ms.assetid: e40f17c5-dd12-4c62-9dbf-00ab491382dc
caps.latest.revision: 5
author: "mcleblanc"
ms.author: "markl"
manager: "markl"
caps.handback.revision: 5
---
# 方法: FTP を使用してファイルをアップロードする
このオプションは、FTP サーバーにファイルをアップロードする方法を示します。  
  
## 使用例  
  
```csharp  
using System;  
using System.IO;  
using System.Net;  
using System.Text;  
  
namespace Examples.System.Net  
{  
    public class WebRequestGetExample  
    {  
        public static void Main ()  
        {  
            // Get the object used to communicate with the server.  
            FtpWebRequest request = (FtpWebRequest)WebRequest.Create("ftp://www.contoso.com/test.htm");  
            request.Method = WebRequestMethods.Ftp.UploadFile;  
  
            // This example assumes the FTP site uses anonymous logon.  
            request.Credentials = new NetworkCredential ("anonymous","janeDoe@contoso.com");  
  
            // Copy the contents of the file to the request stream.  
            StreamReader sourceStream = new StreamReader("testfile.txt");  
            byte [] fileContents = Encoding.UTF8.GetBytes(sourceStream.ReadToEnd());  
            sourceStream.Close();  
            request.ContentLength = fileContents.Length;  
  
            Stream requestStream = request.GetRequestStream();  
            requestStream.Write(fileContents, 0, fileContents.Length);  
            requestStream.Close();  
  
            FtpWebResponse response = (FtpWebResponse)request.GetResponse();  
  
            Console.WriteLine("Upload File Complete, status {0}", response.StatusDescription);  
  
            response.Close();  
            }  
        }  
    }  
}  
```  
  
## コードのコンパイル  
 この例には、次の項目が必要です。  
  
-   **\[System.Net\]** の名前空間への参照。  
  
## 信頼性の高いプログラミング  
  
## .NET Framework セキュリティ