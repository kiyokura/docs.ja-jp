---
title: 'メソッド ベースのクエリ構文例: リレーションシップのナビゲーション'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: a0bfa4b1-99e5-4dd1-9912-4b825a9dc25c
ms.openlocfilehash: 6435cf097b2fab880271d2c79ac8bb1afaf9cb6b
ms.sourcegitcommit: 11f11ca6cefe555972b3a5c99729d1a7523d8f50
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/03/2018
ms.locfileid: "32763216"
---
# <a name="method-based-query-syntax-examples-navigating-relationships"></a>メソッド ベースのクエリ構文例: リレーションシップのナビゲーション
[!INCLUDE[adonet_ef](../../../../../../includes/adonet-ef-md.md)] のナビゲーション プロパティは、アソシエーションの末尾にあるエンティティを見つけるために使用できるショートカット プロパティです。 ナビゲーション プロパティを使用すると、ユーザーは、エンティティ間をナビゲートしたり、あるエンティティからアソシエーション セットを介して関連エンティティにナビゲートしたりできます。 このトピックでは、[!INCLUDE[linq_entities](../../../../../../includes/linq-entities-md.md)] クエリ内でナビゲーション プロパティを介してリレーションシップをナビゲートするためのメソッド ベースのクエリ構文の例を示します。  
  
 これらの例で使用されている、AdventureWorks Sales Model は、AdventureWorks サンプル データベースの Contact、Address、Product、SalesOrderHeader、SalesOrderDetail の各テーブルから作成されています。  
  
 このトピックの例では、次を使用して`using` / `Imports`ステートメント。  
  
 [!code-csharp[DP L2E Examples#ImportsUsing](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DP L2E Examples/CS/Program.cs#importsusing)]
 [!code-vb[DP L2E Examples#ImportsUsing](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DP L2E Examples/VB/Module1.vb#importsusing)]  
  
## <a name="example"></a>例  
 次の例では、メソッドベースのクエリ構文で <xref:System.Linq.Queryable.SelectMany%2A> メソッドを使用して、姓が "Zhou" である連絡先のすべての注文を取得します。 `Contact.SalesOrderHeader` ナビゲーション プロパティは、各連絡先の `SalesOrderHeader` オブジェクトのコレクションを取得するために使用されます。  
  
 [!code-csharp[DP L2E Examples#SelectEachContactsOrders_MQ](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DP L2E Examples/CS/Program.cs#selecteachcontactsorders_mq)]
 [!code-vb[DP L2E Examples#SelectEachContactsOrders_MQ](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DP L2E Examples/VB/Module1.vb#selecteachcontactsorders_mq)]  
  
## <a name="example"></a>例  
 次の例では、メソッドベースのクエリ構文で <xref:System.Linq.Queryable.Select%2A> メソッドを使用して、姓が "Zhou" である連絡先のすべての連絡先 ID と、各連絡先の合計支払額の総計を取得します。 `Contact.SalesOrderHeader` ナビゲーション プロパティは、各連絡先の `SalesOrderHeader` オブジェクトのコレクションを取得するために使用されます。 `Sum` メソッドは、`Contact.SalesOrderHeader` ナビゲーション プロパティを使用して、それぞれの連絡先のすべての注文の合計支払額を合計します。  
  
 [!code-csharp[DP L2E Examples#SelectEachContactsOrders2_MQ](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DP L2E Examples/CS/Program.cs#selecteachcontactsorders2_mq)]
 [!code-vb[DP L2E Examples#SelectEachContactsOrders2_MQ](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DP L2E Examples/VB/Module1.vb#selecteachcontactsorders2_mq)]  
  
## <a name="example"></a>例  
 次の例では、メソッド ベースのクエリ構文を使用し、姓が "Zhou" である連絡先のすべての注文を取得します。 `Contact.SalesOrderHeader` ナビゲーション プロパティは、各連絡先の `SalesOrderHeader` オブジェクトのコレクションを取得するために使用されます。 連絡先の名前と注文が匿名型で返されます。  
  
 [!code-csharp[DP L2E Examples#SelectEachContactsOrders3_MQ](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DP L2E Examples/CS/Program.cs#selecteachcontactsorders3_mq)]
 [!code-vb[DP L2E Examples#SelectEachContactsOrders3_MQ](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DP L2E Examples/VB/Module1.vb#selecteachcontactsorders3_mq)]  
  
## <a name="example"></a>例  
 次の例では、`SalesOrderHeader.Address` ナビゲーション プロパティと `SalesOrderHeader.Contact` ナビゲーション プロパティを使用して、互いに関連付けられている `Address` オブジェクトと `Contact` オブジェクトのコレクションを取得します。 各注文の連絡先の姓、住所、販売注文番号、および Seattle 市に対する合計支払額が匿名型で返されます。  
  
 [!code-csharp[DP L2E Examples#GetOrderInfoThruRelationships_MQ](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DP L2E Examples/CS/Program.cs#getorderinfothrurelationships_mq)]
 [!code-vb[DP L2E Examples#GetOrderInfoThruRelationships_MQ](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DP L2E Examples/VB/Module1.vb#getorderinfothrurelationships_mq)]  
  
## <a name="example"></a>例  
 次の例では、`Where` メソッドを使用して、2003 年 12 月 1 日以降に受けた注文を検索します。次に、`order.SalesOrderDetail` ナビゲーション プロパティを使用して、各注文の詳細を取得します。  
  
 [!code-csharp[DP L2E Examples#WhereNavProperty](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DP L2E Examples/CS/Program.cs#wherenavproperty)]
 [!code-vb[DP L2E Examples#WhereNavProperty](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DP L2E Examples/VB/Module1.vb#wherenavproperty)]  
  
## <a name="see-also"></a>関連項目  
 [ナビゲーション プロパティ](http://msdn.microsoft.com/library/41e1e6b9-8a57-467d-99d9-1857d2ca2ea5)  
 [LINQ to Entities でのクエリ](../../../../../../docs/framework/data/adonet/ef/language-reference/queries-in-linq-to-entities.md)
