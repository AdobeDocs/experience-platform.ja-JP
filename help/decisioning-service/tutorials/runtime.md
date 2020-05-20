---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: APIを使用したDecisioningサービスランタイムの操作
topic: tutorial
translation-type: tm+mt
source-git-commit: 5699022d1f18773c81a0a36d4593393764cb771a
workflow-type: tm+mt
source-wordcount: '2022'
ht-degree: 0%

---


# APIを使用したDecisioningサービスランタイムの操作

このドキュメントでは、Adobe Experience Platform APIを使用してDecisioningサービスのランタイムサービスを操作するためのチュートリアルを提供します。

## はじめに

このチュートリアルでは、判定に関わるExperience Platformサービスについて十分に理解し、顧客体験で提示する次の最適なオファーを決定する必要があります。 このチュートリアルを開始する前に、次のドキュメントを確認してください。

- [Decisioningサービス](./../home.md): オファーの追加と削除のフレームワークを提供し、顧客のエクスペリエンスに最適なアルゴリズムを作成して、提示するためのアルゴリズムを作成します。
- [Experience Data Model(XDM)](../../xdm/home.md): プラットフォームが顧客体験データを編成する際に使用する標準化されたフレームワーク。
- [プロファイルクエリ言語(PQL)](../../segmentation/pql/overview.md): PQLは、ルールとフィルターを定義するために使用します。
- [APIを使用してDecisioningのオブジェクトとルールを管理](./entities.md): Decisioning Servicesランタイムを使用する前に、関連エンティティを設定する必要があります。

以下の節では、プラットフォームAPIを正しく呼び出すために知る必要がある追加情報について説明します。

### サンプルAPI呼び出しの読み取り

