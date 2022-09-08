---
title: Reactor API の秘密鍵
description: イベント転送で使用する Reactor API の秘密鍵を設定する方法の基本について説明します。
exl-id: 0298c0cd-9fba-4b54-86db-5d2d8f9ade54
source-git-commit: 24e79c14268b9eab0e8286eb8cd1352c1dfcd1b6
workflow-type: tm+mt
source-wordcount: '1241'
ht-degree: 87%

---

# Reactor API の秘密鍵

Reactor API では、秘密鍵は認証情報を表すリソースです。秘密鍵は、安全なデータ交換のために別のシステムに認証するために、イベント転送で使用されます。したがって、秘密鍵は、イベント転送プロパティ（`platform` 属性が `edge` に設定されているプロパティ）内でのみ作成できます。

現在、`type_of` 属性で示されているサポートされている秘密鍵タイプは次の 3 つです。

| 秘密鍵タイプ | 説明 |
| --- | --- |
| `token` | 両方のシステムで認識および理解されている認証トークン値を表す単一の文字列。 |
| `simple-http` | ユーザー名とパスワードの 2 つの文字列属性がそれぞれ含まれます。 |
| `oauth2-client_credentials` | [OAuth](https://datatracker.ietf.org/doc/html/rfc6749) 認証仕様をサポートする複数の属性が含まれます。イベント転送では、必要な情報を要求され、指定された間隔でトークンの更新を処理します。 |

{style=&quot;table-layout:auto&quot;}

このガイドでは、イベント転送で使用する秘密鍵の設定方法の概要を説明します。秘密鍵の構造の JSON の例など、Reactor API で秘密鍵を管理する方法のガイダンスについて詳しくは、 [秘密鍵エンドポイントガイド](../endpoints/secrets.md)を参照してください。

## 資格情報

各秘密鍵には、それぞれの認証情報の値を保持する `credentials` 属性が含まれます。条件 [API でのシークレットの作成](../endpoints/secrets.md#create)の場合、各タイプのシークレットには、以下の節で示すように、必要な属性が異なります。

* [`token`](#token)
* [&#39;simple-http&#39;](#simple-http)
* [&#39;oauth2-client_credentials&#39;](#oauth2-client_credentials)
* [&#39;oauth2-google&#39;](#oauth2-google)

### `token` {#token}

`type_of` の値が `token` の秘密鍵には、`credentials` の下に 1 つの属性のみが必要です。

| 認証情報属性 | データタイプ | 説明 |
| --- | --- | --- |
| `token` | 文字列 | 宛先システムによって認識される秘密鍵トークン。 |

{style=&quot;table-layout:auto&quot;}

トークンは静的な値として保存されるため、秘密鍵の作成時に秘密鍵の `expires_at` および `refresh_at` プロパティは `null` に設定されます。

### `simple-http` {#simple-http}

`type_of` の値が `simple-http` の秘密鍵には、`credentials` の下に次の属性が必要です。

| 認証情報属性 | データタイプ | 説明 |
| --- | --- | --- |
| `username` | 文字列 | ユーザー名。 |
| `password` | 文字列 | パスワード。 この値は API 応答には含まれません。 |

{style=&quot;table-layout:auto&quot;}

秘密鍵が作成されると、2 つの属性は `username:password` の BASE64 エンコーディングで交換されます。交換後、秘密鍵の `expires_at` および `refresh_at` プロパティは `null` に設定されます。

### `oauth2-client_credentials` {#oauth2-client_credentials}

`type_of` の値が `oauth2-client_credentials` の秘密鍵には、`credentials` の下に次の属性が必要です。

| 認証情報属性 | データタイプ | 説明 |
| --- | --- | --- |
| `client_id` | 文字列 | OAuth 統合のクライアント ID。 |
| `client_secret` | 文字列 | OAuth 統合用のクライアント秘密鍵。この値は API 応答には含まれません。 |
| `token_url` | 文字列 | OAuth 統合の認証 URL。 |
| `refresh_offset` | 整数 | *（オプション）*&#x200B;更新操作をオフセットする値（秒単位）。秘密鍵の作成時にこの属性を省略すると、デフォルトで値は `14400`（4 時間）に設定されます。 |
| `options` | オブジェクト | *（オプション）* OAuth 統合の追加オプションを指定します。<ul><li>`scope`：認証情報の [OAuth 2.0 スコープ](https://oauth.net/2/scope/)を表す文字列。</li><li>`audience`：[Auth0 アクセストークン](https://auth0.com/docs/protocols/protocol-oauth2)を表す文字列</li></ul> |

`oauth2-client_credentials` 秘密鍵が作成または更新されると、OAuth プロトコルのクライアント認証情報フローに従って、`client_id` と `client_secret`（および場合によっては `options`）が POST リクエストで `token_url` に交換されます。

>[!NOTE]
>
>承認サービス応答本文は、OAuth プロトコルと互換性があることが想定されます。

承認サービスが `200 OK` と JSON 応答本文で応答する場合、本文が解析され、`access_token` がエッジ環境にプッシュされ、`expires_in` を使用して秘密鍵の `expires_at` 属性と `refresh_at` 属性が計算されます。秘密鍵に環境の関連付けがない場合、 `access_token` は破棄されます。

認証情報の交換は、次の条件下で成功したと見なされます。

* `expires_in` は `28800`（8 時間）より大きい値です。
* `refresh_offset` は、`expires_in` から `14400` を引いた値（4 時間）未満です。例えば、`expires_in` が `36000`（10 時間）で、`refresh_offset` が `28800`（8 時間）の場合、`28800` は `36000` - `14400`（`21600`）より大きいため交換は失敗したとみなされます。

正常に交換されると、秘密鍵のステータス属性が `succeeded` に設定され、`expires_at` と `refresh_at` の値が設定されます。

* `expires_at` は、現在の UTC 時間に `expires_in` の値を加えたものです。
* `refresh_at` は、現在の UTC 時間に `expires_in` の値を加え、`refresh_offset` の値を引いたものです。例えば、`expires_in` が `43200`（12 時間）で、`refresh_offset` が `14400`（4 時間）の場合、`refresh_at` プロパティは現在の UTC 時間から `28800`（8 時間）に設定されます。

何らかの理由で交換が失敗した場合、`meta` オブジェクトの `status_details` 属性が関連情報で更新されます。

#### `oauth2-client_credentials` 秘密鍵の更新

`oauth2-client_credentials` 秘密鍵が環境に割り当てられ、そのステータスが（認証情報が正常に交換された）`succeeded` の場合、`refresh_at` で新しい交換が自動的に実行されます。

交換が成功すると、`meta` オブジェクトの `refresh_status` 属性が `succeeded` に設定され、`expires_at`、`refresh_at` および `activated_at` が更新されます。

交換が失敗した場合、操作はさらに 3 回試行され、最後の試行はアクセストークンの有効期限が切れる 2 時間前までに行われます。すべての試行が失敗した場合、`meta` オブジェクトの `refresh_status_details` 属性が関連する詳細で更新されます。

### `oauth2-google` {#oauth2-google}

を使用したシークレット `type_of` 値 `oauth2-google` には、以下の属性が必要です。 `credentials`:

| 認証情報属性 | データタイプ | 説明 |
| --- | --- | --- |
| `scopes` | 配列 | 認証用のGoogle製品範囲を一覧表示します。 次のスコープがサポートされています。<ul><li>[Google 広告](https://developers.google.com/google-ads/api/docs/oauth/overview): `https://www.googleapis.com/auth/adwords`</li><li>[Google Pub/Sub](https://cloud.google.com/pubsub/docs/reference/service_apis_overview): `https://www.googleapis.com/auth/pubsub`</li></ul> |

作成後 `oauth2-google` シークレット。応答には `meta.authorization_url` プロパティ。 Google認証フローを完了するには、この URL をコピーしてブラウザーに貼り付ける必要があります。

#### 再認証： `oauth2-google` 秘密鍵

の認証 URL `oauth2-google` シークレットは、シークレットの作成後 1 時間で有効期限が切れます ( `meta.authorization_url_expires_at`) をクリックします。 この時間が過ぎると、認証プロセスを更新するために秘密鍵を再認証する必要があります。

詳しくは、 [secrets エンドポイントガイド](../endpoints/secrets.md#reauthorize) 再認証方法の詳細 `oauth2-google` シークレットを保持する必要があります。

## 環境の関係

秘密鍵を作成するときは、秘密鍵が存在する[環境](../endpoints/environments.md)を指定する必要があります。秘密鍵は、作成された環境に直ちにデプロイされます。

秘密鍵は、1 つの環境にのみ関連付けることができます。秘密鍵と環境の関係が確立されると、秘密鍵を環境からクリアすることはできず、秘密鍵を別の環境に関連付けることもできません。

>[!NOTE]
>
>このルールの唯一の例外は、問題の環境が削除された場合です。この場合、関係は解除され、秘密鍵を別の環境に割り当てることができます。

秘密鍵の認証情報が正常に交換された後、秘密鍵を環境に関連付けるには、交換アーティファクト（`token` トークン文字列、`simple-http`の Base64 エンコード文字列または `oauth2-client_credentials` のアクセストークン）を、環境に安全に保存します。

交換アーティファクトが環境に正常に保存されると、秘密鍵の `activated_at` 属性が現在の UTC 時間に設定され、データ要素を使用して参照できるようになります。秘密鍵の参照について詳しくは、[次の節](#referencing-secrets)を参照してください。

## 秘密鍵の参照 {#referencing-secrets}

秘密鍵を参照するには、イベント転送プロパティに「[!UICONTROL 秘密鍵]」タイプのデータ要素（[[!UICONTROL コア]拡張機能](../../extensions/web/core/overview.md)によって提供される）を作成する必要があります。このデータ要素を設定する際に、各環境で使用する秘密鍵を指定するよう求められます。その後、HTTP 呼び出しのヘッダー内など、秘密鍵のデータ要素を参照するルールを作成できます。

![秘密鍵データ要素](../../images/api/guides/secrets/data-element.png)

>[!NOTE]
>
>秘密鍵データ要素をライブラリに追加するには、ライブラリが構築されている環境に関連付けられた `succeeded` 秘密鍵が 1 つ以上必要です。例えば、ライブラリの秘密鍵データ要素に、[!UICONTROL ステージング秘密鍵]セクション用に設定された `succeeded` 秘密鍵がないものがある場合、ステージング環境でそのライブラリをビルドしようとすると、エラーが発生します。

実行時、秘密鍵データ要素は、環境に保存されている対応する秘密鍵交換アーティファクトに置き換えられます。

## 次の手順

このガイドでは、Reactor API で秘密鍵を操作するための基本について説明しました。API 呼び出しを使用して秘密鍵を管理する方法について詳しくは、[秘密鍵エンドポイントガイド](../endpoints/secrets.md)を参照してください。
