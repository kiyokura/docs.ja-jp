---
title: '方法 : WCF アクティブ化コンポーネントをインストールして設定する'
ms.date: 03/30/2017
helpviewer_keywords:
- HTTP activation [WCF]
ms.assetid: 33a7054a-73ec-464d-83e5-b203aeded658
ms.openlocfilehash: f362bd1e4a644488e85cdeca674d46ca340bde05
ms.sourcegitcommit: 3d5d33f384eeba41b2dff79d096f47ccc8d8f03d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/04/2018
ms.locfileid: "33491749"
---
# <a name="how-to-install-and-configure-wcf-activation-components"></a>方法 : WCF アクティブ化コンポーネントをインストールして設定する
このトピックでは、Windows プロセス アクティブ化サービス (WAS とも呼ばれます) を設定するために必要な手順を説明で[!INCLUDE[wv](../../../../includes/wv-md.md)]HTTP 経由で通信を行わないサービスのネットワーク プロトコルの Windows Communication Foundation (WCF) をホストします。 以降の各セクションで、この構成に関する手順について概説します。  
  
-   インストール (またはのインストールの確認) WCF アクティブ化コンポーネントです。  
  
-   非 HTTP プロトコルをサポートようにする WAS を構成します。 次の手順では、TCP アクティベーション用に [!INCLUDE[wv](../../../../includes/wv-md.md)] を構成します。  
  
 インストールと構成が、確認後、[する方法: WAS で WCF サービスをホスト](../../../../docs/framework/wcf/feature-details/how-to-host-a-wcf-service-in-was.md)WAS を使用する非 HTTP エンドポイントを公開する WCF サービスを作成する手順についてはします。  
  
### <a name="to-install-the-wcf-non-http-activation-components"></a>WCF 非 HTTP アクティブ化コンポーネントをインストールするには  
  
1.  クリックして、**開始**ボタンをクリックし、をクリックして**コントロール パネルの **です。  
  
2.  をクリックして**プログラム**、クリックして**プログラムと機能**します。  
  
3.  **タスク** メニューのをクリックして**Windows の機能のオンまたはオフ**です。  
  
4.  [!INCLUDE[vstecwinfx](../../../../includes/vstecwinfx-md.md)] ノードを検索し、それを選択して展開します。  
  
5.  選択、 **WCF 非 Http アクティブ化コンポーネント**ボックスし、設定を保存します。  
  
### <a name="to-configure-the-was-to-support-tcp-activation"></a>TCP アクティベーションをサポートするように WAS を構成するには  
  
1.  net.tcp アクティベーションをサポートするには、既定の Web サイトをあらかじめ net.tcp ポートにバインドしておく必要があります。 これは、[!INCLUDE[iisver](../../../../includes/iisver-md.md)] 管理ツール セットと共にインストールされる Appcmd.exe を使用して行います。 管理者レベルのコマンド プロンプト ウィンドウで、次のコマンドを実行します。  
  
    ```  
    %windir%\system32\inetsrv\appcmd.exe set site "Default Web Site" -+bindings.[protocol='net.tcp',bindingInformation='808:*']  
    ```  
  
    > [!NOTE]
    >  このコマンドはテキスト 1 行です。 このコマンドは、net.tcp サイト バインディングを、TCP ポート 808 で任意のホスト名をリッスンする既定の Web サイトに追加します。  
  
2.  サイト内のすべてのアプリケーションが同じ net.tcp バインディングを共有しますが、net.tcp サポートの有効化はアプリケーションごとに指定できます。 アプリケーションで net.tcp を有効にするには、管理者レベルのコマンド プロンプトから、次のコマンドを実行します。  
  
    ```  
    %windir%\system32\inetsrv\appcmd.exe set app   
    "Default Web Site/<WCF Application>" /enabledProtocols:http,net.tcp  
    ```  
  
    > [!NOTE]
    >  このコマンドはテキスト 1 行です。 このコマンドにより、/\<*WCF アプリケーション*> アプリケーションへの両方を使用してアクセスhttp://localhost  */ \<WCF アプリケーション >* と net.tcp://localhost/*\<WCF アプリケーション >* です。  
  
     このサンプル用に追加した net.tcp サイト バインディングを削除します。  
  
     便宜上次の 2 つの手順が、サンプル ディレクトリにある RemoveNetTcpSiteBinding.cmd というバッチ ファイルに実装されています。  
  
    1.  管理者レベルのコマンド プロンプト ウィンドウで次のコマンドを実行して、有効なプロトコルの一覧から net.tcp を削除します。  
  
        ```  
        %windir%\system32\inetsrv\appcmd.exe set app   
        "Default Web Site/servicemodelsamples<WCF Application>" " /enabledProtocols:http  
        ```  
  
        > [!NOTE]
        >  このコマンドはテキスト 1 行です。  
  
    2.  権限のレベルが高いコマンド プロンプト ウィンドウで次のコマンドを実行して、net.tcp サイト バインディングを削除します。  
  
        ```  
        %windir%\system32\inetsrv\appcmd.exe set site "Default Web Site"   
        --bindings.[protocol='net.tcp',bindingInformation='808:*']  
        ```  
  
        > [!NOTE]
        >  このコマンドはテキスト 1 行です。  
  
### <a name="to-remove-nettcp-from-the-list-of-enabled-protocols"></a>有効なプロトコルの一覧から net.tcp を削除するには  
  
1.  有効なプロトコルの一覧から net.tcp を削除するには、管理者レベルのコマンド プロンプト ウィンドウで次のコマンドを実行します。  
  
    ```  
    %windir%\system32\inetsrv\appcmd.exe set app "Default Web Site/servicemodelsamples<WCF Application>" " /enabledProtocols:http  
    ```  
  
    > [!NOTE]
    >  このコマンドはテキスト 1 行です。  
  
### <a name="to-remove-the-nettcp-site-binding"></a>net.tcp サイト バインディングを削除するには  
  
1.  net.tcp サイト バインディングを削除するには、管理者レベルのコマンド プロンプト ウィンドウで次のコマンドを実行します。  
  
    ```  
    %windir%\system32\inetsrv\appcmd.exe set site "Default Web Site"   
    -bindings.[protocol='net.tcp',bindingInformation='808:*']  
    ```  
  
    > [!NOTE]
    >  このコマンドはテキスト 1 行です。  
  
## <a name="see-also"></a>関連項目  
 [TCP アクティベーション](../../../../docs/framework/wcf/samples/tcp-activation.md)  
 [MSMQ アクティベーション](../../../../docs/framework/wcf/samples/msmq-activation.md)  
 [NamedPipe アクティベーション](../../../../docs/framework/wcf/samples/namedpipe-activation.md)  
 [Windows Server App Fabric のホスティング機能](http://go.microsoft.com/fwlink/?LinkId=201276)
