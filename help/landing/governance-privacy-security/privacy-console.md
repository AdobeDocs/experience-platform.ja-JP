---
title: プライバシーコンソールの概要
description: Adobe Experience Platform UI でプライバシー関連のワークフローを監視する方法について説明します。
source-git-commit: 4fa1b826d033ace6536af920b070e8eebbbf401c
workflow-type: tm+mt
source-wordcount: '406'
ht-degree: 3%

---

# プライバシーコンソールの概要

この [!UICONTROL プライバシーコンソール] 「 Adobe Experience Platform UI 」タブには、データ衛生作業指示、Privacy Serviceからのデータ主体リクエスト、Platform ユーザーアクティビティの監査ログなど、Platform でのプライバシー関連操作のダッシュボードビューが表示されます。

ダッシュボードにアクセスするには、「 」を選択します。 **[!UICONTROL プライバシーコンソール]** をクリックします。

![画像表示 [!UICONTROL プライバシーコンソール] Platform UI 内の左側のナビゲーションで選択されている状態](../images/governance-privacy-security/privacy-console/left-nav.png)

ここから、利用可能な [ウィジェットの監視](#widgets) コンソールで提供されるか、次のいずれかを参照できます。 [使用例ガイド](#use-case-guides) Platform のプライバシー機能に関する情報です。

## ウィジェット

[!UICONTROL プライバシーコンソール] は、プライバシー操作を監視するのに役立ついくつかのウィジェットを提供します。

![画像表示 [!UICONTROL プライバシーコンソール] Platform UI 内の左側のナビゲーションで選択されている状態](../images/governance-privacy-security/privacy-console/widgets.png)

>[!NOTE]
>
>組織が購入した SKU によっては、以下に挙げる一部のウィジェットが使用できない場合があります。

各ウィジェットの機能について、以下で説明します。

| ウィジェット名 | 説明 |
| --- | --- |
| データ衛生作業指示ステータス | 以下の消費者削除プロセスのステータスを表示します [データの衛生状態](../../hygiene/home.md) 選択した時間枠に設定します。 ドロップダウンメニューを使用して、過去 7 日間、14 日間、30 日間の期間を変更します。 |
| 最近のデータ衛生作業指示 | 最新の [データの衛生状態](../../hygiene/home.md) システムで処理される作業指示。 ドロップダウンを使用して、最近作成した作業指示と最近更新した作業指示を切り替えます。 |
| 最も頻繁に実行されるアクション | でキャプチャされた、Platform で最も頻繁に実行されるアクションを表示します。 [監査ログ](./audit-logs/overview.md) 選択した時間枠に設定します。 ドロップダウンメニューを使用して、過去 7 日間、14 日間、30 日間の期間を変更します。 |
| 最もアクティブなユーザー | 組織内で最もアクティブな Platform ユーザーを、次の条件でキャプチャして表示します。 [監査ログ](./audit-logs/overview.md) 選択した時間枠に設定します。 ドロップダウンメニューを使用して、過去 7 日間、14 日間、30 日間の期間を変更します。 |
| データ主体のリクエスト | 送信され、完了したデータ主体の要求の数を [Privacy Service](../../privacy-service/home.md) ある日に |

{style=&quot;table-layout:auto&quot;}

## 使用例ガイド

コンソールには、Platform の一般的なプライバシーワークフローを紹介する製品内ガイドがいくつか用意されています。このガイドには、基本的な実装の設定方法に関する簡潔な説明が含まれています。

![画像表示 [!UICONTROL プライバシーコンソール] Platform UI 内の左側のナビゲーションで選択されている状態](../images/governance-privacy-security/privacy-console/use-case-guide.png)

## 次の手順

プライバシーの使用例に結び付く様々な Platform サービスについて詳しくは、次のドキュメントを参照してください。

* [属性ベースのアクセス制御](../../access-control/abac/overview.md)
* [監査ログ](./audit-logs/overview.md)
* [データガバナンス](../../data-governance/home.md)
* [データの衛生状態](../../hygiene/home.md)
* [Privacy Service](../../privacy-service/home.md)
