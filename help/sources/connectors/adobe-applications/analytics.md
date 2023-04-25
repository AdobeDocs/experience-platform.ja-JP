---
title: レポートスイートデータ用のAdobe Analytics Source Connector
description: このドキュメントでは、 Analytics の概要と、Analytics データの使用例を説明します。
exl-id: c4887784-be12-40d4-83bf-94b31eccdc2e
source-git-commit: 83ce7d46e4e64fbe961c964ed5a17ec12a7ec15f
workflow-type: tm+mt
source-wordcount: '1112'
ht-degree: 18%

---

# レポートスイートデータ用の Adobe Analytics ソースコネクタ

Adobe Experience Platformでは、Analytics ソースコネクタを使用してAdobe Analyticsデータを取り込むことができます。 この [!DNL Analytics] ソースコネクタは、収集されたデータをストリーミングします [!DNL Analytics] をリアルタイムで Platform に変換し、SCDS 形式に変換 [!DNL Analytics] データを [!DNL Experience Data Model] (XDM)Platform で使用するフィールド。

このドキュメントでは、 [!DNL Analytics] およびで、の使用例について説明します。 [!DNL Analytics] データ。

## Adobe Analytics と Analytics データ

[!DNL Analytics] は、顧客に関する詳細情報や顧客が Web プロパティとどのようにやり取りするかを学び、デジタルマーケティングの費用が効果的かを確認し、改善点を特定するのに役立つ強力なエンジンです。 [!DNL Analytics] は 1 年に数兆件もの web トランザクションを処理し、 [!DNL Analytics] ソースコネクタを使用すると、この豊富な行動データを簡単にタップして、 [!DNL Real-Time Customer Profile] 数分で

![Adobe Analyticsを含む様々なAdobeアプリケーションからのデータのジャーニーを示す図。](./images/analytics-data-experience-platform.png)

高いレベルで [!DNL Analytics] は、世界中の様々なデジタルチャネルや複数のデータセンターからデータを収集します。 データが収集されると、訪問者 ID、セグメント化および変換アーキテクチャ (VISTA) のルールと処理ルールが適用され、受信データが形成されます。 生データは、この軽量な処理を経た後、次の方法で消費可能と見なされます。 [!DNL Real-Time Customer Profile]. 前述と並行するプロセスでは、同じ処理済みデータがマイクロバッチされ、で使用するために Platform データセットに取り込まれます。 [!DNL Data Science Workspace], [!DNL Query Service]、およびその他のデータ検出アプリケーション。

詳しくは、 [処理ルールの概要](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/processing-rules/processing-rules.html?lang=ja) を参照してください。

## エクスペリエンスデータモデル（XDM）

XDM は公に文書化された仕様で、 Experience Platform 上のサービスとの通信に使用するアプリケーションの共通の構造と定義を提供します。

XDM 標準規格に準拠することで、データを統一的に取り込むことができ、データの配信と情報の収集が容易になります。

XDM について詳しくは、「[XDM システムの概要](../../../xdm/home.md)」を参照してください。

## Adobe Analytics から XDM へのフィールドのマッピング方法

>[!IMPORTANT]
>
>データ準備変換を実行すると、データフロー全体に遅延が生じる場合があります。 追加される待ち時間は、変換ロジックの複雑さに応じて異なります。

ソース接続が確立され、 [!DNL Analytics] Platform ユーザーインターフェイスを使用してデータをExperience Platformに変換すると、データフィールドは自動的にマッピングされ、 [!DNL Real-Time Customer Profile] 分以内に とのソース接続の作成手順 [!DNL Analytics] Platform UI を使用して、 [Analytics ソースコネクタのチュートリアル](../../tutorials/ui/create/adobe-applications/analytics.md).

の間に発生するフィールドマッピングに関する詳細 [!DNL Analytics] とExperience Platform: [Adobe Analyticsフィールドマッピング](./mapping/analytics.md) ガイド。

## Platform の Analytics データで予想される遅延はどのくらいですか。

次の表に、Platform 上の Analytics データで予想される遅延を示します。  遅延は、顧客の構成、データ量、消費者のアプリケーションによって異なります。例えば、Analytics の実装が `A4T` で設定されている場合、パイプラインの遅延が 5 ～ 10 分に増えます。

| Analytics データ | 予想される遅延 |
| -------------- | ---------------- |
| 新しいデータ [!DNL Real-Time Customer Profile] (A4T) **not** 有効 ) | &lt; 2 分 |
| 新しいデータ [!DNL Real-Time Customer Profile] (A4T) **が** 有効 ) | 最大 30 分 |
| データレイクの新しいデータ | &lt; 90 分 |
| 100 億未満のイベントのバックフィル | &lt; 4 週間 |

