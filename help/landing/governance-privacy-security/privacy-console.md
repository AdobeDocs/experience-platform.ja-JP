---
title: プライバシーコンソールの概要
description: Adobe Experience Platform UI でプライバシー関連のワークフローを監視する方法について説明します。
feature: Privacy
source-git-commit: c0eb5b5c3a1968cae2bc19b7669f70a97379239b
workflow-type: tm+mt
source-wordcount: '412'
ht-degree: 4%

---

# プライバシーコンソールの概要

[!UICONTROL  プライバシーコンソール ] タブにあるAdobe Experience Platform UI では、データハイジーン作業指示、ユーザーからのデータ主体のリクエスト、Platform Privacy Serviceアクティビティの監査ログなど、Platform のプライバシー関連操作を示すダッシュボードビューを利用できます。

ダッシュボードにアクセスするには、左側のナビゲーションで **[!UICONTROL プライバシーコンソール]** を選択します。

![Platform UI の左側のナビゲーションで [!UICONTROL  プライバシーコンソール ] が選択されていることを示す画像 ](../images/governance-privacy-security/privacy-console/left-nav.png)

ここから、コンソールに用意されている [ 監視ウィジェット ](#widgets) を確認したり、Platform のプライバシー機能について複数の [ ユースケースガイド ](#use-case-guides) の 1 つを参照したりできます。

## ウィジェット

[!UICONTROL  プライバシーコンソール ] には、プライバシー操作の監視に役立つ複数のウィジェットが用意されています。

![Platform UI の左側のナビゲーションで [!UICONTROL  プライバシーコンソール ] が選択されていることを示す画像 ](../images/governance-privacy-security/privacy-console/widgets.png)

>[!NOTE]
>
>組織が購入した SKU によっては、以下に説明するウィジェットの一部が使用できない場合があります。

各ウィジェットの機能は次のとおりです。

| ウィジェット名 | 説明 |
| --- | --- |
| データハイジーン作業指示ステータス | 選択した時間枠の [ データハイジーン ](../../hygiene/home.md) のレコード削除プロセスのステータスを表示します。 ドロップダウンメニューを使用して、過去 7 日間、14 日間、30 日間の時間枠を変更します。 |
| 最近のデータハイジーン作業指示 | システムによって処理されている最新の [ データハイジーン ](../../hygiene/home.md) 作業指示を表示します。 ドロップダウンを使用して、最近作成した作業指示と最近更新した作業指示を切り替えます。 |
| 最も頻度の高いアクション | 選択した期間に [ 監査ログ ](./audit-logs/overview.md) でキャプチャされた、Platform での最も頻繁なアクションを表示します。 ドロップダウンメニューを使用して、過去 7 日間、14 日間、30 日間の時間枠を変更します。 |
| 最もアクティブなユーザー | 選択した時間枠、[ 監査ログ ](./audit-logs/overview.md) によって記録された、組織内で最もアクティブな Platform ユーザーを表示します。 ドロップダウンメニューを使用して、過去 7 日間、14 日間、30 日間の時間枠を変更します。 |
| データ主体のリクエスト | 特定の日に [Privacy Service](../../privacy-service/home.md) が送信し、完了したデータ主体の要求の数を示します。 |

{style="table-layout:auto"}

## ユースケースガイド

コンソールには、Platform の一般的なプライバシーワークフローを紹介するいくつかの製品内ガイドが用意されています。これには、基本的な実装のセットアップ方法に関する簡潔な手順も含まれています。

![Platform UI の左側のナビゲーションで [!UICONTROL  プライバシーコンソール ] が選択されていることを示す画像 ](../images/governance-privacy-security/privacy-console/use-case-guide.png)

## 次の手順

プライバシーのユースケースに結び付く様々な Platform サービスについて詳しくは、次のドキュメントを参照してください。

* [属性ベースのアクセス制御](../../access-control/abac/overview.md)
* [監査ログ](./audit-logs/overview.md)
* [データガバナンス](../../data-governance/home.md)
* [データハイジーン](../../hygiene/home.md)
* [Privacy Service](../../privacy-service/home.md)
