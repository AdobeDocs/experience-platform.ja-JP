---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: ETL統合の作成
topic: overview
translation-type: tm+mt
source-git-commit: 4817162fe2b7cbf4ae4c1ed325db2af31da5b5d3

---


# Adobe Experience Platform用ETL統合の開発

ETL統合ガイドでは、エクスペリエンスプラットフォーム用の高パフォーマンスで安全なコネクタを作成し、データをプラットフォームに取り込むための一般的な手順について説明しています。


- [カタログ](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/catalog.yaml)
- [データアクセス](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/data-access-api.yaml)
- [データ収集](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/ingest-api.yaml)
- [認証および認証API](../tutorials/authentication.md)
- [スキーマ登録](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/schema-registry.yaml)

このガイドには、ETLコネクタの設計時に使用するサンプルAPI呼び出しも含まれています。また、各Experience Platformサービスの概要とAPIの使用について詳しく説明したドキュメントへのリンクも含まれています。

GitHubでは、Apacheライセンスバージョン2.0の下の [ETL Ecosystem Integration Reference Code](https://github.com/adobe/acp-data-services-etl-reference) （ETLエコシステム統合リファレンスコード）を使用して、サンプル統合を利用できます。

## ワークフロー

次のワークフロー図は、Adobe Experience PlatformコンポーネントとETLアプリケーションおよびコネクタとの統合に関する概要を示しています。

![](images/etl.png)

## Adobe Experience Platformコンポーネント

ETLコネクタ統合には、複数のエクスペリエンスプラットフォームコンポーネントが関係しています。 次のリストでは、主なコンポーネントと機能の概要を説明します。

- **Adobe Identity Management System(IMS)** — アドビのサービスに対する認証のフレームワークを提供します。
- **IMS組織** — 製品やサービスを所有またはライセンスし、そのメンバーへのアクセスを許可できる企業エンティティ。
- **IMS User** - IMS組織のメンバー。 組織とユーザーの関係は多数に及びます。
- **Sandbox** — デジタルエクスペリエンスアプリケーションの開発と発展を支援する、単一のプラットフォームインスタンスの仮想パーティション。
- **データ検出** — 取り込まれたデータおよび変換されたデータのメタデータをExperience Platformに記録します。
- **データアクセス** — エクスペリエンスプラットフォームでデータにアクセスするためのインターフェイスをユーザーに提供します。
- **データ取り込み** — データ取り込みAPIを使用して、エクスペリエンスプラットフォームにデータをプッシュします。
- **スキーマレジストリ** - Experience Platformで使用するデータの構造を記述するスキーマを定義し、保存します。

## Experience Platform APIの概要

次の節では、エクスペリエンスプラットフォームAPIの呼び出しを正しく行うために知っておく必要がある、または手元に置く必要がある追加情報を示します。

### サンプルAPI呼び出しの読み取り

このガイドは、リクエストをフォーマットする方法を示すAPI呼び出しの例を提供します。 これには、パス、必須ヘッダー、適切にフォーマットされたリクエストペイロードが含まれます。 API応答で返されるサンプルJSONも提供されます。 サンプルAPI呼び出しのドキュメントで使用される表記について詳しくは、エクスペリエンスプラットフォームのトラブルシューテ [ィングガイドのAPI呼び出し例の読み方に関する節](../landing/troubleshooting.md#how-do-i-format-an-api-request) （英語のみ）を参照してください。

### 必要なヘッダーの値の収集

プラットフォームAPIを呼び出すには、まず認証チュートリアルを完了する必要 [があります](../tutorials/authentication.md)。 次に示すように、認証チュートリアルで、すべてのエクスペリエンスプラットフォームAPI呼び出しで必要な各ヘッダーの値を入力します。

- 認証：無記名 `{ACCESS_TOKEN}`
- x-api-key: `{API_KEY}`
- x-gw-ims-org-id: `{IMS_ORG}`

エクスペリエンスプラットフォームのすべてのリソースは、特定の仮想サンドボックスに分離されます。 プラットフォームAPIへのすべてのリクエストには、操作が行われるサンドボックスの名前を指定するヘッダーが必要です。

- x-sandbox-name: `{SANDBOX_NAME}`

>[!NOTE] プラットフォームのサンドボックスについて詳しくは、サンドボックスの概要ドキュメ [ントを参照してくださ](../sandboxes/home.md)い。

ペイロード(POST、PUT、PATCH)を含むすべてのリクエストには、次のヘッダーが追加で必要です。

- コンテンツタイプ：application/json

## 一般的なユーザーフロー

まず、ETLユーザーがExperience Platformユーザーインターフェイス(UI)にログインし、標準のコネクターまたはプッシュサービスコネクターを使用して取り込み用のデータセットを作成します。

ユーザーは、UIでデータセットオプションを選択して、出力データセットをスキーマします。 どのスキーマを選択するかは、プラットフォームに取り込むデータ（記録または時系列）のタイプによって異なります。 UI内の[スキーマ]タブをクリックすると、スキーマがサポートする動作タイプを含む、使用可能なすべてのスキーマを表示できます。

ETLツールで、ユーザーは、適切な接続を設定した後（ユーザーの資格情報を使用して）、マッピング変換の設計を開始します。 ETLツールには、既にExperience Platform Connectorsがインストールされていると想定されます（この統合ガイドでは、プロセスが定義されていません）。

サンプルのETLツールおよびワークフローのモックアップは、 [ETLワークフローで提供されています](./workflow.md)。 ETLツールの形式は異なる場合がありますが、ほとんどの場合、類似した機能が公開されています。

>[!NOTE] ETLコネクタは、データを取り込み、オフセットする日付（データを読み込むウィンドウ）を示すタイムスタンプフィルタを指定する必要があります。 ETLツールは、この2つのパラメーターの取り込みまたは他の関連UIの取り込みをサポートする必要があります。 Adobe Experience Platformでは、これらのパラメーターは、使用可能な日付（存在する場合）またはデータセットのバッチオブジェクトに存在する取り込み日付にマップされます。

### 表示リストのデータセット

マッピングにデータのソースを使用すると、使用可能なすべてのリストのデータセットを [Catalog APIを使用して取得できます](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/catalog.yaml)。

1つのAPIリクエストを発行して、使用可能なすべての表示セット()を使用す `GET /dataSets`る場合は、応答のサイズを制限するクエリパラメータを含めることをお勧めします。

データセットのフル _情報が要求される場合_ 、応答ペイロードのサイズが過去3 GBに達する可能性があり、全体のパフォーマンスが低下する可能性があります。 したがって、クエリパラメータを使用して必要な情報のみをフィルタリングすると、カタログクエリの効率が向上します。

#### リストろ過

応答をフィルタリングする場合、フィルターーをアンパサンド(`&`)で区切ることで、1回の呼び出しで複数のパラメーターを使用できます。 一部のクエリパラメーターは、値のコンマ区切りリストを受け取ります。例えば、以下のサンプルリクエストの「プロパティ」フィルターを使用できます。

カタログのクエリは、設定された制限に従って自動的に課金されますが、「limit」応答パラメータを使用して制約をカスタマイズし、返されるオブジェクトの数を制限することができます。 事前設定されたカタログの応答の制限は、次のとおりです。

- limitパラメーターを指定しない場合、応答ペイロードあたりのオブジェクトの最大数は20です。
- その他のすべてのカタログクエリのグローバル制限は100オブジェクトです。
- データセットクエリーの場合、observableSchemaがpropertiesクエリーパラメーターを使用して要求された場合、返されるデータセットの最大数は20です。
- 無効な制限パラメータ（含む） `limit=0`が満たされると、適切な範囲を示すHTTP 400エラーが発生する。
- 制限またはオフセットがヘッダーパラメーターとしてクエリされた場合、ヘッダーとして渡されたものより優先されます。

クエリパラメータについて詳しくは、カタログサービスの概要 [を参照してくださ](../catalog/home.md)い。

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

カタログAPIを呼び出す [方法の詳細な例については](../catalog/home.md) 、「カタログサービスの概要 [」を参照し](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/catalog.yaml)てください。

**応答**

応答には、`limit=3`「name」、「description」および「schemaRef」を示す3つのデータセットが含まれ、このデータセットは `properties` クエリパラメータで示されます。

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

データセットの「schemaRef」プロパティには、データセットの基となるXDMスキーマを参照するURIが含まれます。 XDMスキーマ(「schemaRef」)は、データセットで使用できる可能性のあるすべてのフィールドを表します。 ____ （以下の「observableSchema」を参照）、必ずしも使用されているフィールドとは限りません。

XDMスキーマは、書き込み可能なすべての使用可能なフィールドのリストをユーザーに提示する必要がある場合に使用するスキーマです。

前の応答オブジェクト(`https://ns.adobe.com/{TENANT_ID}/schemas/274f17bc5807ff307a046bab1489fb18`)の最初の「schemaRef.id」値は、スキーマレジストリ内の特定のXDMスキーマを指すURIです。 このスキーマは、スキーマレジストリAPIに対して参照(GET)リクエストを行うことで取得できます。

>[!NOTE] 「schemaRef」プロパティは、現在非推奨になっている「スキーマ」プロパティを置き換えます。 「schemaRef」がデータセットにない場合、または値が含まれていない場合は、「スキーマ」プロパティが存在するかどうかを確認する必要があります。 これは、前の呼び出しのクエリパラメーターで「schemaRef」を「スキーマ」に置き換え `properties` ることで実行できます。 「スキーマ」プロパティの詳細については、次の「 [Dataset &quot;スキーマ&quot;プロパティ](#dataset-schema-property-deprecated---eol-2019-05-30) 」の節を参照してください。

**API形式**

```http
GET /schemaregistry/tenant/schemas/{url encoded schemaRef.id}
```

**リクエスト**

リクエストは、スキーマのURLエンコ `id` ードされたURI（「schemaRef.id」属性の値）を使用し、Acceptヘッダーが必要です。

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/schemas/https%3A%2F%2Fns.adobe.com%2F{TENANT_ID}%2Fschemas%2F274f17bc5807ff307a046bab1489fb18 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Accept: application/vnd.adobe.xed-full+json; version=1' \
```

応答の形式は、リクエストで送信されるAcceptヘッダーのタイプによって異なります。 また、ルックアップリクエストを受け入 `version` れるヘッダーに含める必要があります。 次の表に、使用可能な参照用ヘッダーの概要を示します。

| 承認 | 説明 |
| ------ | ----------- |
| `application/vnd.adobe.xed-id+json` | リスト(GET)のリクエスト、タイトル、ID、バージョン |
| `application/vnd.adobe.xed-full+json; version={major version}` | $refsとallOfが解決され、タイトルと説明があります |
| `application/vnd.adobe.xed+json; version={major version}` | $refとallOfを含む生のファイルには、タイトルと説明が含まれます。 |
| `application/vnd.adobe.xed-notext+json; version={major version}` | 生（$refとallOfを含み、タイトルや説明は含まない） |
| `application/vnd.adobe.xed-full-notext+json; version={major version}` | $refs and allOf resolved, no titles or descriptions |
| `application/vnd.adobe.xed-full-desc+json; version={major version}` | $refsとallOf解決済み、記述子が含まれています |

>[!NOTE] とは、最 `application/vnd.adobe.xed-id+json` もよく使用さ `application/vnd.adobe.xed-full+json; version={major version}` れるAcceptヘッダーです。 `application/vnd.adobe.xed-id+json` は、「title」、「id」および「version」のみを返すので、スキーマレジストリにリソースをリストする場合に推奨されます。 `application/vnd.adobe.xed-full+json; version={major version}` は、すべてのフィールド（「プロパティ」の下にネスト）とタイトルおよび説明を返すので、特定のリソースを（「id」で）表示する場合に推奨されます。

**応答**

返されるJSONスキーマは、構造とフィールドレベルの情報（「type」、「format」、「minimum」、「maximum」など）を示します。JSONとしてシリアル化されたデータの 取り込みにJSON以外のシリアル化形式（ParquetやScalaなど）を使用する場合、『 [スキーマレジストリガイド](../xdm/tutorials/create-schema-api.md) 』には、必要なJSONタイプ(「meta:xdmType」)と、その対応する他の形式での表現を示す表が含まれます。

この表と共に、スキーマレジストリ開発者ガイドには、スキーマレジストリAPIを使用して実行できるすべての呼び出しの詳細な例が記載されています。

### データセットの「スキーマ」プロパティ（廃止 — EOL 2019-05-30）

データセットには「スキーマ」プロパティが含まれている場合があり、現在は廃止され、後方互換性を確保するために一時的に使用可能なままになっています。 例えば、以前に作成したリスト(GET)リクエストと同様のリスト(クエリパラメーターで「スキーマ」が「schemaRef」に置き換えられた場合)は、次のよ `properties` うに返されます。

```json
{
  "5ba9452f7de80400007fc52a": {
    "name": "Sample Dataset 1",
    "description": "Description of Sample Dataset 1.",
    "schema": "@/xdms/context/person"
  }
}
```

データセットの「スキーマ」プロパティに値が入力された場合、スキーマが非推奨のスキーマであることを示し、サポートされている場合は、ETLコネクターがエンドポイント `/xdms` ( `/xdms` Catalog APIの非推奨のエンドポイント [](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/catalog.yaml))の「スキーマ」プロパティの値を使用してレガシースキーマを取得します。

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

>[!NOTE] オプションのクエリ `expansion=xdm`パラメーターは、参照されるパラメーターを完全に展開し、インラインにするようAPIにスキーマします。 この操作は、すべての潜在的なフィールドのリストをユーザーに表示する場合に行います。

**応答**

データセットのスキーマを表 [示する手順と同様](#view-dataset-schema)、応答には、JSONとしてシリアル化されたデータの構造とフィールドレベルの情報を記述したJSONスキーマが含まれます。

>[!NOTE] 「スキーマ」フィールドが空の場合や完全に存在しない場合は、コネクタは「schemaRef」フィールドを読み取り、前の手順で示した [スキーマレジストリAPI](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/schema-registry.yaml) (データセットスキーマを表示する場合)を使用する [必要があります](#view-dataset-schema)。

### 「observableSchema」プロパティ

データセットの「observableSchema」プロパティのJSON構造は、XDMスキーマJSONのJSON構造と一致します。 「observableSchema」には、受信入力ファイルに存在するフィールドが含まれています。 Experience Platformにデータを書き込む場合、ユーザーは、ユーザープラットフォームのすべてのフィールドを使用する必要はありません。ターゲットスキーマ。 代わりに、使用されているフィールドのみを指定する必要があります。

監視可能なスキーマとは、データを読み取る場合や、読み取り/マップに使用できるフィールドのリストを示す場合に使用するスキーマです。

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

ETLアプリケーションは、プレビュー・データの機能を提供する[ことができます(ETLワークフローの図8](./workflow.md))。 データアクセスAPIには、データデータに対する複数のプレビューがあります。

データアクセスAPIを使用してデータをプレビューする手順説明など、追加の情報については、データアクセスチュートリアルを [参照してください](../data-access/tutorials/dataset-data.md)。

### 「プロパティ」パラメーターを使用してデータセットの詳細をクエリする

上記の手順でデータセットのリスト [を表示するように](#view-list-of-datasets)、「properties」クエリパラメータを使用して「files」をリクエストできます。

データセットのクエリーと使用可能な応 [答フィルターの詳細については](../catalog/home.md) 、「カタログサービスの概要」を参照してください。

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

この応答には、「files」プロパティを示す1`limit=1`つのデータセット()が含まれます。

```json
{
  "5bf479a6a8c862000050e3c7": {
    "files": "@/dataSets/5bf479a6a8c862000050e3c7/views/5bf479a654f52014cfffe7f1/files"
  }
}
```

### リストデータセットファイルの「files」属性

GETリクエストを使用して、「files」属性を使用してファイルの詳細を取り込むこともできます。

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

この応答には、データセットファイルIDが最上位のプロパティとして含まれ、ファイルの詳細はデータセットファイルIDオブジェクトに含まれます。

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

前の応答で返されたデータセットファイルIDは、GETリクエストで使用して、データアクセスAPIを介してファイルの詳細を取得できます。

データ [アクセスの概要](../data-access/home.md) (Data Access Overview)には、データアクセスAPIの使用方法の詳細が含まれています。

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

「href」プロパティは、データアクセスAPIを介してプレビューデータを取 [得するために使用できます](../data-access/home.md)。

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

詳細なリクエストや応答を含む、データアクセスAPIに関する詳細は、データアクセスの概要を [参照してください](../data-access/home.md)。

### データセットから「fileDescription」を取得

変換後のデータの出力として、変換先のコンポーネントを使用して、データエンジニアが出力デ[ータセットを選択します(ETLワークフローの図12](workflow.md))。 XDMスキーマは、出力データセットに関連付けられます。 書き込まれるデータは、Data Discovery APIのデータセットエンティティの「fileDescription」属性で識別されます。 この情報は、データセットID(`{DATASET_ID}`)を使用して取得できます。 JSON応答の「fileDescription」プロパティが、要求された情報を提供します。

**API形式**

```shell
GET /catalog/dataSets/{DATASET_ID}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{DATASET_ID}` | アク `id` セスしようとしているデータセットの値。 |

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

データは、 [Data Ingest APIを使用してエクスペリエンスプラットフォームに書き込まれます](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/ingest-api.yaml)。  データの書き込みは非同期的なプロセスです。 データがAdobe Experience Platformに書き込まれると、データが完全に書き込まれた後でのみ、バッチが作成され、成功としてマークされます。

エクスペリエンスプラットフォームのデータは、パーケーファイルの形式で書き込む必要があります。

## 実行段階

実行開始として、コネクタ（ソースコンポーネントで定義）は、データアクセスAPIを使用してExperience Platformからデータを読み [取ります](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/data-access-api.yaml)。 変換プロセスは、特定の時間範囲のデータを読み取ります。 内部的には、ソース・クエリセットのバッチが作成されます。 クエリー時に、開始化（時系列データの周期的なデータ、または増分データ）されたリストデータセットファイルをこれらのバッチに使用し、開始がこれらのデータセットファイルのデータをリクエストします。

### 変換の例

サンプ [ルETL変換ドキュメントには](./transformations.md) 、ID処理やデータ型マッピングなど、多数のサンプル変換が含まれています。 これらの変換を参考にしてください。

### エクスペリエンスプラットフォームからのデータの読み取り

[Catalog APIを使用すると](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/catalog.yaml)、指定した開始時間と終了時間の間にあるすべてのバッチを取得し、作成された順序で並べ替えることができます。

**リクエスト**

```shell
curl -X GET "https://platform.adobe.io/data/foundation/catalog/batches?dataSet=DATASETID&createdAfter=START_TIMESTAMP&createdBefore=END_TIMESTAMP&sort=desc:created" \
  -H "Accept: application/json" \
  -H "Authorization:Bearer {ACCESS_TOKEN}" \
  -H "x-api-key : {API_KEY}" \
  -H "x-gw-ims-org-id: {IMS_ORG}" \
  -H "x-sandbox-name: {SANDBOX_NAME}"
```

バッチのフィルタリングの詳細については、「データアクセスのチュートリ [アル」を参照してくださ](../data-access/tutorials/dataset-data.md)い。

### バッチからのファイルの取得

探しているバッチのIDを取得したら(`{BATCH_ID}`)、データアクセスAPIを使用して、特定のバッチに属するファイルのリストを取得 [できます](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/data-access-api.yaml)。  詳しくは、データアクセスのチュートリアル [を参照してください](../data-access/tutorials/dataset-data.md)。

**リクエスト**

```shell
curl -X GET "https://platform.adobe.io/data/foundation/export/batches/{BATCH_ID}/files" \
  -H "x-gw-ims-org-id: {IMS_ORG}" \
  -H "x-sandbox-name: {SANDBOX_NAME}" \
  -H "Authorization: Bearer {ACCESS_TOKEN}" \
  -H "x-api-key : {API_KEY}"
```

### ファイルIDを使用してファイルにアクセスする

ファイルの一意のID(`{FILE_ID`[](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/data-access-api.yaml) )を使用して、データアクセスAPIを使用して、ファイルの名前、バイト単位のサイズ、ファイルをダウンロードするためのリンクなど、ファイルの特定の詳細にアクセスできます。

**リクエスト**

```shell
curl -X GET "https://platform.adobe.io/data/foundation/export/files/{FILE_ID}" \
  -H "Authorization: Bearer {ACCESS_TOKEN}" \
  -H "x-gw-ims-org-id: {IMS_ORG}" \
  -H "x-sandbox-name: {SANDBOX_NAME}" \
  -H "x-api-key : {API_KEY}"
```

応答は単一のファイルまたはディレクトリを指す場合があります。 各項目の詳細は、データアクセスのチュートリ [アルを参照してください](../data-access/tutorials/dataset-data.md)。

### ファイルの内容にアクセス

データ [アクセスAPIは](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/data-access-api.yaml) 、特定のファイルのコンテンツにアクセスするために使用できます。 コンテンツを取り込むために、ファイルIDを用いてファイルにアクセスする際に返さ `_links.self.href` れた値を用いてGETリクエストを行う。

**リクエスト**

```shell
curl -X GET "https://platform.adobe.io/data/foundation/export/files/{DATASET_FILE_ID}?path=filename1.csv" \
  -H "Authorization: Bearer {ACCESS_TOKEN}" \
  -H "x-gw-ims-org-id: {IMS_ORG}" \
  -H "x-sandbox-name: {SANDBOX_NAME}" \
  -H "x-api-key: {API_KEY}"
```

この要求に対する応答には、ファイルの内容が含まれます。 応答のページ番号付けの詳細を含め、詳しくは、「データアクセスAPIを使用し [てデータをクエリする方法](../data-access/tutorials/dataset-data.md) 」チュートリアルを参照してください。

### レコードのコンプライアンススキーマの検証

データの書き込み中に、XDMスキーマで定義された検証ルールに従ってデータの検証を選択できます。 スキーマの検証の詳細については、GitHubの [ETLエコシステム統合リファレンスコードを参照してください](https://github.com/adobe/experience-platform-etl-reference/blob/fd08dd9f74ae45b849d5482f645f859f330c1951/README.md#validation)。

GitHubにある参照実装を使用している場合は、 [systemプロパティを使用して](https://github.com/adobe/experience-platform-etl-reference/blob/fd08dd9f74ae45b849d5482f645f859f330c1951/README.md)、この実装のスキーマ検証を有効にできま `-DenableSchemaValidation=true`す。

検証は、論理XDM型に対して、およびなどの属性(文字列、 `minLength` 整数な `maxlength` ど)を使用し `minimum` て実 `maximum` 行できます。 『 [スキーマレジストリAPI開発者ガイド](../xdm/api/getting-started.md) 』には、XDMの種類と検証に使用できるプロパティの概要を示す表が含まれています。

>[!NOTE] 様々なタイプに対して提供される最小値と最大値は、タイプがサポートするMIN値とMAX値ですが、これらの値は、選択した最小値と最大値にさらに制限される場合があります。 `integer`

### バッチの作成

データが処理されると、ETLツールは [Batch Ingestion APIを使用してデータをエクスペリエンスプラットフォームに書き戻します](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/ingest-api.yaml)。 データセットにデータを追加する前に、そのデータをバッチにリンクし、後で特定のデータセットにアップロードする必要があります。

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

サンプルのリクエストや応答を含む、バッチの作成に関する詳細は、バッチ取り込みの概要を [参照してくださ](../ingestion/batch-ingestion/overview.md)い。

### データセットへの書き込み

新しいバッチが正常に作成されたら、ファイルを特定のデータセットにアップロードできます。 複数のファイルを1つのバッチで投稿して、プロモートすることができます。 ファイルは、 _Small File Upload API_;ただし、ファイルが大きすぎてゲートウェイの制限を超えている場合は、 _Large File Upload APIを使用できます_。 大きいファイルと小さいファイルの両方のアップロードの使用について詳しくは、バッチ取り込みの概要 [を参照してくださ](../ingestion/batch-ingestion/overview.md)い。

**リクエスト**

エクスペリエンスプラットフォームのデータは、パーケーファイルの形式で書き込む必要があります。

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

すべてのファイルがバッチにアップロードされたら、バッチの完了を通知できます。 これにより、完成したファイルに対してカタログ「DataSetFile」エントリが作成され、生成バッチに関連付けられます。 次に、カタログバッチが成功とマークされ、ダウンストリームフローがトリガーされ、使用可能なデータが取り込まれます。

データは、最初にAdobe Experience Platformのステージング場所に配置され、次に、カタログ化と検証の後、最終的な場所に移動されます。 すべてのデータが永続的な場所に移動されると、バッチは成功とマークされます。

**リクエスト**

```shell
curl -X POST "https://platform.adobe.io/data/foundation/import/batches/{BATCH_ID}?action=COMPLETE" \
  -H "x-gw-ims-org-id: {IMS_ORG}" \
  -H "x-sandbox-name: {SANDBOX_NAME}" \
  -H "Authorization:Bearer {ACCESS_TOKEN}" \
  -H "x-api-key : {API_KEY}"
```

成功した場合、応答はHTTPステータス200 OKを返し、応答本文は空になります。

ETLツールは、データの読み取り時に、ソースデータセットのタイムスタンプを必ずメモします。

次回の変換の実行時(スケジュールやイベントの呼び出しによる場合など)、ETLは、以前に保存したタイムスタンプと今後のすべてのデータからデータを要求する開始を行います。

### 最後のバッチ状態の取得

ETLツールで新しいタスクを実行する前に、最後のバッチが正常に完了したことを確認する必要があります。 Catalog Service API [は](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/catalog.yaml) 、関連するバッチの詳細を提供するバッチ固有のオプションを提供します。

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

新しいタスクは、以下に示すように、以前のバッチ「status」の値が「success」の場合にスケジュールできます。

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

### ID別に最後のバッチステータスを取得

個々のバッチステータスは、 [Catalog Service APIを使用して](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/catalog.yaml) 、を使用してGETリクエストを発行することで取得できま `{BATCH_ID}`す。 使用さ `{BATCH_ID}` れるIDは、バッチの作成時に返されたIDと同じです。

**リクエスト**

```shell
curl -X GET "https://platform.adobe.io/data/foundation/catalog/batches/{BATCH_ID}" \
  -H "Accept: application/json" \
  -H "x-gw-ims-org-id: {IMS_ORG}" \
  -H "x-sandbox-name: {SANDBOX_NAME}" \
  -H "Authorization: Bearer {ACCESS_TOKEN}" \
  -H "x-api-key: {API_KEY}"
```

**応答 — 成功**

次の応答は、「成功」を示しています。

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

障害が発生した場合、「エラー」は応答から抽出され、ETLツールでエラー・メッセージとして表示されます。

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

## 差分データとスナップショット・データとイベントとプロファイル

データは、次のように2つの行列で表すことができます。

| 増分イベント | 増分プロファイル |
|-------------------------------|----------------------|
| スナップショットイベント（あまり考えられません） | スナップショットプロファイル |

イベントデータは、通常、各行にインデックス付きのタイムスタンプ列がある場合に使用されます。

プロファイルデータは、通常、データにタイムスタンプがなく、各行を主キー/複合キーで識別できる場合に使用します。

増分データとは、新しい/更新されたデータのみがシステムに取り込まれ、データセット内の現在のデータに追加される場所です。

スナップショットデータとは、すべてのデータがシステムに取り込まれ、データセット内の以前のデータの一部またはすべてがスナップショットデータに置き換えられたときのことです。

増分イベントの場合、ETLツールは、バッチエンティティの使用可能な日付/作成日を使用する必要があります。 プッシュサービスの場合、利用可能な日付は存在しないので、マークの増分に対してバッチ作成日/更新日が使用されます。 増分イベントのバッチはすべて処理する必要があります。

増分プロファイルの場合、ETLツールはバッチエンティティの作成日/更新日を使用します。 一般に、増分データのすべてのプロファイルデータを処理する必要があります。

スナップショットイベントは、データの大きさにより、非常に低い確率で発生します。 ただし、これが必要な場合は、ETLツールは処理する最後のバッチのみを選択する必要があります。

スナップショット・プロファイルを使用する場合、ETLツールは、システムに到着した最後のデータ・バッチを選択する必要があります。 ただし、変更のバージョンを追跡する必要がある場合は、すべてのバッチを処理する必要があります。 ETLプロセス内の重複除外処理は、ストレージコストの管理に役立ちます。

## バッチ再生とデータ再処理

過去&#39;n&#39;日間、ETL処理中のデータが予期したとおりに発生しなかった場合や、ソースデータ自体が正しくなかった場合は、バッチ再生とデータの再処理が必要になる場合があります。

これを行うには、クライアントのデータ管理者がプラットフォームUIを使用して、破損したデータを含むバッチを削除します。 その後、ETLを再実行し、正しいデータを再入力する必要が生じます。 ソース自体が破損したデータを持つ場合、データエンジニアまたは管理者は、ソースバッチを修正し、データを（Adobe Experience PlatformまたはETLコネクタを介して）再度取り込む必要があります。

生成されるデータのタイプに基づいて、1つのバッチまたは特定のデータセットからすべてのバッチを削除するのは、データエンジニアの選択です。 データは、エクスペリエンスプラットフォームのガイドラインに従って削除/アーカイブされます。

データを削除するETL機能が重要になる可能性が高いシナリオです。

削除が完了したら、バッチが削除された時点からコアサービスの処理を再開するように、クライアント管理者はAdobe Experience Platformを再設定する必要があります。

## 同時バッチ処理

データ管理者やエンジニアは、特定のデータセットの特性に応じて、データの抽出、変換、読み込みを順次的または同時的に行うことができます。 また、これは、変換されたデータを使用してクライアントがターゲット設定している使用事例にも基づきます。

例えば、クライアントが更新可能な永続性ストアに永続的で、イベントの順序や順序が重要な場合、クライアントは、順次ETL変換を使用してジョブを厳密に処理する必要があります。

また、順番が正しくないデータは、指定したタイムスタンプを使用して内部的に並べ替えられるダウンストリームのアプリケーション/プロセスで処理できます。 このような場合、処理時間を改善するために、並列ETL変換が実行可能な可能性があります。

ソースバッチの場合は、クライアントプリファレンスとコンシューマ制約に再び依存します。 行のリージェンシー/順序に関係なくソースデータを並行して取得できる場合、変換プロセスはより高い並列度（順序の違う処理に基づく最適化）のプロセスバッチを作成できます。 ただし、変換がタイムスタンプや優先順位の変更を優先する必要がある場合は、データアクセスAPIまたはETLツールのスケジューラー/呼び出しによって、可能な限りバッチが正しく処理されないようにする必要があります。

## 延期

繰延とは、入力データがまだ完了しておらず、ダウンストリームプロセスに送信できないが、将来使用できる可能性があるプロセスです。 クライアントは、データを保存期間内の将来の時間にリンチ/ステッチが可能になることを期待し、データを保存し、次の変換の実行時に再処理するという決定を下すために、将来の照合に対するデータ・ウィンドウの個々の許容度と処理コストを判断します。 このサイクルは、行が十分に処理されるか、投資を続行するには古すぎると見なされるまで継続されます。 繰り返しごとに、繰り返しデータが生成されます。繰り返しデータは、前の繰り返しで繰り返されたすべての繰り返しデータのスーパーセットです。

Adobe Experience Platformは、現在、遅延データを識別していないので、クライアントの実装では、ETLとデータセットの手動設定に依存して、遅延データの保持に使用できるソースデータセットをプラットフォームミラーリングで別のデータセットを作成する必要があります。 この場合、遅延データはスナップショット・データに似ています。 ETL変換が実行されるたびに、ソースデータは遅延データと統合され、処理用に送信されます。

## チャンゲロ

| 日付 | アクション | 説明 |
| ---- | ------ | ----------- |
| 2019-01-19 | データセットから「fields」プロパティを削除しました。 | データセットには、以前は、データのコピーを含む「フィールド」プロパティが含まれていましたが、スキーマは この機能は使用しないでください。 「fields」プロパティが見つかった場合は無視し、「oversedSchema」または「schemaRef」を代わりに使用する必要があります。 |
| 2019-03-15 | データセットに追加された「schemaRef」プロパティ | データセットの「schemaRef」プロパティには、データセットのベースとなるXDMスキーマを参照するURIが含まれ、そのデータセットで使用できるすべての潜在的なフィールドを表します。 |
| 2019-03-15 | すべてのエンドユーザ識別子が「identityMap」プロパティにマップされます。 | 「identityMap」は、CRM ID、ECID、忠誠度のプログラムIDなど、サブジェクトのすべての一意の識別子をカプセル化したものです。 このマップは、 [Identity Service](../identity-service/home.md) （IDサービス）で使用され、サブジェクトの既知のIDと匿名IDをすべて解決し、各エンドユーザーのIDグラフを1つ作成します。 |
| 2019-05-30 | EOLとデータセットから「スキーマ」プロパティを削除 | データセット「スキーマ」プロパティは、カタログAPIの非推奨のエンドポイントを使用して、スキーマ `/xdms` への参照リンクを提供しました。 これは、新しいスキーマレジストリAPIで参照されているスキーマの「id」、「version」および「contentType」を提供する「schemaRef」に置き換えられました。 |