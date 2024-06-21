---
title: レポートスイートデータ用のAdobe Analytics ソースコネクタ
description: このドキュメントでは、Analytics の概要と、Analytics データのユースケースについて説明します。
exl-id: c4887784-be12-40d4-83bf-94b31eccdc2e
source-git-commit: d56a37c5b1c5768b3f6811be9d30d45628fdabca
workflow-type: tm+mt
source-wordcount: '1189'
ht-degree: 7%

---

# レポートスイートデータ用のAdobe Analytics ソースコネクタ

Adobe Experience Platformでは、Analytics ソースコネクタを通じてAdobe Analytics データを取り込むことができます。 この [!DNL Analytics] ソースコネクタは、で収集されたデータをストリーミングします。 [!DNL Analytics] scds 形式に変換してリアルタイムで Platform に送信 [!DNL Analytics] データ対象 [!DNL Experience Data Model] Platform で使用する（XDM）フィールド。

このドキュメントでは、の概要を説明します [!DNL Analytics] のユースケースについて説明します [!DNL Analytics] データ。

## Adobe Analytics と Analytics データ

[!DNL Analytics] は、顧客についての詳細や web プロパティとのやり取り、デジタルマーケティング費用が効果的な場所を確認、改善点を特定するのに役立つ強力なエンジンです。 [!DNL Analytics] 年間数兆の web トランザクションを処理し、 [!DNL Analytics] ソースコネクタを使用すると、この豊富な行動データを簡単に利用して、 [!DNL Real-Time Customer Profile] ほんの数分で。

![Adobe Analyticsを含む様々なAdobeアプリケーションからのデータのジャーニーを示す図。](./images/analytics-data-experience-platform.png)

大まかには [!DNL Analytics] 世界中のさまざまなデジタルチャネルと複数のデータセンターからデータを収集します。 データが収集されると、訪問者の識別、セグメント化と変換のアーキテクチャ（VISTA）のルール、処理ルールが適用されて、受信データが形成されます。 生データがこの軽量な処理を経た後、次の条件により、消費可能と見なされます。 [!DNL Real-Time Customer Profile]. 前述と並行したプロセスでは、処理された同じデータがマイクロバッチ化され、Platform データセットに取り込まれて、次のユーザーが使用します [!DNL Query Service]、およびその他のデータ検出アプリケーション。

を参照してください。 [処理ルールの概要](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/processing-rules/processing-rules.html?lang=ja) 処理ルールについて詳しくは、こちらを参照してください。

## エクスペリエンスデータモデル（XDM）

XDM は公開ドキュメント化された仕様で、Experience Platform上のサービスとの通信に使用するアプリケーションの共通の構造と定義を提供します。

XDM 標準規格に準拠することで、データを統一的に取り込むことができ、データの配信と情報の収集が容易になります。

XDM について詳しくは、「[XDM システムの概要](../../../xdm/home.md)」を参照してください。

## Adobe Analytics から XDM へのフィールドのマッピング方法

>[!IMPORTANT]
>
>データ準備変換により、データフロー全体に待ち時間が追加される場合があります。 追加される追加の待ち時間は、変換ロジックの複雑さに応じて異なります。

取り込むためのソース接続が確立された場合 [!DNL Analytics] platform ユーザーインターフェイスを使用したExperience Platformへのデータの取り込み。データフィールドは自動的にマッピングされ、に取り込まれます。 [!DNL Real-Time Customer Profile] 数分以内。 を使用したソース接続の作成手順については、を参照してください [!DNL Analytics] platform UI を使用する場合は、を参照してください。 [Analytics ソースコネクタのチュートリアル](../../tutorials/ui/create/adobe-applications/analytics.md).

次の間に発生するフィールドマッピングについて詳しくは、 [!DNL Analytics] とExperience Platformについては、を参照してください。 [Adobe Analytics フィールドマッピング](./mapping/analytics.md) ガイド。

## Platform の Analytics データで予想される遅延はどのくらいですか。

Platform 上の Analytics データで予想される待ち時間の概要を次の表に示します。 待ち時間は、お客様の構成、データ量、および消費者アプリケーションによって異なります。 例えば、Analytics の実装が `A4T` で設定されている場合、パイプラインの遅延が 5 ～ 10 分に増えます。

