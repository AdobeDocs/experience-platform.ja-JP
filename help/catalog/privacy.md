---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: データレイクでのプライバシーリクエストの処理
topic: overview
translation-type: tm+mt
source-git-commit: 1b398e479137a12bcfc3208d37472aae3d6721e1
workflow-type: tm+mt
source-wordcount: '1204'
ht-degree: 25%

---


# プライバシーリクエストの処理( [!DNL Data Lake]

Adobe Experience Platform [!DNL Privacy Service] は、法的および組織のプライバシーに関する規則に従って、個人データにアクセス、販売、または削除するように顧客の要求を処理します。

This document covers essential concepts related to processing privacy requests for customer data stored in the [!DNL Data Lake].

## はじめに

It is recommended that you have a working understanding of the following [!DNL Experience Platform] services before reading this guide:

* [[!DNLPrivacy Service]](../privacy-service/home.md):Adobe Experience Cloudのアプリケーション間での個人データへのアクセス、販売中止、削除に関する顧客の要求を管理します。
* [[!DNLカタログサービス]](home.md):データの場所と内の系列のレコードのシステム [!DNL Experience Platform]。 データセットメタデータの更新に使用できる API を提供します。
* [[!DNL Experience Data Model (XDM) System]](../xdm/home.md):顧客体験データを [!DNL Experience Platform] 整理する際に使用される標準化されたフレームワーク。
* [[!DNL IDサービス]](../identity-service/home.md):デバイスやシステム間でIDをブリッジ化することによって顧客体験データを断片化することによって生じる基本的な課題を解決します。

## ID 名前空間について {#namespaces}

Adobe Experience Platform [!DNL Identity Service] bridges customer identity data across systems and devices. [!DNL Identity Service] は、 **[!UICONTROL identity名前空間]** を使用して、identity値に対するコンテキストを、その接触チャネルのシステムに関連付けることで提供します。 名前空間では、電子メールアドレス（「電子メール」）などの汎用概念を表したり、ID を特定のアプリケーションに関連付けたりすることができます（Adobe Advertising Cloud ID（「AdCloud」）、Adobe Target ID（「TNTID」）など）。

[!DNL Identity Service] グローバルに定義された（標準）ID名前空間とユーザー定義の（カスタム）IDユーザーのストアを維持します。 標準の名前空間はすべての組織（「電子メール」や「ECID」など）で使用できますが、組織は、特定のニーズに合わせてカスタム名前空間を作成することもできます。

For more information about identity namespaces in [!DNL Experience Platform], see the [identity namespace overview](../identity-service/namespaces.md).

## IDデータをデータセットに追加する

のプライバシーリクエストを作成する場合 [!DNL Data Lake]、データを見つけて処理するために、有効なID値(および関連する名前空間)を各顧客に提供する必要があります。 したがって、プライバシー要求の対象となるすべてのデータセットには、関連するXDMスキーマに **[!UICONTROL ID記述子]** が含まれている必要があります。

>[!NOTE]
>
>現在、ID記述子メタデータ（アドホックデータセットなど）をサポートしないスキーマに基づくデータセットは、プライバシー要求で処理できません。

