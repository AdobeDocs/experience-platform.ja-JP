---
title: レポートスイートデータ用のAdobe Analytics Source コネクタ
description: このドキュメントでは、Analytics の概要と、Analytics データのユースケースについて説明します。
exl-id: c4887784-be12-40d4-83bf-94b31eccdc2e
source-git-commit: d56a37c5b1c5768b3f6811be9d30d45628fdabca
workflow-type: tm+mt
source-wordcount: '1189'
ht-degree: 7%

---

# レポートスイートデータ用のAdobe Analytics ソースコネクタ

Adobe Experience Platformでは、Analytics ソースコネクタを通じてAdobe Analytics データを取り込むことができます。 [!DNL Analytics] ソースコネクタは、[!DNL Analytics] で収集されたデータをリアルタイムで Platform にストリーミングし、Platform で使用するために SCDS 形式の [!DNL Analytics] データを [!DNL Experience Data Model] （XDM）フィールドに変換します。

このドキュメントでは、[!DNL Analytics] の概要と [!DNL Analytics] データのユースケースについて説明します。

## Adobe Analytics と Analytics データ

[!DNL Analytics] は、顧客についての詳細や web プロパティとのやり取り、デジタルマーケティング支出が効果的な場所を確認、改善点を特定するのに役立つ強力なエンジンです。 [!DNL Analytics] では、年間何兆もの web トランザクションを処理します。また、[!DNL Analytics] ソースコネクタを使用すると、この豊富な行動データを簡単に利用し、数分で [!DNL Real-Time Customer Profile] ータを強化できます。

![Adobe Analyticsを含む様々なAdobeアプリケーションからのデータのジャーニーを示す図。](./images/analytics-data-experience-platform.png)

高いレベルでは、[!DNL Analytics] は世界中のさまざまなデジタルチャネルと複数のデータセンターからデータを収集します。 データが収集されると、訪問者の識別、セグメント化と変換のアーキテクチャ（VISTA）のルール、処理ルールが適用されて、受信データが形成されます。 生データがこの軽量な処理を経た後、[!DNL Real-Time Customer Profile] による消費の準備が整ったと見なされます。 前述と並行したプロセスでは、処理された同じデータがマイクロバッチ処理され、[!DNL Query Service] などのデータ検出アプリケーションで使用するために Platform データセットに取り込まれます。

処理ルールについて詳しくは、[ 処理ルールの概要 ](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/processing-rules/processing-rules.html?lang=ja) を参照してください。

## エクスペリエンスデータモデル（XDM）

XDM は公開ドキュメント化された仕様で、Experience Platform上のサービスとの通信に使用するアプリケーションの共通の構造と定義を提供します。

XDM 標準規格に準拠することで、データを統一的に取り込むことができ、データの配信と情報の収集が容易になります。

XDM について詳しくは、「[XDM システムの概要](../../../xdm/home.md)」を参照してください。

## Adobe Analytics から XDM へのフィールドのマッピング方法

>[!IMPORTANT]
>
>データ準備変換により、データフロー全体に待ち時間が追加される場合があります。 追加される追加の待ち時間は、変換ロジックの複雑さに応じて異なります。

Platform ユーザーインターフェイスを使用して [!DNL Analytics] データをExperience Platformに取り込むためのソース接続が確立されると、データフィールドは自動的にマッピングされ、数分で [!DNL Real-Time Customer Profile] に取り込まれます。 Platform UI を使用して [!DNL Analytics] とのソース接続を作成する手順については、[Analytics ソースコネクタのチュートリアル ](../../tutorials/ui/create/adobe-applications/analytics.md) を参照してください。

[!DNL Analytics] とExperience Platformの間で発生するフィールドマッピングについて詳しくは、[Adobe Analytics フィールドマッピング ](./mapping/analytics.md) ガイドを参照してください。

## Platform の Analytics データで予想される遅延はどのくらいですか。

Platform 上の Analytics データで予想される待ち時間の概要を次の表に示します。 待ち時間は、お客様の構成、データ量、および消費者アプリケーションによって異なります。 例えば、Analytics の実装が `A4T` で設定されている場合、パイプラインの遅延が 5 ～ 10 分に増えます。

