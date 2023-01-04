---
keywords: Experience Platform;ホーム;人気のトピック;Data Lake のプライバシー;ID 名前空間;プライバシー;Data Lake
solution: Experience Platform
title: データレイクでのプライバシーリクエストの処理
topic-legacy: overview
description: Adobe Experience Platform Privacy Service は、法的および組織のプライバシーに関する規則に従って、個人データへのアクセス、販売のオプトアウト、または削除を求める顧客のリクエストを処理します。このドキュメントでは、データレイクに保存された顧客データのプライバシーリクエストの処理に関する基本的な概念について説明します。
exl-id: c06b0a44-be1a-4938-9c3e-f5491a3dfc19
source-git-commit: 34e0381d40f884cd92157d08385d889b1739845f
workflow-type: tm+mt
source-wordcount: '1428'
ht-degree: 73%

---

# データレイクでのプライバシーリクエストの処理

Adobe Experience Platform [!DNL Privacy Service] は、法的および組織のプライバシーに関する規則に従って、個人データに対するアクセス、販売のオプトアウト、または削除を求める顧客のリクエストを処理します。

このドキュメントでは、データレイクに保存された顧客データのプライバシーリクエストの処理に関する基本的な概念について説明します。

>[!NOTE]
>
>このガイドでは、Experience Platformのデータレイクに対してプライバシーリクエストを実行する方法についてのみ説明します。 リアルタイム顧客プロファイルデータストアに対してもプライバシーリクエストをおこなう予定がある場合は、 [プロファイルのプライバシーリクエストの処理](../profile/privacy.md) を追加しました。
>
>他の Adobe Experience Cloud アプリケーションにプライバシーリクエストを送信する手順については、[Privacy Service のドキュメント](../privacy-service/experience-cloud-apps.md)を参照してください。

## はじめに

このガイドを読む前に、次の [!DNL Experience Platform] サービスに関する十分な理解を得ることをお勧めします。

* [[!DNL Privacy Service]](../privacy-service/home.md) ：Adobe Experience Cloud アプリケーションをまたいで、自身の個人データのアクセス、販売のオプトアウト、または削除に対する顧客リクエストを管理します。
* [[!DNL Catalog Service]](home.md)：[!DNL Experience Platform] 内のデータの場所と系列のレコードのシステム。データセットメタデータの更新に使用できる API を提供します。
* [[!DNL Experience Data Model (XDM) System]](../xdm/home.md)：顧客体験データを編成する際に [!DNL Experience Platform] に使用される標準化されたフレームワーク。
* [[!DNL Identity Service]](../identity-service/home.md)：デバイスやシステムをまたいで ID を結び付けることで、顧客体験データの断片化によって発生する根本的な課題を解決します。

## ID 名前空間について {#namespaces}

Adobe Experience Platform [!DNL Identity Service] は、システムやデバイスをまたいで顧客 ID データを結び付けます。[!DNL Identity Service] は ID 名前空間を使用して、ID の値を元のシステムと関連付け、それらの値に対するコンテキストを提供します。名前空間は、電子メールアドレス（「電子メール」）などの一般的な概念を表すことがあります。また、ID を特定のアプリケーション（Adobe Advertising Cloud ID（「AdCloud」）や Adobe Target ID（「TNTID」）など）に関連付けることができます。

[!DNL Identity Service] は、グローバルに定義された（標準）ID およびユーザー定義の（カスタム）ID 名前空間を保持します。標準の名前空間はすべての組織（「電子メール」や「ECID」など）で使用できますが、組織は、特定のニーズに合わせてカスタム名前空間を作成することもできます。

[!DNL Experience Platform] の ID 名前空間について詳しくは、 [ID 名前空間の概要](../identity-service/namespaces.md)を参照してください。

## ID データをデータセットに追加

