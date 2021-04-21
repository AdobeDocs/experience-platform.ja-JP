---
keywords: Experience Platform；ホーム；人気のあるトピック；Data Lakeのプライバシー；アイデンティティ名前空間；プライバシー；Data Lake
solution: Experience Platform
title: データレークでのプライバシー要求の処理
topic-legacy: overview
description: Adobe Experience Platform Privacy Serviceは、法的および組織のプライバシーに関する規則に基づオプトアウトいて記述された個人データにアクセス、販売、または削除するように顧客の要求を処理します。 このドキュメントでは、データレイクに保存された顧客データのプライバシーリクエストの処理に関する基本的な概念について説明します。
exl-id: c06b0a44-be1a-4938-9c3e-f5491a3dfc19
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '1285'
ht-degree: 27%

---

# [!DNL Data Lake]のプライバシーリクエスト処理

Adobe Experience Platform[!DNL Privacy Service]は、法的および組織のプライバシーに関する規則に基づいて説明したとおり、個人データにアクセス、販売、または削除を求める顧客の要求を処理します。

このドキュメントでは、[!DNL Data Lake]に保存された顧客データに対するプライバシー要求の処理に関する基本的な概念について説明します。

## はじめに

このガイドを読む前に、次の[!DNL Experience Platform]サービスに関する作業を理解しておくことをお勧めします。

* [[!DNL Privacy Service]](../privacy-service/home.md) ：Adobe Experience Cloud アプリケーションをまたいで、自身の個人データのアクセス、販売のオプトアウト、または削除に対する顧客リクエストを管理します。
* [[!DNL Catalog Service]](home.md):データの場所と内の系列のレコードのシステム [!DNL Experience Platform]。データセットメタデータの更新に使用できる API を提供します。
* [[!DNL Experience Data Model (XDM) System]](../xdm/home.md)：顧客体験データを編成する際に [!DNL Experience Platform] に使用される標準化されたフレームワーク。
* [[!DNL Identity Service]](../identity-service/home.md):デバイスやシステム間でIDをブリッジ化することによって顧客体験データを断片化することによって生じる基本的な課題を解決します。

## ID 名前空間について {#namespaces}

Adobe Experience Platform[!DNL Identity Service]は、システムとデバイス間で顧客のIDデータを結合します。 [!DNL Identity Service] identity名前空間を使用して、identity値を接触チャネルのシステムに関連付けることで、identity値のコンテキストを提供します。名前空間では、電子メールアドレス（「電子メール」）などの汎用概念を表したり、ID を特定のアプリケーションに関連付けたりすることができます（Adobe Advertising Cloud ID（「AdCloud」）、Adobe Target ID（「TNTID」）など）。

[!DNL Identity Service] グローバルに定義された（標準）ID名前空間とユーザー定義の（カスタム）IDユーザーのストアを維持します。標準の名前空間はすべての組織（「電子メール」や「ECID」など）で使用できますが、組織は、特定のニーズに合わせてカスタム名前空間を作成することもできます。

[!DNL Experience Platform]のID名前空間について詳しくは、[ID名前空間の概要](../identity-service/namespaces.md)を参照してください。

## IDデータをデータセットに追加する

[!DNL Data Lake]のプライバシーリクエストを作成する場合、データを見つけて処理するには、有効なID値(および関連する名前空間)を各顧客に提供する必要があります。 したがって、プライバシー要求の対象となるすべてのデータセットには、関連するXDMスキーマにID記述子が含まれている必要があります。

>[!NOTE]
>
>現在、ID記述子メタデータ（アドホックデータセットなど）をサポートしないスキーマに基づくデータセットは、プライバシー要求で処理できません。

