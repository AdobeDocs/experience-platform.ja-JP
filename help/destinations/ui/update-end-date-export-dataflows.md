---
title: データセット書き出しデータフローの終了日を更新します（2025 年 5 月 1 日（PT）に対応が必要です）
type: Tutorial
hide: true
hidefromtoc: true
description: 現在の終了日を 2025 年 5 月 1 日（PT）として、データセット書き出しデータフローの終了日を更新する方法を説明します。
source-git-commit: aeabbb56002f8640b79ff3a7e3dc532d01ebbadf
workflow-type: tm+mt
source-wordcount: '385'
ht-degree: 0%

---


# データセット書き出しデータフローの終了日を更新します（2025 年 5 月 1 日（PT）に対応が必要です）

>[!IMPORTANT]
>
>このページのアクション項目は、2024 年 9 月リリースのExperience Platformより前に、組織でデータセット書き出しデータフローを設定した場合に適用されます。

## 何が起きているのでしょうか？

Experience Platformの [2024 年 9 月リリース &#x200B;](/help/release-notes/latest/latest.md#destinations) では、データセットデータフローの書き出し `endTime` 日を設定するオプションが導入されました。 Adobeでは、（2024 年 9 月リリースより前に *作成されたすべてのデータセット書き出しデータフローのデフォルト終了日が 2025 年 5 月 1 日（PT* に導入されました。 これらのデータフローでは、現在、以下に示すものに類似したメッセージが表示されます。

![&#x200B; データセット書き出しデータフローの終了日を更新する必要があるという UI 通知。](/help/destinations/assets/ui/export-datasets/update-end-date.png)

**アクション項目**：これらのデータフローの場合、有効期限が切れる前に終了日を手動で更新する必要があります。そうしないと、書き出しが停止します。 Experience Platform UI を使用して、どのデータフローが 2025 年 5 月 1 日（PT）に停止するように設定されているかを特定します。

## なぜ私に通知されるのですか。

組織は、終了日が 2025 年 5 月 1 日（PT）のアクティブなデータセット書き出しデータフローを持つと識別されました。

## UI を使用して終了日を更新する

Experience Platform UI を使用して、終了日が 2025 年 5 月 1 日（PT）のデータフローを識別し、将来の日付に更新します。

### 更新が必要なデータフローを見つける

**宛先/参照** に移動し、以下に示すように、**データタイプ** 列で **データセット** データタイプを探します。 目的のデータフローを選択して、検査します。

![&#x200B; 「参照」タブでハイライト表示されたデータセット書き出しデータフロー。](/help/destinations/assets/ui/export-datasets/view-dataset-dataflows.png)

### データフローの終了日の更新

データフローの終了日を更新するには：

1. 前の手順で検査用に選択したデータフローで、「**データセットを書き出し**」を選択します。
   ![&#x200B; 「参照」タブでハイライト表示されたデータセットコントロールの書き出し。](/help/destinations/assets/ui/export-datasets/export-datasets-control-highlighted.png)
2. ワークフローで、「**スケジュール**」ステップに進み、「**スケジュールを編集**」を選択します。
   ![&#x200B; スケジュール設定ステップでハイライト表示されたスケジュール管理を編集 &#x200B;](/help/destinations/assets/ui/export-datasets/edit-schedule-control-highlighted.png)
3. 希望する終了日を 2025 年 5 月 1 日（PT）以降に選択し、「**保存**」を選択します。
   ![&#x200B; スケジュール設定ステップでハイライト表示されている終了日制御を選択 &#x200B;](/help/destinations/assets/ui/export-datasets/select-end-date.png)
4. ワークフローの最後に進み、更新内容を保存します。

スケジュール設定ステップについて詳しくは、[&#x200B; データセット UI の書き出しチュートリアル &#x200B;](/help/destinations/api/export-datasets.md#scheduling) を参照してください。

## API を使用した終了日の更新

### 更新が必要なデータフローを見つける

### データフローの終了日の更新
