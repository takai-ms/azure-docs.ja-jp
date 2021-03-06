---
title: "Enterprise Integration Pack の Decode EDIFACT Message コネクタの詳細情報 | Microsoft Docs"
description: "Enterprise Integration Pack と Logic Apps を使用してパートナーを使用する方法について説明します。"
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: anneta
editor: 
ms.assetid: 0e61501d-21a2-4419-8c6c-88724d346e81
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2016
ms.author: padmavc
translationtype: Human Translation
ms.sourcegitcommit: 8bf5fe5527e3de6f2e0950b3ee494b88cd1063a1
ms.openlocfilehash: 96dbedf2dc072795712e4b0a41c424e633263220


---
# <a name="get-started-with-decode-edifact-message"></a>Decode EDIFACT Message を使ってみる
EDI およびパートナー固有のプロパティを検証したり、各トランザクション セットに対して XML ドキュメントを生成したり、処理したトランザクションの受信確認を生成したりできます。

## <a name="create-the-connection"></a>接続の作成
### <a name="prerequisites"></a>前提条件
* Azure アカウント。[無料アカウント](https://azure.microsoft.com/free)を作成できます。
* Decode EDIFACT Message コネクタを使用するには、統合アカウントが必要です。 [統合アカウント](logic-apps-enterprise-integration-create-integration-account.md)、[パートナー](logic-apps-enterprise-integration-partners.md)、および [EDIFACT 契約](logic-apps-enterprise-integration-edifact.md)の作成方法の詳細を確認してください。

### <a name="connect-to-decode-edifact-message-using-the-following-steps"></a>次の手順に従って、Decode EDIFACT Message に接続します。
1. [ロジック アプリの作成](logic-apps-create-a-logic-app.md)に関する記事に例が記載されています。
2. このコネクタにはトリガーがありません。 ロジック アプリを起動するには、他のトリガー (要求トリガーなど) を使用します。  Logic Apps デザイナーで、トリガーを追加して、アクションを追加します。  ドロップダウン リストから [Microsoft が管理している API を表示] を選択し、検索ボックスに「EDIFACT」と入力します。  [Decode EDIFACT Message] を選択します。
   
    ![search EDIFACT](./media/logic-apps-enterprise-integration-edifact-decode/edifactdecodeimage1.png)
3. これまでに統合アカウントへの接続を作成したことがない場合は、接続の詳細情報を求められます。
   
    ![create integration account](./media/logic-apps-enterprise-integration-edifact-decode/edifactdecodeimage2.png)  
4. 統合アカウントの詳細を入力します。  アスタリスクが付いているプロパティは必須です。
   
   | プロパティ | 詳細 |
   | --- | --- |
   | 接続名 * |接続の任意の名前を入力します。 |
   | 統合アカウント * |統合アカウント名を入力します。 統合アカウントとロジック アプリが同じ Azure の場所にあることを確認してください。 |
   
    入力を完了すると、接続の詳細は次のようになります。
   
    ![integration account created](./media/logic-apps-enterprise-integration-edifact-decode/edifactdecodeimage3.png)  
5. **[作成]**
6. 接続が作成されたことを確認します。
   
    ![integration account connection details](./media/logic-apps-enterprise-integration-edifact-decode/edifactdecodeimage5.png)  
7. デコードする EDIFACT フラット ファイル メッセージを選択します。
   
    ![provide mandatory fields](./media/logic-apps-enterprise-integration-edifact-decode/edifactdecodeimage5.png)  

## <a name="edifact-decode-does-following"></a>EDIFACT Decode の機能
* 送信者の修飾子および識別子を受信者の修飾子および識別子と照合することで、契約を解決する
* 1 つのメッセージ内の複数のインターチェンジを分割する
* 取引パートナー契約と照らし合わせてエンベロープを検証する
* インターチェンジを逆アセンブルする
* EDI およびパートナー固有のプロパティに対して、次のような検証を行う
  * インターチェンジ エンベロープの構造の検証
  * 制御スキーマと照らし合わせたエンベロープのスキーマ検証
  * メッセージ スキーマと照らし合わせたトランザクション セット データ要素のスキーマ検証
  * トランザクション セット データ要素に対して実行される EDI 検証
* インターチェンジ、グループ、およびトランザクション セットの制御番号が重複していないことを検証する (構成されている場合) 
  * 以前に受信したインターチェンジと照らし合わせて、インターチェンジ制御番号を確認する。 
  * インターチェンジ内の他のグループ制御番号と照らし合わせて、グループ制御番号を確認する。 
  * グループ内の他のトランザクション セット制御番号と照らし合わせて、トランザクション セット制御番号を確認する。
* 各トランザクション セットの XML ドキュメントを生成する
* インターチェンジ全体を XML に変換する 
  * [インターチェンジをトランザクション セットとして分割 - エラー発生時にトランザクション セットを中断]: インターチェンジの各トランザクション セットを個別の XML ドキュメントとして解析します。 インターチェンジの&1; つ以上のトランザクション セットが検証に失敗した場合、EDIFACT Decode は失敗したトランザクション セットのみを中断します。 
  * [インターチェンジをトランザクション セットとして分割 - エラー発生時にインターチェンジを中断]: インターチェンジの各トランザクション セットを個別の XML ドキュメントとして解析します。  インターチェンジの&1; つ以上のトランザクション セットが検証に失敗した場合、EDIFACT Decode はインターチェンジ全体を中断します。
  * [インターチェンジの保存 - エラー発生時にトランザクション セットを中断]: バッチ インターチェンジ全体に対する XML ドキュメントを作成します。 EDIFACT Decode は、検証に失敗したトランザクション セットだけを中断し、他のすべてのトランザクション セットの処理を継続します。
  * [インターチェンジの保存 - エラー発生時にインターチェンジを中断]: バッチ インターチェンジ全体に対する XML ドキュメントを作成します。 インターチェンジの&1; つ以上のトランザクション セットが検証に失敗した場合、EDIFACT Decode はインターチェンジ全体を中断します。 
* 技術 (制御) 確認または機能確認を生成する (構成されている場合)
  * 技術確認または制御確認は、完全に受信したインターチェンジの構文チェックの結果を報告します。
  * 機能確認は、受信したインターチェンジまたはグループの承認または拒否を確認します。

## <a name="next-steps"></a>次のステップ
[Enterprise Integration Pack についての詳細情報](logic-apps-enterprise-integration-overview.md "Enterprise Integration Pack についての詳細情報") 




<!--HONumber=Jan17_HO3-->


