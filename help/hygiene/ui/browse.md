---
title: データハイジーン作業指示の参照
description: Adobe Experience Platform ユーザーインターフェイスでの既存のデータハイジーン作業指示の表示および管理方法を説明します。
exl-id: 76d4a809-cc2c-434d-90b1-23d88f29c022
source-git-commit: e57b5ec6c6234d4d1fe22f8d03c70d6bd9c02f0f
workflow-type: tm+mt
source-wordcount: '484'
ht-degree: 76%

---

# データハイジーン作業指示の参照 {#browse-work-orders}

>[!CONTEXTUALHELP]
>id="platform_hygiene_workorders"
>title="作業指示 ID"
>abstract="データハイジーンリクエストがシステムに送信されると、リクエストされたタスクを実行するための作業指示が作成されます。つまり、作業指示は、特定のデータハイジーン処理を表し、その現在のステータスおよび他の関連する詳細が含まれます。各作業指示は、作成時に独自の一意の ID が自動的に割り当てられます。"
>text="See the data hygiene UI guide to learn more."

>[!IMPORTANT]
>
>Adobe Experience Platform のデータハイジーン機能は、現在、Healthcare Shield を購入した組織のみが利用できます。

データハイジーンリクエストがシステムに送信されると、リクエストされたタスクを実行するための作業指示が作成されます。作業指示は、データセットのスケジュールされた有効期間（TTL）など、特定のデータハイジーンプロセスを表します。これには、現在のステータスやその他の関連する詳細が含まれます。

このガイドでは、Adobe Experience Platform UI での既存の作業指示の表示および管理方法を説明します。

## 既存の作業指示のリスト表示とフィルタリング

UI で最初に&#x200B;**[!UICONTROL データハイジーン]**&#x200B;ワークスペースにアクセスすると、既存の作業指示のリストが、基本的な情報と共に表示されます。

![Platform UI の[!UICONTROL データハイジーン]ワークスペースを示す画像](../images/ui/browse/work-order-list.png)

<!-- The list only shows work orders for one category at a time. Select **[!UICONTROL Consumer]** to view a list of consumer deletion tasks, and **[!UICONTROL Dataset]** to view a list of time-to-live (TTL) schedules for datasets.

![Image showing the [!UICONTROL Dataset] tab](../images/ui/browse/dataset-tab.png) -->

ファネルアイコン（![ファネルアイコンの画像](../images/ui/browse/funnel-icon.png)）を選択すると、表示された作業指示のフィルターのリストが表示されます。

![表示された作業指示フィルターの画像](../images/ui/browse/filters.png)

| フィルター | 説明 |
| --- | --- |
| [!UICONTROL ステータス] | 作業指示の現在のステータスに基づいてフィルタリングします。<ul><li>**[!UICONTROL 完了]**:ジョブが完了しました。</li><li>**[!UICONTROL 保留中]**:ジョブは作成されましたが、まだ実行されていません。 A [データセット有効期間 (TTL) リクエスト](./ttl.md) は、このステータスが、予定されている削除日より前にあると仮定します。 削除日に達すると、ステータスは [!UICONTROL 実行中] 事前にジョブがキャンセルされている場合を除きます。</li><li>**[!UICONTROL 実行中]**:ジョブが開始され、現在処理中です。</li><li>**[!UICONTROL キャンセル]**:ジョブは、手動のユーザーリクエストの一環としてキャンセルされました。</li></ul> |
| [!UICONTROL 作成日] | 作業指示が行われた日時に基づいてフィルタリングします。 |
| [!UICONTROL 有効期限] | 対象のデータセットに対して予定されている削除日に基づいて TTL リクエストをフィルタリングします。 |
| [!UICONTROL 更新日] | 作業指示が最後に更新された日時に基づいて TTL 要求をフィルタリングします。 TTL の作成と有効期限切れは、更新としてカウントされます。 |

{style=&quot;table-layout:auto&quot;}

## 作業指示の詳細の表示

リストされた作業指示の ID を選択して、そのの詳細を表示します。

![作業指示 ID が選択されていることを示す画像](../images/ui/browse/select-work-order.png)

<!-- Depending on the type of work order selected, different information and controls are provided. These are covered in the sections below.

### Consumer delete details

>[!CONTEXTUALHELP]
>id="platform_hygiene_responsemessages"
>title="Consumer delete response"
>abstract="When a consumer deletion process receives a response from the system, these messages are displayed under the **[!UICONTROL Result]** section. If a problem occurs while a work order is processing, any relevant error messages will appear in this section to help you troubleshoot the issue. To learn more, see the data hygiene UI guide."


The details of a consumer delete request are read-only, displaying its basic attributes such as its current status and the time elapsed since the request was made.

![Image showing the details page for a consumer delete work order](../images/ui/browse/consumer-delete-details.png)

### Dataset TTL details -->

データセット TTL の詳細ページは、その基本属性に関する情報を提供します（削除が発生するまでの残り日数に対してスケジュール設定された有効期限など）。右側のパネルで、TTL を編集またはキャンセルするためのコントロールを使用できます。

![データセット TTL 作業指示の詳細ページを示す画像](../images/ui/browse/ttl-details.png)

## 次の手順

このガイドでは、Platform UI での既存のデータハイジーン作業指示の表示および管理方法について説明しました。独自の作業指示の作成について詳しくは、[データセット TTL のスケジュール設定](./ttl.md)を参照してください。
