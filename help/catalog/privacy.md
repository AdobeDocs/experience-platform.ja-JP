---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: データレークでのプライバシー要求の処理
topic: overview
translation-type: tm+mt
source-git-commit: d3584202554baf46aad174d671084751e6557bbc

---


# データレークでのプライバシー要求の処理

Adobe Experience Platform Privacy Serviceは、法的および組織のプライバシー規制に基づく説明に従って、顧客の要求を処理し、個人データにアクセス、販売、または削除します。

このドキュメントでは、データレークに保存された顧客データに対するプライバシー要求の処理に関する基本的な概念について説明します。

## はじめに

このガイドを読む前に、次のExperience Platformサービスに関する十分な理解を得ておくことをお勧めします。

* [プライバシーサービス](../privacy-service/home.md): Adobe Experience Cloudアプリケーションで個人データにアクセス、オプトアウト、削除を行う場合の顧客の要求を管理します。
* [カタログサービス](home.md): エクスペリエンスプラットフォーム内のデータの場所と系列の記録システムです。 データセットメタデータの更新に使用できるAPIを提供します。
* [Experience Data Model(XDM)System](../xdm/home.md): エクスペリエンスプラットフォームが顧客エクスペリエンスデータを編成する際に使用する標準化されたフレームワークです。
* [IDサービス](../identity-service/home.md): デバイスやシステム間でIDをブリッジ化することによって顧客体験データを断片化することによって生じる基本的な課題を解決します。

## ID名前空間について {#namespaces}

Adobe Experience Platform Identity Serviceは、システムとデバイス間で顧客のIDデータを結合します。 アイデンティティサービスは **ID名前空間** を使用して、ID値を接触チャネルのシステムに関連付けることで、ID値のコンテキストを提供します。 名前空間は、電子メールアドレス（「電子メール」）などの汎用的な概念を表したり、IDを特定のアプリケーション(Adobe Advertising Cloud ID(「AdCloud」)やAdobeターゲットID(「TNTID」)など)に関連付けたりできます。

アイデンティティサービスは、グローバルに定義された（標準）ID名前空間とユーザー定義の（カスタム）IDユーザーのストアを管理します。 標準名前空間は、すべての組織（「電子メール」や「ECID」など）で使用できますが、組織は、特定のニーズに合わせてカスタム名前空間を作成することもできます。

エクスペリエンスプラットフォームのID名前空間について詳しくは、 [ID名前空間の概要を参照してください](../identity-service/namespaces.md)。

## IDデータをデータセットに追加する

Data Lakeのプライバシーリクエストを作成する場合、データを見つけて処理するために、有効なID値(および関連する名前空間)を各顧客に提供する必要があります。 したがって、プライバシー要求の対象となるすべてのデータセットには、関連するXDMスキーマに **ID記述子** が含まれている必要があります。

>[!NOTE] 現在、ID記述子メタデータ（アドホックデータセットなど）をサポートしないスキーマに基づくデータセットは、プライバシー要求で処理できません。

