---
title: UI での  [!DNL Oracle NetSuite Entities]  ソース接続の作成
description: Adobe Experience Platform UI を使用して、OracleNetSuite Entities ソース接続を作成する方法を説明します。
hide: true
hidefromtoc: true
badge: ベータ版
source-git-commit: 053cf0af327b39830f025686e0f8f67c27f1c45c
workflow-type: tm+mt
source-wordcount: '698'
ht-degree: 39%

---

# UI での [!DNL Oracle NetSuite Entities] ソース接続の作成

>[!NOTE]
>
>[!DNL Oracle NetSuite Entities] ソースはベータ版です。詳しくは、 [ソースの概要](../../../../home.md#terms-and-conditions) ベータラベル付きのソースの使用に関する詳細

次のチュートリアルを読んで、連絡先と顧客データを [!DNL Oracle NetSuite Entities] をAdobe Experience Platformにアカウントします。

## はじめに {#getting-started}

このチュートリアルは、 Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)]  システム](../../../../../xdm/home.md)：[!DNL Experience Platform] が顧客体験データの整理に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-Time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

有効な [!DNL Oracle NetSuite] アカウントを既にお持ちの場合は、このドキュメントの残りの部分をスキップし、[データフローの設定](../../dataflow/marketing-automation.md)に関するチュートリアルに進んでください。

>[!TIP]
>
>詳しくは、 [[!DNL Oracle NetSuite] 概要](../../../../connectors/marketing-automation/oracle-netsuite.md) 認証資格情報の取得方法の詳細。

## [!DNL Oracle NetSuite Activities] アカウントを接続 {#connect-account}

Platform UI の左側のナビゲーションバーで「**[!UICONTROL ソース]**」を選択して、[!UICONTROL ソース]ワークスペースにアクセスします。画面の左側にあるカタログから適切なカテゴリを選択することができます。または、使用する特定のソースを検索オプションを使用して探すこともできます。

の下 *マーケティング自動化* カテゴリ、選択 **[!DNL Oracle NetSuite Entities]**&#x200B;を選択し、 **[!UICONTROL データを追加]**.

![「NetSuite エンティティ」カードを含むカタログのOracleUI スクリーンショット](../../../../images/tutorials/create/marketing-automation/oracle-netsuite-entities/catalog-card.png)

The **[!UICONTROL oracleNetSuite Entities アカウントを接続]** ページが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

>[!IMPORTANT]
>
>更新トークンは 7 日後に期限切れになります。 トークンの有効期限が切れたら、更新されたトークンを使用してExperience Platform上にアカウントを作成する必要があります。 更新されたトークンで新しいアカウントを作成しない場合、次のエラーメッセージが表示される場合があります。 `The request could not be processed. Error from flow provider: The request could not be processed. Rest call failed with client error, status code 401 Unauthorized, please check your activity settings.`

### 既存のアカウント {#existing-account}

既存のアカウントを使用するには、新しいデータフローを作成する [!DNL Oracle NetSuite Entities] アカウントを選択し、「**[!UICONTROL 次へ]**」を選択して続行します。

![oracleNetSuite Entities アカウントを既存のアカウントに接続するための Platform UI スクリーンショット](../../../../images/tutorials/create/marketing-automation/oracle-netsuite-entities/existing.png)

### 新規アカウント {#new-account}

新しいアカウントを作成する場合は、「 **[!UICONTROL 新しいアカウント]**&#x200B;をクリックし、名前、オプションの説明および資格情報を入力します。 終了したら「**[!UICONTROL ソースに接続]**」を選択し、新しい接続が確立されるまでしばらく待ちます。

![oracleNetSuite Entities アカウントを新しいアカウントに接続するための Platform UI スクリーンショット](../../../../images/tutorials/create/marketing-automation/oracle-netsuite-entities/new.png)

### データの選択

次に、Experience Platformに取り込むオブジェクトタイプを選択します。

| エンティティタイプ | 説明 |
| --- | --- |
| 連絡先 | 顧客に関連付けられた連絡先名、E メール、電話番号、およびカスタム連絡先関連のフィールドを取得します。 |
| 顧客 | 顧客名、住所、主要識別子などの詳細を含む、特定の顧客データを取得します。 |

>[!BEGINTABS]

>[!TAB 連絡先]

![「連絡先」オプションが選択された状態で設定を示す、OracleNetsuite エンティティの Platform UI スクリーンショット](../../../../images/tutorials/create/marketing-automation/oracle-netsuite-entities/select-data-contact.png)

>[!TAB 顧客]

![顧客オプションを選択した状態で設定を示すOracleNetSite エンティティの Platform UI スクリーンショット](../../../../images/tutorials/create/marketing-automation/oracle-netsuite-entities/select-data-customer.png)

>[!ENDTABS]

## 次の手順 {#next-steps}

このチュートリアルでは、[!DNL Oracle NetSuite Entities] アカウントとの接続を確立しました。次のチュートリアルに進み、 [データフローを設定してマーケティング自動化データを Platform に取り込む](../../dataflow/marketing-automation.md).

## その他のリソース {#additional-resources}

以下の節では、 [!DNL Oracle NetSuite Entities] ソース。

### マッピング {#mapping}

Platform は、選択したターゲットスキーマまたはデータセットに基づいて、自動マッピングされたフィールドに対するインテリジェントなレコメンデーションを提供します。 マッピングルールは、使用例に合わせて手動で調整できます。 必要に応じて、フィールドを直接マッピングするか、データ準備機能を使用してソースデータを変換して計算値を導き出すかを選択できます。マッパーインターフェイスと計算フィールドを使用した包括的な手順については、 [データ準備 UI ガイド](../../../../../data-prep/ui/mapping.md).

>[!NOTE]
>
>表示されるフィールドは、 [!DNL Oracle NetSuite] アカウントはにアクセスできます。 例えば、請求に対するアクセス権がない場合、請求関連のフィールドは表示されません。

### スケジュール設定 {#scheduling}

スケジュール設定時に [!DNL Oracle NetSuite Entities] データフローを取り込むには、次の頻度と間隔の設定を選択する必要があります。

| 頻度 | 間隔 |
| --- | --- |
| `Once` | 1 |

データを取得する際に、 [!DNL Oracle NetSuite] は、タイムスタンプの代わりに、最終変更日または作成日を日付形式で応答します。 したがって、スケジュールは 1 日に制限されます。

スケジュールの値を指定したら、「 」を選択します。 **[!UICONTROL 次へ]**.

![ソースワークフローのスケジュール設定手順。](../../../../images/tutorials/create/marketing-automation/oracle-netsuite-entities/scheduling.png)