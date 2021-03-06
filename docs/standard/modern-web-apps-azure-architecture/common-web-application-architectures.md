---
title: 一般的な Web アプリケーション アーキテクチャ
description: ASP.NET Core および Microsoft Azure での最新の Web アプリケーションの設計 | 一般的な Web アプリケーション アーキテクチャ
author: ardalis
ms.author: wiwagn
ms.date: 10/06/2017
ms.openlocfilehash: 943163ca4c82ad75f177ebe73559d909e7292c52
ms.sourcegitcommit: 3d5d33f384eeba41b2dff79d096f47ccc8d8f03d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/04/2018
ms.locfileid: "33592497"
---
# <a name="common-web-application-architectures"></a>一般的な Web アプリケーション アーキテクチャ

> "優れたアーキテクチャが高価であると思うならば、不完全なアーキテクチャを試してみてください。"  
> _- Brian Foote および Joseph Yoder_

## <a name="summary"></a>まとめ

従来の .NET アプリケーションのほとんどは、実行可能ファイルに対応する単一のユニットとして、または 1 つの IIS appdomain 内で実行される単一の Web アプリケーションとして展開されます。 これは最も簡単な展開モデルであり、多くの内部的アプリケーションおよび小さなパブリック アプリケーションに適切に対応します。 ただし、このように単位ユニットとして展開されても、重要なビジネス アプリケーションの大部分は複数のレイヤーへの論理的な分離から恩恵を受けます。

## <a name="what-is-a-monolithic-application"></a>モノリシック アプリケーションとは?

モノリシック アプリケーションは、そのビヘイビアーの観点から見ると、完全な自己独立型のアプリケーションです。 このアプリケーションは、操作の実行中に他のサービスまたはデータ ストアとやり取りすることがありますが、アプリケーションのビヘイビアーの中核を成す部分は独自のプロセス内で実行され、通常はアプリケーション全体が単一ユニットとして展開されます。 このようなアプリケーションを水平方向にスケーリングする必要がある場合、通常は、アプリケーション全体を複数のサーバーまたは仮想マシンに複製します。

## <a name="all-in-one-applications"></a>オールインワン アプリケーション

アプリケーション アーキテクチャ用の最小限のプロジェクト数は 1 となります。 このアーキテクチャでは、アプリケーションのロジック全体が単一のプロジェクトに含められ、単一のアセンブリにコンパイルされ、単一ユニットとして展開されます。

新しい ASP.NET Core プロジェクトは、Visual Studio またはコマンド ラインのいずれで作成されたかに関係なく、簡単な "オールインワン" モノシリックとして開始されます。 これには、プレゼンテーション、ビジネス、データ アクセス ロジックなど、アプリケーションのあらゆるビヘイビアーが含まれています。 図 5-1 に、単一プロジェクト アプリのファイル構造を示します。

**図 5-1** 単一プロジェクトの ASP.NET Core アプリ

![](./media/image5-1.png)

単一プロジェクトのシナリオでは、フォルダーを使用して懸念事項の分離が実現されます。 既定のテンプレートには、Models、Views、および Controllers の MVC パターン責任用の分離フォルダーに加えて、Data および Services 用の追加のフォルダーが含まれています。 この配置では、プレゼンテーションの詳細は可能な限り Views フォルダーに限定する必要があり、データ アクセス実装の詳細は Data フォルダー内に保持されるクラスに限定する必要があります。 ビジネス ロジックは、Models フォルダーにあるサービスとモデル内に配置する必要があります。