既存のデータセットのXDMスキーマにID記述子を追加する手順を説明します。 既にID記述子を持つデータセットが存在する場合は、[次のセクション](#nested-maps)に進みます。

>[!IMPORTANT]
>
>IDとして設定するスキーマフィールドを決定する際は、ネストされたマップタイプのフィールド](#nested-maps)を使用する場合の[制限事項に注意してください。

データセットスキーマにID記述子を追加するには、次の2つの方法があります。

* [UI の使用](#identity-ui)
* [API の使用](#identity-api)

### UI の使用 {#identity-ui}

[!DNL Experience Platform ]ユーザーインターフェイスでは、**[!UICONTROL スキーマ]**&#x200B;ワークスペースを使用して、既存のXDMスキーマを編集できます。 スキーマにID記述子を追加するには、リストからスキーマを選択し、[!DNL Schema Editor]チュートリアルの[IDフィールドとしてスキーマフィールドを設定する](../xdm/tutorials/create-schema-ui.md#identity-field)の手順に従います。

スキーマ内の適切なフィールドをIDフィールドとして設定したら、[プライバシー要求の送信](#submit)の次のセクションに進むことができます。

### API の使用 {#identity-api}

>[!NOTE]
>
>この節では、データセットのXDMスキーマの固有のURI ID値を把握していることを前提としています。 この値がわからない場合は、[!DNL Catalog Service] APIを使用して取得できます。 開発ガイドの[はじめに](./api/getting-started.md)を読んだ後、[](./api/list-objects.md)のリストまたは[](./api/look-up-object.md)[!DNL Catalog]オブジェクトを参照してデータセットを探すのに必要な手順に従ってください。 スキーマIDは`schemaRef.id`の下にあります
>
> この節には、スキーマレジストリAPIの呼び出しが含まれます。 `{TENANT_ID}`やコンテナの概念を把握するなど、APIの使用に関する重要な情報については、開発者ガイドの[はじめに](../xdm/api/getting-started.md)の節を参照してください。

[!DNL Schema Registry] APIの`/descriptors`エンドポイントにPOSTリクエストを行うことで、データセットのXDMスキーマにID記述子を追加できます。

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
| `xdm:sourceVersion` | `xdm:sourceSchema`で指定されたXDMスキーマのバージョン。 |
| `xdm:sourceProperty` | 記述子を適用するスキーマフィールドへのパス。 |
| `xdm:namespace` | [!DNL Privacy Service]が認識する[標準ID名前空間](../privacy-service/api/appendix.md#standard-namespaces)の1つ、または組織が定義するカスタム名前空間。 |
| `xdm:property` | `xdm:namespace`で使用される名前空間に応じて、&quot;xdm:id&quot;または&quot;xdm:code&quot;を指定します。 |
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
>この節では、[!DNL Data Lake]のプライバシー要求をフォーマットする方法について説明します。 プライバシージョブの送信方法に関する完全な手順（リクエストペイロードで送信されたユーザーIDデータを適切にフォーマットする方法など）については、[[!DNL Privacy Service] UI](../privacy-service/ui/overview.md)または[[!DNL Privacy Service] API](../privacy-service/api/getting-started.md)のドキュメントを確認することを強くお勧めします。

次の節では、[!DNL Privacy Service] UIまたはAPIを使用して[!DNL Data Lake]のプライバシーリクエストを行う方法について概説します。

>[!IMPORTANT]
>
>プライバシー要求が完了するまでに要する時間を保証することはできません。 要求の処理中にデータレーク内で変更が発生した場合、それらのレコードが処理されるかどうかも保証できません。

### UI の使用

UIでジョブリクエストを作成する場合、[!DNL Data Lake]または[!DNL Real-time Customer Profile]に保存されたデータのジョブを処理するには、**[!UICONTROL Products]**&#x200B;の下で&#x200B;**[!UICONTROL AEP Data Lake]**&#x200B;または&#x200B;**[!UICONTROL プロファイル]**&#x200B;を必ず選択してください。

<img src="images/privacy/product-value.png" width="450"><br>

### API の使用

API でジョブリクエストを作成する場合、提供されるすべての `userIDs` は、適用するデータストアに応じて、特定の `namespace` と `type` を使用する必要があります。[!DNL Data Lake]のIDは、`type`値に「unregistered」を使用する必要があります。また、`namespace`値は、該当するデータセットに追加された[プライバシーラベル](#privacy-labels)と一致する必要があります。

さらに、リクエストペイロードの `include` 配列には、リクエストがおこなわれる別のデータストアの製品値を含める必要があります。[!DNL Data Lake]にリクエストを行う場合、配列には値`aepDataLake`を含める必要があります。

次のリクエストは、未登録の「email_label」名前空間を使用して、[!DNL Data Lake]の新しいプライバシージョブを作成します。 `include`配列の[!DNL Data Lake]の製品値も含みます。

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

[!DNL Experience Platform]が[!DNL Privacy Service]から削除要求を受け取ると、[!DNL Platform]は、要求が受信され、影響を受けたデータが削除のマークが付けられたことを[!DNL Privacy Service]に確認します。 レコードは7日以内に[!DNL Data Lake]から削除されます。 この7日間の間は、データはソフトデリートされるので、[!DNL Platform]サービスからはアクセスできません。

今後のリリースでは、[!DNL Platform]は、データが物理的に削除された後、[!DNL Privacy Service]に確認を送ります。

## 次の手順

このドキュメントを読むと、[!DNL Data Lake]のプライバシー要求を処理するのに関する重要な概念について紹介されます。 ID データの管理方法とプライバシージョブの作成方法に関する理解を深めるために、引き続きこのガイド全体に記載されているドキュメントを読むことをお勧めします。

[!DNL Profile]ストアのプライバシー要求を処理する手順については、[リアルタイム顧客プロファイルのプライバシー要求処理](../profile/privacy.md)のドキュメントを参照してください。

## 付録

次の節では、[!DNL Data Lake]でのプライバシー要求の処理に関する追加情報を説明します。

### ネストされたマップタイプフィールドのラベル付け {#nested-maps}

プライバシーのラベル付けをサポートしないネストされたマップタイプフィールドは、次の 2 種類あります。

* 配列型フィールド内のマップタイプフィールド
* 別のマップタイプフィールド内のマップタイプフィールド

上記の 2 つの例のいずれのプライバシージョブの処理も、最終的に失敗します。このため、顧客のプライベートデータを保存する際に、ネストされたマップタイプフィールドを使用しないようにすることをお勧めします。関連する消費者 ID は、レコードベースのデータセットの場合は `identityMap` フィールド（自身はマップタイプのフィールド）内の非マップデータ型、時系列ベースのデータセットの場合は `endUserID` フィールドとして保存する必要があります。
