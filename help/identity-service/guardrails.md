---
keywords: Experience Platform;ID;ID サービス；トラブルシューティング；ガードレール；ガイドライン；制限；
title: ID サービスのガードレール
description: このドキュメントでは、ID グラフの使用を最適化するのに役立つ、ID サービスデータの使用とレート制限に関する情報を提供します。
exl-id: bd86d8bf-53fd-4d76-ad01-da473a1999ab
source-git-commit: 614fc9af8c774a1f79d0ab52527e32b2381487fa
workflow-type: tm+mt
source-wordcount: '1233'
ht-degree: 53%

---

# [!DNL Identity Service]データのガードレール

このドキュメントでは、ID グラフの使用を最適化するのに役立つ [!DNL Identity Service] データの使用とレート制限について説明します。次のガードレールを確認する際は、データが正しくモデル化されていることが前提になっています。データのモデル化方法に関するご質問は、カスタマーサービス担当者にお問い合わせください。

## はじめに

ID データのモデリングには、次の Experience Platform サービスが関係しています。

* [ID](home.md)：様々なデータソースの ID が Platform に取り込まれる際に、それらの ID を結び付けます。
* [[!DNL Real-Time Customer Profile]](../profile/home.md)：複数のソースのデータを使用して、統合された消費者プロファイルを作成します。

## データモデルの上限

静的上限のガードレールと、ID 名前空間で考慮すべき検証ルールに関するガイダンスを次の表に示します。

### 静的上限

ID データに適用される静的上限の概要を次の表に示します。

