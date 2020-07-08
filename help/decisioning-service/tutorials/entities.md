---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: APIを使用したDecisioningサービスエンティティの管理
topic: tutorial
translation-type: tm+mt
source-git-commit: bd9884a24c5301121f30090946ab24d9c394db1b
workflow-type: tm+mt
source-wordcount: '7207'
ht-degree: 0%

---


# APIを使用したDecisioningのオブジェクトとルールの管理

このドキュメントでは、Adobe Experience PlatformAPIを使用するビジネスエンティティを操作するためのチュートリアルを提供 [!DNL Decisioning Service] します。

このチュートリアルは2つの部分で構成されています。

- ビジネスオブジェクトを管理する汎用リポジトリAPIは、 [最初の部分で導入されます](#managing-repository-entities-using-apis)。 これらのAPIは汎用的な意味で、 **任意の種類のビジネスオブジェクトに対して作成、読み取り、更新、削除および検索機能を提供します** 。 一般的なAPIモデルについて説明し、ハイパーテキストアプリケーション言語(HAL)との関係を説明します。

- 第 [2部では、リポジトリAPIに関する知識を適用し、リポジトリAPIを介して管理されるビジネス・エンティティに焦点を当てます](#creating-and-managing-offer-decisioning-entities-using-apis) 。 同じAPIを適用した場合、アクティビティとビジネスルールなど2つの異なるエンティティの管理で唯一の違いは、リクエストと応答のペイロード、および管理対象のオブジェクトのタイプを示す必要なヘッダー値です。

## はじめに

このチュートリアルでは、サー [!DNL Experience Platform] ビスとAPIの規則に関する十分な理解が必要です。 リポジトリは、他の複数のサー [!DNL Platform][!DNL Platform] ビスがビジネスオブジェクトや様々なタイプのメタデータを保存するために使用するサービスです。 複数のランタイムサービスで使用するために、これらのオブジェクトを管理およびクエリするための安全で柔軟な方法を提供します。 そ [!DNL Decisioning Service] の一つ。 このチュートリアルを開始する前に、次のドキュメントを確認してください。

- [!DNL Experience Data Model (XDM)](../../xdm/home.md): Platformが顧客体験データを編成する際に使用する標準化されたフレームワーク。
- [!DNL Decisioning Service](./../home.md): Experience Decisioningの一般的な方法と、特にオファー判定に使用される概念とコンポーネントについて説明します。 お客様の体験の中で提示する最適なオプションを選択するために使用する戦略を説明します。
- [!DNL Profile Query Language (PQL)](../../segmentation/pql/overview.md): PQLはXDMインスタンスを使って式を書く強力な言語です。 PQLは、決定ルールを定義するために使用されます。

以下の節では、APIを正しく呼び出すために知る必要がある追加情報について説明し [!DNL Platform] ます。

### サンプルAPI呼び出しの読み取り

このチュートリアルでは、リクエストをフォーマットする方法を示すAPI呼び出しの例を提供します。 例えば、パス、必須のヘッダー、適切にフォーマットされた要求ペイロードなどです。 API応答で返されるサンプルJSONも提供されます。 サンプルAPI呼び出しのドキュメントで使用される規則について詳しくは、トラブルシューティングガイドのAPI呼び出し例 [を読む方法に関する節](../../landing/troubleshooting.md#how-do-i-format-an-api-request) を参照して [!DNL Experience Platform] ください。

### 必要なヘッダーの値の収集

APIを呼び出すには、まず [!DNL Platform] 認証チュートリアルを完了する必要があり [ます](../../tutorials/authentication.md)。 次に示すように、認証チュートリアルで、すべての [!DNL Experience Platform] API呼び出しに必要な各ヘッダーの値を指定する

- 認証： 無記名 `{ACCESS_TOKEN}`
- x-api-key: `{API_KEY}`
- x-gw-ims-org-id: `{IMS_ORG}`

内のすべてのリソース [!DNL Experience Platform] は、特定の仮想サンドボックスに分離されます。 APIへのすべてのリクエストには、操作が実行されるサンドボックスの名前を指定するヘッダーが必要で [!DNL Platform] す。

- x-sandbox-name: `{SANDBOX_NAME}`

>[!NOTE]
>
>のサンドボックスについて詳し [!DNL Platform]くは、 [Sandboxの概要ドキュメントを参照してください](../../sandboxes/home.md)。

ペイロード(POST、PUT、PATCH)を含むすべてのリクエストには、次の追加のヘッダーが必要です。

- Content-Type: application/json

## リポジトリAPIの規則

[!DNL Decisioning Service] は、互いに関連する多数のビジネスオブジェクトによって制御されます。 すべてのビジネス・オブジェクトは、 [!DNL Platform’s] ビジネス・オブジェクト・リポジトリに格納されます。 このリポジトリの主な特徴は、APIがビジネスオブジェクトのタイプと直交していることです。 APIエンドポイント内のリソースの種類を示すPOST、GET、PUT、PATCH、またはDELETEAPIを使用する代わりに、6つの汎用エンドポイントしか存在しませんが、その曖昧性を解除する必要がある場合に、オブジェクトの種類を示すパラメータを受け取るか返します。 スキーマはリポジトリに登録する必要がありますが、その後、リポジトリはオープンエンドなオブジェクト型のセットに対して使用できます。

上記のヘッダーに加え、リポジトリオブジェクトを作成、読み取り、更新、削除およびクエリするためのAPIには、次の規則があります。

- とのすべてのリポジトリAPI開始のエンドポイントパス `https://platform.adobe.io/data/core/xcore/`です。

APIペイロード形式は、 `Accept` または `Content-Type` ヘッダーとネゴシエートされます。 また `Accept` は `Content-Type``application/vnd.adobe.platform.xcore.{FORMAT}+json` ヘッダー内のメッセージ形式は値で示されます。{FORMAT}は、次の表に従って、特定のリポジトリAPIリクエストまたは応答メッセージに依存します。

| FORMATバリアント | 要求または応答エンティティの説明 |
| --- | --- |
| ハ<br>ルフォローしたパラメータ `schema={schemaId}` | メッセージには、JSONスキーマーで記述されたインスタンスが含まれており、このインスタンスはformatスキーマーで示されます。 インスタンスがJSONプロパティに含まれ `_instance`ます。 応答ペイロードの他の最上位プロパティは、すべてのリソースで使用できるリポジトリ情報を指定します。  HAL形式に準拠するメッセージには、HAL形式の参照を含む `_links` プロパティがあります。 |
| `patch.hal` | メッセージにはJSON PATCHペイロードが含まれ、パッチ対象のインスタンスがHALに準拠していることを前提としています。 つまり、インスタンス独自のインスタンスプロパティだけでなく、インスタンスのHALリンクにもパッチを適用できます。 クライアントが更新できるプロパティには制限があります。 |
| `home.hal` | メッセージには、リポジトリのホームドキュメントリソースのJSON形式の表現が含まれます。 |
| xdm.receipt | メッセージには、作成、更新（完全およびパッチ）または削除操作に対するJSON形式の応答が含まれます。 入金には、ETagの形式でインスタンスのリビジョンを示す制御データが含まれます。 |

各 **形式のバリアントの使用方法は** 、特定のAPIによって異なります。

| API | Content-Type header | ヘッダーを受け入れる |
| --- | --- | --- |
| インスタンスを作成 <br/>コンテナ | `hal`<br/>スキーマパラメーター付き | `xdm.receipt` |
| Update<br/>InstanceUpdateコンテナ | `hal`<br/>スキーマパラメーター付き | `xdm.receipt` |
| パッチインスタンス | `patch.hal` | `xdm.receipt` |
| Delete<br/>instanceDeleteコンテナ | 該当なし | `xdm.receipt` |
| Read<br/>InstanceReadコンテナ | 該当なし | `hal` パラメー `schema` ターと共に |
| リスト<br/>instancesListコンテナ | 該当なし | `hal` 特別な `schema` パラメーターで `https://ns.adobe.com/experience/xcore/hal/results` |
| 検索インスタンス | 該当なし | 特別な `schema` パラメータを持つハル `https://ns.adobe.com/experience/xcore/hal/results` |
| レポルートを読み取る | 該当なし | `home.hal` |

コンテナのcreate、updateおよびread APIの場合、formatパラメーターのスキーマに値が含まれ `https://ns.adobe.com/experience/xcore/container`ます。

`ContainerId` は、インスタンスAPIの最初のパスパラメーターです。 すべてのビジネスエンティティは、いわゆるコンテナに存在します。 コンテナとは、異なる懸念を切り離すための分離メカニズムです。 一般的なエンドポイントに続くリポジトリインスタンスAPIの最初のパス要素が `containerId`です。 識別子は、呼び出し元がアクセス可能なコンテナのリストから取得される。 例えば、コンテナでインスタンスを作成するAPIは `POST https://platform.adobe.io/data/core/xcore/{containerId}/instances`です。

アクセス可能なコンテナのリストは、標準ヘッダーを使用してHTTP GETリクエストでリポジトリルートエンドポイント「/」を呼び出すことで取得されます。

## コンテナへのアクセスの管理

管理者は、類似したプリンシパル、リソースおよびアクセス権限をプロファイルにグループ化できます。 これにより、管理上の負担が軽減され、 [アドビのAdmin ConsoleUIでサポートされ](https://adminconsole.adobe.com)ます。 プロファイルを作成し、ユーザーを割り当てるには、組織のAdobe Experience Platformの製品管理者である必要があります。

1回限りの手順で特定の権限に一致する製品プロファイルを作成し、それらのプロファイルにユーザーを追加するだけで十分です。 プロファイルは、権限が付与されたグループとして機能し、そのグループ内のすべての実際のユーザーまたは技術ユーザーは、その権限を継承します。

### ユーザーと統合にアクセスできるリストコンテナ

通常のユーザーまたは統合に対するコンテナへのアクセス権を管理者が付与すると、それらのコンテナはリポジトリの「ホーム」リストに表示されます。 リストは、呼び出し元がアクセスできるすべてのコンテナのサブセットなので、ユーザーや統合によって異なる場合があります。 コンテナのリストは、製品コンテキストとの関連付けによってフィルタリングできます。 フィルターパラメーターが呼び出され `product` 、繰り返し可能です。 複数の製品コンテキストフィルターを指定した場合、特定のコンテナコンテキストーのいずれかと関連付けられた製品の和集合が返されます。 1つのコンテナを複数の製品コンテキストに関連付けることができます。

現在、 [!DNL Platform] コンテナのコンテキスト [!DNL Decisioning Service]`dma_offers`です。

>[!NOTE]
>
>のコンテキスト [!DNL Platform Decisioning Containers] は、まもなくに変更され `acp`ます。 フィルタリングはオプションですが、フィルターのみによる編集は、今後のリリースで必要 `dma_offers` になる予定です。 この変更に備えるために、クライアントはフィルターを使用しないでください。また、両方の製品コンテキストをフィルタとして適用してください。

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

結果項目に `instanceId` 一覧が表示されていることを確認します。 これは、通常のビジネスオブジェクトを読み取って操作するためのAPIの `containerId` パラメーターとして使用されます。

リストは、アクセス権限に応じて既にユーザー用にフィルタリングされていますが、プロパティクエリによってフィルタリングすることもできます。

## エンティティを管理する汎用API

### インスタンスの作成

リポジトリで新しいインスタンスを作成するAPIは、 `containerId` パスパラメーターを使用し、スキーマパラメーターを使用して、 `Content-Type` ヘッダー内のインスタンスのタイプを識別します。

インスタンスプロパティは、プロパティでラップされたペイロードで提供され `_instance` ます。 インスタンスプロパティは、指定したスキーマIDを持つJSONスキーマに対して有効である必要があります。

HAL `_links` プロパティは必ず存在する必要がありますが、空にすることができます。 これは、このインスタンスに対してカスタムリンクが定義されていないことを意味します。

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

応答には、先ほど作成したオブジェクトのinstanceIdが含まれます。 このinstanceIdは不変であり、常にリポジトリによって割り当てられ、グローバルに一意です。 この値は、物理的な識別子として機能します。

また、スキーマがそのようなプロパティを持つ場合、応答ペイロードの `@id` プロパティにユニバーサルリソース識別子(URI)が返される。 各インスタンスには、そのインスタンスのURIと主キーとして機能するプロパティが必要です。 このIDは、他のインスタンスで使用され、異なるタイプを含む新しいインスタンスとの関係を形成します。 現在のリリースでは、URIはリポジトリによって生成され、 `@id` プロパティに格納されます。 将来のバージョンでは、このルールが緩和され、クライアントが独自のURI値を管理し、それを含むプロパティに名前を付けることが可能になる可能性があります。

これらのURIはURLではなく、直接リソースを取得する手段を提供しないことに注意してください。 その縦横比を示すために、取得プロトコルを指定しないURIスキームのプレフィックスがURIに付けられます。 ただし、URIを使用して、クエリを使用してインスタンスを参照できます。

REST応答には、Locationヘッダーが含まれ、そのヘッダーにはURLコンポーネントが含まれます。このコンポーネントは、作成されたばかりのインスタンスを取得するのに使用できます。 このコンポーネントは相対URI参照であり、リポジトリのベースURIに適用する必要があります。 ベースURIが `Content-Base` ヘッダーに返されます。

この `repo:etag` プロパティは、インスタンスのリビジョンを指定します。 この値は、整合性を維持するために更新操作で使用できます。 HTTPヘッダー `If-Match` を使用して、誤って上書きされる可能性のある他のインスタンスに変更がないように、PUTまたはPATCH API呼び出しに条件を追加できます。 この `repo:etag` 値は、作成、読み取り、更新、削除およびクエリ呼び出しのたびに返されます。 この値は、 ` If-Match` RFC7232 Section 3.1に従って、 [ヘッダーの値として使用されます](https://tools.ietf.org/html/rfc7232#section-3.1)。

残りのプロパティは、インスタンスの作成と最後の変更に使用されたアカウントとAPIキーを示します。 この呼び出しによってインスタンスが作成されたので、それぞれの値はリクエストの値になります。

### IDでインスタンスを参照

Create呼び出しで返されるLocationヘッダー内のURLを使用して、アプリケーションはインスタンスを検索できます。

**リクエスト**

```shell
curl -X GET {ENDPOINT_PATH}/{CONTAINER_ID}/instances/{INSTANCE_ID} \ 
  -H 'Accept: *, application/vnd.adobe.platform.xcore.hal+json; schema="{SCHEMA_ID}" \ 
  -H 'x-api-key: {API_KEY}' \ 
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  _H 'x-sandbox-name: {SANDBOX_NAME}' \ 
  -H 'x-request-id: {NEW_UUID}'  
```

>[!NOTE]
>
>パスパラメーター `instanceId` として指定されているものの、アプリケーションでは、可能な限り、パス自体を構築せず、リストおよび検索操作に含まれるインスタンスへのリンクに従う必要があります。 詳しくは、‎6.4.4‎と6.4.6の節を参照してください。

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

インスタンスのJSONプロパティはプロパティに含まれ、他のルートレベルプロパティはインスタンスに関するメタデータを保持し `_instance` ます。

リソースには、JSONスキーマIDの配列も含まれます。 この配列は、このインスタンスが検証されるJSONスキーマを示します。

各インスタンスには、IANAが登録した自己関係( [RFC5988で定義される])に対応する関係タイプselfのHALリンクが含まれます。

**インスタンスの新しいリビジョンのテスト**

インスタンスの現在の `eTag` 値が応答と共に返され、クライアントは、同じリソース状態の再取得を避けるか、クライアントの知識なしに後のリビジョンの値を上書きしないように、インスタンスに対して条件演算を実行できます。

ルックアップAPIを使用すると、クライアントは `If-None-Match` ヘッダパラメータを指定できます。 この標準HTTPパラメータ [RFC2616の定義を参照してください]。 クライアントが指定するエンティティタグの値は、最新の応答(update、read、リスト、またはsearch API呼び出しから取得された値)です。 この `etag` 値はクライアントに対して不透明で、引用符で囲んだ文字列で指定する必要があります。

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

リポジトリAPIは、インスタンスの最後のリビジョンが指定したetagと一致する場合、ステータス304「未変更」で応答します。

### スキーマのリストインスタンス — 並べ替えとページング

クライアントは、作成中のインスタンスを追跡できないので、物理instanceIdを使用してクライアントにアクセスできます。 Read instance APIの使用は例外です。 クライアントは、他のクライアントが作成したインスタンスにも気づきません。

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

応答は、 `{schemaId}` 指定した値に依存します。 例えば、「https<span></span>://ns.adobe.com/experience/オファー管理/オファーアクティビティ&amp;id=xcore:オファーアクティビティ:fa24f9e8fc15c73」の場合、応答は次のようになります。

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

>[!NOTE]
>
>結果には、特定のリストのインスタンスまたはこのスキーマの最初のページが含まれます。 インスタンスは複数のリストに準拠できるので、複数のスキーマに表示できます。

ページリソースは一時的なもので、読み取り専用です。 更新や削除はできません。 ページングモデルは、クライアントごとの状態を維持することなく、長期にわたって大きなリストのサブセットにランダムアクセスを提供します。

この方法でインスタンスリストにページ別にアクセスするには、そのインスタンスリストで列挙されるエントリに対して安定した並べ替えを定義できる必要があります。 「安定」とは、インスタンスが事前に定義されたページに表示されることを意味するものではありません。 実際、インスタンスをプロパティPに従ってソートしてページ順が形成され、クライアントがこのプロパティPを更新すると、別のクライアントがリストをページングしながら、このインスタンスに再び到達する場合があります。 つまり、モデルは、より最新の結果を返す方を優先します。

ただし、ソート順序が変更不可能なプロパティに基づいている場合、「安定した」ソート順は、ページング操作の最初に存在したすべてのインスタンスに到達することを保証します（ページに到達した時点までに削除された場合を除く）。 この並べ替え順が単調に増加するプロパティに対して設定されている場合、ページング操作の開始後に作成されるインスタンスにも到達します。

クライアントは、必要なページサイズに関するヒントを提供できますが、返されるページに対して多数または少数のインスタンスを提供するのはリポジトリのみです。 安定した順序を保証するために、sortプロパティの値がページの境界で異なるように、サービスはページにアイテムを追加または削除する必要があります。 このようにして、次のページに最後のページの一部の項目が再び含まれなくなったり、最悪のシナリオでは各ページの同じ項目が表示されなくなります。

ページングは、以下のパラメーターで制御します。

- **`orderBy`**: インスタンスリストの並べ替えに使用するプロパティのカンマ区切りの番号付きリストが含まれます。 1つ目のプロパティは1つ目の並べ替えに使用され、2つ目のプロパティは1つ目の並べ替えに使用され、2つ目のプロパティは1つ目の並べ替えに使用されます。 インスタンスごとに一意の値を持つプロパティを指定した場合、結び付けはできず、各項目の後に改ページが発生する可能性があります。 プロパティの名前の前には、昇順を示す「a」 `+` またはそのプロパティの降順を示す「 `-` 」を付けることができます。 プロパティ名のプリフィックスが付いていない場合、結果は昇順で並べ替えられます。 リクエストで指定さ `orderBy` れていない場合、リポジトリは代わりに物理instanceIdプロパティを使用します。
- **`start`**: クライアントは、開始パラメーターを使用して、取得するページを定義します。 開始パラメーターによって、目的のページの開始が決まります。 応答には、指定した値より厳密に大きい（昇順の場合）または厳密に小さい（降順の場合） `orderBy` プロパティ値を持つインスタンスで始まるインスタンスが含まれます。 クエリパラメーターを指定しない場合、最初に指定可能なインスタンス識別子の前に並べ替えるinstanceId値がデフォルトになるので、この値は最初のページから省略されます。
- **`limit`**: 特定のリクエストに対して返す必要のある最大アイテム数に関するヒントとして、正の整数を指定します。 開始パラメータの信頼性の高い動作を提供する必要があるため、実際の応答サイズは小さいか大きくなる場合があります。

### リストのフィルタ

フィルタリングリストの結果は可能で、ページングメカニズムとは無関係に発生します。 フィルターは、リストの順序でインスタンスをスキップするか、特定の条件を満たすインスタンスのみを明示的に含めるように求めます。 クライアントは、プロパティ式をフィルターとして使用するように要求したり、インスタンスの主キーの値として使用するURIのリストを指定したりできます。

- **`property`**: プロパティ名のパスの後に、比較演算子の後に値が続く値が含まれます。 <br/>
返されるインスタンスのリストには、式がtrueと評価されるインスタンスが含まれます。 例えば、インスタンスにペイロードプロパティがあり、可能な値は `status` 、 `draft`と仮定して、 `approved`クエリパラメーターは、ステータスが承認されたインスタンスのみを `archived``deleted``property=_instance.status==approved` 返します。 <br/>
<br/>
指定した値と比較されるプロパティはパスとして識別されます。 個々のパスコンポーネントは、次のように`.`で区切られます。 `instance.xdm:prop1.xdm:prop1_1.xdm:prop1_1`<br/>

文字列、数値、日付/時間の値を持つプロパティの場合、次の演算子を使用できます。 `==`、 `!=`、 `<`、 `<=`、 `>` および `>=`を追加します。 また、文字列値を持つプロパティの場合は、演算子を使用 `~` できます。 演算子は、正規式に従って、指定したプロパティと一致します。 `~` このプロパティの文字列値は、フィルターされた結果にエンティティが含まれるために **式全体** と一致する必要があります。 例えば、プロパティ値内の任意の `cars` 場所で文字列を探す場合は、正規式がである必要があり `.*cars.*`ます。 先頭または末尾がない場合 `.*`、エンティティのみが、それぞれ最初または最後がで始まるプロパティ値を持つエンティティ `cars`と一致します。 演算子の場合、文字の比較では大文字と小文字が区別されません。 `~` 他のすべての演算子では、比較では大文字と小文字が区別されます。<br/><br/>
インスタンスペイロードプロパティだけでなく、フィルター式でも使用できます。 エンベローププロパティは、同じ方法で比較されます。例： `property=repo:lastModifiedDate>=2019-02-23T16:30:00.000Z`. <br/>
<br/>
クエリパラメーターを繰り返して複数のフィルター条件を適用できます。例えば、特定の日付の後、特定の日付の前に最後に変更されたすべてのインスタンスを返します。 `property` これらの式の値は、URLエンコードする必要があります。 式を指定せず、プロパティ名がリストに表示される場合は、指定した名前のプロパティを持つ項目が該当します。<br/>
<br/>

- **`id`**: 場合によっては、リストをインスタンスのURIでフィルタリングする必要があります。 この `property` クエリパラメーターを使用して1つのインスタンスを除外できますが、複数のインスタンスを取得する場合は、URIのリストをリクエストに渡すことができます。 この `id` パラメーターが繰り返され、各オカレンスで1つのURI値が指定されます。URI値 `id={URI_1}&id={URI_2},…` はURLエンコードする必要があります。

ページの結果は、特別なMIME型として返され `application/vnd.adobe.platform.xcore.hal+json; schema="https://ns.adobe.com/experience/xcore/hal/results"`ます。

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

応答には、JSONプロパティの結果の内部に結果項目のリストが含まれ、このページの結果数と、返されたばかりのページから始まるフィルター適用済みリスト内の項目の合計数を示す2つのプロパティの横に表示されます。

### フルテキスト検索と構造化クエリ

文字列プロパティに含まれる用語により複雑なフィルター条件や検索インスタンスを提供したい場合、リポジトリオファーはより強力な検索APIを提供します。

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

リストAPIのページングパラメーターとフィルターパラメーターに加えて、このAPIを使用すると、クライアントはフルテキストパラメーターとブールクエリパラメーターを追加できます。

フルテキスト検索は、以下のパラメーターで制御します。

- **`q`**: インスタンスの文字列プロパティと照合される前に正規化される用語の、スペースで区切られた、順不同のリストが含まれます。 文字列プロパティの用語が分析され、それらの用語も標準化されます。 検索クエリは、パラメーターで指定された1つ以上の用語と一致しようとし `q` ます。 +、-、=、&amp;&amp;の各文字。 ||、>、&lt;、!、(、)、{、}、 [、]、^、&quot;、～、*、?、:、/は、クエリ文字列内の単語の境界を決定する特殊な意味を持ち、その文字と一致するトークンにエスケープする場合はバックスラッシュでエスケープする必要があります。 クエリ文字列は、重複引用符で囲んで、完全に一致する文字列にしたり、特殊文字をエスケープしたりできます。
- **`field`**: 検索語句がプロパティのサブセットとのみ一致する必要がある場合、fieldパラメーターはそのプロパティへのパスを示すことができます。 このパラメーターを繰り返して、一致する必要のある複数のプロパティを示すことができます。
- **`qop`**: 検索の一致動作の変更に使用する制御パラメータが含まれます。 パラメーターをに設定した後、すべての検索用語が一致する必要があり、パラメーターがない場合、またはその値がに設定されている場合、またはいずれかの用語が一致としてカウントされます。

### インスタンスの更新とパッチ適用

インスタンスを更新するには、クライアントが一度にプロパティの完全なリストを上書きするか、JSON PATCHリクエストを使用してリストを含む個々のプロパティ値を操作します。

どちらの場合も、要求のURLは物理インスタンスへのパスを指定し、どちらの場合も、 [作成操作から返されたJSON受信ペイロードのような応答が返されます](#create-instances)。 クライアントは、このAPIの完全なURLパスとして、以前にこのオブジェクトに対するAPI呼び出しから受け取った `Location` ヘッダーまたはHALリンクを使用することが望ましい。 これが不可能な場合、クライアントは、とからURLを構築 `containerId` でき `instanceId`ます。

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

PATCHリクエストは、命令を適用し、結果のエンティティをスキーマと比較して検証し、PUTリクエストと同じエンティティおよび参照整合性ルールを検証します。

**プロパティ値の編集の制御**

次の注釈を使用して、作成時や更新時にプロパティが設定されないようにできます。

- **`"meta:usereditable"`**: Boolean — 要求がユーザーエージェントから送信されたもので、ユーザーまたはテクニカルアカウントのアクセストークンによって呼び出し元を特定した場合、注釈が付けられたプロパティはペイロードに含め `"meta:usereditable": false` ないでください。 値が存在する場合、現在設定されている値と異なる値を持つことはできません。 値が異なる場合、更新またはパッチリクエストは、ステータス422「処理不可エンティティ」で拒否されます。
- **`"meta:immutable"`**: ブール型 — 注釈が付けられたプロパティは、一度設定され `"meta:immutable": true` ると変更できません。 これは、エンドユーザー、テクニカルアカウント統合、または特別なサービスからの要求に適用されます。

**同時更新のテスト**

複数のクライアントが同時に1つのインスタンスを更新しようとする場合があります。 リポジトリは、中央のトランザクション管理なしで、計算ノードのクラスタ上で動作します。 あるクライアントが別のクライアントによって同時に書き込まれるインスタンスを書き込むのを防ぐために、クライアントは条件付き更新またはパッチ要求を使用できます。 リポジトリのヘッダーに `etag` 文字列を指定すると、最初のリクエストのみが成功し、同じ `If-Match``etag` 値を使用する他のクライアントによる後続のリクエストは失敗します。 値は、インスタンスを変更するたびに変更されます。 `etag` クライアントは最新の `etag` 値を取得するためにインスタンスを取得する必要があり、多くの試行の中で1つのクライアントのみがその値で更新を成功できます。 他のクライアントは、409 Conflictというメッセージを表示して拒否されます。

### インスタンスの削除

インスタンスは、DELETE呼び出しを使用して削除できます。 クライアントは、これに対する以前のAPI呼び出しから受け取った `Location` ヘッダーまたはHALリンクを、完全なURLパスとして使用することが望ましい。 これが不可能な場合、クライアントは、および物理からURLを構築 `containerId` でき `instanceId`ます。

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

削除リクエストを受け取ると、リポジトリは削除対象のインスタンスを引き続き参照し、他のスキーマのインスタンスをチェックします。 高可用性を備えた分散システムでは、参照整合性を即座にチェックすることはできません。 外部キーの関係が定義されている場合、チェックは非同期で実行されます。 その結果、削除リクエストの結果に対する応答が若干遅れています。 これらのチェックが行われると、即時応答には、ステータス202「受け入れ済み」と、削除操作の結果を確認するリンクが `Location` ヘッダーに含まれます。 次に、クライアントはそのリンクで結果を確認する必要があります。

削除するインスタンスを参照するインスタンスが見つかった場合、削除操作の拒否結果になります。 他の外部キー参照が見つからない場合は、削除が完了します。 結果がまだ決定されていない場合、応答は、同じ `Location` ヘッダーを持つ別の202受け入れ済みの応答を示し、クライアントに引き続き確認するように求めます。 結果が決定されると、応答は200 OKステータスの応答と、応答のペイロードに元の削除リクエストの結果が含まれることを示します。 「200 OK」の応答は結果が既知で、応答本体に削除リクエストの確認または拒否が含まれることを意味します。

## オファーとそのサブコンポーネントの作成

前の節で説明したAPIは、すべてのタイプのビジネスオブジェクトに一様に適用されます。 例えば、オファーとアクティビティを作成する場合の唯一の違いは、スキーマに準拠するリクエストのJSONスキーマとJSONペイロードを示す `content-type` ヘッダーです。 したがって、以降の節では、これらのスキーマとそれらの関係に焦点を当てるだけで済みます。

コンテンツタイプでAPIを使用する場合 `application/vnd.adobe.platform.xcore.hal+json; schema="{SCHEMA_ID}"`、インスタンス独自のプロパティが、プロパティがある横の `_instance` プロパティに埋め込まれ `_links` ます。 これは、すべてのインスタンスが表される一般的な形式です。

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

>[!NOTE]
>
>簡潔にするため、すべてのJSONスニペットでは、インスタンスプロパティのみが示され、必要な場合にのみエンベローププロパティと_linksセクションが示されます。

### 一般的なオファープロパティ

オファーは判定オプションの一種で、オファーのJSONスキーマは、各オプションインスタンスに付与される標準のオプションプロパティを継承します。

- **`@id`**  — 主キーで、他のオブジェクトからのオプションの参照に使用される、各オプションの固有な識別子。 このプロパティは、インスタンスの作成時に割り当てられ、不変で編集できません。
- **`xdm:name`**  — すべてのオプションに、検索や表示の目的で使用される名前が付けられます。 名前は不変ではなく、インスタンスを一意に識別するために使用することはできません。 名前は自由に選択できますが、オファーインスタンス間で一意である必要があります。

```json
{ 
  "@id": "INSTANCE_URI",                         // meta:immutable=true, meta:usereditable=false 
  "xdm:name": "A name for the Decision Option",  // meta:immutable=false 
  "xdm:characteristics": { 
    properties specific to this instance         // property names can vary per instance 
  } 
}
```

cURLの完全な構文については、「インスタンスの [更新とパッチ適用](#updating-and-patching-instances) 」を参照してください。 オファー `schemaId` がフォールバックオファーの場合は、 `https://ns.adobe.com/experience/offer-management/personalized-offer` またはその `https://ns.adobe.com/experience/offer-management/fallback-offer` である必要があります。

各オファーインスタンスには、そのインスタンスのみに特有のプロパティのオプションのセットを含めることができます。 これらのプロパティに対して、オファーごとに異なるキーを持つことができます。値は文字列である必要があります。 これらのプロパティは、決定およびセグメントルールで使用できます。 また、決定したエクスペリエンスをアセンブルして、メッセージをさらにカスタマイズすることもできます。

### オファーライフサイクル

状態トランジションの簡単なフローに従って、すべてのオプションが実行されます。 開始はドラフト状態で終了し、準備が整うと、状態は承認済みに設定されます。 終了日が過ぎると、アーカイブ済みの状態に移動できます。 この状態では、これらを再び製図状態に移動することで、削除または再利用できます。

![](../images/entities/offer-lifecycle.png)

- **`xdm:status`**  — このプロパティは、インスタンスのライフサイクル管理に使用されます。 この値は、オファーがまだ構築中か（値=ドラフト）を示すために使用されるワークフロー状態を表し、一般にランタイムが考慮できる（値=承認済み）か、それ以上使用しない（値=アーカイブ済み）かを示します。

インスタンスに対する単純なPATCH操作は、通常、 `xdm:status` プロパティを操作する目的で使用されます。

```json
[
  {
    "op":    "replace",
    "path":  "/_instance/xdm:status",
    "value": "approved" 
  }
]
```

cURLの完全な構文については、「インスタンスの [更新とパッチ適用](#updating-and-patching-instances) 」を参照してください。 オファー `schemaId` がフォールバックオファーの場合は、 `https://ns.adobe.com/experience/offer-management/personalized-offer` またはその `https://ns.adobe.com/experience/offer-management/fallback-offer` である必要があります。

### 表示と配置

オファーとは、コンテンツを表す意思決定オプションです。 決定が行われると、オプションが選択され、その識別子を使用して、指定する必要がある配置のコンテンツまたはコンテンツ参照が取得されます。 オファーは複数の表現を持つことができますが、それぞれ異なる配置参照を持つ必要があります。 これにより、所定の配置によって、表現を明確に決定することができます。
決定操作中に、配置はアクティビティオブジェクトとの組み合わせで決定されます。 その配置を参照として表現していないオファーは、選択肢のリストから自動的に削除されます。

プロダクト表現をオファーに追加する前に、配置インスタンスが存在する必要があります。 これらのインスタンスはスキーマID`https://ns.adobe.com/experience/offer-management/offer-placement`。

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

cURLの完全な構文については、「インスタンスの [更新とパッチ適用](#updating-and-patching-instances) 」を参照してください。 オファー `schemaId` がフォールバックオファーの場合は、 `https://ns.adobe.com/experience/offer-management/personalized-offer` またはその `https://ns.adobe.com/experience/offer-management/fallback-offer` である必要があります。

Placement **** インスタンスは次のプロパティを持つことができます。

- **`xdm:name`**  — 人間のインタラクションおよびユーザーインターフェイスで参照するために割り当てられた配置の名前が含まれます。
- **`xdm:description`**  — この場所のコンテンツがメッセージ配信全体でどのように使用されているかを、人間が理解できる意図を伝えるために使用します。 配信チャネルは、新しい配置を定義する場合、このプロパティに情報を追加して、コンテンツの作成者がそれに応じてコンテンツを作成または選択できるようにすることができます。 これらの指示は、正式に解釈も強制もされません。 この土地は単に意図を伝える場所としての役割を果たしている。
- **`xdm:channel`** -チャネルのURI。 チャネルは、動的コンテンツの配信先を示します。 チャネル制約は、オファーが使用される場所を伝えるだけでなく、エクスペリエンスに使用されるコンテンツエディターやバリデーターを決定するためにも使用されます。
- **`xdm:componentType`**  — この配置で説明されている場所に表示できるコンテンツのモデル識別子（URIなど）。 コンポーネントのタイプは次のとおりです。 画像リンク、htmlまたはプレーンテキスト。 各コンポーネントタイプは、コンテンツアイテムに含まれる特定のプロパティのセット（モデル）を意味する場合があります。 コンポーネントタイプのリストは拡張できます。 次の3つの定義済みコンポーネントタイプの値があります。
   - `https://ns.adobe.com/experience/offer-management/content-component-imagelink`
   - `https://ns.adobe.com/experience/offer-management/content-component-text`
   - `https://ns.adobe.com/experience/offer-management/content-component-html`
- **`xdm:contentTypes`**、この配置に必要なコンポーネントのメディアタイプに対する制約。 異なる画像形式など、1つのコンポーネントの種類に対して複数のメディアの種類が存在する場合があります。

**オファー内の表現** 項目は、配列プロパティ内にオブジェクト構造を持ち `xdm:representations`ます。 各項目には次のプロパティを設定できます。

- **`xdm:placement`**  — このプロパティには、配置インスタンスへの参照が含まれます。 値は、リプレゼンテーションがオファーに追加されるときにチェックされます。 そのURIを持つプレースメントインスタンスが存在し、削除済みとしてマークされていない。 さらに、オファーインスタンスに配置参照用の値が同じ2つの表現が存在しないことを確認するチェックが実行されます。
- **`xdm:components`**  — コンテンツコンポーネントは、特定のオファー表示域に関連付けられたフラグメントです。 これらのフラグメントは、後でエンドユーザーの使い勝手の向上に使用されます。 Decisioningサービス自体がエンドユーザーのフルエクスペリエンスを構成するわけではありません。 次のプロパティは、すべてのコンポーネントのモデルの一部です。
   - **`@type`**  — このプロパティはコンポーネントタイプを識別します。 この概念の別の名前は、コンテンツフラグメントモデルです。 コンポーネント `@type` のURIは、エンドユーザーエクスペリエンスを組み立てるアプリケーションやサービスによって定義されるモデルのURIです。
   - **`repo:id`**  — このプロパティには、アセットが保存されるリポジトリ内のコンポーネントのメインリソースに対して、グローバルに一意で不変な識別子が含まれます。
   - **`repo:name`**  — このプロパティには、リポジトリ内のアセットに人間が読み取り可能な名前が含まれます。 この名前はユーザー定義であり、一意であることは保証されません。
   - **`repo:resolveURL`**  — このプロパティには、コンテンツリポジトリ内のアセットを読み取るための一意のリソースロケーターが含まれます。 これにより、呼び出すAPIをクライアントが把握しなくても、アセットを簡単に取得できます。 URLは、アセットのプライマリリソースのバイトを返します。
   - **`dc:format`**  — このプロパティは、Dublin Core Metadata Initiativeから取得されます。 フォーマットは、リソースを表示または操作するのに必要なソフトウェア、ハードウェア、または他の機器を決定するために使用できます。 ベストプラクティスは、制御された用語から値を選択することです(例えば、コンピューターのメディア形式を定義するインターネットメディアの種類のリスト)。
   - **`dc:language`**  — このプロパティには、リソースの言語が含まれます。 言語は、IETF RFC 3066で定義されている言語コードで指定されます。

プロパティで表される定義済みのコンポーネントタイプには次の3つがあり `@type` ます。

- https<span></span>://ns.adobe.com/experience/content-management/content-component-imagelink
- https<span></span>://ns.adobe.com/experience/content-management/content-component-text
- https<span></span>://ns.adobe.com/experience/content-management/content-component-html

プロパティの値に応じて、次の追加の `@type` プロパティ `xdm:components` が含まれます。

- **`xdm:linkURL`**  — コンポーネントが画像リンクの場合に表示されます。 このプロパティには、オファーに関連付けられたリンクが含まれ、エンドユーザが画像のコンテンツを操作したとき `user-agent` にに移動します。
- **`xdm:copyline`**  — コンポーネントがテキストの場合に使用します。 テキストアセットを参照するだけでなく、書式を設定できる長いフォームのテキストオファーの場合でも、xdm:copylineプロパティに短いテキスト文字列を直接格納できます。

追加のプロパティは、クライアントがコンテキスト処理命令を設定および評価するために使用できます。 例えば、オファーUIライブラリクライアントは、表示をより簡単に処理するために、次のオプションのプロパティを追加します。

- アレイ内の各アイテム内で、オファーライブラリUIクライアントが次のプロパティを追加し `xdm:components` ます。 これらのプロパティは、UIへの影響を理解しない限り、削除または操作しないでください。
   - **`offerui:previewThumbnail`**  — これは、オファーライブラリUIがアセットのレンダリングを表示する際に使用するオプションのプロパティです。 このレンディションは、アセット自体と同じではありません。 例えば、コンテンツはHTMLにすることができ、レンディションはその近似値を示すビットマップ画像にすることができます。 この（低品質の）レンディションは、オファーの表現ブロック内に表示されます。

オファーインスタンスに対するPATCH操作の例は、表現を操作する方法を示しています。

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

cURLの完全な構文については、「インスタンスの [更新とパッチ適用](#updating-and-patching-instances) 」を参照してください。 オファー `schemaId` がフォールバックオファーの場合は、 `https://ns.adobe.com/experience/offer-management/personalized-offer` またはその `https://ns.adobe.com/experience/offer-management/fallback-offer` である必要があります。

プロパティがま `xdm:representations` だない場合は、PATCH操作が失敗する可能性があります。 この場合、上記の追加操作の前に、配列を作成する別の追加操作を追加するか、 `xdm:representations` 単一の追加操作で配列を直接設定することができます。
説明したスキーマとプロパティは、すべてのオファータイプ、パーソナライゼーションオファー、およびフォールバックオファーで使用されます。 以下の2つのセクションでは、パーソナライゼーションオファーの側面について、制約と決定ルールについて説明します。

## オファー制約の設定

### カレンダーの制約

一般的な決定オプションには、カレンダーの制約として機能する開始と終了日時を指定できます。 プロパティは次のプロパティに埋め込まれま `xdm:selectionConstraint`す。

- **`xdm:startDate`**  — このプロパティは開始の日時を示します。 値は、RFC 3339規則に従って形式設定された文字列です。例えば、次のタイムスタンプのようになります。 &quot;2019-06-13T11:21:23.356Z&quot;
開始の日時に達していないデシジョニングオプションは、まだデシジョニングの対象と見なされません。
- **`xdm:endDate`**  — このプロパティは終了日時を示します。 値は、RFC 3339規則に従って形式設定された文字列です。例えば、次のタイムスタンプのようになります。 「2019-07-13T11:00:00.000Z」終了日時を過ぎたデシジョンオプションは、決定プロセスでの資格がなくなりました。

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

cURLの完全な構文については、「インスタンスの [更新とパッチ適用](#updating-and-patching-instances) 」を参照してください。 パラメー `schemaId` ターは必ず指定し `https://ns.adobe.com/experience/offer-management/personalized-offer`ます。 フォールバックオファーには制約はありません。

### 制限の制限

上限制約とは、制限のパラメータを定義する決定オプションのコンポーネントです。 上限とは、個々のプロファイルに対して、またすべてのプロファイルに対して、オプションを提案できる回数を制限するプロセスです。 プロパティは1以上の整数値を保持します。 プロパティは、プロパティ内にネストされ `xdm:cappingConstraint`ます。

- **`xdm:globalCap`**  — グローバルキャップとは、オファーが全体的に何回提案できるかを制約するものです。
- **`xdm:profileCap`** -プロファイルキャップとは、あるオファーが特定のプロファイルに何回提案されるかを制約するものです。

パーソナライゼーションオファーの制限制約の設定または変更は、次のPATCH呼び出しを使用して実行できます。

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

cURLの完全な構文については、「インスタンスの [更新とパッチ適用](#updating-and-patching-instances) 」を参照してください。 パラメー `schemaId` ターは必ず指定し `https://ns.adobe.com/experience/offer-management/personalized-offer`ます。 フォールバックオファーには制約はありません。

キャッピング値を除去するには、「add」操作を「remove」操作に置き換えます。 キャッピング値は個別に存在し、個別に設定または削除できることに注意してください。

### 適格性の制約

オファーは、決定プロセスで条件付きで選択できます。 パーソナライゼーションオファーが実施要件ルールへの参照を持つ場合、特定のプロファイルに対してオファーオブジェクトを考慮するには、ルールの条件がtrueに評価される必要があります。 実施要件ルールは、決定オファーとは無関係に作成および管理され、複数のパーソナライゼーションオプションから同じルールを参照できます。

ルールへの参照は、次のプロパティに埋め込まれま `xdm:selectionConstraint`す。

- **`xdm:eligibilityRule`**  — このプロパティは実施要件ルールへの参照を保持します。 値は、スキーマhttps://ns.adobe.com/experience/offer-management/eligibility-ruleのインスタンス `@id` の値です。

ルールの追加と削除は、PATCH操作でも実行できます。

```
[
  {
    "op":   "replace",
    "path": "/_instance/xdm:selectionConstraint/xdm:eligibilityRule",
    "value": "xcore:eligibility-rule:f84c6b33cc63c65" 
  }
]'
```

cURLの完全な構文については、「インスタンスの [更新とパッチ適用](#updating-and-patching-instances) 」を参照してください。 パラメー `schemaId` ターは必ず指定し `https://ns.adobe.com/experience/offer-management/personalized-offer`ます。 フォールバックオファーには制約はありません。

実施要件ルールは、カレンダー制約と共に `xdm:selectionConstraint` プロパティに埋め込まれます。 PATCH操作では、 `SelectionConstraint` プロパティ全体を削除しないでください。

## オファーの優先度の設定

適切な判断オプションをランク付けして、特定のプロファイルに最適なオプションを決定します。 ランキングを支援し、パーソナライゼーションオファー毎に基本優先度を設定できる別のメカニズムでランキングを決定できない場合のデフォルトを提供する。
基本の優先度は、次のプロパティに埋め込まれ `xdm:rank`ます。

- **`xdm:priority`**  — このプロパティは、プロファイル固有のランキング順序が不明な場合に、オファーが別のの上に選択されるデフォルトの順序を表します。 優先度の値を比較した後で、2つ以上のパーソナライゼーションオファーが依然として結び付けられている場合は、ランダムに選択され、オファーの提案で使用されます。 このプロパティの値は、0以上の整数である必要があります。

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

cURLの完全な構文については、「インスタンスの [更新とパッチ適用](#updating-and-patching-instances) 」を参照してください。 パラメー `schemaId` ターは必ず指定し `https://ns.adobe.com/experience/offer-management/personalized-offer`ます。 フォールバックオファーには、ランク付けプロパティはありません。

## 決定ルールの管理

実施要件ルールは、特定のプロファイルに対して特定の決定オプションが適格かどうかを判断するために評価される条件を保持します。 1つ以上の決定オプションにルールを添付すると、このオプションではルールがtrueに評価される必要があり、このユーザーに対して考慮されるオプションが定義されます。 ルールには、プロファイル属性に対するテストを含めたり、このプロファイルのエクスペリエンスイベントに関連する式を評価したり、デシジョンリクエストに渡されたコンテキストデータを含めたりできます。 例えば、条件は次のように記述できます。

> 「エリートの資格を持ち、現在のフライトの便番号を持つ6か月間に3回飛行した個人を含めます。」

インスタンスは、スキーマidhttps://ns.adobe.com/experience/offer-management/eligibility-ruleを使用して作成されます。 createまたはupdate呼び出しの `_instance` プロパティは次のようになります。

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

cURLの完全な構文については、「インスタンスの [更新とパッチ適用](#updating-and-patching-instances) 」を参照してください。 パラメー `schemaId` ターは必ず指定し `https://ns.adobe.com/experience/offer-management/eligibility-rule`ます。

ルールのconditionプロパティの値には、PQL式が含まれています。 コンテキストデータは、特別なパス式ー@{schemaID}を介して参照されます。

ルールは、内のセグメントに自然に合わせて調整され [!DNL Experience Platform] ます。多くの場合、プロファイルのプロパティをテストすることで、セグメントの意図を再利用するだけで `segmentMembership` す。 この `segmentMembership` プロパティには、既に評価されたセグメント条件の結果が含まれます。 これにより、組織は1回、ドメイン固有のオーディエンスを定義し、名前を付けて、条件を1回評価できます。

## オファーコレクションの管理

### タグとタグ付けオファーの作成

オファーは、各コレクションで適用されるフィルター条件が定義されているコレクションに整理できます。 現在、コレクション内のフィルター式は、次の2つの形式のいずれかで指定できます。

1. オファーの `@id` パラメーターは、オファーをコレクションに含めるために、識別子のリスト内のパラメーターと一致する必要があります。 このフィルターは、コレクション内のオファーのURIの定義済みリストに過ぎません。
2. オファーはタグ参照のリストを持つことができ、コレクションのフィルターはタグのリストで構成されます。 オファーは、次の場合にコレクションに含まれます。\
   a. 任意のフィルターのタグがオファーのタグの1つと一致する\
   b. すべてのフィルターのタグは、オファーのタグの1つに一致します

タグは、オファーインスタンスをリンクできる単純なインスタンスです。 これらは、それらを表示するための名前を持つ独自のインスタンスです。 ユーザーインターフェイスで表示しやすくするために、インスタンス間で一意の名前を付ける必要があります。

タグオブジェクトは、決定オファー（オプション）間の分類を確立するために役立ちます。 タグは多くのオファーによってリンクされ、オファーは多くのタグ参照を持つことができます。 オファーのカテゴリは、特定のタグインスタンスのセットに関連するすべてのオファーを参照することで確立されます。

タグインスタンスは、スキーマidhttps://ns.adobe.com/experience/offer-management/tagを使用して作成されます。 createまたはupdate呼び出しの `_instance` プロパティは次のようになります。

```json
{
  "xdm:name": "credit card"
} 
```

cURLの完全な構文については、「インスタンスの [更新とパッチ適用](#updating-and-patching-instances) 」を参照してください。 パラメー `schemaId` ターは必ず指定し `https://ns.adobe.com/experience/offer-management/tag`ます。


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

または、オファーにパッチを適用して、タグのリストを変更することもできます。

```json
[
  {
    "op":    "add",
    "path":  "/_instance/xdm:tags/-",
    "value": "xcore:tag:f66f677ad3c0ba7" 
  }
]' 
```

どちらの場合も、「インスタンスの [更新とパッチ適用](#updating-and-patching-instances) （完全なcURL構文）」を参照してください。 パラメー `schemaId` ターは必ず指定し `https://ns.adobe.com/experience/offer-management/personalized-offer`ます。

追加操作を正常に実行するには、 `xdm:tags` プロパティが既に存在している必要があります。 PATCH操作では、最初にarrayプロパティを追加し、次にその配列にタグ参照を追加することができるインスタンスにタグは存在しません。

### オファーコレクションのフィルターの定義

フィルターインスタンスは、スキーマidhttps://ns.adobe.com/experience/offer-management/offer-filterを使用して作成されます。 createまたはupdate呼び出しの `_instance` プロパティは次のようになります。

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

cURLの完全な構文については、「インスタンスの [更新とパッチ適用](#updating-and-patching-instances) 」を参照してください。 パラメー `schemaId` ターは必ず指定し `https://ns.adobe.com/experience/offer-management/offer-filter`ます。

- **`xdm:filterType`**  — このプロパティは、フィルターがタグを使用して設定されているか、IDによってオファーを直接参照しているかを示します。 タグを使用するようにフィルターを設定すると、フィルタータイプは、すべてのタグが特定のオファーのタグと一致する必要があるか、またはオファーがフィルターに適合するのに十分な任意のタグがあるかをさらに示します。 この列挙プロパティの有効な値は次のとおりです。
   - `offers`
   - `anyTags`
   - `allTags`
- **`ids`**  — プロパティには、の値に応じて、オファーインスタンスまたはタグインスタンスを参照するURIの配列が含まれ `xdm:filterType`ます。 .

次の呼び出しは、オファーが直接参照されている場合のcreateまたはupdate呼び出しの `_instance` プロパティの見え方を示しています。

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

## アクティビティの管理

オファーアクティビティは、判定プロセスを制御するために使用されます。 トピック/カテゴリ別にオファーを絞り込むために、在庫全体に適用されるオファーフィルタを指定します。また、予約スペースに収まるオファーに在庫を絞り込むための配置を指定し、組み合わせ制約によって使用可能なすべてのパーソナライゼーションオプション(オファー)が不適格になる場合の代替オプションを指定します。

アクティビティインスタンスはスキーマIDを使用して作成されます`https://ns.adobe.com/experience/offer-management/offer-activity`。 createまたはupdate呼び出しの `_instance` プロパティは次のようになります。

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

cURLの完全な構文については、「インスタンスの [更新とパッチ適用](#updating-and-patching-instances) 」を参照してください。 パラメー `schemaId` ターは必ず指定し `https://ns.adobe.com/experience/offer-management/offer-activity`ます。

- **`xdm:name`**  — この必須プロパティにはアクティビティ名が含まれています。 この名前は様々なユーザーインターフェイスに表示されます。
- **`xdm:status`**  — このプロパティは、インスタンスのライフサイクル管理に使用されます。 この値は、アクティビティがまだ構築中か（値=ドラフト）を示すために使用されるワークフロー状態を表し、一般にランタイムが考慮できる（値=ライブ）か、それ以上使用しない（値=アーカイブ）かを示します。
- **`xdm:placement`** -オファーがこのアクティビティのコンテキストとして決定されたときに在庫に適用される、デシジョニングプレースメントへの参照を含む必須プロパティ。 値は、使用されるオファー配置のURI(`@id`)です。
- **`xdm:filter`**  — このアクティビティのコンテキストでの決定時に在庫に適用されるオファーフィルターへの参照を含む必須プロパティ。 値は、使用されるオファーフィルターのURI(`@id`)です。
- **`xdm:fallback`**  — フォールバックオファーへの参照を含む必須プロパティ。 フォールバックオファーは、このアクティビティの判定がパーソナライゼーションオファーのいずれにも該当しない場合に使用されます。 値は、フォールバックオファーインスタンスのURI(`@id`)です。

### フォールバックオファーの管理

アクティビティインスタンスを作成する前に、アクティビティの配置に適したフォールバックオファーが存在する必要があります。 フォールバックオファーインスタンスはスキーマ識別子を使用して作成されます`https://ns.adobe.com/experience/offer-management/fallback-offer`。 createまたはupdate呼び出しの `_instance` プロパティには、パーソナライズオファーと同じ一般プロパティが含まれますが、他の制約を持つことはできません。

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

cURLの完全な構文については、「インスタンスの [更新とパッチ適用](#updating-and-patching-instances) 」を参照してください。 パラメー `schemaId` ターは必ず指定し `https://ns.adobe.com/experience/offer-management/fallback-offer`ます。

