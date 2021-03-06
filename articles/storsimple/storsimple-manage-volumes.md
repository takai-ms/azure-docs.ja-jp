---
title: "StorSimple ボリュームの管理 | Microsoft Docs"
description: "StorSimple ボリュームを追加、変更、監視、削除する方法と、必要に応じて StorSimple ボリュームをオフラインにする方法について説明します。"
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: ccabd859-590c-4568-a81d-6ff38f125be3
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 05/11/2016
ms.author: v-sharos
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 2b79492914bb52e970343a2e829622122f03642b


---
# <a name="use-the-storsimple-manager-service-to-manage-volumes"></a>StorSimple Manager サービスを使用してボリュームを管理する
[!INCLUDE [storsimple-version-selector-manage-volumes](../../includes/storsimple-version-selector-manage-volumes.md)]

## <a name="overview"></a>概要
このチュートリアルでは、StorSimple Manager サービスを使用して、StorSimple デバイスおよび StorSimple 仮想デバイスにボリュームを作成して管理する方法について説明します。

StorSimple Manager サービスは、StorSimple ソリューションを単一の Web インターフェイスから管理できる、Azure クラシック ポータルの拡張機能です。 StorSimple Manager サービスを使用すると、ボリュームの管理に加え、StorSimple サービスの作成と管理、デバイスの表示と管理、アラートの表示、バックアップ ポリシーやバックアップ カタログの表示と管理を行うことができます。

> [!NOTE]
> Azure StorSimple は、仮想プロビジョニングされたボリュームのみを作成できます。 完全にプロビジョニングされたボリュームまたは部分的にプロビジョニングされたボリュームを Azure StorSimple システム上に作成することはできません。
> 
> 仮想プロビジョニングは、物理リソースを超える空き容量がストレージにあるように見える仮想化テクノロジです。 Azure StorSimple では、事前に十分なストレージを確保するのではなく、仮想プロビジョニングを使用して、現在の要件を満たすために必要な容量のみを割り当てます。 Azure StorSimple では変化する需要に合わせてクラウド ストレージを増減できるため、クラウド ストレージの柔軟な性質がこの手法を容易にしています。
> 
> 

## <a name="the-volumes-page"></a>[ボリューム] ページ
**[ボリューム]** ページでは、イニシエーター (サーバー) 用に Microsoft Azure StorSimple デバイスにプロビジョニングされているストレージ ボリュームを管理することができます。 このページには、StorSimple デバイス上のボリュームの一覧が表示されます。

 ![[ボリューム] ページ](./media/storsimple-manage-volumes/HCS_VolumesPage.png)

ボリュームは、次の一連の属性で構成されます。

* **[名前]** - ボリュームを識別するのに役立つ、わかりやすい一意の名前。 この名前は、特定のボリュームをフィルター処理する場合に監視レポートにも使用されます。
* **[状態]** - オンラインまたはオフラインにすることができます。 ボリュームがオフラインの場合、ボリュームにアクセスして使用することが許可されているイニシエーター (サーバー) はこのボリュームを認識できません。
* **[容量]** - イニシエーター (サーバー) によって認識されるボリュームの大きさを指定します。 容量は、イニシエーター (サーバー) によって格納できるデータの合計量を指定します。 ボリュームは仮想プロビジョニングされ、データは重複除去されます。 これは、デバイスが構成されたボリューム容量に従って物理ストレージ容量を内部的またはクラウド上に事前割り当てしないことを暗に意味します。 ボリュームの容量は、要求に応じて割り当てられ、使用されます。
* **[種類]** - ボリュームの種類として、階層化またはアーカイブ (階層化のサブタイプ) を指定できます。
* **[アクセス]** - このボリュームへのアクセスを許可するイニシエーター (サーバー) を指定します。 ボリュームに関連付けられているアクセス制御レコード (ACR) のメンバーではないイニシエーターは、このボリュームを認識できません。
* **[監視]** - ボリュームを監視するかどうかを指定します。 ボリュームは、作成されると既定で監視が有効になります。 ただし、ボリュームの複製に対する監視は無効になります。 ボリュームの監視を有効にするには、「ボリュームを監視する」の手順に従います。

