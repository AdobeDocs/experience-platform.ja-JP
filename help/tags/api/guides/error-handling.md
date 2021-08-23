---
title: エラー処理
description: Reactor API でのエラーの処理方法について説明します。
source-git-commit: 6a1728bd995137a7cd6dc79313762ae6e665d416
workflow-type: ht
source-wordcount: '1071'
ht-degree: 100%

---

# エラー処理

Reactor API を呼び出す際に問題が発生した場合は、次のいずれかの方法でエラーが返されることがあります。

* **即時エラー**：リクエストの実行時に即時エラーが発生した場合は、API によってエラー応答が返され、HTTP ステータスは発生した一般的なタイプのエラーを反映します。
* **遅延エラー**：API リクエストの実行時に遅延エラー（非同期アクティビティなど）が発生した場合、関連リソースの `meta.status_details` で API によってエラーが返されることがあります。

## エラーの形式

エラー応答は、[JSON:API のエラーの仕様](http://jsonapi.org/format/#errors)に準拠することを目指しており、通常は次の構造になっています。

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
| `status` | この問題に適用される HTTP ステータスコード。文字列値で表されます。 |
| `code` | アプリケーション固有のエラーコード。文字列値で表されます。 |
| `title` | 問題の概要を人間が判読できる文字で短く表したもの。ローカリゼーション目的以外では&#x200B;**変更しないで**&#x200B;ください。 |
| `detail` | この問題の発生に固有の、人間が判読できる説明。`title` と同様に、このフィールドの値はローカライズできます。 |
| `source` | エラーのソースへの参照を含むオブジェクト。次のメンバーのいずれかが含まれていることがあります。<ul><li>`pointer`：リクエストドキュメント内の関連エンティティを参照する [JSON ポインター（RFC6901）](https://datatracker.ietf.org/doc/html/rfc6901)文字列（プライマリデータオブジェクトの場合は `/data`、特定の属性の場合は `/data/attributes/title`）。</li></ul> |
| `meta` | エラーに関する非標準のメタデータを含むオブジェクト。 |

{style=&quot;table-layout:auto&quot;}

## エラーのリファレンス

次の表に、API が返す様々なエラーを示します。

| エラータイトル | 説明 |
| --- | --- |
| `authentication-failure` | IMS アクセストークンが無効です。再度ログインすると、新しいアクセストークンを取得できます。または、テクニカルアカウントの場合は、新しい JWT を生成し、IMS アクセストークンと入れ替えます。 |
| `connection-refused` | サーバーへの接続を確立できませんでした。 |
| `decrypt-bad-passphrase` | 指定されたパスフレーズでデータを復号化できませんでした。 |
| `decrypt-failed` | 指定された秘密キーでデータを復号化できませんでした。キーがローカルで機能し、空白が削除されていることを確認します。 |
| `decrypt-no-data` | 秘密キーがないと、データを復号化できません。 暗号化された秘密キーを指定してください。 |
| `delegate-descriptor-unresolved` | 拡張機能は、このデリゲート記述子に求められている定義を提供しませんでした。拡張機能の更新が必要になる場合があります。 |
| `deleted-resources` | ライブラリに追加しようとしているリソースが削除されています。 |
| `environment-in-use` | 各環境に割り当てることのできるライブラリは、一度に 1 つのみです。1 つめのオプションは、別の環境を選択することです。2 つめのオプションは、ライブラリを別の環境に移動するかライブラリを削除して、この環境を解放することです。 |
| `environment-required` | ビルドを作成するには、ライブラリに環境が割り当てられている必要があります。 |
| `extension-not-found` | データ要素またはルールコンポーネントを定義する拡張機能がライブラリに含まれていません。必要な拡張機能がすべてライブラリに追加されていることを確認します。 |
| `extension-package-path-error` | extension.json で定義されたパスが正しく構築されませんでした。 |
| `extension-package-transform-definition-error` | オブジェクトプロパティに対して無効な変換を定義しました。各オブジェクトプロパティには、ファイル変換または関数変換のうちの 1 つを定義できます。 |
| `extension-package-zip-error` | ExtensionPackage の解凍中または配布用ファイルの圧縮中にエラーが発生しました。 |
| `host-in-use` | 1 つ以上の環境がホストを使用している場合、そのホストを削除することはできません。 |
| `host-required` | このライブラリに割り当てられた環境には有効なホストがありません。ライブラリに割り当てられている環境を確認します。次に、その環境に有効なホストを割り当てます。 |
| `host-type-error` | 使用前に認証情報を確認する必要があるのは SFTP ホストのみです。このため、事前テストはそのホストタイプでのみ使用できます。 |
| `illegal-custom-code-transform` | customCode 変換を使用することはできません。関数またはファイル変換を指定してください。 |
| `ims-not-authorized` | アカウントの認証中に不明なエラーが発生しました。後でもう一度お試しください。 |
| `ims-session-error` | ログインセッションに問題があります。ログアウトしてから、もう一度ログインしてください。 |
| `internal-error` | 内部エラーが発生しました。数分待ってから、もう一度お試しください。問題が解決しない場合は、カスタマーケアにお問い合わせください。 |
| `invalid-data_element` | 無効なデータ要素をライブラリに追加することはできません。 |
| `invalid-embed_code` | 有効な埋め込みコードでないか、開発環境またはステージング環境にリンクしようとしています。Dynamic Tag Management（DTM）の埋め込みコードは、実稼動環境にのみリンクできます。 |
| `invalid-extension` | 無効な拡張機能をライブラリに追加することはできません。 |
| `invalid-extension_package_id` | 変更できるのは、拡張機能パッケージのオブジェクトプロパティの一部のみです。許可されていないものを変更しようとしました。 |
| `invalid-new-owner-org-id` | 割り当てようとした組織 ID は有効な組織 ID ではありません。 |
| `invalid-org` | アクティブな組織は API にアクセスできません。正しい組織を使用していることを確認します。 |
| `invalid-rule` | 無効なルールをライブラリに追加することはできません。 |
| `invalid-settings-syntax` | 設定 JSON の解析中に構文エラーが発生しました。 |
| `library-file-not-found` | extension.json で定義された必須ファイルが zip パッケージ内に見つかりませんでした。 |
| `minification-error` | 無効なコードまたは ES6 コードが原因で、コードをコンパイルできませんでした。 |
| `multiple-revisions` | 1 つのライブラリには、各リソースの 1 つのリビジョンのみを含めることができます。 |
| `no-available-orgs` | このユーザーアカウントは、タグへのアクセス権を持つ製品プロファイルに属していません。Admin Console を使用して、タグ権限を持つ製品プロファイルにこのユーザーを追加します。 |
| `not-authorized` | このユーザーアカウントには、この操作の実行に必要な権限がありません。 |
| `not-found` | レコードが見つかりませんでした。取得しようとしているオブジェクトの ID を確認します。 |
| `not-unique` | 使用しようとしている名前は既に使用されています。このリソースでは、「name」プロパティは一意である必要があります。 |
| `public-release-not-authorized` | 拡張機能の公開リリースは `launch-ext-dev@adobe.com` によって調整されます。詳しくは、[拡張機能のリリース](../../extension-dev/submit/release.md)に関するドキュメントを参照してください。 |
| `read-only` | このリソースは読み取り専用で、変更できません。 |
| `session-timeout` | ユーザーセッションの有効期限が切れました。ログアウトしてから、もう一度ログインしてください。 |
| `sftp-authentication-failed` | SFTP 接続の認証に失敗しました。 |
| `sftp-connection-timeout` | SFTP 接続がタイムアウトしました。 |
| `sftp-exception` | SFTP を使用してサーバーに接続する際に例外が発生しました。 |
| `sftp-status-exception` | サーバーとの通信を試行中に SFTP 例外が発生しました。 |
| `socket-error` | サーバーとの通信の試行中にソケットエラーが発生しました。 |
| `ssh-disconnect` | SSH セッションが切断されました。 |
| `timeout-error` | サーバーとの接続がタイムアウトしました。 |
| `unknown-error` | 予期しないエラーが発生しました。後でもう一度試すか、カスタマーケアに連絡して、発生時の操作を説明します。 |
| `unsupported-custom-code-language` | サポートされていないカスタムコード言語が指定されました。 |
| `upgraded-extension-required` | 拡張機能のアップグレードをインストールしたら、アップグレードが実稼動になるまで、そのアップグレードをすべてのライブラリに含める必要があります。唯一の例外は、拡張機能がまだ公開されていない場合です。 |
| `upstream-build-required` | これをビルドするには、アップストリームライブラリを正常にビルドする必要があります。 |

{style=&quot;table-layout:auto&quot;}
