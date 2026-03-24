---
title: データセット書き出しデータフローの終了日を更新します（2025年5月1日までに必要なアクション）
type: Tutorial
hide: true
hidefromtoc: true
description: データセット書き出しデータフローの終了日を、現在の終了日である2025年5月1日に更新する方法について説明します。
exl-id: 3f8ff535-3c54-47ac-b297-32f8298881db
source-git-commit: d946d3dbb09c1fe0163fba3a892b4c0f1b331f87
workflow-type: tm+mt
source-wordcount: '385'
ht-degree: 0%

---

# データセット書き出しデータフローの終了日を更新します（2025年5月1日までに必要なアクション）

>[!IMPORTANT]
>
>このページのアクション項目は、2024年9月リリースのExperience Platformより前にデータセット書き出しデータフローを設定した場合に適用されます。

## 何が起きているのでしょうか？ {#what-is-happening}

2024年9月[&#x200B; リリースのExperience Platform](/help/release-notes/latest/latest.md#destinations)では、データセットのデータフローを書き出す`endTime`日付を設定するオプションが導入されました。 Adobeでは、2024年9月リリース *より前に作成されたすべてのデータセット書き出しデータフローに対して、2025年5月1日のデフォルトの終了日も導入されました。*&#x200B;これらのデータフローは、現在、次に示すメッセージと同様のメッセージを表示します。

データセットの書き出しデータフローの終了日を更新する必要性に関する![UI通知。](/help/destinations/assets/ui/export-datasets/update-end-date.png)

**アクション項目**：これらのデータフローのいずれかで、終了日が期限切れになる前に手動で更新する必要があります。そうしないと、書き出しが停止します。 Experience Platform UIを使用して、2025年5月1日に停止するように設定されているデータフローを特定します。

## なぜ通知されるのか？ {#why-notified}

お客様の組織は、2025年5月1日の終了日で、アクティブなデータセット書き出しデータフローを持つことが確認されました。

## UIを使用して終了日を更新する {#use-ui}

Experience Platform UIを使用して、終了日が2025年5月1日のデータフローを特定し、将来の日付に更新します。

### 更新が必要なデータフローを見つける {#find-dataflows}

次に示すように、**宛先/参照**&#x200B;に移動し、**データタイプ**&#x200B;列でデータタイプ **データセット**&#x200B;を探します。 目的のデータフローを選択して検査します。

![参照タブでハイライト表示されたデータセット書き出しデータフロー。](/help/destinations/assets/ui/export-datasets/view-dataset-dataflows.png)

### データフローの終了日の更新 {#update-end-date}

データフローの終了日を更新するには：

1. 前の手順で検査用に選択したデータフローで、「**データセットを書き出し**」を選択します。
   ![参照タブでハイライト表示されたデータセットの書き出しコントロール。](/help/destinations/assets/ui/export-datasets/export-datasets-control-highlighted.png)
2. ワークフローで、**スケジュール** ステップに進み、**スケジュールを編集**&#x200B;を選択します。
   ![&#x200B; スケジュール管理の編集は、スケジュール設定ステップでハイライト表示されています。](/help/destinations/assets/ui/export-datasets/edit-schedule-control-highlighted.png)
3. 2025年5月1日より後に終了日を選択し、**保存**&#x200B;を選択します。
   ![&#x200B; スケジュール設定ステップでハイライト表示された終了日コントロールを選択します。](/help/destinations/assets/ui/export-datasets/select-end-date.png)
4. ワークフローの最後に進み、更新を保存します。

スケジュール設定ステップの詳細については、[&#x200B; データセットの書き出しUI チュートリアル &#x200B;](/help/destinations/api/export-datasets.md#export-datasets-by-using-the)を参照してください。

## APIを使用して終了日を更新する {#use-api}

### 更新が必要なデータフローを見つける {#find-dataflows-api}

### データフローの終了日の更新 {#update-end-date-api}
