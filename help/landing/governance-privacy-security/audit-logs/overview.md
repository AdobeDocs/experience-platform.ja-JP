---
title: 監査ログの概要
description: 監査ログを使用して、Adobe Experience Platform で誰が何のアクションを実行したかを確認する方法を説明します。
exl-id: 00baf615-5b71-4e0a-b82a-ca0ce8566e7f
source-git-commit: ba190bdd1856b2d89fa28679eb7f09c258ddd17c
workflow-type: tm+mt
source-wordcount: '767'
ht-degree: 13%

---

# 監査ログ

Adobe Experience Platformでは、システムで実行されるアクティビティの透明性と可視性を高めるために、様々なサービスや機能のユーザーアクティビティを「監査ログ」の形式で監査できます。 これらのログは、Platform 上の問題のトラブルシューティングに役立つ監査証跡を形成し、企業のデータ管理ポリシーおよび規制要件に効果的に準拠するのに役立ちます。

基本的な意味では、監査ログは、 **who** 実行済み **what** アクションおよび **when**. ログに記録される各アクションには、アクションのタイプ、日時、アクションを実行したユーザーの E メール ID、アクションのタイプに関連する追加の属性を示すメタデータが含まれます。

このドキュメントでは、UI や API での表示および管理方法など、Platform の監査ログについて説明します。

## 監査ログによってキャプチャされるイベントタイプ {#category}

次の表に、監査ログによって記録されるリソースに対するアクションの概要を示します。

