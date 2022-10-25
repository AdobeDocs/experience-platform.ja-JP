---
keywords: Experience Platform;ホーム;人気のトピック;
title: （ベータ版）UI でのAdobe Workfrontソース接続の作成
description: このチュートリアルでは、ユーザーインターフェイスを使用してAdobe WorkfrontデータをAdobe Experience Platformに取り込むためのWorkfrontソース接続を作成する手順を説明します。
source-git-commit: 1af0863766e29c599e02f2a553d237bc62f455d2
workflow-type: tm+mt
source-wordcount: '771'
ht-degree: 22%

---

# （ベータ版）UI でのAdobe Workfrontソース接続の作成

>[!NOTE]
>
>Adobe Workfrontソースはベータ版です。 詳しくは、 [ソースの概要](../../../../home.md#terms-and-conditions) ベータラベル付きのソースの使用に関する詳細

このチュートリアルでは、ユーザーインターフェイスを使用してAdobe WorkfrontデータをAdobe Experience Platformに取り込むためのWorkfrontソース接続を作成する手順を説明します。

## はじめに

>[!IMPORTANT]
>
>Workfrontソースにアクセスするには、Adobe Admin Consoleの管理者として設定されている必要があります。

このチュートリアルは、 Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [エクスペリエンスデータモデル（XDM）システム](../../../../../xdm/home.md)：Experience Platform が顧客体験データを整理するための標準化されたフレームワーク。
* [リアルタイム顧客プロファイル](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムのプロファイルを顧客に提供します。
* [サンドボックス](../../../../../sandboxes/home.md)：Experience Platform には、単一の Platform インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

## UI でのWorkfrontソース接続の作成

Platform の UI で、左側のナビゲーションバーで「**[!UICONTROL ソース]**」を選択し、[!UICONTROL ソース]ワークスペースにアクセスします。この [!UICONTROL カタログ] 画面には、アカウントの作成に使用できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択することができます。また、検索バーを使用して、表示されるソースを絞り込むこともできます。

以下 **[!UICONTROL Adobe]** カテゴリ、選択 **[!UICONTROL Adobe Workfront]** 次に、 **[!UICONTROL データを追加]**.

![ソースカタログとAdobe Workfrontソースがハイライト表示されています。](../../../../images/tutorials/create/workfront/catalog.png)

## データの選択

この [!UICONTROL データを選択] 手順が表示されます。 ここでは、Workfrontサブドメインと Datalane の値を指定する必要があります。 Workfrontサブドメインは、例えば、Workfrontインスタンスへのアクセスに使用する URL と同じです `https://acme.workfront.com/`の場合、データランジは使用するワークフロント環境を表します。

サブドメインとデータレーンを追加したら、「 **[!UICONTROL 次へ]**.

![「サブドメイン」と「データランジ」のプレースホルダー値を持つデータページを選択します。](../../../../images/tutorials/create/workfront/select-data.png)

## データフローの詳細を入力

データフローの詳細手順では、データフローの名前と説明（オプション）を指定できます。 この手順の間に、アラートをサブスクライブして、データフローのステータスに関する通知を受け取ることもできます。 アラートの詳細については、次のチュートリアルを参照してください： [ソース UI でのアラートの購読](../../alerts.md).

データフローの詳細を指定し、目的のアラート設定を構成したら、「 」を選択します。 **[!UICONTROL 次へ]**.

![データフローの詳細ページ（データフロー名、説明、アラート通知に関する情報を含む）](../../../../images/tutorials/create/workfront/dataflow-detail.png)

## レビュー

**[!UICONTROL レビュー]**&#x200B;手順が表示され、新しいデータフローを作成する前に確認できます。詳細は、次のカテゴリに分類されます。

* **[!UICONTROL 接続]**:ソースのタイプ、選択したソースファイルの関連パス、およびそのソースファイル内の列数を表示します。
* **[!UICONTROL データセットの割り当てとフィールドのマッピング]**：ソースデータがどのデータセットに取り込まれるかを、そのデータセットが準拠するスキーマを含めて表示します。

データフローをレビューしたら、「 」を選択します。 **[!UICONTROL 完了]** とは、データフローが作成されるまでしばらく時間をかけます。

![接続情報を要約したレビューページ。](../../../../images/tutorials/create/workfront/review.png)

## 付録

以下の節では、Workfrontソースに関する追加情報を示します。

### Workfront Change Event Schema

Workfrontのデータは、時系列レコードデータとして表されます。データの各行には、イベントの発生時に表示されるタイムスタンプと、そのイベントに関連する属性が表示されます。

セットアップ中に、「 Workfront Change Events from Flow 」という名前のスキーマが作成されます。

| スキーマフィールド | 説明 |
| --- | --- |
| `timestamp` | 選択したイベントが発生した時間。 タイムスタンプは GTM タイムゾーンで表されます。 |
| `_workfront.objectType` | オブジェクトのタイプ。 使用可能な値は次のとおりです。 `project`, `task`, `portfolio`、その他（変更または作成されたオブジェクトに応じる） |
| `_workfront.objectID` | オブジェクトタイプに対応する ID。 |
| `_workfront.created` | この値は `1` イベントがオブジェクトの作成を表す場合。 |
| `_workfront.deleted` | この値は `1` を返します。 |
| `_worfkront.updated` | この値は `1` を返します。 |
| `_workfront.completed` | この値は `1` （オブジェクトが完了済みとマークされている場合） |
| `_workfront.parentObjectType` | （オプション）オブジェクトの親に対応するオブジェクトタイプ。 |
| `_workfront.parentID` | 親オブジェクトの ID。 |
| `_workfront.customData` | イベント中に入力されたすべてのカスタムフォームフィールドと値のマップ。 |

>[!IMPORTANT]
>
>イベントの一部として変更または作成された属性のみが入力されます。 例えば、オブジェクトの名前のみを変更した場合、次のようなフィールドにのみ値が入力されます。<ul><li>`timestamp`</li><li>`_workfront.update (=1)`</li><li>`_workfront.objectType`</li><li>`_workfront.objectID`</li><li>`_workfront.objectName`</li></ul>

## 次の手順

このチュートリアルに従って、データをWorkfrontからExperience Platformに取り込むためのデータフローを作成しました。 次のようなサービスを使用できます。 [クエリサービス](../../../../../query-service/home.md) を使用して、データに関する詳細な分析を実行します。 Workfrontについて詳しくは、 [Workfrontの概要](../../../../connectors/adobe-applications/workfront.md).