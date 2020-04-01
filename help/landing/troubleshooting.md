---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Adobe Experience Platform FAQ and Troubleshooting Guide
topic: getting started
translation-type: tm+mt
source-git-commit: 7f61cee8fb5160d0f393f8392b4ce2462d602981

---


# プラットフォームに関するFAQとトラブルシューティングガイド

このドキュメントでは、Adobe Experience Platformに関するよくある質問と、Experience Platform APIで発生する可能性のある一般的なエラーの高度なトラブルシューティングガイドに回答します。 個々のプラットフォームサービスのトラブルシューティングガイドについては、以下のサービスのトラブルシューテ [ィングディレクトリを参照](#service-troubleshooting-directory) してください。

## FAQ {#faq}

次に、Adobe Experience Platformに関するよくある質問に対する回答をリストします。

## エクスペリエンスプラットフォームAPIとは {#what-are-experience-platform-apis}

エクスペリエンスプラットフォームオファーでは、複数のRESTful APIが使用され、HTTPリクエストを使用してプラットフォームリソースにアクセスします。 これらのサービスAPIは、それぞれ複数のエンドポイントを公開し、リスト(GET)、参照(GET)、編集（PUTまたはPATCH）および削除(DELETE)リソースに対する操作を実行できます。 各サービスで使用できる特定のエンドポイントと操作について詳しくは、Adobe I/Oに関する [APIリファレンスドキュメント](https://www.adobe.io/apis/experienceplatform/home/api-reference.html) を参照してください。

## APIリクエストの形式を設定する方法を教えてください。 {#how-do-i-format-an-api-request}

要求の形式は、使用するプラットフォームAPIによって異なります。 API呼び出しの構造を学ぶ最善の方法は、使用している特定のプラットフォームサービスのドキュメントに記載されている例に従うことです。

### API呼び出しの例を読み取り中

エクスペリエンスプラットフォームのドキュメントに、API呼び出しの例が2つの異なる方法で示されています。 最初に、呼び出しは **API形式で表され、操作(GET、POST**、PUT、PATCH、DELETEなど)と使用中のエンドポイント(例えば、 `/global/classes`)のみを示すテンプレート表現です。 また、テンプレートには、などの呼び出しの作成方法を示すために、変数の場所を示すものもありま `GET /{VARIABLE}/classes/{ANOTHER_VARIABLE}`す。

その後、呼び出しは、 **Request**（リクエスト）内のcURLコマンドとして表示されます。このコマンドには、APIとのやり取りに必要なヘッダーと完全な「ベースパス」が含まれます。 ベースパスは、すべてのエンドポイントに先頭に追加する必要があります。 例えば、前述のエンドポイント `/global/classes` がになりま `https://platform.adobe.io/data/foundation/schemaregistry/global/classes`す。 ドキュメント全体にAPI形式/リクエストパターンが表示され、独自のプラットフォームAPIを呼び出す場合は、例のリクエストに示す完全なパスを使用する必要があります。

### APIリクエストの例

以下は、ドキュメントで使用される形式を示すAPIリクエストの例です。

**API形式**

API形式は、使用されている操作(GET)とエンドポイントを示します。 変数は中括弧で示されます(この場合は `{CONTAINER_ID}`)。

```http
GET /{CONTAINER_ID}/classes
```

**リクエスト**

この例のリクエストでは、API形式の変数には、リクエストパス内の実際の値が与えられます。 必要なすべてのヘッダーと、サンプルのヘッダー値、または機密情報（セキュリティトークンやアクセスIDなど）を含める必要のある変数が表示されます。

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

この応答は、送信されたリクエストに基づいて、APIの呼び出しが成功した後に何を受け取るかを示します。 場合によっては、応答が空白で切り捨てられ、サンプルに表示される情報や追加情報が表示されることがあります。

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

必要なヘッダーやリクエスト本文を含む、プラットフォームAPIの特定のエンドポイントについて詳しくは、 [APIリファレンスのドキュメントを参照してくださ](https://www.adobe.io/apis/experienceplatform/home/api-reference.html)い。

## IMS組織とは何ですか。 {#what-is-my-ims-organization}

IMS組織は、顧客のアドビの代表です。 ライセンスを取得したアドビのソリューションは、このお客様の組織に統合されます。 IMS組織がExperience Platformの権利を付与されると、開発者にアクセスを割り当てることができます。 IMS組織ID(`x-gw-ims-org-id`)は、API呼び出しを実行する必要がある組織を表します。したがって、すべてのAPIリクエストのヘッダーとして必要です。 このIDは、 [Adobe I/Oコンソールで確認できます](https://console.adobe.io/)。「統合」タ **ブで** 、特定の統合の「概要 **」セクションに移動し、「クライアント資格情報」の下のID** を探します ****。 プラットフォームへの認証方法の詳しい手順については、認証のチュートリアルを参照 [してください](../tutorials/authentication.md)。

## APIキーはどこで入手できますか？ {#where-can-i-find-my-api-key}

APIキーは、すべてのAPIリクエストのヘッダーとして必要です。 これは、 [Adobe I/Oコンソールから入手できます](https://console.adobe.io/)。 コンソールの「統合」タブ **で** 、特定の統合の「概要 **」セクションに移動すると、「クライアント資格情報」の下にキーが** 表示されます ****。 プラットフォームへの認証方法の詳しい手順については、認証のチュートリアルを参照 [してください](../tutorials/authentication.md)。

## どうやってアクセストークンを？ {#how-do-i-get-an-access-token}

アクセストークンは、すべてのAPI呼び出しのAuthorizationヘッダーに必要です。 IMS組織の統合にアクセスで `curl` きる場合は、コマンドを使用して生成できます。 アクセストークンは24時間のみ有効で、その後APIを使用し続けるために新しいトークンを生成する必要があります。 認証の生成について詳しくは、アクセストークンのチュートリアルを [参照してくださ](../tutorials/authentication.md)い。

## パラメーターの使用クエリ方法 {#how-do-i-user-query-parameters}

一部のプラットフォームAPIエンドポイントは、クエリパラメーターを受け取って特定の情報を検索し、応答に返された結果をフィルターします。 クエリパラメーターは、リクエストパスに疑問符(`?`)記号が付加され、その後に1つ以上のクエリパラメーターが形式で追加されま `paramName=paramValue`す。 1回の呼び出しで複数のパラメーターを組み合わせる場合、アンパサンド(`&`)を使用して個々のパラメーターを区切る必要があります。 次の例は、複数のパラメーターを使用するリクエストが、ドキュメントでどのようにクエリされているかを示しています。

一般的に使用されるクエリパラメータの例：

```http
GET /tenant/schemas?orderby=title
GET /datasets?limit=36&start=10
GET /batches?createdAfter=1559775880000&orderBy=desc:created
```

特定のサービスまたはクエリで使用できるエンドポイントパラメーターの詳細については、サービス固有のドキュメントを参照してください。

## PATCHリクエストで更新するJSONフィールドを指定する方法を教えてください。 {#how-do-i-indicate-a-json-field-to-update-in-a-patch-request}

プラットフォームAPIの多くのPATCH操作では、 [JSON Pointer](https://tools.ietf.org/html/rfc6901) 文字列を使用して、更新するJSONプロパティを示します。 これらは通常、 [JSONパッチ形式を使用してリクエストペイロードに含まれます](https://tools.ietf.org/html/rfc6902) 。 これらのテクノロジーに必要な構文について詳しくは、 [](api-fundamentals.md) APIの基本原則ガイドを参照してください。

## Postmanを使用してプラットフォームAPIを呼び出すことはできますか。 {#how-do-i-use-postman-to-make-calls-to-platform-apis}

[Postmanは](https://www.getpostman.com/) 、RESTful APIへの呼び出しを視覚化する便利なツールです。 この投 [稿では](https://medium.com/adobetech/using-postman-for-jwt-authentication-on-adobe-i-o-7573428ffe7f) 、Postmanを自動的に認証を実行し、それを使用してエクスペリエンスプラットフォームAPIを利用する方法を説明します。

## プラットフォームの必要システム構成は何ですか。 {#what-are-the-system-requirements-for-platform}

UIとAPIのどちらを使用しているかによって、次の必要システム構成が適用されます。

**UIベースの操作の場合：**
- 最新の標準的なWebブラウザー。 最新バージョンのChromeをお勧めしますが、Firefoxの最新および以前のメジャーリリース、Internet Explorer、Safariもサポートされています。
   - 新しいメジャーバージョンがリリースされるたびに、最新バージョンをサポートするプラットフォーム開始と、3番目に新しいバージョンをサポートするプラットフォームバージョンが削除されます。
- すべてのブラウザーで、CookieとJavaScriptが有効になっている必要があります。

**APIおよび開発者のインタラクションの場合：**
- REST、ストリーミングおよびWebフック統合用に開発する開発環境。

## エラーとトラブルシューティング {#errors-and-troubleshooting}

以下は、Experience Platformサービスの使用時に発生する可能性のあるエラーのリストです。 個々のプラットフォームサービスのトラブルシューティングガイドについては、以下のサービスのトラブルシューテ [ィングディレクトリを参照](#service-troubleshooting-directory) してください。

## APIステータスコード {#api-status-codes}

以下のステータスコードは、どのエクスペリエンスプラットフォームAPIでも見つかる場合があります。 それぞれに様々な原因があり、本項で述べる説明は概して一般的である。 個々のプラットフォームサービスで発生する特定のエラーに関する詳細は、以下のサービスのトラブルシューティ [ングディレクトリを参照し](#service-troubleshooting-directory) てください。

| ステータスコード | 説明 | 考えられる原因 |
--- | --- | ---
| 400 | 不正な要求 | 要求が不適切に構築され、キー情報が欠落している、または正しくない構文が含まれていました。 |
| 401 | 認証に失敗しました | 要求が認証チェックに合格しませんでした。 お使いのアクセストークンが見つからないか、無効です。 詳しくは、 [OAuthトークンエラーの節](#oauth-token-is-missing) 、以下を参照してください。 |
| 403 | 禁止 | リソースが見つかりましたが、リソースを表示するための正しい資格情報がありません。 |
| 404 | 見つかりません | 要求されたリソースがサーバーで見つかりませんでした。 リソースが削除されたか、要求されたパスが正しく入力されていない可能性があります。 |
| 500 | 内部サーバーエラー | これはサーバー側のエラーです。 同時に多数の呼び出しを行う場合、APIの制限に達し、結果をフィルターする必要がある可能性があります。 (詳しくは、Catalog Service API開発者ガイドのデータのフィルタリングに関するサ [ブガイド](../catalog/api/filter-data.md) を参照してください)。要求を再試行する前にしばらく待ち、問題が解決しない場合は管理者に問い合わせてください。 |

## リクエストヘッダーエラー {#request-header-errors}

プラットフォーム内のすべてのAPI呼び出しには、特定のリクエストヘッダーが必要です。 個々のサービスに必要なヘッダーを確認するには、 [APIリファレンスのドキュメントを参照してくださ](https://www.adobe.io/apis/experienceplatform/home/api-reference.html)い。 必要な認証ヘッダーの値を確認するには、認証のチュートリアルを参照 [してください](../tutorials/authentication.md)。 API呼び出しを行う際に、これらのヘッダーのいずれかが見つからないか無効な場合は、次のエラーが発生する可能性があります。

### OAuthトークンがありません {#oauth-token-is-missing}

```json
{
    "error_code": "403010",
    "message": "Oauth token is missing."
}
```

このエラーメッセージは、APIリクエス `Authorization` トにヘッダーがない場合に表示されます。 再試行する前に、認証ヘッダーが有効なヘッダーに含まれていることをアクセストークンしてください。

### OAuthトークンが無効です

```json
{
    "error_code": "401013",
    "message": "Oauth token is not valid"
}
```

このエラーメッセージは、ヘッダーに指定されたアクセストークンが無効な `Authorization` 場合に表示されます。 トークンが正しく入力されていることを確認す [るか、Adobe I/O](../tutorials/authentication.md) Consoleで新しいトークンを生成します。

### APIキーが必要です

```json
{
    "error_code": "403000",
    "message": "Api Key is required"
}
```

このエラーメッセージは、APIリクエストにAPIキーヘッダー(`x-api-key`)がない場合に表示されます。 再試行する前に、ヘッダーが有効なAPIキーに含まれていることを確認してください。

### APIキーが無効です

```json
{
    "error_code": "403003",
    "message": "Api Key is invalid"
}
```

このエラーメッセージは、指定されたAPIキーヘッダー(`x-api-key`)の値が無効な場合に表示されます。 もう一度やり直す前に、キーが正しく入力されていることを確認してください。 APIキーがわからない場合は、 [Adobe I/Oコンソールで確認できます](https://console.adobe.io)。「統合」 **タブで** 、特定の統合の「概要 **」セクションに移動し、「クライアント資格情報」の下のAPIキーを** 検索します ****。


### 見つからないヘッダ

```json
{
    "error_code": "400003",
    "message": "Missing header"
}
```

このエラーメッセージは、IMS組織ヘッダー(`x-gw-ims-org-id`)がAPIリクエストに存在しない場合に表示されます。 再試行する前に、ヘッダーがIMS組織のIDに含まれていることを確認してください。

### プロファイルが無効です

```json
{
    "error_code": "403025",
    "message": "Profile is not valid"
}
```

このエラーメッセージは、ユーザーまたはAdobe I/O統合(ヘッダーの [アクセストークン](#how-do-i-get-an-access-token) ( `Authorization` によって識別)が、ヘッダーで提供されたIMS組織のExperience Platform APIを呼び出す権利がない場合に表示さ `x-gw-ims-org-id` れます。 再試行する前に、ヘッダーにIMS組織の正しいIDが指定されていることを確認してください。 組織IDが不明な場合は、 [Adobe I/O Consoleで確認できます](https://console.adobe.io)。「統合」タ **ブで** 、特定の統合の「概要 **」セクションに移動し、「クライアント資格情報」の下でIDを** 検索します ****。

### 有効なコンテンツタイプが指定されていません

```json
{
    "type": "/placeholder/type/uri",
    "status": 400,
    "title": "BadRequestError",
    "detail": "A valid content-type must be specified"
}
```

このエラーメッセージは、POST、PUT、またはPATCHリクエストのヘッダーが無効か、見つからない場合に表示さ `Content-Type` れます。 ヘッダーがリクエストに含まれ、その値がであることを確認しま `application/json`す。


## サービストラブルシューティングディレクトリ {#service-troubleshooting-directory}

以下は、Experience Platform APIのトラブルシューティングガイドとAPIリファレンスドキュメントのリストです。 各トラブルシューティングガイドは、個々のプラットフォームサービスに固有のよくある質問と問題の解決方法を示します。 APIリファレンスドキュメントは、各サービスで使用可能なすべてのエンドポイントの包括的なガイドを提供し、受け取る可能性のあるリクエストの本文、応答およびエラーコードのサンプルを示します。

| サービス | API リファレンス | トラブルシューティング |
--- | --- | ---
| アクセス制御 | [アクセス制御API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/access-control.yaml) | [アクセス制御トラブルシューティングガイド](../access-control/troubleshooting-guide.md) |
| Catalog | [カタログサービスAPI](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/catalog.yaml) |  |
| データ収集（バッチ） | [データ取り込みAPI](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/ingest-api.yaml) | [バッチ取り込みのトラブルシューティングガイド](../ingestion/batch-ingestion/troubleshooting.md) |
| データ取り込み（ストリーミング） | [データ取り込みAPI](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/ingest-api.yaml) | [ストリーミング取り込みのトラブルシューティングガイド](../ingestion/streaming-ingestion/troubleshooting.md) |
| Data Science Workspace | [Senesi Machine Learning API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/sensei-ml-api.yaml) | [Data Science Workspaceトラブルシューティングガイド](../data-science-workspace/troubleshooting-guide.md) |
| データ使用のラベル付けと実施(DULE) | [ポリシーサービスAPIのスケジュール](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/dule-policy-service.yaml) |  |
| エクスペリエンスデータモデル(XDM) | [スキーマレジストリAPI](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/schema-registry.yaml) | [XDMシステムに関するFAQとトラブルシューティングガイド](../xdm/troubleshooting-guide.md) |
| ID サービス | [IDサービスAPI](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/id-service-api.yaml) | [IDサービストラブルシューティングガイド](../identity-service/troubleshooting-guide.md) |
| クエリサービス | [クエリサービスAPI](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/qs-api.yaml) | [クエリサービストラブルシューティングガイド](../query-service/troubleshooting-guide.md) |
| リアルタイム顧客プロファイル | [リアルタイム顧客プロファイルAPI](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/real-time-customer-profile.yaml) |  |
| サンドボックス | [Sandbox API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/sandbox-api.yaml) | [サンドボックストラブルシューティングガイド](../sandboxes/troubleshooting-guide.md) |
| セグメント化 | [セグメントAPI](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/segmentation.yaml) |