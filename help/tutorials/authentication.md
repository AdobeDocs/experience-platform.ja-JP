---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Authenticate and Access Experience Platform API
topic: tutorial
translation-type: tm+mt
source-git-commit: fca15ebf87559b08dd09e63b5df5655b57ef5977

---


# Experience Platform APIの認証とアクセス

このドキュメントでは、Experience Platform APIを呼び出すために、Adobe Experience Platform開発者アカウントにアクセスするための手順のチュートリアルを提供します。

## API呼び出しを行うための認証

アプリケーションとユーザーのセキュリティを維持するには、Adobe I/O APIへのすべての要求が、OAuthやJSON Web Tokens(JWT)などの標準を使用して認証され、承認される必要があります。 次に、JWTをクライアント固有の情報と共に使用して、個人情報を生成します。アクセストークン

このチュートリアルでは、次のフローチャートで概要を説明するアクセストークンを作成することで、認証手順を説明します。
![](images/authentication/authentication-flowchart.png)

## 前提条件

Experience Platform APIの呼び出しを正常に行うには、次が必要です。

* Adobe Experience Platformへのアクセス権を持つIMS組織
* 登録済みのAdobe IDアカウント
* 開発者および製品のユーザーとして追加する **** 、管理コン **ソール管理** 者です。

次の節では、Adobe IDを作成し、組織の開発者およびユーザーになる手順について説明します。

### Adobe IDの作成

Adobe IDをお持ちでない場合は、次の手順で作成できます。

