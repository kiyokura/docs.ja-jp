---
title: "ICorProfilerCallback2::SurvivingReferences メソッド"
ms.custom: 
ms.date: 03/30/2017
ms.prod: .net-framework
ms.reviewer: 
ms.suite: 
ms.technology: dotnet-clr
ms.tgt_pltfrm: 
ms.topic: reference
api_name: ICorProfilerCallback2.SurvivingReferences
api_location: mscorwks.dll
api_type: COM
f1_keywords: ICorProfilerCallback2::SurvivingReferences
helpviewer_keywords:
- ICorProfilerCallback2::SurvivingReferences method [.NET Framework profiling]
- SurvivingReferences method [.NET Framework profiling]
ms.assetid: f165200e-3a91-47f7-88fc-13ff10c8babc
topic_type: apiref
caps.latest.revision: "16"
author: mairaw
ms.author: mairaw
manager: wpickett
ms.openlocfilehash: 3764bfb79b39af897600202e8bc0f2ffbc4da95b
ms.sourcegitcommit: 4f3fef493080a43e70e951223894768d36ce430a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/21/2017
---
# <a name="icorprofilercallback2survivingreferences-method"></a>ICorProfilerCallback2::SurvivingReferences メソッド
非圧縮ガベージ コレクションを実行した後の、ヒープ内のオブジェクトのレイアウトを報告します。  
  
## <a name="syntax"></a>構文  
  
```  
HRESULT SurvivingReferences(  
    [in] ULONG  cSurvivingObjectIDRanges,  
    [in, size_is(cSurvivingObjectIDRanges)] ObjectID  
                objectIDRangeStart[] ,  
    [in, size_is(cSurvivingObjectIDRanges)] ULONG  
                cObjectIDRangeLength[] );  
```  
  
#### <a name="parameters"></a>パラメーター  
 `cSurvivingObjectIDRanges`  
 [in] 非圧縮ガベージ コレクションを実行した後に存続する、隣接したオブジェクトのブロック数。 つまり、`cSurvivingObjectIDRanges` の値は、`objectIDRangeStart` 配列と `cObjectIDRangeLength` 配列のサイズを表します。これらの配列にはそれぞれ、オブジェクトの各ブロックの `ObjectID` と長さが格納されます。  
  
 `SurvivingReferences` の次の2個の引数は並列配列です。 つまり、`objectIDRangeStart` と `cObjectIDRangeLength` は隣接するオブジェクトの同じブロックを対象としています。  
  
 `objectIDRangeStart`  
 [in] それぞれがメモリ内の有効な隣接オブジェクト ブロックの開始アドレスを表す、`ObjectID` 値の配列。  
  
 `cObjectIDRangeLength`  
 [in] それぞれがメモリ内に存続する隣接オブジェクト ブロックのサイズを表す、整数の配列。  
  
 サイズは、`objectIDRangeStart` 配列内の参照される各ブロックに対して指定します。  
  
## <a name="remarks"></a>コメント  
  
> [!IMPORTANT]
>  このメソッドは、64 ビット プラットフォームで 4 GB より大きいオブジェクトのサイズを `MAX_ULONG` として報告します。 4 GB より大きいオブジェクトを使用して、 [icorprofilercallback 4::survivingreferences2](../../../../docs/framework/unmanaged-api/profiling/icorprofilercallback4-survivingreferences2-method.md)メソッド代わりにします。  
  
 `objectIDRangeStart` 配列と `cObjectIDRangeLength` 配列の要素は、次のように解釈されて、ガベージ コレクションでオブジェクトが存続したかどうかを判断する必要があります。 `ObjectID` 値 (`ObjectID`) が次の範囲内にあるとします。  
  
 `ObjectIDRangeStart[i]` <= `ObjectID` < `ObjectIDRangeStart[i]` + `cObjectIDRangeLength[i]`  
  
 次の範囲内にある `i` のすべての値について、オブジェクトはガベージ コレクションの実行後に存続しています。  
  
 0 <= `i` < `cSurvivingObjectIDRanges`  
  
 非圧縮ガベージ コレクションは、"無効な" オブジェクトによって占有されているメモリをクリアしますが、解放された領域は圧縮しません。 そのため、メモリはヒープに返されますが、"有効な" オブジェクトは移動されません。  
  
 共通言語ランタイム (CLR: Common Language Runtime) は、非圧縮ガベージ コレクションに対して `SurvivingReferences` を呼び出します。 圧縮ガベージ コレクション、 [icorprofilercallback::movedreferences](../../../../docs/framework/unmanaged-api/profiling/icorprofilercallback-movedreferences-method.md)は代わりに呼び出されます。 単一のガベージ コレクションで 1 つのジェネレーションを圧縮できますが、その他のジェネレーションは非圧縮になります。 どの特定のジェネレーションのガベージ コレクションについても、プロファイラーは `SurvivingReferences` コールバックと `MovedReferences` コールバックのいずれかを受け取り、両方を受け取ることはありません。  
  
 特定のガベージ コレクションで複数の `SurvivingReferences` コールバックを受け取ることがあります。この原因としては、内部バッファリングの制限、サーバーのガベージ コレクション中の複数のコールバックなどが考えられます。 あるガベージ コレクションで複数のコールバックが生じる場合、情報が累積されます。つまり、`SurvivingReferences` コールバックで報告されるすべての参照は対象のガベージ コレクション後も存続します。  
  
## <a name="requirements"></a>要件  
 **プラットフォーム:**を参照してください[システム要件](../../../../docs/framework/get-started/system-requirements.md)です。  
  
 **ヘッダー** : CorProf.idl、CorProf.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET framework のバージョン:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>関連項目  
 [ICorProfilerCallback インターフェイス](../../../../docs/framework/unmanaged-api/profiling/icorprofilercallback-interface.md)  
 [ICorProfilerCallback2 インターフェイス](../../../../docs/framework/unmanaged-api/profiling/icorprofilercallback2-interface.md)  
 [SurvivingReferences2 メソッド](../../../../docs/framework/unmanaged-api/profiling/icorprofilercallback4-survivingreferences2-method.md)