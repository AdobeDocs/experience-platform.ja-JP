---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: データレークでのプライバシー要求の処理
topic: overview
translation-type: tm+mt
source-git-commit: bfbf2074a9dcadd809de043d62f7d2ddaa7c7b31
workflow-type: tm+mt
source-wordcount: '1187'
ht-degree: 0%

---


# プライバシーリクエストの処理( [!DNL Data Lake]

Adobe Experience Platform [!DNL Privacy Service] は、法的および組織のプライバシーに関する規則に基づいて説明された個人データにアクセス、販売、または削除するように顧客の要求を処理します。

このドキュメントでは、に保存された顧客データのプライバシーリクエストを処理する際に必要となる基本的な概念について説明 [!DNL Data Lake]します。

## はじめに

このガイドを読む前に、次の [!DNL Experience Platform] サービスに関する作業上の理解を得ておくことをお勧めします。

* [!DNL Privacy Service](../privacy-service/home.md): Adobe Experience Cloudアプリケーションで個人データにアクセス、オプトアウト、削除を行う際の顧客の要求を管理します。
* [!DNL Catalog Service](home.md): データの場所と内の系列のレコードのシステム [!DNL Experience Platform]。 データセットメタデータの更新に使用できるAPIを提供します。
* [!DNL Experience Data Model (XDM) System](../xdm/home.md): 顧客体験データを [!DNL Experience Platform] 整理するための標準化されたフレームワーク。
* [!DNL Identity Service](../identity-service/home.md): デバイスやシステム間でIDをブリッジ化することによって顧客体験データを断片化することによって生じる基本的な課題を解決します。

## ID名前空間について {#namespaces}

Adobe Experience Platform [!DNL Identity Service] は、システムやデバイス間で顧客のIDデータを結合します。 [!DNL Identity Service] は、 **[!UICONTROL identity名前空間]** を使用して、identity値に対するコンテキストを、その接触チャネルのシステムに関連付けることで提供します。 名前空間は、電子メールアドレス（「電子メール」）などの汎用的な概念を表したり、IDを特定のAdvertising Cloud(AdCloud)やAdobe TargetID(「TNTID」)など)に関連付けたりできます。

[!DNL Identity Service] グローバルに定義された（標準）ID名前空間とユーザー定義の（カスタム）IDユーザーのストアを維持します。 標準名前空間は、すべての組織（「電子メール」や「ECID」など）で使用できますが、組織は、特定のニーズに合わせてカスタム名前空間を作成することもできます。

のID名前空間について詳し [!DNL Experience Platform]くは、「 [ID名前空間の概要](../identity-service/namespaces.md)」を参照してください。

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

