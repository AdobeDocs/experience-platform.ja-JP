---
keywords: Experience Platform;ID;ID サービス；トラブルシューティング；ガードレール；ガイドライン；制限；
title: ID サービスのガードレール
description: このドキュメントでは、ID グラフの使用を最適化するのに役立つ、ID サービスデータの使用とレート制限に関する情報を提供します。
exl-id: bd86d8bf-53fd-4d76-ad01-da473a1999ab
source-git-commit: f619bbf2c8d313eabc6444b4bd8c09615a00cc42
workflow-type: tm+mt
source-wordcount: '491'
ht-degree: 15%

---

# [!DNL Identity Service]データのガードレール

このドキュメントでは、の使用とレート制限に関する情報を提供します。 [!DNL Identity Service] id グラフの使用を最適化するのに役立つデータです。 次のガードレールを確認する際は、データが正しくモデル化されていることが前提になっています。データのモデル化方法に関するご質問は、カスタマーサービス担当者にお問い合わせください。

## はじめに

次のExperience Platformサービスは、ID データのモデリングに関係します。

* [ID](home.md)：Platform に取り込まれる際に、異なるデータソースからの ID を結び付けます。
* [[!DNL Real-Time Customer Profile]](../profile/home.md)：複数のソースからのデータを使用して、統合された消費者プロファイルを作成します。

## データモデルの上限

次の表は、静的制限のガードレールと、ID 名前空間で考慮する検証ルールに関するガイダンスを示しています。

### 静的制限

次の表に、ID データに適用される静的制限の概要を示します。

| ガードレール | 上限 | メモ |
| --- | --- | --- |
| グラフ内の ID 数 | 150 | 制限は、サンドボックスレベルで適用されます。 ID グラフは、制限に達すると更新されません。 **注意**:ID グラフの ID の最大数 **個々の結合プロファイルの** は 50 です。 50 個を超える ID を持つ ID グラフに基づく結合プロファイルは、リアルタイム顧客プロファイルから除外されます。 詳しくは、 [プロファイルデータのガードレール](../profile/guardrails.md). |
| XDM レコード内の ID の数 | 20 | 必要な XDM レコードの最小数は 2 です。 |
| カスタム名前空間の数 | なし | 作成できるカスタム名前空間の数に制限はありません。 |
| グラフの数 | なし | 作成できる ID グラフの数に制限はありません。 |
| 名前空間の表示名または ID 記号の文字数 | なし | 名前空間の表示名または ID 記号の文字数に制限はありません。 |

### ID 値の検証

次の表に、ID 値の検証を成功させるために従う必要がある既存のルールの概要を示します。

| 名前空間 | 検証ルール | ルール違反時のシステム動作 |
| --- | --- | --- |
| ECID | <ul><li>ECID の ID 値は 38 文字にする必要があります。</li><li>ECID の ID 値は、数字のみで構成する必要があります。</li></ul> | <ul><li>ECID の ID 値が 38 文字以外の場合、レコードはスキップされます。</li><li>ECID の ID 値に数字以外の文字が含まれている場合、レコードはスキップされます。</li></ul> |
| 非 ECID | ID 値は 1024 文字を超えることはできません。 | ID 値が 1024 文字を超える場合、レコードはスキップされます。 |

### ID 名前空間の取り込み

2023 年 3 月 31 日以降、ID サービスは、新規のお客様向けにAdobe Analytics ID(AAID) の取り込みをブロックします。 この ID は、通常、 [Adobe Analyticsソース](../sources/connectors/adobe-applications/analytics.md) そして [Adobe Audience Managerソース](../sources//connectors/adobe-applications/audience-manager.md) とは冗長です。ECID は同じ Web ブラウザーを表しているからです。 このデフォルト設定を変更する場合は、Adobeアカウントチームにお問い合わせください。

## 次の手順

詳しくは、次のドキュメントを参照してください。 [!DNL Identity Service]:

* [[!DNL Identity Service] の概要](home.md)
* [ID グラフビューア](ui/identity-graph-viewer.md)