このチュートリアルでは、リクエストをフォーマットする方法を示すAPI呼び出しの例を提供します。 例えば、パス、必須のヘッダー、適切にフォーマットされた要求ペイロードなどです。 API応答で返されるサンプルJSONも提供されます。 サンプルAPI呼び出しのドキュメントで使用される表記について詳しくは、Experience PlatformトラブルシューティングガイドのAPI呼び出し例の読み [方に関する節を参照してください](../../landing/troubleshooting.md#how-do-i-format-an-api-request) 。

### 必要なヘッダーの値の収集

プラットフォームAPIを呼び出すには、まず [認証チュートリアルを完了する必要があります](../../tutorials/authentication.md)。 次に示すように、認証チュートリアルで、すべてのExperience Platform API呼び出しに必要な各ヘッダーの値を指定します。

- 認証： 無記名 `{ACCESS_TOKEN}`
- x-api-key: `{API_KEY}`
- x-gw-ims-org-id: `{IMS_ORG}`

エクスペリエンスプラットフォームのすべてのリソースは、特定の仮想サンドボックスに分離されています。 プラットフォームAPIへのすべてのリクエストには、操作が実行されるサンドボックスの名前を指定するヘッダーが必要です。

- x-sandbox-name: `{SANDBOX_NAME}`

>[!NOTE] プラットフォームのサンドボックスについて詳しくは、「 [サンドボックスの概要に関するドキュメント](../../tutorials/authentication.md)」を参照してください。

ペイロード(POST、PUT、PATCH)を含むすべてのリクエストには、次の追加のヘッダーが必要です。

- Content-Type: application/json

ランタイム要求にも必要：

- x-request-id: `{UUID}`

>[!NOTE] `UUID` は、グローバルに一意の文字列で、別のAPI呼び出しで再利用できないUUID形式の文字列です

判定サービスは、相互に関連する多数のビジネスオブジェクトによって制御されます。 すべてのビジネス・オブジェクトは、プラットフォームのビジネス・オブジェクト・リポジトリXDMコア・オブジェクト・リポジトリに格納されます。 このリポジトリの主な特徴は、APIがビジネスオブジェクトのタイプと直交していることです。 APIエンドポイント内のリソースの種類を示すPOST、GET、PUT、PATCH、またはDELETE APIを使用する代わりに、6つの汎用エンドポイントしかありませんが、その曖昧性を解除する必要がある場合に、オブジェクトの種類を示すパラメータを受け取るか返します。 スキーマはリポジトリに登録する必要がありますが、その後、リポジトリはオープンエンドなオブジェクト型のセットに対して使用できます。

とのすべてのXDMコアオブジェクトリポジトリAPI開始のエンドポイントパスで `https://platform.adobe.io/data/core/ode/`す。

エンドポイントに続く最初のパス要素が `containerId`です。 このIDは、XDMコアオブジェクトリポジトリのルートエンドポイントから取得 `GET https://platform.adobe.io/data/core/xcore/`されます。

## 決定モデルのコンパイル

ビジネスロジックエンティティのアクティベーションは、自動的に、連続的に行われます。 新しいオプションがリポジトリに保存され、「承認済み」とマークされるとすぐに、使用可能なオプションのセットを含める候補になります。 決定ルールが更新されるとすぐに、ルールセットが再アセンブルされ、実行時の実行に備えられます。 この自動アクティベーション手順では、ビジネスロジックによって定義された、実行時のコンテキストに依存しない制約が評価されます。 このアクティベーション手順の結果はキャッシュに送信され、キャッシュがDecisioningサービスランタイムで使用できるようになります。

### 配置、フィルター、ライフサイクル状態の影響

オファーは継続的に作成され、ライフサイクルステータスで変更が行われます。また、新しいコンテンツ表示が得られる場合もあります。 アクティビティのオファーフィルターは、タグセットが更新されたオファーを変更したり、一致させたり、除外したりすることができます。 このプロセスは非常に複雑な場合があり、アクティビティやサービスは結果として得られる候補セットを知る必要があります。 判定ランタイムは、承認されていないオファー、オファーフィルターと一致しないアクティビティ、またはアクティビティが参照する配置の表現がないオファーをフィルターに送信する間APIを提供します。

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

このパラメーター `activityId` はURL内で繰り返すことができ、1つのリクエストで最大30個の異なるアクティビティ参照を指定できます。 この応答は、セットアップに起因する予期しない状況を特定するのに役立ち、次のようになります。

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

オブジェクトが更新された時間と、API応答がアクティビティとオファー間のマッピングを反映する時間の間に、わずかな遅延が発生します。 各オブジェクトのリビジョンは、レスポンス内で与えられ、クライアントは、反映されていないオブジェクトに更新があるかどうかを確認できます。

### 診断APIとトラブルシューティング

アクティビティ、オファーおよび実施要件ルールは、Decision Serviceランタイムで使用される内部形式(ランタイムオファーカタログ)にコンパイルされます。 このコンパイルは、オブジェクトが保存され、XDMコアオブジェクトリポジトリにリンクが確立された場合に実行されるチェックで検出されなかったエラーを検出する場合があります。

診断APIは、その手順で発生したコンパイルエラーを取得するために提供されます。また、ルールとアクティビティが最後に再コンパイルされた日時に関する情報を取得するためのエラーがない場合に備えています。

**リクエスト**

```shell
curl -X GET {DECISION_SERVICE_ENDPOINT_PATH}/{CONTAINER_ID}/diagnostics \
  -H 'Accept: application/vnd.adobe.xdm+json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'x-request-id: {NEW_UUID}'
```

このAPI呼び出しの唯一のパラメーターは `containerId`です。 この結果、そのコンテナ内の決定ルール、オファー、アクティビティ、またはオファーフィルターを変更したすべてのクライアントからのすべての更新が行われます。 オブジェクトが更新された時間とコンパイルが終了する時間の間に、数秒の遅延が発生します。 前回の更新のタイムスタンプとエラーは、診断呼び出しへの応答で返されます。

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

## 決定を実行するためのREST API呼び出し

REST APIは、組織がユーザーに対して設定したルール、モデルおよび制約に基づいて次の最適なエクスペリエンスを取得するための、プラットフォーム上で実行されるアプリケーションのルートの1つです。 アプリケーションは、プロファイルのID(プロファイルIDとID名前空間)の1つを送信し、決定サービスがプロファイルを調べ、その情報を使用してビジネスロジックを適用します。 追加のコンテキストデータをリクエストに渡すことができます。ビジネスルールで指定した場合は、データに含まれ、決定を行います。

最大30人のアクティビティに対して同時に決定を求めることで、アプリケーションのパフォーマンスを向上できます。 アクティビティのURIは同じ要求で渡されます。 REST APIは同期的で、これらすべてのアクティビティに対して提案されたオプションまたは制約を満たすパーソナライゼーションオプションがない場合のフォールバックオプションを返します。

2つの異なるアクティビティが、「最良」の選択肢と同じものを考え出す可能性があります。 組み合わせたエクスペリエンスの繰り返しを避けるため、デフォルトでは、Decisioningサービスは同じリクエストで参照されているアクティビティ間を調停します。 調停とは、各アクティビティに対して、上位N個の選択肢が検討されるが、これらのアクティビティ間で複数回選択肢が提案されないことを意味する。 2人のアクティビティの一番上のランクが同じ場合は、そのうちの1つが2番目の選択肢または3番目の選択肢を使用するように選択されます。 これらの重複除外ルールは、アクティビティのいずれかがそのフォールバックオプションを使用する必要がないように試みます。

デシジョンリクエストには、POSTリクエストの本文である引数が含まれます。 本文の形式はJSON `Content-Type` ヘッダー値です `application/vnd.adobe.xdm+json; schema="{REQUEST_SCHEMA_AND_VERSION}"`

現在サポートされているリクエストのスキーマとバージョンは `https://ns.adobe.com/experience/offer-management/decision-request;version=0.9`です。 今後、追加のリクエストスキーマまたはバージョンが提供される予定です。

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

- **`xdm:dryRun`**  — このオプションのプロパティの値をtrueに設定すると、デシジョンリクエストは制限に従いますが、実際にはこれらのカウンターを絞り込まないので、発信者がプロファイルに提案を提示しないことを期待しています。 Decisioningサービスは、提案を正式なXDM決定イベントとして記録せず、レポートデータセットには表示しません。 このプロパティのデフォルト値はfalseで、プロパティが省略された場合、判定はテスト実行と見なされないので、エンドユーザーに表示されます。
- **`xdm:validateContextData`**  — このオプションのプロパティは、コンテキストデータの検証のオン/オフを切り替えます。 検証を有効にすると、指定された各コンテキストデータ項目に対して、スキーマ( `@type` フィールドに基づく)がXDMレジストリから取得され、 `xdm:data` オブジェクトがそれに対して検証されます。

このスキーマごとの要求には、オファーアクティビティ、プロファイルID、およびコンテキストデータ項目の配列を参照するURIの配列が含まれます。

- **`xdm:offerActivities`**  — この必須プロパティは、各項目にオファーアクティビティに関するデータが含まれるオブジェクトの配列です。 オファーアクティビティには次のプロパティがあります。
   - **`xdm:offerActivity`** -アクティビティの固有な識別子(URI)。 これは、オファーアクティビティの `@id` プロパティの値です。
- **`xdm:identityMap`** - XDMスキーマに準拠するJSONオブジェクトを保持する必須プロパティ `https://ns.adobe.com/xdm/context/identitymap`です。 プロパティは、キーがID名前空間コードであるマップを定義し、値は指定された名前空間内のエンドユーザー識別子のリストです。 もしm.
- **`xdm:contextData`** -スキーマへの参照によって記述される項目を含むオプションのプロパティ。 配列内の各コンテキストデータ項目には、次のプロパティが必要です。
   - **`@type`**  — この項目内のオブジェクトのXDMスキーマを参照する必須プロパティ。
   - **`xdm:data`**  — プロパティで指定されたXDMスキーマごとのオブジェクトプロパティを含む必須プロパティ `@type` 。

## Decisioningリクエストの動的コンテキストデータ

前の節では、XDMオブジェクトをデシジョンリクエストに渡す方法について説明しました。 次に、このようなコンテキストオブジェクト配列の例を示します。

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

スキーマは組織で構築されたものにする必要があります。 スキーマの作成方法については、 [スキーマエディタのチュートリアルを参照してください](../../xdm/tutorials/create-schema-ui.md)。 スキーマは名前空間に入り `https://ns.adobe.com/{TENANT_ID}/schemas`ます。

『 [スキーマレジストリAPI開発者ガイド](../../xdm/tutorials/create-schema-api.md) 』では、スキーマにプログラムを使用してアクセスする方法、および開発者がスキーマのテナントIDと数値識別子を取得する方法について説明しています。 バージョン番号は必須で、スキーマレジストリAPIからも提供されます。

組織で定義されたスキーマには、通常、テナント名前空間文字列とも呼ばれ `_{TENANT_ID}`る名前のルートプロパティが含まれます。
_などのグローバルスキーマコンポーネントで使用されるプロパティには、名前空間プレフィックスが付いていることに注意して`https://ns.adobe.com/xdm/context/product` く `xdm:`ださい。 この場合、組織定義のプロパティ `productDetails` はそのデータ型を使用して構築されました。 テナントプロパティはテナント名前空間にちなんで名前が付いたプロパティにネストされますが、グローバルに使用可能なデータ型は予約済みのプレフィックスを使用して、プロパティ名の競合を防ぎます。 `xdm:`

このプロパティには、複数のデータオブジェクトを表示でき `xdm:contextData` ます。 各オブジェクトは、 `@type` プロパティを使用して型を識別する必要があります。
コンテキストデータオブジェクトの値は、PQL式で使用できます。例えば、実施要件ルールの条件で使用できます。 コンテキストデータオブジェクトは、特別なパス参照式を通じて指定する必要があ `@{schemaId}`ります。 次の参照式に従う式は、データオブジェクトのプロパティにアクセスする正規パス式ーです。

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

上の例では、変数 `p` は、 `@type` =でマークされたオブジェクトの配列に対して反復 `https://ns.adobe.com/{TENANT_ID}/schemas/{NUMERIC_SCHEMA_ID}}`します。

PQL構文では、プロパティ名に接頭辞が使用されません。 デフォルトでは、グローバルプロパティは、プリフィックスなしで単に参照され `xdm:` ます。 組織が定義するプロパティは、テナント名前空間にちなんで名前が付けられた **追加の** プロパティ(変数で示される例 `{TENANT_ID}`)内にネストされます。 カスタム定義のプロパティを直接参照できるように、変数 `p` は、追加のネストプロパティを非参照するパスの結果に連結されます。

## プロファイルレコードの使用

プロファイルエンティティおよびエクスペリエンスイベントエンティティのすべてのレコードは、既にプロファイルストアで管理されています。 1つ以上のプロファイルIDを要求に渡すと、それらのIDのプロファイルが識別され、ストアから検索されます。 その後、データは、決定方法によって評価された決定ルールおよびモデルで自動的に使用できます。

プロファイルおよびエクスペリエンスの記録を取得するには、デフォルトの結合ポリシーが適用されます。
プロファイルレコードをプラットフォームデータレイクにアップロードした後、プロファイルレコードを検索できるまで、若干の遅延が生じることに注意してください。 ストリーミングAPIを使用してプロファイルおよびエクスペリエンスの記録を取り込む場合も同様です。数秒後に限り、プロファイルおよびエクスペリエンスのイベントデータを評価する決定ルールの評価にデータを使用できます。