---
title: データセット TTL の管理
description: Adobe Experience Platform UI でデータセットの有効期間 (TTL) をスケジュールする方法について説明します。
exl-id: 97db55e3-b5d6-40fd-94f0-2463fe041671
source-git-commit: 22da9e39e168d9a995c7c134733aa7a1b3587749
workflow-type: tm+mt
source-wordcount: '372'
ht-degree: 0%

---

# データセットの TTL の管理

>[!IMPORTANT]
>
>現在、Adobe Experience Platformのデータ衛生機能は、医療用Adobeシールドを購入した組織でのみ使用できます。

この [[!UICONTROL データの衛生状態] workspace](./overview.md) Adobe Experience Platform UI では、データセットの有効期間 (TTL) をスケジュールできます。

このドキュメントでは、Platform UI でデータセットの TTL をスケジュールおよび管理する方法について説明します。

## TTL のスケジュール設定

新しいリクエストを作成するには、「 **[!UICONTROL リクエストを作成]** をワークスペースのメインページから削除します。

![を示す画像 [!UICONTROL リクエストを作成] ボタンが選択されています](../images/ui/ttl/create-request-button.png)

<!-- The request creation dialog appears. Under the **[!UICONTROL Action]** section, select **[!UICONTROL Dataset]** to update the available controls for TTL scheduling-->

### 日付とデータセットを選択

リクエスト作成ダイアログが表示されます。 以下 **[!UICONTROL アクション]** 「 」セクションで、データセットを削除する日付を選択します。 日付は手動で（形式で）入力できます `mm/dd/yyyy`) またはカレンダーアイコン (![カレンダーアイコンの画像](../images/ui/ttl/calendar-icon.png)) をクリックして、ダイアログから日付を選択します。

![TTL に設定されている有効期限を示す画像](../images/ui/ttl/select-date.png)

次の、以下 **[!UICONTROL データセットの詳細]**、データベースアイコン (![データベースアイコンの画像](../images/ui/ttl/database-icon.png)) をクリックして、データセット選択ダイアログを開きます。 TTL を適用するデータセットをリストから選択し、「 」を選択します。 **[!UICONTROL 完了]**.

![選択されているデータセットを示す画像](../images/ui/ttl/select-dataset.png)

>[!NOTE]
>
>現在のサンドボックスに属するデータセットのみが表示されます。

### リクエストを送信

データセットと TTL 日を選択したら、 **[!UICONTROL 送信]**.

![を示す画像 [!UICONTROL 送信] ボタンが選択されています](../images/ui/ttl/submit.png)

データセットの削除日を確認するメッセージが表示されます。 選択 **[!UICONTROL 送信]** をクリックして続行します。

リクエストが送信されると、作業指示が作成され、 [!UICONTROL データの衛生状態] ワークスペース。 ここから、作業指示のステータスを監視して、要求を処理できます。

## TTL を編集またはキャンセルする

TTL を編集またはキャンセルするには、「 」を選択します **[!UICONTROL データセット]** ワークスペースのメインページで、リストから TTL を選択します。

TTL の詳細ページの右側のレールに、スケジュールされた削除を編集またはキャンセルするためのコントロールが表示されます。

## 次の手順

このドキュメントでは、Experience PlatformUI でデータセットの TTL をスケジュールする方法について説明しました。 データ衛生 API を使用してデータセットの TTL をスケジュールする方法については、 [データセット TTL エンドポイントガイド](../api/ttl.md).
