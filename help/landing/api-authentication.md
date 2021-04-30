---
keywords: Experience Platform；ホーム；人気の高いトピック；認証；アクセス
solution: Experience Platform
title: Experience Platform API の認証とアクセス
topic-legacy: tutorial
type: Tutorial
description: このドキュメントでは、Experience Platform API を呼び出すために Adobe Experience Platform 開発者アカウントにアクセスするための順を追ったチュートリアルを提供します。
exl-id: dfe8a7be-1b86-4d78-a27e-87e4ed8b3d42
translation-type: tm+mt
source-git-commit: ef00f50ea2cda2c173208075123b3fe2b2d7ecd4
workflow-type: tm+mt
source-wordcount: '1207'
ht-degree: 15%

---


# Experience Platform API の認証とアクセス

このドキュメントでは、Experience Platform API を呼び出すために Adobe Experience Platform 開発者アカウントにアクセスするための順を追ったチュートリアルを提供します。このチュートリアルの最後に、すべてのプラットフォームAPI呼び出しに必要な次の資格情報が生成されます。

* `{ACCESS_TOKEN}`
* `{API_KEY}`
* `{IMS_ORG}`

アプリケーションとユーザーのセキュリティを維持するには、Adobe I/O API へのすべてのリクエストが、OAuth や JSON Web Tokens（JWT）などの標準を使用して認証され、承認される必要があります。JWTは、クライアント固有の情報と共に使用して個人アクセストークンを生成します。

このチュートリアルでは、プラットフォームAPI呼び出しを認証するために必要な資格情報を収集する方法について説明します。概要は次のフローチャートです。

![](./images/api-authentication/authentication-flowchart.png)

## 前提条件

Experience PlatformAPIの呼び出しを正常に行うには、次の要件が必要です。

* Adobe Experience Platform へのアクセス権を持つ IMS 組織.
* 開発者およびAdmin Consoleプロファイルのユーザーとして追加できる製品管理者。

このチュートリアルを完了するには、Adobe IDも必要です。 Adobe ID をお持ちでない場合は、次の手順で作成できます。

