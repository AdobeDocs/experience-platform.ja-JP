---
keywords: Experience Platform；ホーム；人気の高いトピック；認証；アクセス
solution: Experience Platform
title: Experience Platform API の認証とアクセス
topic: tutorial
type: Tutorial
description: 'このドキュメントでは、Experience Platform API を呼び出すために Adobe Experience Platform 開発者アカウントにアクセスするための順を追ったチュートリアルを提供します。 '
translation-type: tm+mt
source-git-commit: ca5c8527b1b54856aa1e762a06ddbe404f30ec42
workflow-type: tm+mt
source-wordcount: '908'
ht-degree: 41%

---


# [!DNL Experience Platform] APIの認証とアクセス

このドキュメントでは、[!DNL Experience Platform] APIを呼び出すための、Adobe Experience Platform開発者アカウントへのアクセス権を得るためのチュートリアルを順を追って説明します。

## API を呼び出すための認証

アプリケーションとユーザーのセキュリティを維持するには、Adobe I/O API へのすべてのリクエストが、OAuth や JSON Web Tokens（JWT）などの標準を使用して認証され、承認される必要があります。次に、JWTをクライアント固有の情報と共に使用して個人アクセストークンを生成します。

このチュートリアルでは、次のフローチャートに概要が示されているアクセストークンの作成による認証の手順について説明します。
![](images/api-authentication/authentication-flowchart.png)

## 前提条件

[!DNL Experience Platform] APIの呼び出しを正常に行うには、次の情報が必要です。

* Adobe Experience Platform へのアクセス権を持つ IMS 組織
* 登録済みの Adobe ID アカウント
* ユーザーを製品の&#x200B;**開発者**&#x200B;および&#x200B;**ユーザー**&#x200B;として追加できる Admin Console 管理者です。

次の節では、Adobe ID を作成し、組織の開発者およびユーザーになる手順について説明します。

### Adobe ID の作成

Adobe ID をお持ちでない場合は、次の手順で作成できます。

