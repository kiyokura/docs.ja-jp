---
title: '&lt;oidMap&gt;要素'
ms.date: 03/30/2017
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#oidMap
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/mscorlib/cryptographySettings/oidMap
helpviewer_keywords:
- <oidMap> element
- oidMap element
ms.assetid: 7f0c2246-c070-4748-b96a-2f66a296c539
author: mcleblanc
ms.author: markl
manager: markl
ms.openlocfilehash: db39d7de3566647b5171b71940c78a9a0ab6f5f1
ms.sourcegitcommit: 3d5d33f384eeba41b2dff79d096f47ccc8d8f03d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/04/2018
ms.locfileid: "33350223"
---
# <a name="ltoidmapgt-element"></a>&lt;oidMap&gt;要素
クラスに ASN.1 オブジェクト識別子 (OID) のマッピングが含まれています。  
  
 \<configuration>  
\<mscorlib >  
\<cryptographySettings >  
\<oidMap >  
  
## <a name="syntax"></a>構文  
  
```xml  
<oidMap>   
</oidMap>  
```  
  
## <a name="attributes-and-elements"></a>属性および要素  
 以降のセクションでは、属性、子要素、および親要素について説明します。  
  
### <a name="attributes"></a>属性  
 なし。  
  
### <a name="child-elements"></a>子要素  
  
|要素|説明|  
|-------------|-----------------|  
|[\<oidEntry >](../../../../../docs/framework/configure-apps/file-schema/cryptography/oidentry-element.md)|ASN.1 OID をフレンドリ名にマップします。|  
  
### <a name="parent-elements"></a>親要素  
  
|要素|説明|  
|-------------|-----------------|  
|`configuration`|共通言語ランタイムおよび .NET Framework アプリケーションで使用されるすべての構成ファイルのルート要素です。|  
|`cryptographySettings`|暗号設定を含みます。|  
|`mscorlib`|含まれています、`cryptographySettings`要素。|  
  
## <a name="example"></a>例  
 次の例を使用する方法を示しています、  **\<oidMap >** 要素をそのハッシュ アルゴリズムの実装に ripemd-160 ハッシュ アルゴリズムの OID のマッピングが含まれます。  
  
```xml  
<configuration>  
   <mscorlib>  
      <cryptographySettings>  
         <cryptoNameMapping>  
            <cryptoClasses>  
               <cryptoClass   MyCrypto="MyCryptoClass, MyAssembly  
                  Culture=neutral, PublicKeyToken=a5d015c7d5a0b012,  
                  Version=1.0.0.0"/>  
            </cryptoClasses>  
            <nameEntry name="RIPEMD-160" class="MyCrypto"/>  
         </cryptoNameMapping>  
         <oidMap>  
            <oidEntry OID="1.3.36.3.2.1"  name="MyCryptoClass"/>  
         </oidMap>  
      </cryptographySettings>  
   </mscorlib>  
</configuration>  
```  
  
## <a name="see-also"></a>関連項目  
 [構成ファイル スキーマ](../../../../../docs/framework/configure-apps/file-schema/index.md)  
 [暗号化設定スキーマ](../../../../../docs/framework/configure-apps/file-schema/cryptography/index.md)  
 [暗号サービス](../../../../../docs/standard/security/cryptographic-services.md)  
 [暗号化クラスの設定](../../../../../docs/framework/configure-apps/configure-cryptography-classes.md)  
 [暗号化アルゴリズムへのオブジェクト ID の割り当て](../../../../../docs/framework/configure-apps/map-object-identifiers-to-cryptography-algorithms.md)
