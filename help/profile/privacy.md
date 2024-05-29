---
keywords: Experience Platform;ホーム;人気のトピック
solution: Experience Platform
title: リアルタイム顧客プロファイルでのプライバシーリクエストの処理
type: Documentation
description: Adobe Experience Platform Privacy Service は、プライバシーに関する多数の規則に従って、個人データへのアクセス、販売のオプトアウト、または削除を求める顧客のリクエストを処理します。このドキュメントでは、リアルタイム顧客プロファイルのプライバシーリクエストの処理に関する基本的な概念について説明します。
exl-id: fba21a2e-aaf7-4aae-bb3c-5bd024472214
source-git-commit: e52eb90b64ae9142e714a46017cfd14156c78f8b
workflow-type: tm+mt
source-wordcount: '1743'
ht-degree: 25%

---

# [!DNL Real-Time Customer Profile] でのプライバシーリクエストの処理

Adobe Experience Platform [!DNL Privacy Service] は、EU 一般データ保護規則（GDPR）や [!DNL California Consumer Privacy Act]（CCPA）などのプライバシー規制に従って、個人データへのアクセス、販売のオプトアウト、または削除を求める顧客のリクエストを処理します。

このドキュメントでは、Adobe Experience Platform における [!DNL Real-Time Customer Profile] のプライバシーリクエスト処理に関する基本的な概念について説明します。

>[!NOTE]
>
>このガイドでは、Experience Platform内のプロファイルデータストアに対してプライバシーリクエストを行う方法についてのみ説明します。 Platform データレイクのプライバシーリクエストもおこなう予定がある場合は、のガイドを参照してください。 [データレイクでのプライバシーリクエストの処理](../catalog/privacy.md) このチュートリアルに加えて、
>
>他の Adobe Experience Cloud アプリケーションにプライバシーリクエストを送信する手順については、[Privacy Service のドキュメント](../privacy-service/experience-cloud-apps.md)を参照してください。

>[!IMPORTANT]
>
>このガイドのプライバシーリクエストでは、次のことを行います **ではない** b2B 人物ではないエンティティをカバーします。

## はじめに

このガイドでは、次の点に関する十分な知識が必要です [!DNL Platform] コンポーネント：

