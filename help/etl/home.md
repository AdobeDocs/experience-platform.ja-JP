---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: ETL統合の作成
topic: overview
translation-type: tm+mt
source-git-commit: bd9884a24c5301121f30090946ab24d9c394db1b
workflow-type: tm+mt
source-wordcount: '4227'
ht-degree: 0%

---


# Adobe Experience Platform用ETL統合の開発

ETL統合ガイドでは、Experience Platform用の高パフォーマンスで安全なコネクタを作成し、データをPlatformに取り込むための一般的な手順について説明しています。


- [カタログ](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/catalog.yaml)
- [データアクセス](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/data-access-api.yaml)
- [データ収集](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/ingest-api.yaml)
- [認証および認証API](../tutorials/authentication.md)
- [スキーマレジストリ](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/schema-registry.yaml)

このガイドには、ETLコネクタの設計時に使用するサンプルAPI呼び出しも含まれています。また、各Experience Platformサービスの概要とAPIの使用について詳しく説明したドキュメントへのリンクも含まれています。

GitHubでは、Apacheライセンスバージョン2.0の [ETLエコシステム統合リファレンスコード](https://github.com/adobe/acp-data-services-etl-reference) (ETL)を介してサンプル統合を利用できます。

## ワークフロー

次のワークフロー図は、Adobe Experience PlatformコンポーネントとETLアプリケーションおよびコネクタとの統合に関する概要を示しています。

![](images/etl.png)

## Adobe Experience Platform成分

ETLコネクタ統合には複数のExperience Platformコンポーネントが関係します。 次のリストでは、主要なコンポーネントと機能の概要を説明します。

- **AdobeIdentity Managementシステム(IMS)** — アドビのサービスに対する認証のフレームワークを提供します。
- **IMS組織** — 製品やサービスを所有またはライセンスでき、メンバーへのアクセスを許可する企業エンティティ。
- **IMSユーザー** - IMS組織のメンバー。 組織とユーザーの関係は、多くの人にとって非常に重要です。
- **Sandbox** — デジタルエクスペリエンスアプリケーションの開発と発展に役立つ、単一のPlatformインスタンスを仮想パーティションにします。
- **データ検出** — 取り込まれたデータおよび変換されたデータのメタデータをExperience Platformに記録します。
- **データアクセス** -Experience Platform内のデータにアクセスするためのインターフェイスをユーザーに提供します。
- **データ取り込み** — データ取り込みAPIを使用してデータをExperience Platformにプッシュします。
- **スキーマレジストリ** -Experience Platformで使用するデータの構造を記述するスキーマを定義し、保存します。

## Experience PlatformAPIの概要

以下の節では、Experience PlatformAPIの呼び出しを正常に行うために知る必要がある、または手元にある情報について説明します。

### サンプルAPI呼び出しの読み取り

このガイドは、リクエストをフォーマットする方法を示すAPI呼び出しの例を提供します。 例えば、パス、必須のヘッダー、適切にフォーマットされた要求ペイロードなどです。 API応答で返されるサンプルJSONも提供されます。 サンプルAPI呼び出しのドキュメントで使用される規則について詳しくは、Experience PlatformトラブルシューティングガイドのAPI呼び出し例 [の読み方に関する節](../landing/troubleshooting.md#how-do-i-format-an-api-request) を参照してください。

### 必要なヘッダーの値の収集

PlatformAPIを呼び出すには、まず [認証チュートリアルを完了する必要があります](../tutorials/authentication.md)。 次に示すように、Experience PlatformAPIのすべての呼び出しに必要な各ヘッダーの値を認証チュートリアルで説明します。

- 認証： 無記名 `{ACCESS_TOKEN}`
- x-api-key: `{API_KEY}`
- x-gw-ims-org-id: `{IMS_ORG}`

Experience Platform内のすべてのリソースは、特定の仮想サンドボックスに分離されます。 PlatformAPIへのすべてのリクエストには、操作が実行されるサンドボックスの名前を指定するヘッダーが必要です。

- x-sandbox-name: `{SANDBOX_NAME}`

>[!NOTE]
>
>Platform内のサンドボックスについて詳しくは、「 [Sandboxの概要に関するドキュメント](../sandboxes/home.md)」を参照してください。

ペイロード(POST、PUT、PATCH)を含むすべてのリクエストには、次の追加のヘッダーが必要です。

- Content-Type: application/json

## 一般的なユーザーフロー

最初に、ETLユーザーがExperience Platformユーザーインターフェイス(UI)にログインし、標準のコネクタまたはプッシュサービスコネクタを使用して取り込み用のデータセットを作成します。

ユーザーはUIでデータセットスキーマを選択し、出力データセットを作成します。 スキーマの選択は、Platformに取り込まれるデータ（記録または時系列）の種類に応じて異なります。 UI内の「スキーマ」タブをクリックすると、スキーマがサポートする動作タイプを含む、使用可能なすべてのスキーマを表示できます。

ETLツールでは、ユーザーは、適切な接続を設定した後（資格情報を使用して）、マッピング変換を設計する際に開始が発生します。 ETLツールには、既にExperience Platformコネクタがインストールされていると想定されます（この統合ガイドでは定義されていないプロセス）。

サンプルのETLツールおよびワークフローのモックアップは、 [ETLワークフローで提供されています](./workflow.md)。 ETLツールの形式は異なる場合がありますが、ほとんどの場合、類似した機能が公開されています。

>[!NOTE]
>
>ETLコネクタは、データを取り込み、オフセットする日付（データを読み込むウィンドウ）を示すタイムスタンプフィルタを指定する必要があります。 ETLツールは、このUIまたは別の関連UIでこれら2つのパラメーターの取得をサポートする必要があります。 Adobe Experience Platformでは、これらのパラメーターは、使用可能な日付（存在する場合）またはデータセットのバッチオブジェクトに存在する取得した日付にマップされます。

### データセットの表示リスト

マッピングに使用するデータソースを使用して、 [カタログAPIを使用して使用可能なすべてのデータセットのリストを取得できます](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/catalog.yaml)。

1つのAPIリクエストを発行して、使用可能なすべてのデータセット( `GET /dataSets`)を呼び出すことをお勧めします。

データセットの _フル情報が要求される場合_ 、応答ペイロードのサイズが3 GBを超える可能性があり、全体のパフォーマンスが低下する可能性があります。 したがって、クエリパラメーターを使用して必要な情報のみをフィルタリングすると、カタログクエリの効率が向上します。

#### リストろ過

応答をフィルタリングする場合、アンパサンド(`&`)を使用してフィルターーを区切ることで、1回の呼び出しで複数のパラメーターを使用できます。 一部のクエリパラメーターでは、値をコンマで区切ったリスト（例えば、以下のサンプルリクエストの「プロパティ」フィルター）で受け取ることができます。

カタログの応答は、設定された制限に従って自動的に課金されますが、「limit」クエリパラメータを使用して制約をカスタマイズし、返されるオブジェクトの数を制限することができます。 事前設定済みのカタログ応答の制限は次のとおりです。

- limitパラメーターが指定されていない場合、応答ペイロードあたりのオブジェクトの最大数は20です。
- 他のすべてのCatalogクエリのグローバル制限は、100オブジェクトです。
- データセットクエリーでは、propertiesクエリーパラメーターを使用してobservableSchemaが要求される場合、返されるデータセットの最大数は20です。
- 無効な制限パラメーター(含む `limit=0`)が満たされると、適切な範囲を示すHTTP 400エラーが発生します。
- 制限またはオフセットがクエリパラメータとして渡される場合は、ヘッダとして渡されるパラメータよりも優先されます。

クエリパラメーターについて詳しくは、 [カタログサービスの概要を参照してください](../catalog/home.md)。

**API形式**

```http
GET /catalog/dataSets
GET /catalog/dataSets?{filter1}={value1},{value2}&{filter2}={value3}
```

**リクエスト**

```shell
curl -X GET "https://platform.adobe.io/data/foundation/catalog/dataSets?limit=3&properties=name,description,schemaRef" \
  -H "Authorization: Bearer {ACCESS_TOKEN}" \
  -H "x-api-key: {API_KEY}" \
  -H "x-gw-ims-org-id: {IMS_ORG}" \
  -H "x-sandbox-name: {SANDBOX_NAME}"
```

[Catalog APIの呼び出し方法の詳細な例については、「](../catalog/home.md) Catalog Serviceの概要 [」を参照してください](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/catalog.yaml)。

**応答**

応答には、`limit=3``properties` クエリパラメーターで示される「名前」、「説明」、「スキーマ参照」を示す3つのデータセットが含まれます。

```json
{
    "5b95b155419ec801e6eee780": {
        "name": "Store Transactions",
        "description": "Retails Store Transactions",
        "schemaRef": {
            "id": "https://ns.adobe.com/{TENANT_ID}/schemas/274f17bc5807ff307a046bab1489fb18",
            "contentType": "application/vnd.adobe.xed+json;version=1"
        }
    },
    "5c351fa2f5fee300000fa9e8": {
        "name": "Loyalty Members",
        "description": "Loyalty Program Members",
        "schemaRef": {
            "id": "https://ns.adobe.com/{TENANT_ID}/schemas/fbc52b243d04b5d4f41eaa72a8ba58be",
            "contentType": "application/vnd.adobe.xed+json;version=1"
        }
    },
    "5c1823b19e6f400000993885": {
        "name": "Web Traffic",
        "description": "Retail Web Traffic",
        "schemaRef": {
            "id": "https://ns.adobe.com/{TENANT_ID}/schemas/2025a705890c6d4a4a06b16f8cf6f4ca",
            "contentType": "application/vnd.adobe.xed+json;version=1"
        }
    }
}
```

### 表示データセットスキーマ

データセットの「schemaRef」プロパティには、データセットの基盤となるXDMスキーマを参照するURIが含まれています。 XDMスキーマ(「schemaRef」)は、データセットで使用できる _可能性のあるすべての_ フィールドを表します。必ずしも使用されているフィールドとは限りません __ （下の「observableSchema」を参照）。

XDMスキーマは、書き込み可能なすべてのフィールドのリストをユーザーに提示する必要がある場合に使用するスキーマです。

前の応答オブジェクト(`https://ns.adobe.com/{TENANT_ID}/schemas/274f17bc5807ff307a046bab1489fb18`)の最初の「schemaRef.id」値は、スキーマレジストリ内の特定のXDMスキーマを指すURIです。 スキーマは、スキーマレジストリAPIに対してルックアップ(GET)リクエストを行うことで取得できます。

>[!NOTE]
>
>「schemaRef」プロパティは、現在非推奨になっている「スキーマ」プロパティを置き換えます。 データセットに「schemaRef」がない場合、または値が含まれていない場合は、「schemaRef」プロパティの存在を確認する必要があります。 これは、前の呼び出しの `properties` クエリパラメーターで「schemaRef」を「スキーマ」に置き換えることで可能です。 「スキーマ」プロパティの詳細については、次の「 [データセット「スキーマ」プロパティ](#dataset-schema-property-deprecated---eol-2019-05-30) 」セクションを参照してください。

**API形式**

```http
GET /schemaregistry/tenant/schemas/{url encoded schemaRef.id}
```

**リクエスト**

リクエストでは、スキーマのURLエンコードされた `id` URI（「schemaRef.id」属性の値）を使用し、Acceptヘッダーが必要です。

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/schemas/https%3A%2F%2Fns.adobe.com%2F{TENANT_ID}%2Fschemas%2F274f17bc5807ff307a046bab1489fb18 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Accept: application/vnd.adobe.xed-full+json; version=1' \
```

応答の形式は、要求で送信されるAcceptヘッダーの種類に応じて異なります。 また、ルックアップ要求は、Acceptヘッダーに含め `version` る必要があります。 次の表に、検索に使用できるAcceptヘッダーの概要を示します。

| 同意 | 説明 |
| ------ | ----------- |
| `application/vnd.adobe.xed-id+json` | リスト(GET)の要求、タイトル、ID、バージョン |
| `application/vnd.adobe.xed-full+json; version={major version}` | $refsとallOfが解決されました。タイトルと説明があります。 |
| `application/vnd.adobe.xed+json; version={major version}` | $refとallOfを含む生データには、タイトルと説明が含まれます。 |
| `application/vnd.adobe.xed-notext+json; version={major version}` | 生（$refとallOfを含み、タイトルや説明は含まない） |
| `application/vnd.adobe.xed-full-notext+json; version={major version}` | $refs and allOf resolved, no titles or descriptions |
| `application/vnd.adobe.xed-full-desc+json; version={major version}` | $refsとallOf解決済み、記述子が含まれています |

>[!NOTE]
>
>`application/vnd.adobe.xed-id+json` とは、最も一般的 `application/vnd.adobe.xed-full+json; version={major version}` に使用されるAcceptヘッダーです。 `application/vnd.adobe.xed-id+json` は、「title」、「id」、「version」のみを返すので、スキーマレジストリにリソースをリストする場合に推奨されます。 `application/vnd.adobe.xed-full+json; version={major version}` は、（「プロパティ」の下にネストされた）すべてのフィールド、およびタイトルと説明を返すので、特定のリソースを（「id」で）表示する場合に推奨されます。

**応答**

返されるJSONスキーマは、構造とフィールドレベルの情報（「type」、「format」、「minimum」、「maximum」など）を記述したものです。 の値を含むJSON形式でシリアル化されます。 取り込みにJSON以外のシリアル化形式（ParquetやScalaなど）を使用する場合、『 [スキーマレジストリガイド](../xdm/tutorials/create-schema-api.md) 』には、目的のJSONタイプ(「meta:xdmType」)と、他の形式での対応する表現を示すテーブルが含まれています。

この表と共に、『スキーマレジストリ開発者ガイド』には、スキーマレジストリAPIを使用して可能なすべての呼び出しの詳細な例が記載されています。

### データセットの「スキーマ」プロパティ（廃止 — EOL 2019-05-30）

データセットには、「スキーマ」プロパティが含まれる場合があります。このプロパティは非推奨となり、後方互換性を確保するために一時的に使用可能なままになります。 例えば、以前に作成したリクエストと同様のリスト(GET)リクエスト( `properties` クエリパラメーターで「スキーマ」が「schemaRef」の代わりに使用された)を返すと、次のようになります。

```json
{
  "5ba9452f7de80400007fc52a": {
    "name": "Sample Dataset 1",
    "description": "Description of Sample Dataset 1.",
    "schema": "@/xdms/context/person"
  }
}
```

データセットの「スキーマ」プロパティに値が入力された場合、このシグナルは、スキーマが非推奨の `/xdms` スキーマであること、およびサポートされている場合、ETLコネクターは、「スキーマ」プロパティの値をエンドポイント( `/xdms` カタログAPIで非推奨のエンドポイント [](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/catalog.yaml))と共に使用して従来のスキーマを取得する必要があります。

**API形式**

```http
GET /catalog/{"schema" property without the "@"}
```

**リクエスト**

```shell
curl -X GET "https://platform.adobe.io/data/foundation/catalog/xdms/context/person?expansion=xdm" \
  -H "x-gw-ims-org-id: {IMS_ORG}" \
  -H "x-sandbox-name: {SANDBOX_NAME}" \
  -H "Authorization: Bearer {ACCESS_TOKEN}" \
  -H "x-api-key: {API_KEY}"
```

>[!NOTE]
>
>オプションのクエリパラメーター `expansion=xdm`は、参照されるスキーマを完全に展開し、インラインにするようAPIに指示します。 この操作は、すべてのフィールドのリストをユーザーに表示する場合に行います。

**応答**

データセットスキーマを [表示する手順と同様](#view-dataset-schema)、応答にはJSONとしてシリアル化されたデータの構造とフィールドレベルの情報を記述したJSONスキーマが含まれています。

>[!NOTE]
>
>「スキーマ」フィールドが空の場合や完全に存在しない場合は、コネクタは「schemaRef」フィールドを読み取り、 [スキーマセットスキーマを](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/schema-registry.yaml) 表示する前の手順に示すように、スキーマレジストリAPI [()を使用する必要があります](#view-dataset-schema)。

### 「observableSchema」プロパティ

データセットの「observableSchema」プロパティのJSON構造は、XDMスキーマJSONの構造と一致します。 &quot;observableSchema&quot;には、受信入力ファイルに存在するフィールドが含まれています。 Experience Platformにデータを書き込む場合、ターゲットスキーマのすべてのフィールドを使用する必要はありません。 代わりに、使用するフィールドのみを指定する必要があります。

監視可能なスキーマとは、データを読み取る場合、または読み取り/マップで使用できるフィールドのリストを示す場合に使用するスキーマです。

```json
{
    "598d6e81b2745f000015edcb": {
        "observableSchema": {
            "type": "object",
            "meta:xdmType": "object",
            "properties": {
                "name": {
                    "type": "string",
                },
                "age": {
                    "type": "string",
                }
            }
        }
    }
}
```

### プレビューデータ

ETLアプリケーションは、データをプレビューする機能を提供する場合があります(ETLワークフローの[図](./workflow.md)8を参照)。 データアクセスAPIには、プレビューデータに対して複数のオプションが用意されています。

データアクセスAPIを使用してデータをプレビューする手順説明など、追加情報については、「 [データアクセス](../data-access/tutorials/dataset-data.md)」のチュートリアルを参照してください。

### 「properties」クエリパラメーターを使用したデータセットの詳細の取得

上記の手順でデータセットのリストを [表示するように](#view-list-of-datasets)、「properties」クエリパラメータを使用して「ファイル」をリクエストできます。

データセットのクエリーと利用可能な応答フィルターの詳細については、 [カタログサービスの概要](../catalog/home.md) （英語）を参照してください。

**API形式**

```http
GET /catalog/dataSets?limit={value}&properties={value}
```

**リクエスト**

```shell
curl -X GET "https://platform.adobe.io/data/foundation/catalog/dataSets?limit=1&properties=files" \
  -H "Authorization: Bearer {ACCESS_TOKEN}" \
  -H "x-api-key: {API_KEY}" \
  -H "x-gw-ims-org-id: {IMS_ORG}" \
  -H "x-sandbox-name: {SANDBOX_NAME}"
```

**応答**

この応答には、「files」プロパティを示すデータセット(`limit=1`)が1つ含まれます。

```json
{
  "5bf479a6a8c862000050e3c7": {
    "files": "@/dataSets/5bf479a6a8c862000050e3c7/views/5bf479a654f52014cfffe7f1/files"
  }
}
```

### 「files」属性を使用したリストデータセットファイル

GET要求を使用して、「files」属性を使用してファイルの詳細を取得することもできます。

**API形式**

```http
GET /catalog/dataSets/{DATASET_ID}/views/{VIEW_ID}/files
```

**リクエスト**

```shell
curl -X GET "https://platform.adobe.io/data/foundation/catalog/dataSets/5bf479a6a8c862000050e3c7/views/5bf479a654f52014cfffe7f1/files" \
  -H "Accept: application/json" \
  -H "x-gw-ims-org-id: {IMS_ORG}" \
  -H "x-sandbox-name: {SANDBOX_NAME}" \
  -H "Authorization: Bearer {ACCESS_TOKEN}" \
  -H "x-api-key : {API_KEY}"
```

**応答**

この応答には、データセットファイルIDが最上位のプロパティとして含まれ、そのファイルの詳細はデータセットファイルIDオブジェクトに含まれます。

```json
{
    "194e89b976494c9c8113b968c27c1472-1": {
        "batchId": "194e89b976494c9c8113b968c27c1472",
        "dataSetViewId": "5bf479a654f52014cfffe7f1",
        "imsOrg": "{IMS_ORG}",
        "availableDates": {},
        "createdUser": "{USER_ID}",
        "createdClient": "{API_KEY}",
        "updatedUser": "{USER_ID}",
        "version": "1.0.0",
        "created": 1542749145828,
        "updated": 1542749145828
    },
    "14d5758c107443e1a83c714e56ca79d0-1": {
        "batchId": "14d5758c107443e1a83c714e56ca79d0",
        "dataSetViewId": "5bf479a654f52014cfffe7f1",
        "imsOrg": "{IMS_ORG}",
        "availableDates": {},
        "createdUser": "{USER_ID}",
        "createdClient": "{API_KEY}",
        "updatedUser": "{USER_ID}",
        "version": "1.0.0",
        "created": 1542752699111,
        "updated": 1542752699111
    },
    "ea40946ac03140ec8ac4f25da360620a-1": {
        "batchId": "ea40946ac03140ec8ac4f25da360620a",
        "dataSetViewId": "5bf479a654f52014cfffe7f1",
        "imsOrg": "{IMS_ORG}",
        "availableDates": {},
        "createdUser": "{USER_ID}",
        "createdClient": "{API_KEY}",
        "updatedUser": "{USER_ID}",
        "version": "1.0.0",
        "created": 1542756935535,
        "updated": 1542756935535
    }
}
```

### ファイルの詳細の取得

前の応答で返されたデータセットファイルIDは、GETリクエストで使用し、データアクセスAPIを介してファイルの詳細を取り込むことができます。

デ [ータアクセスの概要](../data-access/home.md) (Data Access Overview)には、データアクセスAPIの使用方法に関する詳細が含まれています。

**API形式**

```http
GET /export/files/{DATASET_FILE_ID}
```

**リクエスト**

```shell
curl -X GET "https://platform.adobe.io/data/foundation/export/files/ea40946ac03140ec8ac4f25da360620a-1" \
  -H "x-gw-ims-org-id: {IMS_ORG}" \
  -H "x-sandbox-name: {SANDBOX_NAME}" \
  -H "Authorization: Bearer {ACCESS_TOKEN}" \
  -H "x-api-key : {API_KEY}"
```

**応答**

```json
[
    {
    "name": "{FILE_NAME}.parquet",
    "length": 2576,
    "_links": {
        "self": {
            "href": "https://platform.adobe.io/data/foundation/export/files/ea40946ac03140ec8ac4f25da360620a-1?path=samplefile.parquet"
            }
        }
    }
]
```

### プレビューファイルデータ

&quot;href&quot;プロパティは、 [データアクセスAPIを介してプレビューデータを取得するのに使用できます](../data-access/home.md)。

**API形式**

```http
GET /export/files/{FILE_ID}?path={FILE_NAME}.{FILE_FORMAT}
```

**リクエスト**

```shell
curl -X GET "https://platform.adobe.io/data/foundation/export/files/ea40946ac03140ec8ac4f25da360620a-1?path=samplefile.parquet" \
  -H "x-gw-ims-org-id: {IMS_ORG}" \
  -H "x-sandbox-name: {SANDBOX_NAME}" \
  -H "Authorization: Bearer {ACCESS_TOKEN}" \
  -H "x-api-key : {API_KEY}"
```

上記の要求に対する応答には、ファイルの内容のプレビューが含まれます。

詳細なリクエストや応答を含む、データアクセスAPIについて詳しくは、「 [データアクセスの概要](../data-access/home.md)」を参照してください。

### データセットから「fileDescription」を取得

変換後のデータの出力として宛先コンポーネントを指定すると、データエンジニアは出力データセットを選択します(ETLワークフロー[の図12](workflow.md))。 XDMスキーマは、出力データセットに関連付けられます。 書き込まれるデータは、Data Discovery APIのデータセットエンティティの「fileDescription」属性で識別されます。 この情報は、データセットID (`{DATASET_ID}`)を使用して取得できます。 JSON応答の「fileDescription」プロパティで、要求された情報が提供されます。

**API形式**

```shell
GET /catalog/dataSets/{DATASET_ID}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{DATASET_ID}` | アクセスしようとしているデータセットの `id` 値。 |

**リクエスト**

```shell
curl -X GET "https://platform.adobe.io/data/foundation/catalog/dataSets/59c93f3da7d0c00000798f68" \
-H "accept: application/json" \
-H "x-gw-ims-org-id: {IMS_ORG}" \
-H "x-sandbox-name: {SANDBOX_NAME}" \
-H "Authorization: Bearer {ACCESS_TOKEN}" \
-H "x-api-key : {API_KEY}"
```

**応答**

```JSON
{
  "59c93f3da7d0c00000798f68": {
    "version": "1.0.4",
    "fileDescription": {
        "persisted": false,
        "format": "parquet"
    }
  }
}
```

データは、 [Data Ingestion APIを使用してExperience Platformに書き込まれます](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/ingest-api.yaml)。  データの書き込みは非同期的なプロセスです。 データがAdobe Experience Platformに書き込まれると、データが完全に書き込まれた後でのみ、バッチが作成され、成功としてマークされます。

Experience Platform内のデータは、パーケーファイルの形式で書き込む必要があります。

## 実行段階

実行開始として、コネクタは（ソースコンポーネントで定義されたとおり） [データアクセスAPIを使用してExperience Platformからデータを読み取り](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/data-access-api.yaml)ます。 変換プロセスは、特定の期間のデータを読み取ります。 内部的には、ソース・データセットのクエリ・バッチが行われます。 クエリー中、これらのバッチに対して、パラメータ化（時系列データまたは増分データをローリング）された開始日付とリストデータセットファイルを使用し、開始がこれらのデータセットファイルに対してデータのリクエストを行います。

### 変換の例

ETL変換 [ドキュメントのサンプルには](./transformations.md) 、ID処理やデータ型マッピングなど、様々な変換の例が含まれています。 これらの変換は参考にしてください。

### Experience Platformからデータを読み取ります

[カタログAPIを使用すると](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/catalog.yaml)、指定した開始時間と終了時間の間にあるすべてのバッチを取得し、作成された順序で並べ替えることができます。

**リクエスト**

```shell
curl -X GET "https://platform.adobe.io/data/foundation/catalog/batches?dataSet=DATASETID&createdAfter=START_TIMESTAMP&createdBefore=END_TIMESTAMP&sort=desc:created" \
  -H "Accept: application/json" \
  -H "Authorization:Bearer {ACCESS_TOKEN}" \
  -H "x-api-key : {API_KEY}" \
  -H "x-gw-ims-org-id: {IMS_ORG}" \
  -H "x-sandbox-name: {SANDBOX_NAME}"
```

フィルタリングバッチの詳細については、「 [データアクセスのチュートリアル](../data-access/tutorials/dataset-data.md)」を参照してください。

### バッチからのファイルの取得

(`{BATCH_ID}`)を探しているバッチのIDを取得したら、 [データアクセスAPIを使用して、特定のバッチに属するファイルのリストを取得できます](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/data-access-api.yaml)。  詳しくは、 [データアクセスのチュートリアル](../data-access/tutorials/dataset-data.md)。

**リクエスト**

```shell
curl -X GET "https://platform.adobe.io/data/foundation/export/batches/{BATCH_ID}/files" \
  -H "x-gw-ims-org-id: {IMS_ORG}" \
  -H "x-sandbox-name: {SANDBOX_NAME}" \
  -H "Authorization: Bearer {ACCESS_TOKEN}" \
  -H "x-api-key : {API_KEY}"
```

### ファイルIDを使用したファイルへのアクセス

ファイルの固有のID(`{FILE_ID`)を使用して、 [データアクセスAPI](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/data-access-api.yaml) （名前、バイト単位のサイズ、ファイルをダウンロードするためのリンクなど）を使用して、ファイルの特定の詳細にアクセスできます。

**リクエスト**

```shell
curl -X GET "https://platform.adobe.io/data/foundation/export/files/{FILE_ID}" \
  -H "Authorization: Bearer {ACCESS_TOKEN}" \
  -H "x-gw-ims-org-id: {IMS_ORG}" \
  -H "x-sandbox-name: {SANDBOX_NAME}" \
  -H "x-api-key : {API_KEY}"
```

応答は、1つのファイルまたはディレクトリを指す場合があります。 それぞれの詳細については、「 [データアクセスのチュートリアル](../data-access/tutorials/dataset-data.md)」を参照してください。

### ファイルの内容にアクセス

この [データアクセスAPI](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/data-access-api.yaml) は、特定のファイルのコンテンツにアクセスするために使用できます。 コンテンツを取り込むために、ファイルIDを用いてファイルにアクセスする際に返された値を用い `_links.self.href` てGET要求を行う。

**リクエスト**

```shell
curl -X GET "https://platform.adobe.io/data/foundation/export/files/{DATASET_FILE_ID}?path=filename1.csv" \
  -H "Authorization: Bearer {ACCESS_TOKEN}" \
  -H "x-gw-ims-org-id: {IMS_ORG}" \
  -H "x-sandbox-name: {SANDBOX_NAME}" \
  -H "x-api-key: {API_KEY}"
```

この要求への応答には、ファイルの内容が含まれます。 応答のページネーションに関する詳細を含む詳細については、「データアクセスAPI [を使用したデータのクエリ方法](../data-access/tutorials/dataset-data.md) 」チュートリアルを参照してください。

### スキーマコンプライアンスのためのレコードの検証

データの書き込み中に、XDMスキーマで定義されている検証ルールに従ってデータの検証を選択できます。 スキーマの検証の詳細については、GitHubの [ETLエコシステム統合リファレンスコードを参照してください](https://github.com/adobe/experience-platform-etl-reference/blob/fd08dd9f74ae45b849d5482f645f859f330c1951/README.md#validation)。

GitHubにある参照実装を使用している場合は、 [systemプロパティを使用して、この実装のスキーマ検証を有効にでき](https://github.com/adobe/experience-platform-etl-reference/blob/fd08dd9f74ae45b849d5482f645f859f330c1951/README.md)`-DenableSchemaValidation=true`ます。

検証は、論理XDM型に対して、文字列 `minLength` や文字列、整数などの属性を使用して実行でき `maxlength` ます。 `minimum` そ `maximum` の他の型に対しても実行できます。 『 [スキーマレジストリAPI開発者ガイド](../xdm/api/getting-started.md) 』には、XDMの種類と検証に使用できるプロパティについて概説した表が含まれています。

>[!NOTE]
>
>様々な `integer` タイプに対して提供される最小値と最大値は、タイプがサポートするMIN値とMAX値ですが、これらの値は、選択した最小値と最大値に制限することができます。

### バッチの作成

データが処理されると、ETLツールは [Batch Ingest APIを使用してExperience Platformにデータを書き戻します](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/ingest-api.yaml)。 データセットにデータを追加する前に、データをバッチにリンクし、そのバッチを特定のデータセットに後でアップロードする必要があります。

**リクエスト**

```SHELL
curl -X POST "https://platform.adobe.io/data/foundation/import/batches" \
  -H "accept: application/json" \
  -H "x-gw-ims-org-id: {IMS_ORG}" \
  -H "x-sandbox-name: {SANDBOX_NAME}" \
  -H "Authorization: Bearer {ACCESS_TOKEN}" \
  -H "x-api-key: {API_KEY}" \
  -d '{
        "datasetId":"{DATASET_ID}"
      }'
```

サンプルのリクエストや応答を含むバッチの作成について詳しくは、 [バッチインジェストの概要を参照してください](../ingestion/batch-ingestion/overview.md)。

### データセットへの書き込み

新しいバッチが正常に作成されたら、ファイルを特定のデータセットにアップロードできます。 1つのバッチに複数のファイルを投稿して、プロモーションが完了するまです。 ファイルは、 _小さいファイルアップロードAPI_；を使用してアップロードできます。 ただし、ファイルが大きすぎてゲートウェイの制限を超えている場合は、 _Large File Upload API_（大）を使用できます。 大きいファイルと小さいファイルの両方のアップロードの使用について詳しくは、 [バッチインジェストの概要を参照してください](../ingestion/batch-ingestion/overview.md)。

**リクエスト**

Experience Platform内のデータは、パーケーファイルの形式で書き込む必要があります。

```shell
curl -X PUT "https://platform.adobe.io/data/foundation/import/batches/{BATCH_ID}/dataSets/{DATASET_ID}/files/{FILE_NAME}.parquet" \
  -H "accept: application/json" \
  -H "x-gw-ims-org-id:{IMS_ORG}" \
  -H "Authorization:Bearer ACCESS_TOKEN" \
  -H "x-api-key: API_KEY" \
  -H "content-type: application/octet-stream" \
  --data-binary "@{FILE_PATH_AND_NAME}.parquet"
```

### バッチアップロード完了のマーク

すべてのファイルがバッチにアップロードされた後、バッチは完了を示すために署名できます。 これにより、完了したファイルに対してカタログ「DataSetFile」エントリが作成され、生成バッチに関連付けられます。 次に、カタログバッチが成功とマークされ、ダウンストリームフローがトリガーされ、使用可能なデータが取り込まれます。

データは、最初にAdobe Experience Platform上のステージング場所にランディングし、次にカタログと検証の後、最終的な場所に移動します。 すべてのデータが永続的な場所に移動されると、バッチは成功とマークされます。

**リクエスト**

```shell
curl -X POST "https://platform.adobe.io/data/foundation/import/batches/{BATCH_ID}?action=COMPLETE" \
  -H "x-gw-ims-org-id: {IMS_ORG}" \
  -H "x-sandbox-name: {SANDBOX_NAME}" \
  -H "Authorization:Bearer {ACCESS_TOKEN}" \
  -H "x-api-key : {API_KEY}"
```

正常終了の場合、応答はHTTPステータス200 OKを返し、応答本文は空になります。

ETLツールは、データの読み取り時に、ソースデータセットのタイムスタンプを必ずメモします。

スケジュールまたはイベントの呼び出しによって、次の変換の実行時に、ETLが開始を実行し、以前に保存されたタイムスタンプと今後のすべてのデータを要求します。

### 最後のバッチ状態の取得

ETLツールで新しいタスクを実行する前に、最後のバッチが正常に完了したことを確認する必要があります。 Catalog [Service API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/catalog.yaml) （カタログサービスAPI）には、関連するバッチの詳細を提供するバッチ固有のオプションが用意されています。

**リクエスト**

```shell
curl -X GET "https://platform.adobe.io/data/foundation/catalog/batches?limit=1&sort=desc:created" \
  -H "Accept: application/json" \
  -H "x-gw-ims-org-id: {IMS_ORG}" \
  -H "x-sandbox-name: {SANDBOX_NAME}" \
  -H "Authorization: Bearer {ACCESS_TOKEN}" \
  -H "x-api-key: {API_KEY}"
```

**応答**

以下に示すように、以前のバッチ「status」の値が「success」の場合は、新しいタスクをスケジュールできます。

```json
"{BATCH_ID}": {
    "imsOrg": "{IMS_ORG}",
    "created": 1494349962314,
    "createdClient": "{API_KEY}",
    "createdUser": "CLIENT_USER_ID@AdobeID",
    "updatedUser": "CLIENT_USER_ID@AdobeID",
    "updated": 1494349963467,
    "status": "success",
    "errors": [],
    "version": "1.0.1",
    "availableDates": {}
}
```

### ID別の最後のバッチ状態の取得

個々のバッチステータスは、を使用してGET要求を発行することで、 [Catalog Service API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/catalog.yaml) (カタログサービスAPI `{BATCH_ID}`)から取得できます。 使用さ `{BATCH_ID}` れるIDは、バッチの作成時に返されるIDと同じです。

**リクエスト**

```shell
curl -X GET "https://platform.adobe.io/data/foundation/catalog/batches/{BATCH_ID}" \
  -H "Accept: application/json" \
  -H "x-gw-ims-org-id: {IMS_ORG}" \
  -H "x-sandbox-name: {SANDBOX_NAME}" \
  -H "Authorization: Bearer {ACCESS_TOKEN}" \
  -H "x-api-key: {API_KEY}"
```

**回答 — 成功**

次の応答は、「成功」を示します。

```json
"{BATCH_ID}": {
    "imsOrg": "{IMS_ORG}",
    "created": 1494349962314,
    "createdClient": "{API_KEY}",
    "createdUser": "{CREATED_USER}",
    "updatedUser": "{UPDATED_USER}",
    "updated": 1494349962314,
    "status": "success",
    "errors": [],
    "version": "1.0.1",
    "availableDates": {}
}
```

**応答 — 失敗**

エラーが発生した場合は、応答から「エラー」を抽出し、ETLツールでエラー・メッセージとして表示できます。

```json
"{BATCH_ID}": {
    "imsOrg": "{IMS_ORG}",
    "created": 1494349962314,
    "createdClient": "{API_KEY}",
    "createdUser": "{CREATED_USER}",
    "updatedUser": "{UPDATED_USER}",
    "updated": 1494349962314,
    "status": "failure",
    "errors": [
        {
            "code": "200",
            "description": "Error in validating schema for file: 'adl://dataLake.azuredatalakestore.net/connectors-dev/stage/BATCHID/dataSetId/contact.csv' with errorMessage=adl://dataLake.azuredatalakestore.net/connectors-dev/stage/BATCHID/dataSetId/contact.csv is not a Parquet file. expected magic number at tail [80, 65, 82, 49] but found [57, 98, 55, 10] and errorType=java.lang.RuntimeException",
            "rows": []
        }
    ],
    "version": "1.0.1",
    "availableDates": {}
}
```

## 差分データとスナップショット・データとイベント、プロファイル

データは、次のように2つの行列で表すことができます。

| 増分イベント | 増分プロファイル |
|-------------------------------|----------------------|
| スナップショット・イベント（可能性は低い） | スナップショットプロファイル |

イベントデータは、通常、各行にインデックス付きのタイムスタンプ列がある場合に使用します。

プロファイルデータは通常、データにタイムスタンプがなく、各行を主キー/複合キーで識別できる場合に使用します。

インクリメンタルデータは、新しい/更新されたデータのみがシステムに取り込まれ、データセットの現在のデータに追加されます。

スナップショットデータとは、すべてのデータがシステムに取り込まれ、データセット内の以前のデータの一部またはすべてがスナップショットデータに置き換えられたものです。

増分イベントの場合、ETLツールはバッチエンティティの使用可能な日付/作成日を使用する必要があります。 プッシュサービスの場合、利用可能な日付は存在しないので、マーキングの増分にバッチ作成日/更新日が使用されます。 増分イベントのバッチはすべて処理する必要があります。

増分プロファイルの場合、ETLツールはバッチエンティティの作成日/更新日を使用します。 一般的に、増分プロファイルデータのバッチごとに処理が必要です。

スナップショット・イベントは、データのサイズが大きいため、非常に低い確率で発生します。 ただし、これが必要な場合、ETLツールは最後のバッチのみを選択して処理する必要があります。

スナップショット・プロファイルを使用する場合、ETLツールは、システムに到着した最後のデータ・バッチを選択する必要があります。 ただし、変更のバージョンを追跡する必要がある場合は、すべてのバッチの処理が必要になります。 ETLプロセス内の重複除外処理は、ストレージコストを管理するのに役立ちます。

## バッチ再生とデータ再処理

過去&#39;n&#39;日間、ETL処理中のデータが予期したとおりに発生しなかった場合、またはソースデータ自体が正しくなかった場合は、バッチ再生とデータ再処理が必要になる場合があります。

これを行うには、クライアントのデータ管理者がPlatformUIを使用して、破損したデータを含むバッチを削除します。 その後、ETLを再実行する必要が生じる可能性が高くなり、これにより正しいデータが再入力されます。 ソース自体が破損したデータを持つ場合、データエンジニア/管理者は、ソースバッチを修正し、データを(Adobe Experience PlatformまたはETLコネクタを介して)再取り込みする必要があります。

生成されるデータのタイプに基づいて、特定のデータセットから1つのバッチまたはすべてのバッチを削除するのは、データエンジニアが選択する方法です。 Experience Platformのガイドラインに従って、データが削除/アーカイブされます。

ETL機能によるデータ削除が重要になる可能性が高いと考えられます。

削除が完了すると、クライアント管理者は、バッチが削除された時点からコアサービスの処理を再開するようにAdobe Experience Platformを再設定する必要があります。

## 同時バッチ処理

データ管理者やエンジニアは、顧客の判断に従って、特定のデータセットの特性に応じて、データの抽出、変換、読み込みを順次的または同時的に行うことを決定できます。 また、これは、クライアントが変換されたデータでターゲット設定を行っている使用事例にも基づきます。

例えば、クライアントが更新可能な永続性ストアに永続的で、イベントの順序や順序が重要な場合、クライアントは、順次ETL変換を使用して、厳密にジョブを処理する必要がある可能性があります。

別の場合は、順番が正しくないデータは、指定されたタイムスタンプを使用して内部的に並べ替えられるダウンストリームのアプリケーション/プロセスで処理できます。 このような場合、並行ETL変換は、処理時間を改善するために実行可能な場合があります。

ソース・バッチの場合、再びクライアント・プリファレンスとコンシューマ制約に依存します。 行のリージェンシー/順序に関係なくソースデータを並行して取得できる場合、変換プロセスは、より高い並列度（順序がずれた処理に基づく最適化）を持つプロセスバッチを作成できます。 ただし、変換がタイムスタンプや優先順位の変更に従う必要がある場合は、データアクセスAPIまたはETLツールのスケジューラー/呼び出しで、可能な限りバッチが正しく処理されないようにする必要があります。

## 繰延

繰り返しとは、入力データがまだダウンストリームプロセスに送り出すのに十分な量に達していないが、将来使用できる可能性があるプロセスです。 クライアントは、データの抽出と次の変換の実行時の再処理に関する判断を通知するため、保存期間内の将来のリンチ/調整/ステッチが可能な場合に、将来の照合に対するデータ・ウィンドウの個々の許容範囲と、処理コストを判断します。 このサイクルは、行が十分に処理されるか、古すぎて投資を続行できないと見なされるまで継続されます。 繰り返しごとに、繰り返し後のデータが生成されます。繰り返し後のデータは、前の繰り返しのすべての繰り返し後のデータのスーパーセットです。

Adobe Experience Platformは、現在、遅延データを識別していないので、クライアントの導入では、ETLとデータセットの手動設定に依存して、遅延データの保持に使用できるソースデータセットのミラーリングPlatform内に別のデータセットを作成する必要があります。 この場合、遅延データは、スナップショット・データに似ています。 ETL変換を実行するたびに、ソースデータは遅延データと統合され、処理用に送信されます。

## チャンゲログ

| 日付 | アクション | 説明 |
| ---- | ------ | ----------- |
| 2019-01-19 | データセットから「fields」プロパティを削除しました | データセットには、以前はスキーマのコピーが含まれていた「fields」プロパティが含まれていました。 この機能は、今後は使用しないでください。 &quot;fields&quot;プロパティが見つかった場合は無視し、代わりに&quot;ovservedSchema&quot;または&quot;schemaRef&quot;を使用します。 |
| 2019-03-15 | &quot;schemaRef&quot;プロパティがデータセットに追加されました | データセットの「schemaRef」プロパティには、データセットのベースとなるXDMスキーマを参照するURIが格納され、そのデータセットで使用できるすべての潜在的なフィールドを表します。 |
| 2019-03-15 | すべてのエンドユーザー識別子が「identityMap」プロパティにマップされます | 「identityMap」は、CRM ID、ECID、忠誠度プログラムIDなど、サブジェクトの一意の識別子をすべてカプセル化したものです。 このマップは、 [IDサービス](../identity-service/home.md) (Identity Service)で使用され、サブジェクトの既知のIDと匿名IDをすべて解決し、エンドユーザーごとに1つのIDグラフを形成します。 |
| 2019-05-30 | EOLと「スキーマ」プロパティをデータセットから削除 | カタログAPIの非推奨の `/xdms` エンドポイントを使用して、データセット「スキーマ」プロパティからスキーマへの参照リンクが提供されました。 これは、新しいスキーマレジストリAPIで参照されているスキーマの「id」、「version」および「contentType」を提供する「schemaRef」に置き換えられました。 |