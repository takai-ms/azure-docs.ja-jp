---
title: Xamarin.Android アプリへのプッシュ通知の追加 | Microsoft Docs
description: Azure App Service と Azure Notification Hubs を使用して、Xamarin.Android アプリにプッシュ通知を送信する方法について説明します。
services: app-service\mobile
documentationcenter: xamarin
author: ggailey777
manager: erikre
editor: ''

ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-android
ms.devlang: dotnet
ms.topic: article
ms.date: 08/12/2016
ms.author: glenga

---
# Xamarin.Android アプリへのプッシュ通知の追加
[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## Overview
このチュートリアルでは、「[Xamarin.Android アプリの作成]」プロジェクトにプッシュ通知を追加して、レコードが挿入されるたびにプッシュ通知が送信されるようにします。このチュートリアルは、「[Xamarin.Android アプリの作成]」チュートリアルに基づいているので、先にそちらを完了する必要があります。ダウンロードしたクイック スタートのサーバー プロジェクトを使用しない場合は、プッシュ通知拡張機能パッケージをプロジェクトに追加する必要があります。サーバーの拡張機能パッケージの詳細については、「[Work with the .NET backend server SDK for Azure Mobile Apps (Azure Mobile Apps 用の .NET バックエンド サーバー SDK を操作する)](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md)」を参照してください。

## 前提条件
このチュートリアルには、次のものが必要です。

* アクティブな Google アカウント。[accounts.google.com](http://go.microsoft.com/fwlink/p/?LinkId=268302) で、Google アカウントにサインアップできます。
* [Google Cloud Messaging のクライアント コンポーネント](http://components.xamarin.com/view/GCMClient/)。このコンポーネントは、チュートリアル中に追加します。

## <a name="create-hub"></a>通知ハブを作成する
[!INCLUDE [app-service-mobile-create-notification-hub](../../includes/app-service-mobile-create-notification-hub.md)]

## <a id="register"></a>Google Cloud Messaging を有効にする
[!INCLUDE [GCM を有効にする](../../includes/mobile-services-enable-google-cloud-messaging.md)]

## プッシュ要求を送信するように Mobile App バックエンドを構成する
[!INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push.md)]

## <a id="update-server"></a>サーバー プロジェクトをプッシュ通知を送信するように更新する
[!INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]

## <a id="configure-app"></a>プッシュ通知のクライアント プロジェクトを構成する
[!INCLUDE [mobile-services-xamarin-android-push-configure-project](../../includes/mobile-services-xamarin-android-push-configure-project.md)]

## <a id="add-push"></a>アプリケーションにプッシュ通知コードを追加する
[!INCLUDE [app-service-mobile-xamarin-android-push-add-to-app](../../includes/app-service-mobile-xamarin-android-push-add-to-app.md)]

## <a name="test"></a>アプリケーションでプッシュ通知をテストする
エミュレーターで仮想デバイスを使用して、アプリをテストできます。エミュレーターで実行するときに必要な追加の構成手順があります。

1. 次に示すように Android Virtual Device (AVD) Manager で Google API がターゲットとして設定された仮想デバイスに対してデプロイまたはデバッグする必要があります。
   
    ![](./media/app-service-mobile-xamarin-android-get-started-push/google-apis-avd-settings.png)
2. **[Apps]**、**[Settings]**、**[Add account]** の順にクリックして、Google アカウントを Android デバイスに追加し、プロンプトに従って既存の Google アカウントをデバイスに追加し、新しいアカウントを作成します。
   
    ![](./media/app-service-mobile-xamarin-android-get-started-push/add-google-account.png)
3. これまでと同様に ToDoList アプリを実行し、新しい ToDo 項目を挿入します。今回は、通知領域に通知アイコンが表示されます。通知ドロアーを開いて、通知の全文を表示できます。
   
    ![](./media/app-service-mobile-xamarin-android-get-started-push/android-notifications.png)

<!-- URLs. -->
[Xamarin.Android quickstart]: app-service-mobile-xamarin-android-get-started.md
[Xamarin.Android アプリの作成]: app-service-mobile-xamarin-android-get-started.md
[Google Cloud Messaging Client Component]: http://components.xamarin.com/view/GCMClient/
[Azure Mobile Services Component]: http://components.xamarin.com/view/azure-mobile-services/

<!---HONumber=AcomDC_0817_2016-->