---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: APIを使用したDecisioningサービスエンティティの管理
topic: tutorial
translation-type: tm+mt
source-git-commit: df85ea955b7a308e6be1e2149fcdfb4224facc53

---


# APIを使用したDecisioningのオブジェクトとルールの管理

このドキュメントでは、Adobe Experience Platform APIを使用してDecisioningサービスのビジネスエンティティを操作するためのチュートリアルを提供します。

このチュートリアルは、次の2つの部分で構成されています。

- ビジネスオブジェクトを管理する汎用リポジトリAPIは、最初の部分で [導入されま](#managing-repository-entities-using-apis)す。 これらのAPIは、あらゆる種類のビジネスオブジェクトの作成、読み取り、更新、削除および検索機能を提供する **という意味で** 、汎用的です。 一般的なAPIモデルが説明され、ハイパーテキストアプリケーション言語(HAL)との関係が説明されます。

- 第2部では、リポジトリAPIに関する知識を適 [用し、リポジトリ](#creating-and-managing-offer-decisioning-entities-using-apis) APIを介して管理されるビジネスエンティティに焦点を当てます。 同じAPIを適用した場合、アクティビティとビジネスルールなど2つの異なるエンティティの管理で違うのは、リクエストと応答のペイロードと、管理対象のオブジェクトのタイプを示す必要なヘッダー値のみです。

## はじめに

このチュートリアルでは、Experience PlatformサービスとAPIの規則に関する十分な理解が必要です。 プラットフォームリポジトリは、ビジネスオブジェクトや様々なタイプのメタデータを保存するために、他の複数のプラットフォームサービスで使用されるサービスです。 複数のランタイムサービスで使用するために、これらのオブジェクトを安全で柔軟な方法で管理およびクエリできます。 Decisioningサービスはその1つです。 このチュートリアルを開始する前に、次のドキュメントを確認してください。

- [エクスペリエンスデータモデル(XDM)](../../xdm/home.md):プラットフォームが顧客エクスペリエンスデータを整理する際に使用する標準化されたフレームワーク。
- [Decisioningサービス](./../home.md):Experience Decisioningの一般的な用途と、特にExperience Decisioningで使用される概念とオファーのコンポーネントについて説明します。 お客様のエクスペリエンスで提示する最適なオプションを選択するために使用される戦略を示します。
- [プロファイルクエリ言語(PQL)](../../segmentation/pql/overview.md):PQLはXDMインスタンスを使って式を書く強力な言語です。 PQLは、決定ルールの定義に使用されます。

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

>[!NOTE] プラットフォームのサンドボックスについて詳しくは、サンドボックスの概要ドキュメ [ントを参照してくださ](../../sandboxes/home.md)い。

ペイロード(POST、PUT、PATCH)を含むすべてのリクエストには、次のヘッダーが追加で必要です。

- コンテンツタイプ：application/json

## リポジトリAPIの規則

Decisioningサービスは、相互に関連する多数のビジネスオブジェクトによって制御されます。 すべてのビジネスオブジェクトは、プラットフォームのビジネスオブジェクトリポジトリに保存されます。 このリポジトリの主な特徴は、APIがビジネスオブジェクトのタイプと直交していることです。 APIエンドポイント内のリソースのタイプを示すPOST、GET、PUT、PATCH、またはDELETE APIを使用する代わりに、6つの汎用エンドポイントしか存在しませんが、その曖昧性を解除する必要がある場合に、オブジェクトのタイプを示すパラメータを受け取るか返します。 スキーマはリポジトリに登録する必要がありますが、その後、リポジトリはオープンエンドなオブジェクト型のセットに対して使用できます。

上記のヘッダーに加え、リポジトリオブジェクトを作成、読み取り、更新、削除およびクエリするAPIには、次の規則があります。

- とのすべてのリポジトリAPI開始のエンドポイントパ `https://platform.adobe.io/data/core/xcore/`ス。

APIペイロード形式は、またはヘッダーとネゴシエート `Accept` され `Content-Type` ます。 またはヘッダー内のメ `Accept` ッセ `Content-Type``application/vnd.adobe.platform.xcore.{FORMAT}+json` ージ形式は、次の表に従って、{FORMAT}が特定のリポジトリAPIリクエストまたは応答メッセージに依存する値で示されます。

| FORMATバリアント | 要求または応答エンティティの説明 |
| --- | --- |
| ハルフォロ<br>ーしたパラメーター `schema={schemaId}` | メッセージには、JSONパラメーターで記述されたインスタンスが含まれ、このスキーマーはformatパラメーターのスキーマで示されます。 インスタンスはJSONプロパティでラップされま `_instance`す。 応答ペイロードの他の最上位プロパティは、すべてのリソースで使用できるリポジトリ情報を指定します。  HAL形式に準拠するメッセージには、HAL形式の参 `_links` 照を含むプロパティが含まれています。 |
| `patch.hal` | メッセージにはJSON PATCHペイロードが含まれ、パッチ適用対象のインスタンスがHALに準拠していると想定しています。 つまり、インスタンスの固有のインスタンスプロパティだけでなく、インスタンスのHALリンクにもパッチを適用できます。 クライアントが更新できるプロパティには制限があります。 |
| `home.hal` | メッセージには、リポジトリのホームリソースのJSON形式のドキュメント表現が含まれています。 |
| xdm.receipt | メッセージには、作成、更新（完全およびパッチ）または削除操作に対するJSON形式の応答が含まれます。 受信には、ETagの形式でインスタンスのリビジョンを示す制御データが含まれます。 |

各形式のバリエーション **の使用方法は** 、特定のAPIによって異なります。

| API | Content-Typeヘッダ | ヘッダーを受け入れる |
| --- | --- | --- |
| 「Create Instance」「 <br/>Create」コンテナ | `hal`<br/>スキーマ | `xdm.receipt` |
| Update<br/>InstanceUpdateコンテナ | `hal`<br/>スキーマ | `xdm.receipt` |
| パッチインスタンス | `patch.hal` | `xdm.receipt` |
| Delete instanceDelete<br/>コンテナ | なし | `xdm.receipt` |
| 「Read<br/>InstanceRead」コンテナ | なし | `hal` パラメータ `schema` ー |
| リストイ<br/>ンスタンスリストコンテナ | なし | `hal` 特別なパラメータ `schema` ーで `https://ns.adobe.com/experience/xcore/hal/results` |
| 検索インスタンス | なし | 特別なパラメーターを持つハ `schema` ル `https://ns.adobe.com/experience/xcore/hal/results` |
| リポジトリのルートを読み取り | なし | `home.hal` |

コンテナの作成、更新、読み取りAPIの場合、formatパラメーターのスキーマーに値が含まれま `https://ns.adobe.com/experience/xcore/container`す。

`ContainerId` は、インスタンスAPIの最初のパスパラメーターです。 すべてのビジネスエンティティは、「エンティティ」と呼ばれるコンテナに存在します。 コンテナとは、異なる懸念を分離するための分離メカニズムです。 一般的なエンドポイントに続くリポジトリインスタンスAPIの最初のパス要素はです `containerId`。 識別子は、呼び出し元がアクセス可能なコンテナのリストから取得されます。 例えば、インスタンスを作成するAPIはです `POST https://platform.adobe.io/data/core/xcore/{containerId}/instances`。

アクセス可能なコンテナのリストは、標準のヘッダーを使用してHTTP GETリクエストでリポジトリのルートエンドポイント「/」を呼び出すことで取得されます。

## コンテナ

管理者は、類似したプリンシパル、リソース、アクセス権限をグループ化して、プロファイルにします。 これにより、管理の負担が軽減され、アドビの管 [理コンソールUIでサポートされます](https://adminconsole.adobe.com)。 プロファイルを作成し、ユーザーを割り当てるには、Adobe Experience Platformおよび組織内のオファーの製品管理者である必要があります。

1回限りの手順で特定の権限に一致する製品プロファイルを作成し、その後、それらのユーザーにプロファイルを追加するだけで十分です。 プロファイルは、権限が付与されたグループとして機能し、そのグループ内のすべての実際のユーザーまたは技術ユーザーは、権限を継承します。

### リストコンテナがユーザーや統合にアクセス可能

管理者が通常のコンテナまたは統合に対するコンテナへのアクセス権を付与すると、そのユーザーはリポジトリのいわゆる「ホーム」リストに表示されます。 リストは、呼び出し元がアクセスできるすべてのコンテナのサブセットなので、ユーザーや統合によって異なる場合があります。 コンテナのリストは、製品との関連付けによってフィルタリングできます。 フィルタパラメーターが呼び出され `product` 、繰り返し使用できます。 複数のコンテナコンテキストフィルターを指定した場合、指定した製品コンテキストのいずれかと関連付けられた製品の和集合が返されます。 1つの製品を複数のコンテナに関連付けることができます。コンテキスト

現在、Platform Decisioningサービスコンテナのコンテキストで `dma_offers`す。

>[!NOTE] Platform Decisioningのコンテナのコンテキストは、近日中にに変更されま `acp`す。 フィルタリングはオプションですが、フィルターによる編集は、 `dma_offers` 今後のリリースでのみ必要になるものになります。 この変更に備えるには、クライアントはフィルターを使用しないか、両方の製品コンテキストをフィルタとして適用します。

**リクエスト**

```shell
curl -X GET {ENDPOINT_PATH}/?product=dma_offers&product=acp \ 
  -H 'Accept: application/vnd.adobe.platform.xcore.home.hal+json' \ 
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \ 
  -H 'x-api-key: {API_KEY}' \ 
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  _H 'x-sandbox-name: {SANDBOX_NAME}' \ 
  -H 'x-request-id: {NEW_UUID}' 
```

**応答**

```json
{ 
    "_embedded": { 
        "https://ns.adobe.com/experience/xcore/container": [ 
            { 
              "instanceId": "82d1f250-85b6-11e9-ac80-99ba4655b277", 
              "schemas": [ 
                "https://ns.adobe.com/experience/xcore/container;version=0.1" 
              ], 
              "productContexts": [ 
                "dma_offers" 
              ], 
              "repo:etag": 1, 
              "repo:createdDate": "2019-06-03T04:17:33.684Z", 
              "repo:lastModifiedDate": "2019-06-03T04:17:33.684Z", 
              "repo:createdBy": "CREATOR_ACCOUNT_ID", 
              "repo:lastModifiedBy": "LAST_UPDATE_ACCOUNT_ID", 
              "repo:createdByClientId": "CLIENT_ID_OR_API_KEY", 
              "repo:lastModifiedByClientId": "CLIENT_ID_OR_API_KEY", 
              "_instance": { 
                "repo:name": "My Organization's container", 
                "dataCenter": "VA7" 
              }, 
              "_links": { 
                "self": { 
                  "href": "/containers/82d1f250-85b6-11e9-ac80-99ba4655b277" 
                } 
              } 
            } 
        ] 
    }, 
    "_links": { 
        "self": { 
            "href": "/"  
        } 
    } 
}  
```

結果項目 `instanceId` に一覧が表示されることを確認します。 通常のビジネスオブジェクトの読み取 `containerId` りと操作を行うために、APIのパラメーターとして使用されます。

リストは、アクセス権限に基づいて既にユーザー用にフィルタリングされていますが、プロパティクエリでフィルタリングできます。

## エンティティを管理する汎用API

### インスタンスの作成

リポジトリで新しいインスタンスを作成するAPIは、 `containerId` pathパラメーターを使用し、ヘッダー内のインスタンスのタイプを、そのイ `Content-Type` ンスタンスのスキーマパラメーターで識別します。

インスタンスプロパティは、プロパティでラップされたペイロードで提供 `_instance` されます。 インスタンスプロパティは、指定したスキーマIDを持つJSONスキーマに対して有効である必要があります。

HALプロパティ `_links` は存在する必要がありますが、空にすることができます。 つまり、このインスタンスに対してカスタムリンクが定義されていません。

**リクエスト**

```shell
curl -X POST {ENDPOINT_PATH}/{CONTAINER_ID}/instances \ 
  -H 'Content-Type: application/vnd.adobe.platform.xcore.hal+json; schema="{SCHEMA_ID}"' \  
  -H 'Accept: application/vnd.adobe.platform.xcore.xdm.receipt+json \ 
  -H 'x-api-key: {API_KEY}' \ 
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  _H 'x-sandbox-name: {SANDBOX_NAME}' \ 
  -H 'x-request-id: {NEW_UUID}' \ 
  -d '{ 
    "_instance": { 
        {JSON_PAYLOAD} 
    }, 
    "_links": { 
    } 
}' 
```

**応答**

```json
{ 
  "instanceId": "3684ceb0-8744-11e9-a989-89f60b24f6cc", 
  "@id": "GENERATED_URI", 
  "repo:etag": 1, 
  "repo:createdDate": "2019-06-05T03:44:25.343Z", 
  "repo:lastModifiedDate": "2019-06-05T03:44:25.343Z", 
  "repo:createdBy": "YOUR_TECHNICAL_ACCOUNT_ID", 
  "repo:lastModifiedBy": "YOUR_TECHNICAL_ACCOUNT_ID", 
  "repo:createdByClientId": "YOUR_API_KEY", 
  "repo:lastModifiedByClientId": "YOUR_API_KEY" 
}
```

応答には、作成されたオブジェクトのinstanceIdが含まれます。 このinstanceIdは不変で、常にリポジトリによって割り当てられ、グローバルに一意です。 この値は、物理的な識別子として機能します。

また、応答ペイロードのプロパティーにユニバーサルリソース識別子(URI)が返さ `@id` れる(そのようなプロパティーがスキーマにある場合)。 各インスタンスには、インスタンスのURIと主キーとして機能するプロパティが必要です。 この識別子は、別のインスタンスが使用して、異なるタイプを含む新しいインスタンスとの関係を形成します。 現在のリリースでは、URIはリポジトリによって生成され、プロパティに含まれ `@id` ます。 将来のバージョンでは、このルールが緩和され、クライアントが独自のURI値を管理し、そのURIを含むプロパティに名前を付けることができるようになる可能性があります。

これらのURIはURLではなく、リソースを直接取得する方法を提供しないことに注意してください。 この縦横比を示すため、URIには、取得プロトコルを指定しないURIスキームのプレフィックスが付けられます。 ただし、このURIを使用して、インスタンスを検索する際にクエリを使用できます。

REST応答には、作成されたインスタンスの取得に使用できるURLコンポーネントを含むLocationヘッダーが含まれます。 このコンポーネントは相対URI参照で、リポジトリのベースURIに適用する必要があります。 ベースURIがヘッダーに返され `Content-Base` ます。

このプロ `repo:etag` パティは、インスタンスのリビジョンを指定します。 この値は、整合性を強化するために、更新操作で使用できます。 HTTPヘッダーを使 `If-Match` 用して、PUTまたはPATCH API呼び出しに条件を追加し、誤って上書きされる可能性のあるインスタンスに対する他の変更がないようにします。 この値 `repo:etag` は、作成、読み取り、更新、削除およびクエリの呼び出しごとに返されます。 この値は、 ` If-Match` RFC7232 Section 3.1に従って、ヘッダーの値と [して使用](https://tools.ietf.org/html/rfc7232#section-3.1)されます。

残りのプロパティは、インスタンスの作成と最後の変更に使用されたアカウントとAPIキーを示します。 この呼び出しによってインスタンスが作成されたので、それぞれの値がリクエストの値になります。

### IDによるインスタンスの参照

Create呼び出しで返されるLocationヘッダーのURLを使用して、アプリケーションはインスタンスを検索できます。

**リクエスト**

```shell
curl -X GET {ENDPOINT_PATH}/{CONTAINER_ID}/instances/{INSTANCE_ID} \ 
  -H 'Accept: *, application/vnd.adobe.platform.xcore.hal+json; schema="{SCHEMA_ID}" \ 
  -H 'x-api-key: {API_KEY}' \ 
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  _H 'x-sandbox-name: {SANDBOX_NAME}' \ 
  -H 'x-request-id: {NEW_UUID}'  
```

>[!NOTE] がパスパ `instanceId` ラメーターとして指定されている場合でも、可能な限り、アプリケーションはパス自体を構築せず、リストや検索操作に含まれるインスタンスへのリンクに従う必要があります。 詳しくは‎、6.4.4と‎6.4.6の節を参照してください。

**応答**

```json
{ 
  "instanceId": "ID_OF_THIS_INSTANCE", 
  "schemas": [ 
    "SCHEMA_ID_OF_INSTANCE" 
  ], 
  "repo:etag": 1, 
  "repo:createdDate": "2019-03-24T15:52:12.725Z", 
  "repo:lastModifiedDate": "2019-03-24T15:52:12.725Z", 
  "repo:createdBy": "CREATOR_ACCOUNT_ID", 
  "repo:lastModifiedBy": "LAST_UPDATE_ACCOUNT_ID", 
  "_instance": { 
    JSON_PROPERTIES_OF_THIS_INSTANCE 
  }, 
  "_links": { 
    "self": { 
      "name": "GENERATED_UNIQUE_LINK_NAME", 
      "href": "RELATIVE_URL_TO_INSTANCE" 
    } 
  } 
} 
```

インスタンスのJSONプロパティはプロパティに含まれ、他のル `_instance` ートレベルのプロパティはインスタンスに関するメタデータを保持します。

リソースには、JSONリソースIDの配列も含まれています。スキーマIDは この配列は、このインスタンスの検証対象のJSONスキーマを示します。

各インスタンスには、IANA登録自己関係( [RFC5988で定義される])に対応する関係タイプselfのHALリンクが含まれます。

**インスタンスの新しいリビジョンのテスト**

インスタンス `eTag` の現在の値が応答と共に返され、クライアントはインスタンスに対して条件演算を実行できます。これにより、同じリソース状態を再度取得しないか、クライアントの知識なしに後のリビジョンの値を上書きしないようにできます。

ルックアップAPIを使用すると、クライアントはヘッダーパラメーターを指 `If-None-Match` 定できます。 この標準HTTPパラメータ [RFC2616の定義を参照してください]。 クライアントが指定するエンティティタグ値は、更新、読み取り、リスト、または検索API呼び出しからの最新の応答で受け取った値です。 値はクライアント `etag` に対して不透明で、引用符で囲まれた文字列として指定する必要があります。

**リクエスト**

```shell
curl -X GET {ENDPOINT_PATH}/{CONTAINER_ID}/instances/{INSTANCE_ID} \ 
  -H 'Accept: *, application/vnd.adobe.platform.xcore.hal+json; schema="{SCHEMA_ID}" \ 
  -H 'If-None-Match: "{LAST_RECEIVED_ETAG}" \ 
  -H 'x-api-key: {API_KEY}' \ 
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  _H 'x-sandbox-name: {SANDBOX_NAME}' \ 
  -H 'x-request-id: {NEW_UUID}'  
```

リポジトリAPIは、インスタンスの最後のリビジョンが指定されたetagである場合、ステータス304「変更なし」で応答します。

### リストのスキーマインスタンス — 並べ替えとページング

クライアントは、作成中のインスタンスを追跡できないので、物理instanceIdを使用してそれらのインスタンスにアクセスできます。 ReadインスタンスAPIの使用は例外です。 また、クライアントは、他のクライアントが作成したインスタンスにも気付きません。

より一般的なアクセスパターンは、すべてのインスタンスのセットをページごとに表示することです。

**リクエスト**

```shell
curl -X GET {ENDPOINT_PATH}/{CONTAINER_ID}/instances?schema="{SCHEMA_ID}" \ 
  -H 'Accept: *, application/vnd.adobe.platform.xcore.hal+json; schema="https://ns.adobe.com/experience/xcore/hal/results" \ 
  -H 'x-api-key: {API_KEY}' \ 
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  _H 'x-sandbox-name: {SANDBOX_NAME}' \ 
  -H 'x-request-id: {NEW_UUID}'  
```

**応答**

応答は指定した値に依存 `{schemaId}` します。 例えば、「https<span></span>://ns.adobe.com/experience/オファー管理/オファー-アクティビティ&amp;id=xcore:オファー-アクティビティ:fa24f9e8fc15c73」の応答は、次のようになります。

```json
{
"requestTime": "2019-06-28T06:54:05.606Z",
"_embedded": {
  "results": [],
  "total": 0,
  "count": 0
  },
  "_links": {
  "self": {
  "href": "/653da250-71b8-11e9-a3fe-9b1d0913f3ed/instances?schema=https://ns.adobe.com/experience/offer-management/offer-activity&id=xcore:offer-activity:fa24f9e8fc15c73",
"@type": "https://ns.adobe.com/experience/xcore/hal/results"
  }
  },
  "containerId": "653da250-71b8-11e9-a3fe-9b1d0913f3ed",
  "schemaNs": "https://ns.adobe.com/experience/offer-management/offer-activity;version=0.1"
}
```

>[!NOTE] 結果には、指定したインスタンスまたはこのスキーマの最初のページのインスタンスが含まれます。リスト インスタンスは複数のスキーマに準拠できるので、複数のインスタンスに表示できます。

ページリソースは一時的で読み取り専用です。更新や削除はできません。 ページングモデルは、クライアントごとの状態を維持することなく、長期間にわたって、大きなリストのサブセットにランダムアクセスを提供する。

この方法でインスタンスリストにページ別にアクセスするには、そのインスタンスリストで列挙されるエントリに対して安定した並べ替えを定義できる必要があります。 「安定」とは、インスタンスが事前に決められたページに表示されることを意味しません。 実際、インスタンスをプロパティPに従って並べ替えてページ順が形成され、クライアントがこのプロパティPを更新すると、別のクライアントがリストをページングしながら、別のページでこのインスタンスに再び到達できます。 つまり、モデルは、より最新の結果を返す方が優先されます。

ただし、並べ替え順序が変更不可能なプロパティに基づく場合、「安定した」並べ替え順序により、ページ操作の最初に存在したすべてのインスタンスに到達することが保証されます（ページに到達した時点までに削除された場合を除く）。 この並べ替え順が単調に増加するプロパティに対して行われる場合、ページング操作の開始後に作成されるインスタンスにも到達します。

クライアントは、必要なページサイズに関するヒントを提供できますが、返されるページのインスタンスを増減するのはリポジトリのみです。 安定した順序を保証するために、ページのソートプロパティの値がページの境界で異なるように、サービスはページの項目を追加または削除する必要があります。 この方法を使用すると、次のページに最後のページの一部の項目が再度含まれなくなるか、最悪の場合は各ページの同じ項目が表示されなくなります。

ページングは、次のパラメーターで制御します。

- **`orderBy`**:インスタンスリストの並べ替えに使用するプロパティのカンマ区切りの順序付きリストが含まれます。 最初のプロパティはプライマリ並べ替えに使用され、2番目のプロパティはプライマリ並べ替えでの関係を解決するなどに使用されます。 インスタンスごとに一意の値を持つプロパティを指定した場合、結び付けはできず、各項目の後に改ページが発生する可能性があります。 プロパティの名前の先頭には、昇順を示すプリフィッ `+` クスを付けたり、そのプロパティ `-` で降順を示すプリフィックスを付けたりできます。 プロパティ名の前に接頭辞が付いていない場合、結果は昇順で並べ替えられます。 リクエ `orderBy` ストで指定されていない場合、リポジトリは代わりに物理instanceIdプロパティを使用します。
- **`start`**:クライアントは開始パラメータを使用して、取得するページを定義します。 開始パラメーターは、目的のページの開始を決定します。 応答には、プロパティ値が厳密に（昇順の場合）より大きいか、または厳密に（降順の場合）より小さい場合に始まるインスタンスが含まれます。 `orderBy` クエリパラメーターを指定しない場合、最初の可能なインスタンス識別子の前に並べ替えるinstanceId値がデフォルト値になるので、この値は最初のページから省略されます。
- **`limit`**:は、特定のリクエストに対して返す必要のある最大アイテム数に関するヒントとして正の整数を指定します。 実際の応答サイズは、応答パラメータの信頼性の高い動作を提供する必要性によって制約される、より小さいか大きい開始です

### フィルタリスト

フィルタリストの結果は可能で、ページングメカニズムとは無関係に発生します。 フィルターは、リストの順序でインスタンスをスキップするか、特定の条件を満たすインスタンスのみを含めるよう明示的に求めます。 クライアントは、プロパティ式をフィルターとして使用するように要求したり、インスタンスのプライマリキーの値として使用するURIのリストを指定したりできます。

- **`property`**:プロパティ名のパスの後に、比較演算子と値が続くパスが含まれます。 <br/>
返されるリストのインスタンスには、式がtrueと評価されるインスタンスが含まれます。 例えば、インスタンスにペイロードプロパティがあり、可能な値が `status` 、 `draft`，であると仮定し、 `approved``archived` クエリパラメーターは、ステータスが承認されたインスタ `deleted``property=_instance.status==approved` ンスのみを返します。 <br/>
<br/>
渡された値と比較されるプロパティは、パスとして識別されます。 個々のパスコンポーネントは、次のように`.`で区切られます。`インスタンス.xdm:prop1.xdm:prop1_1.xdm:prop1_1_1`<br/>

文字列、数値、または日付/時間値を持つプロパティの場合、次の演算子を使用できます。 `==`、、、、、、 `!=`およ `<`びで `<=`す `>``>=`。 また、文字列値を持つプロパティの場合は、演算子を使 `~` 用できます。 演算子 `~` は、正規式に従って、特定のプロパティを フィルター結果にエンティティを含めるには **** 、プロパティの文字列値が式全体と一致する必要があります。 例えば、プロパティ値内の任意の場 `cars` 所で文字列を検索する場合、正規式が必要で `.*cars.*`す。 先頭または末尾がな `.*`い場合、エンティティのみがプロパティ値の先頭または末尾がで始まるエンティティと一致 `cars`します。 演算子の場合、 `~` 文字の比較では大文字と小文字が区別されません。 その他の演算子の場合は、大文字と小文字が区別されます。<br/><br/>
インスタンスのペイロードプロパティだけでなく、フィルター式でも使用できます。 エンベロープのプロパティは、 `property=repo:lastModifiedDate>=2019-02-23T16:30:00.000Z`. <br/>
<br/>クエリ `property` パラメーターを繰り返して、複数のフィルター条件を適用できます。例えば、特定の日付の後、特定の日付の前に最後に変更されたすべてのインスタンスを返します。 これらの式の値は、URLエンコードする必要があります。 式を指定せず、プロパティの名前が単純に表示される場合は、指定した名前のプロパティを持つ項目が該当します。<br/>
<br/>

- **`id`**:場合によっては、リストをインスタンスのURIでフィルタリングする必要があります。 クエリ `property` パラメーターを使用して1つのインスタンスをフィルターで除外できますが、複数のインスタンスを取得する場合は、URIのリストをリクエストに渡すことができます。 このパラ `id` メーターが繰り返され、各オカレンスで1つのURI値が指定されます。URI値 `id={URI_1}&id={URI_2},…` はURLエンコードする必要があります。

ページの結果は、特別なMIMEタイプとして返されま `application/vnd.adobe.platform.xcore.hal+json; schema="https://ns.adobe.com/experience/xcore/hal/results"`す。

**リクエスト**

```shell
curl -X GET {ENDPOINT_PATH}/{CONTAINER_ID}/instances?schema="{SCHEMA_ID}"&orderby${ORDER_BY_PROPERTY_PATH}&property={TIMESTAMP_PROPERTY_PATH}>=2019-02-19T03:19:03.627Z&property${TIMESTAMP_PROPERTY_PATH}<=2019-06-19T03:19:03.627Z \ 
  -H 'Accept: *, application/vnd.adobe.platform.xcore.hal+json; schema="https://ns.adobe.com/experience/xcore/hal/results" \ 
  -H 'x-api-key: {API_KEY}' \ 
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  _H 'x-sandbox-name: {SANDBOX_NAME}' \ 
  -H 'x-request-id: {NEW_UUID}'  
```

**応答**

```json
{ 
  "requestTime": "2019-06-10T22:12:13.642Z", 
  "_embedded": { 
    "results": [ 
      { 
        "instanceId": "ID_OF_THIS_INSTANCE", 
        "schemas": [ 
          "SCHEMA_ID_OF_INSTANCE" 
        ], 
        "repo:etag": 1, 
        "repo:createdDate": "2019-04-19T03:19:03.627Z", 
        "repo:lastModifiedDate": "2019-04-19T03:19:03.627Z", 
        "repo:createdBy": "CREATOR_ACCOUNT_ID", 
        "repo:lastModifiedBy": "LAST_UPDATE_ACCOUNT_ID", 
        "_instance": { 
          JSON_PROPERTIES_OF_THIS_INSTANCE 
        }, 
        "_links": { 
          "self": { 
            "name": "GENERATED_UNIQUE_LINK_NAME", 
            "href": "RELATIVE_URL_TO_INSTANCE" 
          } 
        } 
      }, 
      { 
        "instanceId": "ID_OF_THIS_INSTANCE", 
        "schemas": [ 
          "SCHEMA_ID_OF_INSTANCE" 
        ], 
        "repo:etag": 1, 
        "repo:createdDate": "2019-04-19T20:30:31.361Z", 
        "repo:lastModifiedDate": "2019-04-19T20:30:31.361Z", 
        "repo:createdBy": "CREATOR_ACCOUNT_ID", 
        "repo:lastModifiedBy": "LAST_UPDATE_ACCOUNT_ID", 
        "_instance": { 
          JSON_PROPERTIES_OF_THIS_INSTANCE 
        }, 

        "_links": { 
          "self": { 
            "name": "GENERATED_UNIQUE_LINK_NAME", 
            "href": "RELATIVE_URL_TO_INSTANCE" 
          } 
        } 
      } 
    ], 
    "total": 2, 
    "count": 2 
  }, 
  "_links": { 
    "self": { 
      "href": "RELATIVE_URL_TO_THIS_RESULT" 
    } 
  }, 
  "containerId": "CONTAINER_ID_OF_THIS_LIST", 
  "schemaNs": "SCHEMA_ID_OF_INSTANCE_LIST" 
} 
```

応答には、このページの結果の数と、返されたページから始まるフィルターリスト内のアイテムの合計数を示す2つのプロパティの横のJSONプロパティの結果内の結果アイテムのリストが含まれます。

### フルテキスト検索と構造化クエリ

クライアントが文字列プロパティに含まれる用語別により複雑なフィルター条件や検索インスタンスを提供したい場合、リポジトリオファーはより強力な検索APIを提供します。

**リクエスト**

```shell
curl -X GET {ENDPOINT_PATH}/{CONTAINER_ID}/queries/core/search?schema="{SCHEMA_ID}"&… \ 
  -H 'Accept: *, application/vnd.adobe.platform.xcore.hal+json; schema="https://ns.adobe.com/experience/xcore/hal/results" \ 
  -H 'x-api-key: {API_KEY}' \ 
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  _H 'x-sandbox-name: {SANDBOX_NAME}' \ 
  -H 'x-request-id: {NEW_UUID}'  
```

<!-- TODO: needs example response -->

このAPIを使用すると、リストAPIのページングパラメーターとフィルターパラメーターに加えて、クライアントは全文およびブールクエリパラメーターを追加できます。

全文検索は、次のパラメーターで制御します。

- **`q`**:インスタンスの文字列プロパティと一致する前に正規化される、スペースで区切られた、順序のないリストの用語が含まれます。 文字列プロパティが用語に対して分析され、それらの用語も正規化されます。 検索クエリは、パラメーターで指定された1つ以上の用語と一致しようと `q` します。 +、-、=、&amp;&amp;、||、>、&lt;、!、(、)、{、}、 [,]、^、&quot;、～、*、?、:、/は、クエリ文字列内の単語の境界を決定する特別な意味を持ち、文字と一致するトークン内に出現する場合はバックスラッシュでエスケープする必要があります。 クエリ文字列は、完全に一致する文字列を重複引用符で囲み、特殊文字をエスケープできます。
- **`field`**:検索語句がプロパティのサブセットに対してのみ一致する必要がある場合、fieldパラメーターはそのプロパティのパスを示すことができます。 このパラメーターを繰り返して、一致する必要のある複数のプロパティを示すことができます。
- **`qop`**:検索の一致動作を変更するために使用される制御パラメータが含まれます。 パラメーターをに設定した後、すべての検索用語が一致し、パラメーターが存在しない場合、またはその値がに設定された場合、またはいずれかの用語が一致としてカウントされます。

### インスタンスの更新とパッチ適用

インスタンスを更新するには、クライアントがプロパティの完全なリストを一度に上書きするか、JSON PATCHリクエストを使用してリストを含む個々のプロパティ値を操作します。

どちらの場合も、リクエストのURLは物理インスタンスへのパスを指定し、どちらの場合も、応答は作成操作から返されるJSON受信ペイロードと同様のJSON受信ペイロー [ドになります](#create-instances)。 クライアントは、このAPIの完全なURLパスとして、 `Location` このオブジェクトに対する以前のAPI呼び出しから受け取ったヘッダーまたはHALリンクを使用することをお勧めします。 これが不可能な場合、クライアントは、とからURLを構築 `containerId` できます `instanceId`。

**リクエスト** (PUT)

```shell
curl -X PUT {ENDPOINT_PATH}/{CONTAINER_ID}/instances/{INSTANCE_ID} \ 
  -H 'Content-Type: application/vnd.adobe.platform.xcore.hal+json; schema="{SCHEMA_ID}"' \  
  -H 'Accept: application/vnd.adobe.platform.xcore.xdm.receipt+json \ 
  -H 'x-api-key: {API_KEY}' \ 
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  _H 'x-sandbox-name: {SANDBOX_NAME}' \ 
  -H 'x-request-id: {NEW_UUID}'\ 
  -d '{ 
  "_instance": { 
    {JSON_PROPERTIES_OF_THIS_INSTANCE} 
  }, 
  "_links": { 
    {HAL_LINKS_OF_THIS_INSTANCE} 
  } 
}'  
```

**リクエスト** （パッチ）

```shell
curl -X PATCH {ENDPOINT_PATH}/{CONTAINER_ID}/instances/{INSTANCE_ID} \ 
  -H 'Content-Type: application/vnd.adobe.platform.xcore.patch.hal+json; schema="{SCHEMA_ID}"' \  
  -H 'Accept: application/vnd.adobe.platform.xcore.xdm.receipt+json \ 
  -H 'x-api-key: {API_KEY}' \ 
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  _H 'x-sandbox-name: {SANDBOX_NAME}' \ 
  -H 'x-request-id: {NEW_UUID}' \ 
  -d '[ 
  { 
    {JSON_PATCH_INSTRUCTIONS_FOR_THIS_INSTANCE} 
  } 
]'
```

PATCHリクエストは、命令を適用し、結果のエンティティをスキーマと照合し、PUTリクエストと同じエンティティおよび参照整合性ルールを検証します。

**プロパティ値の編集の制御**

次の注釈を使用して、作成時や更新時にプロパティが設定されないようにすることができます。

- **`"meta:usereditable"`**:ブール型 — 要求元がユーザーまたは技術アカウントのアクセストークンで呼び出し元を識別するユーザーエージェントである場合、注釈が付けられたプロパティはペイロードに `"meta:usereditable": false` 存在しない必要があります。 値が存在する場合は、現在設定されている値と異なる値を持つことはできません。 値が異なる場合、更新またはパッチリクエストは、ステータス422の「処理不能エンティティ」で拒否されます。
- **`"meta:immutable"`**:Boolean — 注釈が付けられたプロパティは、 `"meta:immutable": true` 一度設定すると変更できなくなります。 これは、エンドユーザー、テクニカルアカウントの統合、または特別なサービスからの要求に適用されます。

**同時更新のテスト**

複数のクライアントが同時にインスタンスを更新しようとする場合があります。 リポジトリは、中央のトランザクション管理を行わない計算ノードのクラスタ上で動作します。 あるクライアントが別のクライアントによって同時に書き込まれるインスタンスを書き込むのを防ぐために、クライアントは条件付き更新またはパッチリクエストを使用できます。 リポジトリの `etag` ヘッダーに文字列を指 `If-Match` 定することで、最初のリクエストのみが成功し、同じ値を使用する他のクライアントの後続のリクエストが失敗する `etag` ことを確認します。 値は、イ `etag` ンスタンスを変更するたびに変更されます。 クライアントは、最新の値を取得するためにインスタ `etag` ンスを取得する必要があります。その後、更新を試みた多数のクライアントのうち、1つのクライアントのみが、その値で成功します。 他のクライアントは、409 Conflictというメッセージで拒否されます。

### インスタンスの削除

インスタンスは、DELETE呼び出しで削除できます。 クライアントは、以前にAPI呼び出し `Location` から受け取ったヘッダーまたはHALリンクを、完全なURLパスとして使用することが望ましい。 これが不可能な場合、クライアントは、および物理URLからURLを構 `containerId` 築できます `instanceId`。

**リクエスト**

```shell
curl -X DELETE {ENDPOINT_PATH}/{CONTAINER_ID}/instances/{INSTANCE_ID} \ 
  -H 'Accept: application/vnd.adobe.platform.xcore.xdm.receipt+json \ 
  -H 'x-api-key: {API_KEY}' \ 
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  _H 'x-sandbox-name: {SANDBOX_NAME}' \ 
  -H 'x-request-id: {NEW_UUID}'  
```

**応答**

```json
{ 
  "instanceId": "3684ceb0-8744-11e9-a989-89f60b24f6cc", 
  "@id": "INSTANCE_URI", 
  "repo:etag": 1, 
  "repo:createdDate": "2019-06-05T03:44:25.343Z", 
  "repo:lastModifiedDate": "2019-06-05T03:44:25.343Z", 
  "repo:createdBy": "CREATOR_ACCOUNT_ID", 
  "repo:lastModifiedBy": "YOUR_TECHNICAL_ACCOUNT_ID", 
  "repo:createdByClientId": "CREATOR_API_KEY", 
  "repo:lastModifiedByClientId": "YOUR_API_KEY" 
} 
```

削除リクエストを受け取ると、リポジトリは削除対象のインスタンスを参照し続ける他のスキーマのインスタンスをチェックします。 分散された高可用性システムでは、参照整合性を即座にチェックすることはできません。 外部キーの関係が定義されている場合、チェックは非同期で実行されます。 その結果、削除要求の結果に対する応答が少し遅れています。 これらのチェックが実行されると、即時応答には、ステータス202「受け入れ済み」と、ヘッダー内の削除操作の結果を確認するリンクが含ま `Location` れます。 その後、クライアントはそのリンクをチェックして結果を確認する必要があります。

削除するインスタンスを参照するインスタンスが見つかった場合、削除操作の拒否結果になります。 他の外部キー参照が見つからない場合は、削除が完了します。 結果がまだ決定されていない場合、応答は同じヘッダーを持つ別の202の「受け入れ済み」の応答を示し、クライア `Location` ントに確認を続けるよう求めます。 結果が決定されると、応答は200 OKステータスで応答に含まれ、応答のペイロードには元の削除要求の結果が含まれることを示します。 「200 OK」の応答は、結果が既知で、応答本文に削除要求の確認または拒否が含まれることを意味します。

## オファーとそのサブコンポーネントの作成

前の節で説明したAPIは、すべてのタイプのビジネスオブジェクトに一様に適用されます。 例えば、オファーの作成とアクティビティの違いは、スキーマに準拠するリクエストのJSONペイロードがJSON `content-type` スキーマであることを示すヘッダーだけです。 したがって、以降の節では、これらの関係とそれらのスキーマの関係に焦点を当てるだけです。

コンテンツタイプでAPIを使用する場合、イ `application/vnd.adobe.platform.xcore.hal+json; schema="{SCHEMA_ID}"`ンスタンス独自のプロパティが、プロパティが存在する横にあ `_instance` るプロパティに埋め込まれ `_links` ます。 これは、すべてのインスタンスが表される一般的な形式です。

```json
{ 
  … ENVELOPE PROPERTIES 
  "_instance": { 
    INSTANCE PROPERTIES 
  }, 
  "_links": {
    HAL PROPERTIES 
  } 
}
```

>[!NOTE] 簡潔な理由から、すべてのJSONスニペットでは、インスタンスプロパティのみが示され、必要な場合にのみエンベローププロパティと_linksセクションが示されます。

### 一般的なオファープロパティ

オファーは判定オプションの一種で、オファーのJSONスキーマは各オプションインスタンスが持つ標準のオプションプロパティを継承します。

- **`@id`**  — プライマリ・キーで、他のオブジェクトからオプションを参照するために使用される各オプションの一意の識別子。 このプロパティは、インスタンスの作成時に割り当てられ、不変であり、編集できません。
- **`xdm:name`**  — すべてのオプションには、検索や表示の目的で使用される名前が付けられます。 この名前は不変ではなく、インスタンスを一意に識別するために使用することはできません。 名前は自由に選択できますが、複数のインスタンスで一意である必要があります。オファーインスタンスでは

```json
{ 
  "@id": "INSTANCE_URI",                         // meta:immutable=true, meta:usereditable=false 
  "xdm:name": "A name for the Decision Option",  // meta:immutable=false 
  "xdm:characteristics": { 
    properties specific to this instance         // property names can vary per instance 
  } 
}
```

完全なcURL [構文のインスタンスの更新](#updating-and-patching-instances) 、およびパッチ適用を参照してください。 パラメー `schemaId` ターは、または `https://ns.adobe.com/experience/offer-management/personalized-offer` そのオファーがフ `https://ns.adobe.com/experience/offer-management/fallback-offer` ォールバックオファーの場合に、

各オファーインスタンスには、そのインスタンスのみに固有のプロパティのオプションのセットを含めることができます。 各オファーは、これらのプロパティに異なるキーを持つことができます。値は文字列である必要があります。 これらのプロパティは、決定およびセグメントルールで使用できます。 また、メッセージをさらにカスタマイズするために、決定したエクスペリエンスを組み立てることもできます。

### オファーライフサイクル

すべてのオプションがたどるシンプルな状態トランジションフローが存在します。 開始はドラフト状態で終了し、準備が整ったら、その状態が承認に設定されます。 終了日が過ぎると、アーカイブ済みの状態に移動できます。 この状態では、それらを再び製図状態に移動することで、削除または再利用できます。

![](../images/entities/offer-lifecycle.png)

- **`xdm:status`**  — このプロパティは、インスタンスのライフサイクル管理に使用されます。 この値は、オファーがまだ構築中（値=ドラフト）か、通常はランタイム（値=承認済み）か、それ以上使用しない（値=アーカイブ済み）かを示すために使用されるワークフロー状態を表します。

インスタンスの単純なPATCH操作は、通常、プロパティを操作する目的で使用さ `xdm:status` れます。

```json
[
  {
    "op":    "replace",
    "path":  "/_instance/xdm:status",
    "value": "approved" 
  }
]
```

完全なcURL [構文のインスタンスの更新](#updating-and-patching-instances) 、およびパッチ適用を参照してください。 パラメー `schemaId` ターは、または `https://ns.adobe.com/experience/offer-management/personalized-offer` そのオファーがフ `https://ns.adobe.com/experience/offer-management/fallback-offer` ォールバックオファーの場合に、

### 表示と配置

オファーとは、コンテンツを表す決定オプションです。 決定が行われると、このオプションが選択され、その識別子を使用して、指定する必要がある配置のコンテンツまたはコンテンツ参照が取得されます。 1つのオファーに複数の表現を含めることはできますが、それぞれ異なる配置参照を持つ必要があります。 これにより、特定の配置で、表示を明確に決定できます。
決定操作中、配置は、配置オブジェクトと組み合わせてアクティビティされます。 オファーの中で、その配置を参照として表現していないものは、選択肢のリストから自動的に削除されます。

リプレゼンテーションをオファーに追加する前に、配置インスタンスが存在する必要があります。 これらのインスタンスはスキーマIDで作成されます`https://ns.adobe.com/experience/offer-management/offer-placement`。

```json
{
  "xdm:name": "Kiosk Placement 1",
  "xdm:channel": "https://ns.adobe.com/xdm/channels/web",
  "xdm:componentType": 
     "https://ns.adobe.com/experience/offer-management/content-component-imagelink",
  "xdm:contentTypes": [
    "image/png", "image/png"
  ],
  "xdm:description": "Generic placeholder for offers in the Kiosk application. \nTechnical constraints: max width 530dpi, min width 480 dpi, aspect ratio 12:5. \nStylistic constraints: single background color with text block in complementary colors, \nNo magenta, please!"
} 
```

完全なcURL [構文のインスタンスの更新](#updating-and-patching-instances) 、およびパッチ適用を参照してください。 パラメー `schemaId` ターは、または `https://ns.adobe.com/experience/offer-management/personalized-offer` そのオファーがフ `https://ns.adobe.com/experience/offer-management/fallback-offer` ォールバックオファーの場合に、

Placementインス **タンスは** 、次のプロパティを持つことができます。

- **`xdm:name`**  — 人間のインタラクションおよびユーザーインターフェイスで参照する配置の割り当て名を含みます。
- **`xdm:description`**  — この配置でのコンテンツがメッセージ配信全体でどのように使用されるかを、人間が理解できる意図を伝えるために使用します。 配信チャネルが新しい配置を定義する場合は、このプロパティに情報を追加して、コンテンツの作成者がそれに応じてコンテンツを作成または選択できるようにすることができます。 これらの指示は、正式に解釈されたり、強制されたりはしません。 この財産は単に意図を伝える場所としての役割を果たす。
- **`xdm:channel`** -チャネルのURI。 チャネルは、動的コンテンツの配信先を示します。 チャネル制約は、オファーが使用される場所を伝達するだけでなく、エクスペリエンスに使用されるコンテンツエディターやバリデーターを判断する目的にも使用されます。
- **`xdm:componentType`**  — この配置で説明される場所に表示できるコンテンツのモデル識別子(URI)。 コンポーネントのタイプは次のとおりです。画像リンク、htmlまたはプレーンテキスト。 各コンポーネントタイプは、コンテンツアイテムに含まれる特定のプロパティのセット（モデル）を意味する場合があります。 コンポーネントタイプのリストは拡張できます。 次の3つの定義済みコンポーネントタイプ値があります。
   - `https://ns.adobe.com/experience/offer-management/content-component-imagelink`
   - `https://ns.adobe.com/experience/offer-management/content-component-text`
   - `https://ns.adobe.com/experience/offer-management/content-component-html`
- **`xdm:contentTypes`**、この配置に必要なコンポーネントのメディアタイプの制約。 異なる画像形式など、1つのコンポーネントの種類に対して複数のメディアの種類が存在する場合があります。

**配列プロパティに** 、オファー内の表現項目はオブジェクト構造を持ちま `xdm:representations`す。 各項目には、次のプロパティを設定できます。

- **`xdm:placement`**  — このプロパティには、配置インスタンスへの参照が含まれます。 この値は、リプレゼンテーションがオファーに追加されるときにチェックされます。 そのURIを持つプレースメントインスタンスが存在し、削除済みとしてマークされていない。 また、配置参照の値が同じである2つの表示がオファーインスタンスに含まれていないことを確認するチェックが実行されます。
- **`xdm:components`**  — コンテンツコンポーネントは、特定のフラグメントに関連付けられたオファー表示域です。 これらのフラグメントは、後でエンドユーザーエクスペリエンスの構成に使用されます。 Decisioningサービス自体は、エンドユーザーのフルエクスペリエンスを構成するわけではありません。 次のプロパティは、各コンポーネントのモデルの一部です。
   - **`@type`**  — このプロパティはコンポーネントタイプを識別します。 この概念の別の名前は、コンテンツフラグメントモデルです。 コンポ `@type` ーネントのは、エンドユーザーエクスペリエンスを組み立てるアプリケーションまたはサービスによって定義されるモデルのURIです。
   - **`repo:id`**  — このプロパティには、アセットが保存されるリポジトリ内のコンポーネントのメインリソースに対する、グローバルに一意の不変識別子が含まれます。
   - **`repo:name`**  — このプロパティには、リポジトリ内のアセットの読み取り可能な名前が含まれます。 この名前はユーザー定義で、一意であることは保証されません。
   - **`repo:resolveURL`**  — このプロパティには、コンテンツリポジトリ内のアセットを読み取る一意のリソースロケーターが含まれます。 これにより、呼び出すAPIをクライアントが把握しなくても、アセットを取得しやすくなります。 URLは、アセットのプライマリリソースのバイトを返します。
   - **`dc:format`**  — このプロパティは、Dublin Core Metadata Initiativeから取得されます。 フォーマットは、リソースの表示や操作に必要なソフトウェア、ハードウェア、またはその他の機器を決定するために使用できます。 ベストプラクティスは、制御された語彙から値を選択することです(例えば、コンピューターのメディア形式を定義するインターネットメディアタイプのリスト)。
   - **`dc:language`**  — このプロパティには、リソースの言語が含まれます。 言語は、IETF RFC 3066で定義されている言語コードで指定されています。

プロパティには、次の3つの定義済みコンポーネントタイプが `@type` あります。

- https<span></span>://ns.adobe.com/experience/content-management/content-component-imagelink
- https<span></span>://ns.adobe.com/experience/content-management/content-component-text
- https<span></span>://ns.adobe.com/experience/content-management/content-component-html

プロパティの値に応じて、追加の `@type` プロパティ `xdm:components` が含まれます。

- **`xdm:linkURL`**  — コンポーネントが画像リンクの場合に表示されます。 このプロパティには、画像に関連付けられ、エンドユーザが画像のコンテンツを操 `user-agent` 作したときに画像が移動するリンクが含まれます。
- **`xdm:copyline`**  — コンポーネントがテキストの場合に使用されます。 テキストアセットを参照する(例えば、書式を含む長いフォームのテキストオファーの場合)に加えて、短いテキスト文字列をxdm:copylineプロパティに直接格納できます。

追加のプロパティは、クライアントがコンテキスト処理命令を設定および評価するために使用できます。 例えば、オファーUIライブラリクライアントは、表示をより簡単に処理するために、次のオプションのプロパティを追加します。

- アレイ内の各アイテム内に、 `xdm:components` オファーライブラリUIクライアントが次のプロパティを追加します。 UIへの影響を理解しない限り、これらのプロパティを削除または操作しないでください。
   - **`offerui:previewThumbnail`**  — これは、アセットのレンダリングを表示するためにオファーライブラリのUIで使用されるオプションのプロパティです。 このレンディションは、アセット自体とは異なります。 例えば、コンテンツはHTMLで、レンディションはその近似を示すビットマップ画像のみです。 この（低品質の）レンディションは、オファーの表示ブロック内に表示されます。

オファーインスタンスでのPATCH操作の例は、表現を操作する方法を示します。

```json
[
  {
    "op":    "add",
    "path":  "/_instance/xdm:representations/-",
    "value": {
      "xdm:placement": "xcore:offer-placement:e51944a87919861",
      "xdm:components": [
        {
          "xdm:copyline": "Get what you want!",
          "@type": "https://ns.adobe.com/experience/offer-management/content-component-text",
          "dc:format": "text/plain"
        }
      ]
    } 
  }
]' 
```

完全なcURL [構文のインスタンスの更新](#updating-and-patching-instances) 、およびパッチ適用を参照してください。 パラメー `schemaId` ターは、または `https://ns.adobe.com/experience/offer-management/personalized-offer` そのオファーがフ `https://ns.adobe.com/experience/offer-management/fallback-offer` ォールバックオファーの場合に、

プロパティがまだない場合は、PATCH操作が失敗する場合があ `xdm:representations` ります。 この場合、上記の追加操作の前に、配列を作成する別の追加操作が続くか、または単一の追加操作が `xdm:representations` 配列を直接設定する可能性があります。
説明したスキーマとプロパティは、すべてのオファータイプ、パーソナライゼーションオファーおよびフォールバックオファーで使用されます。 次の2つのセクションで、パーソナライゼーションオファーの特徴を説明する制約と決定ルールについて説明します。

## オファー制約の設定

### カレンダーの制約

一般的な決定オプションには、開始と終了日時を指定し、カレンダーの制約として使用できます。 プロパティは次のプロパティに埋め込まれま `xdm:selectionConstraint`す。

- **`xdm:startDate`**  — このプロパティは開始の日時を示します。 値は、RFC 3339規則に従って形式設定された文字列です。例えば、次のタイムスタンプのようになります。&quot;2019-06-13T11:21:23.356Z&quot;
開始の日時に達していない決定オプションは、まだ決定に適格と見なされません。
- **`xdm:endDate`**  — このプロパティは、終了日時を示します。 値は、RFC 3339規則に従って形式設定された文字列です。例えば、次のタイムスタンプのようになります。「2019-07-13T11:00:00.000Z」終了日時を過ぎたデシジョニングオプションは、デシジョニングプロセスで適格と見なされなくなりました。

カレンダー制約の変更は、次のPATCH呼び出しを使用して実行できます。

```json
[
  {
    "op":   "replace",
    "path": "/_instance/xdm:selectionConstraint",
    "value": {
      "xdm:startDate": "2019-06-13T00:00:00.000Z",
      "xdm:endDate":   "2019-07-13T00:00:00.000Z"
    } 
  }
]' 
```

完全なcURL [構文のインスタンスの更新](#updating-and-patching-instances) 、およびパッチ適用を参照してください。 パラメータ `schemaId` ーは必ず指定する必要があり `https://ns.adobe.com/experience/offer-management/personalized-offer`ます。 フォールバックオファーには制約はありません。

### キャッピング拘束

キャッピング拘束は、キャッピングのパラメータを定義する決定オプションのコンポーネントです。 キャッピングとは、個々のプロファイルおよびすべてのプロファイルに対して、オプションを提案できる回数を制限するプロセスです。 プロパティには1以上の整数値を指定する必要があります。 プロパティはプロパティ内にネストされま `xdm:cappingConstraint`す。

- **`xdm:globalCap`**  — グローバルキャップとは、全体でオファーが何回提案されるかを制限するものです。
- **`xdm:profileCap`** -プロファイルキャップとは、あるオファーを何回提案できるかを制限するものです。

パーソナライゼーションオファーの制限の設定または変更は、次のPATCH呼び出しを使用して実行できます。

```json
[
  {
    "op":   "add",
    "path": "/_instance/xdm:cappingConstraint",
    "value": {
      "xdm:globalCap":  1000000,
      "xdm:profileCap": 5
    } 
  }
]' 
```

完全なcURL [構文のインスタンスの更新](#updating-and-patching-instances) 、およびパッチ適用を参照してください。 パラメータ `schemaId` ーは必ず指定する必要があり `https://ns.adobe.com/experience/offer-management/personalized-offer`ます。 フォールバックオファーには制約はありません。

キャッピング値を削除するには、「add」操作を「remove」操作に置き換えます。 キャッピング値は個別に存在し、個別に設定または削除することもできます。

### 適格性の制約

オファーは、決定プロセスで条件付きで選択できます。 パーソナライゼーションオファーが実施要件ルールへの参照を持つ場合、特定のオファーに対して考慮されるプロファイルオブジェクトに対して、ルールの条件がtrueに評価される必要があります。 実施要件ルールは、決定オプションとは独立して作成および管理され、同じルールを複数のパーソナライゼーションオファーから参照できます。

ルールへの参照は、プロパティに埋め込まれま `xdm:selectionConstraint`す。

- **`xdm:eligibilityRule`**  — このプロパティは、実施要件ルールへの参照を保持します。 値は、https://ns.adobe.com/experience/offer-management/eligibility-ruleのインスタ `@id` ンスのスキーマです。

ルールの追加と削除は、次のPATCH操作でも実行できます。

```
[
  {
    "op":   "replace",
    "path": "/_instance/xdm:selectionConstraint/xdm:eligibilityRule",
    "value": "xcore:eligibility-rule:f84c6b33cc63c65" 
  }
]'
```

完全なcURL [構文のインスタンスの更新](#updating-and-patching-instances) 、およびパッチ適用を参照してください。 パラメータ `schemaId` ーは必ず指定する必要があり `https://ns.adobe.com/experience/offer-management/personalized-offer`ます。 フォールバックオファーには制約はありません。

この実施要件ルールは、カレンダーの制約と共にプ `xdm:selectionConstraint` ロパティに埋め込まれます。 PATCH操作では、プロパティ全体の削除を試みないでく `SelectionConstraint` ださい。

## 優先度の設定オファー

適切な判断オプションがランク付けされ、特定のオプションに最適なプロファイルが決まります。 ランキングを支援し、他のメカニズムでランキングが決定できない場合にはデフォルトを提供し、各パーソナライゼーションオファーに対してベース優先度を設定する。
基本の優先度は、プロパティに埋め込まれま `xdm:rank`す。

- **`xdm:priority`**  — このプロパティは、プロファイル固有のランキング順序が不明な場合に、あるオファーが別の順序の上で選択されるデフォルトの順序を表します。 優先度の値を比較した後で、2つ以上のパーソナライゼーションオファーが結び付けられたままの場合、1つはランダムに選択され、オファーの提案で使用されます。 このプロパティの値は、0以上の整数である必要があります。

基本優先度の調整は、次のPATCH呼び出しを使用して行うことができます。

```shell
curl -X PATCH {ENDPOINT_PATH}/{CONTAINER_ID}/instances/{INSTANCE_ID} \
  -H 'Content-Type: application/vnd.adobe.platform.xcore.patch.hal+json; schema="{SCHEMA_ID}"' \ 
  -H 'Accept: application/vnd.adobe.platform.xcore.xdm.receipt+json \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  _H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'x-request-id: {NEW_UUID}' \
  -d '[
  {
    "op":   "replace",
    "path": "/_instance/xdm:rank/xdm:priority",
    "value": 0 
  }
]'
```

完全なcURL [構文のインスタンスの更新](#updating-and-patching-instances) 、およびパッチ適用を参照してください。 パラメータ `schemaId` ーは必ず指定する必要があり `https://ns.adobe.com/experience/offer-management/personalized-offer`ます。 フォールバックオファーには、ランク付けプロパティはありません。

## 決定ルールの管理

実施要件ルールは、特定の決定オプションが特定の決定に適格かどうかを判断するために評価される条件を保持します。プロファイル。 ルールを1つ以上の決定オプションにアタッチすると、このオプションでは、このユーザーがこのオプションを考慮するためにルールがtrueに評価される必要があると暗黙的に定義されます。 ルールには、プロファイル属性に関するテストを含めたり、このプロファイルのエクスペリエンスイベントを含む式を評価したり、デシジョンリクエストに渡されたコンテキストデータを含めたりできます。 例えば、次のような条件を記述できます。

> 「エリートの資格を持ち、現在の便の便番号を持つ6か月間に3回飛行した人を含めます。」

インスタンスは、スキーマIDhttps://ns.adobe.com/experience/offer-management/eligibility-ruleを使用して作成されます。 createまた `_instance` はupdateの呼び出しのプロパティは次のようになります。

```json
{
  "xdm:name": "Eligible for a free flight upgrade",
  "xdm:condition": {
    "xdm:value": 
      "membership.status = \"elite\" 
       and (select e from xEvent 
            where e.type = \"flight\" 
              and e.flightnumber = @{{SCHEMA_ID}}.flightnumber
              and (e.timestamp occurs <= 6 months before now).count() > 3
           )",
    "xdm:format": "pql/text",
    "xdm:type": "PQL"
  }
}
```

完全なcURL [構文のインスタンスの更新](#updating-and-patching-instances) 、およびパッチ適用を参照してください。 パラメータ `schemaId` ーは必ず指定する必要があり `https://ns.adobe.com/experience/offer-management/eligibility-rule`ます。

ルールの条件プロパティの値には、PQL式が含まれます。 コンテキストデータは、特殊パス式@{schemaID}を介して参照されます。

ルールは、Experience Platformのセグメントと自然に一致し、多くの場合、プロファイルのプロパティをテストすることで、セグメントの意図を再利用するだけ `segmentMembership` です。 このプ `segmentMembership` ロパティには、既に評価されたセグメント条件の結果が含まれます。 これにより、組織はドメイン固有のオーディエンスを1回定義し、名前を付けて、条件を1回評価できます。

## オファーコレクションの管理

### タグの作成とタグオファー

オファーは、各コレクションで適用されるフィルター条件を定義するコレクション内に整理できます。 現在、コレクション内の式をフィルターするには、次の2つの形式のいずれかを使用できます。

1. コレク `@id` ション内にオファーを含めるには、オファーのリストが識別子のパラメータと一致する必要があります。 このフィルターは、コレクション内の定義済みリストのURIのオファーに過ぎません。
2. オファーはタグ参照のリストを持つことができ、コレクションのフィルターはタグのリストで構成されます。 オファーは、次の場合にコレクションに含まれます。\
   a.フィルターのタグのいずれかが、オファーのタグの1つと一致する\
   b.すべてのフィルターのタグが、オファーのタグの1つと一致する

タグは、オファーインスタンスをリンクできる単純なインスタンスです。 これらは、表示する名前を持つ独自のインスタンスです。 名前をインスタンス間で一意にする必要があります。これにより、ユーザーインターフェイスで表示しやすくなります。

タグオブジェクトは、決定オプション(オファー)間の分類を確立します。 タグは多くのオファーによってリンクでき、オファーは多くのタグ参照を持つことができます。 オファーのカテゴリは、特定のタグインスタンスのセットに関連するすべてのオファーを参照することで確立されます。

タグインスタンスは、スキーマIDhttps://ns.adobe.com/experience/offer-management/tagを使用して作成されます。 createまた `_instance` はupdateの呼び出しのプロパティは次のようになります。

```json
{
  "xdm:name": "credit card"
} 
```

完全なcURL [構文のインスタンスの更新](#updating-and-patching-instances) 、およびパッチ適用を参照してください。 パラメータ `schemaId` ーは必ず指定する必要があり `https://ns.adobe.com/experience/offer-management/tag`ます。


オファーインスタンスは、次のようなタグ参照のリストを使用して作成できます。

```json
{
  "xdm:name": "ABC Bank Credit Card",
  "xdm:tags": [
    "xcore:tag:f66f67dbe6d6ee1",
    "xcore:tag:f2df943c428b6f7"
  ]
}
```

または、オファーにパッチを適用してタグのリストを変更できます。

```json
[
  {
    "op":    "add",
    "path":  "/_instance/xdm:tags/-",
    "value": "xcore:tag:f66f677ad3c0ba7" 
  }
]' 
```

どちらの場合も、完全なcURL構文のイ [ンスタンスの更新](#updating-and-patching-instances) とパッチの適用を参照してください。 パラメータ `schemaId` ーは必ず指定する必要があり `https://ns.adobe.com/experience/offer-management/personalized-offer`ます。

追加操作を正常に `xdm:tags` 実行するには、プロパティが既に存在している必要があります。 PATCH操作では、最初に配列プロパティを追加し、次にその配列にタグ参照を追加することができるインスタンスにタグが存在しません。

### フィルターコレクションのオファーの定義

フィルターインスタンスは、スキーマIDhttps://ns.adobe.com/experience/offer-management/offer-filterを使用して作成されます。 createまた `_instance` はupdateの呼び出しのプロパティは次のようになります。

```json
{
  "xdm:name": "All Upgrade offers",
  "xdm:filterType": "allTags",
  "ids": [
    "xcore:tag:f66f67dbe6d6ee1",
    "xcore:tag:f66f677ad3c0ba7"
  ]
}
```

完全なcURL [構文のインスタンスの更新](#updating-and-patching-instances) 、およびパッチ適用を参照してください。 パラメータ `schemaId` ーは必ず指定する必要があり `https://ns.adobe.com/experience/offer-management/offer-filter`ます。

- **`xdm:filterType`**  — このプロパティは、フィルターがタグを使用して設定されるか、IDによってオファーを直接参照するかを示します。 フィルターがタグを使用するように設定されている場合、フィルタータイプは、すべてのタグが特定のオファーのタグと一致する必要があるか、またはオファーがフィルターに適合するのに十分なタグがあるかをさらに示すことができます。 この列挙プロパティの有効な値は次のとおりです。
   - `offers`
   - `anyTags`
   - `allTags`
- **`ids`**  — プロパティには、の値に応じて、オファーインスタンスまたはタグインスタンスへの参照であるURIの配列が含まれま `xdm:filterType`す。.

次の呼び出しは、オファーが直接参 `_instance` 照されている場合に、createまたはupdate呼び出しのプロパティがどのように表示されるかを示しています。

```json
{
  "xdm:name": "All Upgrade offers",
  "xdm:filterType": "offers",
  "ids": [
    "xcore:personalized-offer:f85e8298e53398d",
    "xcore:personalized-offer:f83a85c2bca3f1f",
    "xcore:personalized-offer:f9195bd412180b1",
    "xcore:personalized-offer:f67be37b6ad6ee6",
    "xcore:personalized-offer:f87bcc710ba6e93",
    "xcore:personalized-offer:f8855c30c0b20f8",
    "xcore:personalized-offer:f67bca7032d6ee5",
    "xcore:personalized-offer:f67bab756ed6ee4",
    "xcore:personalized-offer:f65281d506d6edf"
  ] 
} 
```

## アクティビティ

オファーアクティビティは、判定プロセスの制御に使用されます。 トピック/カテゴリ別にオファーを絞り込むために、在庫合計に適用するオファーフィルタを指定します。また、予約領域に収まるオファーに在庫を絞り込むための配置を指定し、組み合わせ制約で使用可能なすべてのパーソナライゼーションオプション(オファー)が無効になる場合の代替オプションを指定します。

アクティビティインスタンスは、スキーマIDを使用して作成されま`https://ns.adobe.com/experience/offer-management/offer-activity`す。 createまた `_instance` はupdateの呼び出しのプロパティは次のようになります。

```json
{
  "xdm:name": "Call center IVR Personalization",
  "xdm:startDate": "2019-03-01T05:59:59.999Z",
  "xdm:endDate":   "2019-12-27T00:00:00.000Z",
  "xdm:status":    "live",
  "xdm:placement": "xcore:offer-placement:f652486959731ff",
  "xdm:filter":    "xcore:offer-filter:f6998eb62ed6f15",
  "xdm:fallback":  "xcore:fallback-offer:f6529b31b3c0ba6"
}
```

完全なcURL [構文のインスタンスの更新](#updating-and-patching-instances) 、およびパッチ適用を参照してください。 パラメータ `schemaId` ーは必ず指定する必要があり `https://ns.adobe.com/experience/offer-management/offer-activity`ます。

- **`xdm:name`**  — この必須プロパティにはアクティビティ名が含まれます。 この名前は、様々なユーザーインターフェイスで表示されます。
- **`xdm:status`**  — このプロパティは、インスタンスのライフサイクル管理に使用されます。 この値は、アクティビティがまだ構築中（値=ドラフト）か、通常はランタイム（値=ライブ）か、それ以上使用しない（値=アーカイブ済み）かを示すために使用されるワークフロー状態を表します。
- **`xdm:placement`** -オファーのコンテキストが決定されたときに在庫に適用される、デシジョニング配置への参照を含む必須プロパティ。アクティビティ 値は、使用される`@id`オファー配置のURIです。
- **`xdm:filter`**  — このオファーのコンテキストで決定が行われたときに在庫に適用されるアクティビティフィルターへの参照を含む必須プロパティ。 値は、使用されるオファー`@id`フィルターのURI()です。
- **`xdm:fallback`**  — フォールバックオファーへの参照を含む必須プロパティ。 フォールバックオファーは、このアクティビティの判定がパーソナライゼーションオファーを満たさない場合に使用されます。 値は、フォールバックオファー`@id`インスタンスのURI()です。

### フォールバックオファー

アクティビティインスタンスを作成する前に、オファーの配置に適したフォールバックインスタンスが存在する必要があります。 フォールバックオファーインスタンスは、スキーマ識別子を使用して作成されま`https://ns.adobe.com/experience/offer-management/fallback-offer`す。 createまた `_instance` はupdateの呼び出しのプロパティには、パーソナライゼーションオファーと同じ一般プロパティが含まれますが、他の制約を含めることはできません。

```json
{
  "xdm:name": "Default for Kiosk Placements",
  "xdm:status": "approved",
  "xdm:representations": [
    {
      "xdm:placement": "xcore:offer-placement:e91afcf81bad130"
      "xdm:components": [
        {
          "dc:language": ["en" ],
          "@type": "https://ns.adobe.com/experience/offer-management/content-component-html",
          "dc:format": "text/html"
        }
      ]
    }
  ]
}  
```

完全なcURL [構文のインスタンスの更新](#updating-and-patching-instances) 、およびパッチ適用を参照してください。 パラメータ `schemaId` ーは必ず指定する必要があり `https://ns.adobe.com/experience/offer-management/fallback-offer`ます。

