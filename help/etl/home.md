---
keywords: Experience Platform;ホーム;人気のトピック;ETL;etl;etl 統合;ETL 統合
solution: Experience Platform
title: Adobe Experience Platform 用 ETL 統合の開発
description: ETL 統合ガイドでは、Experience Platform 用の高パフォーマンスで安全なコネクタを作成し、データを Platform に取得するための一般的な手順について説明しています。
exl-id: 7d29b61c-a061-46f8-a31f-f20e4d725655
source-git-commit: 1a7ba52b48460d77d0b7695aa0ab2d5be127d921
workflow-type: tm+mt
source-wordcount: '4075'
ht-degree: 100%

---

# Adobe Experience Platform 用 ETL 統合の開発

ETL 統合ガイドでは、[!DNL Experience Platform] 用の高パフォーマンスで安全なコネクタを作成し、データを [!DNL Platform] に取り込むための一般的な手順について説明しています。


- [[!DNL Catalog]](https://www.adobe.io/experience-platform-apis/references/catalog/)
- [[!DNL Data Access]](https://www.adobe.io/experience-platform-apis/references/data-access/)
- [[!DNL Data Ingestion]](https://www.adobe.io/experience-platform-apis/references/data-ingestion/)
- [Experience Platform API の認証と承認](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja)
- [[!DNL Schema Registry]](https://www.adobe.io/experience-platform-apis/references/schema-registry/)

このガイドには、ETL コネクタの設計時に使用する API 呼び出しの例も含まれています。また、各 [!DNL Experience Platform] サービスの概要と API の使用について詳しく説明したドキュメントへのリンクも含まれています。

[!DNL GitHub] では、[!DNL Apache] ライセンスバージョン 2.0 の [ETL Ecosystem Integration Reference Code](https://github.com/adobe/acp-data-services-etl-reference)（ETL エコシステム統合リファレンスコード）を使用して、サンプル統合を利用できます。

## ワークフロー

次のワークフロー図は、Adobe Experience Platform コンポーネントと ETL アプリケーションおよびコネクタとの統合に関する概要を示しています。

![](images/etl.png)

## Adobe Experience Platform コンポーネント

ETL コネクタ統合には、複数の Experience Platform コンポーネントが関係しています。次のリストでは、主なコンポーネントと機能の概要を説明します。

- **Adobe Identity Management System（IMS）** — アドビのサービスに対する認証のフレームワークを提供します。
- **IMS 組織** — 製品やサービスを所有またはライセンスし、そのメンバーへのアクセスを許可できる企業エンティティ。
- **IMS ユーザー** - IMS 組織のメンバー。組織とユーザーの関係は多対多です。
- **[!DNL Sandbox]** - デジタルエクスペリエンスアプリケーションの開発と発展を支援する、単一の [!DNL Platform] インスタンスの仮想パーティション。
- **データ検出** - 取得されたデータおよび変換されたデータのメタデータを [!DNL Experience Platform] で記録します。
- **[!DNL Data Access]** - [!DNL Experience Platform] でデータにアクセスするためのインターフェイスをユーザーに提供します。
- **[!DNL Data Ingestion]** - [!DNL Data Ingestion] API を使用してデータを [!DNL Experience Platform] にプッシュします。
- **[!DNL Schema Registry]** - [!DNL Experience Platform] で使用するデータの構造を記述するスキーマを定義し、保存します。

## API を使い始める [!DNL Experience Platform]

次の節では、[!DNL Experience Platform] API の呼び出しを正しくおこなうために知っておく必要がある、または手元に用意しておく必要がある追加情報を提供します。

### API 呼び出し例の読み取り

ここでは、リクエストの形式を説明するために API 呼び出しの例を示します。これには、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、 トラブルシューテングガイドの[API 呼び出し例の読み方](../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください[!DNL Experience Platform]。

### 必須ヘッダーの値の収集

[!DNL Platform] API を呼び出すには、まず[認証チュートリアル](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja)を完了する必要があります。次に示すように、すべての [!DNL Experience Platform] API 呼び出しに必要な各ヘッダーの値は認証チュートリアルで説明されています。

- Authorization： Bearer `{ACCESS_TOKEN}`
- x-api-key： `{API_KEY}`
- x-gw-ims-org-id： `{ORG_ID}`

[!DNL Experience Platform] のすべてのリソースは、特定の仮想サンドボックスに分離されています。[!DNL Platform] API へのすべてのリクエストには、操作がおこなわれるサンドボックスの名前を指定するヘッダーが必要です。

- x-sandbox-name：`{SANDBOX_NAME}`

>[!NOTE]
>
>[!DNL Platform] のサンドボックスについて詳しくは、[サンドボックスの概要に関するドキュメント](../sandboxes/home.md)を参照してください。

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、次のような追加ヘッダーが必要です。

- Content-Type：application/json

## 一般的なユーザーフロー

まず、ETL ユーザーが [!DNL Experience Platform] ユーザーインターフェイス（UI）にログインし、標準のコネクタまたはプッシュサービスコネクタを使用して取得用のデータセットを作成します。

UI で、ユーザーはデータセットスキーマを選択して出力データセットを作成します。どのスキーマを選択するかは、[!DNL Platform] に取り込むデータ（レコードまたは時系列）のタイプによって異なります。UI 内の「スキーマ」タブをクリックすると、スキーマがサポートする動作タイプを含む、使用可能なすべてのスキーマを表示できます。

ETL ツールで適切な接続を設定した後（ユーザーの資格情報を使用して）、マッピング変換のデザインを開始します。ETL ツールには、既に [!DNL Experience Platform] コネクタがインストールされていると想定しています（この統合ガイドでは、プロセスが定義されていません）。

サンプル ETL ツールおよびワークフローのモックアップは、[ETL ワークフロー](./workflow.md)で提供されています。ETL ツールの形式は異なる場合がありますが、ほとんどの場合、類似した機能が公開されています。

>[!NOTE]
>
> ETL コネクタは、データを取得する日付とオフセットを記録するタイムスタンプフィルター（つまり、データを読み取るウィンドウ）を指定する必要があります。ETL ツールは、この UI または別の関連する UI でこれら 2 つのパラメーターを取ることをサポートする必要があります。Adobe Experience Platform では、これらのパラメーターは、使用可能な日付（存在する場合）またはデータセットのバッチオブジェクトに存在する取得日付にマップされます。

### データセットリストの表示

マッピングにデータのソースを使用すると、使用可能なすべてのデータセットのリストを [[!DNL Catalog API]](https://www.adobe.io/experience-platform-apis/references/catalog/) を使用して取得できます。

1 つの API リクエストを発行して、使用可能なすべてのデータセット（`GET /dataSets`）を表示することができます。ベストプラクティスは応答のサイズを制限するクエリパラメーターを含めることです。

完全なデータセット情報がリクエストされている場合、応答ペイロードのサイズが 3GB を超える可能性があり、全体的なパフォーマンスが低下する可能性があります。したがって、クエリパラメーターを使用して必要な情報のみをフィルタリングすると、[!DNL Catalog] クエリの効率が向上します。

#### フィルタリングのリスト

応答をフィルタリングする場合、パラメータをアンパサンド（`&`）で区切ることにより、1 回の呼び出しで複数のフィルタを使用できます）。一部のクエリパラメーターは、値のコンマ区切りリストを受け取ります。例は、以下のリクエスト例の「プロパティ」フィルターです。

[!DNL Catalog] の応答は、設定された制限に従って自動的に計測されますが、「limit」クエリパラメーターを使用して、制約をカスタマイズし、返されるオブジェクトの数を制限できます。事前設定された [!DNL Catalog] 応答の制限は、次のとおりです。

- Limit パラメーターを指定しない場合、応答ペイロードあたりのオブジェクトの最大数は 20 です。
- その他のすべての [!DNL Catalog] クエリのグローバル制限は 100 オブジェクトです。
- データセットクエリの場合、properties クエリーパラメーターを使用して observableSchema がリクエストされた場合、返されるデータセットの最大数は 20 です。
- 無効な limit パラメーター（`limit=0` を含む）が満たされると、適切な範囲を示す HTTP 400 エラーが発生します。
- 制限またはオフセットがクエリパラメータとして渡された場合、それらはヘッダーとして渡されたものよりも優先されます。

クエリパラメーターについて詳しくは、「[カタログサービスの概要](../catalog/home.md)」を参照してください。

**API 形式**

```http
GET /catalog/dataSets
GET /catalog/dataSets?{filter1}={value1},{value2}&{filter2}={value3}
```

**リクエスト**

```shell
curl -X GET "https://platform.adobe.io/data/foundation/catalog/dataSets?limit=3&properties=name,description,schemaRef" \
  -H "Authorization: Bearer {ACCESS_TOKEN}" \
  -H "x-api-key: {API_KEY}" \
  -H "x-gw-ims-org-id: {ORG_ID}" \
  -H "x-sandbox-name: {SANDBOX_NAME}"
```

[[!DNL Catalog API]](https://www.adobe.io/experience-platform-apis/references/catalog/) を呼び出す方法を示す詳細な例については、[Catalog Service の概要](../catalog/home.md)を参照してください。

**応答** 

応答には、`properties` クエリパラメータで示されるように、「name」、「description」および「schemaRef」を示す 3 つ（`limit=3`）のデータセットが含まれます。

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

### データセットスキーマの表示

データセットの「schemaRef」プロパティには、データセットの基となる XDM スキーマを参照する URI が含まれます。XDM スキーマ（「schemaRef」）は、データセットで使用できる可能性のあるすべてのフィールドを表し、必ずしも使用されているフィールドとは限りません（以下の「observableSchema」を参照）。

XDM スキーマは、書き込み可能なすべての使用可能なフィールドのリストをユーザーに提示する必要がある場合に使用するスキーマです。

前の応答オブジェクト（`https://ns.adobe.com/{TENANT_ID}/schemas/274f17bc5807ff307a046bab1489fb18`）の最初の「schemaRef.id」値は、[!DNL Schema Registry] 内の特定の XDM スキーマを指す URI です。このスキーマは、[!DNL Schema Registry] API に対して検索（GET）リクエストを実行することで取得できます。

>[!NOTE]
>
> 「schemaRef」プロパティは、現在廃止されている「schema」プロパティを置き換えます。「schemaRef」がデータセットにない場合、または値が含まれていない場合は、「schema」プロパティが存在するかどうかを確認する必要があります。これは、前の呼び出しの `properties` クエリパラメーターで「schemaRef」を「schema」に置き換えることで実行できます。「schema」プロパティの詳細については、次の「[データセット「schema」プロパティ](#dataset-schema-property-deprecated---eol-2019-05-30)」の節を参照してください。

**API 形式**

```http
GET /schemaregistry/tenant/schemas/{url encoded schemaRef.id}
```

**リクエスト**

リクエストは、スキーマの URL エンコードされた `id` URI（「schemaRef.id」属性の値）を使用し、Accept ヘッダーが必要です。

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/schemaregistry/tenant/schemas/https%3A%2F%2Fns.adobe.com%2F{TENANT_ID}%2Fschemas%2F274f17bc5807ff307a046bab1489fb18 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Accept: application/vnd.adobe.xed-full+json; version=1' \
```

応答の形式は、リクエストで送信される Accept ヘッダーのタイプによって異なります。また、検索リクエストの Accept ヘッダーには `version` を含める必要があります。次の表に、検索に使用可能な Accept ヘッダーの概要を示します。

| Accept | 説明 |
| ------ | ----------- |
| `application/vnd.adobe.xed-id+json` | リクエスト、タイトル、ID、バージョンをリスト（GET）する |
| `application/vnd.adobe.xed-full+json; version={major version}` | $refs および allOf を解決、タイトルと説明を含む |
| `application/vnd.adobe.xed+json; version={major version}` | $Ref および allOf で生、タイトルと説明を含む |
| `application/vnd.adobe.xed-notext+json; version={major version}` | $Ref および allOf で生、タイトルや説明なし |
| `application/vnd.adobe.xed-full-notext+json; version={major version}` | $refs および allOf を解決、タイトルや説明なし |
| `application/vnd.adobe.xed-full-desc+json; version={major version}` | $refs および allOf を解決、記述子を含む |

>[!NOTE]
>
>`application/vnd.adobe.xed-id+json`、、`application/vnd.adobe.xed-full+json; version={major version}`は最もよく使用される Accept ヘッダーです。`application/vnd.adobe.xed-id+json` は、「title」、「id」、および「version」のみを返すので、[!DNL Schema Registry] のリソースをリストする場合に推奨されます。`application/vnd.adobe.xed-full+json; version={major version}` は、すべてのフィールド（「プロパティ」の下にネスト）とタイトルおよび説明を返すので、特定のリソースを（「id」で）表示する場合に推奨されます。

**応答** 

返される JSON スキーマは、JSON としてシリアル化されたデータの構造とフィールドレベルの情報（「type」、「format」、「minimum」、「maximum」など）を示します。取得に JSON 以外のシリアル化形式（Parquet や Scala など）を使用する場合、『[スキーマレジストリガイド](../xdm/tutorials/create-schema-api.md)』には、必要な JSON タイプ（「meta:xdmType」）と、その対応する他の形式での表現を示す表が含まれます。

[!DNL Schema Registry] デベロッパーガイドには、この表以外にも、[!DNL Schema Registry] API を使用して実行できるすべての呼び出しの詳細な例が記載されています。

### データセットの「schema」プロパティ（廃止 — EOL 2019/05/30（PT））

データセットには、現在は廃止されいるが後方互換性を確保するために一時的に使用可能なままになっている、「schema」プロパティが含まれている場合があります。例えば、以前に作成したリスト（GET）リクエストと同様のリスト（`properties` クエリパラメーターで「schema」が「schemaRef」に置き換えられた場合）は、次のように返されます。

```json
{
  "5ba9452f7de80400007fc52a": {
    "name": "Sample Dataset 1",
    "description": "Description of Sample Dataset 1.",
    "schema": "@/xdms/context/person"
  }
}
```

データセットの「schema」プロパティに値が入力された場合、これはスキーマが非推奨の `/xdms` スキーマであることを示します。また、サポートされている場合、ETL コネクタは `/xdms` エンドポイント（[[!DNL Catalog API]](https://www.adobe.io/experience-platform-apis/references/catalog/) の非推奨のエンドポイント）の「schema」プロパティの値を使用してレガシースキーマを取得します。

**API 形式**

```http
GET /catalog/{"schema" property without the "@"}
```

**リクエスト**

```shell
curl -X GET "https://platform.adobe.io/data/foundation/catalog/xdms/context/person?expansion=xdm" \
  -H "x-gw-ims-org-id: {ORG_ID}" \
  -H "x-sandbox-name: {SANDBOX_NAME}" \
  -H "Authorization: Bearer {ACCESS_TOKEN}" \
  -H "x-api-key: {API_KEY}"
```

>[!NOTE]
>
> オプションの `expansion=xdm` クエリパラメーターは、参照されているスキーマを完全に展開してインライン化するように API に指示します。この操作は、すべての潜在的なフィールドのリストをユーザーに表示する場合に行います。

**応答** 

[データセットのスキーマを表示](#view-dataset-schema)する手順と同様、応答には、JSON としてシリアル化されたデータの構造とフィールドレベルの情報を記述した JSON スキーマが含まれます。

>[!NOTE]
>
>「schema」フィールドが空の場合や完全に存在しない場合は、コネクタは「schemaRef」フィールドを読み取り、前の手順で示した[スキーマレジストリ API](https://www.adobe.io/experience-platform-apis/references/schema-registry/) を使用して、[データセットスキーマを表示](#view-dataset-schema)する必要があります。

### 「observableSchema」プロパティ

データセットの「observableSchema」プロパティの JSON 構造は、XDM スキーマ JSON と一致します。「observableSchema」には、受信入力ファイルに存在するフィールドが含まれています。[!DNL Experience Platform] にデータを書き込む場合、ターゲットスキーマのすべてのフィールドを使用する必要はありません。代わりに、使用されているフィールドのみを指定する必要があります。

観察可能なスキーマとは、データを読み取る場合や、読み取りやマップに使用できるフィールドのリストを示す場合に使用するスキーマです。

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

### データのプレビュー

ETL アプリケーションは、プレビューデータの機能を提供することができます（[ETL ワークフローの図 8](./workflow.md)）。データアクセス API には、データをプレビューするオプションが複数があります。

データアクセス API を使用してデータをプレビューする手順説明など、追加の情報については、「[データアクセスチュートリアル](../data-access/tutorials/dataset-data.md)」を参照してください。

### 「properties」パラメーターを使用したデータセット詳細のクエリ

上記の手順で[データセットのリストを表示](#view-list-of-datasets)するように、「properties」クエリパラメータを使用して「files」をリクエストできます。

データセットのクエリと使用可能な応答フィルターについて詳しくは、「[カタログサービスの概要](../catalog/home.md)」を参照してください。

**API 形式**

```http
GET /catalog/dataSets?limit={value}&properties={value}
```

**リクエスト**

```shell
curl -X GET "https://platform.adobe.io/data/foundation/catalog/dataSets?limit=1&properties=files" \
  -H "Authorization: Bearer {ACCESS_TOKEN}" \
  -H "x-api-key: {API_KEY}" \
  -H "x-gw-ims-org-id: {ORG_ID}" \
  -H "x-sandbox-name: {SANDBOX_NAME}"
```

**応答** 

この応答には、「files」プロパティを示す 1 つのデータセット（`limit=1`）が含まれます。

```json
{
  "5bf479a6a8c862000050e3c7": {
    "files": "@/dataSets/5bf479a6a8c862000050e3c7/views/5bf479a654f52014cfffe7f1/files"
  }
}
```

### 「files」属性を使用したデータセットファイルのリスト

GET リクエストを使用して、「files」属性を使用してファイルの詳細を取り込むこともできます。

**API 形式**

```http
GET /catalog/dataSets/{DATASET_ID}/views/{VIEW_ID}/files
```

**リクエスト**

```shell
curl -X GET "https://platform.adobe.io/data/foundation/catalog/dataSets/5bf479a6a8c862000050e3c7/views/5bf479a654f52014cfffe7f1/files" \
  -H "Accept: application/json" \
  -H "x-gw-ims-org-id: {ORG_ID}" \
  -H "x-sandbox-name: {SANDBOX_NAME}" \
  -H "Authorization: Bearer {ACCESS_TOKEN}" \
  -H "x-api-key: {API_KEY}"
```

**応答** 

この応答には、データセットファイル ID が最上位のプロパティとして含まれ、ファイルの詳細はデータセットファイル ID オブジェクトに含まれます。

```json
{
    "194e89b976494c9c8113b968c27c1472-1": {
        "batchId": "194e89b976494c9c8113b968c27c1472",
        "dataSetViewId": "5bf479a654f52014cfffe7f1",
        "imsOrg": "{ORG_ID}",
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
        "imsOrg": "{ORG_ID}",
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
        "imsOrg": "{ORG_ID}",
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

前の応答で返されたデータセットファイル ID を GET リクエストで使用し、[!DNL Data Access] API を介してファイルの詳細を取得できます。

[データアクセスの概要](../data-access/home.md)には、[!DNL Data Access] API の使用方法の詳細が含まれています。

**API 形式**

```http
GET /export/files/{DATASET_FILE_ID}
```

**リクエスト**

```shell
curl -X GET "https://platform.adobe.io/data/foundation/export/files/ea40946ac03140ec8ac4f25da360620a-1" \
  -H "x-gw-ims-org-id: {ORG_ID}" \
  -H "x-sandbox-name: {SANDBOX_NAME}" \
  -H "Authorization: Bearer {ACCESS_TOKEN}" \
  -H "x-api-key: {API_KEY}"
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

### ファイルデータのプレビュー

「href」プロパティは、[[!DNL Data Access API]](../data-access/home.md) を介してプレビューデータを取得するために使用できます。

**API 形式**

```http
GET /export/files/{FILE_ID}?path={FILE_NAME}.{FILE_FORMAT}
```

**リクエスト**

```shell
curl -X GET "https://platform.adobe.io/data/foundation/export/files/ea40946ac03140ec8ac4f25da360620a-1?path=samplefile.parquet" \
  -H "x-gw-ims-org-id: {ORG_ID}" \
  -H "x-sandbox-name: {SANDBOX_NAME}" \
  -H "Authorization: Bearer {ACCESS_TOKEN}" \
  -H "x-api-key: {API_KEY}"
```

上記のリクエストに対する応答には、ファイルの内容のプレビューが含まれます。

詳細なリクエストや応答を含む、[!DNL Data Access] API に関する詳細については、「[データアクセスの概要](../data-access/home.md)」を参照してください。

### データセットからの「fileDescription」の取得

変換後のデータの出力として、変換先のコンポーネントを使用して、データエンジニアが出力データセットを選択します（[ETL ワークフローの図 12](workflow.md)）。XDM スキーマは、出力データセットに関連付けられます。書き込まれるデータは、Data Discovery API のデータセットエンティティの「fileDescription」属性で識別されます。この情報は、データセット ID（`{DATASET_ID}`）を使用して取得できます。JSON 応答の「fileDescription」プロパティが、リクエストされた情報を提供します。

**API 形式**

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
-H "x-gw-ims-org-id: {ORG_ID}" \
-H "x-sandbox-name: {SANDBOX_NAME}" \
-H "Authorization: Bearer {ACCESS_TOKEN}" \
-H "x-api-key: {API_KEY}"
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

データは、[データ取得 API ](https://www.adobe.io/experience-platform-apis/references/data-ingestion/)を使用して [!DNL Experience Platform] に書き込まれます。  データの書き込みは非同期的なプロセスです。データが Adobe Experience Platform に書き込まれると、データが完全に書き込まれた後でのみ、バッチが作成され、成功に指定されます。

[!DNL Experience Platform] のデータは、Parquet ファイル形式で書き込む必要があります。

## 実行段階

実行を開始すると、コネクタ（ソースコンポーネントで定義）は、[[!DNL Data Access API]](https://www.adobe.io/experience-platform-apis/references/data-access/) を使用して [!DNL Experience Platform] からデータを読み取ります。変換プロセスは、特定の時間範囲のデータを読み取ります。内部的には、ソースデータセットのバッチをクエリします。クエリ中に、パラメーター化された（時系列データの場合はローリング、または増分データ）開始日を使用し、それらのバッチのデータセットファイルをリストし、それらのデータセットファイルのデータのリクエストを開始します。

### 変換の例

[ETL 変換例](./transformations.md)ドキュメントには、ID 処理やデータタイプマッピングなど、多数の変換例が含まれています。これらの変換を参考にしてください。

### [!DNL Experience Platform] からのデータの読み取り

[[!DNL Catalog API]](https://www.adobe.io/experience-platform-apis/references/catalog/) を使用すると、指定した開始時間と終了時間の間にあるすべてのバッチを取得し、作成順に並べ替えることができます。

**リクエスト**

```shell
curl -X GET "https://platform.adobe.io/data/foundation/catalog/batches?dataSet=DATASETID&createdAfter=START_TIMESTAMP&createdBefore=END_TIMESTAMP&sort=desc:created" \
  -H "Accept: application/json" \
  -H "Authorization:Bearer {ACCESS_TOKEN}" \
  -H "x-api-key: {API_KEY}" \
  -H "x-gw-ims-org-id: {ORG_ID}" \
  -H "x-sandbox-name: {SANDBOX_NAME}"
```

バッチのフィルタリングについて詳しくは、[データアクセスのチュートリアル](../data-access/tutorials/dataset-data.md)を参照してください。

### バッチからのファイルの取得

探しているバッチの ID を取得したら（`{BATCH_ID}`）、[[!DNL Data Access API]](https://www.adobe.io/experience-platform-apis/references/data-access/) を使用して、特定のバッチに属するファイルのリストを取得できます。詳しくは、[[!DNL Data Access] チュートリアル](../data-access/tutorials/dataset-data.md)を参照してください。

**リクエスト**

```shell
curl -X GET "https://platform.adobe.io/data/foundation/export/batches/{BATCH_ID}/files" \
  -H "x-gw-ims-org-id: {ORG_ID}" \
  -H "x-sandbox-name: {SANDBOX_NAME}" \
  -H "Authorization: Bearer {ACCESS_TOKEN}" \
  -H "x-api-key: {API_KEY}"
```

### ファイル ID を使用したファイルへのアクセス

ファイルの一意の ID（`{FILE_ID`）と [[!DNL Data Access API]](https://www.adobe.io/experience-platform-apis/references/data-access/) を使用して、ファイルの名前、バイト単位のサイズ、ダウンロードリンクなど、ファイルの特定の詳細にアクセスできます。

**リクエスト**

```shell
curl -X GET "https://platform.adobe.io/data/foundation/export/files/{FILE_ID}" \
  -H "Authorization: Bearer {ACCESS_TOKEN}" \
  -H "x-gw-ims-org-id: {ORG_ID}" \
  -H "x-sandbox-name: {SANDBOX_NAME}" \
  -H "x-api-key: {API_KEY}"
```

応答は単一のファイルまたはディレクトリを指す場合があります。各項目について詳しくは、[[!DNL Data Access] チュートリアル](../data-access/tutorials/dataset-data.md)を参照してください。

### ファイルコンテンツへのアクセス

[[!DNL Data Access API]](https://www.adobe.io/experience-platform-apis/references/data-access/) は、特定のファイルのコンテンツにアクセスするために使用できます。コンテンツを取り込むには、ファイル ID を用いてファイルにアクセスする際に `_links.self.href` に返された値を用いて GET リクエストを実行します。

**リクエスト**

```shell
curl -X GET "https://platform.adobe.io/data/foundation/export/files/{DATASET_FILE_ID}?path=filename1.csv" \
  -H "Authorization: Bearer {ACCESS_TOKEN}" \
  -H "x-gw-ims-org-id: {ORG_ID}" \
  -H "x-sandbox-name: {SANDBOX_NAME}" \
  -H "x-api-key: {API_KEY}"
```

このリクエストに対する応答には、ファイルのコンテンツが含まれます。応答のページ番号付けの詳細を含め、詳しくは、「[データアクセス API を使用してデータをクエリする方法](../data-access/tutorials/dataset-data.md)」チュートリアルを参照してください。

### レコードのコンプライアンススキーマの検証

データの書き込み中に、XDM スキーマで定義された検証ルールに従ってデータの検証を選択できます。スキーマの検証について詳しくは、[ [!DNL GitHub] の ETL エコシステム統合リファレンスコード](https://github.com/adobe/experience-platform-etl-reference/blob/fd08dd9f74ae45b849d5482f645f859f330c1951/README.md#validation)を参照してください。

[[!DNL GitHub]](https://github.com/adobe/experience-platform-etl-reference/blob/fd08dd9f74ae45b849d5482f645f859f330c1951/README.md) にある参照実装を使用している場合は、`-DenableSchemaValidation=true` システムプロパティを使用して、この実装のスキーマ検証を有効にできます。

検証は、`minLength` および `maxlength` （文字列用）、`minimum` および `maximum` （整数用）などの属性のような属性を使用して、論理 XDM 型に対して実行できます。『[スキーマレジストリ API 開発者ガイド](../xdm/api/getting-started.md)』には、XDM の種類と検証に使用できるプロパティの概要を示す表が含まれています。

>[!NOTE]
>
> 様々な `integer` 型に対して提供される最小値と最大値は、型がサポートする MIN 値と MAX 値ですが、これらの値は、選択した最小値と最大値にさらに制限できます。

### バッチの作成

データが処理されると、ETL ツールは、[バッチ取得 API](https://www.adobe.io/experience-platform-apis/references/data-ingestion/) を使用してデータを [!DNL Experience Platform] に書き戻します。データセットにデータを追加する前に、そのデータをバッチにリンクし、後で特定のデータセットにアップロードする必要があります。

**リクエスト**

```SHELL
curl -X POST "https://platform.adobe.io/data/foundation/import/batches" \
  -H "accept: application/json" \
  -H "x-gw-ims-org-id: {ORG_ID}" \
  -H "x-sandbox-name: {SANDBOX_NAME}" \
  -H "Authorization: Bearer {ACCESS_TOKEN}" \
  -H "x-api-key: {API_KEY}" \
  -d '{
        "datasetId":"{DATASET_ID}"
      }'
```

リクエスト例や応答例を含む、バッチの作成に関する詳細は、「[バッチ取得の概要](../ingestion/batch-ingestion/overview.md)」を参照してください。

### データセットへの書き込み

新しいバッチが正常に作成されたら、ファイルを特定のデータセットにアップロードできます。複数のファイルを 1 つのバッチで投稿して、プロモーションすることができます。ファイルは、小ファイルアップロード API を使用してアップロードできます。ただし、ファイルが大きすぎてゲートウェイの制限を超えている場合は、大ファイルアップロード API を使用できます。小ファイルアップロードと大ファイルアップロードの両方の使用について詳しくは、「[バッチ取得の概要](../ingestion/batch-ingestion/overview.md)」を参照してください。

**リクエスト**

[!DNL Experience Platform] のデータは、Parquet ファイル形式で書き込む必要があります。

```shell
curl -X PUT "https://platform.adobe.io/data/foundation/import/batches/{BATCH_ID}/dataSets/{DATASET_ID}/files/{FILE_NAME}.parquet" \
  -H "accept: application/json" \
  -H "x-gw-ims-org-id:{ORG_ID}" \
  -H "Authorization:Bearer ACCESS_TOKEN" \
  -H "x-api-key: API_KEY" \
  -H "content-type: application/octet-stream" \
  --data-binary "@{FILE_PATH_AND_NAME}.parquet"
```

### バッチアップロード完了のマーク

すべてのファイルがバッチにアップロードされたら、バッチの完了を通知できます。これにより、完成したファイルに対して [!DNL Catalog] の「DataSetFile」エントリが作成され、生成バッチに関連付けられます。これにより、[!DNL Catalog] のバッチが成功とマークされ、ダウンストリームフローがトリガーされて使用可能なデータを取り込みます。

データは、最初に Adobe Experience Platform のステージング場所に配置され、次に、カタログ化と検証の後、最終的な場所に移動されます。すべてのデータが永続的な場所に移動されると、バッチは成功に指定されます。

**リクエスト**

```shell
curl -X POST "https://platform.adobe.io/data/foundation/import/batches/{BATCH_ID}?action=COMPLETE" \
  -H "x-gw-ims-org-id: {ORG_ID}" \
  -H "x-sandbox-name: {SANDBOX_NAME}" \
  -H "Authorization:Bearer {ACCESS_TOKEN}" \
  -H "x-api-key: {API_KEY}"
```

成功した場合、応答は HTTP ステータス 200 OK を返し、応答本文は空になります。

ETL ツールは、データの読み取り時に、ソースデータセットのタイムスタンプを必ずメモします。

次回の変換の実行時（スケジュールやイベントの呼び出しによる場合など）、ETL は、以前に保存したタイムスタンプと今後のすべてのデータからデータをリクエストする開始を行います。

### 最後のバッチ状態の取得

ETL ツールで新しいタスクを実行する前に、最後のバッチが正常に完了したことを確認する必要があります。[[!DNL Catalog Service API]](https://www.adobe.io/experience-platform-apis/references/catalog/) は、関連するバッチの詳細を提供するバッチ固有のオプションを提供します。

**リクエスト**

```shell
curl -X GET "https://platform.adobe.io/data/foundation/catalog/batches?limit=1&sort=desc:created" \
  -H "Accept: application/json" \
  -H "x-gw-ims-org-id: {ORG_ID}" \
  -H "x-sandbox-name: {SANDBOX_NAME}" \
  -H "Authorization: Bearer {ACCESS_TOKEN}" \
  -H "x-api-key: {API_KEY}"
```

**応答** 

新しいタスクは、以下に示すように、以前のバッチ「status」の値が「success」の場合にスケジュールできます。

```json
"{BATCH_ID}": {
    "imsOrg": "{ORG_ID}",
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

### ID 別の最後のバッチステータスの取得

個々のバッチステータスは、`{BATCH_ID}` を使用して GET リクエストを発行することで、[[!DNL Catalog Service API]](https://www.adobe.io/experience-platform-apis/references/catalog/) を介して取得できます。使用される `{BATCH_ID}` は、バッチの作成時に返された ID と同じです。

**リクエスト**

```shell
curl -X GET "https://platform.adobe.io/data/foundation/catalog/batches/{BATCH_ID}" \
  -H "Accept: application/json" \
  -H "x-gw-ims-org-id: {ORG_ID}" \
  -H "x-sandbox-name: {SANDBOX_NAME}" \
  -H "Authorization: Bearer {ACCESS_TOKEN}" \
  -H "x-api-key: {API_KEY}"
```

**応答 — 成功**

次の応答は、「成功」を示しています。

```json
"{BATCH_ID}": {
    "imsOrg": "{ORG_ID}",
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

障害が発生した場合、「エラー」は応答から抽出され、ETL ツールでエラーメッセージとして表示されます。

```json
"{BATCH_ID}": {
    "imsOrg": "{ORG_ID}",
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

## 増分データ vs スナップショットデータとイベント vs プロファイル

データは、次のように 2 つの行列で表すことができます。

| 増分イベント | 増分プロファイル |
|-------------------------------|----------------------|
| スナップショットイベント（あまり考えられません） | スナップショットプロファイル |

イベントデータは、通常、各行にインデックス付きのタイムスタンプ列がある場合に使用されます。

プロファイルデータは、通常、データにタイムスタンプがなく、各行を主キー／複合キーで識別できる場合に使用します。

増分データとは、新しい/更新されたデータのみがシステムに取り込まれ、データセット内の現在のデータに追加される場所です。

スナップショットデータとは、すべてのデータがシステムに取り込まれ、データセット内の以前のデータの一部またはすべてを置き換えるときです。

増分イベントの場合、ETL ツールは、バッチエンティティの使用可能な日付／作成日を使用する必要があります。プッシュサービスの場合、利用可能な日付は存在しないので、増分をマークするのにバッチ作成日／更新日が使用されます。増分イベントのバッチはすべて処理する必要があります。

増分プロファイルの場合、ETL ツールはバッチエンティティの作成日／更新日を使用します。一般に、増分データのすべてのプロファイルデータを処理する必要があります。

スナップショットイベントは、データの大きさにより、非常に低い確率で発生します。ただし、これが必要な場合は、ETL ツールは最後のバッチのみを選択して処理する必要があります。

スナップショットプロファイルを使用する場合、ETL ツールは、システムに到着した最後のデータバッチを選択する必要があります。ただし、変更のバージョンをトラックする必要がある場合は、すべてのバッチを処理する必要があります。ETL プロセス内の重複除外処理は、ストレージコストの管理に役立ちます。

## バッチ再生とデータ再処理

過去「n」日間、ETL 処理されたデータが予期したとおりに発生しなかった場合や、ソースデータ自体が正しくなかった場合は、バッチ再生とデータ再処理が必要になる場合があります。

これをおこなうには、クライアントのデータ管理者が [!DNL Platform] UI を使用して、破損したデータを含むバッチを削除します。その後、ETL を再実行するのに、正しいデータを再入力する必要が生じます。ソース自体に破損したデータがあった場合、データエンジニアまたは管理者は、ソースバッチを修正し、データを（Adobe Experience Platform または ETL コネクタを介して）再度取得する必要があります。

生成されるデータのタイプに基づいて、特定のデータセットから 1 つのバッチを削除するかすべてのバッチを削除するかは、データエンジニアの選択です。データは、[!DNL Experience Platform] のガイドラインに従って削除またはアーカイブされます。

データを削除する ETL 機能が重要になる可能性が高いシナリオです。

削除が完了したら、クライアント管理者は、バッチが削除された時点からコアサービスの処理を再開するように Adobe Experience Platform を再設定する必要があります。

## 同時バッチ処理

クライアントの裁量により、データ管理者やエンジニアは、特定のデータセットの特性に応じて、データの抽出、変換、読み込みを順次または同時の方法でおこなうことができます。また、これは、変換されたデータを使用してクライアントがターゲット設定している使用事例にも基づきます。

例えば、クライアントが更新可能な永続ストアに永続化していて、イベントのシーケンスまたは順序が重要な場合、クライアントは順次 ETL 変換を使用してジョブを厳密に処理する必要がある場合があります。

他の場合では、指定されたタイムスタンプを使用して内部的に並び替えるダウンストリームアプリケーションやプロセスによって、順序が狂ったデータを処理できます。このような場合、処理時間を改善するために、並列 ETL 変換を実行できる可能性があります。

ソースバッチの場合は、クライアントの好みと消費者の制約に再び依存します。行のリージェンシー／順序に関係なくソースデータを並行して取得できる場合、変換プロセスはより高い並列度（順序の違う処理に基づく最適化）のプロセスバッチを作成できます。ただし、変換がタイムスタンプや優先順位の変更を優先する必要がある場合は、データアクセス API または ETL ツールのスケジューラー／呼び出しは、バッチが順不同で処理されないようにする必要があります。

## 遅延

遅延とは、入力データがまだ完了しておらず、下流プロセスに送信できないが、将来使用できる可能性があるプロセスです。クライアントは、将来の照合のためのデータウィンドウ処理に対する個々の許容値と処理コストを決定し、データを脇に置いて次の変換実行で再処理する決定を通知し、保持期間内の将来のある時点でデータを強化および調整／ステッチできることを期待します。このサイクルは、行が十分に処理されるか、投資を続行するには古すぎると見なされるまで継続されます。すべての反復は、前の反復でのすべての遅延データのスーパーセットである遅延データを生成します。

Adobe Experience Platform は、現在、遅延データを識別していません。そのため、クライアント実装では、ETL およびデータセットの手動構成により、[!DNL Platform] で、遅延データの保持に使用できるソースデータセットをミラーリングする、別のデータセットを作成する必要があります。この場合、遅延データはスナップショットデータに似ています。ETL 変換が実行されるたびに、ソースデータは遅延データと統合され、処理用に送信されます。

## 変更ログ

| 日付 | アクション | 説明 |
| ---- | ------ | ----------- |
| 2019-01-19 | データセットから「fields」プロパティを削除しました。 | データセットには、以前は、スキーマのコピーを含む「fields」プロパティが含まれていました。この機能は使用しないでください。「fields」プロパティが見つかった場合は無視し、「observedSchema」または「schemaRef」を代わりに使用する必要があります。 |
| 2019/03/15（PT） | 「schemaRef」プロパティをデータセットに追加しました。 | データセットの「schemaRef」プロパティには、データセットの基となる XDM スキーマを参照する URI が含まれ、そのデータセットで使用できるすべての潜在的なフィールドを表します。 |
| 2019/03/15（PT） | すべてのエンドユーザー識別子が「identityMap」プロパティにマップされます。 | 「identityMap」は、CRM ID、ECID、ロイヤリティプログラム ID など、主体のすべての一意の ID をカプセル化したものです。このマップは、[[!DNL Identity Service]](../identity-service/home.md) で使用され、サブジェクトの既知の ID と匿名 ID をすべて解決し、各エンドユーザーの ID グラフを 1 つ作成します。 |
| 2019/05/30（PT） | データセットから「schema」プロパティを EOL と削除しました。 | データセットの「schema」プロパティは、[!DNL Catalog] API の廃止済み `/xdms` エンドポイントを使用して、スキーマへの参照リンクを提供しました。これは、スキーマの「id」、「version」、「contentType」を提供する「schemaRef」に置き換えられ、新しい [!DNL Schema Registry] API で参照されます。 |
