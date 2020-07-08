---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Experience PlatformAPIの認証とアクセス
topic: tutorial
translation-type: tm+mt
source-git-commit: bd9884a24c5301121f30090946ab24d9c394db1b
workflow-type: tm+mt
source-wordcount: '877'
ht-degree: 3%

---


# Experience PlatformAPIの認証とアクセス

このドキュメントでは、Experience PlatformAPIを呼び出すためのAdobe Experience Platform開発者アカウントへのアクセス権を取得するためのチュートリアルを順を追って説明します。

## API呼び出しを行うための認証

アプリケーションとユーザーのセキュリティを維持するには、Adobe I/O APIへのすべての要求が、OAuthやJSON Web Tokens(JWT)などの標準を使用して認証され、承認される必要があります。 次に、JWTをクライアント固有の情報と共に使用して個人アクセストークンを生成します。

このチュートリアルでは、次のフローチャートで概要を説明するアクセストークンを作成する際の認証手順について説明します。
![](images/authentication/authentication-flowchart.png)

## 前提条件

Experience PlatformAPIの呼び出しを正常に行うには、以下が必要です。

* Adobe Experience Platformへのアクセス権を持つIMS組織
* 登録されたAdobe IDアカウント
* Admin Console管理者がユーザーを **開発者** および製品 **** のユーザーとして追加します。

次の各節では、Adobe IDを作成し、組織の開発者およびユーザーになるための手順について説明します。

### Adobe IDの作成

Adobe IDがない場合は、次の手順を使用して作成できます。

1. Adobe Developer Console [に移動](https://console.adobe.io)
2. 「新規アカウントを **作成」をクリックします**
3. 入会プロセスの完了

## 組織のExperience Platformの開発者およびユーザーになる

Adobe I/Oで統合を作成する前に、IMS組織の製品に対する開発者権限をアカウントに持っている必要があります。 Admin Consoleの開発者アカウントに関する詳細は、開発者管理の [サポートドキュメント](https://helpx.adobe.com/jp/enterprise/using/manage-developers.html) を参照してください。

**開発者アクセスの取得**

組織のAdmin Console管理者に連絡して、 [Admin Consoleを使用して組織の製品の1つの開発者として追加します](https://adminconsole.adobe.com/)。

![](images/authentication/assign-developer.png)

先に進むには、管理者が開発者として少なくとも1つの製品プロファイルに割り当てる必要があります。

![](images/authentication/add-developer.png)

開発者として割り当てられたら、 [Adobe I/O上で統合を作成するためのアクセス権が与えられます](https://www.adobe.com/go/devs_console_ui)。 これらの統合は、外部のアプリケーションやサービスからAdobe APIへのパイプラインとなります。

**ユーザーアクセスの取得**

また、Admin Console管理者がユーザを製品に追加する必要があります。

![](images/authentication/assign-users.png)

開発者を追加するプロセスと同様に、先に進むには、管理者が少なくとも1つの製品プロファイルを割り当てる必要があります。

![](images/authentication/assign-user-details.png)

## Adobe Developer Consoleでのアクセス資格情報の生成

>[!NOTE]
>
>『 [Privacy Service開発者ガイド](../privacy-service/api/getting-started.md)』からこのドキュメントをフォローしている場合は、このガイドに戻って、Privacy Service固有のアクセス資格情報を生成できます。

Adobe Developer Consoleを使用して、次の3つのアクセス資格情報を生成する必要があります。

* `{IMS_ORG}`
* `{API_KEY}`
* `{ACCESS_TOKEN}`

およ `{IMS_ORG}``{API_KEY}` びは1回だけ生成する必要があり、今後のPlatformAPI呼び出しで再利用できます。 ただし、一時的な `{ACCESS_TOKEN}` ので、24時間ごとに再生成する必要があります。

手順の詳細は以下のとおりです。

### 1回限りのセットアップ

Adobe Developer Consoleに移動し、 [Adobe IDでサインインします](https://www.adobe.com/go/devs_console_ui) 。 次に、Adobe Developer Consoleドキュメントで空のプロジェクトの [作成に関するチュートリアルに概要を説明している手順に従い](https://www.adobe.io/apis/experienceplatform/console/docs.html#!AdobeDocs/adobeio-console/master/projects-empty.md) ます。

新しいプロジェクトを作成したら、プ **[!UICONTROL ロジェクト概要]** 画面の「 _API_ 」をクリックします。

![](images/authentication/add-api-button.png)

API __ 追加画面が表示されます。 Adobe Experience Platformの製品アイコンをクリックし、「 **[!UICONTROL Experience PlatformAPI]** 」を選択してから、「 **[!UICONTROL 次へ]**」をクリックします。

![](images/authentication/add-platform-api.png)

プロジェクトに追加するAPIとしてExperience Platformを選択したら、サービスアカウント(JWT)を使用したプロジェクトへのAPIの [追加のチュートリアルに説明されている手順に従い](https://www.adobe.io/apis/experienceplatform/console/docs.html#!AdobeDocs/adobeio-console/master/services-add-api-jwt.md) （「APIを設定」の手順から開始）、プロセスを完了します。

APIがプロジェクトに追加されると、 _プロジェクトの概要_ ページに、すべてのExperience PlatformAPIの呼び出しで必要な以下の資格情報が表示されます。

* `{API_KEY}` （クライアントID）
* `{IMS_ORG}` (Organization ID)

![](./images/authentication/api-key-ims-org.png)

### 各セッションの認証

最後に必要な秘密鍵証明書は、収集する必要がある秘密鍵証明書 `{ACCESS_TOKEN}`です。 との値とは異なり、新しいトークン `{API_KEY}``{IMS_ORG}`は、PlatformAPIを引き続き使用するために24時間ごとに生成する必要があります。

新しいトークンを生成するに `{ACCESS_TOKEN}`は、『Developer Console credentials』ガイドのJWTトークンを [生成する手順に従います](https://www.adobe.io/apis/experienceplatform/console/docs.html#!AdobeDocs/adobeio-console/master/credentials.md) 。

## アクセス資格情報のテスト

3つの必要な資格情報をすべて収集したら、以下のAPI呼び出しを行うことができます。 この呼び出しは、スキーマレジストリの `global` コンテナ内のすべてのExperience Data Model(XDM)クラスをリストします。

**API形式**

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

応答が以下に示す応答と類似している場合は、資格情報が有効で、機能しています。 （この応答は領域のために切り捨てられました。）

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

## JWT認証およびAPI呼び出しにPostmanを使用

[Postman](https://www.getpostman.com/) は、RESTful APIを操作するための一般的なツールです。 この [Medium post](https://medium.com/adobetech/using-postman-for-jwt-authentication-on-adobe-i-o-7573428ffe7f) （中）では、JWT認証を自動的に実行し、それをAdobe Experience PlatformAPIの使用に使用するようにpostmanを設定する方法について説明します。

## 次の手順

このドキュメントを読むと、PlatformAPIのアクセス資格情報を収集し、正常にテストすることができます。 ドキュメント全体で提供されているAPI呼び出しの例に従うことができ [ます](../landing/documentation/overview.md)。

このチュートリアルで収集した認証値に加えて、多くのPlatformAPIでは、有効な認証値をヘッダーとして指定す `{SANDBOX_NAME}` る必要があります。 See the [sandboxes overview](../sandboxes/home.md) for more information.