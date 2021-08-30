---
keywords: Experience Platform、ホーム、人気のあるトピック、APIエラーコード、APIエラーコード、エラーコードAPI、エラーコードAPI、APIリクエストエラー、APIトラブルシューティング、APIエラー
solution: Experience Platform
title: Adobe Experience Platform に関する FAQ とトラブルシューティングガイド
description: よくある質問への回答、および Experience Platform の一般的なエラーのトラブルシューティングに関するガイドを見つけます。
landing-page-description: よくある質問への回答、および Experience Platform の一般的なエラーのトラブルシューティングに関するガイドを見つけます。
topic-legacy: getting started
type: Documentation
exl-id: 3e6d29aa-2138-421b-8bee-82b632962c01
source-git-commit: f5ce7b7f09c624c53065757bb8a9b09f989dce0a
workflow-type: tm+mt
source-wordcount: '1731'
ht-degree: 67%

---

# [!DNL Platform] FAQとトラブルシューティングガイド

このドキュメントでは、Adobe Experience Platformに関するよくある質問と、[!DNL Experience Platform] APIで発生する可能性のある一般的なエラーの概要レベルのトラブルシューティングガイドについて回答します。 個々の[!DNL Platform]サービスのトラブルシューティングガイドについては、以下の[サービストラブルシューティングディレクトリ](#service-troubleshooting-directory)を参照してください。

## FAQ {#faq}

次に、Adobe Experience Platform に関するよくある質問に対する回答をリストします。

## [!DNL Experience Platform] APIとは {#what-are-experience-platform-apis}

[!DNL Experience Platform] は、HTTPリクエストを使用してリソースにアクセスする複数のRESTful APIを提 [!DNL Platform] 供します。これらのサービス API は、それぞれ複数のエンドポイントを公開し、リスト（GET）、ルックアップ（GET）、編集（PUT または PATCH）および削除（DELETE）リソースに対する操作を実行できます。各サービスで使用できる特定のエンドポイントと操作について詳しくは、Adobe I/O の [API リファレンスドキュメント](https://www.adobe.com/go/platform-api-reference-en)を参照してください。

## API リクエストの形式を設定する方法を教えてください。  {#how-do-i-format-an-api-request}

リクエストの形式は、使用する[!DNL Platform] APIによって異なります。 API呼び出しの構造を学ぶ最善の方法は、使用している特定の[!DNL Platform]サービスのドキュメントに記載されている例に従うことです。

APIリクエストの形式について詳しくは、『Platform API入門ガイド』のAPI呼び出し例](./api-guide.md#sample-api)の節を参照してください。[

## IMS 組織とは何ですか。  {#what-is-my-ims-organization}

IMS 組織は、顧客のアドビ代表です。ライセンスを取得したアドビのソリューションは、この顧客組織に統合されます。IMS組織が[!DNL Experience Platform]の権利を付与されると、開発者にアクセス権を割り当てることができます。 IMS 組織 ID（`x-gw-ims-org-id`）は、API 呼び出しを実行する必要がある組織を表すもので、すべての API リクエストのヘッダーとして必要です。このIDは、[Adobe開発者コンソール](https://www.adobe.com/go/devs_console_ui)で確認できます。「**統合**」タブで、特定の統合の「**概要**」セクションに移動し、「**クライアント資格情報**」の下にIDを探します。 [!DNL Platform]への認証方法の詳しい手順については、[認証に関するチュートリアル](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja#platform-apis)を参照してください。

## API キーはどこで入手できますか？  {#where-can-i-find-my-api-key}

API キーは、すべての API リクエストのヘッダーとして必要です。これは、[Adobe開発者コンソール](https://www.adobe.com/go/devs_console_ui)で確認できます。 コンソールの「**統合**」タブで、特定の統合の「**概要**」セクションに移動すると、「**クライアント資格情報**」の下にキーが表示されます。[!DNL Platform]に対する認証方法の詳しい手順については、[認証に関するチュートリアル](https://www.adobe.com/go/platform-api-authentication-en)を参照してください。

## アクセストークンはどのように入手できますか？  {#how-do-i-get-an-access-token}

アクセストークンは、すべての API 呼び出しの Authorization ヘッダーに必要です。IMS 組織の統合にアクセスできる場合は、`curl` コマンドを使用して生成できます。アクセストークンは 24 時間のみ有効で、その後 API を使用し続けるためには新しいトークンを生成する必要があります。アクセストークンの生成について詳しくは、[認証に関するチュートリアル](https://www.adobe.com/go/platform-api-authentication-en)を参照してください。

## クエリーパラメーターの使用方法  {#how-do-i-user-query-parameters}

一部の[!DNL Platform] APIエンドポイントは、クエリパラメーターを受け取って特定の情報を検索し、応答で返される結果をフィルタリングします。 リクエストパスに疑問符（`?`）記号が付加され、その後に 1 つ以上のクエリーパラメーターが `paramName=paramValue` 形式で追加されます。1 回の呼び出しで複数のパラメーターを組み合わせる場合、アンパサンド（`&`）を使用して個々のパラメーターを区切る必要があります。次の例は、複数のクエリーパラメーターを使用するリクエストがドキュメントでどのように表現されているかを示しています。

以下は、一般的に使用されるクエリーパラメーターの例です。

```http
GET /tenant/schemas?orderby=title
GET /datasets?limit=36&start=10
GET /batches?createdAfter=1559775880000&orderBy=desc:created
```

特定のサービスまたはクエリで使用できるエンドポイントパラメーターの詳細については、サービス固有のドキュメントを参照してください。

## PATCH リクエストで更新する JSON フィールドを指定する方法を教えてください。  {#how-do-i-indicate-a-json-field-to-update-in-a-patch-request}

[!DNL Platform] APIの多くのPATCH操作では、[JSON Pointer](https://tools.ietf.org/html/rfc6901)文字列を使用して、更新するJSONプロパティを示します。 これらは通常、[JSON パッチ](https://tools.ietf.org/html/rfc6902) 形式を使用してリクエストペイロードに含まれます。これらのテクノロジーに必要な構文について詳しくは、[API の基本原則ガイド](api-fundamentals.md)を参照してください。

## Postmanを使用して[!DNL Platform] APIを呼び出すことはできますか？ {#how-do-i-use-postman-to-make-calls-to-platform-apis}

[Postman は](https://www.postman.com/)、RESTful API への呼び出しを視覚化する便利なツールです。[Platform API入門ガイド](api-guide.md)には、Postmanコレクションを読み込むためのビデオと手順が含まれています。 さらに、各サービスのPostmanコレクションのリストが提供されます。

## [!DNL Platform]の必要システム構成は何ですか？{#what-are-the-system-requirements-for-platform}

UI と API のどちらを使用しているかによって、次の必要システム構成が適用されます。

**UI ベースの操作の場合：**
- 最新の標準的な Web ブラウザー。[!DNL Chrome]の最新バージョンをお勧めしますが、[!DNL Firefox]、[!DNL Internet Explorer]、Safariの最新および以前のメジャーリリースもサポートされています。
   - 新しいメジャーバージョンがリリースされるたびに、[!DNL Platform]は最新バージョンのサポートを開始しますが、3番目に新しいバージョンのサポートは終了します。
- すべてのブラウザーで、Cookie と JavaScript が有効になっている必要があります。

**API および開発者のインタラクションの場合：**
- REST、ストリーミング、Webhook 統合を開発する開発環境。

## エラーとトラブルシューティング {#errors-and-troubleshooting}

以下は、[!DNL Experience Platform]サービスを使用する際に発生する可能性のあるエラーのリストです。 個々の[!DNL Platform]サービスのトラブルシューティングガイドについては、以下の[サービストラブルシューティングディレクトリ](#service-troubleshooting-directory)を参照してください。

## API ステータスコード {#api-status-codes}

以下のステータスコードは、どの[!DNL Experience Platform] APIでも発生する場合があります。 それぞれに様々な原因があり、本項で述べる説明は概して一般的なものです。個々の[!DNL Platform]サービスで発生する特定のエラーに関する詳細は、以下の[サービストラブルシューティングディレクトリ](#service-troubleshooting-directory)を参照してください。

| ステータスコード | 説明 | 考えられる原因 |
|--- | --- | ---|
| 400 | Bad request | リクエストが不適切に構築され、キー情報が欠落している、または正しくない構文が含まれていました。 |
| 401 | Authentication failed | リクエストが認証チェックに合格しませんでした。アクセストークンが見つからないか、無効です。詳しくは、以下の「[OAuth トークンエラー](#oauth-token-is-missing)」の節を参照してください。 |
| 403 | Forbidden | リソースが見つかりましたが、リソースを表示するための正しい資格情報がありません。 |
| 404 | Not found | リクエストされたリソースがサーバーで見つかりませんでした。リソースが削除されたか、リクエストされたパスが正しく入力されていない可能性があります。 |
| 500 | Internal server error | これはサーバーサイドのエラーです。同時に多数の呼び出しをおこなう場合、API の制限に達し、結果をフィルターする必要がある可能性があります。（詳しくは、 [データのフィルタリング](../catalog/api/filter-data.md)に関するAPI開発者ガイド[!DNL Catalog Service]を参照してください）。 リクエストを再試行する前にしばらく待ち、問題が解決しない場合は管理者に問い合わせてください。 |

## リクエストヘッダーエラー {#request-header-errors}

[!DNL Platform]内のすべてのAPI呼び出しには、特定のリクエストヘッダーが必要です。 個々のサービスに必要なヘッダーを確認するには、 [API リファレンスのドキュメント](https://www.adobe.com/go/platform-api-reference-en)を参照してください。必要な認証ヘッダーの値を確認するには、[認証に関するチュートリアル](https://www.adobe.com/go/platform-api-authentication-en)を参照してください。API 呼び出しをおこなう際に、これらのヘッダーのいずれかが見つからないか無効な場合は、次のエラーが発生する可能性があります。

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

このエラーメッセージは、`Authorization` ヘッダーに指定されたアクセストークンが無効な場合に表示されます。トークンが正しく入力されていることを確認するか、Adobe I/O コンソールで[新しいトークンを生成](https://www.adobe.com/go/platform-api-authentication-en)します。

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

### ユーザー領域が見つかりません

```json
{
    "error_code": "403027",
    "message": "User region is missing"
}
```

このエラーメッセージは、アカウント（指定された認証資格情報で表される）がExperience Platform用の製品プロファイルに関連付けられていない場合に表示されます。 『Platform API認証チュートリアル』の[アクセス資格情報](./api-authentication.md#authentication-for-each-session)の生成手順に従って、アカウントにPlatformを追加し、それに応じて認証資格情報を更新します。

## サービストラブルシューティングディレクトリ {#service-troubleshooting-directory}

以下は、 [!DNL Experience Platform] APIのトラブルシューティングガイドとAPIリファレンスドキュメントのリストです。 各トラブルシューティングガイドでは、個々の[!DNL Platform]サービスに固有のよくある質問と問題の解決方法を示します。 API リファレンスドキュメントは、各サービスで使用可能なすべてのエンドポイントの包括的なガイドを提供し、受け取る可能性のあるリクエストの本文、応答、エラーコードのサンプルを示します。

| サービス | API リファレンス | トラブルシューティング |
| --- | --- | --- |
| アクセス制御 | [アクセス制御 API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/access-control.yaml) | [アクセス制御トラブルシューティングガイド](../access-control/troubleshooting-guide.md) |
| Adobe Experience Platformでのデータ取得 | [[!DNL Data Ingestion API]](https://www.adobe.io/experience-platform-apis/references/data-ingestion/) | [バッチ取得トラブルシューティング](../ingestion/batch-ingestion/troubleshooting.md)<br><br>[ガイドストリーミング取得トラブルシューティングガイド](../ingestion/streaming-ingestion/troubleshooting.md) |
| Adobe Experience Platform Data Science Workspace | [[!DNL Sensei Machine Learning API]](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/sensei-ml-api.yaml) | [[!DNL Data Science Workspace] トラブルシューティングガイド](../data-science-workspace/troubleshooting-guide.md) |
| Adobe Experience Platform のデータガバナンス | [[!DNL Policy Service API]](https://www.adobe.io/experience-platform-apis/references/policy-service/) |  |
| Adobe Experience Platform ID サービス | [[!DNL Identity Service API]](https://www.adobe.io/experience-platform-apis/references/identity-service) | [[!DNL Identity Service] トラブルシューティングガイド](../identity-service/troubleshooting-guide.md) |
| Adobe Experience Platform クエリサービス | [[!DNL Query Service API]](https://www.adobe.io/experience-platform-apis/references/query-service/) | [[!DNL Query Service] トラブルシューティングガイド](../query-service/troubleshooting-guide.md) |
| Adobe Experience Platform Segmentation | [[!DNL Segmentation API]](https://www.adobe.io/experience-platform-apis/references/segmentation/) |
| [!DNL Catalog Service] | [[!DNL Catalog Service API]](https://www.adobe.io/experience-platform-apis/references/catalog/) |  |
| [!DNL Experience Data Model] (XDM) | [[!DNL Schema Registry API]](https://www.adobe.io/experience-platform-apis/references/schema-registry/) | [[!DNL XDM System] FAQとトラブルシューティングガイド](../xdm/troubleshooting-guide.md) |
| [!DNL Flow Service] ([!DNL Sources] および [!DNL Destinations]) | [[!DNL Flow Service API]](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/flow-service.yaml) |  |
| [!DNL Real-time Customer Profile] | [[!DNL Real-time Customer Profile API]](https://www.adobe.com/go/profile-apis-en) | [[!DNL Profile] トラブルシューティングガイド](../profile/troubleshooting.md) |
| サンドボックス | [サンドボックス API](https://www.adobe.io/experience-platform-apis/references/sandbox) | [サンドボックストラブルシューティングガイド](../sandboxes/troubleshooting-guide.md) |
