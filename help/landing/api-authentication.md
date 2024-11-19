---
solution: Experience Platform
title: Experience Platform API の認証とアクセス
type: Tutorial
description: このドキュメントでは、Experience Platform API を呼び出すために Adobe Experience Platform 開発者アカウントにアクセスするための順を追ったチュートリアルを提供します。
role: Developer
feature: API
exl-id: dfe8a7be-1b86-4d78-a27e-87e4ed8b3d42
source-git-commit: 48c75f88d6862fa602e43929b72a6eac27d20e07
workflow-type: tm+mt
source-wordcount: '2383'
ht-degree: 5%

---


# Experience Platform API の認証とアクセス

このドキュメントでは、Experience PlatformAPI を呼び出すためにAdobe Experience Platform開発者アカウントにアクセスする、順を追ったチュートリアルを提供します。 このチュートリアルの最後では、すべての Platform API 呼び出しのヘッダーとして必要な、次の資格情報を生成または収集します。

* `{ACCESS_TOKEN}`
* `{API_KEY}`
* `{ORG_ID}`

>[!TIP]
>
>上記の 3 つの資格情報に加えて、多くの Platform API では、有効な `{SANDBOX_NAME}` をヘッダーとして指定する必要もあります。 サンドボックスについて詳しくは、[ サンドボックスの概要 ](../sandboxes/home.md) を参照してください。また、組織で使用可能なサンドボックスの一覧表示について詳しくは、[ サンドボックス管理エンドポイント ](/help/sandboxes/api/sandboxes.md#list) のドキュメントを参照してください。

アプリケーションとユーザーのセキュリティを維持するには、Experience PlatformAPI へのすべてのリクエストが、OAuth などの標準を使用して認証および承認される必要があります。

このチュートリアルでは、以下のフローチャートに示すように、Platform API 呼び出しの認証に必要な資格情報の収集方法を説明します。 必要な資格情報のほとんどは、1 回限りの初期設定で収集できます。 ただし、アクセストークンは 24 時間ごとに更新する必要があります。

![](./images/api-authentication/authentication-flowchart.png)

## 前提条件 {#prerequisites}

Experience PlatformAPI を正常に呼び出すには、次の条件を満たす必要があります。

* Adobe Experience Platformへのアクセス権を持つ組織。
* Admin Consoleおよび製品プロファイルのユーザーとして追加できる開発管理者。
* Experience Platformシステム管理者。API を使用してExperience Platformの様々な部分で読み取りまたは書き込み操作を実行するために必要な、属性ベースのアクセス制御をユーザーに付与できます。

このチュートリアルを完了するには、Adobe IDも必要です。 Adobe ID をお持ちでない場合は、次の手順で作成できます。

1. [Adobe Developer Console](https://console.adobe.io) に移動します。
2. **[!UICONTROL 新しいアカウントを作成]** を選択します。
3. 新規登録プロセスを完了します。

## Experience Platformのための開発者およびユーザーのアクセス権の取得 {#gain-developer-user-access}

Adobe Developer Consoleで統合を作成する前に、アカウントがAdobe Admin ConsoleのExperience Platform製品プロファイルの開発者権限とユーザー権限を持っている必要があります。

### 開発者アクセス権の取得 {#gain-developer-access}

Experience Platform [[!DNL Admin Console]](https://adminconsole.adobe.com/) を使用して開発者向け製品プロファイルに自分を追加する場合は、組織内の [!DNL Admin Console] 管理者にお問い合わせください。 [ 製品プロファイルの開発者アクセスの管理 ](https://helpx.adobe.com/jp/enterprise/admin-guide.html/enterprise/using/manage-developers.ug.html) の方法について詳しくは、[!DNL Admin Console] のドキュメントを参照してください。

開発者として割り当てられたら、[Adobe Developer Console](https://www.adobe.com/go/devs_console_ui) で統合の作成を開始できます。 これらの統合は、外部のアプリやサービスからAdobeAPI へのパイプラインです。

### ユーザーアクセスの取得 {#gain-user-access}

また、[!DNL Admin Console] 管理者も、同じ製品プロファイルにユーザーとして追加する必要があります。 ユーザーアクセスを使用すると、実行する API 操作の結果を UI で確認できます。

詳しくは、[ でのユーザーグループの管理  [!DNL Admin Console]](https://helpx.adobe.com/enterprise/admin-guide.html/enterprise/using/user-groups.ug.html) のガイドを参照してください。

## API キー（クライアント ID）と組織 ID の生成 {#generate-credentials}

>[!NOTE]
>
>[Privacy ServiceAPI ガイド ](../privacy-service/api/getting-started.md) のこのドキュメントを読む場合は、ガイドに戻って、[!DNL Privacy Service] に固有のアクセス認証情報を生成することができます。

[!DNL Admin Console] を通じて開発者およびユーザーに Platform へのアクセス権を付与したら、次の手順は、Adobe Developer Consoleで `{ORG_ID}` と `{API_KEY}` の資格情報を生成することです。 これらの資格情報は 1 回だけ生成する必要があり、今後の Platform API 呼び出しで再利用できます。

>[!TIP]
>
>Platform API を操作するために必要なすべての認証資格情報は、Developer Consoleに移動する代わりに、API リファレンスドキュメントページから直接取得できます。 機能について ](#get-credentials-functionality) 詳細を参照 [。

### プロジェクトにExperience Platformを追加する {#add-platform-to-project}

[Adobe Developer Console](https://www.adobe.com/go/devs_console_ui) に移動し 、Adobe ID を使用してログインします。次に、Adobe Developer Console のドキュメントの[空のプロジェクトの作成](https://developer.adobe.com/developer-console/docs/guides/projects/projects-empty/)チュートリアルで概説されている手順に従います。

新しいプロジェクトを作成したら、**[!UICONTROL プロジェクトの概要]** 画面で **[!UICONTROL API を追加]** を選択します。

>[!TIP]
>
>複数の組織用にプロビジョニングされている場合は、インターフェイスの右上隅にある組織セレクターを使用して、必要な組織に属していることを確認します。

![ 「API を追加」オプションがハイライト表示されたDeveloper Console画面 ](./images/api-authentication/add-api.png)

**[!UICONTROL API の追加]**&#x200B;画面が表示されます。 Adobe Experience Platformの製品アイコンを選択し、「**[!UICONTROL Experience PlatformAPI]**」を選択してから「**[!UICONTROL 次へ]**」を選択します。

![Experience PlatformAPI を選択します ](./images/api-authentication/platform-api.png)。

>[!TIP]
>
>**[!UICONTROL ドキュメントを表示]** オプションを選択して、別のブラウザーウィンドウに移動し、[Experience PlatformAPI リファレンスドキュメント ](https://developer.adobe.com/experience-platform-apis/) を参照します。

### [!UICONTROL OAuth サーバー間 ] 認証タイプを選択します {#select-oauth-server-to-server}

次に、「[!UICONTROL OAuth サーバー間 ] 認証タイプ」を選択してアクセストークンを生成し、Experience PlatformAPI にアクセスします。

>[!IMPORTANT]
>
>今後サポートされるトークン生成方法は、**[!UICONTROL OAuth サーバー間]** メソッドのみです。 以前サポートされていた **[!UICONTROL サービスアカウント（JWT）]** メソッドは非推奨となり、新しい統合用に選択することはできません。 JWT 認証方法を使用した既存の統合は 2025 年 1 月 1 日（PT）まで引き続き機能しますが、Adobeでは、その日までに既存の統合を新しい [!UICONTROL OAuth サーバー間認証方法 ] に移行することを強くお勧めします。 詳しくは、「[!BADGE  非推奨 ]」の節を参照してください。{type=negative}[JSON web トークン（JWT）を生成 ](#jwt) ます。

![ 認証 API の OAuth サーバー間Experience Platform方式を選択します。](./images/api-authentication/oauth-authentication-method.png)

### 統合用の製品プロファイルの選択 {#select-product-profiles}

**[!UICONTROL API の設定]** 画面で、「**[!UICONTROL AEP-Default-All-Users]**」を選択します。

<!--
Your integration's service account will gain access to granular features through the product profiles selected here.

-->

>[!IMPORTANT]
>
Platform の特定の機能にアクセスするには、必要な属性ベースのアクセス制御権限を付与するシステム管理者も必要です。 詳しくは、[ 必要な属性ベースのアクセス制御権限の取得 ](#get-abac-permissions) の節を参照してください。

![ 統合用の製品プロファイルを選択します ](./images/api-authentication/select-product-profiles.png)。

準備ができたら、「**[!UICONTROL 設定済み API を保存]**」を選択します。

Experience PlatformAPI との統合を設定するための上記の手順は、次のビデオチュートリアルでも参照できます。

>[!VIDEO](https://video.tv.adobe.com/v/28832/?learn=on)

### 資格情報の収集 {#gather-credentials}

API がプロジェクトに追加されると、プロジェクトの **[!UICONTROL Experience PlatformAPI]** ページに、Experience PlatformAPI へのすべての呼び出しで必要な次の資格情報が表示されます。

![Developer Console で API を追加した後の統合情報 ](./images/api-authentication/api-integration-information.png)

* `{API_KEY}` （[!UICONTROL  クライアント ID]）
* `{ORG_ID}` （[!UICONTROL  組織 ID]）

<!--

![](././images/api-authentication/api-key-ims-org.png)

<!--

In addition to the above credentials, you also need the generated **[!UICONTROL Client Secret]** for a future step. Select **[!UICONTROL Retrieve client secret]** to reveal the value, and then copy it for later use.

![](././images/api-authentication/client-secret.png)

-->

## アクセストークンの生成 {#generate-access-token}

次の手順では、Platform API 呼び出しで使用する `{ACCESS_TOKEN}` 資格情報を生成します。 `{API_KEY}` と `{ORG_ID}` の値とは異なり、Platform API を引き続き使用するには、新しいトークンを 24 時間ごとに生成する必要があります。 「**[!UICONTROL アクセストークンを生成]**」を選択します（下図を参照）。

![ アクセストークンの生成方法を表示 ](././images/api-authentication/generate-access-token.gif)

>[!TIP]
>
また、Postman環境とコレクションを使用してアクセストークンを生成することもできます。 詳しくは、[Postmanを使用した API 呼び出しの認証とテスト ](#use-postman) に関する節を参照してください。

## 認証資格情報の作成と取得については、API リファレンスドキュメントを参照してください。 {#get-credentials-functionality}

Experience Platformの 2024 年 11 月リリース以降、[!UICONTROL Developer Console] にアクセスしなくても、API リファレンスページからExperience PlatformAPI を直接使用するための資格情報を取得できます。 [ フローサービス API – 宛先ページ ](https://developer.adobe.com/experience-platform-apis/references/destinations/) から、以下の例を表示します。

![API リファレンスページの上部でハイライト表示された資格情報の取得機能。](././images/api-authentication/get-credentials-highlighted.png)

Platform API を呼び出すための資格情報を取得するには、任意のExperience PlatformAPI リファレンスページに移動し、ページ上部の「**[!UICONTROL ログイン]**」を選択します。 **[!UICONTROL 個人用アカウント]** または **[!UICONTROL 会社または学校アカウント]** を使用してサインインします。

ログイン後、「**[!UICONTROL 新しい認証情報を作成]**」を選択して、Platform API にアクセスするための新しい認証情報のセットを作成します。

![Platform API にアクセスするための新しい資格情報の作成 ](././images/api-authentication/create-credentials.gif)

次に、ドロップダウンセレクターを使用して、資格情報ウィンドウを開き、アクセストークンを生成して、API キーと組織 ID を取得します。 Platform API の使用を開始するには、API リファレンスページの [**[!UICONTROL  試す ]**](/help/release-notes/2024/may-2024.md#interactive-api-documentation) ブロックに資格情報をコピーします。

![ ドロップダウンセレクターを使用して資格情報を表示し、アクセストークンを生成します。](././images/api-authentication/view-copy-credentials.gif)

>[!TIP]
>
Experience PlatformAPI リファレンスドキュメントの様々なエンドポイントページ間を移動しても、ページ上部の資格情報ブロックは表示されたままになります。

## [!BADGE  非推奨 ]{type=negative} JSON web トークン（JWT）を生成 {#jwt}

>[!WARNING]
>
アクセストークンを生成する JWT メソッドは非推奨（廃止予定）になりました。 すべての新しい統合は、[OAuth サーバー間認証方法 ](#select-oauth-server-to-server) を使用して作成する必要があります。 また、Adobeを引き続き機能させるには、2025 年 1 月 1 日（PT）までに既存の統合環境を OAuth 方式に移行する必要があります。 以下の重要なドキュメントをお読みください。
> 
* [JWT から OAuth へのアプリケーションの移行ガイド ](https://developer.adobe.com/developer-console/docs/guides/authentication/ServerToServerAuthentication/migration/)
* [OAuth を使用した新旧のアプリケーションの実装ガイド ](https://developer.adobe.com/developer-console/docs/guides/authentication/ServerToServerAuthentication/implementation/)
* [OAuth サーバー間資格情報方式を使用する場合の利点 ](https://developer.adobe.com/developer-console/docs/guides/authentication/ServerToServerAuthentication/migration/#why-oauth-server-to-server-credentials)

+++ 非推奨（廃止予定）の情報の表示

次の手順では、アカウントの資格情報に基づいて JSON web トークン（JWT）を生成します。 この値は、Platform API 呼び出しで使用する `{ACCESS_TOKEN}` 認証情報を生成するために使用され、24 時間ごとに再生成する必要があります。

>[!IMPORTANT]
>
このチュートリアルを目的として、次の手順ではDeveloper Console内で JWT を生成する方法の概要を説明します。 ただし、この生成方法は、テストおよび評価の目的でのみ使用してください。
>
通常の使用では、JWT を自動的に生成する必要があります。 プログラムによる JWT の生成方法について詳しくは、Adobe Developerの [ サービスアカウント認証ガイド ](https://www.adobe.io/developer-console/docs/guides/authentication/JWT/) を参照してください。

左側のナビゲーションで「**[!UICONTROL サービスアカウント （JWT）]**」を選択し、「**[!UICONTROL JWT を生成]**」を選択します。

![](././images/api-authentication/generate-jwt.png)

「**[!UICONTROL カスタム JWT を生成]**」の下にあるテキストボックスに、Platform API をサービスアカウントに追加する際に以前に生成した秘密鍵の内容を貼り付けます。 次に、「**[!UICONTROL トークンを生成]**」を選択します。

![](././images/api-authentication/paste-key.png)

ページが更新され、生成された JWT と、アクセストークンを生成できるサンプル cURL コマンドが表示されます。 このチュートリアルでは、「**[!UICONTROL 生成された JWT]**」の横にある「**[!UICONTROL コピー]** を選択して、トークンをクリップボードにコピーします。

![](././images/api-authentication/copy-jwt.png)

**アクセストークンの生成**

JWT を生成したら、API 呼び出しで JWT を使用して `{ACCESS_TOKEN}` を生成できます。 `{API_KEY}` と `{ORG_ID}` の値とは異なり、Platform API を引き続き使用するには、新しいトークンを 24 時間ごとに生成する必要があります。

**リクエスト**

次のリクエストは、ペイロードで指定された資格情報に基づいて新しい `{ACCESS_TOKEN}` を生成します。 このエンドポイントは、フォームデータのみをペイロードとして受け入れるので、`multipart/form-data` の `Content-Type` ヘッダーを指定する必要があります。

```shell
curl -X POST https://ims-na1.adobelogin.com/ims/exchange/jwt \
  -H 'Content-Type: multipart/form-data' \
  -F 'client_id={API_KEY}' \
  -F 'client_secret={SECRET}' \
  -F 'jwt_token={JWT}'
```

| プロパティ | 説明 |
| --- | --- |
| `{API_KEY}` | [ 前の手順 ] で取得した `{API_KEY}` ール（[!UICONTROL  クライアント ID](#api-ims-secret)。 |
| `{SECRET}` | [ 前の手順 ](#api-ims-secret) で取得したクライアント秘密鍵。 |
| `{JWT}` | [ 前の手順 ](#jwt) で生成した JWT。 |

>[!NOTE]
>
同じ API キー、クライアントの秘密鍵、JWT を使用して、セッションごとに新しいアクセストークンを生成できます。 これにより、アプリケーションでのアクセストークンの生成を自動化できます。

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
| `token_type` | タイプ of 返されるトークン。 アクセストークンの場合、この値は常に `bearer` です。 |
| `access_token` | 生成された `{ACCESS_TOKEN}`。 この値（先頭に `Bearer` という単語が付く）は、すべての Platform API 呼び出しの `Authentication` ヘッダーとして必要です。 |
| `expires_in` | アクセストークンの有効期限が切れるまでの残り時間（ミリ秒）。 この値が 0 に達したら、Platform API の使用を続行するために、新しいアクセストークンを生成する必要があります。 |

+++

## アクセス資格情報のテスト {#test-credentials}

アクセストークン、API キー、組織 ID の 3 つの必要な資格情報をすべて収集したら、次の API 呼び出しを行うことができます。 この呼び出しでは、組織で使用可能なすべての標準 [!DNL Experience Data Model] （XDM）クラスを一覧表示します。 [Postman](#use-postman) に呼び出しを読み込んで実行します。

>[!BEGINSHADEBOX]

**リクエスト**

```SHELL
curl -X GET https://platform.adobe.io/data/foundation/schemaregistry/global/classes \
  -H 'Accept: application/vnd.adobe.xed-id+json' \
  -H 'Authorization: Bearer {{ACCESS_TOKEN}}' \
  -H 'x-api-key: {{API_KEY}}' \
  -H 'x-gw-ims-org-id: {{ORG_ID}}'
```

**応答**

応答が以下に示すものに類似している場合、資格情報は有効であり、機能します。 （スペース節約のために応答は部分的に表示されています。）

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

>[!ENDSHADEBOX]

>[!IMPORTANT]
>
上記の呼び出しはアクセス資格情報をテストするには十分ですが、属性ベースの適切なアクセス制御権限がないと、複数のリソースにアクセスして変更することができません。 詳しくは、以下の **必要な属性ベースのアクセス制御権限の取得** 節を参照してください。

## 必要な属性ベースのアクセス制御権限の取得 {#get-abac-permissions}

Experience Platform内の複数のリソースにアクセスしたり、変更したりするには、適切なアクセス制御権限が必要です。 システム管理者から [ 必要な権限 ](/help/access-control/ui/permissions.md) を付与できます。 詳しくは、「役割の API 資格情報の管理 [ に関する節を参照し ](/help/access-control/abac/ui/permissions.md#manage-api-credentials-for-role) ください。

システム管理者が API を使用して Platform リソースへのアクセスに必要な権限を付与する方法について詳しくは、以下のビデオチュートリアルも参照してください。

>[!VIDEO](https://video.tv.adobe.com/v/28832/?learn=on&t=159)

## Postmanを使用した API 呼び出しの認証とテスト {#use-postman}

[Postman](https://www.postman.com/) は、開発者が RESTful API を参照およびテストできる一般的なツールです。 Experience PlatformのPostman コレクションおよび環境を使用して、Experience PlatformAPI の作業を高速化できます。 詳しくは、[ 環境でのPostmanの使用 ](/help/landing/postman.md) およびコレクションとExperience Platformの基本を学ぶを参照してください。

Experience Platformコレクションおよび環境でのPostmanの使用について詳しくは、以下のビデオチュートリアルも参照してください。

**Experience PlatformAPI で使用するPostman環境のダウンロードと読み込み**

>[!VIDEO](https://video.tv.adobe.com/v/28832/?learn=on&t=106)

**Postman コレクションを使用してアクセストークンを生成する**

[Identity Management サービス Postman コレクションをダウンロードし ](https://github.com/adobe/experience-platform-postman-samples/tree/master/apis/ims) 以下のビデオを視聴してアクセストークンの生成方法を確認してください。

>[!VIDEO](https://video.tv.adobe.com/v/29698/?learn=on)

**Experience PlatformAPI Postman コレクションをダウンロードして API とやり取りする**

>[!VIDEO](https://video.tv.adobe.com/v/29704/?learn=on)

<!--
This [Medium post](https://medium.com/adobetech/using-postman-for-jwt-authentication-on-adobe-i-o-7573428ffe7f) describes how you can set up Postman to automatically perform JWT authentication and use it to consume Platform APIs.
-->

## システム管理者：Experience Platform権限を使用して開発者と API のアクセス制御を付与します {#grant-developer-and-api-access-control}

>[!NOTE]
>
システム管理者のみが、権限の API 資格情報を表示および管理できます。

Adobe Developer Consoleで統合を作成する前に、アカウントがAdobe Admin ConsoleのExperience Platform製品プロファイルの開発者権限とユーザー権限を持っている必要があります。

### 製品プロファイルへの開発者の追加 {#add-developers-to-product-profile}

[[!DNL Admin Console]](https://adminconsole.adobe.com/) に移動し、Adobe IDでログインします。

**[!UICONTROL 商品]** を選択して、商品のリストから ]**2}Adobe Experience Platform} を選択します。**[!UICONTROL 

![Admin Consoleに掲載されている製品リスト ](././images/api-authentication/products.png)

「**[!UICONTROL 製品プロファイル]**」タブで、「**[!UICONTROL AEP-Default-All-Users]**」を選択します。 または、検索バーを使用して名前を入力して製品プロファイルを検索します。

![ 製品プロファイルの検索 ](././images/api-authentication/select-product-profile.png)

「**[!UICONTROL 開発者]**」タブを選択し、「**[!UICONTROL 開発者を追加]**」を選択します。

![ 「開発者」タブから開発者を追加 ](././images/api-authentication/add-developer1.png)

開発者の **[!UICONTROL メールまたはユーザー名]** を入力します。 有効な [!UICONTROL  メールまたはユーザー名 ] には、開発者の詳細が表示されます。 「**[!UICONTROL 保存]**」を選択します。

![ メールまたはユーザー名を使用して開発者を追加する ](././images/api-authentication/add-developer-email.png)

開発者が正常に追加され、「[!UICONTROL  開発者 ]」タブに表示されます。

![ 「開発者」タブに開発者が表示されます ](././images/api-authentication/developer-added.png)

<!--

Commenting out this part since it duplicates information from the section Add Experience Platform to a project

### Set up an API

A developer can add and configure an API within a project in the Adobe Developer Console.

Select your project, then select **[!UICONTROL Add API]**.

![Add API to a project](././images/api-authentication/add-api-project.png)

In the **[!UICONTROL Add an API]** dialog box select **[!UICONTROL Adobe Experience Platform]**, then select **[!UICONTROL Experience Platform API]**.

![Add an API in Experience Platform](././images/api-authentication/add-api-platform.png)

In the **[!UICONTROL Configure API]** screen, select **[!UICONTROL AEP-Default-All-Users]**.

-->

### API を役割に割り当てる

システム管理者は、Experience PlatformUI で API をロールに割り当てることができます。

**[!UICONTROL 権限]** と、API を追加する役割を選択します。 「**[!UICONTROL API 資格情報]**」タブを選択し、「**[!UICONTROL API 資格情報を追加]**」を選択します。

![ 選択した役割の「API 資格情報」タブ ](././images/api-authentication/api-credentials.png)

役割に追加する API を選択して、「**[!UICONTROL 保存]**」を選択します。

![ 選択可能な API のリスト ](././images/api-authentication/select-api.png)

「[!UICONTROL API 資格情報 ]」タブに戻り、新しく追加された API が一覧表示されます。

![ 新しく追加された API が表示された「API 資格情報」タブ ](././images/api-authentication/api-credentials-with-added-api.png)

## その他のリソース {#additional-resources}

Experience PlatformAPI の概要の詳細については、以下にリンクされているその他のリソースを参照してください

* [Experience PlatformAPI の認証とアクセス ](https://experienceleague.adobe.com/docs/platform-learn/tutorials/platform-api-authentication.html?lang=ja) ビデオチュートリアルページ
* アクセストークンを生成するための [Identity Management サービス Postman コレクション ](https://github.com/adobe/experience-platform-postman-samples/tree/master/apis/ims)
* [Experience PlatformAPI Postman コレクション ](https://github.com/adobe/experience-platform-postman-samples/tree/master/apis/experience-platform)

## 次の手順 {#next-steps}

このドキュメントでは、Platform API のアクセス資格情報を収集し、正常にテストしました。 [ ドキュメント ](../landing/documentation/overview.md) 全体を通して提供される API 呼び出しの例に従うことができるようになりました。

このチュートリアルで収集した認証値に加えて、多くの Platform API では、有効な `{SANDBOX_NAME}` をヘッダーとして指定する必要もあります。 詳しくは、[サンドボックスの概要](../sandboxes/home.md)を参照してください。