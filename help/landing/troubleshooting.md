---
keywords: Experience Platform;home;popular topics;API error codes;API error code;error code API;error codes API;API request error;API troubleshooting;API error
solution: Experience Platform
title: Adobe Experience Platform に関する FAQ とトラブルシューティングガイド
description: よくある質問への回答、および Experience Platform の一般的なエラーのトラブルシューティングに関するガイドを見つけます。
landing-page-description: Find answers to frequently asked questions and a guide for troubleshooting common errors in Experience Platform.
topic: getting started
type: Documentation
translation-type: tm+mt
source-git-commit: 72f60ef80a23f5ca4e70147ee6aa6027028fefd0
workflow-type: tm+mt
source-wordcount: '1954'
ht-degree: 76%

---


# [!DNL Platform] FAQとトラブルシューティングガイド

This document provides answers to frequently asked questions about Adobe Experience Platform, as well as a high-level troubleshooting guide for common errors that may be encountered in any [!DNL Experience Platform] API. For troubleshooting guides on individual [!DNL Platform] services, see the [service troubleshooting directory](#service-troubleshooting-directory) below.

## FAQ {#faq}

次に、Adobe Experience Platform に関するよくある質問に対する回答をリストします。

## What are [!DNL Experience Platform] APIs? {#what-are-experience-platform-apis}

[!DNL Experience Platform] オファーは、HTTP要求を使用してリ [!DNL Platform] ソースにアクセスする複数のRESTful APIを提供します。 これらのサービス API は、それぞれ複数のエンドポイントを公開し、リスト（GET）、ルックアップ（GET）、編集（PUT または PATCH）および削除（DELETE）リソースに対する操作を実行できます。各サービスで使用できる特定のエンドポイントと操作について詳しくは、Adobe I/O の [API リファレンスドキュメント](http://www.adobe.com/go/platform-api-reference-en)を参照してください。

## API リクエストの形式を設定する方法を教えてください。 {#how-do-i-format-an-api-request}

Request formats vary depending on the [!DNL Platform] API being used. The best way to learn how to structure your API calls is by following along with the examples provided in the documentation for the particular [!DNL Platform] service you are using.

### API 呼び出し例の読み取り

The documentation for [!DNL Experience Platform] shows example API calls in two different ways. まず、呼び出しは **API 形式**&#x200B;で表されます。これは、操作（GET、POST、PUT、PATCH、DELETE など）と使用中のエンドポイント（例えば、`/global/classes`）のみを示すテンプレート表現です。また、テンプレートには、`GET /{VARIABLE}/classes/{ANOTHER_VARIABLE}` などの呼び出しの作成方法を示すために、変数の位置を示すものもあります。

その後、呼び出しは、**リクエスト**&#x200B;内の cURL コマンドとして表示されます。これには、API とのやり取りに必要なヘッダーと完全な「ベースパス」が含まれます。ベースパスは、すべてのエンドポイントの前に追加する必要があります。例えば、前述の `/global/classes` エンドポイントは `https://platform.adobe.io/data/foundation/schemaregistry/global/classes` になります。API 形式とリクエストパターンはドキュメントを通して示されています。独自で Platform API を呼び出す場合は、例のリクエストに示す完全なパスを使用する必要があります。

### API リクエストの例

以下は、ドキュメントで使用される形式を示す API リクエストの例です。

**API 形式**

API 形式は、操作（GET）と使用されているエンドポイントを示します。変数は中括弧で示されます（この場合は `{CONTAINER_ID}`）。

```http
GET /{CONTAINER_ID}/classes
```

**リクエスト**

この例のリクエストでは、API 形式の変数には、リクエストパス内の実際の値が与えられます。必要なすべてのヘッダーと、サンプルのヘッダー値、または機密情報（セキュリティトークンやアクセス ID など）を含めるための変数が表示されます。

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/schemaregistry/global/classes \
  -H 'Accept: application/vnd.adobe.xed-id+json' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答** 

この応答は、送信されたリクエストに基づいて、API の呼び出しが成功した後に何を受け取るかを示します。場合によっては、応答がスペースを節約するために切り捨てられいるため、サンプルに表示されている情報に加えて他の情報が表示されることがあります。

```json
{
    "results": [
        {
            "title": "XDM ExperienceEvent",
            "$id": "https://ns.adobe.com/xdm/context/experienceevent",
            "meta:altId": "_xdm.context.experienceevent",
            "version": "1"
        },
        {
            "title": "XDM Individual Profile",
            "$id": "https://ns.adobe.com/xdm/context/profile",
            "meta:altId": "_xdm.context.profile",
            "version": "1"
        }
    ],
    "_links": {}
}
```

必要なヘッダーやリクエスト本文を含む、Platform API の特定のエンドポイントについて詳しくは、[API リファレンスのドキュメント](http://www.adobe.com/go/platform-api-reference-en)を参照してください。

## IMS 組織とは何ですか。 {#what-is-my-ims-organization}

IMS 組織は、顧客のアドビ代表です。ライセンスを取得したアドビのソリューションは、この顧客組織に統合されます。When an IMS organization is entitled to [!DNL Experience Platform], it can assign access to developers. IMS 組織 ID（`x-gw-ims-org-id`）は、API 呼び出しを実行する必要がある組織を表すもので、すべての API リクエストのヘッダーとして必要です。This ID can be found through the [Adobe Developer Console](https://www.adobe.com/go/devs_console_ui): in the **Integrations** tab, navigate to the **Overview** section for any particular integration to find the ID under **Client Credentials**. For a step-by-step walkthrough of how to authenticate into [!DNL Platform], see the [authentication tutorial](http://www.adobe.com/go/platform-api-authentication-en).

## API キーはどこで入手できますか？ {#where-can-i-find-my-api-key}

API キーは、すべての API リクエストのヘッダーとして必要です。It can be found through the [Adobe Developer Console](https://www.adobe.com/go/devs_console_ui). コンソールの「**統合**」タブで、特定の統合の「**概要**」セクションに移動すると、「**クライアント資格情報**」の下にキーが表示されます。For a step-by-step walkthrough of how to authenticate to [!DNL Platform], see the [authentication tutorial](http://www.adobe.com/go/platform-api-authentication-en).

## アクセストークンはどのように入手できますか？ {#how-do-i-get-an-access-token}

アクセストークンは、すべての API 呼び出しの Authorization ヘッダーに必要です。IMS 組織の統合にアクセスできる場合は、`curl` コマンドを使用して生成できます。アクセストークンは 24 時間のみ有効で、その後 API を使用し続けるためには新しいトークンを生成する必要があります。アクセストークンの生成について詳しくは、[認証に関するチュートリアル](http://www.adobe.com/go/platform-api-authentication-en)を参照してください。

## クエリーパラメーターの使用方法 {#how-do-i-user-query-parameters}

Some [!DNL Platform] API endpoints accept query parameters to locate specific information and filter the results returned in the response. リクエストパスに疑問符（`?`）記号が付加され、その後に 1 つ以上のクエリーパラメーターが `paramName=paramValue` 形式で追加されます。1 回の呼び出しで複数のパラメーターを組み合わせる場合、アンパサンド（`&`）を使用して個々のパラメーターを区切る必要があります。次の例は、複数のクエリーパラメーターを使用するリクエストがドキュメントでどのように表現されているかを示しています。

以下は、一般的に使用されるクエリーパラメーターの例です。

```http
GET /tenant/schemas?orderby=title
GET /datasets?limit=36&start=10
GET /batches?createdAfter=1559775880000&orderBy=desc:created
```

特定のサービスまたはクエリで使用できるエンドポイントパラメーターの詳細については、サービス固有のドキュメントを参照してください。

## PATCH リクエストで更新する JSON フィールドを指定する方法を教えてください。 {#how-do-i-indicate-a-json-field-to-update-in-a-patch-request}

Many PATCH operations in [!DNL Platform] APIs use [JSON Pointer](https://tools.ietf.org/html/rfc6901) strings to indicate JSON properties to update. これらは通常、[JSON パッチ](https://tools.ietf.org/html/rfc6902) 形式を使用してリクエストペイロードに含まれます。これらのテクノロジーに必要な構文について詳しくは、[API の基本原則ガイド](api-fundamentals.md)を参照してください。

## Can I use Postman to make calls to [!DNL Platform] APIs? {#how-do-i-use-postman-to-make-calls-to-platform-apis}

[Postman は](https://www.postman.com/)、RESTful API への呼び出しを視覚化する便利なツールです。この[投稿](https://medium.com/adobetech/using-postman-for-jwt-authentication-on-adobe-i-o-7573428ffe7f)では、Postman を設定して自動的に認証を実行し、それを使用して API を利用する方法を説明します。[!DNL Experience Platform]

## What are the system requirements for [!DNL Platform]? {#what-are-the-system-requirements-for-platform}

UI と API のどちらを使用しているかによって、次の必要システム構成が適用されます。

**UI ベースの操作の場合：**
- 最新の標準的な Web ブラウザー。While the latest version of [!DNL Chrome] is recommended, current and previous major releases of [!DNL Firefox], [!DNL Internet Explorer], and Safari are also supported.
   - Each time a new major version is released, [!DNL Platform] starts supporting the most recent version while support for the third most recent version is dropped.
- すべてのブラウザーで、Cookie と JavaScript が有効になっている必要があります。

**API および開発者のインタラクションの場合：**
- REST、ストリーミング、Webhook 統合を開発する開発環境。

## エラーとトラブルシューティング {#errors-and-troubleshooting}

The following is a list of errors that you may encounter when using any [!DNL Experience Platform] service. For troubleshooting guides on individual [!DNL Platform] services, see the [service troubleshooting directory](#service-troubleshooting-directory) below.

## API ステータスコード {#api-status-codes}

The following status codes may be encountered on any [!DNL Experience Platform] API. それぞれに様々な原因があり、本項で述べる説明は概して一般的なものです。For more details regarding specific errors in individual [!DNL Platform] services, please see the [service troubleshooting directory](#service-troubleshooting-directory) below.

| ステータスコード | 説明 | 考えられる原因 |
--- | --- | ---
| 400 | Bad request | リクエストが不適切に構築され、キー情報が欠落している、または正しくない構文が含まれていました。 |
| 401 | Authentication failed | リクエストが認証チェックに合格しませんでした。アクセストークンが見つからないか、無効です。詳しくは、以下の「[OAuth トークンエラー](#oauth-token-is-missing)」の節を参照してください。 |
| 403 | Forbidden | リソースが見つかりましたが、リソースを表示するための正しい資格情報がありません。 |
| 404 | Not found | リクエストされたリソースがサーバーで見つかりませんでした。リソースが削除されたか、リクエストされたパスが正しく入力されていない可能性があります。 |
| 500 | Internal server error | これはサーバーサイドのエラーです。同時に多数の呼び出しをおこなう場合、API の制限に達し、結果をフィルターする必要がある可能性があります。(See the [!DNL Catalog Service] API developer guide sub-guide on [filtering data](../catalog/api/filter-data.md) to learn more.) リクエストを再試行する前にしばらく待ち、問題が解決しない場合は管理者に問い合わせてください。 |

## リクエストヘッダーエラー {#request-header-errors}

All API calls in [!DNL Platform] require specific request headers. 個々のサービスに必要なヘッダーを確認するには、 [API リファレンスのドキュメント](http://www.adobe.com/go/platform-api-reference-en)を参照してください。必要な認証ヘッダーの値を確認するには、[認証に関するチュートリアル](http://www.adobe.com/go/platform-api-authentication-en)を参照してください。API 呼び出しをおこなう際に、これらのヘッダーのいずれかが見つからないか無効な場合は、次のエラーが発生する可能性があります。

### OAuth token is missing {#oauth-token-is-missing}

```json
{
    "error_code": "403010",
    "message": "Oauth token is missing."
}
```

このエラーメッセージは、API リクエストに `Authorization` ヘッダーがない場合に表示されます。再試行する前に、Authorization ヘッダーが有効なアクセストークンに含まれていることを確認してください。

### OAuth token is not valid

```json
{
    "error_code": "401013",
    "message": "Oauth token is not valid"
}
```

このエラーメッセージは、`Authorization` ヘッダーに指定されたアクセストークンが無効な場合に表示されます。トークンが正しく入力されていることを確認するか、Adobe I/O コンソールで[新しいトークンを生成](http://www.adobe.com/go/platform-api-authentication-en)します。

### API key is required

```json
{
    "error_code": "403000",
    "message": "Api Key is required"
}
```

このエラーメッセージは、API リクエストに API キーヘッダー（`x-api-key`）がない場合に表示されます。再試行する前に、ヘッダーが有効な API キーに含まれていることを確認してください。

### API key is invalid

```json
{
    "error_code": "403003",
    "message": "Api Key is invalid"
}
```

このエラーメッセージは、指定された API キーヘッダー（`x-api-key`）の値が無効な場合に表示されます。再試行する前に、キーが正しく入力されていることを確認してください。API キーがわからない場合は、[Adobe I/O コンソール](https://console.adobe.io)で確認できます。「**統合**」タブで 、特定の統合の「**概要**」セクションに移動すると、「**クライアント資格情報**」の下に API キーが表示されます。


### Missing header

```json
{
    "error_code": "400003",
    "message": "Missing header"
}
```

このエラーメッセージは、IMS 組織ヘッダー（`x-gw-ims-org-id`）が API リクエストに存在しない場合に表示されます。再試行する前に、IMS 組織の ID を含むヘッダーが含まれていることを確認してください。

### Profile is not valid

```json
{
    "error_code": "403025",
    "message": "Profile is not valid"
}
```

このエラーメッセージは、ユーザーまたは Adobe I/O 統合（[](#how-do-i-get-an-access-token) ヘッダーの`Authorization`アクセストークン[!DNL Experience Platform]によって識別）が、`x-gw-ims-org-id` ヘッダーで提供された IMS 組織に対して API を呼び出す権利がない場合に表示されます。再試行する前に、ヘッダーに IMS 組織の正しい ID が指定されていることを確認してください。組織 ID が不明な場合は、[Adobe I/O コンソール](https://console.adobe.io)で確認できます。「**統合**」タブで、特定の統合の「**概要**」セクションに移動すると、「**クライアント資格情報**」の下に ID が表示されます。

### Valid content-type not specified

```json
{
    "type": "/placeholder/type/uri",
    "status": 400,
    "title": "BadRequestError",
    "detail": "A valid content-type must be specified"
}
```

このエラーメッセージは、POST、PUT、PATCH リクエストの `Content-Type` ヘッダーが無効か、見つからない場合に表示されます。ヘッダーがリクエストに含まれ、その値が `application/json` であることを確認します。


## サービストラブルシューティングディレクトリ {#service-troubleshooting-directory}

The following is a list of troubleshooting guides and API reference documentation for [!DNL Experience Platform] APIs. Each troubleshooting guide provides answers to frequently asked questions and solutions to problems that are specific to individual [!DNL Platform] services. API リファレンスドキュメントは、各サービスで使用可能なすべてのエンドポイントの包括的なガイドを提供し、受け取る可能性のあるリクエストの本文、応答、エラーコードのサンプルを示します。

| サービス | API リファレンス | トラブルシューティング |
| --- | --- | --- |
| アクセス制御 | [アクセス制御 API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/access-control.yaml) | [アクセス制御トラブルシューティングガイド](../access-control/troubleshooting-guide.md) |
| Adobe Experience Platformでのデータ取得 | [[!DNL Data Ingestion API]](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/ingest-api.yaml) | [Batch Ingestionのトラブルシューティング](../ingestion/batch-ingestion/troubleshooting.md)<br><br>[ガイドストリーミング取り込みのトラブルシューティングガイド](../ingestion/streaming-ingestion/troubleshooting.md) |
| Adobe Experience Platformデータサイエンスワークスペース | [[!DNL Sensei Machine Learning API]](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/sensei-ml-api.yaml) | [[!DNL Data Science Workspace] トラブルシューティングガイド](../data-science-workspace/troubleshooting-guide.md) |
| Adobe Experience Platform のデータガバナンス | [[!DNL Policy Service API]](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/dule-policy-service.yaml) |  |
| Adobe Experience Platform ID サービス | [[!DNL Identity Service API]](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/id-service-api.yaml) | [[!DNL Identity Service] トラブルシューティングガイド](../identity-service/troubleshooting-guide.md) |
| Adobe Experience Platform クエリサービス | [[!DNL Query Service API]](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/qs-api.yaml) | [[!DNL Query Service] トラブルシューティングガイド](../query-service/troubleshooting-guide.md) |
| Adobe Experience Platform分類 | [[!DNL Segmentation API]](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/segmentation.yaml) |
| [!DNL Catalog Service] | [[!DNL Catalog Service API]](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/catalog.yaml) |  |
| [!DNL Experience Data Model] (XDM) | [[!DNL Schema Registry API]](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/schema-registry.yaml) | [[!DNL XDM System] FAQとトラブルシューティングガイド](../xdm/troubleshooting-guide.md) |
| [!DNL Flow Service] ([!DNL Sources] および [!DNL Destinations]) | [[!DNL Flow Service API]](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/flow-service.yaml) |  |
| [!DNL Real-time Customer Profile] | [[!DNL Real-time Customer Profile API]](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/real-time-customer-profile.yaml) | [[!DNL Profile] トラブルシューティングガイド](../profile/troubleshooting.md) |
| サンドボックス | [サンドボックス API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/sandbox-api.yaml) | [サンドボックストラブルシューティングガイド](../sandboxes/troubleshooting-guide.md) |

