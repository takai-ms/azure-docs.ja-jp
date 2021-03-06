---
title: Cache ASP.NET 出力キャッシュ プロバイダー
description: Azure Redis Cache を使用して ASP.NET ページ出力をキャッシュする方法について説明します。
services: redis-cache
documentationcenter: na
author: steved0x
manager: douge
editor: tysonn

ms.service: cache
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: cache-redis
ms.workload: tbd
ms.date: 09/27/2016
ms.author: sdanie

---
# <a name="asp.net-output-cache-provider-for-azure-redis-cache"></a>Azure Redis Cache の ASP.NET 出力キャッシュ プロバイダー
Redis 出力キャッシュ プロバイダーは、出力キャッシュ データ用のプロセス外ストレージ メカニズムです。 このデータは、完全な HTTP 応答専用です (ページ出力キャッシュ)。 プロバイダーは、ASP.NET 4 で導入された新しい出力キャッシュ プロバイダー拡張機能ポイントに接続します。

Redis 出力キャッシュ プロバイダーを使用するには、まず対象のキャッシュを構成し、Redis 出力キャッシュ プロバイダー NuGet パッケージを使用して ASP.NET アプリケーションを構成します。 このトピックでは、Redis 出力キャッシュ プロバイダーを使用するようにアプリケーションを構成する手順について説明します。 Azure Redis Cache インスタンスを作成および構成する方法の詳細については、 [キャッシュの作成](cache-dotnet-how-to-use-azure-redis-cache.md#create-a-cache)に関するページを参照してください。

## <a name="store-asp.net-page-output-in-the-cache"></a>ASP.NET ページ出力をキャッシュに格納する
Visual Studio で Redis 出力キャッシュ プロバイダー NuGet パッケージを使用してクライアント アプリケーションを構成するには、**ソリューション エクスプローラー**でプロジェクトを右クリックし、**[NuGet パッケージの管理]** をクリックします。

![Azure Redis Cache Manage NuGet パッケージ](./media/cache-aspnet-output-cache-provider/redis-cache-manage-nuget-menu.png)

検索ボックスに「**RedisOutputCacheProvider**」と入力し、結果の中からそのプロバイダーを選択して、**[インストール]** をクリックします。

![Azure Redis Cache 出力キャッシュプロバイダー](./media/cache-aspnet-output-cache-provider/redis-cache-page-output-provider.png)

Redis 出力キャッシュ プロバイダー NuGet パッケージは、StackExchange.Redis.StrongName パッケージに依存します。 StackExchange.Redis.StrongName パッケージは、プロジェクト内に存在しなければインストールされます。 厳密な名前を持つ StackExchange.Redis.StrongName パッケージ以外に、厳密な名前を持たない StackExchange.Redis バージョンもあります。 厳密な名前を持たない StackExchange.Redis バージョンをプロジェクトで使用している場合は、Redis 出力キャッシュ プロバイダー NuGet パッケージのインストールの前か後にこのバージョンをアンインストールする必要があります。アンインストールしなかった場合は、名前の競合がプロジェクトで発生します。 これらのパッケージの詳細については、[.NET キャッシュ クライアントの構成](cache-dotnet-how-to-use-azure-redis-cache.md#configure-the-cache-clients)に関するトピックを参照してください。

NuGet パッケージによって、必要なアセンブリ参照がダウンロードされて追加されます。さらに、web.config ファイルには、ASP.NET アプリケーションが Redis 出力キャッシュ プロバイダーを使用するために必要な構成を記述した次のセクションが追加されます。

    <caching>
      <outputCachedefault Provider="MyRedisOutputCache">
        <providers>
          <!--
          <add name="MyRedisOutputCache"
            host = "127.0.0.1" [String]
            port = "" [number]
            accessKey = "" [String]
            ssl = "false" [true|false]
            databaseId = "0" [number]
            applicationName = "" [String]
            connectionTimeoutInMilliseconds = "5000" [number]
            operationTimeoutInMilliseconds = "5000" [number]
          />
          -->
          <add name="MyRedisOutputCache" type="Microsoft.Web.Redis.RedisOutputCacheProvider" host="127.0.0.1" accessKey="" ssl="false"/>
        </providers>
      </outputCache>
    </caching>

コメント化されたセクションには、属性の例と各属性のサンプル設定が記述されています。

属性の構成には、Microsoft Azure ポータルのキャッシュ ブレードの値を使用してください。その他の値は適宜構成します。 キャッシュ プロパティにアクセスする方法については、[Redis キャッシュ設定の構成](cache-configure.md#configure-redis-cache-settings)に関するトピックを参照してください。

* **host** : キャッシュ エンドポイントを指定します。
* **port** : SSL の設定に応じて、非 SSL ポートまたは SSL ポートを使用します。
* **accessKey** : キャッシュのプライマリ キーまたはセカンダリ キーを使用します。
* **ssl** : キャッシュとクライアント間の通信を SSL で保護する場合は true、保護しない場合は false を指定します。 必ず適切なポートを指定してください。
  * 既定では、新しいキャッシュに対して非 SSL ポートは無効になっています。 SSL ポートを使用するには、この設定に true を指定します。 非 SSL ポートの有効化の詳細については、[キャッシュの構成](cache-configure.md)に関するトピックの「[アクセス ポート](cache-configure.md#access-ports)」セクションを参照してください。
* **databaseId** : キャッシュ出力データに使用するデータベースを指定します。 指定されていない場合は、既定値の 0 が使用されます。
* **applicationName**: キーが <AppName>_<SessionId>_Data として Redis に保存されます。 これによって、複数のアプリケーションで同じキーを共有できます。 このパラメーターは省略可能です。指定されていない場合は、既定値が使用されます。
* **connectionTimeoutInMilliseconds** : この設定によって、StackExchange.Redis クライアントの connectTimeout 設定を上書きすることができます。 指定されていない場合は、connectTimeout 設定の既定値である 5000 が使用されます。 詳細については、 [StackExchange.Redis 構成モデル](http://go.microsoft.com/fwlink/?LinkId=398705)に関するページを参照してください。
* **operationTimeoutInMilliseconds** : この設定によって、StackExchange.Redis クライアントの syncTimeout 設定を上書きすることができます。 指定されていない場合は、syncTimeout 設定の既定値である 1000 が使用されます。 詳細については、 [StackExchange.Redis 構成モデル](http://go.microsoft.com/fwlink/?LinkId=398705)に関するページを参照してください。

出力をキャッシュする各ページに、OutputCache ディレクティブを追加します。

    <%@ OutputCache Duration="60" VaryByParam="*" %>

この例では、キャッシュされたページ データが 60 秒間キャッシュに保持され、パラメーターの組み合わせごとに異なるバージョンのページがキャッシュされます。 OutputCache ディレクティブの詳細については、「 [@OutputCache](http://go.microsoft.com/fwlink/?linkid=320837)に関するページを参照してください。

ここまでの手順を実行すると、アプリケーションは Redis 出力キャッシュ プロバイダーを使用するように構成されます。

## <a name="next-steps"></a>次のステップ
「 [Azure Redis Cache の ASP.NET セッション状態プロバイダー](cache-aspnet-session-state-provider.md)」をご覧ください。

<!--HONumber=Oct16_HO2-->


