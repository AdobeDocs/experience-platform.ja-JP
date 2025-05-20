---
keywords: Experience Platform;ホーム;人気のトピック
solution: Experience Platform
title: リアルタイム顧客プロファイルでのプライバシーリクエストの処理
type: Documentation
description: Adobe Experience Platform Privacy Service は、プライバシーに関する多数の規則に従って、個人データへのアクセス、販売のオプトアウト、または削除を求める顧客のリクエストを処理します。このドキュメントでは、リアルタイム顧客プロファイルのプライバシーリクエストの処理に関する基本的な概念について説明します。
exl-id: fba21a2e-aaf7-4aae-bb3c-5bd024472214
source-git-commit: 6eaa384feb1b84e6081f03cb4de9687ad26f437d
workflow-type: tm+mt
source-wordcount: '1757'
ht-degree: 24%

---

# [!DNL Real-Time Customer Profile] でのプライバシーリクエストの処理

Adobe Experience Platform [!DNL Privacy Service] は、EU 一般データ保護規則（GDPR）や [!DNL California Consumer Privacy Act]（CCPA）などのプライバシー規制に従って、個人データへのアクセス、販売のオプトアウト、または削除を求める顧客のリクエストを処理します。

このドキュメントでは、Adobe Experience Platform における [!DNL Real-Time Customer Profile] のプライバシーリクエスト処理に関する基本的な概念について説明します。

>[!NOTE]
>
>このガイドでは、Experience Platformのプロファイルデータストアに対してプライバシーリクエストを行う方法についてのみ説明します。 Experience Platform Data Lake のプライバシーリクエストもおこなう予定がある場合は、このチュートリアルに加えて [Data Lake でのプライバシーリクエストの処理 ](../catalog/privacy.md) に関するガイドを参照してください。
>
>他の Adobe Experience Cloud アプリケーションにプライバシーリクエストを送信する手順については、[Privacy Service のドキュメント](../privacy-service/experience-cloud-apps.md)を参照してください。

>[!IMPORTANT]
>
>このガイドのプライバシーリクエストは **B2B 非ユーザーエンティティを対象としていま** ん。

## はじめに

このガイドでは、次の [!DNL Experience Platform] コンポーネントに関する十分な知識が必要です。