データレイクのプライバシーリクエストを作成する場合、データを見つけて適切に処理するために、有効な ID 値（および関連する名前空間）を各顧客に提供する必要があります。 したがって、プライバシーリクエストの対象となるすべてのデータセットには、関連する XDM スキーマに ID 記述子が含まれている必要があります。

>[!NOTE]
>
>現在、ID 記述子メタデータ（アドホックデータセットなど）をサポートしないスキーマに基づくデータセットは、プライバシーリクエストで処理できません。

この節では、既存のデータセットの XDM スキーマに ID 記述子を追加する手順を説明します。ID 記述子を持つデータセットが既に存在する場合は、[次の節](#nested-maps)に進みます。

>[!IMPORTANT]
>
>ID として設定するスキーマフィールドを決定する際は、[ネストされたマップタイプのフィールドを使用する場合の制限事項](#nested-maps)に注意してください。

データセットスキーマに ID 記述子を追加するには、次の 2 つの方法があります。

* [UI の使用](#identity-ui)
* [API の使用](#identity-api)

### UI の使用 {#identity-ui}

[!DNL Experience Platform ]ユーザーインターフェイスでは、**[!UICONTROL スキーマ]**&#x200B;ワークスペースを使用して、既存の XDM スキーマを編集できます。スキーマに ID 記述子を追加するには、リストからスキーマを選択し、[!DNL Schema Editor] チュートリアルの[ID フィールドとしてスキーマフィールドを設定する](../xdm/tutorials/create-schema-ui.md#identity-field)の手順に従います。

スキーマ内の適切なフィールドを ID フィールドとして設定したら、[プライバシーリクエストの送信](#submit)に関する次の節に進むことができます。

### API の使用 {#identity-api}

>[!NOTE]
>
>この節では、データセットの XDM スキーマの固有の URI ID 値を把握していることを前提としています。この値がわからない場合は、[!DNL Catalog Service] API を使用して取得できます。デベロッパーガイドの[はじめに](./api/getting-started.md)の節を読んだ後、[!DNL Catalog] オブジェクトの[リスト](./api/list-objects.md)または[検索](./api/look-up-object.md)で概説されている手順に従ってください。スキーマ ID は `schemaRef.id` の下にあります。
>
>また、この節では、スキーマレジストリ API の呼び出し方法を理解していることを前提としています。 `{TENANT_ID}` やコンテナの概念を把握するなど、API の使用に関連する重要な情報については、API ガイドの「[はじめに](../xdm/api/getting-started.md)」の節を参照してください。

[!DNL Schema Registry] API の `/descriptors` エンドポイントに POST リクエストをおこなうことで、データセットの XDM スキーマに ID 記述子を追加できます。

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
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
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
| `@type` | 作成する記述子のタイプ。ID 記述子の場合、値は「xdm:descriptorIdentity」にする必要があります。 |
| `xdm:sourceSchema` | データセットの XDM スキーマの一意の URI ID。 |
| `xdm:sourceVersion` | `xdm:sourceSchema` で指定された XDM スキーマのバージョン。 |
| `xdm:sourceProperty` | 記述子を適用するスキーマフィールドへのパス。 |
| `xdm:namespace` | [!DNL Privacy Service] が認識する[標準 ID 名前空間](../privacy-service/api/appendix.md#standard-namespaces)の 1 つ、または組織が定義するカスタム名前空間。 |
| `xdm:property` | `xdm:namespace` で使用される名前空間に応じて、「xdm:id」または「xdm:code」を指定します。 |
| `xdm:isPrimary` | オプションのブール値。true の場合は、フィールドがプライマリ ID であることを示します。スキーマには、1 つのプライマリ ID のみを含めることができます。含めない場合のデフォルトは false です。 |

**応答** 

応答が成功すると、HTTP ステータス 201（作成済み）と、新しく作成された記述子の詳細が返されます。

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
>この節では、データレイクのプライバシーリクエストの形式を設定する方法について説明します。 リクエストペイロードで送信されたユーザー ID データの形式を適切に設定する方法など、プライバシージョブの送信方法に関する完全な手順については、[[!DNL Privacy Service] UI](../privacy-service/ui/overview.md) または [[!DNL Privacy Service] API](../privacy-service/api/getting-started.md) のドキュメントを確認することを強くお勧めします。

次の節では、 [!DNL Privacy Service] UI または API。

>[!IMPORTANT]
>
>プライバシーリクエストが完了するまでに要する時間を確定することはできません。リクエストの処理中にデータレイク内で変更が発生した場合、それらのレコードが処理されるかどうかも保証できません。

### UI の使用

UI でジョブリクエストを作成する場合は、必ず **[!UICONTROL AEP データレイク]** under **[!UICONTROL 製品]** データレイクに保存されたデータのジョブを処理するために使用されます。

![プライバシーリクエスト作成ダイアログで選択されたデータレイク製品を示す画像](./images/privacy/product-value.png)

### API の使用

API でジョブリクエストを作成する場合、提供されるすべての `userIDs` は、適用するデータストアに応じて、特定の `namespace` と `type` を使用する必要があります。データレイクの ID は、を使用する必要があります `unregistered` その `type` 値と `namespace` 一致する値 [プライバシーラベル](#privacy-labels) を適用可能なデータセットに追加しました。

さらに、リクエストペイロードの `include` 配列には、リクエスト対象である別のデータストアの製品値を含める必要があります。データレイクにリクエストを送信する場合、配列には `aepDataLake`.

次のリクエストは、未登録の `email_label` 名前空間。 また、 `include` 配列：

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

>[!IMPORTANT]
>
>Platform は、組織に属するすべての[サンドボックス](../sandboxes/home.md)でプライバシーリクエストを処理します。その結果、リクエストに含まれる `x-sandbox-name` ヘッダーはシステムによって無視されます。

## リクエスト処理の削除

[!DNL Experience Platform] が [!DNL Privacy Service] から削除リクエストを受信すると、[!DNL Platform] は、[!DNL Privacy Service] に対し、リクエストを受信し、影響を受けるデータが削除用にマークされている旨の確認を送信します。その後、7 日以内にレコードがデータレイクから削除されます。 この 7 日間の期間中、データはソフト削除されるので、どの [!DNL Platform] サービスからもアクセスできません。

また、 `ProfileService` または `identity` プライバシーリクエストでは、関連するデータが別々に処理されます。 詳しくは、 [プロファイルの削除リクエスト処理](../profile/privacy.md#delete) を参照してください。

## 次の手順

このドキュメントでは、データレイクのプライバシーリクエストの処理に関する重要な概念を紹介しました。 ID データの管理方法とプライバシージョブの作成方法に関する理解を深めるために、引き続きこのガイド全体に記載されているドキュメントを読むことをお勧めします。

次のドキュメントを参照してください： [リアルタイム顧客プロファイルのプライバシーリクエスト処理](../profile/privacy.md) のプライバシーリクエストの処理手順を参照してください。 [!DNL Profile] ストア。

## 付録

次の節には、データレイクでプライバシーリクエストを処理するための追加情報が含まれています。

### ネストされたマップタイプフィールドのラベル付け {#nested-maps}

プライバシーのラベル付けをサポートしないネストされたマップタイプフィールドは、次の 2 種類あります。

* 配列型フィールド内のマップタイプフィールド
* 別のマップタイプフィールド内のマップタイプフィールド

上記の 2 つの例のいずれのプライバシージョブの処理も、最終的に失敗します。このため、顧客のプライベートデータを保存する際に、ネストされたマップタイプフィールドを使用しないようにすることをお勧めします。関連する消費者 ID は、レコードベースのデータセットの場合は `identityMap` フィールド（自身はマップタイプのフィールド）内の非マップデータ型、時系列ベースのデータセットの場合は `endUserID` フィールドとして保存する必要があります。