| ガードレール | 上限 | メモ |
| --- | --- | --- |
| グラフ内の ID の数 | 50 | 50 個の ID がリンクされたグラフが更新されると、ID サービスは、「先入れ先出し」メカニズムを適用し、最も古い ID を削除して、最新の ID 用の領域を確保します。削除は、ID タイプとタイムスタンプに基づいて行われます。上限は、サンドボックスレベルで適用されます。詳しくは、 [削除ロジックについて](#deletion-logic). |
| 単一のバッチ取り込みでの ID へのリンク数 | 50 | 1 つのバッチに、望ましくないグラフ結合の原因となる、異常な ID が含まれている場合があります。 これを防ぐために、ID サービスは、既に 50 個以上の ID にリンクされている ID を取り込みません。 |
| XDM レコード内の ID 数 | 20 | 最低限必要な XDM レコード数は 2 です。 |
| カスタム名前空間の数 | なし | 作成できるカスタム名前空間の数に制限はありません。 |
| 名前空間の表示名または ID 記号の文字数 | なし | 名前空間の表示名または ID 記号の文字数に制限はありません。 |

### ID 値の検証

ID 値の検証を成功させるために従う必要がある既存のルールの概要を次の表に示します。

| 名前空間 | 検証ルール | ルール違反時のシステムの動作 |
| --- | --- | --- |
| ECID | <ul><li>ECID の ID 値はちょうど 38 文字にする必要があります。</li><li>ECID の ID 値は、数字のみで構成する必要があります。</li><li>ID 値は、「null」、「anonymous」、「invalid」、または空の文字列にできません（例：「&quot;, &quot;&quot;, &quot;&quot;」）。</li></ul> | <ul><li>ECID の ID 値がちょうど 38 文字でない場合、そのレコードはスキップされます。</li><li>ECID の ID 値に数字以外の文字が含まれている場合、そのレコードはスキップされます。</li><li>ID の取り込みがブロックされます。</li></ul> |
| ECID 以外 | ID 値は 1024 文字を超えることはできません。 | ID 値が 1024 文字を超える場合、そのレコードはスキップされます。 |

### ID 名前空間の取り込み

2023年3月31日（PT）以降、ID サービスは、新規のお客様について Adobe Analytics ID (AAID) の取り込みをブロックします。この ID は、通常、[Adobe Analytics ソース](../sources/connectors/adobe-applications/analytics.md)と [Adobe Audience Manager ソース](../sources//connectors/adobe-applications/audience-manager.md)を通じて取り込まれ、ECID が同じ web ブラウザーを表すため冗長です。このデフォルト設定を変更する場合は、Adobe アカウントチームにお問い合わせください。

## 容量の ID グラフが更新された場合の削除ロジックの理解 {#deletion-logic}

完全な ID グラフが更新されると、ID サービスは、最新の ID を追加する前に、グラフ内の最も古い ID を削除します。これは、ID データの正確性と関連性を維持するためです。この削除プロセスは、次の 2 つの主なルールに従います。

### ルール #1：削除は、名前空間の ID タイプに基づいて優先付けされる

削除の優先順位は次のとおりです。

1. Cookie ID
2. デバイス ID
3. クロスデバイス ID、メール、電話

### ルール #2：削除は、ID に保存されたタイムスタンプに基づいて行われる

グラフにリンクされた各 ID には、対応する独自のタイムスタンプがあります。完全なグラフが更新されると、ID サービスは最も古いタイムスタンプを持つ ID を削除します。

完全なグラフが新しい ID で更新されると、これら 2 つのルールが連携して、削除する古い ID が指定されます。ID サービスは、まず最も古い Cookie ID、次に最も古いデバイス ID、最後に最も古いクロスデバイス IDやメール、電話の順に削除します。

>[!NOTE]
>
>削除するように指定された ID がグラフ内で他の複数の ID にリンクしている場合、その ID を接続するリンクも削除されます。

>[!BEGINSHADEBOX]

**削除ロジックを視覚的に表現したもの**

![最新の ID を受け入れるために削除される最も古い ID の例](./images/graph-limits-v3.png)

*図の注意：*

* `t` = タイムスタンプ.
* タイムスタンプの値は、指定した ID の最新性に対応します。 例： `t1` は、最初にリンクされた id（古い ID）を表し、 `t51` は、最新のリンクされた id を表します。

この例では、左側のグラフを新しい ID で更新する前に、ID サービスは、最も古いタイムスタンプを持つ既存の ID を最初に削除します。 ただし、最も古い ID はデバイス ID なので、ID サービスは、削除の優先順位リストの上位のタイプの名前空間（この場合は `ecid-3`）に到達するまで、その ID をスキップします。削除の優先順位が上位のタイプの最も古い ID が削除されると、グラフは新しいリンク `ecid-51` で更新されます。

* まれに、タイムスタンプと ID タイプが同じ 2 つの ID がある場合、ID サービスは、 [XID](./api/list-native-id.md) 削除を実行します。

>[!ENDSHADEBOX]

### 実装に対する影響

以下の節では、削除ロジックが ID サービス、リアルタイム顧客プロファイル、WebSDK に与える影響について説明します。

#### ID サービス：カスタム名前空間の ID タイプの変更

実稼働用サンドボックスに次の情報が含まれている場合は、Adobeのアカウントチームに連絡して、ID タイプの変更をリクエストしてください。

* ユーザー識別子（CRM ID など）を cookie/デバイス ID タイプとして設定するカスタム名前空間。
* Cookie/デバイスの識別子をクロスデバイス ID タイプとして設定するカスタム名前空間。

この機能を使用できるようになると、ID 数の上限である 50 個を超えるグラフは、最大 50 個の ID にまで減らされます。 Real-Time CDP B2C Edition では、以前はセグメント化とアクティベーションから無視されていたので、オーディエンスの資格を持つプロファイルの数が最小限に増える可能性がありました。

#### リアルタイム顧客プロファイル：アドレス可能なオーディエンスへの影響

削除は、リアルタイム顧客プロファイルではなく、ID サービス内のデータに対してのみ発生します。

* その結果、ECID が ID グラフの一部ではなくなったので、この動作により、1 つの ECID でより多くのプロファイルが作成される可能性があります。
* アドレス可能なオーディエンスの権利付与番号の範囲内に留まるには、を有効にすることをお勧めします。 [偽名プロファイルデータの有効期限](../profile/pseudonymous-profiles.md) をクリックして、古いプロファイルを削除します。

#### リアルタイム顧客プロファイルおよび WebSDK:プライマリID の削除

CRM ID に対する認証済みイベントを保持する場合は、プライマリ ID を ECID から CRM ID に変更することをお勧めします。 この変更の実装手順については、次のドキュメントを参照してください。

* [Experience Platformタグの ID マップの設定](../tags/extensions/client/web-sdk/data-element-types.md#identity-map).
* [Experience PlatformWeb SDK の ID データ](../edge/identity/overview.md#using-identitymap)



## 次の手順

[!DNL Identity Service] について詳しくは、次のドキュメントを参照してください。

* [[!DNL Identity Service] の概要](home.md)
* [ID グラフビューア](ui/identity-graph-viewer.md)

その他のExperience Platformサービスガードレール、エンドツーエンドの遅延情報、およびReal-Time CDP製品説明ドキュメントのライセンス情報の詳細については、次のドキュメントを参照してください。

* [Real-Time CDP Guardrails](/help/rtcdp/guardrails/overview.md)
* [エンドツーエンドの待ち時間図](https://experienceleague.adobe.com/docs/blueprints-learn/architecture/architecture-overview/deployment/guardrails.html?lang=en#end-to-end-latency-diagrams) 様々なExperience Platformサービス。
* [Real-time Customer Data Platform（B2C 版 — プライムパッケージおよび究極パッケージ）](https://helpx.adobe.com/jp/legal/product-descriptions/real-time-customer-data-platform-b2c-edition-prime-and-ultimate-packages.html)
* [Real-time Customer Data Platform（B2P — プライムおよび究極のパッケージ）](https://helpx.adobe.com/legal/product-descriptions/real-time-customer-data-platform-b2p-edition-prime-and-ultimate-packages.html)
* [Real-time Customer Data Platform（B2B — プライムおよび究極のパッケージ）](https://helpx.adobe.com/legal/product-descriptions/real-time-customer-data-platform-b2b-edition-prime-and-ultimate-packages.html)