1. [Adobe開発者コンソール](https://console.adobe.io)に移動します
2. 「**[!UICONTROL 新しいアカウントを作成]**」を選択します。
3. サインアッププロセスの完了

## 組織の[!DNL Experience Platform]の開発者とユーザーになる

Adobe I/O で統合を作成する前に、IMS 組織の製品の開発者権限を持つアカウントが必要です。Admin Console の開発者アカウントに関する詳しい情報については、開発者管理用の[サポートドキュメント](https://helpx.adobe.com/jp/enterprise/using/manage-developers.html)を参照してください。

**開発者アクセスの獲得**

[[!DNL Admin Console]](https://adminconsole.adobe.com/)を使用して組織の製品の1つの開発者として追加するには、組織の[!DNL Admin Console]管理者にお問い合わせください。

![](images/api-authentication/assign-developer.png)

先に進むには、管理者が開発者を 1 つ以上の製品プロファイルに割り当てる必要があります。

![](images/api-authentication/add-developer.png)

開発者として割り当てられると、[Adobe I/O](https://www.adobe.com/go/devs_console_ui) 上で統合を作成するためのアクセス権が与えられます。これらの統合は、外部のアプリケーションやサービスから Adobe API へのパイプラインです。

**ユーザーアクセスの取得**

[!DNL Admin Console]管理者がユーザーとして製品に追加する必要もあります。

![](images/api-authentication/assign-users.png)

開発者を追加するプロセスと同様に、先に進むには、管理者が少なくとも 1 つの製品プロファイルを割り当てる必要があります。

![](images/api-authentication/assign-user-details.png)

## Adobeデベロッパーコンソールでのアクセス資格情報の生成

>[!NOTE]
>
>[Privacy Service開発者ガイド](../privacy-service/api/getting-started.md)からこのドキュメントをフォローしている場合は、このガイドに戻って[!DNL Privacy Service]固有のアクセス資格情報を生成できます。

AdobeDeveloper Consoleを使用して、次の3つのアクセス資格情報を生成する必要があります。

* `{IMS_ORG}`
* `{API_KEY}`
* `{ACCESS_TOKEN}`

`{IMS_ORG}`と`{API_KEY}`は1回だけ生成する必要があり、将来の[!DNL Platform] API呼び出しで再利用できます。 ただし、`{ACCESS_TOKEN}`は一時的で、24時間ごとに再生成する必要があります。

手順の詳細は以下のとおりです。

### 1回限りのセットアップ

[Adobeデベロッパーコンソール](https://www.adobe.com/go/devs_console_ui)に移動し、Adobe IDでサインインします。 次に、Adobe開発者コンソールドキュメントの[空のプロジェクト](https://www.adobe.io/apis/experienceplatform/console/docs.html#!AdobeDocs/adobeio-console/master/projects-empty.md)の作成に関するチュートリアルに説明されている手順に従います。

新しいプロジェクトを作成したら、**プロジェクトの概要**&#x200B;画面で追加&#x200B;**[!UICONTROL API]**&#x200B;を選択します。

![](images/api-authentication/add-api-button.png)

**追加API**&#x200B;画面が表示されます。 Adobe Experience Platformの商品アイコンを選択し、**[!UICONTROL Experience PlatformAPI]**&#x200B;を選択してから、**[!UICONTROL 次へ]**&#x200B;を選択します。

![](images/api-authentication/add-platform-api.png)

プロジェクトに追加するAPIとして[!DNL Experience Platform]を選択したら、[サービスアカウント(JWT)](https://www.adobe.io/apis/experienceplatform/console/docs.html#!AdobeDocs/adobeio-console/master/services-add-api-jwt.md)を使用したプロジェクトへのAPIの追加（「APIを設定」の手順から開始）のチュートリアルに従って、プロセスを終了します。

APIがプロジェクトに追加されると、**プロジェクトの概要**&#x200B;ページに、[!DNL Experience Platform] APIへのすべての呼び出しで必要な次の資格情報が表示されます。

* `{API_KEY}` (クライアント ID)
* `{IMS_ORG}` (Organization ID)

![](./images/api-authentication/api-key-ims-org.png)

### 各セッションの認証

最後に必要な資格情報は`{ACCESS_TOKEN}`です。 `{API_KEY}`および`{IMS_ORG}`の値と異なり、[!DNL Platform] APIを使用し続けるには、24時間ごとに新しいトークンを生成する必要があります。

新しい`{ACCESS_TOKEN}`を生成するには、『Developer Console秘密鍵証明書』ガイドの[JWTトークン](https://www.adobe.io/apis/experienceplatform/console/docs.html#!AdobeDocs/adobeio-console/master/credentials.md)の生成手順に従います。

## アクセス資格情報のテスト

3つの必要な資格情報をすべて収集したら、以下のAPI呼び出しを行うことができます。 この呼び出しは、スキーマレジストリの`global`コンテナ内のすべての[!DNL Experience Data Model] (XDM)クラスをリストします。

**API 形式**

```http
GET /global/classes
```

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

## JWT 認証および API 呼び出しでの Postman の使用

[Postman](https://www.postman.com/) は、RESTful API を使用するための一般的なツールです。この [Medium post](https://medium.com/adobetech/using-postman-for-jwt-authentication-on-adobe-i-o-7573428ffe7f) では 、JWT 認証を自動的に実行し、それを使用して Adobe Experience Platform API を使用するように Postman を設定する方法について説明します。

## 次の手順

このドキュメントを読むと、[!DNL Platform] APIのアクセス資格情報を収集し、テストに成功します。 プラットフォームAPIの[はじめにガイド](api-guide.md)に記載されている例に従うことができるようになりました。 このガイドには、各プラットフォームサービスのAPIガイドへのリンクが含まれ、追加情報が記載されています。 エラー、ポストマン、JSONの場合。

このチュートリアルで収集した認証値に加えて、多くの[!DNL Platform] APIでは、有効な`{SANDBOX_NAME}`をヘッダーとして指定する必要があります。 詳しくは、「[サンドボックスの概要](../sandboxes/home.md)」を参照してください。