* [UIの使用](#identity-ui)
* [APIの使用](#identity-api)

### UIの使用 {#identity-ui}

ユー [!DNL Experience Platform ]ザーインターフェイスでは、 __スキーマワークスペースを使用して、既存のXDMスキーマを編集できます。 ID記述子をスキーマに追加するには、リストからスキーマを選択し、チュートリアルのIDフィールドとしてスキーマフィールドを[設定する手順に従い](../xdm/tutorials/create-schema-ui.md#identity-field)[!DNL Schema Editor]ます。

スキーマ内の適切なフィールドをIDフィールドとして設定したら、次のセクションに進んで、プライバシー [要求の送信に関するセクション](#submit)。

### Using the API {#identity-api}

>[!NOTE]
>
>この節では、データセットのXDMスキーマの固有のURI ID値を把握していることを前提としています。 この値がわからない場合は、 [!DNL Catalog Service] APIを使用して取得できます。 開発者ガイドの「 [はじめに](./api/getting-started.md) 」の節を読んだら、に示す [手順に従って](./api/list-objects.md) 一覧表示するか [](./api/look-up-object.md)[!DNL Catalog] 、データセットを見つけるオブジェクトを調べます。 スキーマIDは、 `schemaRef.id`
>
> この節には、スキーマレジストリAPIの呼び出しが含まれます。 APIの使用に関する重要な情報(コンテナの概念や概念を把握する `{TENANT_ID}` など)については、開発者ガイドの [はじめに](../xdm/api/getting-started.md) （英語のみ）の節を参照してください。

APIでエンドポイントにPOSTリクエストを行うことで、データセットのXDMスキーマにID記述子を追加でき `/descriptors`[!DNL Schema Registry] ます。

**API形式**

```http
POST /descriptors
```

**リクエスト**

次のリクエストは、サンプルスキーマの「電子メールアドレス」フィールドにID記述子を定義します。

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
| `xdm:isPrimary` | オプションのboolean値。 trueの場合は、フィールドが主IDであることを示します。 スキーマには、1つのプライマリIDのみを含めることができます。 含めない場合のデフォルトはfalseです。 |

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

## 要求の送信 {#submit}

>[!NOTE]
>
>この節では、のプライバシー要求を書式設定する方法について説明 [!DNL Data Lake]します。 プライバシージョブの送信方法に関する完全な手順（リクエストペイロードで送信されたユーザーIDデータを適切に書式設定する方法など）については、 [!DNL Privacy Service UI](../privacy-service/ui/overview.md) または [!DNL Privacy Service API](../privacy-service/api/getting-started.md) ドキュメントを確認することを強くお勧めします。

次の節では、 [!DNL Data Lake][!DNL Privacy Service] UIまたはAPIを使用してプライバシーをリクエストする方法について概説します。

### UIの使用

UIでジョブリクエストを作成する場合、Productsの下で **[!UICONTROL AEP Data Lake]** または **[!UICONTROL プロファイル]** （またはその両方）を選択して、それぞれ _[!UICONTROL に格納されているジョブを]_[!DNL Data Lake][!DNL Real-time Customer Profile]UIで処理します。

<img src="images/privacy/product-value.png" width="450"><br>

### APIの使用

APIでジョブリクエストを作成する場合、提供され `userIDs` るジョブリクエストは、適用するデータストアに応じて、特定の `namespace` を使用する `type` 必要があります。 のIDは、値に「unregistered」を使用する [!DNL Data Lake] 必要があります。また、該当するデータセットに追加された `type` プライバシーラベル `namespace`[](#privacy-labels) と一致する値を使用する必要があります。

さらに、要求ペイロードの `include` 配列には、要求が行われる別のデータストアの製品値を含める必要があります。 にリクエストを行う場合 [!DNL Data Lake]は、配列に値を含める必要があり `aepDataLake`ます。

次のリクエストは、未登録の「email_label」名前空間を使用して、の新しいプライバシージョブ [!DNL Data Lake]を作成します。 また、 [!DNL Data Lake]`include` 配列内ののの製品値も含まれます。

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

## 削除要求処理

から削除リクエスト [!DNL Experience Platform] を受け取ると、リクエストが受信され、影響を受けるデータが削除のマーク [!DNL Privacy Service]が付けられたこと [!DNL Platform] を [!DNL Privacy Service] 確認するメッセージがに送信されます。 レコードは7日以内に削除さ [!DNL Data Lake] れます。 その7日間の期間は、データはソフトデリートされるので、どの [!DNL Platform] サービスからもアクセスできません。

今後のリリースでは、データ [!DNL Platform] が物理的に削除された [!DNL Privacy Service] 後に、に確認メッセージが送信されます。

## 次の手順

このドキュメントを読むことで、のプライバシー要求の処理に関する重要な概念について説明し [!DNL Data Lake]ます。 IDデータの管理方法とプライバシージョブの作成方法についての理解を深めるために、このガイドに記載されているドキュメントを読み続けることをお勧めします。

ストアのプライバシー要求を処理する手順については、リアルタイム顧客プロファイルの [プライバシー要求処理に関するドキュメント](../profile/privacy.md)[!DNL Profile] （英語）を参照してください。

## 付録

次の節では、のプライバシー要求を処理するための追加情報について説明し [!DNL Data Lake]ます。

### ネストされたマップタイプフィールドのラベル付け {#nested-maps}

プライバシーのラベル付けをサポートしない、ネストされたマップタイプのフィールドは2種類あることに注意してください。

* 配列型フィールド内のmap-typeフィールド
* 別のmap-typeフィールド内のmap-typeフィールド

上記の2つの例のいずれかに対するプライバシージョブの処理は、最終的に失敗します。 このため、プライベート顧客データを保存する際に、ネストされたマップタイプのフィールドを使用しないことをお勧めします。 関連するコンシューマIDは、レコードベースのデータセット用のフィールド（自体はマップタイプのフィールド）内の非マップデータ型、または時系列ベースのデータセット用の `identityMap``endUserID` フィールドとして保存する必要があります。