| リソース | アクション |
| --- | --- |
| [データセット](../../../catalog/datasets/overview.md) | <ul><li>選択からの    </li><li>更新</li><li>Delete</li><li>有効にする対象 [リアルタイム顧客プロファイル](../../../profile/home.md)</li><li>プロファイルに対して無効にする</li></ul> |
| [スキーマ](../../../xdm/schema/composition.md) | <ul><li>選択からの    </li><li>更新</li><li>削除</li><li>プロファイルに対して有効にする</li></ul> |
| [クラス](../../../xdm/schema/composition.md#class) | <ul><li>選択からの    </li><li>更新</li><li>削除</li></ul> |
| [フィールドグループ](../../../xdm/schema/composition.md#field-group) | <ul><li>選択からの    </li><li>更新</li><li>削除</li></ul> |
| [データタイプ](../../../xdm/schema/composition.md#data-type) | <ul><li>選択からの    </li><li>更新</li><li>削除</li></ul> |
| [サンドボックス](../../../sandboxes/home.md) | <ul><li>選択からの    </li><li>更新</li><li>リセット</li><li>削除</li></ul> |
| [宛先](../../../destinations/home.md) | <ul><li>選択からの    </li><li>更新</li><li>削除</li><li>有効にする</li><li>Disable（無効）</li><li>データセットの有効化</li><li>データセットの削除</li><li>プロファイルの有効化</li><li>プロファイルの削除</li></ul> |
| [セグメント](../../../segmentation/home.md) | <ul><li>選択からの    </li><li>削除</li><li>セグメントの有効化</li><li>セグメント削除</li></ul> |
| [結合ポリシー](../../../profile/merge-policies/overview.md) | <ul><li>選択からの    </li><li>更新</li><li>削除</li></ul> |
| [計算済み属性](../../../profile/computed-attributes/overview.md) | <ul><li>選択からの    </li><li>更新</li><li>削除</li></ul> |
| [製品プロファイル](../../../access-control/home.md) | <ul><li>選択からの    </li><li>更新</li><li>削除</li></ul> |
| [アカウント (Adobe)](../../../access-control/home.md) | <ul><li>選択からの    </li><li>更新</li><li>削除</li></ul> |
| [クエリテンプレート](../../../access-control/home.md) | <ul><li>選択からの    </li><li>更新</li><li>削除</li></ul> |
| [スケジュール済みクエリ](../../../access-control/home.md) | <ul><li>選択からの    </li><li>更新</li><li>削除</li></ul> |

## 監査ログへのアクセス

組織でこの機能を有効にすると、アクティビティの発生に応じて監査ログが自動的に収集されます。 ログ収集を手動で有効にする必要はありません。

監査ログを表示および書き出すには、 **[!UICONTROL ユーザーアクティビティログを表示]** アクセス制御権限が付与されている ( [!UICONTROL データガバナンス] カテゴリ ) です。 Platform 機能の個々の権限を管理する方法については、 [アクセス制御ドキュメント](../../../access-control/home.md).

## UI での監査ログの管理

監査ログは、 **[!UICONTROL 監査]** ワークスペース（Platform UI 内） ワークスペースには、デフォルトで、記録されたログのリストが最新のログから最新のログへと並べ替えて表示されます。

![監査ログダッシュボード](../../images/audit-logs/audits.png)

監査ログは、365 日間保持され、その後システムから削除されます。 したがって、元に戻せる期間は最大 365 日までです。

リストからイベントを選択すると、その詳細が右側のパネルに表示されます。

![イベントの詳細](../../images/audit-logs/select-event.png)

### 監査ログのフィルタリング

>[!NOTE]
>
>この新機能により、表示されるデータは 2022 年 3 月まで遡ります。 選択したリソースに応じて、2022 年 1 月より前のデータを利用できるようになります。


ファネルアイコン (![フィルターアイコン](../../images/audit-logs/icon.png)) をクリックして、結果を絞り込むのに役立つフィルターコントロールのリストを表示します。 選択した各種フィルターに関係なく、直近の 1000 件のレコードのみが表示されます。

![フィルター](../../images/audit-logs/filters.png)

UI の監査イベントには、次のフィルターを使用できます。

| Filter | 説明 |
| --- | --- |
| [!UICONTROL カテゴリ] | ドロップダウンメニューを使用して、表示される結果を次の項目でフィルタリングします。 [カテゴリ](#category). |
| [!UICONTROL アクション] | アクションでフィルターします。 現在のみ [!UICONTROL 作成] および [!UICONTROL 削除] アクションはフィルタリングできます。 |
| [!UICONTROL ユーザー] | 完全なユーザー ID を入力します ( 例： `johndoe@acme.com`) を使用して、ユーザーでフィルタリングできます。 |
| [!UICONTROL ステータス] | アクションが許可（完了）されたか、または不足により拒否されたかによってフィルターします [アクセス制御](../../../access-control/home.md) 権限。 |
| [!UICONTROL 日付] | 結果をフィルターする日付範囲を定義する開始日または終了日を選択します。 データは 90 日間のルックバック期間で書き出すことができます ( 例：2021-12-15 ～ 2022-03-15)。 これは、イベントタイプによって異なる場合があります。 |

フィルターを削除するには、該当するフィルターのピルアイコンの「X」を選択するか、「 **[!UICONTROL すべてクリア]** をクリックして、すべてのフィルターを削除します。

![フィルターをクリア](../../images/audit-logs/clear-filters.png)

### 監査ログの書き出し

監査ログの現在のリストを書き出すには、 **[!UICONTROL ログをダウンロード]**.

![ログをダウンロード](../../images/audit-logs/download.png)

表示されるダイアログで、目的の形式 ( **[!UICONTROL CSV]** または **[!UICONTROL JSON]**) を選択し、「 **[!UICONTROL ダウンロード]**. ブラウザーは、生成されたファイルをダウンロードし、コンピューターに保存します。

![ダウンロード形式を選択](../../images/audit-logs/select-download-format.png)

## API での監査ログの管理

UI で実行できるすべてのアクションは、API 呼び出しを使用して実行することもできます。 詳しくは、 [API リファレンスドキュメント](https://www.adobe.io/experience-platform-apis/references/audit-query/) を参照してください。

## Adobe Admin Consoleの監査ログの管理

Adobe Admin Consoleでアクティビティの監査ログを管理する方法については、次を参照してください。 [文書](https://helpx.adobe.com/enterprise/using/audit-logs.html).

## 次の手順と追加のリソース

このガイドでは、監査ログをExperience Platformで管理する方法を説明します。 Platform アクティビティの監視方法について詳しくは、 [Observability Insights](../../../observability/home.md) および [データ取得の監視](../../../ingestion/quality/monitor-data-ingestion.md).

Experience Platformの監査ログに関する理解を深めるには、次のビデオをご覧ください。

>[!VIDEO](https://video.tv.adobe.com/v/341450?quality=12&learn=on)
