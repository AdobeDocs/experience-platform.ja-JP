---
keywords: Experience Platform；ホーム；人気のあるトピック；認証；アクセス
solution: Experience Platform
title: Experience Platform API の認証とアクセス
topic-legacy: tutorial
type: Tutorial
description: このドキュメントでは、Experience Platform API を呼び出すために Adobe Experience Platform 開発者アカウントにアクセスするための順を追ったチュートリアルを提供します。
exl-id: dfe8a7be-1b86-4d78-a27e-87e4ed8b3d42
source-git-commit: ef00f50ea2cda2c173208075123b3fe2b2d7ecd4
workflow-type: tm+mt
source-wordcount: '1207'
ht-degree: 19%

---


# Experience Platform API の認証とアクセス

このドキュメントでは、Experience Platform API を呼び出すために Adobe Experience Platform 開発者アカウントにアクセスするための順を追ったチュートリアルを提供します。このチュートリアルの最後に、すべての Platform API 呼び出しに必要な次の資格情報が生成されます。

* `{ACCESS_TOKEN}`
* `{API_KEY}`
* `{IMS_ORG}`

アプリケーションとユーザーのセキュリティを維持するには、Adobe I/O API へのすべてのリクエストが、OAuth や JSON Web Tokens（JWT）などの標準を使用して認証され、承認される必要があります。JWT は、個人用アクセストークンの生成にクライアント固有の情報と共に使用されます。

このチュートリアルでは、次のフローチャートに概要を示すように、Platform API 呼び出しを認証するために必要な資格情報を収集する方法について説明します。

![](./images/api-authentication/authentication-flowchart.png)

## 前提条件

Experience PlatformAPI を正しく呼び出すには、次が必要です。

* Adobe Experience Platform へのアクセス権を持つ IMS 組織.
* Admin Consoleプロファイルの開発者およびユーザーとしてユーザーを追加できる製品管理者。

このチュートリアルを完了するには、Adobe IDも必要です。 Adobe ID をお持ちでない場合は、次の手順で作成できます。