| Analytics データ | 予想される遅延 |
| -------------- | ---------------- |
| [!DNL Real-Time Customer Profile] への新しいデータ （A4T **無効**） | &lt; 2 分 |
| [!DNL Real-Time Customer Profile] への新しいデータ （A4T **は** 有効） | 最大 30 分 |
| データレイクの新しいデータ | &lt; 2.25 時間 |
| [ ステッチ ](https://experienceleague.adobe.com/docs/analytics-platform/using/stitching/overview.html?lang=en) を行わずに新しいデータをCustomer Journey Analyticsに | &lt; 3.75 時間 |
| ステッチでCustomer Journey Analyticsする新しいデータ | &lt; 7 時間 |
| 100 億未満のイベントのバックフィル | &lt; 4 週間 |

Customer Journey Analyticsの待ち時間について詳しくは、[Customer Journey Analyticsガードレール ](https://experienceleague.adobe.com/docs/analytics-platform/using/cja-admin/guardrails.html?lang=en) を参照してください。

実稼動用サンドボックスの Analytics のバックフィルは、デフォルトで 13 か月です。 非実稼動用サンドボックス内の Analytics データの場合、バックフィルは 3 か月に設定されます。 上記の表で言及されている 100 億イベントの制限は、期待される待ち時間に厳密に関係します。

実稼動サンドボックスで Analytics ソースデータフローを作成すると、次の 2 つのデータフローが作成されます。

* データレイクへの履歴レポートスイートデータの 13 か月のバックフィルを行うデータフロー。 このデータフローは、バックフィルが完了すると終了します。
* ライブデータをデータレイクと [!DNL Real-Time Customer Profile] に送信するデータフローフロー。 このデータフローは継続的に実行されます。

>[!NOTE]
>
>Analytics のバックフィルデータは [!DNL Profile] に取り込まれないので、ライセンスプロファイルでは考慮されません。

## [!DNL Analytics] データのプライマリ識別情報

[!DNL Analytics] ソースコネクタからのすべてのヒットには、ECID と AAID のどちらが存在するかによって異なるプライマリ識別子が含まれます。 ECID がある場合、ECID がプライマリ識別子として指定されます。 AAID がある場合、AAID がプライマリとして指定されます。

次の表は、[!DNL Analytics] データの ID フィールドに関する詳細を示しています。

| ID フィールド | 説明 |
| --- | --- |
| AAID | AAID は、Adobe Analyticsのプライマリデバイス ID で、[!DNL Analytics] ソースを介して渡されるすべてのイベントに存在することが保証されます。 AAID は、*従来の Analytics ID* または `s_vi` Cookie ID と呼ばれることもあります。 ただし、AAID は、`s_vi` cookie が存在しなくても作成されます。 AAID は、[[!DNL Analytics]  データフィード ](https://experienceleague.adobe.com/docs/analytics/export/analytics-data-feed/data-feed-contents/datafeeds-reference.html) の `post_visid_high` 列と `post_visid_low` 列で表されます。 任意のイベントで、AAID フィールドには、[ID の操作の順序 ](https://experienceleague.adobe.com/docs/id-service/using/reference/analytics-reference/analytics-order-of-operations.html) で説明されている複数の異なるタイプの 1 つである可能性がある単一の ID が含ま  [!DNL Analytics]  ます。 **メモ**：レポートスイート全体では、AAID にはイベント間でタイプが混在する場合があります。 |
| ECID | ECID （Experience CloudID）は個別のデバイス ID フィールドで、Experience Cloud ID サービスを使用して実装さ [!DNL Analytics] ると、Adobe Analyticsで設定されます。 ECID は、MCID （Marketing CloudID）とも呼ばれます。 ECID がイベントに存在する場合、AAID は、Analytics [ 猶予期間 ](https://experienceleague.adobe.com/docs/id-service/using/reference/analytics-reference/grace-period.html) が設定されているかどうかに応じて、ECID に基づいている可能性があります。 ECID は、Analytics データフィードの `mcvisid` で表されます。 ECID について詳しくは、[ECID の概要 ](../../../identity-service/features/ecid.md) を参照してください。 ECID と [!DNL Analytics] の連携方法について詳しくは、[Analytics とExperience CloudID のリクエスト ](https://experienceleague.adobe.com/docs/id-service/using/reference/analytics-reference/legacy-analytics.html) のドキュメントを参照してください。 |
| AACUSTOMID | AACUSTOMID は、[!DNL Analytics] 実装の `s.VisitorID` 変数の使用に基づいて、Adobe Analyticsで設定される個別の ID フィールドです。 AACUSTOMID は、[[!DNL Analytics]  データフィード ](https://experienceleague.adobe.com/docs/analytics/export/analytics-data-feed/data-feed-contents/datafeeds-reference.html) の `cust_visid` 列で表されます。 AACUSTOMID が存在する場合、AAID は AACUSTOMID に基づきます。これは、[ID の操作の順序  [!DNL Analytics] ](https://experienceleague.adobe.com/docs/id-service/using/reference/analytics-reference/analytics-order-of-operations.html) で定義されているように、AACUSTOMID が他のすべての ID に優先されるためです。 |

### [!DNL Analytics] ソースによる ID の扱い方

[!DNL Analytics] ソースは、次のように、これらの ID を XDM フォームでExperience Platformに渡します。

* `endUserIDs._experience.aaid.id`
* `endUserIDs._experience.mcid.id`
* `endUserIDs._experience.aacustomid.id`

これらのフィールドは、ID としてマークされません。 代わりに、同じ ID （イベントに存在する場合）がキーと値のペアとして XDM の `identityMap` にコピーされます。

* `{ "key": "AAID", "value": [ { "id": "<identity>", "primary": <true or false> } ] }`
* `{ "key": "ECID", "value": [ { "id": "<identity>", "primary": <true or false> } ] }`
* `{ "key": "AACUSTOMID", "value": [ { "id": "<identity>", "primary": false } ] }`

ID が `identityMap` にコピーされると、`endUserIDs._experience.mcid.namespace.code` も同じイベントに設定されます。

* AAID が存在する場合、`endUserIDs._experience.aaid.namespace.code` は「AAID」に設定されます。
* ECID が存在する場合、`endUserIDs._experience.mcid.namespace.code` は「ECID」に設定されます。
* AACUSTOMID が存在する場合、`endUserIDs._experience.aacustomid.namespace.code` は「AACUSTOMID」に設定されます。

ID マップでは、ECID が存在する場合、イベントのプライマリ ID としてマークされます。 この場合、AAID は、[ID サービスの猶予期間 ](https://experienceleague.adobe.com/docs/id-service/using/reference/analytics-reference/grace-period.html) が原因で、ECID に基づいている可能性があります。 それ以外の場合、AAID は、イベントのプライマリ ID としてマークされます。 AACUSTOMID は、イベントのプライマリ ID としてマークされることはありません。 ただし、AACUSTOMID が存在する場合、AAID は、操作のExperience Cloud順序により AACUSTOMID に基づきます。

>[!NOTE]
>
>データ準備を使用すると、Analytics からのセカンダリ ID （AAID や AACUSTOMID など）をフィルタリングできます。 除外した場合、これらの ID は、受信する Analytics データで使用可能な場合、プロファイルには取り込まれません。 フィルタリングされていないデータは、引き続きデータレイクに読み込まれます。
