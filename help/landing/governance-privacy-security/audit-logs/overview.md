---
title: 監査ログの概要
description: 監査ログを使用して、Adobe Experience Platformで誰が何のアクションを実行したかを確認する方法を説明します。
source-git-commit: 937225ff08e2e02c5840f86d6ed50644e05bdfe5
workflow-type: tm+mt
source-wordcount: '463'
ht-degree: 4%

---

# 監査ログ（ベータ版）

>[!IMPORTANT]
>
>Adobe Experience Platformの監査ログ機能は、現在ベータ版です。 このドキュメントで説明する機能は、変更される場合があります。

Adobe Experience Platformでは、システムで実行されるアクティビティの透明性と可視性を高めるために、様々なサービスや機能のユーザーアクティビティを「監査ログ」の形式で監査できます。 これらのログは、Platformに関する問題のトラブルシューティングに役立つ監査記録を形成し、企業のデータ管理ポリシーおよび規制要件に効果的に対応するのに役立ちます。

基本的には、監査ログは、誰が&#x200B;****&#x200B;何&#x200B;**アクションを実行したか、および**&#x200B;いつ&#x200B;**を実行したかを**&#x200B;に伝えます。 ログに記録される各アクションには、アクションのタイプ、日時、アクションを実行したユーザーのEメールID、アクションのタイプに関連する追加の属性を示すメタデータが含まれます。

このドキュメントでは、UIやAPIでのログの表示方法や管理方法など、Platformの監査ログについて説明します。

## 監査ログによってキャプチャされるイベントタイプ

次の表に、監査ログによって記録されるリソースに対するアクションの概要を示します。

| リソース | アクション |
| --- | --- |
| [サンドボックス](../../../sandboxes/home.md) | <ul><li>選択からの    </li><li>更新</li><li>リセット</li><li>Delete</li></ul> |
| [データセット](../../../catalog/datasets/overview.md) | <ul><li>選択からの    </li><li>更新</li><li>削除</li><li>[リアルタイム顧客プロファイル](../../../profile/home.md)を有効にする</li></ul> |
| [スキーマ](../../../xdm/schema/composition.md) | <ul><li>選択からの    </li><li>更新</li><li>削除</li></ul> |
| [[フィールド]領域](../../../xdm/schema/composition.md#field-group) | <ul><li>選択からの    </li><li>更新</li><li>削除</li></ul> |
| [宛先](../../../destinations/home.md) | <ul><li>アクティブ化</li></ul> |

## 監査ログへのアクセス

組織でこの機能を有効にすると、アクティビティの発生に応じて監査ログが自動的に収集されます。 手動でログ収集を有効にする必要はありません。

監査ログを表示およびエクスポートするには、「監査ログの表示」アクセス制御権限が付与されている必要があります。 Platform機能の個々の権限を管理する方法については、[アクセス制御に関するドキュメント](../../../access-control/home.md)を参照してください。

## UIでの監査ログの管理

Platform UIの&#x200B;**[!UICONTROL 監査]**&#x200B;ワークスペース内で、様々なExperience Platform機能の監査ログを表示できます。 ワークスペースには、記録されたログのリストがデフォルトで最新のログから最新のログに並べ替えて表示されます。

![監査ログダッシュボード](../../images/audit-logs/audits.png)

システムには、昨年の監査ログのみが表示されます。 この制限を超えるログは自動的にシステムから削除されます。

リストからイベントを選択し、右側のパネルに詳細を表示します。

![イベントの詳細](../../images/audit-logs/select-event.png)

<!-- (Planned for post-beta release)
### Export an audit log

Select **[!UICONTROL Download log]** to export an audit log.
-->

## APIでの監査ログの管理

UIで実行できるすべてのアクションは、API呼び出しを使用して実行することもできます。 詳しくは、[APIリファレンスドキュメント](https://www.adobe.io/experience-platform-apis/references/audit-query/)を参照してください。

## Adobe Admin Consoleの監査ログの管理

Adobe Admin Consoleでアクティビティの監査ログを管理する方法については、次の[ドキュメント](https://helpx.adobe.com/enterprise/using/audit-logs.html)を参照してください。

## 次の手順

このガイドでは、監査ログをExperience Platformで管理する方法について説明します。 Platformアクティビティの監視方法について詳しくは、[観察性インサイト](../../../observability/home.md)および[データ取得の監視](../../../ingestion/quality/monitor-data-ingestion.md)に関するドキュメントを参照してください。