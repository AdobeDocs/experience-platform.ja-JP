---
title: プライバシーコンソールの概要
description: Adobe Experience Platform UI でプライバシー関連のワークフローを監視する方法について説明します。
feature: Privacy
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '422'
ht-degree: 4%

---

# プライバシーコンソールの概要

「[!UICONTROL  プライバシーコンソール ]」タブでは、Adobe Experience Platform UI で、データハイジーン作業指示、Experience Platformからのデータ主体のリクエスト、Experience Platform ユーザーアクティビティの監査ログなど、Privacy Serviceのプライバシー関連操作を示すダッシュボードビューが提供されます。

ダッシュボードにアクセスするには、左側のナビゲーションで **[!UICONTROL プライバシーコンソール]** を選択します。

![Experience Platform UI 内の左側のナビゲーションで [!UICONTROL  プライバシーコンソール ] が選択されていることを示す画像 ](../images/governance-privacy-security/privacy-console/left-nav.png)

ここから、コンソールに用意されている [ モニタリングウィジェット ](#widgets) を確認したり、Experience Platformのプライバシー機能に関する [ ユースケースガイド ](#use-case-guides) の 1 つを参照したりできます。

## ウィジェット

[!UICONTROL  プライバシーコンソール ] には、プライバシー操作の監視に役立つ複数のウィジェットが用意されています。

![Experience Platform UI 内の左側のナビゲーションで [!UICONTROL  プライバシーコンソール ] が選択されていることを示す画像 ](../images/governance-privacy-security/privacy-console/widgets.png)

>[!NOTE]
>
>組織が購入した SKU によっては、以下に説明するウィジェットの一部が使用できない場合があります。

各ウィジェットの機能は次のとおりです。

| ウィジェット名 | 説明 |
| --- | --- |
| データハイジーン作業指示ステータス | 選択した時間枠の [ データハイジーン ](../../hygiene/home.md) のレコード削除プロセスのステータスを表示します。 ドロップダウンメニューを使用して、過去 7 日間、14 日間、30 日間の時間枠を変更します。 |
| 最近のデータハイジーン作業指示 | システムによって処理されている最新の [ データハイジーン ](../../hygiene/home.md) 作業指示を表示します。 ドロップダウンを使用して、最近作成した作業指示と最近更新した作業指示を切り替えます。 |
| 最も頻度の高いアクション | 選択した期間に [ 監査ログ ](./audit-logs/overview.md) でキャプチャされたExperience Platformに関する最も頻繁なアクションを示します。 ドロップダウンメニューを使用して、過去 7 日間、14 日間、30 日間の時間枠を変更します。 |
| 最もアクティブなユーザー | 選択した期間、[ 監査ログ ](./audit-logs/overview.md) で記録された、組織内で最もアクティブなExperience Platform ユーザーを表示します。 ドロップダウンメニューを使用して、過去 7 日間、14 日間、30 日間の時間枠を変更します。 |
| データ主体のリクエスト | 特定の日に [Privacy Service](../../privacy-service/home.md) によって送信され、完了したデータ主体の要求の数を示します。 |

{style="table-layout:auto"}

## ユースケースガイド

コンソールには、Experience Platformの一般的なプライバシーワークフローを紹介する複数の製品内ガイドが用意されています。これには、基本的な実装のセットアップ方法に関する簡潔な手順も含まれます。

![Experience Platform UI 内の左側のナビゲーションで [!UICONTROL  プライバシーコンソール ] が選択されていることを示す画像 ](../images/governance-privacy-security/privacy-console/use-case-guide.png)

## 次の手順

プライバシーのユースケースに結び付く様々なExperience Platform サービスについて詳しくは、次のドキュメントを参照してください。

* [属性ベースのアクセス制御](../../access-control/abac/overview.md)
* [監査ログ](./audit-logs/overview.md)
* [データガバナンス](../../data-governance/home.md)
* [データハイジーン](../../hygiene/home.md)
* [Privacy Service](../../privacy-service/home.md)