ボリュームに関連する最も一般的なタスクは次のとおりです。

* ボリュームを追加する
* ボリュームを変更する
* ボリュームを削除する
* ボリュームをオフラインにする
* ボリュームを監視する

## <a name="add-a-volume"></a>ボリュームを追加する
[ボリュームの作成](storsimple-deployment-walkthrough-u1.md#step-6-create-a-volume) は、StorSimple ソリューションのデプロイメント時に完了しています。 ボリュームを追加するには、同様の手順を実行します。

### <a name="to-add-a-volume"></a>ボリュームを追加するには
1. **[デバイス]** ページで、デバイスを選択し、ダブルクリックします。次に、**[ボリューム コンテナー]** タブをクリックします。
2. ボリューム コンテナーを選択し、対応する行の矢印をクリックして、コンテナーに関連付けられているボリュームにアクセスします。
3. ページの下部にある **[追加]** をクリックします。 ボリュームの追加ウィザードが起動されます。
   
     ![ボリュームの追加ウィザードの基本設定](./media/storsimple-manage-volumes/AddVolume1.png)
4. ボリュームの追加ウィザードの **[基本設定]**で、次の操作を行います。
   
   1. **[名前]** に、ボリュームの名前を指定します。
   2. **[プロビジョニング容量]** に、ボリュームのプロビジョニング容量を GB または TB 単位で指定します。 物理デバイスの場合、1 GB ～ 64 TB の範囲で容量を指定する必要があります。 StorSimple 仮想デバイス上のボリュームにプロビジョニングできる最大容量は 30 TB です。
   3. ボリュームの **[使用法の種類]** を選択します。 階層化ボリュームをアーカイブ データに使用する場合は、 **[アクセス頻度の低いアーカイブ データにこのボリュームを使用します]** チェック ボックスをオンにすると、ボリュームの重複除去チャンク サイズが 512 KB に変更されます。 このオプションをオフにすると、対応する階層化されたボリュームに 64 KB のチャンク サイズが使用されます。 重複除去チャンク サイズを大きくすると、デバイスは大きなアーカイブ データをクラウドに迅速に転送できるようになります ("階層化ボリューム" は、以前は "プライマリ ボリューム" と呼ばれていました)。
   4. 矢印アイコン ![矢印アイコン](./media/storsimple-manage-volumes/HCS_ArrowIcon.png)をクリックして **[追加設定]** ページに移動します。
      
        ![ボリュームの追加ウィザードの追加設定](./media/storsimple-manage-volumes/AddVolume2.png)
5. **[追加設定]**で、新しいアクセス制御レコード (ACR) を追加します。
   
   1. ボックスの一覧で、アクセス制御レコード (ACR) を選択します。 また、新しい ACR を追加することもできます。 ACR は、ホストの IQN をレコードに記載されている IQN と照合することによって、ボリュームにアクセスできるホストを判定します。
   2. **[このボリュームの既定のバックアップの有効化]** チェックボックスをオンにして、既定のバックアップを有効にすることをお勧めします。
   3. チェック マーク アイコン  ![チェック マーク アイコン](./media/storsimple-manage-volumes/HCS_CheckIcon.png)  をクリックして、指定した設定でボリュームを作成します。

新しいボリュームが使用できるようになります。

## <a name="modify-a-volume"></a>ボリュームを変更する
ボリュームを拡張する必要がある場合やボリュームにアクセスするホストを変更する必要がある場合は、ボリュームを変更します。

> [!IMPORTANT]
> * デバイス上のボリューム サイズを変更した場合、ホスト上のボリュームのサイズも変更する必要があります。
> * ここで説明されているホスト側の手順は、Windows Server 2012 (2012R2) 向けです。 Linux などの他のホスト オペレーティング システムでは、手続きが異なります。 別のオペレーティング システムで稼働しているホスト上のボリュームを変更する場合は、お使いのホスト オペレーティング システムの指示を参照してください。
> 
> 

### <a name="to-modify-a-volume"></a>ボリュームを変更するには
1. **[デバイス]** ページで、デバイスを選択し、ダブルクリックします。次に、**[ボリューム コンテナー]** タブをクリックします。 このページには、デバイスに関連付けられているすべてのボリューム コンテナーが表形式で表示されます。
2. ボリューム コンテナーを選択し、クリックしてそのコンテナー内のすべてのボリュームを一覧表示します。
3. **[ボリューム]** ページで、ボリュームを選択し、**[変更]** をクリックします。
4. ボリュームの変更ウィザードの **[基本設定]**では、次の操作を実行できます。
   
   * **[アクセス頻度の低いアーカイブ データにこのボリュームを使用します]** チェック ボックスをオンにして、ボリュームの重複除去チャンク サイズを 512 KB に変更することで、階層化ボリュームをアーカイブ ボリュームに変更する場合は、**[名前]** と **[種類]** を編集します。
   * **プロビジョニング容量**を増やす。 **プロビジョニング容量** は、増やすことしかできません。 ボリュームを作成した後で小さくすることはできません。
     
     > [!NOTE]
     > ボリュームにボリューム コンテナーを割り当てた後でボリューム コンテナーを変更することはできません。
     > 
     > 
5. **[追加設定]**では、次の操作を実行できます。
   
   * ACR を変更する (ボリュームがオフラインの場合)。 ボリュームがオンラインの場合は、最初にボリュームをオフラインにする必要があります。 ACR を変更する前に、「 [ボリュームをオフラインにする](#take-a-volume-offline) 」の手順を参照してください。
   * ボリュームをオフラインにした後、ACR の一覧を変更する。
     
     > [!NOTE]
     > ボリュームの **[このボリュームの既定のバックアップの有効化]** オプションは変更できません。
     > 
     > 
6. チェック マーク アイコン  ![チェック マーク アイコン](./media/storsimple-manage-volumes/HCS_CheckIcon.png)」を参照してください。 ボリュームを更新中であることを示すメッセージが Azure クラシック ポータルに表示されます。 ボリュームが正常に更新されると、成功メッセージが表示されます。
7. ボリュームを拡張する場合は、Windows ホスト コンピューターで次の手順を実行します。
   
   1. **[コンピューターの管理]** ->**[ディスクの管理]** の順に移動します。
   2. **[ディスクの管理]** を右クリックし、**[ディスクの再スキャン]** を選択します。
   3. ディスクの一覧で、更新したボリュームを選択して右クリックし、 **[ボリュームの拡張]**を選択します。 ボリューム拡張ウィザードが起動します。 **[次へ]**をクリックします。
   4. 既定の値を使用してウィザードを完了します。 ウィザードが終了すると、サイズが増えたボリュームが表示されます。

![ビデオ](./media/storsimple-manage-volumes/Video_icon.png) **ビデオ**

ボリュームを展開する方法を説明したビデオについては、 [こちら](https://azure.microsoft.com/documentation/videos/expand-a-storsimple-volume/)を参照してください。

## <a name="take-a-volume-offline"></a>ボリュームをオフラインにする
ボリュームを変更または削除する場合は、ボリュームをオフラインにすることが必要になる場合があります。 ボリュームがオフラインのときは、ボリュームに対して読み取り/書き込みアクセスを行うことはできません。 デバイス上だけでなく、ホスト上のボリュームもオフラインにする必要があります。 ボリュームをオフラインにするには、次の手順を実行します。

### <a name="to-take-a-volume-offline"></a>ボリュームをオフラインにするには
1. ボリュームをオフラインにする前に、対象のボリュームが使用されていないことを確認します。
2. 最初に、ホスト上でボリュームをオフラインにします。 これにより、ボリューム上のデータが破損するリスクを排除できます。 具体的な手順については、ホストのオペレーティング システムの説明を参照してください。
3. ホストがオフラインになったら、次の手順を実行して、デバイス上のボリュームをオフラインにします。
   
   1. **[デバイス]** ページで、デバイスを選択し、ダブルクリックします。次に、**[ボリューム コンテナー]** タブをクリックします。 **[ボリューム コンテナー]** タブには、デバイスに関連付けられているすべてのボリューム コンテナーが表形式で表示されます。
   2. ボリューム コンテナーを選択し、クリックしてそのコンテナー内のすべてのボリュームを一覧表示します。
   3. ボリュームを選択し、 **[オフラインにする]**をクリックします。
   4. 確認を求められたら、 **[はい]**をクリックします。 ボリュームがオフラインになります。
      
      ボリュームがオフラインになると、 **[オンラインにする]** オプションが使用可能になります。

> [!NOTE]
> **[オフラインにする]** コマンドは、ボリュームをオフラインにする要求をデバイスに送信します。 ホストがボリュームを使用しているときにこのコマンドを実行すると、接続が切断されますが、ボリュームをオフラインにする操作は失敗しません。
> 
> 

## <a name="delete-a-volume"></a>ボリュームを削除する
> [!IMPORTANT]
> ボリュームは、オフラインのときに限り削除することができます。
> 
> 

ボリュームを削除するには、次の手順を実行します。

### <a name="to-delete-a-volume"></a>ボリュームを削除するには
1. **[デバイス]** ページで、デバイスを選択し、ダブルクリックします。次に、**[ボリューム コンテナー]** タブをクリックします。
2. 削除するボリュームが含まれているボリューム コンテナーを選択します。 ボリューム コンテナーをクリックして **[ボリューム]** ページにアクセスします。
3. このコンテナーに関連付けられているすべてのボリュームが表形式で表示されます。 削除するボリュームの状態を確認します。 削除するボリュームがオフラインでない場合は、最初に 「 [ボリュームをオフラインにする](#take-a-volume-offline)」の手順に従ってボリュームをオフラインにします。
4. ボリュームがオフラインになったら、ページ下部にある **[削除]** をクリックします。
5. 確認を求められたら、 **[はい]**をクリックします。 ボリュームが削除されます。**[ボリューム]** ページに、コンテナー内のボリュームの更新された一覧が表示されます。

## <a name="monitor-a-volume"></a>ボリュームを監視する
ボリュームの監視機能を使用すると、ボリュームの I/O に関連する統計情報を収集できます。 既定では、最初に作成される 32 個のボリュームに対して監視が有効に設定されます。 追加のボリュームに対しては、監視は既定で無効に設定されます。 また、複製されたボリュームの監視も既定で無効に設定されます。

ボリュームの監視を有効または無効にするには、次の手順を実行します。

### <a name="to-enable-or-disable-volume-monitoring"></a>ボリュームの監視を有効または無効にするには
1. **[デバイス]** ページで、デバイスを選択し、ダブルクリックします。次に、**[ボリューム コンテナー]** タブをクリックします。
2. ボリュームが含まれているボリューム コンテナーを選択し、ボリューム コンテナーをクリックして **[ボリューム]** ページにアクセスします。
3. このコンテナーに関連付けられているすべてのボリュームが表形式で表示されます。 ボリュームまたはボリュームの複製をクリックして選択します。
4. ページの下部にある **[変更]**をクリックします。
5. ボリュームの変更ウィザードの **[基本設定]** で、**[監視]** ボックスの一覧の **[有効]** または **[無効]** を選択します。
   
    ![ボリュームの基本設定を変更する](./media/storsimple-manage-volumes/HCS_MonitorVolumeM.png)

## <a name="next-steps"></a>次のステップ
* [StorSimple ボリュームを複製する](storsimple-clone-volume.md)方法について説明します。
* [StorSimple Manager サービスを使用した StorSimple デバイスの管理方法](storsimple-manager-service-administration.md)




<!--HONumber=Nov16_HO3-->


