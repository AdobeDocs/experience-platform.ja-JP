---
title: データセット有効期限の管理
description: Adobe Experience Platform UI でデータセットの有効期限をスケジュールする方法を説明します。
exl-id: 97db55e3-b5d6-40fd-94f0-2463fe041671
source-git-commit: 5a12c75a54f420b2ca831dbfe05105dfd856dc4d
workflow-type: tm+mt
source-wordcount: '438'
ht-degree: 93%

---

# データセット有効期限の管理

>[!IMPORTANT]
>
>Adobe Experience Platform のデータハイジーン機能は、現在、Healthcare Shield を購入した組織のみが利用できます。

Adobe Experience Platform UI の[[!UICONTROL データハイジーン]ワークスペース](./overview.md)では、データセットの有効期限をスケジュールできます。データセットが有効期限に達すると、データレイク、ID サービス、リアルタイム顧客プロファイルは、別々のプロセスを開始し、各サービスからデータセットの内容を削除します。 3 つのサービスすべてからデータを削除すると、有効期限が完了とマークされます。

>[!WARNING]
>
>データセットの有効期限が切れるように設定されている場合、ダウンストリームワークフローに悪影響が及ばないように、データをそのデータセットに取り込む可能性があるデータフローを手動で変更する必要があります。

このドキュメントでは、Platform UI のデータセット有効期限をスケジュール設定および管理する方法を説明します。

## データセット有効期限のスケジュール設定

新しいリクエストを作成するには、ワークスペースのメインページから「**[!UICONTROL リクエストを作成]**」を選択します。

![「[!UICONTROL リクエストを作成]」ボタンが選択されていることを示す画像](../images/ui/ttl/create-request-button.png)

<!-- The request creation dialog appears. Under the **[!UICONTROL Action]** section, select **[!UICONTROL Dataset]** to update the available controls for dataset expiration scheduling-->

### 日付およびデータセットの選択

リクエスト作成ダイアログが表示されます。「**[!UICONTROL アクション]**」セクションで、データセットを削除する日付を選択します。手動で日付を入力（`mm/dd/yyyy` 形式）するか、カレンダーアイコン（![カレンダーアイコンの画像](../images/ui/ttl/calendar-icon.png)）を選択して、ダイアログから日付を選択します。

![データセットの有効期限が設定されていることを示す画像](../images/ui/ttl/select-date.png)

次に、「**[!UICONTROL データセットの詳細]**」で、データベースアイコン（![データベースアイコンの画像](../images/ui/ttl/database-icon.png)）を選択して、データセット選択ダイアログを開きます。リストから有効期限を適用するデータセットを選択して、「**[!UICONTROL 完了]**」を選択します。

![データセットが選択されていることを示す画像](../images/ui/ttl/select-dataset.png)

>[!NOTE]
>
>現在のサンドボックスに属するデータセットのみが表示されます。

### リクエストの送信

データセットと有効期限を選択したら、「**[!UICONTROL 送信]**」を選択します。

![「[!UICONTROL 送信]」ボタンが選択されていることを示す画像](../images/ui/ttl/submit.png)

データセットが削除される日付を確認するよう求められます。「**[!UICONTROL 送信]**」をクリックして続行します。

リクエストが送信されると、作業指示が作成され、[!UICONTROL データハイジーン]ワークスペースのメインタブに表示されます。ここから、リクエストを処理する作業指示のステータスを監視できます。

## データセット有効期限の編集またはキャンセル

データセットの有効期限を編集またはキャンセルするには、ワークスペースのメインページで&#x200B;**[!UICONTROL データセット]**&#x200B;を選択して、リストからデータセットの有効期限を選択します。

データセット有効期限の詳細ページで、右側のパネルにスケジュール設定された削除を編集またはキャンセルするためのコントロールが表示されます。

## 次の手順

このドキュメントでは、Experience Platform UI でデータセットの有効期限をスケジュール設定する方法について説明しました。Data Hygiene API を使用したデータセット有効期限のスケジュール設定方法については、[データセット有効期限のエンドポイントガイド](../api/dataset-expiration.md)を参照してください。
