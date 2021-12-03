---
title: エラー処理
description: Reactor API でのエラーの処理方法を説明します。
exl-id: 336c0ced-1067-4519-94e1-85aea700fce6
source-git-commit: f3c23665229a83d6c63c7d6026ebf463069d8ad9
workflow-type: ht
source-wordcount: '1068'
ht-degree: 100%

---

# エラー処理

Reactor API を呼び出す際に問題が発生した場合は、次のいずれかの方法でエラーが返されることがあります。

* **即時エラー**：即時エラーになるリクエストを実行すると、API によってエラー応答が返され、HTTP ステータスには、発生した一般的なタイプのエラーが反映されます。
* **遅延エラー**：遅延エラー（非同期アクティビティなど）を引き起こす API リクエストを実行すると、API から関連リソースの `meta.status_details` でエラーが返される場合があります。

## エラー形式

エラー応答は、[JSON:API エラーの仕様](http://jsonapi.org/format/#errors)に準拠することを目的とし、通常は次の構造に従います。

```json
{
  "errors": [
    {
      "id": "8a5526da-ab12-4be9-b084-2efe537f388c",
      "status": "404",
      "code": "not-found",
      "title": "Record Not Found",
      "meta": {
        "request_id": "jfb0dQ2e0XVTkQ6AOfEJFfTDjguw9x3d"
      },
      "source": {
        "pointer": "/data"
      }
    }
  ]
}
```

| プロパティ | 説明 |
| --- | --- |
| `id` | この問題の発生に対する一意の ID です。 |
| `status` | この問題に適用される HTTP ステータスコードを文字列値で表します。 |
| `code` | アプリケーション固有のエラーコードで、文字列値で表されます。 |
| `title` | ローカリゼーション目的以外で、発生から次の発生までの間に&#x200B;**変更すべきでない**、人間が判読可能な、問題の短い要約です。 |
| `detail` | この問題の発生に固有の、人間が判読可能な説明です。`title` と同様に、このフィールドの値はローカライズできます。 |
| `source` | エラーのソースへの参照を含むオブジェクトです。オプションで、次のメンバーのいずれかを含みます。<ul><li>`pointer`：要求ドキュメント内の関連エンティティを参照する [JSON ポインター（RFC6901）](https://datatracker.ietf.org/doc/html/rfc6901) 文字列（例えば、プライマリデータオブジェクトの `/data` や、特定の属性の `/data/attributes/title`）。</li></ul> |
| `meta` | エラーに関する非標準のメタデータを含むオブジェクトです。 |

{style=&quot;table-layout:auto&quot;}

## エラーリファレンス

次の表に、API が返す様々なエラーを示します。

| エラータイトル | 説明 |
| --- | --- |
| `authentication-failure` | IMS アクセストークンが無効です。再度ログインすると、新しいアクセストークンを取得できます。または、テクニカルアカウントの場合、新しい JWT を生成し、IMS アクセストークンを入れ替えます。 |
| `connection-refused` | サーバーへの接続を確立できませんでした。 |
| `decrypt-bad-passphrase` | 指定されたパスフレーズでデータを復号化できませんでした。 |
| `decrypt-failed` | 指定された秘密キーでデータを復号化できませんでした。キーがローカルで機能し、空白が削除されていることを確認します。 |
| `decrypt-no-data` | 秘密キーがないと、データを復号化できません。暗号化された秘密キーを指定してください。 |
| `delegate-descriptor-unresolved` | 拡張機能は、このデリゲート記述子の予期された定義を提供しませんでした。 拡張機能の更新が必要になる場合があります。 |
| `deleted-resources` | ライブラリに追加しようとしているリソースが削除されました。 |
| `environment-in-use` | 環境は、一度に 1 つのライブラリにのみ割り当てることができます。オプション 1 は、別の環境を選択することです。オプション 2 は、ライブラリを別の環境に移動するか、ライブラリを削除して、この環境を解放することです。 |
| `environment-required` | ビルドを作成するには、ライブラリに環境が割り当てられている必要があります。 |
| `extension-not-found` | データ要素またはルールコンポーネントを定義する拡張機能は、ライブラリに含まれません。必要な拡張機能がすべてライブラリに追加されていることを確認します。 |
| `extension-package-path-error` | extension.json で定義されたパスが正しく構築されませんでした。 |
| `extension-package-transform-definition-error` | オブジェクトプロパティに対して無効な変換を定義しました。各オブジェクトプロパティには、ファイル変換または関数変換のいずれかを 1 つ定義できます。 |
| `extension-package-zip-error` | ExtensionPackage を解凍中、または配布用にファイルを圧縮中にエラーが発生しました。 |
| `host-in-use` | 1 つ以上の環境がホストを使用している場合、ホストは削除されない場合があります。 |
| `host-required` | このライブラリに割り当てられた環境に有効なホストがありません。ライブラリに割り当てられている環境を確認します。次に、その環境に有効なホストを割り当てます。 |
| `host-type-error` | SFTP ホストのみが使用前に認証情報を検証する必要があるため、事前テストはそのホストタイプでのみ使用できます。 |
| `illegal-custom-code-transform` | customCode 変換を使用することはできません。関数またはファイル変換を指定してください。 |
| `ims-not-authorized` | アカウントの認証中に不明なエラーが発生しました。後でもう一度やり直してください。 |
| `ims-session-error` | ログインセッションに問題があります。ログアウトしてから、もう一度ログインしてください。 |
| `internal-error` | 内部エラーが発生しました。数分待ってから、もう一度お試しください。問題が解決しない場合は、クライアントケアにお問い合わせください。 |
| `invalid-data_element` | 無効なデータ要素をライブラリに追加できません。 |
| `invalid-embed_code` | 有効な埋め込みコードでないか、開発環境またはステージング環境にリンクしようとしています。Dynamic Tag Management（DTM）埋め込みコードは、実稼動環境にのみリンクできます。 |
| `invalid-extension` | 無効な拡張機能をライブラリに追加することはできません。 |
| `invalid-extension_package_id` | 拡張機能パッケージのオブジェクトプロパティの一部のみを変更できます。許可されていないプロパティを変更しようとしました。 |
| `invalid-new-owner-org-id` | 割り当てようとした組織 ID が有効な組織 ID ではありません。 |
| `invalid-org` | アクティブな組織が API へのアクセス権を持っていません。正しい組織を使用していることを確認します。 |
| `invalid-rule` | 無効なルールをライブラリに追加することはできません。 |
| `invalid-settings-syntax` | 設定 JSON の解析中に構文エラーが発生しました。 |
| `library-file-not-found` | extension.json で定義された必須のファイルが zip パッケージ内に見つかりませんでした。 |
| `minification-error` | 無効なコードが原因で、コードをコンパイルできませんでした。 |
| `multiple-revisions` | 1 つのライブラリに含めることができるリビジョンは、各リソースで 1 つだけです。 |
| `no-available-orgs` | このユーザーアカウントは、タグへのアクセス権を持つ製品プロファイルに属していません。Admin Console を使用して、タグ権限を持つ製品プロファイルにこのユーザーを追加します。 |
| `not-authorized` | このユーザーアカウントには、この操作を実行するために必要な権限がありません。 |
| `not-found` | レコードが見つかりませんでした。取得しようとしているオブジェクトの ID を検証します。 |
| `not-unique` | 使用しようとしている名前は既に使用中です。このリソースでは、「name」プロパティは一意である必要があります。 |
| `public-release-not-authorized` | 拡張機能の公開リリースは `launch-ext-dev@adobe.com` が調整します。 詳しくは、[拡張機能のリリース](../../extension-dev/submit/release.md)に関するドキュメントを参照してください。 |
| `read-only` | これは読み取り専用で、変更できません。 |
| `session-timeout` | ユーザーセッションの有効期限が切れました。ログアウトしてから、もう一度ログインしてください。 |
| `sftp-authentication-failed` | SFTP 接続の認証に失敗しました。 |
| `sftp-connection-timeout` | SFTP 接続がタイムアウトしました。 |
| `sftp-exception` | SFTP を使用してサーバーに接続する際に例外が発生しました。 |
| `sftp-status-exception` | サーバーとの通信中に SFTP 例外が発生しました。 |
| `socket-error` | サーバーとの通信中にソケットエラーが発生しました。 |
| `ssh-disconnect` | SSH セッションが切断されました。 |
| `timeout-error` | サーバーとの接続がタイムアウトしました。 |
| `unknown-error` | 予期しないエラーが発生しました。後でもう一度試すか、カスタマーケアに連絡して、発生時の操作を説明してもらうことができます。 |
| `unsupported-custom-code-language` | サポートされていないカスタムコード言語が指定されました。 |
| `upgraded-extension-required` | 拡張機能のアップグレードをインストールしたら、アップグレードが実稼動環境に移行するまで、そのアップグレードをすべてのライブラリに含める必要があります。唯一の例外は、拡張機能がまだ公開されていない場合です。 |
| `upstream-build-required` | これをビルドする前に、アップストリームライブラリを正常にビルドする必要があります。 |

{style=&quot;table-layout:auto&quot;}
