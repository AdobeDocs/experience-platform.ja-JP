---
keywords: Experience Platform;home;popular topics;decision events;decision event;Decision events
solution: Experience Platform
title: API を使用した判定サービスランタイムの操作
topic: tutorial
type: Tutorial
description: 'このドキュメントでは、Adobe Experience Platform API を使用した判定サービスのランタイムサービスの操作に関するチュートリアルを提供します。 '
translation-type: tm+mt
source-git-commit: 97dfd3a9a66fe2ae82cec8954066bdf3b6346830
workflow-type: tm+mt
source-wordcount: '2017'
ht-degree: 82%

---


# API を使用した判定サービスランタイムの操作

This document provides a tutorial for working with the runtime services of [!DNL Decisioning Service] using Adobe Experience Platform APIs.

## はじめに

This tutorial requires a working understanding of the [!DNL Experience Platform] services involved in decisioning and determining the next best offer to present during customer experiences. このチュートリアルを開始する前に、次のドキュメントを確認してください。

- [[!DNL Decisioning Service]](./../home.md):オファーの追加と削除のフレームワークを提供し、顧客のエクスペリエンスに最適なアルゴリズムを作成して、提示するためのアルゴリズムを作成します。
- [[!DNL Experience Data Model (XDM)]](../../xdm/home.md):プラットフォームが顧客体験データを編成する際に使用する標準化されたフレームワーク。
- [[!DNLプロファイルクエリ言語(PQL)]](../../segmentation/pql/overview.md):PQLは、ルールとフィルターを定義するために使用します。
- [API を使用して判定オブジェクトおよびルールを管理](./entities.md)：判定サービスランタイムを使用する前に、関連エンティティを設定する必要があります。

The following sections provide additional information that you will need to know in order to successfully make calls to the [!DNL Platform] APIs.

### API 呼び出し例の読み取り

