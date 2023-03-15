---
title: クラウドコネクタ拡張機能の概要
description: Adobe Experience Platform の Cloud Connector イベント転送拡張機能について説明します。
exl-id: f3713652-ac32-4171-8dda-127c8c235849
source-git-commit: c7344d0ac5b65c6abae6a040304f27dc7cd77cbb
workflow-type: tm+mt
source-wordcount: '1616'
ht-degree: 100%

---

# クラウドコネクタ拡張機能の概要

>[!NOTE]
>
>Adobe Experience Platform Launch は、Adobe Experience Platform のデータ収集テクノロジースイートとしてリブランドされています。 その結果、製品ドキュメント全体でいくつかの用語が変更されました。用語の変更点の一覧については、次の[ドキュメント](../../../term-updates.md)を参照してください。

Cloud Connector イベント転送拡張機能を使用すると、宛先にデータを送信したり、宛先からデータを取得したりするためのカスタム HTTP リクエストを作成できます。Cloud Connector の拡張機能は、Postman を Adobe Experience Platform Edge ネットワークに配置する場合と似ており、専用の拡張機能を持たないエンドポイントへのデータ送信に使用できます。

このリファレンスは、この拡張機能を使用してルールを作成するときに使用できるオプションに関する情報です。

## Cloud Connector 拡張機能のアクションタイプ

この節では、Adobe Experience Platform の Cloud Connector 拡張機能で使用できる「データを送信」アクションタイプについて説明します。

### リクエストタイプ

エンドポイントに必要なリクエストのタイプを選択するには、 [!UICONTROL リクエストタイプ]ドロップダウンで適切なタイプを選択します。

