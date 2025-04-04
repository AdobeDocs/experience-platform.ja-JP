---
title: UI での SugarCRM イベントソース接続の作成
description: Adobe Experience Platform UI を使用して SugarCRM イベントソース接続を作成する方法を説明します。
exl-id: db346ec0-2c57-4b82-8a39-f15d4cd377d4
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '660'
ht-degree: 35%

---

# UI での [!DNL SugarCRM Events] ソース接続の作成

このチュートリアルでは、Adobe Experience Platform ユーザーインターフェイスを使用して [!DNL SugarCRM Events] ソース接続を作成する手順について説明します。

## はじめに

このチュートリアルは、 Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)]  システム](../../../../../xdm/home.md)：[!DNL Experience Platform] が顧客体験データの整理に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-Time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

有効な [!DNL SugarCRM] アカウントを既にお持ちの場合は、このドキュメントの残りの部分をスキップし、[データフローの設定](../../dataflow/crm.md)に関するチュートリアルに進んでください。

### 必要な資格情報の収集

[!DNL SugarCRM Events] をExperience Platformに接続するには、次の接続プロパティの値を指定する必要があります。

| 資格情報 | 説明 | 例 |
| --- | --- | --- |
| `Host` | ソースが接続する SugarCRM API エンドポイント。 | `developer.salesfusion.com` |
| `Username` | SugarCRM 開発者アカウントのユーザー名。 | `abc.def@example.com@sugarmarketdemo000.com` |
| `Password` | SugarCRM 開発者アカウントのパスワード。 | `123456789` |

### [!DNL SugarCRM] 用のExperience Platform スキーマの作成

[!DNL SugarCRM] ソース接続を作成する前に、まずソースに使用するExperience Platform スキーマを作成する必要もあります。 スキーマの作成方法に関する包括的な手順については、[Experience Platform スキーマの作成 ](../../../../../xdm/schema/composition.md) に関するチュートリアルを参照してください。

![SugarCRM イベントのスキーマ例を示すExperience Platform UI のスクリーンショット ](../../../../images/tutorials/create/sugarcrm-events/sugarcrm-schema-events.png)

>[!WARNING]
>
>スキーマをマッピングする際には、Experience Platformに必要な必須の `event_id` フィールドと `timestamp` フィールドもマッピングする必要があります。

## [!DNL SugarCRM Events] アカウントを接続

Experience Platform UI の左側のナビゲーションバーで「**[!UICONTROL ソース]**」を選択し、「[!UICONTROL  ソース ] ワークスペースにアクセスします。 [!UICONTROL カタログ]画面には、アカウントを作成できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択することができます。または、使用する特定のソースを検索オプションを使用して探すこともできます。

*CRM* カテゴリ内で、「**[!UICONTROL SugarCRM イベント]**」、「**[!UICONTROL データを追加]**」の順に選択します。

![SugarCRM イベントカードを含むカタログのExperience Platform UI のスクリーンショット ](../../../../images/tutorials/create/sugarcrm-events/catalog-sugarcrm-events.png)

**[!UICONTROL SugarCRM イベントアカウントを接続]** ページが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

### 既存のアカウント

既存のアカウントを使用するには、新しいデータフローを作成する [!DNL SugarCRM Events] アカウントを選択し、「**[!UICONTROL 次へ]**」を選択して続行します。

![SugarCRM イベントアカウントを既存のアカウントに接続するためのExperience Platform UI のスクリーンショット ](../../../../images/tutorials/create/sugarcrm-events/existing.png)

### 新規アカウント

新しいアカウントを作成する場合は、「**[!UICONTROL 新しいアカウント]**」を選択し、続けて名前、説明（オプション）、の認証情報を指定します。 終了したら「**[!UICONTROL ソースに接続]**」を選択し、新しい接続が確立されるまでしばらく待ちます。

![SugarCRM イベントアカウントを新しいアカウントに接続するためのExperience Platform UI のスクリーンショット ](../../../../images/tutorials/create/sugarcrm-events/new.png)

## 次の手順

このチュートリアルでは、[!DNL SugarCRM Events] アカウントとの接続を確立しました。次のチュートリアルに進み、[ データをExperience Platformに取り込むためのデータフローの設定 ](../../dataflow/crm.md) を行いましょう。

## その他のリソース

以下の節では、[!DNL SugarCRM] ソースを使用する際に参照できるその他のリソースを示します。

### ガードレール {#guardrails}

[!DNL SugarCRM] API のスロットル率は、1 分あたりの呼び出し 90 回か 1 日あたりの呼び出し 2000 回のいずれか早い方です。 ただし、この制限は、接続仕様にパラメーターを追加することで回避されました。このパラメーターはリクエスト時間を遅延させるので、レート制限に到達することはありません。

### 検証 {#validation}

ソースを正しく設定し、データが取り込まれてい [!DNL SugarCRM Events] ことを検証するには、次の手順に従います。

* Experience Platform UI で、ソースカタログの [!DNL SugarCRM Events] ーザーカードメニューの横にある **[!UICONTROL データフローを表示]** を選択します。 次に、「**[!UICONTROL データセットをプレビュー]**」をクリックして、取り込まれたデータを確認します。

* 操作しているオブジェクトタイプに応じて、以下の [!DNL SugarMarket] のイベントページに表示されるカウントに対して集計データを検証できます。

![ アカウントのリストを表示する SugarMarket アカウントページからのスクリーンショット ](../../../../images/tutorials/create/sugarcrm-events/sugarmarket-events.png)

>[!NOTE]
>
>[!DNL SugarMarket] ページには、削除されたオブジェクトの数は含まれません。 ただし、このソースを通じて取得されたデータには、削除されたカウントも含まれ、これらは削除されたフラグでマークされます。
