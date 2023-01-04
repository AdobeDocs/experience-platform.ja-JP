---
keywords: Experience Platform;ホーム;人気のトピック;
title: （ベータ版）UI での Adobe Workfront ソース接続の作成
description: このチュートリアルでは、ユーザーインターフェイスを使用して Workfront データを Adobe Experience Platform に取り込むための Adobe Workfront ソース接続を作成する手順について説明します。
exl-id: f82e852a-c9d1-4ecc-bc54-2b39d3b4cc1e
source-git-commit: 34e0381d40f884cd92157d08385d889b1739845f
workflow-type: tm+mt
source-wordcount: '771'
ht-degree: 97%

---

# （ベータ版）UI での Adobe Workfront ソース接続の作成

>[!NOTE]
>
>Adobe Workfront ソースはベータ版です。ベータラベル付きソースの使用について詳しくは、[ソースの概要](../../../../home.md#terms-and-conditions)を参照してください。

このチュートリアルでは、ユーザーインターフェイスを使用して Workfront データを Adobe Experience Platform に取り込むための Adobe Workfront ソース接続を作成する手順について説明します。

## はじめに

>[!IMPORTANT]
>
>Workfront ソースにアクセスするには、Adobe Admin Console で管理者として設定されている必要があります。

このチュートリアルは、 Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [エクスペリエンスデータモデルl（XDM）システム](../../../../../xdm/home.md)：Experience Platform が顧客体験データを整理するための標準化されたフレームワークです。
* [リアルタイム顧客プロファイル](../../../../../profile/home.md):複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。
* [サンドボックス](../../../../../sandboxes/home.md)：Experience Platform には、単一の Platform インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

## UI での Workfront ソース接続の作成

Platform UI の左側のナビゲーションバーで「**[!UICONTROL ソース]**」を選択して、[!UICONTROL ソース]ワークスペースにアクセスします。[!UICONTROL カタログ]画面には、アカウントの作成に使用できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択することができます。また、検索バーを使用して、表示されるソースを絞り込むこともできます。

**[!UICONTROL アドビアプリケーション]**&#x200B;カテゴリで、「**[!UICONTROL Adobe Workfront]**」を選択し、次に「**[!UICONTROL データを追加]**」を選択します。

![Adobe Workfront ソースがハイライト表示されているソースカタログ](../../../../images/tutorials/create/workfront/catalog.png)

## データの選択

[!UICONTROL データを選択]ステップが表示されます。ここでは、Workfront サブドメインとデータレーンの値を指定する必要があります。Workfront サブドメインは、`https://acme.workfront.com/` など、Workfront インスタンスへのアクセスに使用する URL と同じですが、データレーンは、使用する Workfront 環境を表します。

サブドメインとデータレーンを追加したら、「**[!UICONTROL 次へ]**」を選択します。

![サブドメインとデータレーンのプレースホルダー値を含んだデータを選択ページ](../../../../images/tutorials/create/workfront/select-data.png)

## データフローの詳細を入力

データフローの詳細ステップでは、データフローの名前と説明（オプション）を入力できます。このステップでは、アラートを購読して、データフローのステータスに関する通知を受け取れるようにすることもできます。アラートについて詳しくは、[ソース UI でのアラートの購読](../../alerts.md)に関するチュートリアルを参照してください。

データフローの詳細を指定し、目的のアラート設定をしたら、「**[!UICONTROL 次へ]**」を選択します。

![データフローの名前、説明およびアラート通知に関する情報を含んだデータフローの詳細ページ](../../../../images/tutorials/create/workfront/dataflow-detail.png)

## レビュー

**[!UICONTROL レビュー]**&#x200B;手順が表示され、新しいデータフローを作成する前に確認できます。詳細は、次のカテゴリに分類されます。

* **[!UICONTROL 接続]**：ソースのタイプ、選択したソースファイルの関連パスおよびそのソースファイル内の列の数を表示します。
* **[!UICONTROL データセットの割り当てとフィールドのマッピング]**：ソースデータがどのデータセットに取り込まれるかを、そのデータセットが準拠するスキーマを含めて表示します。

データフローをレビューしたら、「**[!UICONTROL 終了]**」を選択し、データフローが作成されるまでしばらく待ちます。

![接続情報を要約したレビューページ](../../../../images/tutorials/create/workfront/review.png)

## 付録

以下の節では、Workfront ソースに関する追加情報を示します。

### Workfront 変更イベントスキーマ

Platform の Workfront データは、時系列のレコードデータとして表されます。データの各行には、イベント発生時のタイムスタンプと、そのイベントに関連する属性が表示されます。

設定時に、フローから Workfront 変更イベントというスキーマが作成されます。

| スキーマフィールド | 説明 |
| --- | --- |
| `timestamp` | 選択したイベントが発生した時間。タイムスタンプは GMT タイムゾーンで表されます。 |
| `_workfront.objectType` | オブジェクトタイプ。使用可能な値には、変更または作成されたオブジェクトに応じて、`project`、`task`、`portfolio` などがあります。 |
| `_workfront.objectID` | オブジェクトタイプに対応する ID。 |
| `_workfront.created` | イベントがオブジェクトの作成を表している場合、この値は `1` に設定されます。 |
| `_workfront.deleted` | オブジェクトが削除された場合、この値は `1` に設定されます。 |
| `_worfkront.updated` | オブジェクトが更新された場合、この値は `1` に設定されます。 |
| `_workfront.completed` | オブジェクトが完了とマークされている場合、この値は `1` に設定されます。 |
| `_workfront.parentObjectType` | （オプション）オブジェクトの親に対応するオブジェクトタイプ。 |
| `_workfront.parentID` | 親オブジェクトの ID。 |
| `_workfront.customData` | イベント中に入力されたすべてのカスタムフォームフィールドと値のマップ。 |

>[!IMPORTANT]
>
>イベントの一部として変更または作成された属性のみが入力されます。例えば、オブジェクトの名前のみを変更する場合は、次のフィールドのみ、値が入力されます。<ul><li>`timestamp`</li><li>`_workfront.update (=1)`</li><li>`_workfront.objectType`</li><li>`_workfront.objectID`</li><li>`_workfront.objectName`</li></ul>

## 次の手順

このチュートリアルでは、Workfront から Experience Platform にデータを取り込むためのデータフローを作成しました。[クエリサービス](../../../../../query-service/home.md)などのサービスを使用して、データをさらに分析できるようになりました。Workfront について詳しくは、[Workfront の概要](../../../../connectors/adobe-applications/workfront.md)を参照してください。
