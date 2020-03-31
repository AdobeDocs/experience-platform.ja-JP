---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: APIを使用したDecisioningサービスランタイムの操作
topic: tutorial
translation-type: tm+mt
source-git-commit: 5699022d1f18773c81a0a36d4593393764cb771a

---


# APIを使用したDecisioningサービスランタイムの操作

このドキュメントでは、Adobe Experience Platform APIを使用してDecisioningサービスのランタイムサービスを操作するためのチュートリアルを提供します。

## はじめに

このチュートリアルでは、Experience Platformのサービスを十分に理解し、顧客体験の中で提示する次の最適なオファーを決定する必要があります。 このチュートリアルを開始する前に、次のドキュメントを確認してください。

- [Decisioningサービス](./../home.md):顧客のエクスペリエンスに最適なオファーを選択するためのアルゴリズムの追加と削除のフレームワークを提供します。
- [エクスペリエンスデータモデル(XDM)](../../xdm/home.md):プラットフォームが顧客エクスペリエンスデータを整理する際に使用する標準化されたフレームワーク。
- [プロファイルクエリ言語(PQL)](../../segmentation/pql/overview.md):PQLは、ルールとフィルターの定義
- [APIを使用してDecisioningのオブジェクトとルールを管理します](./entities.md)。Decisioning Servicesランタイムを使用する前に、関連エンティティを設定する必要があります。

以下の節では、プラットフォームAPIを正しく呼び出すために知っておく必要がある追加情報を示します。

### サンプルAPI呼び出しの読み取り