1. [Adobeデベロッパーコンソール](https://console.adobe.io)に移動します。
2. 「**[!UICONTROL 新しいアカウントを作成]**」を選択します。
3. 入会プロセスを完了します。

## Experience Platformの開発者とユーザーアクセスの獲得

Adobe開発者コンソールで統合を作成する前に、Adobe Admin ConsoleのExperience Platform製品プロファイルに対する開発者権限とユーザー権限をアカウントに持っている必要があります。

### 開発者アクセスの獲得

組織の[!DNL Admin Console]管理者に連絡して、[[!DNL Admin Console]](https://adminconsole.adobe.com/)を使用してExperience Platform製品プロファイルに開発者として追加してください。 製品プロファイル](https://helpx.adobe.com/jp/enterprise/admin-guide.html/enterprise/using/manage-developers.ug.html)の開発者アクセスを[管理する方法については、[!DNL Admin Console]のドキュメントを参照してください。

開発者として割り当てられたら、[Adobe開発者コンソール](https://www.adobe.com/go/devs_console_ui)で統合の作成に開始できます。 これらの統合は、外部のアプリケーションやサービスからAdobeAPIへのパイプラインです。

### ユーザーアクセスの取得

また、[!DNL Admin Console]管理者が、同じ製品プロファイルにユーザーとして追加する必要があります。 詳しくは、 [!DNL Admin Console]](https://helpx.adobe.com/enterprise/admin-guide.html/enterprise/using/user-groups.ug.html)の[ユーザーグループの管理に関するガイドを参照してください。

## APIキー、IMS組織ID、クライアントシークレット{#api-ims-secret}の生成

>[!NOTE]
>
>[Privacy Service開発者ガイド](../privacy-service/api/getting-started.md)からこのドキュメントをフォローしている場合は、このガイドに戻って[!DNL Privacy Service]固有のアクセス資格情報を生成できます。

[!DNL Admin Console]経由でプラットフォームへの開発者とユーザーのアクセス権を付与したら、次の手順は、Adobeデベロッパーコンソールで`{IMS_ORG}`および`{API_KEY}`秘密鍵証明書を生成することです。 これらの資格情報は1回だけ生成する必要があり、今後のプラットフォームAPI呼び出しで再利用できます。

### プロジェクト追加へのExperience Platform

[Adobeデベロッパーコンソール](https://www.adobe.com/go/devs_console_ui)に移動し、Adobe IDでサインインします。 次に、Adobe開発者コンソールドキュメントの[空のプロジェクト](https://www.adobe.io/apis/experienceplatform/console/docs.html#!AdobeDocs/adobeio-console/master/projects-empty.md)の作成に関するチュートリアルに説明されている手順に従います。

新しいプロジェクトを作成したら、**[!UICONTROL プロジェクトの概要]**&#x200B;画面で追加&#x200B;**[!UICONTROL API]**&#x200B;を選択します。

![](./images/api-authentication/add-api.png)

**[!UICONTROL 追加API]**&#x200B;画面が表示されます。 Adobe Experience Platformの商品アイコンを選択し、**[!UICONTROL Experience PlatformAPI]**&#x200B;を選択してから、**[!UICONTROL 次へ]**&#x200B;を選択します。

![](./images/api-authentication/platform-api.png)

ここから、[サービスアカウント(JWT)](https://www.adobe.io/apis/experienceplatform/console/docs.html#!AdobeDocs/adobeio-console/master/services-add-api-jwt.md)（「APIを設定」の手順から始まる）を使用してAPIをプロジェクトに追加する方法についてのチュートリアルで説明されている手順に従って、プロセスを完了します。

>[!IMPORTANT]
>
>上記にリンクされたプロセスの特定の手順で、ブラウザーは秘密鍵と関連する公開証明書を自動的にダウンロードします。 この秘密鍵は、このチュートリアルの後の手順で必要となるので、マシン上のどこに保存されるかをメモしておきます。

### 資格情報の収集

APIがプロジェクトに追加されると、プロジェクトの&#x200B;**[!UICONTROL Experience PlatformAPI]**&#x200B;ページに、すべてのExperience PlatformAPIの呼び出しで必要な次の資格情報が表示されます。

* `{API_KEY}` ([!UICONTROL クライアント ID])
* `{IMS_ORG}` ([!UICONTROL Organization ID])

![](././images/api-authentication/api-key-ims-org.png)

上記の資格情報に加えて、今後の手順で生成する&#x200B;**[!UICONTROL クライアントシークレット]**&#x200B;も必要です。 [**[!UICONTROL クライアントシークレットを取得]**]を選択して値を表示し、後で使用できるようにコピーします。

![](././images/api-authentication/client-secret.png)

## JSON Webトークン(JWT) {#jwt}の生成

次の手順は、アカウントの資格情報に基づいてJSON Web Token(JWT)を生成することです。 この値は、プラットフォームAPI呼び出しで使用する`{ACCESS_TOKEN}`秘密鍵証明書の生成に使用されます。この秘密鍵証明書は24時間ごとに再生成する必要があります。

左のナビゲーションで「**[!UICONTROL サービスアカウント(JWT)]**」を選択し、「**[!UICONTROL JWTを生成]**」を選択します。

![](././images/api-authentication/generate-jwt.png)

「**[!UICONTROL カスタムJWT]**&#x200B;を生成」に表示されるテキストボックスに、以前に生成した秘密鍵の内容をサービスアカウントに貼り付けます。 次に、「**[!UICONTROL トークンを生成]**」を選択します。

![](././images/api-authentication/paste-key.png)

ページが更新され、生成されたJWTと、アクセストークンを生成できるサンプルのcURLコマンドが表示されます。 このチュートリアルの目的で、「**[!UICONTROL 生成されたJWT]**」の横の「**[!UICONTROL コピー]**」を選択して、トークンをクリップボードにコピーします。

![](././images/api-authentication/copy-jwt.png)

## アクセストークンの生成

JWTを生成したら、API呼び出しでJWTを使用して`{ACCESS_TOKEN}`を生成できます。 `{API_KEY}`と`{IMS_ORG}`の値とは異なり、プラットフォームAPIを使用し続けるには、新しいトークンを24時間ごとに生成する必要があります。

**リクエスト**

次のリクエストは、ペイロードで提供された秘密鍵証明書に基づいて新しい`{ACCESS_TOKEN}`を生成します。 このエンドポイントは、フォームデータをペイロードとしてのみ受け入れるので、`multipart/form-data`の`Content-Type`ヘッダーを指定する必要があります。

```shell
curl -X POST https://ims-na1.adobelogin.com/ims/exchange/jwt \
  -H 'Content-Type: multipart/form-data' \
  -F 'client_id={API_KEY}' \
  -F 'client_secret={SECRET}' \
  -F 'jwt_token={JWT}'
```

| プロパティ | 説明 |
| --- | --- |
| `{API_KEY}` | [前の手順](#api-ims-secret)で取得した`{API_KEY}`（[!UICONTROL クライアントID]）。 |
| `{SECRET}` | [前の手順](#api-ims-secret)で取得したクライアントシークレット。 |
| `{JWT}` | [前の手順](#jwt)で生成したJWT。 |

>[!NOTE]
>
>同じAPIキー、クライアントシークレットおよびJWTを使用して、セッションごとに新しいアクセストークンを生成できます。 これにより、アプリケーションでのアクセストークン生成を自動化できます。

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
| `token_type` | 返されるトークンのタイプ。 アクセストークンの場合、この値は常に`bearer`です。 |
| `access_token` | 生成された`{ACCESS_TOKEN}`。 この値の先頭に「`Bearer`」という語が付く場合は、すべてのプラットフォームAPI呼び出しの`Authentication`ヘッダーとして必要です。 |
| `expires_in` | アクセストークンの有効期限が切れるまでの残りのミリ秒数です。 この値が0に達したら、引き続きプラットフォームAPIを使用するために新しいアクセストークンを生成する必要があります。 |

## アクセス資格情報のテスト

3つの必要な資格情報をすべて収集したら、以下のAPI呼び出しを行うことができます。 この呼び出しは、貴社で利用可能なすべての標準[!DNL Experience Data Model] (XDM)クラスをリストします。

**リクエスト**

```SHELL
curl -X GET https://platform.adobe.io/data/foundation/schemaregistry/global/classes \
  -H 'Accept: application/vnd.adobe.xed-id+json' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}'
```

**応答** 

応答が以下に示す応答と類似している場合は、資格情報が有効で、機能しています。 （スペース節約のために応答は部分的に表示されています。）

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

## Postmanを使用したAPI呼び出しの認証とテスト

[](https://www.postman.com/) Postmanisは、開発者がRESTful APIを調べたりテストしたりできる一般的なツールです。この[Medium post](https://medium.com/adobetech/using-postman-for-jwt-authentication-on-adobe-i-o-7573428ffe7f)では、Postmanを自動的にJWT認証を実行し、それを使用してプラットフォームAPIを使用するように設定する方法を説明します。

## 次の手順

このドキュメントを読むと、Platform APIのアクセス資格情報を収集し、正常にテストすることができます。 現在は、[ドキュメント](../landing/documentation/overview.md)全体で提供されているAPI呼び出しの例に従うことができます。

このチュートリアルで収集した認証値に加えて、多くのプラットフォームAPIでは、有効な`{SANDBOX_NAME}`をヘッダーとして指定する必要があります。 詳しくは、「[サンドボックスの概要](../sandboxes/home.md)」を参照してください。