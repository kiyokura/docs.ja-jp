---
title: "セキュリティとユーザー入力 | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-standard"
ms.tgt_pltfrm: ""
ms.topic: "article"
dev_langs: 
  - "VB"
  - "CSharp"
  - "C++"
  - "jsharp"
helpviewer_keywords: 
  - "コード セキュリティ, ユーザー入力"
  - "安全なコーディング, ユーザー入力"
  - "セキュリティ [.NET Framework], ユーザー入力"
  - "ユーザー入力, セキュリティ"
ms.assetid: 9141076a-96c9-4b01-93de-366bb1d858bc
caps.latest.revision: 7
author: "mairaw"
ms.author: "mairaw"
manager: "wpickett"
caps.handback.revision: 5
---
# セキュリティとユーザー入力
さまざまな種類の入力 \(Web 要求または URL からのデータ、Microsoft Windows フォーム アプリケーションを制御する入力など\) で得られるユーザー データは、他のデータを呼び出すときのパラメーターとして直接使用されることがあり、コードに悪影響を与える可能性があります。  この状況は、悪意のあるコードが異常なパラメーターを使用してコードを呼び出すことに類似しているため、同様の対策が必要です。  ユーザー入力を安全にすることは、実際には非常に難しいことです。信頼できない可能性があるデータの存在を追跡するためのスタック フレームがないからです。  
  
 これらは、セキュリティとは無関係に見えるコード内に存在しながら、他のコードを通じて悪意のあるデータを通してしまうため、最も微妙で最も見つけるのが難しいセキュリティ バグと言えます。  これらのバグを探すには、さまざまな種類の入力データを追跡し、可能な値の範囲を推測し、さらにこのデータを見ているコードがこれらのすべてのケースを処理できるのかを考慮します。  値の範囲をチェックし、コードが処理できないデータを拒否することによって、これらのバグを修正できます。  
  
 ユーザー データに関して、考慮が必要な点を以下に示します。  
  
-   サーバー応答内のどのユーザー データも、クライアント上のサーバー側のコンテキストで実行されます。  Web サーバーがユーザー データを受け取り、返される Web ページに挿入するように、サーバーからたとえば **\<script\>** タグを含めて、実行される可能性があります。  
  
-   クライアントは任意の URL を要求できるため、注意が必要です。  
  
-   次のような異常なパスまたは不正なパスについて考慮します。  
  
    -   。\\、非常に長いパス。  
  
    -   ワイルドカード文字 \(\*\) の使用。  
  
    -   トークンの拡張 \(%token%\)。  
  
    -   特殊な意味を持つ異常な形式のパス。  
  
    -   代替ファイル システム ストリーム名。たとえば、`filename::$DATA` など。  
  
    -   `longfilename` に対する `longfi~1` などのファイル名の短縮バージョン。  
  
-   Eval\(userdata\) は任意のことを実行できるため、注意が必要です。  
  
-   いくつかのユーザー データを持つ名前への遅延バインディングには注意が必要です。  
  
-   Web データを処理する場合は、許可するさまざまなエスケープの形式について考慮します。次のような形式があります。  
  
    -   16 進エスケープ \(%nn\)。  
  
    -   Unicode エスケープ \(%nnn\)。  
  
    -   Overlong UTF\-8 エスケープ \(%nn%nn\)。  
  
    -   二重エスケープ \(%nn が %mmnn になります。%mm は '%' のエスケープです\)。  
  
-   複数の標準形式が含まれるユーザー名には注意が必要です。  たとえば、Microsoft Windows 2000 では、MYDOMAIN\\*username* 形式または *username*@mydomain.example.com 形式のいずれかを使用します。  
  
## 参照  
 [安全なコーディングのガイドライン](../../../docs/standard/security/secure-coding-guidelines.md)