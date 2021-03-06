---
title: 中間層クライアント アプリケーション
ms.date: 03/30/2017
ms.assetid: f9714a64-d0ae-4a98-bca0-5d370fdbd631
ms.openlocfilehash: 4cca832266b2eb2ab7b1b4eb1a5fe937525db97d
ms.sourcegitcommit: 3d5d33f384eeba41b2dff79d096f47ccc8d8f03d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/04/2018
ms.locfileid: "33493495"
---
# <a name="middle-tier-client-applications"></a>中間層クライアント アプリケーション
このトピックでは、Windows Communication Foundation (WCF) を使用する中間層クライアント アプリケーションに固有のさまざまな問題について説明します。  
  
## <a name="increasing-middle-tier-client-performance"></a>中間層クライアントのパフォーマンス向上  
 使用する Web サービスなどの従来の通信技術と比べ[!INCLUDE[vstecasp](../../../../includes/vstecasp-md.md)]、WCF クライアントのインスタンスの作成を WCF の豊富な機能セットが複雑になることができます。 たとえば、<xref:System.ServiceModel.ChannelFactory%601> オブジェクトを開いている場合、サービスとの間にセキュリティで保護されたセッションを確立できますが、そのぶんクライアント インスタンスの起動時間が長くなります。 通常、これらの追加機能は影響しませんクライアント アプリケーションが大幅にので、WCF クライアントはいくつかの呼び出しを実行してから閉じます。  
  
 中間層クライアント アプリケーションでは、ただし、多くの WCF クライアント オブジェクトをすばやく作成でき、その結果、増加の初期化要件が発生できます。 サービスを呼び出すときに中間層アプリケーションのパフォーマンスを向上させる方法は主に 2 つあります。  
  
-   WCF クライアント オブジェクトをキャッシュし、可能な限り、後続の呼び出しの再利用ができます。  
  
-   作成、<xref:System.ServiceModel.ChannelFactory%601>オブジェクトし、そのオブジェクトを使用して、新しい WCF クライアント呼び出しごとに、チャネル オブジェクトを作成します。  
  
 これらの方法を使用するときに検討すべきいくつかの問題があります。  
  
-   サービスがセッションを使用してクライアント固有の状態を保持している場合、中間層クライアントのサービスの状態が関連付けられているため複数クライアント層の要求と中間層の WCF クライアントを再ことはできません。  
  
-   場合は、サービスは、クライアントごとの認証を実行する必要があります、ために、中間層の WCF クライアント (または WCF クライアント チャネル オブジェクト) を再利用してではなく、中間層で受信要求ごとに新しいクライアントを作成する必要があります、中間層のクライアントの資格情報WCF クライアント後は変更できません (または<xref:System.ServiceModel.ChannelFactory%601>) が作成されました。  
  
-   チャネルにより作成されたチャネルとクライアントは、スレッド セーフですが複数のメッセージをネットワークに同時に書き込むことをサポートしていない場合があります。 大容量メッセージ (特にストリーミング) を送信すると、送信操作はブロックされ別の送信が完了するまで待機する場合があります。 チャネルを再利用するサービスに制御フローが戻った場合、これにより、同時性の欠如とデッドロックの可能性という 2 種類の問題が発生します (つまり、共有クライアントがサービスを呼び出すと、そのコード パスは結果的に共有クライアントにコールバックします)。 これは再利用する WCF クライアントの種類に関係なく当てはまります。  
  
-   チャネルを共有しているかどうかに関係なく、エラーの発生したチャネルを処理する必要があります。 ただし、チャネルを再利用するときに、エラーの発生しているチャネルは複数の保留中の要求を削除するか送信することができます。  
  
 複数の要求用のクライアントを再利用するためのベスト プラクティスを示す例を次を参照してください。 [ASP.NET クライアントでのデータ バインディング](../../../../docs/framework/wcf/samples/data-binding-in-an-aspnet-client.md)です。  
  
 さらに、<xref:System.Xml.Serialization.XmlSerializer> を使用してシリアル化できるデータ型を使用するクライアントの起動時のパフォーマンスを向上したり、実行時にこのようなデータ型のシリアル化コードを生成およびコンパイルしたりできます (この場合、起動時のパフォーマンスが低下することがあります)。 [ServiceModel メタデータ ユーティリティ ツール (Svcutil.exe)](../../../../docs/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe.md)アプリケーションのコンパイル済みアセンブリから、必要なシリアル化コードを生成することによってこれらのアプリケーションの起動時のパフォーマンスを向上させることができます。 詳細については、次を参照してください。[する方法: スタートアップ時間の WCF クライアント アプリケーション、XmlSerializer を使用してを向上させる](../../../../docs/framework/wcf/feature-details/startup-time-of-wcf-client-applications-using-the-xmlserializer.md)です。  
  
## <a name="see-also"></a>関連項目  
 [WCF クライアントを使用したサービスへのアクセス](../../../../docs/framework/wcf/feature-details/accessing-services-using-a-client.md)