* [[!DNL Privacy Service]](../privacy-service/home.md) ：Adobe Experience Cloud アプリケーションをまたいで、自身の個人データのアクセス、販売のオプトアウト、または削除に対する顧客リクエストを管理します。
* [[!DNL Identity Service]](../identity-service/home.md)：デバイスやシステムをまたいで ID を結び付けることで、顧客体験データの断片化によって発生する根本的な課題を解決します。
* [[!DNL Real-Time Customer Profile]](home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

## ID 名前空間について {#namespaces}

Adobe Experience Platform [!DNL Identity Service] は、システムやデバイスをまたいで顧客 ID データを結び付けます。[!DNL Identity Service] は **ID 名前空間**&#x200B;を使用して、ID の値を元のシステムと関連付け、それらの値を識別するコンテキストを提供します。名前空間は、電子メールアドレス（「電子メール」）などの一般的な概念を表すことがあります。また、ID を特定のアプリケーション（Adobe Advertising Cloud ID（「AdCloud」）や Adobe Target ID（「TNTID」）など）に関連付けることができます。

ID サービスは、グローバルに定義された（標準）ID およびユーザー定義の（カスタム）ID 名前空間を保持します。標準の名前空間はすべての組織（「電子メール」や「ECID」など）で使用できますが、組織は、特定のニーズに合わせてカスタム名前空間を作成することもできます。

[!DNL Experience Platform] の ID 名前空間について詳しくは、 [ID 名前空間の概要](../identity-service/features/namespaces.md)を参照してください。

## リクエストの送信 {#submit}

以下のセクションでは、[!DNL Privacy Service] の API または UI を使用して [!DNL Real-Time Customer Profile] に対しプライバシーリクエストを行う方法について概説しています。これらのセクションを読む前に、[Privacy Service API](../privacy-service/api/getting-started.md) または [Privacy Service UI](../privacy-service/ui/overview.md) のドキュメントを確認するか認識しておく必要があります。 これらのドキュメントは、リクエストペイロードで送信されたユーザー ID データの形式を適切に設定する方法など、プライバシージョブの送信方法に関する完全な手順を提供します。

>[!IMPORTANT]
>
>Privacy Serviceは、ID ステッチを実行しない結合ポリシーを使用してのみ [!DNL Profile] データを処理できます。 詳しくは、[ 結合ポリシーの制限 ](#merge-policy-limitations) の節を参照してください。
>
>プライバシーリクエストは、規制要件内で非同期に処理され、完了するまでにかかる時間は様々です。 リクエストの処理中に [!DNL Profile] データに変更が生じた場合、それらの受信レコードもそのリクエストで処理される保証はありません。 プライバシージョブがリクエストされた時点でデータレイクまたはプロファイルストアに保持されているプロファイルのみが削除されることが保証されます。 削除ジョブの実行中に、削除リクエストのサブジェクトに関連するプロファイルデータを取り込んだ場合、すべてのプロファイルフラグメントが削除されるとは限りません。
>お客様は、削除依頼の際にExperience Platformまたはプロファイルサービスから受信するデータを把握する責任を負います。このデータは、お客様のレコードストアに挿入されるからです。 削除中または削除中のデータの取り込みに注意する必要があります。

### API の使用

API でジョブリクエストを作成する際は、`userIDs` 内で指定するいずれの ID に対しても固有の `namespace` および `type` を使用する必要があります。[!DNL Identity Service] によって認識される有効な [ID 名前空間](#namespaces) を `namespace` 値に指定する必要があり、`type` には、`standard` または `unregistered` のいずれかを（標準名前空間とカスタム名前空間のそれぞれに応じて）指定する必要があります。

>[!NOTE]
>
>ID グラフとExperience Platform データセットでのプロファイルフラグメントの配布方法によっては、各顧客に対して複数の ID を指定する必要が生じる場合があります。 詳しくは、次の節 [ プロファイルフラグメント ](#fragments) を参照してください。

さらに、リクエストペイロードの `include` 配列には、リクエスト対象である別のデータストアの製品値を含める必要があります。ID に関連付けられたプロファイルデータを削除するには、配列に値 `ProfileService` を含める必要があります。 顧客の ID グラフの関連付けを削除するには、配列に値 `identity` を含める必要があります。

>[!NOTE]
>
>`include` 配列内で `ProfileService` と `identity` を使用した場合の影響について詳しくは、このドキュメントの後半の [ プロファイルリクエストと ID リクエスト ](#profile-v-identity) に関する節を参照してください。

次のリクエストは、1 件の顧客データ用の新しいプライバシージョブを [!DNL Profile] ストアに作成します。 顧客の `userIDs` 配列には、2 つの ID 値が指定されています。1 つは標準の `Email` ID 名前空間、もう 1 つはカスタム `Customer_ID` 名前空間を使用します。 また、`include` の配列に [!DNL Profile] （`ProfileService`）の製品値も含まれます。

**リクエスト**

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
>Experience Platformは、組織に属するすべての [ サンドボックス ](../sandboxes/home.md) でプライバシーリクエストを処理します。 その結果、リクエストに含まれる `x-sandbox-name` ヘッダーはシステムによって無視されます。

**製品の反応**

プロファイルサービスの場合、プライバシージョブが完了すると、要求されたユーザー ID に関する情報を含んだ応答が JSON 形式で返されます。

```json
{
    "privacyResponse": {
        "jobId": "7467850f-9698-11ed-8635-355435552164",
        "response": [
            {
                "sandbox": "prod",
                "mergePolicyId": "none",
                "result": {
                    "person": {
                        "gender": "female"           
                    },
                    "personalEmail": {
                        "address": "ajones@acme.com",
                    },
                    "identityMap": {
                        "crmid": [
                            {
                                "id": "5b7db37a-bc7a-46a2-a63e-2cfe7e1cc068"
                            }
                        ]
                    }
                }
            },
            {
                "sandbox": "prod",
                "mergePolicyId": "none",
                "result": {
                    "person": {
                        "gender": "male"
                    },
                    "id": 12345678,
                    "identityMap": {
                        "crmid": [
                            {
                                "id": "e9d439f2-f5e4-4790-ad67-b13dbd89d52e"
                            }
                        ]
                    }
                }
            }
        ]
    }
}
```

### UI の使用

UI でジョブリクエストを作成する場合は、「**[!UICONTROL Products]**」の下の「**[!UICONTROL AEP Data Lake]**」または「**[!UICONTROL Profile]**」を必ず選択し、それぞれデータレイクまたは [!DNL Real-Time Customer Profile] に保存されたデータのジョブを処理します。

![UI で作成され、製品でプロファイルオプションが選択されたアクセスジョブリクエスト ](./images/privacy/product-value.png)

## プライバシーリクエストのプロファイルフラグメント {#fragments}

[!DNL Profile] データストアでは、個々の顧客の個人データは、多くの場合、複数のプロファイルフラグメントで構成され、ID グラフを通じて人物に関連付けられます。 [!DNL Profile] ストアに対してプライバシーリクエストを行う場合、リクエストは、プロファイル全体ではなく、プロファイルフラグメントレベルでのみ処理されることに注意してください。

例えば、顧客属性データを 3 つの異なるデータセットに格納し、それらのデータセットで異なる識別子を使用してデータを個々の顧客に関連付ける場合を考えてみましょう。

| データセット名 | プライマリ ID フィールド | 保存された属性 |
| --- | --- | --- |
| データセット 1 | `customer_id` | `address` |
| データセット 2 | `email_id` | `firstName`、`lastName` |
| データセット 3 | `email_id` | `mlScore` |

データセットの 1 つはプライマリ識別子として `customer_id` を使用し、他の 2 つは `email_id` を使用します。 ユーザー ID 値として `email_id` のみを使用してプライバシーリクエスト（アクセスまたは削除）を送信した場合、`firstName`、`lastName`、`mlScore` 属性のみが処理され、`address` は影響を受けません。

プライバシーリクエストがすべての関連する顧客属性を処理するようにするには、これらの属性を保存できるすべての適用可能なデータセットにプライマリ ID 値を指定する必要があります（顧客あたり最大 9 つの ID）。 ID として一般的にマークされるフィールドについて詳しくは、[ スキーマ構成の基本 ](../xdm/schema/composition.md#identity) の ID フィールドに関する節を参照してください。

## リクエスト処理の削除 {#delete}

[!DNL Experience Platform] が [!DNL Privacy Service] から削除リクエストを受信すると、[!DNL Experience Platform] は、[!DNL Privacy Service] に対し、リクエストを受信し、影響を受けるデータが削除用にマークされている旨の確認を送信します。その後、プライバシージョブが完了すると、レコードが削除されます。

>[!IMPORTANT]
>
>プライバシーの削除リクエストは即時には行われず、関係するサービスや、地理的な場所などのその他の影響を受ける要因によって異なる場合があります。 プライバシージョブの完了期間は 15～45 日ですが、保証はされていません。

プロファイル （`ProfileService`）のプライバシーリクエストに製品として ID サービス （`identity`）とデータレイク （`aepDataLake`）も含めたかどうかに応じて、プロファイルに関連する様々なデータセットが、場合によっては異なる時間にシステムから削除されます。

| 含まれる製品 | エフェクト |
| --- | --- |
| `ProfileService` のみ | 削除リクエストが完了したという確認がPrivacy Serviceから送信されると、プロファイルはすぐに削除済みと見なされます。 ただし、プロファイルの ID グラフは残るので、同じ ID を持つ新しいデータが取り込まれると、プロファイルを再構築できる可能性があります。 プロファイルに関連付けられた個人を特定できないデータも、データレイクに残ります。 |
| `ProfileService` および `identity` | プロファイルと関連する ID グラフは、削除リクエストが完了したという確認をPrivacy Serviceが送信するとすぐに、直ちに削除されます。 プロファイルに関連付けられた個人を特定できないデータも、データレイクに残ります。 |
| `ProfileService` および `aepDataLake` | プロファイルは、削除リクエストが完了したという確認をPrivacy Serviceが送信するとすぐに削除されます。 ただし、プロファイルの ID グラフは残るので、同じ ID を持つ新しいデータが取り込まれると、プロファイルを再構築できる可能性があります。<br><br> データレイク製品がリクエストを受信し、現在処理中であることを応答すると、プロファイルに関連付けられたデータはソフト削除されるので、[!DNL Experience Platform] サービスからアクセスできません。 ジョブが完了すると、データはデータレイクから完全に削除されます。 |
| `ProfileService`、`identity` および `aepDataLake` | プロファイルと関連する ID グラフは、削除リクエストが完了したという確認をPrivacy Serviceが送信するとすぐに、直ちに削除されます。<br><br> データレイク製品がリクエストを受信し、現在処理中であることを応答すると、プロファイルに関連付けられたデータはソフト削除されるので、[!DNL Experience Platform] サービスからアクセスできません。 ジョブが完了すると、データはデータレイクから完全に削除されます。 |

ジョブステータスのトラッキングについて詳しくは、[[!DNL Privacy Service]  ドキュメント ](../privacy-service/home.md#monitor) を参照してください。

### プロファイルリクエストと ID リクエスト {#profile-v-identity}

プロファイル（`ProfileService`）に対して削除リクエストが行われたが、ID サービス（`identity`）に対して削除リクエストが行われなかった場合、生成されるジョブでは、顧客（または一連の顧客）について収集された属性データは削除されますが、ID グラフで確立された関連付けは削除されません。

例えば、顧客の ID を使用する削除リクエストで、それらの `email_id` に保存されてい `customer_id` すべての属性データを削除するとします。 ただし、その後、同じ `customer_id` で取り込まれたデータは、関連付けがまだ存在するので、引き続き適切な `email_id` に関連付けられます。

特定の顧客のプロファイルとすべての ID の関連付けを削除するには、必ずプロファイルと ID サービスの両方を削除リクエストのターゲット製品として含めてください。

### 結合ポリシーの制限 {#merge-policy-limitations}

Privacy Serviceは、ID ステッチを実行しない結合ポリシーを使用してのみ [!DNL Profile] データを処理できます。 UI を使用してプライバシーリクエストが処理されているかどうかを確認する場合は、[!UICONTROL ID ステッチ &#x200B;] タイプとして **[!DNL None]** を持つポリシーを使用していることを確認してください。 つまり、[!UICONTROL ID ステッチ &#x200B;] が [!UICONTROL &#x200B; プライベートグラフ &#x200B;] に設定されている結合ポリシーは使用できません。

>![ 結合ポリシーの ID のステッチが「なし」に設定されている ](./images/privacy/no-id-stitch.png)

## 次の手順

このドキュメントでは、[!DNL Experience Platform] におけるプライバシーリクエストの処理に関する重要な概念について説明します。ID データの管理方法とプライバシージョブの作成方法に関する理解を深めるには、引き続きこのガイドに記載されているドキュメントを参照してください。

[!DNL Profile] で使用されない [!DNL Experience Platform] リソースのプライバシーリクエストの処理について詳しくは、[ データレイクでのプライバシーリクエストの処理 ](../catalog/privacy.md) のドキュメントを参照してください。
