---
title: UI での Mixpanel Sourceの作成
description: Adobe Experience Platform UI を使用して Mixpanel ソース接続を作成する方法を説明します。
exl-id: 2a02f6a4-08ed-468c-8052-f5b7be82d183
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '805'
ht-degree: 33%

---

# UI での [!DNL Mixpanel] ソース接続の作成

このチュートリアルでは、Adobe Experience Platform Experience Platformのユーザーインターフェイスを使用して [!DNL Mixpanel] ソース接続を作成する手順について説明します。

## はじめに

このチュートリアルは、 Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)]  システム](../../../../../xdm/home.md)：[!DNL Experience Platform] が顧客体験データの整理に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-Time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

### 必要な資格情報の収集

[!DNL Mixpanel] をExperience Platformに接続するには、次の接続プロパティの値を指定する必要があります。

| 資格情報 | 説明 | 例 |
| --- | --- | --- |
| ユーザー名 | [!DNL Mixpanel] アカウントに対応するサービス アカウントのユーザー名。 詳しくは、[[!DNL Mixpanel]  サービスアカウントのドキュメント ](https://developer.mixpanel.com/reference/service-accounts#authenticating-with-a-service-account) を参照してください。 | `Test8.6d4ee7.mp-service-account` |
| パスワード | [!DNL Mixpanel] アカウントに対応するサービス アカウントのパスワード。 | `dLlidiKHpCZtJhQDyN2RECKudMeTItX1` |
| プロジェクト ID | [!DNL Mixpanel] プロジェクト ID。 この ID は、ソース接続を作成するために必要です。 詳しくは、[[!DNL Mixpanel]  プロジェクト設定ドキュメント ](https://help.mixpanel.com/hc/en-us/articles/115004490503-Project-Settings) および [[!DNL Mixpanel]  プロジェクトの作成と管理に関するガイド ](https://help.mixpanel.com/hc/en-us/articles/115004505106-Create-and-Manage-Projects) を参照してください。 | `2384945` |
| タイムゾーン | [!DNL Mixpanel] プロジェクトに対応するタイムゾーン。 ソース接続を作成するにはタイムゾーンが必要です。 詳しくは、[Mixpanel プロジェクト設定ドキュメント ](https://help.mixpanel.com/hc/en-us/articles/115004490503-Project-Settings) を参照してください。 | `Pacific Standard Time` |

[!DNL Mixpanel] ソースの認証について詳しくは、[[!DNL Mixpanel]  ソースの概要 ](../../../../connectors/analytics/mixpanel.md) を参照してください。

## [!DNL Mixpanel] アカウントを接続

Experience Platform UI の左側のナビゲーションバーで「**[!UICONTROL ソース]**」を選択し、「[!UICONTROL  ソース ] ワークスペースにアクセスします。 [!UICONTROL カタログ]画面には、アカウントを作成できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択することができます。または、使用する特定のソースを検索オプションを使用して探すこともできます。

*Analytics* カテゴリで、「[!DNL Mixpanel]」を選択し、次に **[!UICONTROL データを追加]** を選択します。

![カタログ](../../../../images/tutorials/create/mixpanel-export-events/catalog.png)

**[!UICONTROL Mixpanel アカウントを接続]** ページが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

### 既存のアカウント

既存のアカウントを使用するには、新しいデータフローを作成する [!DNL Mixpanel] アカウントを選択し、「**[!UICONTROL 次へ]**」を選択して続行します。

![既存](../../../../images/tutorials/create/mixpanel-export-events/existing.png)

### 新しいアカウント

新しいアカウントを作成する場合は、「**[!UICONTROL 新しいアカウント]**」を選択し、続けて名前、説明（オプション）、の認証情報を指定します。 終了したら「**[!UICONTROL ソースに接続]**」を選択し、新しい接続が確立されるまでしばらく待ちます。

![新規](../../../../images/tutorials/create/mixpanel-export-events/new.png)

## プロジェクト ID とタイムゾーンを選択 {#project-id-and-timezone}

>[!CONTEXTUALHELP]
>id="platform_sources_mixpanel_timezone"
>title="Mixpanel 取り込みのタイムゾーンの設定"
>abstract="Experience Platformは指定されたプロジェクトタイムゾーンを使用して Mixpanel から関連データを取り込むので、タイムゾーンは Mixpanel プロファイルのタイムゾーン設定と同じである必要があります。 Mixpanel は、Mixpanel データストアにイベントを記録する前に、プロジェクトのタイムゾーンに合わせて自身のタイムゾーンを調整します。"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/sources/ui-tutorials/create/analytics/mixpanel.html?lang=ja#project-id-and-timezone" text="詳しくは、ドキュメントを参照してください"

ソースが認証されたら、プロジェクト ID とタイムゾーンを指定し、「**[!UICONTROL 選択]**」を選択します。

[!DNL Mixpanel] データをExperience Platformに取り込む前に指定するタイムゾーンは、[!DNL Mixpanel] プロファイルタイムゾーン設定と同じである必要があります。 データのタイムゾーンの変更は新しいイベントにのみ適用され、古いイベントは以前に指定したタイムゾーンに残ります。 [!DNL Mixpanel] は夏時間を調整し、取り込みタイムスタンプを適切に調整します。 タイムゾーンがデータに与える影響について詳しくは、[ プロジェクトのタイムゾーンの管理 ](https://help.mixpanel.com/hc/en-us/articles/115004547203-Manage-Timezones-for-Projects-in-Mixpanel) に関する [!DNL Mixpanel] ガイドを参照してください。

しばらくすると、右側のインターフェイスがプレビューパネルに更新され、データフローを作成する前にスキーマを調べることができます。 終了したら、「**[!UICONTROL 次へ]**」を選択します。

![ 設定 ](../../../../images/tutorials/create/mixpanel-export-events/authentication-configuration.png)

## 次の手順

このチュートリアルでは、[!DNL Mixpanel] アカウントとの接続を確立しました。次のチュートリアルに進み、[ 分析データをExperience Platformに取り込むためのデータフローの設定 ](../../dataflow/analytics.md) を行いましょう。

## その他のリソース {#additional-resources}

以下の節では、[!DNL Mixpanel] ソースを使用する際に参照できるその他のリソースを示します。

### 検証 {#validation}

次に、[!DNL Mixpanel] ソースに正常に接続したことと、[!DNL Mixpanel] のイベントがExperience Platformに取り込まれていることを検証する手順の概要を説明します。

Experience Platformの UI で、左側のナビゲーションバーから **[!UICONTROL データセット]** を選択して、[!UICONTROL  データセット ] ワークスペースにアクセスします。 [!UICONTROL  データセットアクティビティ ] 画面には、実行の詳細が表示されます。

![dataset-activity](../../../../images/tutorials/create/mixpanel-export-events/dataset-activity.png)

次に、表示するデータフローのデータフロー実行 ID を選択して、そのデータフロー実行に関する特定の詳細を確認します。

![ データフローの監視 ](../../../../images/tutorials/create/mixpanel-export-events/dataflow-monitoring.png)

最後に、「**[!UICONTROL データセットをプレビュー]**」を選択して、取り込まれたデータを表示します。

![ データセットをプレビュー ](../../../../images/tutorials/create/mixpanel-export-events/preview-dataset.png)

このデータを [!DNL Mixpanel] > [!DNL Events] ページのデータと照合して検証できます。 詳しくは、[[!DNL Mixpanel]  イベントに関するドキュメント ](https://help.mixpanel.com/hc/en-us/articles/4402837164948-Events-formerly-Live-View-) を参照してください。

![mixpanel-events](../../../../images/tutorials/create/mixpanel-export-events/mixpanel-events.png)

### Mixpanel スキーマ

次の表に、[!DNL Mixpanel] 用に設定する必要があるサポート対象のマッピングを示します。

>[!TIP]
>
>API について詳しくは、[ イベント書き出し API/ダウンロード ](https://developer.mixpanel.com/reference/raw-event-export) を参照してください。


| ソース | タイプ |
|---|---|
| `distinct_id` | 文字列 |
| `event_name` | 文字列 |
| `import` | ブール値 |
| `insert_id` | 文字列 |
| `item_id` | 文字列 |
| `item_name` | 文字列 |
| `item_price` | 文字列 |
| `mp_api_endpoint` | 文字列 |
| `mp_api_timestamp_ms` | 整数 |
| `mp_processing_time_ms` | 整数 |
| `time` | 整数 |

### 制限 {#limits}

* [Export API のレート制限 ](https://help.mixpanel.com/hc/en-us/articles/115004602563-Rate-Limits-for-API-Endpoints) に示すように、同時クエリは最大 100 個、1 時間あたり 60 個です。