このチュートリアルでは、API 呼び出しの例を提供し、リクエストの形式を設定する方法を示します。この中には、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、 トラブルシューテングガイドの[API 呼び出し例の読み方](../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください。[!DNL Experience Platform]

### 必須ヘッダーの値の収集

In order to make calls to [!DNL Platform] APIs, you must first complete the [authentication tutorial](../../tutorials/authentication.md). Completing the authentication tutorial provides the values for each of the required headers in all [!DNL Experience Platform] API calls, as shown below:

- Authorization: Bearer `{ACCESS_TOKEN}`
- x-api-key: `{API_KEY}`
- x-gw-ims-org-id: `{IMS_ORG}`

All resources in [!DNL Experience Platform] are isolated to specific virtual sandboxes. All requests to [!DNL Platform] APIs require a header that specifies the name of the sandbox the operation will take place in:

- x-sandbox-name: `{SANDBOX_NAME}`

>[!NOTE]
>
>For more information on sandboxes in [!DNL Platform], see the [sandbox overview documentation](../../tutorials/authentication.md).

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、以下のような追加ヘッダーが必要です。

- Content-Type: application/json

ランタイムリクエストにも必要：

- x-request-id: `{UUID}`

>[!NOTE]
>
>`UUID` は、グローバルに一意の UUID 形式の文字列で、別の API 呼び出しで再利用できません

[!DNL Decisioning Service] は、互いに関連する多数のビジネスオブジェクトによって制御されます。 All business objects are stored in [!DNL Platform’s] business object repository, XDM Core Object Repository. このリポジトリーの主な特長は、API がビジネスオブジェクトのタイプと直交していることです。API エンドポイントのリソースのタイプを示す POST、GET、PUT、PATCH、DELETE API を使用する代わりに、6 つの汎用エンドポイントしかありませんが、あいまいさ解消が必要なときにオブジェクトのタイプを示すパラメーターを受け入れるまたは返します。スキーマはリポジトリーに登録する必要がありますが、その後、リポジトリをオープンエンドなオブジェクト型のセットに対して使用できます。

すべての XDM コアオブジェクトリポジトリー API のエンドポイントパスは `https://platform.adobe.io/data/core/ode/` で開始します。

エンドポイントに続く最初のパス要素は、`containerId` です。この識別子は、XDM コアオブジェクトリポジトリーのルートエンドポイント（`GET https://platform.adobe.io/data/core/xcore/`）を通じて取得されます。

## 意思決定モデルのコンパイル

ビジネスロジックエンティティのアクティベーションは、自動的かつ連続的に発生します。新しいオプションがリポジトリーに保存され、「承認済み」とマークされるとすぐに、そのオプションは使用可能なオプションのセットを含めるための候補となります。判定ルールが更新されるとすぐに、ルールセットが再度アセンブルされ、ランタイムの実行に備えます。この自動アクティベーション手順では、ビジネスロジックによって定義され、実行時のコンテキストに依存しない制約が評価されます。The results of this activation step are sent to a cache where they are available to the [!DNL Decisioning Service] runtime.

### 配置、フィルター、ライフサイクル状態の影響

オファーは継続的に作成され、ライフサイクルのステータスで変更がおこなわれたり、新しいコンテンツ表示に変わったりする可能性があります。アクティビティのオファーフィルターは、タグセットが更新されたオファーを変更、照合、または除外することができます。このプロセスは非常に複雑になる可能性があり、アプリケーションやサービスは、結果として生成されるアクティビティの候補セットがどのようなものになるか知っておく必要があります。判定ランタイムは、承認されていないオファー、オファーフィルターと一致しないオファー、またはアクティビティが参照する配置の表現を持たないオファーを除外する、アクティビティとオファーの間の API を提供します。

**リクエスト**

```shell
curl -X GET {DECISION_SERVICE_ENDPOINT_PATH}/{CONTAINER_ID}/offers?activityId={ACTIVITY_URI}' \
  -H 'Accept: application/vnd.adobe.xdm+json \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'x-request-id: {NEW_UUID}'
```

**応答** 

このパラメーター `activityId` は URL 内で繰り返すことができ、1 回のリクエストで最大 30 個の異なるアクティビティ参照を指定できます。この応答は、設定に起因する予期しない状況を見つけるのに役立ち、次のようになります。

```json
{
  "xdm:activityOffers": [
    {
      "xdm:offerActivity": {
        "@id": "{ACTIVITY_URI}",
        "_links": {
          "self": {
            "href": "{repository_endpoint}/{CONTAINER_ID}/instances/{ACTIVITY_INSTANCE_ID}"
          }
        },
        "repo:etag": "1"
      },
      "xdm:offers": [
        {
          "@id": "{OFFER_URI_1}",
          "_links": {
            "self": {
              "href": "{REPOSITORY_ENDPOINT}/{CONTAINER_ID}/instances/{OFFER_INSTANCE_ID_1}"
            }
          },
          "repo:etag": "15"
        },
        {
          "@id": "{OFFER_URI_2}",
          "_links": {
            "self": {
              "href": "{REPOSITORY_ENDPOINT}/{CONTAINER_ID}/instances/{OFFER_INSTANCE_ID_2}"
            }
          },
          "repo:etag": "5"
        }
      ],
      "xdm:fallbackOffer": {
        "@id": "{FALLBACK_URI}",
        "_links": {
          "self": {
            "href": "{REPOSITORY_ENDPOINT}/{CONTAINER_ID}/instances/{FALLBACK_INSTANCE_ID}"
          }
        },
        "repo:etag": "2"
      }
    }
  ]
}
```

オブジェクトが更新されてから API 応答がアクティビティとオファーの間のマッピングを反映させるまでの時間には、数秒の遅延があります。各オブジェクトのリビジョンは、反映されていないオブジェクトの更新があるかどうかをクライアントが確認できるように、応答内で提供されます。

### 診断 API とトラブルシューティング

アクティビティ、オファーおよび実施要件ルールは、判定サービスランタイムで使用される内部形式（ランタイムオファーカタログ）にコンパイルされます。コンパイル時に、オブジェクトの格納時に実行されたチェックで検出されなかったエラーや、XDM コアオブジェクトリポジトリーでリンクが確立された場合、エラーが検出される可能性があります。

診断 API は、その手順で発生したコンパイルエラーを取得するために提供され、エラーがない場合は、ルールとアクティビティが最後に再コンパイルされた日時に関する情報を取得するために提供されます。

**リクエスト**

```shell
curl -X GET {DECISION_SERVICE_ENDPOINT_PATH}/{CONTAINER_ID}/diagnostics \
  -H 'Accept: application/vnd.adobe.xdm+json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'x-request-id: {NEW_UUID}'
```

この API 呼び出しの唯一のパラメーターは `containerId` です。この結果、そのコンテナ内の判定ルール、オファー、アクティビティまたはオファーフィルターを変更したすべてのクライアントからのすべての更新がおこなわれます。オブジェクトが更新されてからコンパイルが完了するまでの時間には、数秒の遅延があります。前回の更新のタイムスタンプとエラーは、診断呼び出しの応答で返されます。

**応答** 

```json
{
  "xdm:operations": {
    "xdm:offerCatalogUpdate": {
      "xdm:date": "2019-06-19T23:28:44.855Z",
      "xdm:errors": []
    },
    "xdm:activitiesUpdate": {
      "xdm:date": "2019-06-19T23:28:40.114Z",
      "xdm:errors": []
    }
  }
}
```

## 決定を実行する REST API 呼び出し

The REST API is one of the routes for applications running on top of [!DNL Platform] to obtain the next best experience based on the rules, models and constraints that the organization has set for their users. Applications send one of the profile’s identities (profile ID and identity namespace) the [!DNL Decisioning Service] will look up the profile and the information is used to apply the business logic. 追加のコンテキストデータをリクエストに渡すことができます。ビジネスルールで指定した場合は、データに含めて判定します。

最大 30 個のアプリケーションに対する判定一度にリクエストすることで、アプリケーションのパフォーマンスを向上させることができます。同じリクエストでアクティビティの URI が渡されます。REST API は同期的であり、これらすべてのアクティビティに対して提案されたオプション、またはパーソナライゼーションオプションが制約を満たさない場合はフォールバックオプションを返します。

2 つの異なるアクティビティのオプションが、「最良」と同じとなる可能性があります。To avoid repeating a composed experience, by default, [!DNL Decisioning Service] arbitrates between the activities that are referenced in the same request. 判別とは、各アクティビティに対して、上位 N 個のオプションを考慮し、これらのアクティビティをオプションが複数回提案されないことを意味します。2 つのアクティビティの最上位オプションが同じ場合、そのうちいずれかが 2 番目または 3 番目のオプションなどを使用するように選択されます。これらの重複除外ルールは、任意のアクティビティでそのフォールバックオプションを使用する必要が出るのを防ごうとします。

意思決定リクエストには、POST リクエストの本文の引数が含まれます。本文は JSON `Content-Type`ヘッダー値`application/vnd.adobe.xdm+json; schema="{REQUEST_SCHEMA_AND_VERSION}"`としてフォーマットされます。

要求スキーマと現時点でサポートされているバージョンは `https://ns.adobe.com/experience/offer-management/decision-request;version=0.9` です。今後、追加のリクエストスキーマまたはバージョンが提供されます。

**リクエスト**

```shell
curl -X POST {DECISION_SERVICE_ENDPOINT_PATH}/{CONTAINER_ID}/decisions \
  -H 'Accept: application/json, application/problem+json \
  -H '
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'x-request-id: {NEW_UUID}' \
  -d '{
  "xdm:dryRun": {DRY_RUN_TRUE_FALSE},
  "xdm:validateContextData": {VALIDATE_CONTEXT_DATA_TRUE_FALSE},

  "xdm:offerActivities":[
    {
      "xdm:offerActivity": "{ACTIVITY_URI_1}"
    },
  ],
  "xdm:identityMap":{
    "{PROFILE_ID_NAMESPACE_CODE}":[
      {
        "xdm:id":"{PROFILE_ID}"
      }
    ]
  },
  "xdm:profileModel":"{PROFILE_MODEL}",
  "xdm:contextData": [
    {
      "@type": "{CONTEXT_DATASSCHEMA_ID}"
      "xdm:data": { JSON PROPERTIES OF CONTEXT ENTITY }
    }
  ] ,
  "xdm:allowDuplicatePropositions": {
    "xdm:acrossActivities": {DUPLICATE_OFFER_IDS_OF_TRUE_FALSE},
  }
}’
```

- **`xdm:dryRun`** - このオプションのプロパティの値が true に設定されている場合、判定リクエストは制約に従いますが、実際にはこれらのカウンタを絞り込みません。呼び出し元はプロファイルに対する提案を提示しないでください。The [!DNL Decisioning Service] will not record the proposition as an official XDM decision event and it will not appear in reporting datasets. このプロパティのデフォルト値は false です。プロパティが省略された場合、判定はテスト実行と見なされないので、エンドユーザーに表示されることになります。
- **`xdm:validateContextData`** - このオプションのプロパティは、コンテキストデータの検証のオンとオフを切り替えます。検証がオンになっている場合、指定された各コンテキストデータ項目に対して、スキーマ（`@type` フィールドに基づく）が XDM レジストリから取得され、それに対して `xdm:data` オブジェクトが検証されます。

このスキーマごとの要求には、オファーアクティビティ、プロファイル ID およびコンテキストデータ項目の配列を参照する URI の配列が含まれます。

- **`xdm:offerActivities`** - この必須プロパティは、各項目にオファーアクティビティに関するデータが含まれるオブジェクトの配列です。オファーアクティビティには次のプロパティがあります。
   - **`xdm:offerActivity`** - アクティビティの固有な識別子（URI）。これは、オファーアクティビティの `@id` プロパティの値です。
- **`xdm:identityMap`** - XDM スキーマに準拠する JSON オブジェクトを保持する必須プロパティ `https://ns.adobe.com/xdm/context/identitymap`。このプロパティは、キーが ID 名前空間コードで、値が特定の名前空間内にあるエンドユーザー識別子のリストであるマップを定義します。If m.
- **`xdm:contextData`** - スキーマへの参照として記述される項目を含む、オプションのプロパティ。配列内の各コンテキストデータ項目には、次のプロパティが必要です。
   - **`@type`** - この項目のオブジェクトの XDM スキーマを参照する必須プロパティ。
   - **`xdm:data`** - `@type` プロパティで指定された XDM スキーマに従うオブジェクトプロパティを含む必須プロパティ。

## 判定リクエストの動的コンテキストデータ

前の節では、XDM オブジェクトが判定リクエストに渡される仕組みについて説明しました。以下に示したのは、そのようなコンテキストオブジェクト配列の例です。

```json
"xdm:contextData": [
  {
    "@type":" https://ns.adobe.com/{TENANT_ID_OF_ORG}/schemas/{NUMERIC_SCHEMA_ID};version=1",
    "xdm:data":{ 
      "{TENANT_ID_OF_ORG}": {
        "productDetails":{ 
          "xdm:gender":      "unisex",
          "xdm:fabrication": "leather",
          "xdm:category":    "wallets",
        }
      }
    }
  }
]
```

スキーマは組織によって構築される必要があります。スキーマの作成方法については、[スキーマエディターのチュートリアル](../../xdm/tutorials/create-schema-ui.md)を参照してください。あなたのスキーマは名前空間 `https://ns.adobe.com/{TENANT_ID}/schemas` 内に格納されます。

『[スキーマレジストリ API 開発者ガイド](../../xdm/tutorials/create-schema-api.md)』では、プログラムを使用してプログラムにアクセスする方法と、開発者がテナント ID とスキーマの数値識別子を取得する方法について説明しています。バージョン番号は必須で、スキーマレジストリ API からも提供されます。

組織で定義されたスキーマには、通常、`_{TENANT_ID}` という名前のルートプロパティ（テナント名前空間文字列とも呼ばれる）があります。
`https://ns.adobe.com/xdm/context/product` などのグローバルスキーマコンポーネントで使用されるプロパティには、名前空間プレフィックス `xdm:` が付きます。この場合、組織定義の `productDetails` プロパティはそのデータ型を使用して構築されます。テナントプロパティはテナント名前空間にちなんで名前が付けられたプロパティにネストされますが、グローバルに使用可能なデータ型には予約済みのプレフィックス `xdm:` を使用して、プロパティ名の競合を防ぎます。

`xdm:contextData` プロパティには複数のデータオブジェクトをリストできます。各オブジェクトは、`@type` プロパティを介して型を識別する必要があります。
コンテキストデータオブジェクトの値は、PQL 式（例えば、実施要件ルールの条件）で使用できます。コンテキストデータオブジェクトは、特別なパス参照式 `@{schemaId}` を通じて指定する必要があります。この参照式に続く式は、データオブジェクトのプロパティにアクセスする正規パス式です。

```json
{
  "xdm:name": "Eligible for a free gender-specific item of interest",
  "xdm:condition": {
    "xdm:value":
      "gender in (\"female\",\"non-specific\")  
       and (
         select p from
           @{https://ns.adobe.com/{TENANT_ID}/schemas/{NUMERIC_SCHEMA_ID}}._{TENANT_ID}
         select e from xEvent 
            where e.type = \"productSearch\" 
              and e.category = p.category 
              and p.gender in (\"female\",\"unisex\")
       )",
    "xdm:format": "pql/text",
    "xdm:type": "PQL"
  }
}
```

上の例では、変数 `p` は、`@type`=`https://ns.adobe.com/{TENANT_ID}/schemas/{NUMERIC_SCHEMA_ID}}` でマークされたオブジェクトの配列に対して繰り返されます。

PQL 構文では、プロパティ名に接頭辞は使用されません。デフォルトでは、グローバルプロパティは `xdm:` プレフィックスなしで参照されます。組織で定義したプロパティは、テナント 名前空間（例では変数 `{TENANT_ID}` で示される）の名前にちなんだ&#x200B;**追加**&#x200B;プロパティ内にネストされます。カスタム定義プロパティを直接参照できるように、変数 `p` は、追加のネストプロパティを逆参照するパスの結果に結び付けられます。

## プロファイルレコードの使用

プロファイルおよびエクスペリエンスイベントエンティティのすべてのレコードは、既にプロファイルストアで管理されています。1 つ以上のプロファイル ID を要求に渡すと、その ID のプロファイルが識別され、ストアから検索されます。その後、データは自動的に、判定戦略によって評価された判定ルールおよびモデルで使用できるようになります。

デフォルトの結合ポリシーが適用され、プロファイルおよびエクスペリエンスの記録を取得します。Note, that after uploading profile records to the [!DNL Platform] datalake there is a slight delay until the profile records can be looked up. ストリーミング API 経由でプロファイルおよびエクスペリエンスレコードを取り込む場合も同様ですが、数秒後にのみ、プロファイルおよびエクスペリエンスイベントデータを評価する判定ルールの評価用にデータを使用できるようになります。