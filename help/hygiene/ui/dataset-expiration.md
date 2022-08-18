---
title: データセット有効期限の管理
description: Adobe Experience Platform UI でデータセットの有効期限をスケジュールする方法を説明します。
exl-id: 97db55e3-b5d6-40fd-94f0-2463fe041671
source-git-commit: 769c872d67141b4973b29a492e72c23491e2d1ae
workflow-type: tm+mt
source-wordcount: '408'
ht-degree: 47%

---

# データセット有効期限の管理

>[!IMPORTANT]
>
>Adobe Experience Platform のデータハイジーン機能は、現在、Healthcare Shield を購入した組織のみが利用できます。

この [[!UICONTROL データの衛生状態] workspace](./overview.md) Adobe Experience Platform UI では、データセットの有効期限をスケジュールできます。 データセットが有効期限に達すると、データレイク、ID サービス、リアルタイム顧客プロファイルは、別々のプロセスを開始し、各サービスからデータセットの内容を削除します。 3 つのサービスすべてからデータを削除すると、有効期限が完了とマークされます。

このドキュメントでは、Platform UI でデータセットの有効期限をスケジュールおよび管理する方法について説明します。

## データセットの有効期限のスケジュール設定

新しいリクエストを作成するには、ワークスペースのメインページから「**[!UICONTROL リクエストを作成]**」を選択します。

![「[!UICONTROL リクエストを作成]」ボタンが選択されていることを示す画像](../images/ui/ttl/create-request-button.png)

<!-- The request creation dialog appears. Under the **[!UICONTROL Action]** section, select **[!UICONTROL Dataset]** to update the available controls for dataset expiration scheduling-->

### 日付およびデータセットの選択

リクエスト作成ダイアログが表示されます。「**[!UICONTROL アクション]**」セクションで、データセットを削除する日付を選択します。手動で日付を入力（`mm/dd/yyyy` 形式）するか、カレンダーアイコン（![カレンダーアイコンの画像](../images/ui/ttl/calendar-icon.png)）を選択して、ダイアログから日付を選択します。

![データセットの有効期限が設定されていることを示す画像](../images/ui/ttl/select-date.png)

次に、「**[!UICONTROL データセットの詳細]**」で、データベースアイコン（![データベースアイコンの画像](../images/ui/ttl/database-icon.png)）を選択して、データセット選択ダイアログを開きます。有効期限を適用するデータセットをリストから選択し、「 」を選択します。 **[!UICONTROL 完了]**.

![データセットが選択されていることを示す画像](../images/ui/ttl/select-dataset.png)

>[!NOTE]
>
>現在のサンドボックスに属するデータセットのみが表示されます。

### リクエストの送信

データセットと有効期限を選択したら、 **[!UICONTROL 送信]**.

![「[!UICONTROL 送信]」ボタンが選択されていることを示す画像](../images/ui/ttl/submit.png)

データセットが削除される日付を確認するよう求められます。「**[!UICONTROL 送信]**」をクリックして続行します。

リクエストが送信されると、作業指示が作成され、[!UICONTROL データハイジーン]ワークスペースのメインタブに表示されます。ここから、リクエストを処理する作業指示のステータスを監視できます。

## データセットの有効期限の編集またはキャンセル

データセットの有効期限を編集またはキャンセルするには、 **[!UICONTROL データセット]** ワークスペースのメインページで、リストからデータセットの有効期限を選択します。

データセットの有効期限の詳細ページの右側のレールに、スケジュールされた削除を編集またはキャンセルするためのコントロールが表示されます。

## 次の手順

このドキュメントでは、Experience PlatformUI でデータセットの有効期限をスケジュールする方法を説明しました。 データ衛生 API を使用してデータセットの有効期限をスケジュールする方法については、 [データセット有効期限エンドポイントガイド](../api/dataset-expiration.md).
