---
title: データセット TTL の管理
description: Adobe Experience Platform UI でデータセットの Time to Live（TTL）をスケジュール設定する方法を説明します。
exl-id: 97db55e3-b5d6-40fd-94f0-2463fe041671
source-git-commit: 7f1e4bdf54314cab1f69619bcbb34216da94b17e
workflow-type: tm+mt
source-wordcount: '370'
ht-degree: 95%

---

# データセット TTL の管理

>[!IMPORTANT]
>
>現在、Adobe Experience Platformのデータ衛生機能は、Healthcare Shield を購入した組織でのみ利用できます。

Adobe Experience Platform UI の[[!UICONTROL データハイジーン]ワークスペース](./overview.md)を使用すると、データセットの Time to Live（TTL）をスケジュール設定できます。

このドキュメントでは、Platform UI のデータセット TTL をスケジュール設定および管理する方法を説明します。

## TTL のスケジュール設定

新しいリクエストを作成するには、ワークスペースのメインページから「**[!UICONTROL リクエストを作成]**」を選択します。

![「[!UICONTROL リクエストを作成]」ボタンが選択されていることを示す画像](../images/ui/ttl/create-request-button.png)

<!-- The request creation dialog appears. Under the **[!UICONTROL Action]** section, select **[!UICONTROL Dataset]** to update the available controls for TTL scheduling-->

### 日付およびデータセットの選択

リクエスト作成ダイアログが表示されます。「**[!UICONTROL アクション]**」セクションで、データセットを削除する日付を選択します。手動で日付を入力（`mm/dd/yyyy` 形式）するか、カレンダーアイコン（![カレンダーアイコンの画像](../images/ui/ttl/calendar-icon.png)）を選択して、ダイアログから日付を選択します。

![TTL の有効期限が設定されていることを示す画像](../images/ui/ttl/select-date.png)

次に、「**[!UICONTROL データセットの詳細]**」で、データベースアイコン（![データベースアイコンの画像](../images/ui/ttl/database-icon.png)）を選択して、データセット選択ダイアログを開きます。リストから TTL を適用するデータセットを選択して、「**[!UICONTROL 完了]**」を選択します。

![データセットが選択されていることを示す画像](../images/ui/ttl/select-dataset.png)

>[!NOTE]
>
>現在のサンドボックスに属するデータセットのみが表示されます。

### リクエストの送信

データセットと TTL の日付を選択したら、「**[!UICONTROL 送信]**」を選択します。

![「[!UICONTROL 送信]」ボタンが選択されていることを示す画像](../images/ui/ttl/submit.png)

データセットが削除される日付を確認するよう求められます。「**[!UICONTROL 送信]**」をクリックして続行します。

リクエストが送信されると、作業指示が作成され、[!UICONTROL データハイジーン]ワークスペースのメインタブに表示されます。ここから、リクエストを処理する作業指示のステータスを監視できます。

## TTL の編集またはキャンセル

TTL を編集またはキャンセルするには、ワークスペースのメインページで&#x200B;**[!UICONTROL データセット]**&#x200B;を選択して、リストから TTL を選択します。

TTL の詳細ページで、右側のパネルにスケジュール設定された削除を編集またはキャンセルするためのコントロールが表示されます。

## 次の手順

このドキュメントでは、Experience Platform UI でデータセット TTL をスケジュール設定する方法について説明しました。Data Hygiene API を使用したデータセット TTL のスケジュール設定方法については、[データセット TTL エンドポイントガイド](../api/ttl.md)を参照してください。
