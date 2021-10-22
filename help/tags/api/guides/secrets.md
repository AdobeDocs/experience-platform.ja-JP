---
title: Reactor API のシークレット
description: イベント転送で使用するために、Reactor API でシークレットを設定する方法について説明します。
source-git-commit: 6822199c3ecf4414893a8b8dfc650e3da40a6470
workflow-type: tm+mt
source-wordcount: '1115'
ht-degree: 3%

---

# Reactor API のシークレット

Reactor API では、秘密は認証資格情報を表すリソースです。 セキュリティで保護されたデータ交換が別のシステムに認証されるようにするために、イベント転送にシークレットが使用されます。 したがって、シークレットは、イベント転送プロパティ (属性が「」 `platform` に設定されたプロパティ) 内にのみ作成でき `edge` ます。

現在は、次の3つの属性で表されるシークレットタイプがサポートされてい `type_of` ます。

| シークレットタイプ | 説明 |
| --- | --- |
| `token` | 両方のシステムによって認識され、認識される認証トークン値を表す1つのストリング。 |
| `simple-http` | には、それぞれ、ユーザー名とパスワードの2つの string 属性が含まれています。 |
| `oauth2` | には、OAuth 認証仕様をサポートするための属性が含まれてい [ ](https://datatracker.ietf.org/doc/html/rfc6749) ます。 イベントのフォワーディングにより、必要な情報を入力するよう求めるメッセージが表示されます。次に、指定した間隔でこれらのトークンを更新することができます。 |

{style=&quot;table-layout:auto&quot;}

このガイドでは、イベント転送で使用するためにシークレットを設定する方法の概要を説明しています。 Reactor API の機密情報を管理する方法について詳しくは、「機密情報管理ガイド」を参照してください [ ](../endpoints/secrets.md) 。

## 資格情報

各シークレットには、 `credentials` それぞれの資格情報の値を保持する属性が含まれています。 各タイプの機密情報には、次に示すように、必要な属性が異なります。

### `token`

次の場合にのみ、値を持つシークレットに `type_of` `token` 1 つの属性を指定する必要があり `credentials` ます。

| Credential 属性 | データタイプ | 説明 |
| --- | --- | --- |
| `token` | 文字列 | 宛先システムによって認識される秘密トークン。 |

{style = &quot;テーブル-layout: auto&quot;}

トークンは静的な値として格納されます。したがって、シークレットは、作成される `expires_at` `refresh_at` ときに設定され `null` ます。

### `simple-http`

値が指定されているシークレットに `type_of` `simple-http` は、次の属性が必要 `credentials` です。

| Credential 属性 | データタイプ | 説明 |
| --- | --- | --- |
| `username` | 文字列 | ユーザー名です。 |
| `password` | 文字列 | パスワードを入力します。 この値は、API 応答には含まれません。 |

{style = &quot;テーブル-layout: auto&quot;}

シークレットを作成すると、2つの属性は BASE64 エンコードと共に交換され `username:password` ます。 交換が完了すると、secret `expires_at` および `refresh_at` プロパティはに設定され `null` ます。

### `oauth2`

>[!NOTE]
>
>現在、 [ OAuth シークレットには、クライアントの資格情報許可タイプのみ ](https://www.oauth.com/oauth2-servers/access-tokens/client-credentials/) がサポートされています。

値が指定されているシークレットに `type_of` `oauth2` は、次の属性が必要 `credentials` です。

| Credential 属性 | データタイプ | 説明 |
| --- | --- | --- |
| `client_id` | 文字列 | OAuth 統合のクライアント ID。 |
| `client_secret` | 文字列 | OAuth 統合のクライアントシークレット この値は、API 応答には含まれません。 |
| `authorization_url` | 文字列 | OAuth 統合の認証 URL です。 |
| `refresh_offset` | 整数 | *(オプション)* 更新操作のオフセットを秒単位で指定します。 シークレットの作成時にこの属性を省略した場合、初期設定では、値は `14400` (4 時間) に設定されます。 |
| `options` | オブジェクト | *(オプション)* OAuth 統合の追加オプションを指定します。<ul><li>`scope`: [ 資格情報の OAuth 2.0 スコープを表すストリング ](https://oauth.net/2/scope/) 。</li><li>`audience`: [ Auth0 アクセストークンを表すストリング ](https://auth0.com/docs/protocols/protocol-oauth2) 。</li></ul> |

`oauth2`シークレットが作成または更新されると、 `client_id` および (場合によっては) は、 `client_secret` `options` `authorization_url` OAuth プロトコルのクライアント資格情報フローに従って、and (および、場合によっては) がへの POST 要求で交換されます。

>[!NOTE]
>
>承認サービスの応答本文が OAuth プロトコルと互換性があることを想定しています。

承認サービスと JSON 応答本文の両方が応答する場合は、 `200 OK` 本文が解析され、 `access_token` が edge 環境にプッシュされ、 `expires_in` `expires_at` 秘密の and 属性を計算するために使用され `refresh_at` ます。 シークレットに環境アソシエーションがない場合 `access_token` は、が破棄されます。

資格情報の交換は、次のような条件下で成功したと見なされます。

* `expires_in` (8 時間) より大きい場合に指定 `28800` します。
* `refresh_offset` は、 `expires_in` -(4 時間) の値よりも小さいことを示し `14400` ます。 例えば、 `expires_in` is `36000` (10 時間)、a `refresh_offset` `28800` (8 時間) は、 `28800` -() より大きいため、交換は失敗したものと見なされ `36000` `14400` `21600` ます。

交換が成功した場合は、シークレットの状態属性がに設定され、 `succeeded` の値がに `expires_at` `refresh_at` 設定されます。

* `expires_at` には、現在の UTC 時刻にの値を指定し `expires_in` ます。
* `refresh_at` には、現在の UTC 時刻に、の値を負の値で指定 `expires_in` `refresh_offset` します。 例えば、 `expires_in` is `43200` (12 時間) および `refresh_offset` is (4 時間) を指定した場合、このプロパティは `14400` 現在の `refresh_at` `28800` UTC 時刻以降 (8 時間) に設定されます。

何らかの理由で exchange に障害が発生した場合は、オブジェクトの属性が関連性のある `status_details` `meta` 情報に更新されます。

### シークレットの更新 `oauth2`

`oauth2`環境にシークレットが割り当てられていて、その状態が `succeeded` (資格情報が正しく交換されたときに) ある場合は、新しい交換が自動的に実行され `refresh_at` ます。

交換が成功した場合 `refresh_status` は、オブジェクトの属性 `meta` が while、and に設定され、 `succeeded` それに `expires_at` `refresh_at` `activated_at` 応じて更新されます。

Exchange に障害が発生した場合、操作はあと3回試行されます。これにより、アクセストークンの有効期限が切れるまでに最後の2時間を超えていました。 すべての試みが失敗した場合は、 `refresh_status_details` オブジェクトの属性が `meta` 関連する詳細情報によって更新されます。

## 環境関係

シークレットを作成する際には、 [ ](../endpoints/environments.md) そのファイルが存在する環境を指定する必要があります。 シークレットは、それらが作成された環境に直ちに展開されます。

シークレットは、1つの環境にのみ関連付けることができます。 シークレットと環境の関係が確立されると、その機密情報は環境からクリアされず、シークレットは別の環境に関連付けられることはありません。

>[!NOTE]
>
>このルールに対する唯一の例外は、問題の環境が削除された場合です。 このような場合は、関係が削除され、他の環境に割り当てられることがあります。

シークレット情報が交換された後、環境にシークレットが関連付けられるようにするために、その環境には exchange のアーティファクト (トークンストリング、Base64 でエンコードされた `token` ストリング、 `simple-http` またはアクセストークン `oauth2` ) が安全に保存されます。

Exchange アーティファクトが環境に正常に保存された後に、secret の `activated_at` 属性が現在の UTC 時刻に設定され、データエレメントを使用して参照できるようになりました。 シークレットの [ 参照について詳しくは、次の項を参照してください ](#referencing-secrets) 。

## シークレットの参照 {#referencing-secrets}

シークレットを参照するには、「secret」タイプのデータエレメントを event フォワーディングプロパティに作成しておく必要があります。これは、  中核拡張機能によって提供され [ ](../../extensions/web/core/overview.md) ます。 このデータエレメントを設定する場合は、環境によって使用されるシークレットを指定するよう求めるメッセージが表示されます。 次に、HTTP 呼び出しのヘッダー内など、秘匿データエレメントを参照するルールを作成できます。

![秘匿データエレメント](../../images/api/guides/secrets/data-element.png)

>[!NOTE]
>
>秘匿データエレメントをライブラリに追加するには、 `succeeded` そのライブラリが構築されている環境に1つ以上のシークレットが関連付けられている必要があります。 例えば、ライブラリに含まれている機密データエレメントに、ステージングシークレットセクションに対するシークレットが設定されていないものがある場合、 `succeeded`  ステージング環境でそのライブラリを構築しようとすると、エラーが発生します。

実行時に、秘匿データエレメントは、環境に保存された、対応する secret exchange アーティファクトに置き換えられます。

## 次の手順

このガイドでは、Reactor API を使用したシークレットの操作について説明しています。 API 呼び出しを使用してシークレットを管理する方法について詳しくは、『 [ シークレット endpoint guide 』を参照してください ](../endpoints/secrets.md) 。