既存のデータセットのXDMスキーマにID記述子を追加する手順を説明します。 ID記述子を持つデータセットが既に存在する場合は、 [次の節に進みます](#nested-maps)。

>[!IMPORTANT]
>
>IDとして設定するスキーマフィールドを決定する場合は、入れ子のマップタイプフィールドを使用する [制限事項に注意してください](#nested-maps)。

データセットスキーマにID記述子を追加するには、次の2つの方法があります。

* [UI の使用](#identity-ui)
* [API の使用](#identity-api)

### UI の使用 {#identity-ui}

ユー [!DNL Experience Platform ]ザーインターフェイスでは、 __ スキーマワークスペースを使用して、既存のXDMスキーマを編集できます。 ID記述子をスキーマに追加するには、リストからスキーマを選択し、チュートリアルのIDフィールドとしてスキーマフィールドを [設定する手順に従い](../xdm/tutorials/create-schema-ui.md#identity-field)[!DNL Schema Editor] ます。

スキーマ内の適切なフィールドをIDフィールドとして設定したら、次のセクションに進んで、プライバシー [要求の送信に関するセクション](#submit)。

### API の使用 {#identity-api}

>[!NOTE]
>
>この節では、データセットのXDMスキーマの固有のURI ID値を把握していることを前提としています。 この値がわからない場合は、 [!DNL Catalog Service] APIを使用して取得できます。 開発者ガイドの「 [はじめに](./api/getting-started.md) 」の節を読んだら、に示す [手順に従って](./api/list-objects.md) 一覧表示するか [](./api/look-up-object.md)[!DNL Catalog] 、データセットを見つけるオブジェクトを調べます。 スキーマIDは、 `schemaRef.id`
>
> この節には、スキーマレジストリAPIの呼び出しが含まれます。 APIの使用に関する重要な情報(コンテナの概念や概念を把握する `{TENANT_ID}` など)については、開発者ガイドの [はじめに](../xdm/api/getting-started.md) （英語のみ）の節を参照してください。

APIでエンドポイントにPOSTリクエストを行うことで、データセットのXDMスキーマにID記述子を追加でき `/descriptors`[!DNL Schema Registry] ます。

**API 形式**

```http
POST /descriptors
```

**リクエスト**

次のリクエストでは、スキーマ例の「email address」フィールドに ID 記述子を定義します。

```shell
curl -X POST \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/descriptors \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '
      {
        "@type": "xdm:descriptorIdentity",
        "xdm:sourceSchema": "https://ns.adobe.com/{TENANT_ID}/schemas/fbc52b243d04b5d4f41eaa72a8ba58be",
        "xdm:sourceVersion": 1,
        "xdm:sourceProperty": "/personalEmail/address",
        "xdm:namespace": "Email",
        "xdm:property": "xdm:code",
        "xdm:isPrimary": false
      }'
```

| プロパティ | 説明 |
| --- | --- |
| `@type` | 作成する記述子の型です。 ID記述子の場合、値は&quot;xdm:descriptorIdentity&quot;にする必要があります。 |
| `xdm:sourceSchema` | データセットのXDMスキーマの一意のURI ID。 |
| `xdm:sourceVersion` | で指定されているXDMスキーマのバージョンで `xdm:sourceSchema`す。 |
| `xdm:sourceProperty` | 記述子を適用するスキーマフィールドへのパス。 |
| `xdm:namespace` | が認識する [標準ID名前空間](../privacy-service/api/appendix.md#standard-namespaces) 、 [!DNL Privacy Service]または組織が定義するカスタム名前空間の1つ。 |
| `xdm:property` | で使用される名前空間に応じて、「xdm:id」または「xdm:code」のいずれか `xdm:namespace`。 |
| `xdm:isPrimary` | オプションのブール値。trueの場合は、フィールドが主IDであることを示します。 スキーマには、1 つのプライマリ ID のみを含めることができます。含めない場合のデフォルトはfalseです。 |

**応答** 

応答が成功すると、HTTPステータス201（作成済み）と、新たに作成された記述子の詳細が返されます。

```json
{
  "@type": "xdm:descriptorIdentity",
  "xdm:sourceSchema": "https://ns.adobe.com/{TENANT_ID}/schemas/fbc52b243d04b5d4f41eaa72a8ba58be",
  "xdm:sourceVersion": 1,
  "xdm:sourceProperty": "/personalEmail/address",
  "xdm:namespace": "Email",
  "xdm:property": "xdm:code",
  "xdm:isPrimary": false,
  "meta:containerId": "tenant",
  "@id": "f3a1dfa38a4871cf4442a33074c1f9406a593407"
}
```

## リクエストの送信 {#submit}

>[!NOTE]
>
>This section covers how to format privacy requests for the [!DNL Data Lake]. It is strongly recommended that you review the [[!DNL Privacy Service] UI](../privacy-service/ui/overview.md) or [[!DNL Privacy Service] API](../privacy-service/api/getting-started.md) documentation for complete steps on how to submit a privacy job, including how to properly format submitted user identity data in request payloads.

次の節では、 [!DNL Data Lake][!DNL Privacy Service] UIまたはAPIを使用してプライバシーをリクエストする方法について概説します。

### UI の使用

When creating job requests in the UI, be sure to select **[!UICONTROL AEP Data Lake]** and/or **[!UICONTROL Profile]** under _[!UICONTROL Products]_ in order to process jobs for data stored in the [!DNL Data Lake] or [!DNL Real-time Customer Profile], respectively.

<img src="images/privacy/product-value.png" width="450"><br>

### API の使用

API でジョブリクエストを作成する場合、提供されるすべての `userIDs` は、適用するデータストアに応じて、特定の `namespace` と `type` を使用する必要があります。IDs for the [!DNL Data Lake] must use &quot;unregistered&quot; for their `type` value, and a `namespace` value that matches one the [privacy labels](#privacy-labels) that have been added to applicable datasets.

さらに、リクエストペイロードの `include` 配列には、リクエストがおこなわれる別のデータストアの製品値を含める必要があります。When making requests to the [!DNL Data Lake], the array must include the value `aepDataLake`.

The following request creates a new privacy job for the [!DNL Data Lake], using the unregistered &quot;email_label&quot; namespace. It also includes the product value for the [!DNL Data Lake] in the `include` array:

```shell
curl -X POST \
  https://platform.adobe.io/data/core/privacy/jobs \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -d '{
    "companyContexts": [
      {
        "namespace": "imsOrgID",
        "value": "{IMS_ORG}"
      }
    ],
    "users": [
      {
        "key": "user12345",
        "action": ["access","delete"],
        "userIDs": [
          {
            "namespace": "email_label",
            "value": "ajones@acme.com",
            "type": "unregistered"
          },
          {
            "namespace": "email_label",
            "value": "jdoe@example.com",
            "type": "unregistered"
          }
        ]
      }
    ],
    "include": ["aepDataLake"],
    "expandIds": false,
    "priority": "normal",
    "regulation": "ccpa"
}'
```

## リクエスト処理の削除

When [!DNL Experience Platform] receives a delete request from [!DNL Privacy Service], [!DNL Platform] sends confirmation to [!DNL Privacy Service] that the request has been received and affected data has been marked for deletion. The records are then removed from the [!DNL Data Lake] within seven days. During that seven-day window, the data is soft-deleted and is therefore not accessible by any [!DNL Platform] service.

In future releases, [!DNL Platform] will send confirmation to [!DNL Privacy Service] after data has been physically deleted.

## 次の手順

By reading this document, you have been introduced to the important concepts involved with processing privacy requests for the [!DNL Data Lake]. ID データの管理方法とプライバシージョブの作成方法に関する理解を深めるために、引き続きこのガイド全体に記載されているドキュメントを読むことをお勧めします。

See the document on [privacy request processing for Real-time Customer Profile](../profile/privacy.md) for steps on processing privacy requests for the [!DNL Profile] store.

## 付録

次の節では、のプライバシー要求を処理するための追加情報について説明し [!DNL Data Lake]ます。

### ネストされたマップタイプフィールドのラベル付け {#nested-maps}

プライバシーのラベル付けをサポートしないネストされたマップタイプフィールドは、次の 2 種類あります。

* 配列型フィールド内のマップタイプフィールド
* 別のマップタイプフィールド内のマップタイプフィールド

上記の 2 つの例のいずれのプライバシージョブの処理も、最終的に失敗します。このため、顧客のプライベートデータを保存する際に、ネストされたマップタイプフィールドを使用しないようにすることをお勧めします。関連する消費者 ID は、レコードベースのデータセットの場合は `identityMap` フィールド（自身はマップタイプのフィールド）内の非マップデータ型、時系列ベースのデータセットの場合は `endUserID` フィールドとして保存する必要があります。