* [[!DNL Privacy Service]](../privacy-service/home.md) ：Adobe Experience Cloud アプリケーションをまたいで、自身の個人データのアクセス、販売のオプトアウト、または削除に対する顧客リクエストを管理します。
* [[!DNL Identity Service]](../identity-service/home.md)：デバイスやシステムをまたいで ID を結び付けることで、顧客体験データの断片化によって発生する根本的な課題を解決します。
* [[!DNL Real-Time Customer Profile]](home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

## ID 名前空間について {#namespaces}

Adobe Experience Platform [!DNL Identity Service] は、システムやデバイスをまたいで顧客 ID データを結び付けます。[!DNL Identity Service] は **ID 名前空間**&#x200B;を使用して、ID の値を元のシステムと関連付け、それらの値を識別するコンテキストを提供します。名前空間は、電子メールアドレス（「電子メール」）などの一般的な概念を表すことがあります。また、ID を特定のアプリケーション（Adobe Advertising Cloud ID（「AdCloud」）や Adobe Target ID（「TNTID」）など）に関連付けることができます。

ID サービスは、グローバルに定義された（標準）ID およびユーザー定義の（カスタム）ID 名前空間を保持します。標準の名前空間はすべての組織（「電子メール」や「ECID」など）で使用できますが、組織は、特定のニーズに合わせてカスタム名前空間を作成することもできます。

[!DNL Experience Platform] の ID 名前空間について詳しくは、 [ID 名前空間の概要](../identity-service/features/namespaces.md)を参照してください。

## リクエストの送信 {#submit}

以下のセクションでは、[!DNL Privacy Service] の API または UI を使用して [!DNL Real-Time Customer Profile] に対しプライバシーリクエストを行う方法について概説しています。これらのセクションを読む前に、 [PRIVACY SERVICEAPI](../privacy-service/api/getting-started.md) または [PRIVACY SERVICEUI](../privacy-service/ui/overview.md) ドキュメント。 これらのドキュメントは、リクエストペイロードで送信されたユーザー ID データの形式を適切に設定する方法など、プライバシージョブの送信方法に関する完全な手順を提供します。

>[!IMPORTANT]
>
>Privacy Serviceは処理することしかできません [!DNL Profile] id ステッチを実行しない結合ポリシーを使用するデータ。 の節を参照してください。 [結合ポリシーの制限](#merge-policy-limitations) を参照してください。
>
>プライバシーリクエストは、規制要件内で非同期に処理され、完了するまでにかかる時間は様々です。 で変更があった場合 [!DNL Profile] リクエストの処理中にデータが送信されても、受信したレコードがそのリクエストで処理されることは保証されません。 プライバシージョブがリクエストされた時点でデータレイクまたはプロファイルストアに保持されているプロファイルのみが削除されることが保証されます。 削除ジョブの実行中に、削除リクエストのサブジェクトに関連するプロファイルデータを取り込んだ場合、すべてのプロファイルフラグメントが削除されるとは限りません。
>削除要求時に Platform またはプロファイルサービスから受信するデータを認識するのは、お客様の責任です。このデータはレコードストアに挿入されるからです。 削除中または削除中のデータの取り込みに注意する必要があります。

### API の使用

API でジョブリクエストを作成する際は、`userIDs` 内で指定するいずれの ID に対しても固有の `namespace` および `type` を使用する必要があります。[!DNL Identity Service] によって認識される有効な [ID 名前空間](#namespaces) を `namespace` 値に指定する必要があり、`type` には、`standard` または `unregistered` のいずれかを（標準名前空間とカスタム名前空間のそれぞれに応じて）指定する必要があります。

>[!NOTE]
>
>ID グラフと Platform データセットでのプロファイルフラグメントの配布方法によっては、各顧客に対して複数の ID を指定する必要が生じる場合があります。 次の節を参照してください [プロファイルフラグメント](#fragments) を参照してください。

さらに、リクエストペイロードの `include` 配列には、リクエスト対象である別のデータストアの製品値を含める必要があります。ID に関連付けられたプロファイルデータを削除するには、配列に値が含まれている必要があります `ProfileService`. 顧客の ID グラフの関連付けを削除するには、配列に値を含める必要があります `identity`.

>[!NOTE]
>
>の節を参照してください。 [プロファイルリクエストと id リクエスト](#profile-v-identity) を使用した場合の影響について詳しくは、このドキュメントの後半で説明します。 `ProfileService` および `identity` 内 `include` 配列。

次のリクエストは、1 件の顧客データ用に新しいプライバシージョブをで作成します。 [!DNL Profile] ストア。 で顧客に対して 2 つの ID 値が指定されています `userIDs` 配列。標準を使用するもの `Email` id 名前空間、およびカスタムを使用するその他の名前空間 `Customer_ID` 名前空間。 また、の製品値も含まれます [!DNL Profile] （`ProfileService`） `include` 配列：

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
>Platform は、組織に属するすべての[サンドボックス](../sandboxes/home.md)でプライバシーリクエストを処理します。その結果、リクエストに含まれる `x-sandbox-name` ヘッダーはシステムによって無視されます。

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

UI でジョブリクエストを作成する場合は、必ず次を選択します。 **[!UICONTROL AEP データレイク]** および/または **[!UICONTROL Profile]** 未満 **[!UICONTROL 製品]** データレイクに保存されたデータのジョブを処理するため、または [!DNL Real-Time Customer Profile]、それぞれ。

![UI で作成され、製品でプロファイルオプションが選択されたアクセスジョブリクエスト](./images/privacy/product-value.png)

## プライバシーリクエストのプロファイルフラグメント {#fragments}

が含まれる [!DNL Profile] データストア。個々の顧客の個人データは、多くの場合、複数のプロファイルフラグメントで構成され、id グラフを通じて人物に関連付けられます。 にプライバシーリクエストを送信する場合 [!DNL Profile] ストア。リクエストは、プロファイル全体ではなく、プロファイルフラグメントレベルでのみ処理されることに注意してください。

例えば、顧客属性データを 3 つの異なるデータセットに格納し、それらのデータセットで異なる識別子を使用してデータを個々の顧客に関連付ける場合を考えてみましょう。

| データセット名 | プライマリ ID フィールド | 保存された属性 |
| --- | --- | --- |
| データセット 1 | `customer_id` | `address` |
| データセット 2 | `email_id` | `firstName`、`lastName` |
| データセット 3 | `email_id` | `mlScore` |

いずれかのデータセットはを使用します `customer_id` プライマリ識別子としての使用に対して、他の 2 つは `email_id`. を使用してプライバシーリクエスト（アクセスまたは削除）を送信する場合 `email_id` ユーザー ID の値として、 `firstName`, `lastName`、および `mlScore` 属性が処理される一方、 `address` 影響を受けません。

プライバシーリクエストがすべての関連する顧客属性を処理するようにするには、これらの属性を保存できるすべての適用可能なデータセットにプライマリ ID 値を指定する必要があります（顧客あたり最大 9 つの ID）。 の ID フィールドについては、の節を参照してください [スキーマ構成の基本](../xdm/schema/composition.md#identity) 一般的に ID としてマークされるフィールドの詳細を確認してください。

## リクエスト処理の削除 {#delete}

[!DNL Experience Platform] が [!DNL Privacy Service] から削除リクエストを受信すると、[!DNL Platform] は、[!DNL Privacy Service] に対し、リクエストを受信し、影響を受けるデータが削除用にマークされている旨の確認を送信します。その後、プライバシージョブが完了すると、レコードが削除されます。

>[!IMPORTANT]
>
>プライバシーの削除リクエストは即時には行われず、関係するサービスや、地理的な場所などのその他の影響を受ける要因によって異なる場合があります。 プライバシージョブの完了期間は 15～45 日ですが、保証はされていません。

ID サービスも含めたかどうかに応じて（`identity`）とデータレイク（`aepDataLake`）をプロファイルのプライバシーリクエストの製品として使用します（`ProfileService`）、プロファイルに関連する様々なデータセットが、異なる時間にシステムから削除される可能性があります。

| 含まれる製品 | エフェクト |
| --- | --- |
| `ProfileService` のみ | プロファイルは、削除リクエストが受信されたという確認を Platform が送信するとすぐに削除されます。 ただし、プロファイルの ID グラフは残るので、同じ ID を持つ新しいデータが取り込まれると、プロファイルを再構築できる可能性があります。 プロファイルに関連付けられたデータもデータレイクに残ります。 |
| `ProfileService` および `identity` | プロファイルと関連する ID グラフは、削除リクエストが受信されたという確認を Platform が送信するとすぐに、直ちに削除されます。 プロファイルに関連付けられたデータは、データレイクに残ります。 |
| `ProfileService` および `aepDataLake` | プロファイルは、削除リクエストが受信されたという確認を Platform が送信するとすぐに削除されます。 ただし、プロファイルの ID グラフは残るので、同じ ID を持つ新しいデータが取り込まれると、プロファイルを再構築できる可能性があります。<br><br>データレイク製品がリクエストを受信し、現在処理中であることを応答すると、プロファイルに関連付けられたデータはソフト削除されるので、どのユーザーもアクセスできません [!DNL Platform] サービス。 ジョブが完了すると、データはデータレイクから完全に削除されます。 |
| `ProfileService`, `identity`、および `aepDataLake` | プロファイルと関連する ID グラフは、削除リクエストが受信されたという確認を Platform が送信するとすぐに、直ちに削除されます。<br><br>データレイク製品がリクエストを受信し、現在処理中であることを応答すると、プロファイルに関連付けられたデータはソフト削除されるので、どのユーザーもアクセスできません [!DNL Platform] サービス。 ジョブが完了すると、データはデータレイクから完全に削除されます。 |

を参照してください。 [[!DNL Privacy Service] 詳細を見る](../privacy-service/home.md#monitor) ジョブステータスのトラッキングの詳細情報

### プロファイルリクエストと ID リクエスト {#profile-v-identity}

プロファイルの削除リクエストが行われた場合（`ProfileService`）、ID サービス （`identity`）を選択した場合、結果のジョブでは、顧客（または顧客のセット）について収集された属性データは削除されますが、ID グラフで確立された関連付けは削除されません。

例えば、顧客のを使用する削除リクエストの場合 `email_id` および `customer_id` これらの ID の下に保存されているすべての属性データを削除します。 ただし、その後同じ下に取り込まれたデータ `customer_id` は、引き続き適切なに関連付けられます `email_id`関連付けがまだ存在するので、

特定の顧客のプロファイルとすべての ID の関連付けを削除するには、必ずプロファイルと ID サービスの両方を削除リクエストのターゲット製品として含めてください。

### 結合ポリシーの制限 {#merge-policy-limitations}

Privacy Serviceは処理することしかできません [!DNL Profile] id ステッチを実行しない結合ポリシーを使用するデータ。 UI を使用してプライバシーリクエストが処理されているかどうかを確認する場合は、でポリシーを使用していることを確認してください **[!DNL None]** as its [!UICONTROL ID ステッチ] タイプ。 つまり、次の場合は結合ポリシーを使用できません。 [!UICONTROL ID ステッチ] はに設定されています。 [!UICONTROL プライベートグラフ].

>![結合ポリシーの ID のステッチは、「なし」に設定されています](./images/privacy/no-id-stitch.png)

## 次の手順

このドキュメントでは、[!DNL Experience Platform] におけるプライバシーリクエストの処理に関する重要な概念について説明します。ID データの管理方法とプライバシージョブの作成方法に関する理解を深めるには、引き続きこのガイドに記載されているドキュメントを参照してください。

のプライバシーリクエストの処理について [!DNL Platform] が使用していないリソース [!DNL Profile]のドキュメントを参照してください。 [データレイクでのプライバシーリクエストの処理](../catalog/privacy.md).
