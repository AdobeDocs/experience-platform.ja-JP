---
title: データ衛生作業指示の参照
description: Adobe Experience Platformユーザーインターフェイスで既存のデータ衛生作業指示を表示および管理する方法について説明します。
exl-id: 76d4a809-cc2c-434d-90b1-23d88f29c022
source-git-commit: 18ef55a084ced8c26e583598f9016dd9f4741153
workflow-type: tm+mt
source-wordcount: '555'
ht-degree: 2%

---

# データの衛生作業指示を参照

>[!CONTEXTUALHELP]
>id="platform_hygiene_workorders"
>title="作業指示 ID"
>abstract="データの衛生要求がシステムに送信されると、要求されたタスクを実行する作業指示が作成されます。 つまり、作業指示は、特定のデータの衛生処理を表し、現在のステータスやその他の関連する詳細が含まれます。 各作業指示は、作成時に自動的に固有の ID を割り当てます。 詳しくは、『データ衛生 UI ガイド』を参照してください。"

>[!IMPORTANT]
>
>現在、Adobe Experience Platformのデータ衛生機能は、医療用Adobeシールドを購入した組織でのみ使用できます。

データの衛生要求がシステムに送信されると、要求されたタスクを実行する作業指示が作成されます。 作業指示は、特定のデータの衛生処理（消費者データの削除など）を表し、現在のステータスやその他の関連する詳細が含まれます。

このガイドでは、Adobe Experience Platform UI で既存の作業指示を表示および管理する方法について説明します。

## 既存の作業指示のリストとフィルタリング

最初に **[!UICONTROL データの衛生状態]** UI のワークスペースには、既存の作業指示のリストと基本的な詳細が表示されます。

![を示す画像 [!UICONTROL データの衛生状態] Platform UI のワークスペース](../images/ui/browse/work-order-list.png)

リストには、一度に 1 つのカテゴリの作業指示のみが表示されます。 選択 **[!UICONTROL 消費者]** 消費者削除タスクのリストを表示するには、次の手順に従います。 **[!UICONTROL データセット]** ：データセットの有効期間 (TTL) スケジュールのリストを表示します。

![を示す画像 [!UICONTROL データセット] タブ](../images/ui/browse/dataset-tab.png)

ファネルアイコン (![ファネルアイコンの画像](../images/ui/browse/funnel-icon.png)) をクリックして、表示されている作業指示のフィルタのリストを表示します。

![表示された作業指示フィルターの画像](../images/ui/browse/filters.png)

表示しているタブに応じて、様々なフィルターを使用できます。

| Filter | カテゴリ | 説明 |
| --- | --- | --- |
| [!UICONTROL ステータス] | [!UICONTROL 消費者] &amp; [!UICONTROL データセット] | 作業指示の現在のステータスに基づいてフィルタリングします。 |
| [!UICONTROL 作成日] | [!UICONTROL 消費者] | 消費者の削除リクエストがおこなわれた日時に基づいてフィルターします。 |
| [!UICONTROL 作成日] | [!UICONTROL データセット] | 消費者の削除リクエストがおこなわれた日時に基づいてフィルターします。 |
| [!UICONTROL 削除日] | [!UICONTROL データセット] | TTL がスケジュールした削除日に基づいてフィルタリングします。 |
| [!UICONTROL 更新日] | [!UICONTROL データセット] | データセットの TTL が最後に更新された日時に基づいてフィルタリングします。 TTL の作成と有効期限は更新としてカウントされます。 |

{style=&quot;table-layout:auto&quot;}

## 作業指示の詳細の表示

リストに表示されている作業指示の ID を選択して、詳細を表示します。

![選択されている作業指示 ID を示す画像](../images/ui/browse/select-work-order.png)

選択した作業指示のタイプに応じて、異なる情報とコントロールが表示されます。 これらは以下の節で説明します。

### 消費者削除の詳細

<!-- (Not available for initial release)
>[!CONTEXTUALHELP]
>id="platform_hygiene_responsemessages"
>title="Consumer delete response"
>abstract="When a consumer deletion process receives a response from the system, these messages are displayed under the **[!UICONTROL Result]** section. If a problem occurs while a work order is processing, any relevant error messages will appear in this section to help you troubleshoot the issue. To learn more, see the data hygiene UI guide."
-->

消費者の削除リクエストの詳細は読み取り専用で、現在のステータスやリクエストがおこなわれてから経過した時間などの基本属性が表示されます。

![消費者削除作業指示の詳細ページを示す画像](../images/ui/browse/consumer-delete-details.png)

### データセットの TTL の詳細

データセット TTL の詳細ページには、削除が発生する前の日数の予定有効期限など、基本属性に関する情報が表示されます。 右側のレールでは、コントロールを使用して TTL を編集またはキャンセルできます。

![データセットの TTL 作業指示の詳細ページを示す画像](../images/ui/browse/ttl-details.png)

## 次の手順

このガイドでは、Platform UI で既存のデータ衛生作業指示を表示および管理する方法について説明します。 独自の作業指示の作成について詳しくは、次のドキュメントを参照してください。

* [消費者の削除](./delete-consumer.md)
* [データセットの TTL のスケジュール設定](./ttl.md)