| メソッド | 説明 |
|---|---|
| [GET](https://developer.mozilla.org/ja-JP/docs/Web/HTTP/Methods/GET) | 指定したリソースの表現を要求します。GET を使用したリクエストでは、データのみが取得されます。 |
| [POST](https://developer.mozilla.org/ja-JP/docs/Web/HTTP/Methods/POST) | 指定されたリソースにエンティティを送信します。多くの場合、状態が変更されたり、サーバーに副作用が発生したりします。 |
| [PUT](https://developer.mozilla.org/ja-JP/docs/Web/HTTP/Methods/PUT) | ターゲットリソースの現在の表現をすべてリクエストのペイロードで置き換えます。 |
| [PATCH](https://developer.mozilla.org/ja-JP/docs/Web/HTTP/Methods/PATCH) | リソースに部分的な変更を適用します。 |
| [DELETE](https://developer.mozilla.org/ja-JP/docs/Web/HTTP/Methods/DELETE) | 指定したリソースを削除します。 |

### エンドポイント URL

「リクエストタイプ」ドロップダウンメニューの横にあるテキストフィールドに、データを送信するエンドポイントの URL を入力します。

### クエリパラメーター、ヘッダー、および本文の設定

各タブ（「クエリのパラメーター」、「ヘッダー」および「本文のデータ要素」）を使用して、特定のエンドポイントに送信するデータを制御します。

#### クエリのパラメーター

クエリ文字列パラメーターとして送信するキーと値のペアごとにキーと値を定義します。データ要素を手動で入力するには、中括弧を使用して、イベント転送向けにデータ要素をトークン化します。 「siteSection」という名前のデータ要素の値をキーまたは値として参照するには、「`{{siteSection}}`」と入力します。または、ドロップダウンメニューで以前に作成したデータ要素を選択します。

クエリのパラメーターを追加するには、「**[!UICONTROL さらに追加]**」を選択してください。

#### ヘッダー

ヘッダーとして送信するキーと値のペアごとにキーと値を定義します。データ要素を手動で入力するには、中括弧を使用して、イベント転送向けにデータ要素をトークン化します。 「pageName」という名前のデータ要素の値をキーまたは値として参照するには、「`{{pageName}}`」と入力します。または、ドロップダウンメニューで以前に作成したデータ要素を選択します。

ヘッダーを追加するには、「**[!UICONTROL さらに追加]**」を選択します。

次の表に、定義済みのヘッダーのリストを示します。これらのヘッダーに制限されることなく、必要に応じて独自のカスタムヘッダーを追加し、必要に応じて使用可能にできます。

>[!NOTE]
>
>これらのヘッダーの詳細については、[https://developer.mozilla.org/ja-JP/docs/Web/HTTP/Headers](https://developer.mozilla.org/ja-JP/docs/Web/HTTP/Headers) を参照してください。

| ヘッダー | 説明 |
|---|---|
| [A-IM](https://developer.mozilla.org/ja-JP/docs/Web/HTTP/Headers/Accept) |  |
| [Accept](https://developer.mozilla.org/ja-JP/docs/Web/HTTP/Headers/Accept) |  |
| [Accept-Charset](https://developer.mozilla.org/ja-JP/docs/Web/HTTP/Headers/Accept-Charset) |  |
| [Accept-Encoding](https://developer.mozilla.org/ja-JP/docs/Web/HTTP/Headers/Accept-Encoding) |  |
| [Accept-Language](https://developer.mozilla.org/ja-JP/docs/Web/HTTP/Headers/Accept-Language) |  |
| [Accept-Datetime](https://developer.mozilla.org/ja-JP/docs/Web/HTTP/Headers/Accept) | 元のリソースの過去の状態にアクセスしたい旨を示すため、ユーザエージェントによって送信されます。このため、`Accept-Datetime` ヘッダーは、元のリソースの TimeGate に対して発行される HTTP リクエストで伝送され、その値はアクセスを希望する元のリソースの過去の状態の日時を示します。 |
| Access-Control-Request-Headers | [プリフライトリクエスト](https://developer.mozilla.org/ja-jp/docs/Glossary/preflight_request)を発行する際にブラウザーによって使用され、実際のリクエストの際に、クライアントがどの [HTTP ヘッダー](https://developer.mozilla.org/ja-JP/docs/Web/HTTP/Headers)を送信する可能性があるかがサーバーに通知されます。 |
| Access-Control-Request-Method | [プリフライトリクエスト](https://developer.mozilla.org/ja-jp/docs/Glossary/preflight_request)を発行する際にブラウザーによって使用され、実際のリクエストの際に使用される [HTTP メソッド](https://developer.mozilla.org/ja-JP/docs/Web/HTTP/Methods)がサーバーに通知されます。プリフライトリクエストは常に[オプション](https://developer.mozilla.org/ja-JP/docs/Web/HTTP/Methods/OPTIONS)であり、実際のリクエストと同じメソッドを使用しないので、このヘッダーが必要です。 |
| Authorization | サーバーでユーザーエージェントを認証するための資格情報が含まれます。 |
| [Cache-Control](https://developer.mozilla.org/ja-JP/docs/Web/HTTP/Headers/Cache-Control) | リクエストと応答の両方のキャッシュメカニズム用のディレクティブ。 |
| [接続](https://developer.mozilla.org/ja-JP/docs/Web/HTTP/Headers/Connection) | 現在のトランザクションが終了した後、ネットワーク接続を開いたままにするかどうかを制御します。 |
| [Content-Length](https://developer.mozilla.org/ja-JP/docs/Web/HTTP/Headers/Content-Length) | リソースのサイズ（10 進数バイト数）。 |
| [Content-Type](https://developer.mozilla.org/ja-JP/docs/Web/HTTP/Headers/Content-Type) | リソースのメディアタイプを示します。 |
| cookie | サーバーによって [`Set-Cookie`](https://developer.mozilla.org/ja-JP/docs/Web/HTTP/Headers/Set-Cookie) ヘッダーで以前に送信された、保存済み [HTTP Cookie](https://developer.mozilla.org/ja-JP/docs/Web/HTTP/Cookies) が含まれます。 |
| Date | 一般的な HTTP ヘッダーには、メッセージの送信元の日時が含まれます。 |
| [DNT](https://developer.mozilla.org/ja-JP/docs/Web/HTTP/Headers/DNT) | ユーザーのトラッキングの設定を示します。 |
| Expect | リクエストを適切に処理するためにサーバーが満たす必要があると考えられる事項を示します。 |
| Forwarded | [リバースプロキシサーバー](https://developer.mozilla.org/ja-JP/docs/Web/HTTP/Proxy_servers_and_tunneling)からの情報が含まれます。この情報は、プロキシがリクエストのパスに関連している場合に変更または失われます。 |
|  から | 要求元のユーザーエージェントを制御するユーザーのインターネット電子メールアドレスが含まれます。 |
| ホスト | リクエストの送信先サーバーのホスト番号とポート番号を指定します。 |
| If-Match |  |
| If-Modified-Since |  |
| [If-None-Match](https://developer.mozilla.org/ja-JP/docs/Web/HTTP/Headers/If-None-Match) |  |
| [if-Range](https://developer.mozilla.org/ja-JP/docs/Web/HTTP/Headers/If-Range) |  |
| [If-Unmodified-Since](https://developer.mozilla.org/ja-JP/docs/Web/HTTP/Headers/If-Unmodified-Since) |  |
| [Max-Forwards](https://developer.mozilla.org/ja-JP/docs/Web/HTTP/Headers/If-Unmodified-Since) |  |
| [Origin](https://developer.mozilla.org/ja-JP/docs/Web/HTTP/Headers/Origin) |  |
| [Pragma](https://developer.mozilla.org/ja-JP/docs/Web/HTTP/Headers/Pragma) | 実装固有のヘッダー。リクエスト応答チェーンの任意の場所で様々な効果を持つ場合があります。Cache-Control ヘッダーが存在しない HTTP/1.0 キャッシュとの下位互換性を確保するために使用されます。 |  |
| [Proxy-Authorization](https://developer.mozilla.org/ja-JP/docs/Web/HTTP/Headers/Proxy-Authorization) |
| [範囲](https://developer.mozilla.org/ja-JP/docs/Web/HTTP/Headers/Range) | サーバーが返すドキュメントの一部を示します。 |  |
| [参照元](https://developer.mozilla.org/ja-JP/docs/Web/HTTP/Headers/Referer) | 現在要求されているページへのリンク元である、前の Web ページのアドレス。 |  |
| TE | ユーザーエージェントが受け入れる転送エンコーディングを指定します。（非公式に、より直感的に `Accept-Transfer-Encoding` と呼ぶこともできます） |
| Upgrade | [`Upgrade` ヘッダーフィールドに関連する RFC ドキュメントは、RFC 7230、6.7 項](https://tools.ietf.org/html/rfc7230#section-6.7)です。この規格では、現在のクライアント、サーバー、転送プロトコル接続で、別のプロトコルにアップグレードまたは変更するためのルールが定められています。例えば、このヘッダーの規格では、クライアントは HTTP 1.1 から HTTP 2.0 に変更できます（サーバーで `Upgrade` ヘッダーフィールドを確認して実装する場合）。 どちらも、`Upgrade` ヘッダーフィールドについて指定されている条項に同意する必要はありません。クライアントヘッダーとサーバーヘッダーの両方で使用できます。`Upgrade` ヘッダーフィールドを指定した場合、送信者は `upgrade` オプションを指定して `Connection` ヘッダーフィールドも送信する必要があります。 |  |
| [User-Agent](https://developer.mozilla.org/ja-JP/docs/Web/HTTP/Headers/User-Agent) | ネットワークプロトコルピアが、要求元のソフトウェアユーザーエージェントのアプリケーションタイプ、オペレーティングシステム、ソフトウェアベンダー、またはソフトウェアバージョンを識別できる、特徴的な文字列が含まれます。 |
| [Via](https://developer.mozilla.org/ja-JP/docs/Web/HTTP/Headers/Via) | フォワードプロキシとリバースプロキシの両方によって追加されます。これは、リクエストヘッダーと応答ヘッダーに表示できます。 |
| [警告](https://developer.mozilla.org/ja-JP/docs/Web/HTTP/Headers/Warning) | 発生する可能性がある問題についての一般的な警告情報。 |
| X-CSRF-Token |  |
| X-Requested-With |  |

#### 本文（JSON）

リクエストの本文で送信するキーと値のペアごとに、キーと値を定義します。データ要素を手動で入力するには、中括弧を使用して、イベント転送向けにデータ要素をトークン化します。 「appSection」という名前のデータ要素の値をキーまたは値として参照するには、「`{{appSection}}`」と入力します。または、ドロップダウンメニューで以前に作成したデータ要素を選択します。

キーと値のペアを追加するには、「**[!UICONTROL さらに追加]**」を選択します。

#### 本文（生）

リクエストの本文で送信するキーと値のペアごとに、キーと値を定義します。データ要素を手動で入力するには、中括弧を使用して、イベント転送向けにデータ要素をトークン化します。 「appSection」という名前のデータ要素の値をキーまたは値として参照するには、「`{{appSection}}`」と入力します。または、ドロップダウンメニューで以前に作成したデータ要素を選択します。1 つ以上のデータ要素を追加できます。

### アドバンス

イベント転送のルール内のアクションは順番に実行されます。 クライアントからの着信イベントにないデータを外部ソースから取得し、この応答を受け取って、同じルール内の後続アクションでデータを変換する、または最終的な宛先に送信する必要がある場合があります。詳細セクションの「リクエスト応答を保存」で、この処理を有効にできます。

エンドポイントからの応答の本文を保存するには、「 **[!UICONTROL リクエストの応答を保存]**」ボックスをチェックして、テキストフィールドで応答キーを定義します。

応答キーを `productDetails` として定義した場合は、このデータをデータ要素で参照し、同じルール内の後続のアクションでこのデータ要素を参照します。`productDetail` を参照するデータ要素を作成するには、タイプ `path` のデータ要素を作成し、次のパスを入力します。

```Json
arc.ruleStash.[EXTENSION-NAME-HERE].responses.[RESPONSE-KEY-HERE] 

arc.ruleStash.adobe-cloud-connector.reponses.productDetails 
```