実稼動サンドボックスの Analytics バックフィルのデフォルト値は 13 ヶ月です。 非実稼動用サンドボックスの Analytics データの場合、バックフィルは 3 ヶ月に設定されます。 上記の表に示した 100 億件のイベントの制限は、予想される待ち時間に厳密に関係しています。

実稼働用サンドボックスで Analytics ソースのデータフローを作成する場合、2 つのデータフローが作成されます。

* 13 か月間にわたって履歴レポートスイートデータをデータレイクにバックフィルするデータフロー。 このデータフローは、バックフィルが完了すると終了します。
* ライブデータをデータレイクとに送信するデータフロー [!DNL Real-Time Customer Profile]. このデータフローは継続的に実行されます。

>[!NOTE]
>
>Analytics バックフィルデータが [!DNL Profile] したがって、はライセンスプロファイルでは考慮されません。

## プライマリ識別子 [!DNL Analytics] データ

次の [!DNL Analytics] ソースコネクタには、ECID または AAID のどちらが存在するかに依存するプライマリ識別子が含まれます。 ECID がある場合、その ECID がプライマリ識別子として指定されます。 AAID がある場合、AAID がプライマリとして指定されます。

次の表に、 [!DNL Analytics] データ。

| ID フィールド | 説明 |
| --- | --- |
| AAID | AAID はAdobe Analyticsの主なデバイス識別子で、 [!DNL Analytics] ソース。 AAID は、 *従来の Analytics ID* または `s_vi` cookie ID。 これにもかかわらず、AAID は `s_vi` cookie が存在しません。 AAID は、 `post_visid_high` および `post_visid_low` 列 [[!DNL Analytics] データフィード](https://experienceleague.adobe.com/docs/analytics/export/analytics-data-feed/data-feed-contents/datafeeds-reference.html?lang=ja). 任意のイベントで、 AAID フィールドには単一の ID が含まれます。これは、 [～の操作の順序 [!DNL Analytics] ID](https://experienceleague.adobe.com/docs/id-service/using/reference/analytics-reference/analytics-order-of-operations.html). **注意**:レポートスイート全体で、AAID に複数のイベントタイプが混在している場合があります。 |
| ECID | ECID(Experience CloudID) は、別のデバイス識別子フィールドで、Adobe Analyticsで [!DNL Analytics] は、Experience CloudID サービスを使用して実装されます。 ECID は、MCID(Marketing CloudID) とも呼ばれます。 イベントに ECID が存在する場合、AAID は、Analytics が [猶予期間](https://experienceleague.adobe.com/docs/id-service/using/reference/analytics-reference/grace-period.html) が設定されている。 ECID は、 `mcvisid` Analytics データフィード内で使用されます。 ECID について詳しくは、 [ECID の概要](../../../identity-service/ecid.md). ECID がと連携する方法について詳しくは、 [!DNL Analytics]を参照し、 [Analytics およびExperience CloudID のリクエスト](https://experienceleague.adobe.com/docs/id-service/using/reference/analytics-reference/legacy-analytics.html?lang=ja). |
| AACUSTOMID | AACUSTOMID は、 `s.VisitorID` 変数の [!DNL Analytics] 実装。 AACUSTOMID は、 `cust_visid` 列 [[!DNL Analytics] データフィード](https://experienceleague.adobe.com/docs/analytics/export/analytics-data-feed/data-feed-contents/datafeeds-reference.html?lang=ja). AACUSTOMID が存在する場合、AAID は AACUSTOMID に基づきます。これは、AACUSTOMID が [～の操作の順序 [!DNL Analytics] ID](https://experienceleague.adobe.com/docs/id-service/using/reference/analytics-reference/analytics-order-of-operations.html). |

### 方法 [!DNL Analytics] ソースで ID を扱う

この [!DNL Analytics] ソースは、これらの ID を次のように XDM 形式のExperience Platformに渡します。

* `endUserIDs._experience.aaid.id`
* `endUserIDs._experience.mcid.id`
* `endUserIDs._experience.aacustomid.id`

これらのフィールドは、ID としてマークされません。代わりに、同じ ID が XDM の `identityMap` をキーと値のペアとして使用します。

* `{ "key": "AAID", "value": [ { "id": "<identity>", "primary": <true or false> } ] }`
* `{ "key": "ECID", "value": [ { "id": "<identity>", "primary": <true or false> } ] }`
* `{ "key": "AACUSTOMID", "value": [ { "id": "<identity>", "primary": false } ] }`

ID マップで、ECID が存在する場合は、イベントのプライマリ ID としてマークされます。 この場合、AAID は、 [ID サービスの猶予期間](https://experienceleague.adobe.com/docs/id-service/using/reference/analytics-reference/grace-period.html). それ以外の場合、AAID は、イベントのプライマリ ID としてマークされます。AACUSTOMID は、イベントのプライマリ ID としてマークされることはありません。ただし、AACUSTOMID が存在する場合、AAID は操作のExperience Cloud順序に基づいて AACUSTOMID に基づきます。