単一プロジェクトのモノリシック ソリューションは簡単なのですが、いくつかの欠点があります。 プロジェクトの規模と複雑さが増大するに従い、ファイルとフォルダーの数も増加し続けます。 アルファベット順にグループ化されていない複数のフォルダーには UI の懸念事項が存在します (Models、Views、Controllers)。 この問題は、Filters や ModelBinders などの UI レベル構造が独自のフォルダーに追加される場合にのみ深刻になります。 ビジネス ロジックは Models フォルダーと Services フォルダーに分散され、どのフォルダーのどのクラスが他のどれに依存すべきかは明示されません。 プロジェクト レベルでのこの編成不足は、[スパゲティ コード](http://deviq.com/spaghetti-code/)の原因となることがよくあります。

これらの問題に対処するために、アプリケーションは多くの場合、マルチ プロジェクト ソリューションに展開されます。ここで、各プロジェクトは、アプリケーションの特定の "*レイヤー*" に配置されているとみなされます。

## <a name="what-are-layers"></a>レイヤーとは?

アプリケーションの複雑さが増すにつれて、その複雑さを管理することが必要です。その方法の 1 つが、アプリケーションをその責任や懸念事項に従って分割するというものです。 この方法は懸念事項の分離の原則に従って実施されます。また、この方法を使用すれば、特定の機能が実装されている場所を開発者が容易に見つけられるように、増大するコードベースを常に整理された状態に維持することができます。 ただし、レイヤー化されたアーキテクチャには、単なるコード編成に勝る利点が複数あります。

コードをレイヤーに編成することにより、よく使われる下位機能をアプリケーション全体で再利用できるようになります。 この再利用を行うと、コードをほとんど記述する必要がなくなり、DRY 原則に従って単一の実装でアプリケーションを標準化できるようになるので、便利です。

レイヤー化されたアーキテクチャの場合、アプリケーションではレイヤー間のやり取りに対して制限を加えることができます。 この制限を利用することで、カプセル化を容易に実現できます。 レイヤーを変更または置換した場合、そのレイヤーと連携しているその他のレイヤーのみが影響を受けます。 レイヤー同士の依存関係を制限することにより、1 つの変更がアプリケーション全体に影響しないように変更の及ぼす影響を軽減することができます。

レイヤー (およびカプセル化) により、アプリケーション内で機能を容易に置換できるようになります。 たとえば、アプリケーションで最初は永続化のために独自の SQL Server データベースが使用されている場合でも、後でクラウド ベースの永続化の方法または Web API の背後での永続化の方法を使用するように選択することが可能です。 論理層内でアプリケーションによって永続化の実装が適切にカプセル化されている場合、その SQL Server に固有のレイヤーを、同じパブリック インターフェイスを実装する新しいレイヤーに置き換えることが可能です。

将来的な要件の変更に合わせて実装を交換できることに加えて、アプリケーション レイヤーではテスト目的で実装を容易に交換することもできます。 アプリケーションの実際のデータ レイヤーまたは UI レイヤーに対して動作するテストを記述しなくても、これらのレイヤーをテスト時に、要求に対して既知の応答を提供するフェイク実装に置き換えることができます。 この方法は、アプリケーションの実際のインフラストラクチャに対してテストを実行する場合と比較して、一般的にテストの記述が簡単であり、テストをより迅速に実行することができます。

論理的なレイヤー化は、エンタープライズ ソフトウェア アプリケーションでのコード編成を改善するための一般的な手法です。コードをレイヤーに編成する方法には複数あります。

> [!NOTE]
> "*レイヤー*" とは、アプリケーション内での論理的な分離を表します。 アプリケーション ロジックを個々のサーバーまたはプロセスに物理的に分散するイベントでは、このような個々の物理的展開ターゲットを "*階層*" と呼んでいます。 N レイヤー アプリケーションを単一の階層に展開することは可能であり、これは非常に一般的な方法です。

## <a name="traditional-n-layer-architecture-applications"></a>従来の "N レイヤー" アーキテクチャ アプリケーション

図 5-2 に、レイヤーへのアプリケーション ロジックの最も一般的な編成を示します。

**図 5-2** 代表的なアプリケーション レイヤー。

![](./media/image5-2.png)

これらのレイヤーは多くの場合、UI、BLL (ビジネス ロジック層)、DAL (データ アクセス層) と略称で表現されます。 このアーキテクチャを使用する場合、ユーザーは UI レイヤーを介して要求を行います。UI レイヤーは BLL とのみやり取りします。 次に BLL は、データ アクセス要求のために DAL を呼び出すことができます。 UI レイヤーは DAL に要求を直接出すことも、他の手段によって永続化と直接やり取りすることもありません。 同様に、BLL は DAL を経由して永続化とやり取りするだけです。 この方法では、各レイヤーには独自の既知の責任があります。

この従来のレイヤー化アプローチの欠点の 1 つは、コンパイル時の依存関係が上位から下位に向かって適用されるということです。 つまり、UI レイヤーは BLL に依存し、BLL は DAL に依存するということです。 このことは、アプリケーション内で、通常、最も重要なロジックを保持する BLL が、データ アクセスの実装の詳細 (およびしばしばデータベースの存在) に依存することを意味します。 このようなアーキテクチャでビジネス ロジックをテストするのは困難なことが多く、テスト データベースが必要となります。 次のセクションで示すように、依存関係逆転の原則 (Dependency Inversion principle) を使用すれば、この問題に対処できます。

図 5-3 に、責任 (またはレイヤー) ごとに 3 つのプロジェクトにアプリケーションを分割するソリューションの例を示します。

**図 5-3** 3 つのプロジェクトによる単純なモノリシック アプリケーション。

![](./media/image5-3.png)

このアプリケーションでは編成上の目的で複数のプロジェクトを使用していますが、アプリケーションは引き続き単一ユニットとして展開され、そのクライアントは単一の Web アプリとしてこのアプリケーションとやり取りします。 これにより、展開プロセスが非常に簡単になります。 図 5-4 に、そのようなアプリを Windows Azure を使用してホストする場合の方法を示します。

![](./media/image5-4.png)

**図 5-4** Azure Web アプリの単純な展開

アプリケーション ニーズの拡大に合わせて、より複雑で堅牢な展開ソリューションが必要な場合があります。 図 5-5 に、追加の機能をサポートするより複雑な展開計画の例を示します。

![](./media/image5-5.png)

**図 5-5** Azure App Service への Web アプリの公開

内部的には、このプロジェクトを責任に基づいて複数のプロジェクトに編成することにより、アプリケーションの保守容易性が向上します。

このユニットをスケール アップまたはスケール アウトすることで、クラウド ベースのオンデマンドのスケーラビリティを活用することができます。 スケール アップとは、アプリをホストしているサーバーに CPU、メモリ、ディスク領域などのリソースを追加することを意味します。 スケール アウトとは、そのようなサーバーが物理サーバーか仮想マシンかに関係なく、サーバーのインスタンスを追加することを意味します。 アプリが複数のインスタンスでホストされている場合は、ロード バランサーを使用して個々のアプリ インスタンスに要求が割り当てられます。

Azure 内で Web アプリケーションをスケーリングする最も簡単な方法は、アプリケーションの App Service プランでスケーリングを手動で構成するというものです。 図 5-6 に、アプリを提供しているインスタンスの数を構成する適切な Azure ダッシュボード画面を示します。

![](./media/image5-6.png)

**図 5-6** App での App Service プランによるスケーリング。

## <a name="clean-architecture"></a>クリーン アーキテクチャ

依存関係逆転の原則ならびにドメイン駆動設計 (DDD) の原則に従うアプリケーションは、同様のアーキテクチャに到達する傾向があります。 このアーキテクチャには長年にわたってさまざまな名称が付けられてきました。 最初の名前の 1 つがヘキサゴナル アーキテクチャ (Hexagonal Architecture) でした。その後に使用された名前がポート アンド アダプター (Ports-and-Adapters) でした。 最近では、このアーキテクチャは[オニオン アーキテクチャ](http://jeffreypalermo.com/blog/the-onion-architecture-part-1/)または[クリーン アーキテクチャ](https://8thlight.com/blog/uncle-bob/2012/08/13/the-clean-architecture.html)として引用されています。 最も新しい名前が "クリーン アーキテクチャ" です。この電子ブックでは、アーキテクチャの説明で基本的にこの名前が使用されています。

> [!NOTE]
> クリーン アーキテクチャという用語は、DDD 原則を使用して構築されたアプリケーションにも、DDD 原則を使用して構築されていないアプリケーションにも適用できます。 前者の場合、この組み合わせを "クリーン DDD アーキテクチャ" と呼ぶことがあります。

クリーン アーキテクチャでは、ビジネス ロジックとアプリケーション モデルをアプリケーションの中心に配置します。 ビジネス ロジックはデータ アクセスまたはその他のインフラストラクチャの懸念事項に依存するのでなく、この依存関係は逆転されます。つまり、インフラストラクチャおよび実装の詳細はアプリケーション コアに依存します。 これを達成するには、アプリケーション コア内で抽象化またはインターフェイスを定義し、それらをインフラストラクチャ レイヤーで定義された型によって実装します。 このアーキテクチャを視覚化するための一般的な方法としては、オニオンに似た一連の同心円を使用します。 図 5-X に、このスタイルのアーキテクチャを表現した例を示します。

![](./media/image5-7.png)

**図 5-7** クリーン アーキテクチャ。オニオン ビュー

この図では、依存関係が最も内側の円に向かっています。 したがって、アプリケーション コア (この名前は、この図の中心に位置するところから取ったもの) は他のアプリケーション レイヤーに依存していないことがわかります。 真中には、アプリケーションのエンティティとのインターフェイスがあります。 そのすぐ外側 (まだアプリケーション コア内) は、ドメイン サービスです。ここでは通常、内側の円で定義されたインターフェイスを実装します。 アプリケーション コアの外側にはユーザー インターフェイス レイヤーとインフラストラクチャ レイヤーがあります。この 2 つはアプリケーション コアに依存しますが、相互に依存関係があるとは限りません。

図 5-X に、UI とその他のレイヤーの間の依存関係をより正確に反映させた、従来の水平方向レイヤー図を示します。

![](./media/image5-8.png)

**図 5-8** クリーン アーキテクチャ。水平方向のレイヤー ビュー

破線の矢印はコンパイル時の依存関係を表し、実線の矢印は実行時専用の依存関係を表します。 クリーン アーキテクチャを使用する場合、UI レイヤーではコンパイル時にアプリケーション コアで定義されたインターフェイスを操作します。インフラストラクチャ レイヤーで定義された実装型を把握しなくてもよいのが理想的です。 ただし、実行時にこれらの実装型は、アプリを実行するために必須となるので、存在する必要があり、また依存関係挿入を介してアプリケーション コアのインターフェイスに接続される必要があります。

図 5-9 に、これらの推奨事項に従って ASP.NET Core アプリケーションのアーキテクチャを構築した場合の詳細なビューを示します。

![ASPNET コア アーキテクチャ](./media/image5-9.png)

**図 5-9** クリーン アーキテクチャに基づく ASP.NET Core アーキテクチャの図。

アプリケーション コアはインフラストラクチャに依存していないので、このレイヤーを対象にした自動化された単体テストを記述するのは非常に簡単です。 図 5-10 と図 5-11 に、このアーキテクチャにテストがどのように適合するのかを示します。

![UnitTestCore](./media/image5-10.png)

**図 5-10** 分離環境でのアプリケーション コアの単体テスト。

![IntegrationTests](./media/image5-11.png)

**図 5-11** 外部依存関係を持つインフラストラクチャ実装の統合テスト。

UI レイヤーはインフラストラクチャ プロジェクトで定義された型に対して直接的な依存関係を持たないことから、テストの促進、またはアプリケーション要件の変更への対応を目的として実装を交換することも容易にできます。 ASP.NET Core には依存関係挿入の使用とサポートが組み込まれているので、このアーキテクチャは重要なモノリシック アプリケーションを構築する方法として最適です。

モノリシック アプリケーションの場合、アプリケーション コア、インフラストラクチャ、ユーザー インターフェイスの各プロジェクトはいずれも単一のアプリケーションとして実行されます。 ランタイム アプリケーションのアーキテクチャは、図 5-12 のようなものになります。

![ASPNET コア アーキテクチャ 2](./media/image5-12.png)

**図 5-12** ASP.NET Core アプリのランタイム アーキテクチャの例。

### <a name="organizing-code-in-clean-architecture"></a>クリーン アーキテクチャ内でのコードの整理

クリーン アーキテクチャでは、各プロジェクトが明確な責任を担っています。 そのため、各プロジェクトにはそれぞれ特定の型が属しており、該当するプロジェクトではこれらの型に対応するフォルダーを頻繁に確認できます。

アプリケーション コアでは、エンティティ、サービス、およびインターフェイスが含まれるビジネス モデルを保持します。 これらのインターフェイスには、データ アクセス、ファイル システム アクセス、ネットワーク呼び出しなどのインフラストラクチャを使用して実行される操作のための抽象化が含まれます。このレイヤーで定義されたサービスまたはインターフェイスは場合によって、UI またはインフラストラクチャに対して依存関係を持たない非エンティティ型を操作する必要があります。 これらは単純なデータ転送オブジェクト (DTO) として定義することができます。

> ### <a name="application-core-types"></a>アプリケーション コアの種類
> -   エンティティ (永続化されたビジネス モデル クラス)
> -   インターフェイス
> -   サービス
> -   DTO

インフラストラクチャ プロジェクトには、通常、データ アクセス実装が含まれます。 代表的な ASP.NET Core Web アプリケーションでは、これにはエンティティ フレームワーク DbContext、定義済みの任意の EF コア移行、およびデータ アクセス実装クラスが含まれます。 データ アクセス実装コードを抽象化するには、[リポジトリ デザイン パターン](http://deviq.com/repository-pattern/)を使用するのが最も一般的な方法です。

データ アクセス実装に加えて、インフラストラクチャ プロジェクトにはインフラストラクチャの懸念事項とやり取りする必要があるサービスの実装を含める必要があります。 これらのサービスではアプリケーション コアで定義されているインターフェイスを実装する必要があります。そのため、インフラストラクチャにはアプリケーション コア プロジェクトへの参照を含める必要があります。

> ### <a name="infrastructure-types"></a>インフラストラクチャの種類
> -   EF コア型 (DbContext、移行)
> -   データ アクセス実装型 (リポジトリ)
> -   インフラストラクチャに固有のサービス (FileLogger、SmtpNotifier など)

ASP.NET Core MVC アプリケーション内のユーザー インターフェイス レイヤーは、アプリケーションのエントリ ポイントになると共に、ASP.NET Core MVC プロジェクトになります。 このプロジェクトはアプリケーション コア プロジェクトを参照する必要があり、その型はアプリケーション コアで定義されているインターフェイスを介してインフラストラクチャと厳密にやり取りする必要があります。 UI レイヤーにおいて、インフラストラクチャ レイヤー型の直接的なインスタンス化 (またはその静的呼び出し) は許可すべきではありません。

> ### <a name="ui-layer-types"></a>UI レイヤーの種類
> -   Controllers
> -   フィルター
> -   ビュー
> -   ViewModels
> -   スタートアップ

スタートアップ クラスはアプリケーションの構成と、インターフェイスへの実装型の接続とを担当します。これにより、依存関係挿入は実行時に適切に機能します。

> [!NOTE]
> UI プロジェクトの Startup.cs ファイル内の ConfigureServices で依存関係挿入を接続するために、プロジェクトはインフラストラクチャ プロジェクトを参照することが必要な場合があります。 この依存関係を排除するには、カスタム DI コンテナーを使用するのが最も簡単な方法です。 このサンプルの目的を考慮すると、UI プロジェクトでインフラストラクチャ プロジェクトを参照できるようにするのが最も簡単な方法です。

## <a name="monolithic-applications-and-containers"></a>モノリシック アプリケーションとコンテナー 

モノリシックに展開された単一の Web アプリケーションまたはサービスを構築し、それをコンテナーとして展開することができます。 アプリケーション内では、それはモノリシックとはならずにいくつかのライブラリ、コンポーネント、またはレイヤーに編成される場合があります。 外部的には、単一のプロセス、単一の Web アプリケーション、または単一のサービスのような単一のコンテナーです。

このモデルを管理するには、アプリケーションを表す単一のコンテナーを展開します。 ロード バランサーを前面に配置してコピーを追加するだけで、スケーリングできます。 単一のコンテナーまたは VM で単一の展開を管理することで、このように簡単になります。

![](./media/image5-13.png)

図 5-X に示すように、複数のコンポーネント/ライブラリ、または内部レイヤーを各コンテナーに含めることができます。 ただし、"*コンテナーは 1 つのことを実行し、それを 1 つのプロセスで実行する*" というコンテナーの原則に従えば、このモノリシック パターンは矛盾する可能性があります。

このアプローチの欠点は、アプリケーションが大きくなり、スケーリングする必要が出てきた場合に顕著になります。 アプリケーション全体がスケーリングすれば、実際には問題ではありません。 ただし、ほとんどの場合、スケーリングする必要があるネックはアプリケーションの一部であり、他のコンポーネントはそれほど使用されません。

一般的な E コマースを例にとると、スケーリングする必要が生じるのは多くの場合、製品情報のコンポーネントです。 製品を購入するユーザーよりも多くのユーザーが製品を参照します。 より多くの顧客が、支払いパイプラインではなくバスケットを使用します。 コメントを追加したり、購入履歴を表示したりする顧客はそれほどいません。 また、単一のリージョンで、コンテンツとマーケティング キャンペーンを管理する必要がある従業員数は多分、ほんの一握りです。 モノリシック デザインのスケーリングにより、コード全体は複数回展開されます。

ただし、すべてのコンポーネントをスケーリングする問題に加えて、単一のコンポーネントを変更するには、アプリケーション全体を完全に再テストし、すべてのインスタンスを完全に再展開する必要があります。

モノリシック アプローチは一般的であり、多くの組織が、このアーキテクチャ アプローチによって開発を行っています。 多くの組織が十分に良好な結果を収めている一方で、限界に達している組織もあります。 多くの組織は、このモデルでアプリケーションを設計していました。これは、ツールとインフラストラクチャのせいでサービス指向アーキテクチャ (SOA) の構築が非常に困難だったためと、アプリケーションが大きくなるまで SOA の必要性がわからなかったためです。 モノリシック アプローチの限界に達しているとわかった場合は、コンテナーとマイクロサービスを有効活用できるようにアプリに分割することを次の論理的な手順とすることができます。

![](./media/image5-14.png)

Microsoft Azure のモノリシック アプリケーションは、各インスタンスに専用の VM を使用して展開できます。 [Azure Virtual Machine Scale Sets](https://docs.microsoft.com/azure/virtual-machine-scale-sets/) を使用すると、VM のスケーリングを簡単に行うことができます。 [Azure App Service](https://azure.microsoft.com/services/app-service/) では、VM の管理を必要とせずに、モノリシック アプリケーションを実行し、インスタンスを簡単にスケーリングすることができます。 Azure App Services では、Docker コンテナーの単一インスタンスも実行できるため、展開が簡単になります。 Docker を使用すれば、1 つの VM を Docker ホストとして展開し、複数のインスタンスを実行できます。 図 5-14 に示すように、Azure バランサーを使用してスケーリングを管理できます。

さまざまなホストへの展開は、従来の展開手法で管理できます。 Docker ホストは、**docker run** などのコマンドを使用して手動で管理するか、継続的デリバリー (CD) パイプラインなどのオートメーションによって管理できます。

### <a name="monolithic-application-deployed-as-a-container"></a>コンテナーとして展開するモノリシック アプリケーション

コンテナーを使用してモノリシック アプリケーションの展開を管理することには利点があります。 コンテナーのインスタンスをスケーリングする処理は、追加の VM を展開するよりもはるかに高速で簡単です。 VM Scale Sets を使用して VM を拡張する場合も、インスタンス化には時間がかかります。 アプリのインスタンスとして展開する場合、アプリの構成は VM の一部として管理されます。

更新プログラムを Docker イメージとして展開する方がはるかに高速で、ネットワークの効率が高くなります。 通常、Docker イメージは秒単位で起動するので、ロールアウトが高速になります。 Docker インスタンスの破棄は、**docker stop** コマンドの発行と同じくらい簡単で、通常は 1 秒未満で完了します。

コンテナーは本質的に設計上、変更不可であるため、VM の破損について心配する必要はありません。一方、更新スクリプトではディスク上に残された特定の構成またはファイルが考慮されない場合があります。

モノシリック アプリが Docker から恩恵を受けることができる一方で、モノリシック アプリケーションを、スケーリング、開発、および展開を個別に実行できるサブシステムに分割することが、マイクロサービスの領域への入り口になる場合があります。

> ### <a name="references--common-web-architectures"></a>参照: 一般的な Web アーキテクチャ
> - **クリーン アーキテクチャ**  
> <https://8thlight.com/blog/uncle-bob/2012/08/13/the-clean-architecture.html>
> - **オニオン アーキテクチャ**  
> <http://jeffreypalermo.com/blog/the-onion-architecture-part-1/>
> - **リポジトリ パターン**  
> <http://deviq.com/repository-pattern/>
> - **クリーン アーキテクチャ ソリューションのサンプル**  
> <https://github.com/ardalis/cleanarchitecture>
> - **マイクロサービス電子書籍の設計** <http://aka.ms/MicroservicesEbook>

>[!div class="step-by-step"]
[前へ] (architectural-principles.md) [次へ] (common-client-side-web-technologies.md)