| Analytics データ | 予想される遅延 |
| -------------- | ---------------- |
| 新しいデータ： [!DNL Real-Time Customer Profile] （A4T **ではない** 有効） | &lt; 2 分 |
| 新しいデータ： [!DNL Real-Time Customer Profile] （A4T **等しい** 有効） | 最大 30 分 |
| データレイクの新しいデータ | &lt; 2.25 時間 |
| を使用しない場合のCustomer Journey Analyticsへの新しいデータ [ステッチ](https://experienceleague.adobe.com/docs/analytics-platform/using/stitching/overview.html?lang=en) | &lt; 3.75 時間 |
| ステッチでCustomer Journey Analyticsする新しいデータ | &lt; 7 時間 |
| 100 億未満のイベントのバックフィル | &lt; 4 週間 |

Customer Journey Analyticsの待ち時間について詳しくは、次を参照してください。 [Customer Journey Analyticsガードレール](https://experienceleague.adobe.com/docs/analytics-platform/using/cja-admin/guardrails.html?lang=en).

実稼動用サンドボックスの Analytics のバックフィルは、デフォルトで 13 か月です。 非実稼動用サンドボックス内の Analytics データの場合、バックフィルは 3 か月に設定されます。 上記の表で言及されている 100 億イベントの制限は、期待される待ち時間に厳密に関係します。

実稼動サンドボックスで Analytics ソースデータフローを作成すると、次の 2 つのデータフローが作成されます。

* データレイクへの履歴レポートスイートデータの 13 か月のバックフィルを行うデータフロー。 このデータフローは、バックフィルが完了すると終了します。
* ライブデータをデータレイクおよびに送信するデータフローフロー [!DNL Real-Time Customer Profile]. このデータフローは継続的に実行されます。

>[!NOTE]
>
>Analytics バックフィルデータはに取り込まれません [!DNL Profile] そのため、はライセンスプロファイルでは考慮されません。

## のプライマリ識別情報 [!DNL Analytics] データ

からのすべてのヒット [!DNL Analytics] ソースコネクタには、ECID と AAID のどちらが存在するかに応じて異なるプライマリ識別子が含まれています。 ECID がある場合、ECID がプライマリ識別子として指定されます。 AAID がある場合、AAID がプライマリとして指定されます。

の ID フィールドについて詳しくは、次の表を参照してください [!DNL Analytics] データ。

| ID フィールド | 説明 |
| --- | --- |
| AAID | AAID は、Adobe Analyticsのプライマリデバイス ID で、 [!DNL Analytics] ソース。 AAID は、以下のように呼ばれることもあります *従来の Analytics ID* またはとして `s_vi` cookie ID。 これにもかかわらず、AAID は、 `s_vi` cookie が存在しません。 AAID は、 `post_visid_high` および `post_visid_low` の列 [[!DNL Analytics] データフィード](https://experienceleague.adobe.com/docs/analytics/export/analytics-data-feed/data-feed-contents/datafeeds-reference.html). 任意のイベントでは、AAID フィールドには、に記載されている複数の異なるタイプの 1 つである可能性がある単一の ID が含まれます [の業務の順序 [!DNL Analytics] ID](https://experienceleague.adobe.com/docs/id-service/using/reference/analytics-reference/analytics-order-of-operations.html). **注意**：レポートスイート全体では、AAID にはイベント間でタイプが混在する場合があります。 |
| ECID | ECID （Experience CloudID）は個別のデバイス ID フィールドで、次の場合にAdobe Analyticsに入力されます [!DNL Analytics] は、Experience Cloud ID サービスを使用して実装されます。 ECID は、MCID （Marketing CloudID）とも呼ばれます。 ECID がイベントに存在する場合、AAID は、Analytics に応じて、ECID に基づいている可能性があります [猶予期間](https://experienceleague.adobe.com/docs/id-service/using/reference/analytics-reference/grace-period.html) が設定されました。 ECID は、 `mcvisid` （Analytics データフィード内）。 ECID について詳しくは、 [ECID の概要](../../../identity-service/features/ecid.md). ECID との連携方法について説明します [!DNL Analytics]のドキュメントを参照してください。 [Analytics とExperience CloudID のリクエスト](https://experienceleague.adobe.com/docs/id-service/using/reference/analytics-reference/legacy-analytics.html). |
| AACUSTOMID | AACUSTOMID は、の使用に基づいて、Adobe Analyticsで設定される個別の ID フィールドです。 `s.VisitorID` 内の変数 [!DNL Analytics] 実装。 AACUSTOMID は、 `cust_visid` 列： [[!DNL Analytics] データフィード](https://experienceleague.adobe.com/docs/analytics/export/analytics-data-feed/data-feed-contents/datafeeds-reference.html). AACUSTOMID が存在する場合、AACUSTOMID は、で定義された他のすべての識別子に優先されるので、AAID は、AACUSTOMID に基づきます [の業務の順序 [!DNL Analytics] ID](https://experienceleague.adobe.com/docs/id-service/using/reference/analytics-reference/analytics-order-of-operations.html). |

### 方法 [!DNL Analytics] ソースでの id の扱い

この [!DNL Analytics] ソースは、次のように、これらの ID を XDM フォームでExperience Platformに渡します。

* `endUserIDs._experience.aaid.id`
* `endUserIDs._experience.mcid.id`
* `endUserIDs._experience.aacustomid.id`

これらのフィールドは、ID としてマークされません。 代わりに、同じ ID （イベントに存在する場合）が XDM にコピーされます `identityMap` キーと値のペアとして：

* `{ "key": "AAID", "value": [ { "id": "<identity>", "primary": <true or false> } ] }`
* `{ "key": "ECID", "value": [ { "id": "<identity>", "primary": <true or false> } ] }`
* `{ "key": "AACUSTOMID", "value": [ { "id": "<identity>", "primary": false } ] }`

ID がにコピーされる場合 `identityMap`, `endUserIDs._experience.mcid.namespace.code` も同じイベントに設定されます。

* AAID が存在する場合、 `endUserIDs._experience.aaid.namespace.code` は「AAID」に設定されています。
* ECID が存在する場合、 `endUserIDs._experience.mcid.namespace.code` は「ECID」に設定されています。
* AACUSTOMID が存在する場合、 `endUserIDs._experience.aacustomid.namespace.code` は、「AACUSTOMID」に設定されています。

ID マップでは、ECID が存在する場合、イベントのプライマリ ID としてマークされます。 この場合、AAID は、以下の理由により、ECID に基づいている可能性があります [ID サービスの猶予期間](https://experienceleague.adobe.com/docs/id-service/using/reference/analytics-reference/grace-period.html). それ以外の場合、AAID は、イベントのプライマリ ID としてマークされます。 AACUSTOMID は、イベントのプライマリ ID としてマークされることはありません。 ただし、AACUSTOMID が存在する場合、AAID は、操作のExperience Cloud順序により AACUSTOMID に基づきます。

>[!NOTE]
>
>データ準備を使用すると、Analytics からのセカンダリ ID （AAID や AACUSTOMID など）をフィルタリングできます。 除外した場合、これらの ID は、受信する Analytics データで使用可能な場合、プロファイルには取り込まれません。 フィルタリングされていないデータは、引き続きデータレイクに読み込まれます。
