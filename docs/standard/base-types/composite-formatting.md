---
title: "複合書式指定 | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-standard"
ms.tgt_pltfrm: ""
ms.topic: "article"
helpviewer_keywords: 
  - "複合書式設定"
  - "書式指定子, 複合書式設定"
  - "オブジェクト [.NET Framework], 書式設定 (複数のオブジェクトの)"
  - "パラメーター指定子"
  - "文字列 [.NET Framework], 配置"
  - "文字列 [.NET Framework], 複合"
ms.assetid: 87b7d528-73f6-43c6-b71a-f23043039a49
caps.latest.revision: 36
author: "rpetrusha"
ms.author: "ronpet"
manager: "wpickett"
caps.handback.revision: 33
---
# 複合書式指定
.NET Framework の複合書式指定機能は、オブジェクトのリストおよび複合書式指定文字列を入力として使用します。  複合書式指定文字列は、固定テキストに、書式指定項目と呼ばれるインデックス化されたプレースホルダーが混合されて構成されます。このプレースホルダーはリスト内のオブジェクトに対応します。  書式設定操作によって生成される結果の文字列は、元の固定テキストに文字列で表されたリスト内のオブジェクトが混合されて構成されます。  
  
 複合書式指定機能をサポートするメソッドには、次のようなものがあります。  
  
-   <xref:System.String.Format%2A?displayProperty=fullName>。書式設定された結果文字列を返します。  
  
-   <xref:System.Text.StringBuilder.AppendFormat%2A?displayProperty=fullName>。書式設定された結果文字列を <xref:System.Text.StringBuilder> オブジェクトに追加します。  
  
-   <xref:System.Console.WriteLine%2A?displayProperty=fullName> メソッドの一部のオーバーロード。書式設定された結果文字列をコンソールに表示します。  
  
-   <xref:System.IO.TextWriter.WriteLine%2A?displayProperty=fullName> メソッドの一部のオーバーロード。書式設定された結果文字列をストリームまたはファイルに書き込みます。  <xref:System.IO.TextWriter> から派生したクラス \(<xref:System.IO.StreamWriter>、<xref:System.Web.UI.HtmlTextWriter> など\) も、この機能を共有します。  
  
