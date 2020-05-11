---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Adobe Experience Platform FAQ and Troubleshooting Guide
topic: getting started
translation-type: tm+mt
source-git-commit: d9aa21a7439a6c40f6f51dfbdf5c7b3690c4593a
workflow-type: tm+mt
source-wordcount: '2001'
ht-degree: 2%

---


# プラットフォームに関するFAQとトラブルシューティングガイド

このドキュメントでは、Adobe Experience Platformに関するよくある質問と、Experience Platform APIで発生する可能性のある一般的なエラーの高レベルのトラブルシューティングガイドに回答します。 個々のプラットフォームサービスのトラブルシューティングガイドについては、 [サービスのトラブルシューティングディレクトリ](#service-troubleshooting-directory) （下記）を参照してください。

## FAQ {#faq}

次に、Adobe Experience Platformに関するよくある質問に対する回答のリストを示します。

## エクスペリエンスプラットフォームAPIとは {#what-are-experience-platform-apis}

エクスペリエンスプラットフォームオファーでは、HTTP要求を使用してプラットフォームリソースにアクセスする複数のRESTful APIを使用します。 これらのサービスAPIはそれぞれ複数のエンドポイントを公開し、リスト(GET)、参照(GET)、編集（PUTやPATCH）、削除(DELETE)の各リソースに対する操作を実行できます。 各サービスで使用できる特定のエンドポイントと操作について詳しくは、Adobe I/Oに関する [APIリファレンスドキュメント](https://www.adobe.io/apis/experienceplatform/home/api-reference.html) を参照してください。

## APIリクエストをフォーマットする方法を教えてください。 {#how-do-i-format-an-api-request}

リクエストの形式は、使用するプラットフォームAPIによって異なります。 API呼び出しの構造を学ぶ最善の方法は、使用している特定のプラットフォームサービスのドキュメントに記載されている例に従うことです。

### API呼び出しの例を読み取り中

エクスペリエンスプラットフォームのドキュメントに、API呼び出しの例が2つの異なる方法で示されています。 最初に、呼び出しは **API形式で表示され**、操作(GET、POST、PUT、PATCH、DELETE)と使用中のエンドポイント(例えば、 `/global/classes`)のみを示すテンプレート表現です。 また、テンプレートには、呼び出しの作成方法を示す変数の場所を示すものもあります（例：） `GET /{VARIABLE}/classes/{ANOTHER_VARIABLE}`。

その後、呼び出しは **リクエスト内のcURLコマンドとして表示されます**。このコマンドには、APIとの正常なやり取りに必要なヘッダーと完全な「ベースパス」が含まれます。 基本パスは、すべてのエンドポイントに前後に付加する必要があります。 例えば、前述の `/global/classes` エンドポイントがになり `https://platform.adobe.io/data/foundation/schemaregistry/global/classes`ます。 ドキュメント全体にAPI形式/リクエストパターンが表示され、独自のプラットフォームAPIを呼び出す場合は、例のリクエストに示す完全なパスを使用する必要があります。

### APIリクエストの例

以下は、ドキュメントで遭遇する形式を示すAPIリクエストの例です。

**API形式**

API形式は、操作(GET)と使用中のエンドポイントを示します。 変数は波括弧で示されます(この場合は `{CONTAINER_ID}`)。

```http
GET /{CONTAINER_ID}/classes
```

**リクエスト**

この例のリクエストでは、API形式の変数に、リクエストパス内の実際の値が与えられます。 必要なヘッダーはすべて、サンプルのヘッダー値や、機密情報（セキュリティトークンやアクセスIDなど）を含める必要がある変数と共に表示されます。

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

この応答は、APIの呼び出しが成功した後、送信されたリクエストに基づいて何を受け取るかを示します。 場合によっては、応答が空白で切り捨てられることがあります。つまり、サンプルに表示される情報や追加情報が表示される可能性があります。

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

必要なヘッダーやリクエスト本文を含む、プラットフォームAPIの特定のエンドポイントについて詳しくは、 [APIリファレンスドキュメントを参照してください](https://www.adobe.io/apis/experienceplatform/home/api-reference.html)。

## IMS組織とは {#what-is-my-ims-organization}

IMS組織は、顧客をアドビに代表するものです。 ライセンスを取得したアドビソリューションはすべて、このお客様の組織に統合されます。 IMS組織がエクスペリエンスプラットフォームの権利を付与されると、開発者にアクセスを割り当てることができます。 IMS組織ID(`x-gw-ims-org-id`)は、API呼び出しを実行する必要がある組織を表すので、すべてのAPIリクエストのヘッダーとして必要です。 このIDは、 [Adobe Developer Console](https://www.adobe.com/go/devs_console_ui): 「 **統合** 」タブで、特定の統合の「 **概要** 」セクションに移動し、「 **クライアント資格情報**」の下のIDを探します。 プラットフォームへの認証方法の詳しい手順については、 [認証のチュートリアルを参照してください](../tutorials/authentication.md)。

## APIキーはどこで入手できますか。 {#where-can-i-find-my-api-key}

APIキーは、すべてのAPIリクエストのヘッダーとして必要です。 これは、 [Adobe Developer Consoleから入手できます](https://www.adobe.com/go/devs_console_ui)。 コンソールの「 **統合** 」タブで、特定の統合の「 **概要** 」セクションに移動すると、「 **クライアント資格情報**」の下にキーが表示されます。 プラットフォームへの認証方法の詳しい手順については、 [認証のチュートリアルを参照してください](../tutorials/authentication.md)。

## アクセストークンを入手する方法 {#how-do-i-get-an-access-token}

アクセストークンは、すべてのAPI呼び出しの認証ヘッダーに必要です。 IMS組織の統合にアクセスできる場合は、 `curl` コマンドを使用してこれらを生成できます。 アクセストークンは24時間のみ有効です。その後、APIを使用し続けるには新しいトークンを生成する必要があります。 アクセストークンの生成について詳しくは、 [認証のチュートリアルを参照してください](../tutorials/authentication.md)。

## クエリパラメーターの使用方法 {#how-do-i-user-query-parameters}

一部のプラットフォームAPIエンドポイントは、特定のクエリを検索し、応答で返された結果をフィルターするために、情報パラメーターを受け取ります。 リクエストパスには疑問符(`?`)が付き、その後に形式を使用した1つ以上のクエリパラメーターが追加され `paramName=paramValue`ます。 1回の呼び出しで複数のパラメーターを組み合わせる場合は、アンパサンド(`&`)を使用して個々のパラメーターを区切る必要があります。 複数のクエリパラメーターを使用するリクエストがドキュメントでどのように表されるかを、次の例に示します。

一般的に使用されるクエリパラメーターの例を次に示します。

```http
GET /tenant/schemas?orderby=title
GET /datasets?limit=36&start=10
GET /batches?createdAfter=1559775880000&orderBy=desc:created
```

特定のサービスまたはエンドポイントで使用できるクエリパラメーターについて詳しくは、サービス固有のドキュメントを参照してください。

## PATCHリクエストで更新するJSONフィールドを指定する方法を教えてください。 {#how-do-i-indicate-a-json-field-to-update-in-a-patch-request}

プラットフォームAPIの多くのPATCH操作では、 [JSONポインタ](https://tools.ietf.org/html/rfc6901) 文字列を使用して、更新するJSONプロパティを示します。 これらは、通常、 [JSONパッチ](https://tools.ietf.org/html/rfc6902) 形式を使用したリクエストペイロードに含まれます。 これらのテクノロジーに必要な構文の詳細については、 [APIの基本的なガイド](api-fundamentals.md) （英語）を参照してください。

## Postmanを使用してプラットフォームAPIを呼び出すことはできますか。 {#how-do-i-use-postman-to-make-calls-to-platform-apis}

[Postman](https://www.getpostman.com/) は、RESTful APIへの呼び出しを視覚化するのに便利なツールです。 この [Medium投稿](https://medium.com/adobetech/using-postman-for-jwt-authentication-on-adobe-i-o-7573428ffe7f) では、Postmanを設定して自動的に認証を実行し、それを使用してエクスペリエンスプラットフォームAPIを利用する方法について説明します。

## プラットフォームの必要システム構成 {#what-are-the-system-requirements-for-platform}

UIとAPIのどちらを使用しているかに応じて、次の必要システム構成が適用されます。

**UIベースの操作の場合：**
- 最新の標準的なWebブラウザーです。 最新バージョンのChromeをお勧めしますが、Firefox、Internet Explorer、Safariの最新および以前のメジャーリリースもサポートされます。
   - 新しいメジャーバージョンがリリースされるたびに、最新バージョンをサポートするプラットフォーム開始と、3番目に新しいバージョンのサポートは削除されます。
- すべてのブラウザーでCookieとJavaScriptを有効にする必要があります。

**APIと開発者の対話用：**
- REST、ストリーミングおよびWebフック統合用に開発する開発環境です。

## エラーとトラブルシューティング {#errors-and-troubleshooting}

以下は、Experience Platformサービスの使用時に発生する可能性があるエラーのリストです。 個々のプラットフォームサービスのトラブルシューティングガイドについては、 [サービスのトラブルシューティングディレクトリ](#service-troubleshooting-directory) （下記）を参照してください。

## APIステータスコード {#api-status-codes}

任意のエクスペリエンスプラットフォームAPIで、次のステータスコードが検出される場合があります。 それぞれに様々な原因があり、本項での説明は本来一般的である。 個々のプラットフォームサービスで発生する特定のエラーに関する詳細は、 [サービスのトラブルシューティングディレクトリ](#service-troubleshooting-directory) （下記）を参照してください。

| ステータスコード | 説明 | 考えられる原因 |
--- | --- | ---
| 400 | 不正な要求 | 要求が不適切に構築され、キー情報が欠落しているか、または正しくない構文が含まれていました。 |
| 401 | 認証に失敗しました | 要求は認証チェックに合格しませんでした。 アクセストークンが見つからないか、無効です。 詳しくは、 [OAuthトークンエラーの節](#oauth-token-is-missing) 、以下を参照してください。 |
| 403 | 禁止 | リソースが見つかりましたが、表示に必要な資格情報がありません。 |
| 404 | 見つかりません | 要求されたリソースがサーバーで見つかりませんでした。 リソースが削除されたか、要求されたパスが正しく入力されていない可能性があります。 |
| 500 | 内部サーバーエラー | これはサーバー側のエラーです。 同時に多数の呼び出しを行う場合、APIの制限に達する可能性があり、結果をフィルターする必要があります。 (詳しくは、Catalog Service API開発者ガイドのデータ [フィルタリングに関するサブガイド](../catalog/api/filter-data.md) を参照してください)。 要求を再試行する前に少し待ってください。問題が解決しない場合は、管理者に問い合わせてください。 |

## 要求ヘッダーエラー {#request-header-errors}

プラットフォーム内のすべてのAPI呼び出しには、特定のリクエストヘッダーが必要です。 個々のサービスに必要なヘッダーを確認するには、 [APIリファレンスドキュメントを参照してください](https://www.adobe.io/apis/experienceplatform/home/api-reference.html)。 必要な認証ヘッダーの値を確認するには、 [認証のチュートリアル](../tutorials/authentication.md)を参照してください。 API呼び出しを行う際に、これらのヘッダーのいずれかが欠落しているか無効になっている場合は、次のエラーが発生する可能性があります。

### OAuthトークンが見つかりません {#oauth-token-is-missing}

```json
{
    "error_code": "403010",
    "message": "Oauth token is missing."
}
```

このエラーメッセージは、APIリクエストに `Authorization` ヘッダーがない場合に表示されます。 再試行する前に、認証ヘッダーが有効なアクセストークンに含まれていることを確認してください。

### OAuthトークンが無効です

```json
{
    "error_code": "401013",
    "message": "Oauth token is not valid"
}
```

このエラーメッセージは、ヘッダーに指定されたアクセストークンが有効でない場合に表示され `Authorization` ます。 トークンが正しく入力されていることを確認するか、Adobe I/Oコンソールで新しいトークン [を](../tutorials/authentication.md) 生成します。

### APIキーが必要です

```json
{
    "error_code": "403000",
    "message": "Api Key is required"
}
```

このエラーメッセージは、APIリクエストにAPIキーヘッダー(`x-api-key`)がない場合に表示されます。 もう一度やり直す前に、ヘッダーが有効なAPIキーに含まれていることを確認してください。

### APIキーが無効です

```json
{
    "error_code": "403003",
    "message": "Api Key is invalid"
}
```

このエラーメッセージは、指定されたAPIキーヘッダー(`x-api-key`)の値が無効な場合に表示されます。 キーが正しく入力されていることを確認してから、もう一度やり直してください。 APIキーがわからない場合は、 [Adobe I/Oコンソールで確認できます](https://console.adobe.io)。 「 **統合** 」タブで、特定の統合の「 **概要** 」セクションに移動し、「 **クライアント資格情報**」の下のAPIキーを探します。


### 見出しがありません

```json
{
    "error_code": "400003",
    "message": "Missing header"
}
```

このエラーメッセージは、IMS組織ヘッダー(`x-gw-ims-org-id`)がAPIリクエストに見つからない場合に表示されます。 もう一度やり直す前に、ヘッダーがIMS組織のIDに含まれていることを確認してください。

### プロファイルが無効です

```json
{
    "error_code": "403025",
    "message": "Profile is not valid"
}
```

このエラーメッセージは、ユーザーまたはAdobe I/O統合(ヘッダーの [アクセストークン](#how-do-i-get-an-access-token) によって識別される)が、ヘッダーに指定されたIMS組織に対するExperience Platform APIの呼び出しを行う権利を持っていない場合に表示され `Authorization``x-gw-ims-org-id` ます。 再試行する前に、ヘッダーでIMS組織の正しいIDを指定していることを確認してください。 組織IDがわからない場合は、 [Adobe I/Oコンソールで確認できます](https://console.adobe.io)。 「 **統合** 」タブで、特定の統合の「 **概要** 」セクションに移動し、「 **クライアント資格情報**」の下のIDを探します。

### 有効なコンテンツタイプが指定されていません

```json
{
    "type": "/placeholder/type/uri",
    "status": 400,
    "title": "BadRequestError",
    "detail": "A valid content-type must be specified"
}
```

このエラーメッセージは、POST、PUT、またはPATCHリクエストのヘッダーが無効か、入力されていない場合に表示され `Content-Type` ます。 ヘッダーがリクエストに含まれ、その値がであることを確認し `application/json`ます。


## サービストラブルシューティングディレクトリ {#service-troubleshooting-directory}

以下は、エクスペリエンスプラットフォームAPIのトラブルシューティングガイドとAPIリファレンスドキュメントのリストです。 各トラブルシューティングガイドは、個々のプラットフォームサービスに固有の問題に関するよくある質問と解決方法に対する回答を提供します。 APIリファレンスドキュメントでは、各サービスで使用可能なすべてのエンドポイントの包括的なガイドを提供し、サンプルのリクエスト本文、応答、エラーコードを表示します。

| サービス | API リファレンス | トラブルシューティング |
--- | --- | ---
| アクセス制御 | [アクセス制御API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/access-control.yaml) | [アクセス制御トラブルシューティングガイド](../access-control/troubleshooting-guide.md) |
| Catalog | [カタログサービスAPI](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/catalog.yaml) |  |
| データ収集（バッチ） | [データ取り込みAPI](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/ingest-api.yaml) | [Batch Ingestionトラブルシューティングガイド](../ingestion/batch-ingestion/troubleshooting.md) |
| データ取り込み（ストリーミング） | [データ取り込みAPI](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/ingest-api.yaml) | [ストリーミング取り込みのトラブルシューティングガイド](../ingestion/streaming-ingestion/troubleshooting.md) |
| Data Science Workspace | [Senesie Machine Learning API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/sensei-ml-api.yaml) | [Data Science Workspaceトラブルシューティングガイド](../data-science-workspace/troubleshooting-guide.md) |
| データ使用のラベル付けと実施(DULE) | [ポリシーサービスAPIのスケジュール](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/dule-policy-service.yaml) |  |
| エクスペリエンスデータモデル(XDM) | [スキーマレジストリAPI](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/schema-registry.yaml) | [XDMシステムに関するFAQとトラブルシューティングガイド](../xdm/troubleshooting-guide.md) |
| ID サービス | [IDサービスAPI](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/id-service-api.yaml) | [IDサービストラブルシューティングガイド](../identity-service/troubleshooting-guide.md) |
| クエリサービス | [クエリサービスAPI](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/qs-api.yaml) | [クエリサービストラブルシューティングガイド](../query-service/troubleshooting-guide.md) |
| リアルタイム顧客プロファイル | [リアルタイム顧客プロファイルAPI](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/real-time-customer-profile.yaml) |  |
| サンドボックス | [Sandbox API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/sandbox-api.yaml) | [サンドボックストラブルシューティングガイド](../sandboxes/troubleshooting-guide.md) |
| セグメント化 | [セグメントAPI](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/segmentation.yaml) |