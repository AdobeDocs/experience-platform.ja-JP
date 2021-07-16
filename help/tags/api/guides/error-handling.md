---
title: エラー処理
description: Reactor APIでのエラーの処理方法を説明します。
source-git-commit: 6a1728bd995137a7cd6dc79313762ae6e665d416
workflow-type: tm+mt
source-wordcount: '1071'
ht-degree: 1%

---

# エラー処理

Reactor APIを呼び出す際に問題が発生した場合は、次のいずれかの方法でエラーが返されることがあります。

* **即時エラー**:即時エラーになるリクエストを実行すると、APIによってエラー応答が返され、HTTPステータスには、発生した一般的なタイプのエラーが反映されます。
* **遅延エラー**:遅延エラー（非同期アクティビティなど）を引き起こすAPIリクエストを実行すると、APIから関連リソースのでエラーが返される場 `meta.status_details` 合があります。

## エラー形式

エラー応答は、[JSON:APIエラーの仕様](http://jsonapi.org/format/#errors)に準拠することを目的とし、通常は次の構造に従います。

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
| `id` | この問題の発生に対する一意の識別子。 |
| `status` | この問題に適用されるHTTPステータスコードを文字列値で表します。 |
| `code` | アプリケーション固有のエラーコードで、文字列値で表されます。 |
| `title` | ローカリゼーション目的以外で、**を発生から発生に変更しない、人間が読み取り可能な問題の短い要約です。** |
| `detail` | この問題の発生に固有の、人間が読み取り可能な説明。 `title`と同様に、このフィールドの値はローカライズできます。 |
| `source` | エラーのソースへの参照を含むオブジェクト。オプションで、次のメンバーのいずれかを含みます。<ul><li>`pointer`:要求ドキュメ [ント内の関連エンティティを参照する](https://datatracker.ietf.org/doc/html/rfc6901) JSONポインター(RFC6901) `/data` 文字列(例えば、プライマリデータオブジェクトの場合 `/data/attributes/title` や、特定の属性の場合)。</li></ul> |
| `meta` | エラーに関する非標準のメタデータを含むオブジェクト。 |

{style=&quot;table-layout:auto&quot;}

## エラーリファレンス

次の表に、APIが返す様々なエラーを示します。

| エラータイトル | 説明 |
| --- | --- |
| `authentication-failure` | IMSアクセストークンが無効です。 再度サインインすると、新しいアクセストークンを取得できます。 または、テクニカルアカウントの場合、新しいJWTを生成し、IMSアクセストークンを入れ替えます。 |
| `connection-refused` | サーバーへの接続を確立できませんでした。 |
| `decrypt-bad-passphrase` | 指定されたパスフレーズでデータを復号化できませんでした。 |
| `decrypt-failed` | 指定された秘密鍵でデータを復号化できませんでした。 キーがローカルで機能し、空白が削除されていることを確認します。 |
| `decrypt-no-data` | 秘密鍵がないと、データを復号化できません。 暗号化された秘密鍵を指定してください。 |
| `delegate-descriptor-unresolved` | 拡張機能は、このデリゲート記述子の予期された定義を提供しませんでした。 拡張機能の更新が必要になる場合があります。 |
| `deleted-resources` | ライブラリに追加しようとしているリソースが削除されました。 |
| `environment-in-use` | 環境は、一度に1つのライブラリにのみ割り当てることができます。 オプション1は、別の環境を選択することです。 オプション2は、ライブラリを別の環境に移動するか、ライブラリを削除して、この環境を解放します。 |
| `environment-required` | ビルドを作成するには、ライブラリに環境が割り当てられている必要があります。 |
| `extension-not-found` | データ要素またはルールコンポーネントを定義する拡張機能は、ライブラリに含まれません。 必要な拡張機能がすべてライブラリに追加されていることを確認します。 |
| `extension-package-path-error` | extension.jsonで定義されたパスが正しく構築されませんでした。 |
| `extension-package-transform-definition-error` | オブジェクトプロパティに対して無効な変換を定義しました。 各オブジェクトプロパティには、1つの変換を定義でき、ファイル変換または関数変換にする必要があります。 |
| `extension-package-zip-error` | ExtensionPackageの解凍中または配布用のファイルの圧縮中にエラーが発生しました。 |
| `host-in-use` | 1つ以上の環境がホストを使用している場合、ホストは削除されない場合があります。 |
| `host-required` | このライブラリに割り当てられた環境に有効なホストがありません。 ライブラリに割り当てられている環境を確認します。 次に、その環境に有効なホストを割り当てます。 |
| `host-type-error` | SFTPホストのみが資格情報を確認してから使用できるので、事前テストはそのホストタイプでのみ使用できます。 |
| `illegal-custom-code-transform` | customCode変換を使用することはできません。 関数またはファイル変換を指定してください。 |
| `ims-not-authorized` | アカウントの認証中に不明なエラーが発生しました。 後でもう一度やり直してください。 |
| `ims-session-error` | サインインセッションに問題があります。 ログアウトしてから、もう一度ログインしてください。 |
| `internal-error` | 内部エラーが発生しました。 数分待ってから、もう一度お試しください。 問題が解決しない場合は、ClientCareにお問い合わせください。 |
| `invalid-data_element` | 無効なデータ要素をライブラリに追加できません。 |
| `invalid-embed_code` | 有効な埋め込みコードでないか、開発環境またはステージング環境にリンクしようとしています。 Dynamic Tag Management(DTM)埋め込みコードは、実稼動環境にのみリンクできます。 |
| `invalid-extension` | 無効な拡張機能をライブラリに追加することはできません。 |
| `invalid-extension_package_id` | 拡張機能パッケージのオブジェクトプロパティの一部のみを変更できます。 許可されていないものの1つを変更しようとしました。 |
| `invalid-new-owner-org-id` | 割り当てようとした組織IDが有効な組織IDではありません。 |
| `invalid-org` | アクティブな組織はAPIにアクセスできません。 正しい組織を使用していることを確認します。 |
| `invalid-rule` | 無効なルールをライブラリに追加することはできません。 |
| `invalid-settings-syntax` | 設定JSONの解析中に構文エラーが発生しました。 |
| `library-file-not-found` | extension.jsonで定義された必須のファイルがzipパッケージ内に見つかりませんでした。 |
| `minification-error` | 無効なコードまたはES6コードが原因で、コードをコンパイルできませんでした。 |
| `multiple-revisions` | 1つのライブラリに含めることができるリビジョンは、各リソースの1つだけです。 |
| `no-available-orgs` | このユーザーアカウントは、タグへのアクセス権を持つ製品プロファイルに属していません。 「 」Admin Consoleを使用して、タグ権限を持つ製品プロファイルにこのユーザーを追加します。 |
| `not-authorized` | このユーザーアカウントには、この操作を実行するために必要な権限がありません。 |
| `not-found` | レコードが見つかりませんでした。 取得しようとしているオブジェクトのIDを確認します。 |
| `not-unique` | 使用しようとしている名前は既に使用中です。 このリソースでは、&#39;name&#39;プロパティは一意である必要があります。 |
| `public-release-not-authorized` | 拡張機能の公開リリースは`launch-ext-dev@adobe.com`が調整します。 詳しくは、[拡張機能のリリース](../../extension-dev/submit/release.md)に関するドキュメントを参照してください。 |
| `read-only` | このリソースは読み取り専用で、変更できません。 |
| `session-timeout` | ユーザーセッションの有効期限が切れました。 ログアウトしてから、もう一度ログインしてください。 |
| `sftp-authentication-failed` | SFTP接続の認証に失敗しました。 |
| `sftp-connection-timeout` | SFTP接続がタイムアウトしました。 |
| `sftp-exception` | SFTPを使用してサーバーに接続する際に例外が発生しました。 |
| `sftp-status-exception` | サーバーとの通信中にSFTP例外が発生しました。 |
| `socket-error` | サーバーと通信中にソケットエラーが発生しました。 |
| `ssh-disconnect` | SSHセッションが切断されました。 |
| `timeout-error` | サーバーとの接続がタイムアウトしました。 |
| `unknown-error` | 予期しないエラーが発生しました。 後でもう一度試すか、カスタマーケアに連絡して、発生時の操作を説明してもらうことができます。 |
| `unsupported-custom-code-language` | サポートされていないカスタムコード言語が指定されました。 |
| `upgraded-extension-required` | 拡張機能のアップグレードをインストールしたら、アップグレードが実稼動環境に移行するまで、そのアップグレードをすべてのライブラリに含める必要があります。 唯一の例外は、拡張機能がまだ公開されていない場合です。 |
| `upstream-build-required` | アップストリームライブラリを正常にビルドするには、アップストリームライブラリをビルドする前にが必要です。 |

{style=&quot;table-layout:auto&quot;}
