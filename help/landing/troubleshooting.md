---
keywords: Experience Platform;ホーム;人気のトピック;API エラーコード;API エラーコード;エラーコード API;エラーコード API;API リクエストエラー;API トラブルシューティング;API エラー
solution: Experience Platform
title: Adobe Experience Platform に関する FAQ とトラブルシューティングガイド
description: よくある質問への回答、および Experience Platform の一般的なエラーのトラブルシューティングに関するガイドをご覧ください。
landing-page-description: よくある質問への回答、および Experience Platform の一般的なエラーのトラブルシューティングに関するガイドをご覧ください。
short-description: よくある質問への回答、および Experience Platform の一般的なエラーのトラブルシューティングに関するガイドをご覧ください。
type: Documentation
exl-id: 3e6d29aa-2138-421b-8bee-82b632962c01
source-git-commit: 76ef5638316a89aee1c6fb33370af943228b75e1
workflow-type: tm+mt
source-wordcount: '1877'
ht-degree: 99%

---

# [!DNL Platform] に関する FAQ とトラブルシューティングガイド

このドキュメントでは、Adobe Experience Platform に関するよくある質問への回答と、[!DNL Experience Platform] API で発生する可能性がある一般的なエラーの高度なトラブルシューティングガイドを提供します。個々の [!DNL Platform] サービスのトラブルシューティングガイドについては、以下の[サービストラブルシューティングディレクトリ](#service-troubleshooting-directory)を参照してください。

## FAQ {#faq}

次に、Adobe Experience Platform に関するよくある質問に対する回答をリストします。

## [!DNL Experience Platform] API とは何ですか？ {#what-are-experience-platform-apis}

[!DNL Experience Platform] では、HTTP リクエストを使用して [!DNL Platform] リソースにアクセスするために複数の RESTful API が使用されます。これらのサービス API は、それぞれ複数のエンドポイントを公開し、リスト（GET）、ルックアップ（GET）、編集（PUT または PATCH）および削除（DELETE）リソースに対する操作を実行できます。各サービスで使用できる特定のエンドポイントと操作について詳しくは、Adobe I/O の [API リファレンスドキュメント](https://www.adobe.com/go/platform-api-reference-en)を参照してください。

## API リクエストの形式を設定する方法を教えてください。  {#how-do-i-format-an-api-request}

リクエストの形式は、使用する [!DNL Platform] API によって異なります。API 呼び出しの構造を学ぶ最善の方法は、使用している特定の [!DNL Platform] サービスのドキュメントに記載されている例に従うことです。

API リクエストの形式について詳しくは、『Platform API 入門ガイド』の [API 呼び出しのサンプルを参照する](./api-guide.md#sample-api)の節を参照してください。

## IMS 組織とは何ですか。  {#what-is-my-ims-organization}

IMS 組織は、顧客のアドビ代表です。ライセンスを取得したアドビのソリューションは、この顧客組織に統合されます。IMS 組織が [!DNL Experience Platform] の権利を付与されると、開発者にアクセスを割り当てることができます。IMS 組織 ID（`x-gw-ims-org-id`）は、API 呼び出しを実行する必要がある組織を表すもので、すべての API リクエストのヘッダーとして必要です。この ID は、[Adobe Developer Console](https://www.adobe.com/go/devs_console_ui) で確認できます。「**統合**」タブで、特定の統合の「**概要**」セクションに移動すると「**クライアント資格情報**」の下に ID が表示されます。[!DNL Platform] への認証方法の詳しい手順については、[認証に関するチュートリアル](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja)を参照してください。

## API キーはどこで入手できますか？  {#where-can-i-find-my-api-key}

API キーは、すべての API リクエストのヘッダーとして必要です。これは、[Adobe Developer Console](https://www.adobe.com/go/devs_console_ui) から入手できます。コンソールの「**統合**」タブで、特定の統合の「**概要**」セクションに移動すると、「**クライアント資格情報**」の下にキーが表示されます。[!DNL Platform] への認証方法の詳しい手順については、[認証に関するチュートリアル](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja)を参照してください。

## アクセストークンはどのように入手できますか？  {#how-do-i-get-an-access-token}

アクセストークンは、すべての API 呼び出しの Authorization ヘッダーに必要です。IMS 組織の統合にアクセスできる場合は、`curl` コマンドを使用して生成できます。アクセストークンは 24 時間のみ有効で、その後 API を使用し続けるためには新しいトークンを生成する必要があります。アクセストークンの生成について詳しくは、[認証に関するチュートリアル](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja)を参照してください。

## クエリーパラメーターの使用方法  {#how-do-i-user-query-parameters}

一部の [!DNL Platform] API エンドポイントは、クエリパラメーターを受け取って特定の情報を検索し、応答で返される結果をフィルタリングします。リクエストパスに疑問符（`?`）記号が付加され、その後に 1 つ以上のクエリーパラメーターが `paramName=paramValue` 形式で追加されます。1 回の呼び出しで複数のパラメーターを組み合わせる場合、アンパサンド（`&`）を使用して個々のパラメーターを区切る必要があります。次の例は、複数のクエリーパラメーターを使用するリクエストがドキュメントでどのように表現されているかを示しています。

以下は、一般的に使用されるクエリーパラメーターの例です。

```http
GET /tenant/schemas?orderby=title
GET /datasets?limit=36&start=10
GET /batches?createdAfter=1559775880000&orderBy=desc:created
```

特定のサービスまたはクエリで使用できるエンドポイントパラメーターの詳細については、サービス固有のドキュメントを参照してください。

## PATCH リクエストで更新する JSON フィールドを指定する方法を教えてください。  {#how-do-i-indicate-a-json-field-to-update-in-a-patch-request}

[!DNL Platform] API の多くの PATCH 操作では、[JSON Pointer](https://tools.ietf.org/html/rfc6901) 文字列を使用して、更新する JSON プロパティを示します。これらは通常、[JSON パッチ](https://tools.ietf.org/html/rfc6902) 形式を使用してリクエストペイロードに含まれます。これらのテクノロジーに必要な構文について詳しくは、[API の基本原則ガイド](api-fundamentals.md)を参照してください。

## Postman を使用して [!DNL Platform] API を呼び出すことはできますか？ {#how-do-i-use-postman-to-make-calls-to-platform-apis}

[Postman](https://www.postman.com/) は、RESTful API への呼び出しを視覚化する便利なツールです。『[Platform API 入門ガイド](api-guide.md)』には、Postman コレクションを読み込むためのビデオと手順が含まれています。さらに、各サービスについて Postman コレクションのリストが提供されます。

## [!DNL Platform] の必要システム構成は何ですか？{#what-are-the-system-requirements-for-platform}

UI と API のどちらを使用しているかによって、次の必要システム構成が適用されます。

**UI ベースの操作の場合：**
- 最新の標準的な Web ブラウザー。最新バージョンの [!DNL Chrome] をお勧めしますが、[!DNL Firefox] の最新および以前のメジャーリリース、[!DNL Internet Explorer]、Safari もサポートされています。
   - 新しいメジャーバージョンがリリースされるたびに、[!DNL Platform] は最新バージョンのサポートを開始しますが、3 番目に新しいバージョンのサポートは終了します。
- すべてのブラウザーで、Cookie と JavaScript が有効になっている必要があります。

**API および開発者のインタラクションの場合：**
- REST、ストリーミング、Webhook 統合を開発する開発環境。

## エラーとトラブルシューティング {#errors-and-troubleshooting}

以下は、[!DNL Experience Platform] サービスの使用時に発生する可能性のあるエラーのリストです。個々の [!DNL Platform] サービスのトラブルシューティングガイドについては、以下の[サービストラブルシューティングディレクトリ](#service-troubleshooting-directory)を参照してください。

## API ステータスコード {#api-status-codes}

以下のステータスコードは、どの [!DNL Experience Platform] API でも発生する場合があります。それぞれに様々な原因があり、本項で述べる説明は概して一般的なものです。個々の [!DNL Platform] サービスで発生する特定のエラーについて詳しくは、以下の[サービストラブルシューティングディレクトリ](#service-troubleshooting-directory)を参照してください。

| ステータスコード | 説明 | 考えられる原因 |
|--- | --- | ---|
| 400 | Bad request | リクエストが不適切に構築され、キー情報が欠落している、または正しくない構文が含まれていました。 |
| 401 | Authentication failed | リクエストが認証チェックに合格しませんでした。アクセストークンが見つからないか、無効です。詳しくは、以下の「[OAuth トークンエラー](#oauth-token-is-missing)」の節を参照してください。 |
| 403 | Forbidden | リソースが見つかりましたが、リソースを表示するための正しい資格情報がありません。 |
| 404 | Not found | リクエストされたリソースがサーバーで見つかりませんでした。リソースが削除されたか、リクエストされたパスが正しく入力されていない可能性があります。 |
| 500 | Internal server error | これはサーバーサイドのエラーです。同時に多数の呼び出しをおこなう場合、API の制限に達し、結果をフィルターする必要がある可能性があります。（詳しくは、[!DNL Catalog Service] API 開発者ガイドの[データのフィルタリング](../catalog/api/filter-data.md)に関するサブガイドを参照してください。）リクエストを再試行する前にしばらく待ち、問題が解決しない場合は管理者に問い合わせてください。 |

## リクエストヘッダーエラー {#request-header-errors}

[!DNL Platform] 内のすべての API 呼び出しには、特定のリクエストヘッダーが必要です。個々のサービスに必要なヘッダーを確認するには、 [API リファレンスのドキュメント](https://www.adobe.com/go/platform-api-reference-en)を参照してください。必要な認証ヘッダーの値を確認するには、[認証に関するチュートリアル](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja)を参照してください。API 呼び出しをおこなう際に、これらのヘッダーのいずれかが見つからないか無効な場合は、次のエラーが発生する可能性があります。

### OAuth トークンがありません {#oauth-token-is-missing}

```json
{
    "error_code": "403010",
    "message": "Oauth token is missing."
}
```

このエラーメッセージは、API リクエストに `Authorization` ヘッダーがない場合に表示されます。再試行する前に、Authorization ヘッダーが有効なアクセストークンに含まれていることを確認してください。

### OAuth token is not valid {#oauth-token-is-not-valid}

```json
{
    "error_code": "401013",
    "message": "Oauth token is not valid"
}
```

このエラーメッセージは、`Authorization` ヘッダーに指定されたアクセストークンが無効な場合に表示されます。トークンが正しく入力されていることを確認するか、Adobe I/O コンソールで[新しいトークンを生成](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja)します。

### API key is required {#api-key-is-required}

```json
{
    "error_code": "403000",
    "message": "Api Key is required"
}
```

このエラーメッセージは、API リクエストに API キーヘッダー（`x-api-key`）がない場合に表示されます。再試行する前に、ヘッダーが有効な API キーに含まれていることを確認してください。

### API key is invalid {#api-key-is-invalid}

```json
{
    "error_code": "403003",
    "message": "Api Key is invalid"
}
```

このエラーメッセージは、指定された API キーヘッダー（`x-api-key`）の値が無効な場合に表示されます。再試行する前に、キーが正しく入力されていることを確認してください。API キーがわからない場合は、[Adobe I/O コンソール](https://console.adobe.io)で確認できます。「**統合**」タブで、特定の統合の「**概要**」セクションに移動すると、「**クライアント資格情報**」の下に API キーが表示されます。

### Missing header {#missing-header}

```json
{
    "error_code": "400003",
    "message": "Missing header"
}
```

このエラーメッセージは、IMS 組織ヘッダー（`x-gw-ims-org-id`）が API リクエストに存在しない場合に表示されます。再試行する前に、IMS 組織の ID を含むヘッダーが含まれていることを確認してください。

### Profile is not valid {#profile-is-not-valid}

```json
{
    "error_code": "403025",
    "message": "Profile is not valid"
}
```

このエラーメッセージは、ユーザーまたは Adobe I/O 統合（`Authorization` ヘッダーの[アクセストークン](#how-do-i-get-an-access-token)によって識別）が、`x-gw-ims-org-id` ヘッダーで提供された IMS 組織に対して [!DNL Experience Platform] API を呼び出す権利がない場合に表示されます。再試行する前に、ヘッダーに IMS 組織の正しい ID が指定されていることを確認してください。組織 ID が不明な場合は、[Adobe I/O コンソール](https://console.adobe.io)で確認できます。「**統合**」タブで、特定の統合の「**概要**」セクションに移動すると、「**クライアント資格情報**」の下に ID が表示されます。

### etag 更新エラー {#refresh-etag-error}

```json
{
"errorMessage":"Supplied version=[\\\\\\\"a200a2a3-0000-0200-0000-123178f90000\\\\\\\"] does not match the current version on entity=[\\\\\\\"a200cdb2-0000-0200-0000-456179940000\\\\\\\"]"
}
```

フロー、接続、ソースコネクタ、またはターゲット接続など、別の API 呼び出し元がソースまたは宛先エンティティに対して変更を加えた場合、etag エラーが発生することがあります。バージョンの不一致により、行おうとしている変更はエンティティの最新バージョンには適用されません。

これを解決するには、エンティティを再度取得し、変更がエンティティの新しいバージョンと互換性があることを確認してから、新しい etag を `If-Match` ヘッダーに配置し、最後に API 呼び出しを行う必要があります。

### Valid content-type not specified {#valid-content-type-not-specified}

```json
{
    "type": "/placeholder/type/uri",
    "status": 400,
    "title": "BadRequestError",
    "detail": "A valid content-type must be specified"
}
```

このエラーメッセージは、POST、PUT、PATCH リクエストの `Content-Type` ヘッダーが無効か、見つからない場合に表示されます。ヘッダーがリクエストに含まれ、その値が `application/json` であることを確認します。

### ユーザー領域がありません {#user-region-is-missing}

```json
{
    "error_code": "403027",
    "message": "User region is missing"
}
```

このエラーメッセージは、次の 2 つの場合のいずれかで表示されます。
- 間違った形式または不正な形式の IMS 組織ヘッダー（`x-gw-ims-org-id`）が API リクエストで渡された場合。再試行する前に、IMS 組織の正しい ID が含まれていることを確認してください。
- アカウント（指定された認証資格情報で表される）が Experience Platform の製品プロファイルに関連付けられていない場合。Platform API 認証チュートリアルの[アクセス資格情報の生成](./api-authentication.md#authentication-for-each-session)の手順に従って、Platform をアカウントに追加し、それに応じて認証資格情報を更新します。

## サービストラブルシューティングディレクトリ {#service-troubleshooting-directory}

以下は、[!DNL Experience Platform] API のトラブルシューティングガイドと API リファレンスドキュメントのリストです。各トラブルシューティングガイドでは、よくある質問への回答と、個々の [!DNL Platform] サービスに固有の問題に対する解決方法を提供します。API リファレンスドキュメントは、各サービスで使用可能なすべてのエンドポイントの包括的なガイドを提供し、受け取る可能性のあるリクエストの本文、応答、エラーコードのサンプルを示します。

| サービス | API リファレンス | トラブルシューティング |
| --- | --- | --- |
| アクセス制御 | [アクセス制御 API](https://www.adobe.io/experience-platform-apis/references/access-control/) | [アクセス制御トラブルシューティングガイド](../access-control/troubleshooting-guide.md) |
| Adobe Experience Platform でのデータ取得 | [[!DNL Batch Ingestion API]](https://developer.adobe.com/experience-platform-apis/references/batch-ingestion/) | [バッチ取得トラブルシューティングガイド](../ingestion/batch-ingestion/troubleshooting.md) |
| Adobe Experience Platform でのデータ取得 | [[!DNL Streaming Ingestion API]](https://developer.adobe.com/experience-platform-apis/references/streaming-ingestion/) | [ストリーミング取得トラブルシューティングガイド](../ingestion/streaming-ingestion/troubleshooting.md) |
| Adobe Experience Platform データサイエンスワークスペース | [[!DNL Sensei Machine Learning API]](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/sensei-ml-api.yaml) | [[!DNL Data Science Workspace] トラブルシューティングガイド](../data-science-workspace/troubleshooting-guide.md) |
| Adobe Experience Platform のデータガバナンス | [[!DNL Policy Service API]](https://www.adobe.io/experience-platform-apis/references/policy-service/) |  |
| Adobe Experience Platform ID サービス | [[!DNL Identity Service API]](https://www.adobe.io/experience-platform-apis/references/identity-service) | [[!DNL Identity Service] トラブルシューティングガイド](../identity-service/troubleshooting-guide.md) |
| Adobe Experience Platform クエリサービス | [[!DNL Query Service API]](https://www.adobe.io/experience-platform-apis/references/query-service/) | [[!DNL Query Service] トラブルシューティングガイド](../query-service/troubleshooting-guide.md) |
| Adobe Experience Platform セグメント化 | [[!DNL Segmentation API]](https://www.adobe.io/experience-platform-apis/references/segmentation/) |
| [!DNL Catalog Service] | [[!DNL Catalog Service API]](https://www.adobe.io/experience-platform-apis/references/catalog/) |  |
| [!DNL Experience Data Model]（XDM） | [[!DNL Schema Registry API]](https://www.adobe.io/experience-platform-apis/references/schema-registry/) | [[!DNL XDM System]  に関する FAQ とトラブルシューティングガイド](../xdm/troubleshooting-guide.md) |
| [!DNL Flow Service]（[!DNL Sources] および [!DNL Destinations]） | [[!DNL Flow Service API]](https://www.adobe.io/experience-platform-apis/references/flow-service/) |  |
| [!DNL Real-Time Customer Profile] | [[!DNL Real-Time Customer Profile API]](https://www.adobe.com/go/profile-apis-en) | [[!DNL Profile] トラブルシューティングガイド](../profile/troubleshooting.md) |
| サンドボックス | [サンドボックス API](https://www.adobe.io/experience-platform-apis/references/sandbox) | [サンドボックストラブルシューティングガイド](../sandboxes/troubleshooting-guide.md) |