1. Adobe I/O [コンソールに移動](https://console.adobe.io)
2. 「新規アカ **ウントを作成」をクリック**
3. 入会プロセスの完了


### 組織のエクスペリエンスプラットフォームの開発者とユーザーになる

Adobe I/Oで統合を作成する前に、IMS組織の製品の開発者権限を持つアカウントが必要です。 管理コンソールの開発者アカウントに関する詳細な情報は、開発者を管理するサポート [ドキュメント](https://helpx.adobe.com/enterprise/using/manage-developers.html) を参照してください。

**開発者アクセスの獲得**

組織の管理コンソール管理者に連絡して、管理コンソールを使用して組織の製品の1つの開発者として追加し [てください](https://adminconsole.adobe.com/)。

![](images/authentication/assign-developer.png)

先に進むには、管理者が開発者を1つ以上の製品プロファイルに割り当てる必要があります。

![](images/authentication/add-developer.png)

開発者として割り当てられると、 [Adobe I/O上で統合を作成するためのアクセス権が与えられます](https://console.adobe.io/)。 これらの統合は、外部のアプリケーションやサービスからAdobe APIへのパイプラインです。

**ユーザーアクセスの取得**

また、管理コンソール管理者がユーザを製品に追加する必要があります。

![](images/authentication/assign-users.png)

開発者を追加するプロセスと同様に、先に進むには、管理者が少なくとも1つの製品プロファイルを割り当てる必要があります。

![](images/authentication/assign-user-details.png)


## 1回のセットアップ

次の手順は、1回のみ実行する必要があります。

* Adobe I/Oコンソールにログイン
* 統合の作成
* ダウンアクセス値のコピー

統合を取得し、アクセス値を取得したら、将来の認証に再利用できます。 各手順の詳細は以下のとおりです。

### Adobe I/Oコンソールにログインする

Adobe I/O [コンソールに移動し](https://console.adobe.io/) 、Adobe IDを使用してサインインします。

ログインしたら、画面の上部にあ **る「統合** 」タブをクリックします。 統合は、選択したIMS組織用に作成されるサービスアカウントです。 統合が作成されたIMS組織の呼び出しのみを行うことができます。

>[!NOTE]
>アカウントが複数の組織に関連付けられている場合は、画面の右上にあるドロップダウンメニューを使用して、それらを簡単に切り替えることができます。

### 統合の作成

「統合」ペー **ジで** 、「新規統合」をク **リックして** 、プロセスを開始します。 このプロセスには、次の3つの手順が含まれます。
* 統合のタイプの選択
* 統合するAdobeサービスの選択
* 追加統合の詳細、公開鍵、製品プロファイル

![](images/authentication/integrations.png)

#### 統合のタイプの選択

次の画面では、APIにアクセスするか、近いリアルタイムイベントを受け取るか 「 **Access an API** 」を選択し、「 **Continue**」を選択します。

![](images/authentication/create-new-integration.png)

#### 統合するAdobeサービスの選択

アカウントが複数のIMS組織に関連付けられている場合は、右上のドロップダウンメニューを使用して、組織を切り替えることができます。 APIにア **クセスするには** 、Adobe Experience Platformの下の「 **Workshop** and **Experience Platform API** 」を選択します。

![](images/authentication/integration-select-service.png)

「続行 **** 」をクリックして、次のセクションに移動します。

#### 追加統合の詳細、公開鍵、製品プロファイル

次の画面で、統合の詳細を入力し、公開鍵証明書を入力し、製品の選択を求めるプロンプトが表示されます。プロファイル

![](images/authentication/integration-details.png)

まず、統合の詳細を入力します。 次に、製品のオプションをプロファイルします。 製品プロファイルは、前の手順で選択したサービスに属する機能のグループに対して、詳細なアクセス権を付与します。

証明書セクションの場合は、証明書を生成する必要があります。

**MacOSおよびLinuxプラットフォームの場合：**

コマンドラインを開き、次のコマンドを実行します。

`openssl req -x509 -sha256 -nodes -days 365 -newkey rsa:2048 -keyout private.key -out certificate_pub.crt`


**Windowsプラットフォームの場合：**

1. 公開証明書を生成するには、opensslクライアントをダウンロードします( [Openssl Windowsクライアントなど](https://bintray.com/vszakats/generic/download_file?file_path=openssl-1.1.1-win64-mingw.zip))。

1. フォルダーを展開し、C:/libs/の場所にコピーします。

1. コマンドラインプロンプトを開き、次のコマンドを実行します。

   `set OPENSSL_CONF=C:/libs/openssl-1.1.1-win64-mingw/openssl.cnf`

   `cd C:/libs/openssl-1.1.1-win64-mingw/`

   `openssl req -x509 -sha256 -nodes -days 365 -newkey rsa:2048 -keyout private.key -out certificate_pub.crt`

次のような応答が表示され、自分に関する情報の入力を求められます。

```
Generating a 2048 bit RSA private key
.................+++
.......................................+++
writing new private key to 'private.key'
-----
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) []:
State or Province Name (full name) []:
Locality Name (eg, city) []:
Organization Name (eg, company) []:
Organizational Unit Name (eg, section) []:
Common Name (eg, fully qualified host name) []:
Email Address []:
```

情報を入力すると、次の2つのファイルが生成されます。 `certificate_pub.crt` と `private.key`

>[!NOTE]
>`certificate_pub.crt` は365日で有効期限が切れます。 上のコマンドでの値を変更すると、期間を長くできますが、 `days` 秘密鍵証明書を `openssl` 定期的に回転させるのがよいセキュリティプラクティスです。

後のセ `private.key` クションでは、を使用してJWTを生成します。

は、API `certificate_pub.crt` キーの作成に使用されます。 Adobe I/Oコンソールに戻り、「ファイルを選択」をクリ **ックして** 、ファイルをアップロード `certificate_pub.crt` します。

「統合を **作成」をクリックし** 、プロセスを完了します。

### アクセス値のコピー

統合の作成後、詳細を表示できます。 「 **Retrieve Client Secret** 」をクリックすると、画面は次のようになります。

![](images/authentication/access-values.png)

の値（組織ID）をコ `{API KEY}`ピ `{IMS ORG}` ーダウンし、次の手 `{CLIENT SECRET}` 順で使用するために使用します。

## 各セッションの認証

最後の手順は、API呼び出しを認 `{ACCESS_TOKEN}` 証するために使用するユーザーを生成することです。 このアクセストークンは、Adobe Experience Platformに対して行うすべてのAPI呼び出しの認証ヘッダーに含まれている必要があります。 アクセストークンは24時間後に期限が切れ、その後、APIを使用し続けるために新しいトークンを生成する必要があります。

### JWTを作成

Adobe I/Oコンソールの統合の詳細ページで、「 **JWT** 」タブに移動します。

![](images/authentication/generate-jwt.png)

前の節で作成した内容を入力するよ `private.key` うに求めるプロンプトが表示されます。 コマンドラインを開いて、ファイルの表示を指定 `private.key` します。

```shell
cat private.key
```

出力は次のようになります。

```shell
-----BEGIN PRIVATE KEY-----
MIIEvAIBADANBgkqhkiG9w0BAQEFAASCBKYwggSiAgEAAoIBAQCYjPj18NrVlmrc
H+YUTuwWrlHTiPfkBGM0P1HbIOdwrlSTCmPhmaNNG5+mEiULJLWlrhQpx/7uQVNW
......
xbWgBWatJ2hUhU5/K2iFlNJBVXyNy7rN0XzOagLRJ1uS2CM6Hn3vBOqLbHRG4Pen
J1LvEocGunT12UJekLdEaQR4AKodIyjv5opvewrzxUZhVvUIIgeU5vUpg9smCXai
wPW5MQjmygodzCh7+eGLrg==
-----END PRIVATE KEY-----
```

出力全体をコピーしてテキストフィールドに貼り付け、「JWTを生成」をク **リックします**。 生成したJWTを次の手順にコピーします。

![](images/authentication/generated-jwt.png)

### 生成アクセストークン

cURLコマンドを使用してアクセストークンを生成できます。 cURLがインストールされていない場合は、を使用してインストールできま `npm install curl`す。 cURLについて詳しくは、こちらを参照してくだ [さい](https://curl.haxx.se/)

cURLをインストールしたら、次のコマンド内のフィールドを、独自の、およびと置き換える必 `{API_KEY}`要が `{CLIENT_SECRET}`ありま `{JWT_TOKEN}`す。

```SHELL
curl -X POST "https://ims-na1.adobelogin.com/ims/exchange/jwt/" \
  -F "client_id={API_KEY}" \
  -F "client_secret={CLIENT_SECRET}" \
  -F "jwt_token={JWT_TOKEN}"
```

成功した場合、出力は次のようになります。

```JSON
{
  "token_type":"bearer",
  "access_token":"eyJ4NXUiOiJpbXNfbmExLXN0ZzEta2V5LT2VyIiwiYWxnIjoiUlMyNTYifQ.eyJpZCI6IjE1MjAzMDU0ODY5MDhfYzMwM2JkODMtMWE1My00YmRiLThhNjctMWDhhNDJiNTE1X3VlMSIsImNsaWVudF9pZCI6ImYwNjY2Y2M4ZGVhNzQ1MWNiYzQ2ZmI2MTVkMzY1YzU0IiwidXNlcl9pZCI6IjA0ODUzMkMwNUE5ODg2QUQwQTQ5NDEzOUB0ZWNoYWNjdC5hZG9iZS5jb20iLCJzdGF0ZSI6IntcInNlc3Npb25cIjpcImh0dHBzOi8vaW1zLW5hMS1zdGcxLmFkb2JlbG9naW4uY29tL2ltcy9zZXNzaW9uL3YxL05UZzJZemM1TVdFdFlXWTNaUzAwT1RWaUxUZ3lPVFl0WkdWbU5EUTVOelprT0dFeUxTMHdORGcxTXpKRPVGc0TmtGRU1FRTBPVFF4TXpsQWRHVmphR0ZqWTNRdVlXUnZZbVV1WTI5dFwifSIsInR5cGUiOiJhY2Nlc3NfdG9rZW4iLCJhcyI6Imltcy1uYTEtc3RnMSIsImZnIjoiU0hRUlJUQ0ZTWFJJTjdSQjVVQ09NQ0lBWVU9PT09PT0iLCJtb2kiOiJhNTYwOWQ5ZiIsImMiOiJMeksySTBuZ2F2M1BhWWIxV0J3d3FRPT0iLCJleHBpcmVzX2luIjoiODY0MDAwMDAiLCJzY29wZSI6Im9wZW5pZCxzZXNzaW9uLEFkb2JlSUQscmVhZF9vcmdhbml6YXRpb25zLGFkZGl0aW9uYWxfaW5mby5wcm9qZWN0ZWRQcm9kdWN0Q29udGV4dCIsImNyZWF0ZWRfYXQiOiIxNTIwMzA1NDg2OTA4In0.EBgpw0JyKVzbjIBmH6fHDZUvJpvNG8xf8HUHNCK2l-dnVJqXxdi0seOk_kjVodkIa3evC54V560N60vi_mzt7gef-g954VH6l3gFh6XQ7yqRJD2LMW7G1lhQGhga4hrQCnJlfSQoztvIp9hkar9Zcu-MYgyEB5UlwK3KtB3elu7vJGk35F3T9OnqVL4PFj0Ix6zcuN_4gikgQgmtoUjuXULinbtu9Bkmdf7so9FvhapUd5ZTUTTMrAfJ36gEOQPqsuzlu9oUQaYTAn8v4B9TgoS0Paslo6WIksc4f_rSVWsbO6_TSUqIOi0e_RyL6GkMBA1ELA-Dkgbs-jUdkw",
  "expires_in":86399947
}
```

アクセストークンはキーの下の値 `access_token` です。 このアクセストークン `expires_in` は86399947ミリ秒（24時間）です。 その後、上記と同じ手順に従って新しいアクセストークンを生成する必要があります。

これで、Adobe Experience PlatformでAPIリクエストを作成する準備が整いました。

### アクセスコードのテスト

お使いのアクセストークンが有効かどうかをテストするには、次のAPI呼び出しを行います。 この呼び出しは、リスト内のすべてのクラスを `global` コンテナします。

>[!NOTE]
>`{API_KEY}` 上で `{IMS_ORG}` 生成した値を参照します。

**リクエスト**

```SHELL
curl -X GET https://platform.adobe.io/data/foundation/schemaregistry/global/classes \
  -H 'Accept: application/vnd.adobe.xed-id+json' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}'
```


次に示すような応答の場合は、有効で `access_token` 機能します。 （この応答は領域のために切り捨てられました。）

**応答**

```JSON
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
  ]
}
```

## JWT認証およびAPI呼び出しにポストマンを使用

[Postmanは](https://www.getpostman.com/) 、RESTful APIを使用するための一般的なツールです。 この [Medium postでは](https://medium.com/adobetech/using-postman-for-jwt-authentication-on-adobe-i-o-7573428ffe7f) 、JWT認証を自動的に実行し、それを使用してAdobe Experience Platform APIを使用するようにポストマンを設定する方法について説明します。