このチュートリアルでは、リクエストをフォーマットする方法を示すAPI呼び出しの例を示します。 これには、パス、必須ヘッダー、適切にフォーマットされたリクエストペイロードが含まれます。 API応答で返されるサンプルJSONも提供されます。 サンプルAPI呼び出しのドキュメントで使用される表記について詳しくは、エクスペリエンスプラットフォームのトラブルシューテ [ィングガイドのAPI呼び出し例の読み方に関する節](../../landing/troubleshooting.md#how-do-i-format-an-api-request) （英語のみ）を参照してください。

### 必要なヘッダーの値の収集

プラットフォームAPIを呼び出すには、まず認証チュートリアルを完了する必要 [があります](../../tutorials/authentication.md)。 次に示すように、認証チュートリアルで、すべてのエクスペリエンスプラットフォームAPI呼び出しで必要な各ヘッダーの値を入力します。

- 認証：無記名 `{ACCESS_TOKEN}`
- x-api-key: `{API_KEY}`
- x-gw-ims-org-id: `{IMS_ORG}`

エクスペリエンスプラットフォームのすべてのリソースは、特定の仮想サンドボックスに分離されます。 プラットフォームAPIへのすべてのリクエストには、操作が行われるサンドボックスの名前を指定するヘッダーが必要です。

- x-sandbox-name: `{SANDBOX_NAME}`

>[!NOTE] プラットフォームのサンドボックスについて詳しくは、サンドボックスの概要ドキュメ [ントを参照してくださ](../../tutorials/authentication.md)い。

ペイロード(POST、PUT、PATCH)を含むすべてのリクエストには、次のヘッダーが追加で必要です。

- コンテンツタイプ：application/json

ランタイム要求にも必要：

- x-request-id: `{UUID}`

>[!NOTE] は、グ `UUID` ローバルに一意のUUID形式の文字列で、異なるAPI呼び出しで再利用できない

Decisioningサービスは、相互に関連する多数のビジネスオブジェクトによって制御されます。 すべてのビジネスオブジェクトは、プラットフォームのビジネスオブジェクトリポジトリXDMコアオブジェクトリポジトリに保存されます。 このリポジトリの主な特徴は、APIがビジネスオブジェクトのタイプと直交していることです。 APIエンドポイント内のリソースのタイプを示すPOST、GET、PUT、PATCH、またはDELETE APIを使用する代わりに、6つの汎用エンドポイントしか存在しませんが、その曖昧性を解除する必要がある場合に、オブジェクトのタイプを示すパラメータを受け取るか返します。 スキーマはリポジトリに登録する必要がありますが、その後、リポジトリはオープンエンドなオブジェクト型のセットに対して使用できます。

すべてのXDMコアオブジェクトリポジトリAPIのエンドポイントパ `https://platform.adobe.io/data/core/ode/`ス。

エンドポイントに続く最初のパス要素は、です `containerId`。 この識別子は、XDM Core Object Repositoryのルートエンドポイントを通じて取得されま `GET https://platform.adobe.io/data/core/xcore/`す。

## 決定モデルのコンパイル

ビジネスロジックアクティベーションのエンティティは、自動的に、かつ連続的に発生します。 新しいオプションがリポジトリに保存され、「承認済み」とマークされるとすぐに、使用可能なオプションのセットを含める候補となります。 決定ルールが更新されるとすぐに、ルールセットが再アセンブルされ、実行時の実行に備えられます。 この自動アクティベーション手順では、ビジネスロジックによって定義され、実行時のコンテキストに依存しない制約が評価されます。 このアクティベーション手順の結果は、Decisioningサービスランタイムで使用できるキャッシュに送信されます。

### 配置、フィルター、ライフサイクル状態の影響

オファーは継続的に作成され、ライフサイクルステータスで変更が行われるか、新しいコンテンツ表示が取得される場合があります。 アクティビティのオファーフィルターは、タグセットが更新されたオファーを変更したり、一致させたり、除外したりすることができます。 このプロセスは非常に複雑になる可能性があり、アプリケーションやサービスは、結果として生成されるアクティビティの候補セットを知る必要があります。 Decisioningランタイムは、承認されていないオファー、オファーフィルターと一致しないオファー、またはアクティビティが参照する配置の表現を持たないをフィルターするアクティビティ間APIを提供します。

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

このパラメーター `activityId` はURL内で繰り返すことができ、1回のリクエストで最大30個の異なるアクティビティ参照を指定できます。 この応答は、設定に起因する予期しない状況を見つけるのに役立ち、次のようになります。

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

オブジェクトが更新されてから、API応答がアクティビティとオファーのマッピングを反映するまでの間に、わずかな遅延が発生します。 各オブジェクトのリビジョンは、クライアントが反映されていないオブジェクトの更新があるかどうかを確認できるように、応答内で与えられます。

### 診断APIとトラブルシューティング

アクティビティ、オファーおよび実施要件ルールは、Decision Service Runtimeで使用される内部形式(ランタイムオファーカタログ)にコンパイルされます。 コンパイル時に、オブジェクトの格納時に実行されたチェックで検出されなかったエラーや、XDMコアオブジェクトリポジトリでリンクが確立された場合に、エラーが検出される可能性があります。

診断APIは、その手順で発生したコンパイルエラーを取得するために提供され、ルールとアクティビティが最後に再コンパイルされた日時に関する情報を取得するためのエラーがない場合に使用されます。

**リクエスト**

```shell
curl -X GET {DECISION_SERVICE_ENDPOINT_PATH}/{CONTAINER_ID}/diagnostics \
  -H 'Accept: application/vnd.adobe.xdm+json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'x-request-id: {NEW_UUID}'
```

このAPI呼び出しの唯一のパラメーターはで `containerId`す。 この結果、そのコンテナ内の決定ルール、オファー、アクティビティまたはオファーフィルターを変更したすべてのクライアントからのすべての更新が行われます。 オブジェクトが更新されてからコンパイルが完了するまでの時間は、数秒の遅延です。 前回の更新のタイムスタンプとエラーは、診断呼び出しの応答で返されます。

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

## 決定を実行するREST API呼び出し

REST APIは、組織がユーザーに対して設定したルール、モデル、制約に基づいて次の最良のエクスペリエンスを取得するために、プラットフォームの上で実行されるアプリケーションのルートの1つです。 プロファイルは、プロファイルID(プロファイルIDとID名前空間)の1つをアプリケーションから送信し、判定サービスは、その情報を使用してビジネスロジックを適用します。 追加のコンテキストデータをリクエストに渡すことができます。ビジネスルールで指定した場合、データに含めて決定します。

最大30個のアプリケーションを同時に決定することで、アプリケーションのアクティビティを向上できます。 同じ要求でアクティビティのURIが渡されます。 REST APIは同期的で、これらすべてのアクティビティに対して提案されたオプション、またはパーソナライゼーションオプションが制約を満たさない場合はフォールバックオプションを返します。

2つの異なるアクティビティが、「最良」と同じオプションを持つ可能性があります。 組み立てられたエクスペリエンスの繰り返しを避けるため、デフォルトでは、Decisioningサービスは同じリクエストで参照されるアクティビティ間を調停します。 調停とは、各アクティビティに対して、上位N個のオプションが考慮されるが、これらのアクティビティ間で複数回のオプションが提案されないことを意味します。 2つのアクティビティの最上位オプションが同じ場合、その1つが2番目の選択肢または3番目の選択肢（以下同じ）を使用するように選択されます。 これらの重複除外ルールは、どのアクティビティもそのフォールバックオプションを使用する必要がないことを回避します。

デシジョンリクエストには、POSTリクエストの本文の引数が含まれます。 本文はJSONヘッダー値として形式設定さ `Content-Type` れます。 `application/vnd.adobe.xdm+json; schema="{REQUEST_SCHEMA_AND_VERSION}"`

要求スキーマと現時点でサポートされているバージョンはで `https://ns.adobe.com/experience/offer-management/decision-request;version=0.9`す。 今後、追加のリクエストスキーマまたはバージョンが提供されます。

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

- **`xdm:dryRun`**  — このオプションのプロパティの値がtrueに設定されている場合、デシジョンリクエストは制限に従いますが、実際にはこれらのカウンタを絞り込まないので、プロファイルに提案を提示しないことを期待しています。 Decisioningサービスは、提案を正式なXDM決定イベントとして記録せず、レポートデータセットにも表示しません。 このプロパティのデフォルト値はfalseで、プロパティが省略された場合、判定はテストの実行と見なされないので、エンドユーザーに表示されます。
- **`xdm:validateContextData`**  — このオプションのプロパティは、コンテキストデータの検証のオン/オフを切り替えます。 検証がオンになっている場合、指定された各コンテキストデータ項目に対して、スキーマ(フィールドに基づく `@type` )がXDMレジストリから取得され、オブジェクトがそれに対し `xdm:data` て検証されます。

このスキーマごとの要求には、オファーアクティビティ、プロファイルIDおよびコンテキストデータ項目の配列を参照するURIの配列が含まれます。

- **`xdm:offerActivities`**  — この必須プロパティは、各項目にオファーアクティビティのデータが含まれるオブジェクトの配列です。 オファーアクティビティには次のプロパティがあります。
   - **`xdm:offerActivity`** -アクティビティの固有な識別子(URI)。 これは、オファーアクティビティのプロパティの値です。 `@id`
- **`xdm:identityMap`** - XDMスキーマに準拠するJSONオブジェクトを保持する必須プロパティ `https://ns.adobe.com/xdm/context/identitymap`。 このプロパティは、キーがID名前空間コードで、値が特定の名前空間内のエンドユーザ識別子のリストであるマップを定義します。 もしm.
- **`xdm:contextData`**  — アイテムへの参照で記述されるアイテムを含むオプションのプロパティスキーマ。 配列内の各コンテキストデータ項目には、次のプロパティが必要です。
   - **`@type`**  — この項目のオブジェクトのXDMスキーマを参照する必須プロパティ。
   - **`xdm:data`**  — プロパティで指定されたXDMスキーマごとのオブジェクトプロパティを含む必須プロ `@type` パティ。

## Decisioningリクエストの動的コンテキストデータ

前の節では、XDMオブジェクトをデシジョンリクエストに渡す方法を示しました。 次に、このようなコンテキストオブジェクト配列の例を示します。

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

スキーマは組織によって構築された必要があります。 スキーマの作成方法については、 [スキーマエディタのチュートリアルを参照](../../xdm/tutorials/create-schema-ui.md)。 あなたのスキーマは名前空間だ `https://ns.adobe.com/{TENANT_ID}/schemas`。

『 [スキーマレジストリAPI開発者ガイド](../../xdm/tutorials/create-schema-api.md) 』では、スキーマにプログラムを使用してアクセスする方法と、開発者がテナントIDとスキーマの数値識別子を取得する方法について説明しています。 バージョン番号は必須で、バージョンレジストリAPIからもスキーマ番号が提供されます。

組織で定義されたスキーマには、通常、テナントの名前空間文字列とも呼ば `_{TENANT_ID}`れる、という名前のルートプロパティがあります。
_などのグローバルスキーマコンポーネントで使用されるプロパティには、`https://ns.adobe.com/xdm/context/product` 「_」というプリフィックスが付きま `xdm:`す。 この場合、組織定義のプロパティはそのデ `productDetails` ータ型を使用して構築されます。 テナントプロパティはテナント名前空間にちなんで名前が付けられたプロパティにネストされますが、グローバルに使用可能なデータ型は予約済みのプレフィッ `xdm:` クスを使用して、プロパティ名の競合を防ぎます。

このプロパティには複数のデータオブジェクトを表示で `xdm:contextData` きます。 各オブジェクトは、プロパティを介して型を識別する必要が `@type` あります。
コンテキストデータオブジェクトの値は、PQL式(例えば、実施要件ルールの条件)で使用できます。 コンテキストデータオブジェクトは、特別なパス参照式を通じて指定する必要がありま `@{schemaId}`す。 この参照式に続く式は、データオブジェクトのプロパティにアクセスする正規パス式です。

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

上の例では、変数は、 `p` =でマークされたオブジェクトの配列に対して反復 `@type` します `https://ns.adobe.com/{TENANT_ID}/schemas/{NUMERIC_SCHEMA_ID}}`。

PQL構文では、プロパティ名に接頭辞は使用されません。 デフォルトでは、グローバルプロパティはプレフィックスなしで参照さ `xdm:` れます。 組織で定義したプロパティは、テナント **名前空間** （変数で示される例）の名前にちなんだ追加のプロパティ内にネストさ `{TENANT_ID}`れます。 カスタム定義プロパティを直接参照できるように、変数は、追加のネ `p` ストプロパティを参照解除するパスの結果に連結されます。

## プロファイルレコードの使用

プロファイルおよびエクスペリエンスイベントエンティティのすべてのレコードは、既にプロファイルストアで管理されています。 1つ以上のプロファイルIDを要求に渡すと、そのIDのプロファイルが識別され、ストアから検索されます。 その後、データは、決定戦略によって評価された決定ルールおよびモデルに対して自動的に使用できます。

デフォルトの結合ポリシーが適用され、プロファイルおよびエクスペリエンスの記録を取得します。
プロファイルレコードをPlatformデータレークにアップロードした後、プロファイルレコードを検索できるまで、若干の遅延が生じます。 ストリーミングAPIを使用してプロファイルおよびエクスペリエンスレコードを取り込む場合も同様ですが、数秒後にのみ、プロファイルおよびエクスペリエンスイベントデータを評価する決定ルールの評価用にデータを使用できます。