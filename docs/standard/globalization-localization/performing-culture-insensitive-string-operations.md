---
title: カルチャを認識しない文字列操作の実行
ms.date: 03/30/2017
ms.technology: dotnet-standard
helpviewer_keywords:
- case mappings
- custom case mappings
- culture, custom sorting rules
- custom sorting rules
- culture-insensitive string operations, options for performing
- culture, custom case mappings
- culture-insensitive string operations, method overloads
ms.assetid: 579ef891-1f83-4c63-9ebd-2f40406b5b91
author: rpetrusha
ms.author: ronpet
ms.openlocfilehash: 9500550fe415d77bacb44011622ddd83ffc8a9ec
ms.sourcegitcommit: 3d5d33f384eeba41b2dff79d096f47ccc8d8f03d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/04/2018
ms.locfileid: "33575376"
---
# <a name="performing-culture-insensitive-string-operations"></a>カルチャを認識しない文字列操作の実行
カルチャを認識する文字列操作を既定で実行するほとんどの .NET Framework メソッドには、<xref:System.Globalization.CultureInfo> パラメーターを渡すことによって使用するカルチャを明示的に指定できるメソッド オーバーロードが用意されています。 これらのオーバーロードによって、大文字小文字のマップおよび並べ替え規則のカルチャによる違いを排除し、カルチャを認識しない結果を確保できます。  
  
 ここでは、既定ではカルチャを認識する .NET Framework のメソッドを使用して、カルチャを認識しない文字列操作を実行する方法を説明します。  
  
## <a name="in-this-section"></a>このセクションの内容  
 [カルチャを認識しない文字列比較の実行](../../../docs/standard/globalization-localization/performing-culture-insensitive-string-comparisons.md)  
 <xref:System.String.Compare%2A?displayProperty=nameWithType> メソッドと <xref:System.String.CompareTo%2A?displayProperty=nameWithType> メソッドを使用してカルチャを認識しない文字列比較を実行する方法について説明します。  
  
 [カルチャを認識しない大文字と小文字の変更の実行](../../../docs/standard/globalization-localization/performing-culture-insensitive-case-changes.md)  
 <xref:System.String.ToUpper%2A?displayProperty=nameWithType>、<xref:System.String.ToLower%2A?displayProperty=nameWithType>、<xref:System.Char.ToUpper%2A?displayProperty=nameWithType>、<xref:System.Char.ToLower%2A?displayProperty=nameWithType> の各メソッドを使用してカルチャを認識しない大文字と小文字の変換を実行する方法について説明します。  
  
 [カルチャを認識しないコレクションの操作の実行](../../../docs/standard/globalization-localization/performing-culture-insensitive-string-operations-in-collections.md)  
 <xref:System.Collections.CaseInsensitiveComparer>、<xref:System.Collections.CaseInsensitiveHashCodeProvider> クラス、<xref:System.Collections.SortedList>、<xref:System.Collections.ArrayList.Sort%2A?displayProperty=nameWithType>、<xref:System.Collections.Specialized.CollectionsUtil.CreateCaseInsensitiveHashtable%2A?displayProperty=nameWithType> を使用して、コレクションでカルチャを認識しない操作を実行する方法について説明します。  
  
 [カルチャを認識しない配列の操作の実行](../../../docs/standard/globalization-localization/performing-culture-insensitive-string-operations-in-arrays.md)  
 <xref:System.Array.Sort%2A?displayProperty=nameWithType> メソッドと <xref:System.Array.BinarySearch%2A?displayProperty=nameWithType> メソッドを使用してカルチャを認識しない配列の操作を実行する方法について説明します。  
  
## <a name="related-sections"></a>関連項目  
 [カルチャを認識しない文字列操作](../../../docs/standard/globalization-localization/culture-insensitive-string-operations.md)  
 文字列操作を実行する際にカルチャに注意する理由について説明し、カルチャを認識する操作を実行するときとカルチャを認識しない操作を実行するときのガイドラインを示します。
