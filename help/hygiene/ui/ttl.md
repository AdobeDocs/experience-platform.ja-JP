---
title: データセット TTL の管理
description: Adobe Experience Platform UI でデータセットの有効期間 (TTL) をスケジュールする方法について説明します。
source-git-commit: 931b847761e649696aa8433d53233593efd4d1ee
workflow-type: tm+mt
source-wordcount: '412'
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

リクエスト作成ダイアログが表示されます。 以下 **[!UICONTROL アクション]** セクション、選択 **[!UICONTROL データセット]** :TTL スケジュール設定に使用可能なコントロールを更新します。

![を示す画像 [!UICONTROL データセット] 選択したオプション](../images/ui/ttl/create-request-button.png)

### 日付とデータセットを選択

以下 **[!UICONTROL アクション]** 「 」セクションで、データセットを削除する日付を選択します。 日付は手動で（形式で）入力できます `mm/dd/yyyy`) またはカレンダーアイコン (![カレンダーアイコンの画像](../images/ui/ttl/calendar-icon.png)) をクリックして、ダイアログから日付を選択します。

![TTL に設定されている有効期限を示す画像](../images/ui/ttl/select-date.png)

次の、以下 **[!UICONTROL データセットの詳細]**、データベースアイコン (![データベースアイコンの画像](../images/ui/ttl/database-icon.png)) をクリックして、データセット選択ダイアログを開きます。 TTL を適用するデータセットをリストから選択し、「 」を選択します。 **[!UICONTROL 完了]**.

![選択されているデータセットを示す画像](../images/ui/ttl/select-dataset.png)

>[!NOTE]
>
>現在のサンドボックスに属するデータセットのみが表示されます。

### リクエストを送信

データセットと TTL 日を選択したら、 **[!UICONTROL 送信]**.

![を示す画像 [!UICONTROL 送信] ボタンが選択されています](../images/ui/ttl/select-dataset.png)

データセットの削除日を確認するメッセージが表示されます。 選択 **[!UICONTROL 送信]** をクリックして続行します。

リクエストが送信されると、作業指示が作成され、 [!UICONTROL 消費者] タブ [!UICONTROL データの衛生状態] ワークスペース。 ここから、作業指示のステータスを監視して、要求を処理できます。

## TTL を編集またはキャンセルする

TTL を編集またはキャンセルするには、「 」を選択します **[!UICONTROL データセット]** ワークスペースのメインページで、リストから TTL を選択します。

TTL の詳細ページの右側のレールに、スケジュールされた削除を編集またはキャンセルするためのコントロールが表示されます。

## 次の手順

このドキュメントでは、Experience PlatformUI でデータセットの TTL をスケジュールする方法について説明しました。 UI でのその他のデータ衛生タスクの実行方法について詳しくは、 [データ衛生 UI の概要](./overview.md).

データ衛生 API を使用してデータセットの TTL をスケジュールする方法については、 [データセット TTL エンドポイントガイド](../api/ttl.md).
