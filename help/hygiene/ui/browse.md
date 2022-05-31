---
title: データ衛生作業指示の参照
description: Adobe Experience Platformユーザーインターフェイスで既存のデータ衛生作業指示を表示および管理する方法について説明します。
exl-id: 76d4a809-cc2c-434d-90b1-23d88f29c022
source-git-commit: c24aa700eb425770266bbee5c187e2e87b15a9ac
workflow-type: tm+mt
source-wordcount: '413'
ht-degree: 1%

---

# データの衛生作業指示を参照 {#browse-work-orders}

>[!CONTEXTUALHELP]
>id="platform_hygiene_workorders"
>title="作業指示 ID"
>abstract="データの衛生要求がシステムに送信されると、要求されたタスクを実行する作業指示が作成されます。 つまり、作業指示は、特定のデータの衛生処理を表し、現在のステータスやその他の関連する詳細が含まれます。 各作業指示は、作成時に自動的に固有の ID を割り当てます。"
>text="See the data hygiene UI guide to learn more."

>[!IMPORTANT]
>
>現在、Adobe Experience Platformのデータ衛生機能は、医療用Adobeシールドを購入した組織でのみ使用できます。

データの衛生要求がシステムに送信されると、要求されたタスクを実行する作業指示が作成されます。 作業指示は、データセットの有効期限 (TTL) など、特定のデータの衛生処理を表します。この中には、現在のステータスやその他の関連する詳細が含まれます。

このガイドでは、Adobe Experience Platform UI で既存の作業指示を表示および管理する方法について説明します。

## 既存の作業指示のリストとフィルタリング

最初に **[!UICONTROL データの衛生状態]** UI のワークスペースには、既存の作業指示のリストと基本的な詳細が表示されます。

![を示す画像 [!UICONTROL データの衛生状態] Platform UI のワークスペース](../images/ui/browse/work-order-list.png)

<!-- The list only shows work orders for one category at a time. Select **[!UICONTROL Consumer]** to view a list of consumer deletion tasks, and **[!UICONTROL Dataset]** to view a list of time-to-live (TTL) schedules for datasets.

![Image showing the [!UICONTROL Dataset] tab](../images/ui/browse/dataset-tab.png) -->

ファネルアイコン (![ファネルアイコンの画像](../images/ui/browse/funnel-icon.png)) をクリックして、表示されている作業指示のフィルタのリストを表示します。

![表示された作業指示フィルターの画像](../images/ui/browse/filters.png)

| Filter | 説明 |
| --- | --- |
| [!UICONTROL ステータス] | 作業指示の現在のステータスに基づいてフィルタリングします。 |
| [!UICONTROL 作成日] | データセットの TTL リクエストがおこなわれた日時に基づいてフィルタリングします。 |
| [!UICONTROL 削除日] | TTL がスケジュールした削除日に基づいてフィルタリングします。 |
| [!UICONTROL 更新日] | データセットの TTL が最後に更新された日時に基づいてフィルタリングします。 TTL の作成と有効期限は更新としてカウントされます。 |

{style=&quot;table-layout:auto&quot;}

## 作業指示の詳細の表示

リストに表示されている作業指示の ID を選択して、詳細を表示します。

![選択されている作業指示 ID を示す画像](../images/ui/browse/select-work-order.png)

<!-- Depending on the type of work order selected, different information and controls are provided. These are covered in the sections below.

### Consumer delete details

>[!CONTEXTUALHELP]
>id="platform_hygiene_responsemessages"
>title="Consumer delete response"
>abstract="When a consumer deletion process receives a response from the system, these messages are displayed under the **[!UICONTROL Result]** section. If a problem occurs while a work order is processing, any relevant error messages will appear in this section to help you troubleshoot the issue. To learn more, see the data hygiene UI guide."


The details of a consumer delete request are read-only, displaying its basic attributes such as its current status and the time elapsed since the request was made.

![Image showing the details page for a consumer delete work order](../images/ui/browse/consumer-delete-details.png)

### Dataset TTL details -->

データセット TTL の詳細ページには、削除が発生する前の日数の予定有効期限など、基本属性に関する情報が表示されます。 右側のレールでは、コントロールを使用して TTL を編集またはキャンセルできます。

![データセットの TTL 作業指示の詳細ページを示す画像](../images/ui/browse/ttl-details.png)

## 次の手順

このガイドでは、Platform UI で既存のデータ衛生作業指示を表示および管理する方法について説明します。 独自の作業指示の作成について詳しくは、 [データセットの TTL のスケジュール設定](./ttl.md).
