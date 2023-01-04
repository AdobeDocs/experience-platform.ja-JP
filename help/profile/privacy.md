---
keywords: Experience Platform;ホーム;人気のトピック
solution: Experience Platform
title: リアルタイム顧客プロファイルでのプライバシーリクエストの処理
type: Documentation
description: Adobe Experience Platform Privacy Service は、プライバシーに関する多数の規則に従って、個人データへのアクセス、販売のオプトアウト、または削除を求める顧客のリクエストを処理します。このドキュメントでは、リアルタイム顧客プロファイルのプライバシーリクエストの処理に関する基本的な概念について説明します。
exl-id: fba21a2e-aaf7-4aae-bb3c-5bd024472214
source-git-commit: 34e0381d40f884cd92157d08385d889b1739845f
workflow-type: tm+mt
source-wordcount: '1563'
ht-degree: 30%

---

# [!DNL Real-Time Customer Profile] でのプライバシーリクエストの処理

Adobe Experience Platform [!DNL Privacy Service] は、EU 一般データ保護規則（GDPR）や [!DNL California Consumer Privacy Act]（CCPA）などのプライバシー規制に従って、個人データへのアクセス、販売のオプトアウト、または削除を求める顧客のリクエストを処理します。

このドキュメントでは、Adobe Experience Platform における [!DNL Real-Time Customer Profile] のプライバシーリクエスト処理に関する基本的な概念について説明します。

>[!NOTE]
>
>このガイドでは、プライバシーでのプロファイルデータストアに対するプライバシーリクエストのExperience Platformについてのみ説明します。 Platform データレイクに対してプライバシーリクエストもおこなう予定がある場合は、 [データレイクでのプライバシーリクエストの処理](../catalog/privacy.md) を追加しました。
>
>他の Adobe Experience Cloud アプリケーションにプライバシーリクエストを送信する手順については、[Privacy Service のドキュメント](../privacy-service/experience-cloud-apps.md)を参照してください。

## はじめに

このガイドを読む前に、次の [!DNL Experience Platform] サービスに関する十分な理解を得ることをお勧めします。