1. [Adobe開発者コンソール ](https://console.adobe.io) に移動します。
2. 「**[!UICONTROL 新しいアカウントを作成]**」を選択します。
3. サインアッププロセスを完了します。

## Experience Platformの開発者とユーザーアクセスの獲得

Adobe開発者コンソールで統合を作成する前に、Adobe Admin ConsoleのExperience Platform製品プロファイルの開発者とユーザー権限を持つアカウントが必要です。

### 開発者アクセスの獲得

組織の [!DNL Admin Console] 管理者に問い合わせて、[[!DNL Admin Console]](https://adminconsole.adobe.com/) を使用してExperience Platform製品プロファイルに開発者として追加してもらってください。 [ 製品プロファイル ](https://helpx.adobe.com/jp/enterprise/admin-guide.html/enterprise/using/manage-developers.ug.html) の開発者アクセスを管理する方法については、[!DNL Admin Console] のドキュメントを参照してください。

開発者として割り当てられたら、[Adobe開発者コンソール ](https://www.adobe.com/go/devs_console_ui) で統合の作成を開始できます。 これらの統合は、外部のアプリやサービスからAdobeAPI へのパイプラインです。

### ユーザーアクセスの取得

また、[!DNL Admin Console] 管理者は、同じ製品プロファイルにユーザーとして追加する必要があります。 詳しくは、 [!DNL Admin Console]](https://helpx.adobe.com/enterprise/admin-guide.html/enterprise/using/user-groups.ug.html) の [ ユーザーグループの管理に関するガイドを参照してください。

## API キー、IMS Org ID およびクライアントの秘密鍵の生成 {#api-ims-secret}

>[!NOTE]
>
>[Privacy Service開発者ガイド ](../privacy-service/api/getting-started.md) からこのドキュメントに従う場合は、このガイドに戻って、[!DNL Privacy Service] に固有のアクセス資格情報を生成できます。

[!DNL Admin Console] を通じて Platform への開発者とユーザーのアクセス権を付与されたら、次の手順は、Adobe開発者コンソールで `{IMS_ORG}` および `{API_KEY}` 資格情報を生成することです。 これらの資格情報は 1 回だけ生成する必要があり、今後の Platform API 呼び出しで再利用できます。

### プロジェクトにExperience Platformを追加する

[Adobe Developer Console](https://www.adobe.com/go/devs_console_ui) に移動し 、Adobe ID を使用してログインします。次に、Adobe Developer Console のドキュメントの[空のプロジェクトの作成](https://www.adobe.io/apis/experienceplatform/console/docs.html#!AdobeDocs/adobeio-console/master/projects-empty.md)チュートリアルで概説されている手順に従います。

新しいプロジェクトを作成したら、**[!UICONTROL プロジェクトの概要]** 画面で「**[!UICONTROL API]** を追加」を選択します。

![](./images/api-authentication/add-api.png)

**[!UICONTROL API の追加]**&#x200B;画面が表示されます。 Adobe Experience PlatformのExperience Platformアイコンを選択し、**[!UICONTROL 製品 API]** を選択してから、「**[!UICONTROL 次へ]**」を選択します。

![](./images/api-authentication/platform-api.png)

ここから、サービスアカウント (JWT)](https://www.adobe.io/apis/experienceplatform/console/docs.html#!AdobeDocs/adobeio-console/master/services-add-api-jwt.md)（「API を設定」の手順から開始）を使用してプロジェクトに API を追加する方法に関するチュートリアル ([) で説明されている手順に従ってプロセスを完了します。

>[!IMPORTANT]
>
>上記にリンクされたプロセスの特定の手順で、ブラウザーは秘密鍵と関連する公開証明書を自動的にダウンロードします。 この秘密鍵は、このチュートリアルの後半の手順で必要になるので、お使いのコンピューター上のどこに保存されるかに注意してください。

### 資格情報の収集

API がプロジェクトに追加されると、プロジェクトの **[!UICONTROL Experience PlatformAPI]** ページに、Experience PlatformAPI へのすべての呼び出しで必要な次の資格情報が表示されます。

* `{API_KEY}` ([!UICONTROL クライアント ID])
* `{IMS_ORG}` ([!UICONTROL Organization ID])

![](././images/api-authentication/api-key-ims-org.png)

上記の資格情報に加えて、今後の手順で生成された **[!UICONTROL クライアントシークレット]** も必要です。 「 **[!UICONTROL クライアントの秘密鍵を取得]** 」を選択して値を表示し、後で使用するためにコピーします。

![](././images/api-authentication/client-secret.png)

## JSON Web トークン (JWT) の生成 {#jwt}

次の手順では、アカウントの資格情報に基づいて JSON Web トークン (JWT) を生成します。 この値は、Platform API 呼び出しで使用する `{ACCESS_TOKEN}` 秘密鍵証明書を生成するために使用されます。この秘密鍵証明書は 24 時間ごとに再生成する必要があります。

左のナビゲーションで「**[!UICONTROL サービスアカウント (JWT)]**」を選択し、「**[!UICONTROL JWT を生成]**」を選択します。

![](././images/api-authentication/generate-jwt.png)

「**[!UICONTROL カスタム JWT を生成]**」の下に表示されるテキストボックスに、Platform API をサービスアカウントに追加した際に以前に生成した秘密鍵の内容を貼り付けます。 次に、「**[!UICONTROL トークンを生成]**」を選択します。

![](././images/api-authentication/paste-key.png)

ページが更新され、生成された JWT と、アクセストークンを生成できるサンプルの cURL コマンドが表示されます。 このチュートリアルの目的で、「**[!UICONTROL 生成された JWT]**」の横にある「**[!UICONTROL コピー]**」を選択して、トークンをクリップボードにコピーします。

![](././images/api-authentication/copy-jwt.png)

## アクセストークンの生成

JWT を生成したら、API 呼び出しで JWT を使用して `{ACCESS_TOKEN}` を生成できます。 `{API_KEY}` と `{IMS_ORG}` の値とは異なり、Platform API を引き続き使用するには、24 時間ごとに新しいトークンを生成する必要があります。

**リクエスト**

次のリクエストは、ペイロードで指定された資格情報に基づいて新しい `{ACCESS_TOKEN}` を生成します。 このエンドポイントは、フォームデータをペイロードとして受け取るだけなので、 `multipart/form-data` の `Content-Type` ヘッダーを指定する必要があります。

```shell
curl -X POST https://ims-na1.adobelogin.com/ims/exchange/jwt \
  -H 'Content-Type: multipart/form-data' \
  -F 'client_id={API_KEY}' \
  -F 'client_secret={SECRET}' \
  -F 'jwt_token={JWT}'
```

| プロパティ | 説明 |
| --- | --- |
| `{API_KEY}` | [ 前の手順 ](#api-ims-secret) で取得した `{API_KEY}`（[!UICONTROL  クライアント ID]）。 |
| `{SECRET}` | [ 前の手順 ](#api-ims-secret) で取得したクライアントシークレット。 |
| `{JWT}` | [ 前の手順 ](#jwt) で生成した JWT。 |

>[!NOTE]
>
>同じ API キー、クライアントの秘密鍵、JWT を使用して、各セッションに対して新しいアクセストークンを生成できます。 これにより、アプリケーションでのアクセストークンの生成を自動化できます。

**応答** 

```json
{
  "token_type": "bearer",
  "access_token": "{ACCESS_TOKEN}",
  "expires_in": 86399992
}
```

| プロパティ | 説明 |
| --- | --- |
| `token_type` | 返されるトークンのタイプ。 アクセストークンの場合、この値は常に `bearer` です。 |
| `access_token` | 生成された `{ACCESS_TOKEN}`。 この値の先頭に `Bearer` が付く場合、すべての Platform API 呼び出しの `Authentication` ヘッダーとして必要です。 |
| `expires_in` | アクセストークンの有効期限が切れるまでの残り時間（ミリ秒）。 この値が 0 に達したら、引き続き Platform API を使用するために、新しいアクセストークンを生成する必要があります。 |

## アクセス資格情報のテスト

3 つの必須資格情報をすべて収集したら、次の API 呼び出しをおこないます。 この呼び出しは、組織で使用可能なすべての標準 [!DNL Experience Data Model] (XDM) クラスをリストします。

**リクエスト**

```SHELL
curl -X GET https://platform.adobe.io/data/foundation/schemaregistry/global/classes \
  -H 'Accept: application/vnd.adobe.xed-id+json' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}'
```

**応答**

応答が以下に示すような場合は、資格情報が有効で機能しています。 （スペース節約のために応答は部分的に表示されています。）

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

## Postman を使用した API 呼び出しの認証とテスト

[](https://www.postman.com/) Postmanis は、開発者が RESTful API を調べてテストできる一般的なツールです。この [Medium post](https://medium.com/adobetech/using-postman-for-jwt-authentication-on-adobe-i-o-7573428ffe7f) では、JWT 認証を自動的に実行し、それを使用して Platform API を使用するように Postman を設定する方法を説明します。

## 次の手順

このドキュメントでは、Platform API のアクセス資格情報を収集し、テストに成功しました。 [ ドキュメント ](../landing/documentation/overview.md) 全体で提供されている API 呼び出しの例に従うことができるようになりました。

このチュートリアルで収集した認証値に加えて、多くの Platform API では、ヘッダーとして有効な `{SANDBOX_NAME}` を指定する必要もあります。 詳しくは、「[サンドボックスの概要](../sandboxes/home.md)」を参照してください。