-   [Debug.WriteLine\(String, Object\<xref:System.Diagnostics.Debug.WriteLine%28System.String%2CSystem.Object%5B%5D%29?displayProperty=fullName>。書式設定されたメッセージをトレース リスナーに出力します。  
  
-   [Trace.TraceError\(String, Object\<xref:System.Diagnostics.Trace.TraceError%28System.String%2CSystem.Object%5B%5D%29?displayProperty=fullName>、[Trace.TraceInformation\(String, Object\<xref:System.Diagnostics.Trace.TraceInformation%28System.String%2CSystem.Object%5B%5D%29?displayProperty=fullName>、および [Trace.TraceWarning\(String, Object\<xref:System.Diagnostics.Trace.TraceWarning%28System.String%2CSystem.Object%5B%5D%29?displayProperty=fullName> メソッド。書式設定されたメッセージをトレース リスナーに出力します。  
  
-   [TraceSource.TraceInformation\(String, Object\<xref:System.Diagnostics.TraceSource.TraceInformation%28System.String%2CSystem.Object%5B%5D%29?displayProperty=fullName> メソッド。情報提供メソッドをトレース リスナーに書き込みます。  
  
## 複合書式指定文字列  
 複合書式指定文字列とオブジェクト リストは、複合書式指定機能をサポートするメソッドの引数として使用されます。  複合書式指定文字列は、1 つ以上の書式指定項目が混合された 0 個以上の固定テキストで構成されます。  固定テキストはユーザーが任意に選択した文字列で、各書式指定項目はリスト内のオブジェクトまたはボックス化された構造体に対応します。  複合書式指定機能は、各書式指定項目がリスト内の対応するオブジェクトの文字列表現で置換された新しい文字列を返します。  
  
 次の <xref:System.String.Format%2A> コードがあるとします。  
  
 [!code-csharp[Formatting.Composite#1](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.Composite/cs/Composite1.cs#1)]
 [!code-vb[Formatting.Composite#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.Composite/vb/Composite1.vb#1)]  
  
 固定テキストは、"`Name =` " および "`, hours =` " です。  書式指定項目の 1 つは、インデックスが 0 である "`{0}`" であり、オブジェクト  `myName` に対応します。もう 1 つはインデックスが 1 である "`{1:hh}`" であり、オブジェクト  `DateTime.Now` に対応します。  
  
## 書式指定項目の構文  
 各書式指定項目は、次の形式を使用し、次のコンポーネントで構成されます。  
  
 `{` *index*\[`,`*alignment*\]\[`:`*formatString*\]`}`  
  
 対になった中かっこ \("{" と "}"\) が必要です。  
  
### Index コンポーネント  
 必須の *index* コンポーネントは、パラメーター指定子とも呼ばれ、オブジェクトのリスト内で対応する項目を識別するための 0 から始まる数値です。  つまり、パラメーター指定子が 0 である書式指定項目はリスト内の最初のオブジェクトを書式設定し、パラメーター指定子が 1 である書式指定項目はリスト内の 2 番目のオブジェクトを書式設定します。  次の例には、10 未満の素数を表す 5 つのパラメーター指定子が含まれています。  
  
 [!code-csharp[Formatting.Composite#7](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.Composite/cs/index1.cs#7)]
 [!code-vb[Formatting.Composite#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.Composite/vb/index1.vb#7)]  
  
 同じパラメーター指定子を指定することによって、複数の書式指定項目でオブジェクトのリスト内の同じ要素を参照できます。  たとえば、複合書式指定文字列で "0x{0:X} {0:E} {0:N}" のように指定することによって、1 つの数値を 16 進形式、指数形式、および数値形式で書式設定できます。以下に例を示します。  
  
 [!code-csharp[Formatting.Composite#10](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.Composite/cs/index1.cs#10)]
 [!code-vb[Formatting.Composite#10](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.Composite/vb/index1.vb#10)]  
  
 各書式指定項目は、リスト内のどのオブジェクトでも参照できます。  たとえば、3 つのオブジェクトが存在する場合、2 番目、1 番目、3 番目のオブジェクトを書式設定するには、"{1} {0} {2}" のような複合書式指定文字列を指定します。  書式指定項目で参照されないオブジェクトは無視されます。  パラメーター指定子がオブジェクトのリストの範囲外の項目を指定する場合は、ランタイムに <xref:System.FormatException> がスローされます。  
  
### Alignment コンポーネント  
 省略可能な *alignment* コンポーネントは、書式設定フィールドの幅を指定する符号付き整数です。  *alignment* の値が書式設定する文字列の長さよりも小さい場合、*alignment* は無視され、書式設定する文字列の長さがフィールドの幅として使用されます。  フィールド内の書式設定されたデータは、*alignment* が正の場合は右揃え、*alignment* が負の場合は左揃えされます。  埋め込みが必要な場合は、空白が使用されます。  *alignment* を指定する場合は、コンマが必要です。  
  
 次の例では、2 つの配列を定義します。1 つの配列には従業員の名前が含まれていて、もう 1 つの配列には従業員が 2 週間にわたって作業した時間数が含まれています。  複合書式指定文字列では、名前が 20 文字のフィールドに左揃えに指定され、時間数が 5 文字のフィールドに右揃えに指定されます。  "N1" 標準書式指定文字列も、時間を 1 桁の小数部で書式設定するために使用されることに注意してください。  
  
 [!code-csharp[Formatting.Composite#8](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.Composite/cs/alignment1.cs#8)]
 [!code-vb[Formatting.Composite#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.Composite/vb/alignment1.vb#8)]  
  
### Format String コンポーネント  
 オプションの *formatString* コンポーネントは、書式設定されるオブジェクトの種類に適した書式指定文字列です。  対応するオブジェクトが数値の場合は標準またはカスタムの数値書式指定文字列を指定し、対応するオブジェクトが <xref:System.DateTime> オブジェクトの場合は標準またはカスタムの日時書式指定文字列を指定し、対応するオブジェクトが列挙値の場合は[列挙型書式指定文字列](../../../docs/standard/base-types/enumeration-format-strings.md)を指定します。  *formatString* が指定されない場合は、数値、日付と時刻、または列挙型の汎用 \("G"\) 書式指定子が使用されます。  *formatString* を指定する場合はコロンが必要です。  
  
 次の表は、定義済みの一連の書式指定文字列をサポートする .NET Framework クラス ライブラリ内の型または型のカテゴリの一覧です。サポートされている書式指定文字列が示されているトピックへのリンクも含まれています。  文字列の書式設定とは拡張可能な機構で、既存のすべての型に対する新しい書式指定文字列を定義できるだけでなく、アプリケーション定義の型でサポートされる一連の書式指定文字列も定義できます。  詳しくは、<xref:System.IFormattable> および <xref:System.ICustomFormatter> のインターフェイスに関するトピックを参照してください。  
  
|型または型のカテゴリ|参照トピック|  
|----------------|------------|  
|日付と時刻の型 \(<xref:System.DateTime>、<xref:System.DateTimeOffset>\)|[標準の日時書式指定文字列](../../../docs/standard/base-types/standard-date-and-time-format-strings.md)<br /><br /> [カスタム日時書式指定文字列](../../../docs/standard/base-types/custom-date-and-time-format-strings.md)|  
|列挙型 \(<xref:System.Enum?displayProperty=fullName> から派生したすべての型\)|[列挙型書式指定文字列](../../../docs/standard/base-types/enumeration-format-strings.md)|  
|数値型 \(<xref:System.Numerics.BigInteger>、<xref:System.Byte>、<xref:System.Decimal>、<xref:System.Double>、<xref:System.Int16>、<xref:System.Int32>、<xref:System.Int64>、<xref:System.SByte>、<xref:System.Single>、<xref:System.UInt16>、<xref:System.UInt32>、<xref:System.UInt64>\)|[標準の数値書式指定文字列](../../../docs/standard/base-types/standard-numeric-format-strings.md)<br /><br /> [カスタム数値書式指定文字列](../../../docs/standard/base-types/custom-numeric-format-strings.md)|  
|<xref:System.Guid>|<xref:System.Guid.ToString%28System.String%29?displayProperty=fullName>|  
|<xref:System.TimeSpan>|[標準の時間間隔書式指定文字列](../../../docs/standard/base-types/standard-timespan-format-strings.md)<br /><br /> [カスタム時間間隔書式指定文字列](../../../docs/standard/base-types/custom-timespan-format-strings.md)|  
  
### エスケープ中かっこ \({}\)  
 左中かっこ \({\) および右中かっこ \(}\) は、書式指定項目の開始および終了として解釈されます。  したがって、左中かっこおよび右中かっこを文字として表示するためには、エスケープ シーケンスを使用する必要があります。  左中かっこを 1 つ \("{"\) 表示するには、左中かっこ 2 つ \("{{"\) を固定テキストに指定します。また、右中かっこを 1 つ \("}"\) 表示するには、右中かっこ 2 つ \("}}"\) を指定します。  書式指定項目に使用されている中かっこは、指定されている順序に従って解釈されます。  入れ子になった中かっこを解釈する機能はサポートされていません。  
  
 エスケープされた中かっこの解釈によっては、予測しない結果になる場合があります。  たとえば、"{{{0:D}}}" という書式指定項目について考えます。この書式指定項目は、左中かっこ、10 進数として書式設定された数値、および右中かっこを表示することを意図しています。  しかし、この書式指定項目は、実際には、次のように解釈されます。  
  
1.  最初の 2 つの左中かっこ \("{{"\) がエスケープされ、左中かっこ 1 つが作成されます。  
  
2.  次の 3 文字 \("{0:"\) は、書式指定項目の開始として解釈されます。  
  
3.  次の文字 \("D"\) は、Decimal 標準数値書式指定子として解釈できますが、エスケープされた次の 2 つの右中かっこ \("}}"\) からは単一の中かっこが作成されます。  結果として作成される文字列 \("D}"\) は、標準数値書式指定子ではないため、リテラル文字列 "D}" の表示を意味するカスタム書式指定文字列として解釈されます。  
  
4.  最後の中かっこ \("}"\) は、書式指定項目の終わりとして解釈されます。  
  
5.  最終的には、"{D}" というリテラル文字列が表示されます。  書式設定対象だった数値は、表示されません。  
  
 エスケープした中かっこと書式指定項目とが誤って解釈されないコードを記述するためには、中かっこと書式指定項目を別々に書式設定するという方法があります。  つまり、最初の書式設定操作でリテラルの開く中かっこを表示し、次の書式設定操作で書式指定項目の結果を表示し、最後の操作でリテラルの閉じる中かっこを表示します。  このアプローチの例を次に示します。  
  
 [!code-csharp[Formatting.Composite#2](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.Composite/cs/Escaping1.cs#2)]
 [!code-vb[Formatting.Composite#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.Composite/vb/Escaping1.vb#2)]  
  
### 処理の順序  
 複合書式指定メソッドの呼び出しに、値が <xref:System.IFormatProvider> でない `null` 引数が含まれている場合、ランタイムはその <xref:System.IFormatProvider.GetFormat%2A?displayProperty=fullName> メソッドを呼び出して、<xref:System.ICustomFormatter> 実装を要求します。  このメソッドが <xref:System.ICustomFormatter> 実装を返すことができる場合、実装は後で使用できるようにキャッシュされます。  
  
 書式指定項目に対応するパラメーター リストのそれぞれの値は、次の手順を実行することで文字列に変換されます。  最初の 3 つの手順の条件のいずれかに該当する場合は、その手順で値の文字列形式が返され、後続の手順は実行されません。  
  
1.  書式設定する値が `null` の場合は、空の文字列 \(""\) が返されます。  
  
2.  <xref:System.ICustomFormatter> 実装が利用できる場合、ランタイムはその <xref:System.ICustomFormatter.Format%2A> メソッドを呼び出します。  メソッドには書式指定項目の *formatString* 値 \(ある場合\) または `null` \(ない場合\) と、<xref:System.IFormatProvider> 実装が渡されます。  
  
3.  値が <xref:System.IFormattable> インターフェイスを実装している場合は、インターフェイスの <xref:System.IFormattable.ToString%28System.String%2CSystem.IFormatProvider%29> メソッドが呼び出されます。  メソッドは、*formatString* 値 \(書式指定項目内に値がある場合\) または `null` \(ない場合\) を受け取ります。  <xref:System.IFormatProvider> 引数は、次のように判断されます。  
  
    -   数値の場合、null 以外の <xref:System.IFormatProvider> 引数を持つ複合書式指定メソッドが呼び出されると、ランタイムは <xref:System.Globalization.NumberFormatInfo> メソッドから <xref:System.IFormatProvider.GetFormat%2A?displayProperty=fullName> オブジェクトを要求します。  オブジェクトを 1 つも取得できないか、引数の値が `null` であるか、または複合書式指定メソッドに <xref:System.IFormatProvider> パラメーターがない場合は、現在のスレッド カルチャの <xref:System.Globalization.NumberFormatInfo> オブジェクトが使用されます。  
  
    -   日付と時刻の値の場合、null 以外の <xref:System.IFormatProvider> 引数を持つ複合書式指定メソッドが呼び出されると、ランタイムは <xref:System.Globalization.DateTimeFormatInfo> メソッドから <xref:System.IFormatProvider.GetFormat%2A?displayProperty=fullName> オブジェクトを要求します。  オブジェクトを 1 つも取得できないか、引数の値が `null` であるか、または複合書式指定メソッドに <xref:System.IFormatProvider> パラメーターがない場合は、現在のスレッド カルチャの <xref:System.Globalization.DateTimeFormatInfo> オブジェクトが使用されます。  
  
    -   他の型のオブジェクトの場合、<xref:System.IFormatProvider> 引数を持つ複合書式指定が呼び出されると、その値 \(`null` オブジェクトが指定されない場合の <xref:System.IFormatProvider> を含む\) は <xref:System.IFormattable.ToString%2A?displayProperty=fullName> 実装に直接渡されます。  それ以外の場合は、現在のスレッド カルチャを表す <xref:System.Globalization.CultureInfo> オブジェクトが <xref:System.IFormattable.ToString%2A?displayProperty=fullName> 実装に渡されます。  
  
4.  `ToString` をオーバーライドするか、基底クラスの動作を継承する、型のパラメーターなしの <xref:System.Object.ToString?displayProperty=fullName> メソッドが呼び出されます。  この場合、書式指定項目の *formatString* コンポーネントで指定されている書式指定文字列は、存在していても無視されます。  
  
 前の手順が実行された後、アラインメントが適用されます。  
  
## コード例  
 複合書式指定を使用して文字列を作成する方法と、オブジェクトの `ToString` メソッドを使用して文字列を作成する方法を示すコード例を次に示します。  この 2 つの書式設定方法では、等価の文字列が作成されます。  
  
 [!code-csharp[Formatting.Composite#3](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.Composite/cs/Composite1.cs#3)]
 [!code-vb[Formatting.Composite#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.Composite/vb/Composite1.vb#3)]  
  
 現在の曜日が木曜日であり、月が 5 月であり、現在のカルチャが英語 \(U.S.\) の場合、上記のコード例で作成される 2 つの文字列の値はどちらも `Thursday May` です。  
  
 <xref:System.Console.WriteLine%2A?displayProperty=fullName> は、<xref:System.String.Format%2A?displayProperty=fullName> と同じ機能を公開します。  これら 2 つのメソッドの唯一の違いは、<xref:System.String.Format%2A?displayProperty=fullName> が結果を文字列で返すのに対し、<xref:System.Console.WriteLine%2A?displayProperty=fullName> は、<xref:System.Console> オブジェクトに関連付けられた出力ストリームに結果を書き出す点です。  <xref:System.Console.WriteLine%2A?displayProperty=fullName> メソッドを使用して値 `MyInt` を通貨値として書式設定するコード例を次に示します。  
  
 [!code-csharp[Formatting.Composite#4](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.Composite/cs/Composite1.cs#4)]
 [!code-vb[Formatting.Composite#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.Composite/vb/Composite1.vb#4)]  
  
 1 つのオブジェクトを 2 つの方法で書式設定する例を含め、複数のオブジェクトを書式設定する例を次に示します。  
  
 [!code-csharp[Formatting.Composite#5](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.Composite/cs/Composite1.cs#5)]
 [!code-vb[Formatting.Composite#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.Composite/vb/Composite1.vb#5)]  
  
 書式設定でアラインメントを使用する方法を示す例を次に示します。  アラインメント結果を強調表示するため、書式が設定される引数を縦棒 \(&#124;\) で囲んでいます。  
  
 [!code-csharp[Formatting.Composite#6](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.Composite/cs/Composite1.cs#6)]
 [!code-vb[Formatting.Composite#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.Composite/vb/Composite1.vb#6)]  
  
## 参照  
 <xref:System.Console.WriteLine%2A>   
 <xref:System.String.Format%2A?displayProperty=fullName>   
 [型の書式設定](../../../docs/standard/base-types/formatting-types.md)   
 [標準の数値書式指定文字列](../../../docs/standard/base-types/standard-numeric-format-strings.md)   
 [カスタム数値書式指定文字列](../../../docs/standard/base-types/custom-numeric-format-strings.md)   
 [標準の日時書式指定文字列](../../../docs/standard/base-types/standard-date-and-time-format-strings.md)   
 [カスタム日時書式指定文字列](../../../docs/standard/base-types/custom-date-and-time-format-strings.md)   
 [標準の時間間隔書式指定文字列](../../../docs/standard/base-types/standard-timespan-format-strings.md)   
 [カスタム時間間隔書式指定文字列](../../../docs/standard/base-types/custom-timespan-format-strings.md)   
 [列挙型書式指定文字列](../../../docs/standard/base-types/enumeration-format-strings.md)