* [[!DNL Privacy Service]](../privacy-service/home.md) ：Adobe Experience Cloud アプリケーションをまたいで、自身の個人データのアクセス、販売のオプトアウト、または削除に対する顧客リクエストを管理します。
* [[!DNL Identity Service]](../identity-service/home.md)：デバイスやシステムをまたいで ID を結び付けることで、顧客体験データの断片化によって発生する根本的な課題を解決します。
* [[!DNL Real-Time Customer Profile]](home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

## ID 名前空間について {#namespaces}

Adobe Experience Platform [!DNL Identity Service] は、システムやデバイスをまたいで顧客 ID データを結び付けます。[!DNL Identity Service] は **ID 名前空間**&#x200B;を使用して、ID の値を元のシステムと関連付け、それらの値を識別するコンテキストを提供します。名前空間は、電子メールアドレス（「電子メール」）などの一般的な概念を表すことがあります。また、ID を特定のアプリケーション（Adobe Advertising Cloud ID（「AdCloud」）や Adobe Target ID（「TNTID」）など）に関連付けることができます。

ID サービスは、グローバルに定義された（標準）ID およびユーザー定義の（カスタム）ID 名前空間を保持します。標準の名前空間はすべての組織（「電子メール」や「ECID」など）で使用できますが、組織は、特定のニーズに合わせてカスタム名前空間を作成することもできます。

[!DNL Experience Platform] の ID 名前空間について詳しくは、 [ID 名前空間の概要](../identity-service/namespaces.md)を参照してください。

## リクエストの送信 {#submit}

以下のセクションでは、[!DNL Privacy Service] の API または UI を使用して [!DNL Real-Time Customer Profile] に対しプライバシーリクエストを行う方法について概説しています。これらの節を読む前に、 [Privacy ServiceAPI](../privacy-service/api/getting-started.md) または [Privacy ServiceUI](../privacy-service/ui/overview.md) リクエストペイロードで送信されたユーザー id データを適切に書式設定する方法など、プライバシージョブを送信する手順に関する完全なドキュメントです。

>[!IMPORTANT]
>
>Privacy Serviceは処理のみ可能 [!DNL Profile] id ステッチを実行しない結合ポリシーを使用するデータ。 詳しくは、 [結合ポリシーの制限](#merge-policy-limitations) を参照してください。
>
>また、プライバシーリクエストの完了に要する時間は保証できないことに注意する必要があります。 変更が [!DNL Profile] リクエストの処理中にデータを保証することはできません。

### API の使用

API でジョブリクエストを作成する際は、`userIDs` 内で指定するいずれの ID に対しても固有の `namespace` および `type` を使用する必要があります。[!DNL Identity Service] によって認識される有効な [ID 名前空間](#namespaces) を `namespace` 値に指定する必要があり、`type` には、`standard` または `unregistered` のいずれかを（標準名前空間とカスタム名前空間のそれぞれに応じて）指定する必要があります。

>[!NOTE]
>
>ID グラフと、プロファイルフラグメントを Platform データセットでどのように配布するかに応じて、各顧客に対して複数の ID を指定する必要が生じる場合があります。 次の節を参照してください [プロファイルフラグメント](#fragments) を参照してください。

さらに、リクエストペイロードの `include` 配列には、リクエスト対象である別のデータストアの製品値を含める必要があります。ID に関連付けられたプロファイルデータを削除するには、配列に `ProfileService`. 顧客の ID グラフの関連付けを削除するには、配列に `identity`.

>[!NOTE]
>
>詳しくは、 [プロファイルリクエストと id リクエスト](#profile-v-identity) このドキュメントの後半で、 `ProfileService` および `identity` 内 `include` 配列。

次のリクエストは、 [!DNL Profile] ストア。 顧客に対して、 `userIDs` 配列；標準を使ったもの `Email` ID 名前空間と、カスタムを使用する他の `Customer_ID` 名前空間。 また、 [!DNL Profile] (`ProfileService`) を `include` 配列：

```shell
curl -X POST \
  https://platform.adobe.io/data/core/privacy/jobs \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'Content-Type: application/json' \
  -d '{
    "companyContexts": [
      {
        "namespace": "imsOrgID",
        "value": "{ORG_ID}"
      }
    ],
    "users": [
      {
        "key": "user12345",
        "action": ["access","delete"],
        "userIDs": [
          {
            "namespace": "Email",
            "value": "ajones@acme.com",
            "type": "standard"
          },
          {
            "namespace": "Customer_ID",
            "value": "12345678",
            "type": "unregistered"
          }
        ]
      }
    ],
    "include": ["ProfileService","identity"],
    "expandIds": false,
    "priority": "normal",
    "regulation": "ccpa"
}'
```

>[!IMPORTANT]
>
>Platform は、組織に属するすべての[サンドボックス](../sandboxes/home.md)でプライバシーリクエストを処理します。その結果、リクエストに含まれる `x-sandbox-name` ヘッダーはシステムによって無視されます。

### UI の使用

UI でジョブリクエストを作成する場合は、必ず **[!UICONTROL AEP データレイク]** および/または **[!UICONTROL プロファイル]** under **[!UICONTROL 製品]** データレイクに保存されたデータのジョブを処理するため、または [!DNL Real-Time Customer Profile]、それぞれ。

![UI で作成されるアクセスジョブリクエストで、「製品」で「プロファイル」オプションが選択されているもの](./images/privacy/product-value.png)

## プライバシーリクエストのプロファイルフラグメント {#fragments}

内 [!DNL Profile] データストアとは、多くの場合、個々の顧客の個人データを複数のプロファイルフラグメントで構成し、id グラフを通じて個人に関連付けます。 プライバシーリクエストを [!DNL Profile] に保存する際には、要求はプロファイル全体ではなく、プロファイルフラグメントレベルでのみ処理されることに注意する必要があります。

例えば、顧客属性データを 3 つの異なるデータセットに保存し、異なる識別子を使用してそのデータを個々の顧客に関連付ける場合を考えてみましょう。

| データセット名 | プライマリID フィールド | 保存済み属性 |
| --- | --- | --- |
| データセット 1 | `customer_id` | `address` |
| データセット 2 | `email_id` | `firstName`、`lastName` |
| データセット 3 | `email_id` | `mlScore` |

データセットの 1 つが `customer_id` を主識別子として使用する一方、他の 2 つは `email_id`. 以下のみを使用してプライバシーリクエスト（アクセスまたは削除）を送信する場合 `email_id` ユーザー ID 値として、 `firstName`, `lastName`、および `mlScore` 属性が処理され、 `address` 影響を受けない

プライバシーリクエストがすべての関連する顧客属性を確実に処理するには、該当するすべてのデータセットに対して、それらの属性を保存できるプライマリ ID 値を指定する必要があります（1 人の顧客につき最大 9 つの ID）。 詳しくは、 [スキーマ構成の基本](../xdm/schema/composition.md#identity) id として一般的にマークされるフィールドについて詳しくは、を参照してください。

## リクエスト処理の削除 {#delete}

[!DNL Experience Platform] が [!DNL Privacy Service] から削除リクエストを受信すると、[!DNL Platform] は、[!DNL Privacy Service] に対し、リクエストを受信し、影響を受けるデータが削除用にマークされている旨の確認を送信します。その後、プライバシージョブが完了すると、レコードは削除されます。

ID サービス (`identity`) とデータレイク (`aepDataLake`) をプロファイルのプライバシーリクエスト (`ProfileService`) の場合、プロファイルに関連する様々なデータセットが、次のように異なるタイミングでシステムから削除されます。

| 含まれる製品 | エフェクト |
| --- | --- |
| `ProfileService` のみ | 削除リクエストを受け取ったことを示す確認が Platform から送信されるとすぐに、プロファイルが削除されます。 ただし、プロファイルの ID グラフは引き続き残り、同じ ID を持つ新しいデータが取り込まれると、プロファイルを再構築できる可能性があります。 プロファイルに関連付けられたデータも、データレイクに残ります。 |
| `ProfileService` および `identity` | 削除リクエストを受け取ったことを示す確認が Platform から送信されるとすぐに、プロファイルとそれに関連する ID グラフが削除されます。 プロファイルに関連付けられたデータは、データレイクに残ります。 |
| `ProfileService` および `aepDataLake` | 削除リクエストを受け取ったことを示す確認が Platform から送信されるとすぐに、プロファイルが削除されます。 ただし、プロファイルの ID グラフは引き続き残り、同じ ID を持つ新しいデータが取り込まれると、プロファイルを再構築できる可能性があります。<br><br>データレイク製品が要求を受信し、現在処理中であることを応答すると、プロファイルに関連付けられたデータはソフト削除されるので、誰もがアクセスできません [!DNL Platform] サービス。 ジョブが完了すると、データはデータレイクから完全に削除されます。 |
| `ProfileService`, `identity`, および `aepDataLake` | 削除リクエストを受け取ったことを示す確認が Platform から送信されるとすぐに、プロファイルとそれに関連する ID グラフが削除されます。<br><br>データレイク製品が要求を受信し、現在処理中であることを応答すると、プロファイルに関連付けられたデータはソフト削除されるので、誰もがアクセスできません [!DNL Platform] サービス。 ジョブが完了すると、データはデータレイクから完全に削除されます。 |

詳しくは、 [[!DNL Privacy Service] ドキュメント](../privacy-service/home.md#monitor) を参照してください。

### プロファイルリクエストと ID リクエスト {#profile-v-identity}

プロファイルに対して削除リクエストがおこなわれた場合 (`ProfileService`) ではなく、ID サービス (`identity`) の場合、結果のジョブは、顧客（または顧客のセット）に対して収集された属性データを削除しますが、ID グラフで確立された関連付けは削除しません。

例えば、顧客の `email_id` および `customer_id` は、これらの ID の下に保存されているすべての属性データを削除します。 ただし、その後、同じ `customer_id` が適切な `email_id`と呼ばれ、関連付けはまだ存在するので、

特定の顧客のプロファイルとすべての ID 関連付けを削除するには、削除リクエストにプロファイルと ID サービスの両方をターゲット製品として含めてください。

### 結合ポリシーの制限 {#merge-policy-limitations}

Privacy Serviceは処理のみ可能 [!DNL Profile] id ステッチを実行しない結合ポリシーを使用するデータ。 UI を使用してプライバシーリクエストが処理されているかどうかを確認する場合は、 **[!DNL None]** その [!UICONTROL ID のステッチ] タイプ。 つまり、 [!UICONTROL ID のステッチ] が [!UICONTROL プライベートグラフ].
>![結合ポリシーの ID の組み合わせは「なし」に設定されます](./images/privacy/no-id-stitch.png)
>
## 次の手順

このドキュメントでは、[!DNL Experience Platform] におけるプライバシーリクエストの処理に関する重要な概念について説明します。ID データの管理方法とプライバシージョブの作成方法に関する理解を深めるために、引き続きこのガイド全体に記載されているドキュメントを読むことをお勧めします。

のプライバシーリクエストの処理に関する情報 [!DNL Platform] 使用されないリソース [!DNL Profile]を参照し、 [データレイクでのプライバシーリクエストの処理](../catalog/privacy.md).