既存のデータセットのXDMスキーマにID記述子を追加する手順を説明します。 ID記述子を持つデータセットが既に存在する場合は、 [次の節に進みます](#nested-maps)。

>[!IMPORTANT] IDとして設定するスキーマフィールドを決定する場合は、入れ子のマップタイプフィールドを使用する [制限事項に注意してください](#nested-maps)。

データセットスキーマにID記述子を追加するには、次の2つの方法があります。

* [UIの使用](#identity-ui)
* [APIの使用](#identity-api)

### UIの使用 {#identity-ui}

Experience Platformユーザーインターフェイスでは、Workspaceで既存のXDMスキーマを編集でき _[!UICONTROL Schemas]_ます。 スキーマにID記述子を追加するには、リストからスキーマを選択し、スキーマエディタのチュートリアルで、スキーマフィールドをIDフィールドとして[設定する手順](../xdm/tutorials/create-schema-ui.md#identity-field)に従います。

スキーマ内の適切なフィールドをIDフィールドとして設定したら、次のセクションに進んで、プライバシー [要求の送信に関するセクション](#submit)。

### Using the API {#identity-api}

>[!NOTE] この節では、データセットのXDMスキーマの固有のURI ID値を把握していることを前提としています。 この値がわからない場合は、Catalog Service APIを使用して取得できます。 開発ガイドの「 [はじめに](./api/getting-started.md) 」の節を読んだ後、に示す [リストの](./api/list-objects.md) 手順 [、または](./api/look-up-object.md) Catalogオブジェクトを参照してデータセットを見つける手順に従います。 スキーマIDは、 `schemaRef.id`
>
> この節には、スキーマレジストリAPIの呼び出しが含まれます。 APIの使用に関する重要な情報(コンテナの概念や概念を把握する `{TENANT_ID}` など)については、開発者ガイドの [はじめに](../xdm/api/getting-started.md) （英語のみ）の節を参照してください。

スキーマレジストリAPIのエンドポイントにPOSTリクエストを行うことで、データセットのXDMスキーマにID記述子を追加でき `/descriptors` ます。

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
| `xdm:namespace` | プライバシーサービスで認識される [標準的なID名前空間](../privacy-service/api/appendix.md#standard-namespaces) 、または組織で定義されるカスタム名前空間の1つ。 |
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

>[!NOTE] この節では、データレークのプライバシー要求をフォーマットする方法について説明します。 プライバシーサービスのUI [(](../privacy-service/ui/overview.md) プライバシーサービスのAPI [)または](../privacy-service/api/getting-started.md) プライバシーサービスのAPI（プライバシーサービスのAPI）のドキュメントで、リクエストペイロードで送信したユーザーIDデータを適切に書式設定する方法など、プライバシージョブの送信方法に関する詳細な手順を確認することを強くお勧めします。

次の節では、プライバシーサービスのUIまたはAPIを使用して、Data Lakeのプライバシーを要求する方法について概要を説明します。

### UIの使用

UIでジョブリクエストを作成する場合、Data LakeまたはReal-time Customerプロファイルに保存されたデータのジョブを処理するには、「 **Products** (製品 **)」で「AEP Data Lake** (プロファイル)」または「 __ AEP Data Lake（ジョブ）」を必ず選択してください。

<img src="images/privacy/product-value.png" width="450"><br>

### APIの使用

APIでジョブリクエストを作成する場合、提供され `userIDs` るジョブリクエストは、適用するデータストアに応じて、特定の `namespace` を使用する `type` 必要があります。 Data LakeのIDは、値に「unregistered」を使用する必要があり `type` ます。また、該当するデータセットに追加された `namespace` プライバシーラベル [](#privacy-labels) と一致する値を使用する必要があります。

さらに、要求ペイロードの `include` 配列には、要求が行われる別のデータストアの製品値を含める必要があります。 データレークにリクエストを行う場合、配列には値「aepDataLake」を含める必要があります。

次のリクエストは、未登録の「email_label」名前空間を使用して、Data Lake用の新しいプライバシージョブを作成します。 また、次のように、アレイ内のData Lakeの製品値も含まれ `include` ます。

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

Experience Platformがプライバシーサービスから削除のリクエストを受け取ると、プラットフォームは、リクエストが受信され、影響を受けるデータが削除のマークが付けられたことをプライバシーサービスに確認するメッセージを送信します。 レコードは7日以内にデータレークから削除されます。 この7日間の期間は、データはソフト削除されるので、どのプラットフォームサービスからもアクセスできません。

今後のリリースでは、データが物理的に削除された後、プラットフォームはプライバシーサービスに確認メッセージを送信します。

## 次の手順

このドキュメントを読むと、Data Lakeのプライバシー要求の処理に関する重要な概念について説明しています。 IDデータの管理方法とプライバシージョブの作成方法についての理解を深めるために、このガイドに記載されているドキュメントを読み続けることをお勧めします。

プロファイルストアに対するプライバシー要求の処理手順については、リアルタイム [プロファイル向けプライバシー要求の処理に関するドキュメント](../profile/privacy.md) （英語）を参照してください。

## 付録

次の節では、データレークでのプライバシー要求の処理に関する追加情報について説明します。

### ネストされたマップタイプフィールドのラベル付け {#nested-maps}

プライバシーのラベル付けをサポートしない、ネストされたマップタイプのフィールドは2種類あることに注意してください。

* 配列型フィールド内のmap-typeフィールド
* 別のmap-typeフィールド内のmap-typeフィールド

上記の2つの例のいずれかに対するプライバシージョブの処理は、最終的に失敗します。 このため、プライベート顧客データを保存する際に、ネストされたマップタイプのフィールドを使用しないことをお勧めします。 関連するコンシューマIDは、レコードベースのデータセット用のフィールド（自体はマップタイプのフィールド）内の非マップデータ型、または時系列ベースのデータセット用の `identityMap``endUserID` フィールドとして保存する必要があります。