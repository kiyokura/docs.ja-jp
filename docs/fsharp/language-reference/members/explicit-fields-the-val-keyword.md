---
title: "明示的なフィールド: val キーワード (F#)"
description: "については、f# 'val' キーワードは、型を初期化せずにクラスまたは構造体の型に値を格納する場所を宣言するために使用します。"
keywords: "visual f#, f#, 関数型プログラミング"
author: cartermp
ms.author: phcart
ms.date: 05/16/2016
ms.topic: language-reference
ms.prod: .net
ms.technology: devlang-fsharp
ms.devlang: fsharp
ms.assetid: 3bdbc745-436b-407f-bf54-5d11ca829cd0
ms.openlocfilehash: cee53a48f08aec89b0bdd40189ed331cadee877d
ms.sourcegitcommit: bd1ef61f4bb794b25383d3d72e71041a5ced172e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/18/2017
---
# <a name="explicit-fields-the-val-keyword"></a>明示的なフィールド: val キーワード

`val` キーワードを使用すると、クラス型または構造体型の値を格納する場所を初期化せずに宣言することができます。 この方法で宣言されている記憶域の場所といいます*明示的なフィールド*です。 `val` キーワードの別の用途として、`member` キーワードと組み合わせて自動実装プロパティを宣言する方法があります。 自動実装プロパティの詳細については、次を参照してください。[プロパティ](properties.md)です。


## <a name="syntax"></a>構文

```fsharp
val [ mutable ] [ access-modifier ] field-name : type-name
```

## <a name="remarks"></a>コメント
クラス型または構造体型のフィールドを定義するには、通常、`let` 束縛を使用します。 ただし、`let` 束縛は、クラス コンストラクターの一部として初期化する必要があります。これは、必ずしも可能または必要であるとは限らず、望ましくない場合もあります。 初期化されていないフィールドが必要な場合は、`val` キーワードを使用できます。

明示的なフィールドは静的にも非静的にもできます。 *アクセス修飾子*できます`public`、 `private`、または`internal`です。 既定では、明示的なフィールドは public です。 常に private であるクラスの `let` 束縛とは、この点が異なります。

[DefaultValue](https://msdn.microsoft.com/library/a3a3307b-8c05-441e-b109-245511614d58)属性は、明示的なフィールドをプライマリ コンス トラクターを持つクラス型に必要です。 この属性は、フィールドが 0 に初期化されることを示します。 フィールドの型ではゼロ初期化をサポートしている必要があります。 型が次のいずれかである場合は、ゼロ初期化がサポートされています。

- 値が 0 のプリミティブ型。

- 標準値、外れ値、または値の表現として null 値をサポートする型。 これには、クラス、タプル、レコード、関数、インターフェイス、.NET 参照型、`unit` 型、判別された共用体型が含まれます。

- .NET 値型。

- すべてのフィールドで既定値 0 がサポートされている構造体。


たとえば、`someField` と呼ばれる変更できないフィールドには、.NET によるコンパイル済み表現を使用した、`someField@` という名前のバッキング フィールドが含まれており、ユーザーは `someField` という名前のプロパティを使用して、格納されている値にアクセスします。

変更可能なフィールドの場合、.NET によるコンパイル済み表現は .NET フィールドになります。


>[!WARNING] 
`Note`.NET Framework 名前空間`System.ComponentModel`を同じ名前を持つ属性が含まれています。 この属性の詳細については、「`System.ComponentModel.DefaultValueAttribute`」を参照してください。


次のコードは、明示的なフィールドの使用方法を示しています。また、比較のために、プライマリ コンストラクターを持つクラスの `let` 束縛も示しています。 `let` 束縛のフィールド `myInt1` が private であることに注意してください。 `let` 束縛のフィールド `myInt1` をメンバー メソッドから参照する際は、自己識別子 `this` は必要ありません。 ただし、明示的なフィールド `myInt2` と `myString` を参照する際は、自己識別子が必要です。

[!code-fsharp[Main](../../../../samples/snippets/fsharp/lang-ref-2/snippet6701.fs)]

出力は次のとおりです。

```
11 12 abc
30 def
```

次のコードは、プライマリ コンストラクターを持たないクラスでの明示的なフィールドの使用方法を示しています。 この場合、`DefaultValue` 属性は必要ありませんが、その型用に定義されているコンストラクターですべてのフィールドが初期化される必要があります。

[!code-fsharp[Main](../../../../samples/snippets/fsharp/lang-ref-2/snippet6702.fs)]

出力は `35 22`になります。

次のコードは、構造体での明示的なフィールドの使用方法を示しています。 構造体は値型であるため、フィールドの値を 0 に設定する既定のコンストラクターが自動的に含まれます。 そのため、`DefaultValue` 属性は必要ありません。

[!code-fsharp[Main](../../../../samples/snippets/fsharp/lang-ref-2/snippet6703.fs)]

出力は `11 xyz`になります。

明示的なフィールドは日常的に使用するためのものではありません。 通常、可能な場合は、明示的なフィールドでなく、クラスで `let` 束縛を使用してください。 明示的なフィールドは、特定の相互運用のシナリオ (ネイティブ API に対するプラットフォーム呼び出しで使用される構造体を定義する必要がある場合など) や COM 相互運用のシナリオで役立ちます。 詳細については、次を参照してください。[外部関数](../functions/external-functions.md)です。 また、プライマリ コンストラクターを持たないクラスを生成する F# コード ジェネレーターを使用している場合にも、明示的なフィールドが必要になることがあります。 明示的なフィールドは、thread-static 変数や同様のコンストラクターでも役立ちます。 詳細については、「`System.ThreadStaticAttribute`」を参照してください。

キーワード `member val` が型定義にまとめて表示された場合は、自動的に実装されたプロパティの定義です。 詳細については、次を参照してください。[プロパティ](properties.md)です。


## <a name="see-also"></a>関連項目
[プロパティ](properties.md)

[メンバー](index.md)

[クラス内の `let` バインド](let-bindings-in-classes.md)