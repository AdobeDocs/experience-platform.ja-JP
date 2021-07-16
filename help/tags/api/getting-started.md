---
title: Reactor APIの概要
description: 必要なアクセス資格情報を生成する手順など、Reactor APIの使用を開始する方法について説明します。
source-git-commit: 6a1728bd995137a7cd6dc79313762ae6e665d416
workflow-type: tm+mt
source-wordcount: '1070'
ht-degree: 2%

---

# Reactor APIの概要

[Reactor API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/reactor.yaml)を使用するには、各リクエストに次の認証ヘッダーを含める必要があります。

* `Authorization: Bearer {ACCESS_TOKEN}`
* `x-api-key: {API_KEY}`
* `x-gw-ims-org-id: {IMS_ORG}`

このガイドでは、Adobe開発者コンソールを使用して各ヘッダーの値を収集し、Reactor APIの呼び出しを開始する方法について説明します。

## Adobe Experience Platformへの開発者アクセスの獲得

Reactor APIの認証値を生成するには、まず開発者がExperience Platformにアクセスできる必要があります。 開発者アクセスを得るには、[認証に関するチュートリアル](http://www.adobe.com/go/platform-api-authentication-en)の最初の手順に従ってください。 「Adobe開発者コンソールでアクセス資格情報を生成する」の手順に進んだら、このチュートリアルに戻ってReactor API専用の資格情報を生成します。

## アクセス資格情報の生成

Adobe開発者コンソールを使用して、次の3つのアクセス資格情報を生成する必要があります。

* `{IMS_ORG}`
* `{API_KEY}`
* `{ACCESS_TOKEN}`

IMS組織のID(`{IMS_ORG}`)とAPIキー(`{API_KEY}`)は、最初に生成された後、今後のAPI呼び出しで再利用できます。 ただし、アクセストークン(`{ACCESS_TOKEN}`)は一時的なもので、24時間ごとに再生成する必要があります。

これらの値を生成する手順については、以下で詳しく説明します。

### 1回限りのセットアップ

[Adobe開発者コンソール](https://www.adobe.com/go/devs_console_ui)に移動し、Adobe IDでログインします。 次に、開発者コンソールのドキュメントの[空のプロジェクト](https://www.adobe.io/apis/experienceplatform/console/docs.html#!AdobeDocs/adobeio-console/master/projects-empty.md)の作成に関するチュートリアルで説明されている手順に従います。

プロジェクトを作成したら、**プロジェクトの概要**&#x200B;画面で「**API**&#x200B;を追加」を選択します。

![](../images/api/getting-started/add-api-button.png)

**Add an API**&#x200B;画面が表示されます。 「**次へ**」を選択する前に、使用可能なAPIのリストから「**Experience PlatformリアクターAPI**」を選択します。

![](../images/api/getting-started/add-launch-api.png)

次の画面で、新しいキーペアを生成するか、独自の公開鍵をアップロードするJSON Webトークン(JWT)秘密鍵証明書を作成するよう求められます。 このチュートリアルでは、「**キーペアを生成**」オプションを選択し、右下隅で「**キーペアを生成**」を選択します。

![](../images/api/getting-started/create-jwt.png)

次の画面で、キーペアが正常に生成されたことを確認し、公開証明書と秘密鍵を含む圧縮フォルダーが自動的にマシンにダウンロードされます。 この秘密鍵は、後の手順でアクセストークンを生成するために必要です。

「**次へ**」を選択して続行します。

![](../images/api/getting-started/keypair-generated.png)

次の画面で、API統合に関連付ける1つ以上の製品プロファイルを選択するよう求めるプロンプトが表示されます。

>[!NOTE]
>
>製品プロファイルは、Adobe Admin Consoleを通じて組織で管理され、Adobe Experience Platform Launchの詳細な機能に関する特定の権限セットが含まれます。 製品プロファイルとその権限は、組織内の管理者権限を持つユーザーのみが管理できます。 API用に選択する製品プロファイルが不明な場合は、管理者に問い合わせてください。

リストから目的の製品プロファイルを選択し、「**設定済みAPIを保存**」を選択してAPIの登録を完了します。

![](../images/api/getting-started/select-product-profile.png)

APIをプロジェクトに追加すると、プロジェクトページがExperience PlatformReactor APIページに再び表示されます。 ここから、「**サービスアカウント(JWT)**」セクションまでスクロールします。このセクションには、Reactor APIへのすべての呼び出しで必要な次のアクセス資格情報が表示されます。

* **クライアントID**:クライアントIDは必須で、 `{API_KEY}` ヘッダーで指定する必要があ `x-api-key` ります。
* **組織ID**:組織IDは、ヘッ `{IMS_ORG}` ダーで使用する必要がある値 `x-gw-ims-org-id` です。

![](../images/api/getting-started/access-creds.png)

### 各セッションの認証

`{API_KEY}`と`{IMS_ORG}`の値が揃ったので、最後の手順で`{ACCESS_TOKEN}`値が生成されます。

>[!NOTE]
>
>これらのトークンは、24時間後に期限切れになります。 アプリケーションにこの統合を使用する場合は、アプリケーション内からBearerトークンをプログラムで取得することをお勧めします。

使用事例に応じて、アクセストークンを生成する方法は2つあります。

* [トークンの手動生成](#manual)
* [プログラムによるトークンの生成](#program)

#### アクセストークンの手動生成 {#manual}

前にダウンロードした秘密鍵をテキストエディターまたはブラウザーで開き、その内容をコピーします。 次に、開発者コンソールに戻り、「**トークン**&#x200B;を生成」を選択する前に、プロジェクトのReactor APIページの「**アクセストークン**&#x200B;を生成」セクションに秘密鍵を貼り付けます。

![](../images/api/getting-started/paste-private-key.png)

新しいアクセストークンが生成され、トークンをクリップボードにコピーするためのボタンが提供されます。 この値は、必要な`Authorization`ヘッダーに使用され、`Bearer {ACCESS_TOKEN}`の形式で指定する必要があります。

![](../images/api/getting-started/token-generated.png)

#### プログラムによるアクセストークンの生成 {#program}

アプリケーションにLaunch統合を使用している場合、APIリクエストを通じてプログラムによってアクセストークンを生成できます。 これを実現するには、次の値を取得する必要があります。

* クライアント ID (`{API_KEY}`)
* クライアントシークレット(`{SECRET}`)
* JSON Webトークン(`{JWT}`)

クライアントIDと暗号鍵は、前の手順](#one-time-setup)で説明したように、プロジェクトのメインページから取得できます。[

![](../images/api/getting-started/auto-access-creds.png)

JWT秘密鍵証明書を取得するには、左側のナビゲーションで「**サービスアカウント(JWT)**」に移動し、「**JWTを生成**」タブを選択します。 このページの「**カスタムJWTを生成**」で、秘密鍵の内容を指定されたテキストボックスに貼り付けて、「**トークンを生成**」を選択します。

![](../images/api/getting-started/generate-jwt.png)

生成されたJWTは、処理が完了すると以下に表示されます。また、必要に応じてトークンのテストに使用できるサンプルのcURLコマンドも含まれます。 「**コピー**」ボタンを使用して、トークンをクリップボードにコピーします。

![](../images/api/getting-started/jwt-generated.png)

資格情報を収集したら、以下のAPI呼び出しをアプリケーションに統合して、プログラムでアクセストークンを生成できます。

**リクエスト**

リクエストは、次に示す認証資格情報を提供して、`multipart/form-data`ペイロードを送信する必要があります。

```shell
curl -X POST \
  https://ims-na1.adobelogin.com/ims/exchange/jwt/ \
  -H 'Content-Type: multipart/form-data' \
  -F 'client_id={API_KEY}' \
  -F 'client_secret={SECRET}' \
  -F 'jwt_token={JWT}'
```

**応答**

正常な応答は、新しいアクセストークンと、有効期限が切れるまでの残りの秒数を返します。

```json
{
  "token_type": "bearer",
  "access_token": "{ACCESS_TOKEN}",
  "expires_in": 86399999
}
```

| プロパティ | 説明 |
| :-- | :-- |
| `access_token` | 新しく生成されたアクセストークンの値。 この値は、必要な`Authorization`ヘッダーに使用され、`Bearer {ACCESS_TOKEN}`の形式で指定する必要があります。 |
| `expires_in` | トークンの有効期限が切れるまでの残り時間（ミリ秒）。 トークンの有効期限が切れたら、新しいトークンを生成する必要があります。 |

{style=&quot;table-layout:auto&quot;}

## 次の手順

このチュートリアルの手順に従うことで、`{IMS_ORG}`、`{API_KEY}`、および`{ACCESS_TOKEN}`に有効な値を指定できます。 これらの値を、Reactor APIへの簡単なcURLリクエストで使用してテストできるようになりました。

まず、[に対するAPI呼び出しを試みて、すべての会社](./endpoints/companies.md#list)をリストします。

>[!NOTE]
>
>組織に会社が存在しない場合、応答はHTTPステータス404（未検出）になります。 403（禁止）エラーが発生しない限り、アクセス資格情報は有効で機能します。

アクセス資格情報が機能していることを確認したら、引き続き他のAPIリファレンスドキュメントを参照して、APIの多くの機能を確認します。

## その他のリソース

JWTライブラリおよびSDK:[https://jwt.io/](https://jwt.io/)

Postman APIの開発：[https://www.postman.com/](https://www.postman.com/)
