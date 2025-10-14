---
solution: Experience Platform
title: Experience Platform API の認証とアクセス
type: Tutorial
description: このドキュメントでは、Experience Platform API を呼び出すために Adobe Experience Platform 開発者アカウントにアクセスするための順を追ったチュートリアルを提供します。
role: Developer
feature: API
exl-id: dfe8a7be-1b86-4d78-a27e-87e4ed8b3d42
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '2507'
ht-degree: 5%

---


# Experience Platform API の認証とアクセス

このドキュメントでは、Experience Platform API を呼び出すためにAdobe Experience Platform開発者アカウントにアクセスする、順を追ったチュートリアルを提供します。 このチュートリアルの最後では、すべてのExperience Platform API 呼び出しでヘッダーとして必要な次の資格情報を生成または収集します。

* `{ACCESS_TOKEN}`
* `{API_KEY}`
* `{ORG_ID}`

>[!TIP]
>
>上記の 3 つの資格情報に加えて、多くのExperience Platform API では、有効な `{SANDBOX_NAME}` をヘッダーとして指定する必要もあります。 サンドボックスについて詳しくは、[&#x200B; サンドボックスの概要 &#x200B;](../sandboxes/home.md) を参照してください。また、組織で使用可能なサンドボックスの一覧表示について詳しくは、[&#x200B; サンドボックス管理エンドポイント &#x200B;](/help/sandboxes/api/sandboxes.md#list) のドキュメントを参照してください。

アプリケーションとユーザーのセキュリティを維持するには、Experience Platform API へのすべてのリクエストが、OAuth などの標準を使用して認証および承認される必要があります。

このチュートリアルでは、以下のフローチャートに示すように、Experience Platform API 呼び出しの認証に必要な資格情報の収集方法を説明します。 必要な資格情報のほとんどは、1 回限りの初期設定で収集できます。 ただし、アクセストークンは 24 時間ごとに更新する必要があります。

![1 回限りの初期設定と後続の各セッションの認証フローの要件 &#x200B;](./images/api-authentication/authentication-flowchart.png)

## 前提条件 {#prerequisites}

Experience Platform API を正常に呼び出すには、次の条件を満たす必要があります。

* Adobe Experience Platformへのアクセス権を持つ組織。
* Admin Console管理者（製品プロファイルの開発者およびユーザーとして追加できます）。
* Experience Platform システム管理者。API を使用してExperience Platformの様々な部分で読み取りまたは書き込み操作を実行するために必要な、属性ベースのアクセス制御をユーザーに付与できます。

このチュートリアルを完了するには、Adobe IDも必要です。 Adobe ID をお持ちでない場合は、次の手順で作成できます。

1. [Adobe Developer Console](https://console.adobe.io) に移動します。
2. **[!UICONTROL 新しいアカウントを作成]** を選択します。
3. 新規登録プロセスを完了します。

## Experience Platformの開発者およびユーザーアクセスの取得 {#gain-developer-user-access}

Adobe Developer Consoleで統合を作成する前に、アカウントにAdobe Admin ConsoleのExperience Platform製品プロファイルの開発者権限とユーザー権限が必要です。

### 開発者アクセス権の取得 {#gain-developer-access}

開発者としてAdmin Console製品プロファイルに追加する場合は、組織のExperience Platform管理者にお問い合わせください。 [&#x200B; 製品プロファイルの開発者アクセスの管理 &#x200B;](https://helpx.adobe.com/jp/enterprise/admin-guide.html/enterprise/using/manage-developers.ug.html) の方法について詳しくは、Admin Consoleのドキュメントを参照してください。

開発者として割り当てられたら、[Adobe Developer Console](https://www.adobe.com/go/devs_console_ui) で統合の作成を開始できます。 これらの統合は、外部のアプリやサービスからAdobe API へのパイプラインです。

### ユーザーアクセスの取得 {#gain-user-access}

また、Admin Console管理者が、あなたを同じ製品プロファイルのユーザーとして追加する必要もあります。 ユーザーアクセスを使用すると、実行する API 操作の結果を UI で確認できます。

詳しくは、[Admin Consoleでのユーザーグループの管理 &#x200B;](https://helpx.adobe.com/jp/enterprise/admin-guide.html/enterprise/using/user-groups.ug.html) のガイドを参照してください。

## API キー（クライアント ID）と組織 ID の生成 {#generate-credentials}

>[!NOTE]
>
>[Privacy Service API ガイド &#x200B;](../privacy-service/api/getting-started.md) のこのドキュメントを参照する場合は、ガイドに戻って、[!DNL Privacy Service] に固有のアクセス認証情報を生成することができます。

Admin Consoleを通じて開発者およびユーザーにExperience Platformへのアクセス権を付与したら、次の手順は、Adobe Developer Consoleで `{ORG_ID}` と `{API_KEY}` の資格情報を生成することです。 これらの資格情報は 1 回だけ生成する必要があり、今後のExperience Platform API 呼び出しで再利用できます。

>[!TIP]
>
>Developer Consoleに移動する代わりに、Experience Platform API を使用するために必要なすべての認証資格情報を、API リファレンスドキュメントページから直接取得できます。 機能について [&#x200B; 詳細を参照 &#x200B;](#get-credentials-functionality)。

### プロジェクトへのExperience Platformの追加 {#add-platform-to-project}

[Adobe Developer Console](https://www.adobe.com/go/devs_console_ui) に移動し 、Adobe ID を使用してログインします。次に、Adobe Developer Console のドキュメントの[空のプロジェクトの作成](https://developer.adobe.com/developer-console/docs/guides/projects/projects-empty/)チュートリアルで概説されている手順に従います。

新しいプロジェクトを作成したら、**[!UICONTROL プロジェクトの概要]** 画面で **[!UICONTROL API を追加]** を選択します。

>[!TIP]
>
>複数の組織用にプロビジョニングされている場合は、インターフェイスの右上隅にある組織セレクターを使用して、必要な組織に属していることを確認します。

![&#x200B; 「API を追加」オプションがハイライト表示されたDeveloper Console画面 &#x200B;](./images/api-authentication/add-api.png)

**[!UICONTROL API の追加]**&#x200B;画面が表示されます。 **[!UICONTROL Adobe Experience Platform]** の商品アイコンを選択し、「**[!UICONTROL Experience Platform API]**」を選択してから **[!UICONTROL 次へ]** を選択します。

![API を追加画面でExperience Platform API を選択します。](./images/api-authentication/platform-api.png)

>[!TIP]
>
>**[!UICONTROL ドキュメントを表示]** オプションを選択して、別のブラウザーウィンドウに移動し、[Experience Platform API リファレンスドキュメント &#x200B;](https://developer.adobe.com/experience-platform-apis/) を参照します。

### [!UICONTROL OAuth サーバー間 &#x200B;] 認証タイプを選択します {#select-oauth-server-to-server}

次に、「**[!UICONTROL OAuth サーバー間]**」認証タイプを選択してアクセストークンを生成し、Experience Platform API にアクセスします。 「**[!UICONTROL 次へ]**」を選択する前に、「**[!UICONTROL 資格情報名]**」テキストフィールドに資格情報に意味のある名前を付けます。

>[!IMPORTANT]
>
>今後サポートされるトークン生成方法は、**[!UICONTROL OAuth サーバー間]** メソッドのみです。 以前サポートされていた **[!UICONTROL サービスアカウント（JWT）]** メソッドは非推奨となり、新しい統合用に選択することはできません。 JWT 認証方法を使用した既存の統合は 2025 年 6 月 30 日（PT）まで引き続き機能しますが、Adobeでは、その日までに既存の統合を新しい [!UICONTROL OAuth サーバー間認証方法 &#x200B;] に移行することを強くお勧めします。 詳しくは、「[!BADGE &#x200B; 非推奨 &#x200B;]{type=negative}[JSON web トークン（JWT）の生成 &#x200B;](#jwt) の節を参照してください。

![Experience Platform API の OAuth サーバー間認証方法を選択します。](./images/api-authentication/oauth-authentication-method.png)

### 統合用の製品プロファイルの選択 {#select-product-profiles}

**[!UICONTROL API を設定]** 画面で、「**[!UICONTROL AEP-Default-All-Users]**」を選択し、アクセス権を取得するその他の製品プロファイルも選択します。

>[!IMPORTANT]
>
>Experience Platformの特定の機能にアクセスするには、必要な属性ベースのアクセス制御権限を付与するシステム管理者も必要です。 詳しくは、[&#x200B; 必要な属性ベースのアクセス制御権限の取得 &#x200B;](#get-abac-permissions) の節を参照してください。

![&#x200B; 統合用の製品プロファイルを選択します &#x200B;](./images/api-authentication/select-product-profiles.png)。

準備ができたら、「**[!UICONTROL 設定済み API を保存]**」を選択します。

Experience Platform API との統合を設定するための上記の手順は、次のビデオチュートリアルでも参照できます。

>[!VIDEO](https://video.tv.adobe.com/v/31656/?learn=on&captions=jpn)

### 資格情報の収集 {#gather-credentials}

API がプロジェクトに追加されると、プロジェクトの **[!UICONTROL OAuth サーバー間]** ページに、Experience Platform API へのすべての呼び出しで必要な次の資格情報が表示されます。

![Developer Console で API を追加した後の統合情報 &#x200B;](./images/api-authentication/api-integration-information.png)

* `{API_KEY}` （[!UICONTROL &#x200B; クライアント ID]）
* `{ORG_ID}` （[!UICONTROL &#x200B; 組織 ID]）

<!--

![](././images/api-authentication/api-key-ims-org.png)

<!--

In addition to the above credentials, you also need the generated **[!UICONTROL Client Secret]** for a future step. Select **[!UICONTROL Retrieve client secret]** to reveal the value, and then copy it for later use.

![](././images/api-authentication/client-secret.png)

-->

## アクセストークンの生成 {#generate-access-token}

次の手順では、Experience Platform API 呼び出しで使用する `{ACCESS_TOKEN}` 資格情報を生成します。 `{API_KEY}` と `{ORG_ID}` の値とは異なり、Experience Platform API を引き続き使用するには、新しいトークンを 24 時間ごとに生成する必要があります。 「**[!UICONTROL アクセストークンを生成]**」を選択します（下図を参照）。

![&#x200B; アクセストークンの生成方法を表示 &#x200B;](././images/api-authentication/generate-access-token.png)

>[!TIP]
>
>また、Postman環境とコレクションを使用してアクセストークンを生成することもできます。 詳しくは、[Postmanを使用した API 呼び出しの認証とテスト &#x200B;](#use-postman) に関する節を参照してください。

## 認証資格情報の作成と取得については、API リファレンスドキュメントを参照してください。 {#get-credentials-functionality}

2024 年 11 月リリースのExperience Platform以降、[!UICONTROL Developer Console] にアクセスしなくても、API リファレンスページからExperience Platform API を直接使用するための資格情報を取得できます。 [&#x200B; フローサービス API – 宛先ページ &#x200B;](https://developer.adobe.com/experience-platform-apis/references/destinations/) から、以下の例を表示します。

![API リファレンスページの上部でハイライト表示された資格情報の取得機能。](././images/api-authentication/get-credentials-highlighted.png)

Experience Platform API を呼び出すための資格情報を取得するには、Experience Platform API リファレンスページに移動し、ページ上部の「**[!UICONTROL ログイン]**」を選択します。 **[!UICONTROL 個人用アカウント]** または **[!UICONTROL 会社または学校アカウント]** を使用してサインインします。

ログイン後、「**[!UICONTROL 新しい認証情報を作成]**」を選択して、Experience Platform API にアクセスするための新しい認証情報のセットを作成します。

![Experience Platform API にアクセスするための新しい資格情報を作成します &#x200B;](././images/api-authentication/create-credentials.gif)。

次に、ドロップダウンセレクターを使用して、資格情報ウィンドウを開き、アクセストークンを生成して、API キーと組織 ID を取得します。 Experience Platform API の使用を開始するには、API リファレンスページの [**[!UICONTROL &#x200B; 試す &#x200B;]**](/help/release-notes/2024/may-2024.md#interactive-api-documentation) ブロックに資格情報をコピーします。

![&#x200B; ドロップダウンセレクターを使用して資格情報を表示し、アクセストークンを生成します。](././images/api-authentication/view-copy-credentials.gif)

>[!TIP]
>
>Experience Platform API リファレンスドキュメントの様々なエンドポイントページ間を移動しても、ページ上部の資格情報ブロックは表示されたままになります。

## [!BADGE &#x200B; 非推奨 &#x200B;]{type=negative}JSON Web トークン（JWT）を生成 {#jwt}

>[!WARNING]
>
>アクセストークンを生成する JWT メソッドは非推奨（廃止予定）になりました。 すべての新しい統合は、[OAuth サーバー間認証方法 &#x200B;](#select-oauth-server-to-server) を使用して作成する必要があります。 また、Adobeでは、統合を引き続き機能させるには、2025 年 6 月 30 日（PT）までに既存の統合を OAuth 方式に移行する必要があります。 以下の重要なドキュメントをお読みください。
> 
> * [JWT から OAuth へのアプリケーションの移行ガイド &#x200B;](https://developer.adobe.com/developer-console/docs/guides/authentication/ServerToServerAuthentication/migration/)
> * [OAuth を使用した新旧のアプリケーションの実装ガイド &#x200B;](https://developer.adobe.com/developer-console/docs/guides/authentication/ServerToServerAuthentication/implementation/)
> * [OAuth サーバー間資格情報方式を使用する場合の利点 &#x200B;](https://developer.adobe.com/developer-console/docs/guides/authentication/ServerToServerAuthentication/migration/#why-oauth-server-to-server-credentials)

+++ 非推奨（廃止予定）の情報の表示

次の手順では、アカウントの資格情報に基づいて JSON web トークン（JWT）を生成します。 この値は、Experience Platform API 呼び出しで使用する `{ACCESS_TOKEN}` 資格情報を生成するために使用されます。この資格情報は、24 時間ごとに再生成する必要があります。

>[!IMPORTANT]
>
>このチュートリアルを目的として、次の手順ではDeveloper Console内で JWT を生成する方法の概要を説明します。 ただし、この生成方法は、テストおよび評価の目的でのみ使用してください。
>
>通常の使用では、JWT を自動的に生成する必要があります。 プログラムによる JWT の生成方法について詳しくは、Adobe Developerの [&#x200B; サービスアカウント認証ガイド &#x200B;](https://www.adobe.io/developer-console/docs/guides/authentication/JWT/) を参照してください。

左側のナビゲーションで「**[!UICONTROL サービスアカウント （JWT）]**」を選択し、「**[!UICONTROL JWT を生成]**」を選択します。

![](././images/api-authentication/generate-jwt.png)

「**[!UICONTROL カスタム JWT を生成]**」の下にあるテキストボックスに、Experience Platform API をサービスアカウントに追加する際に以前に生成した秘密鍵の内容を貼り付けます。 次に、「**[!UICONTROL トークンを生成]**」を選択します。

![](././images/api-authentication/paste-key.png)

ページが更新され、生成された JWT と、アクセストークンを生成できるサンプル cURL コマンドが表示されます。 このチュートリアルでは、「**[!UICONTROL 生成された JWT]**」の横にある「**[!UICONTROL コピー]** を選択して、トークンをクリップボードにコピーします。

![](././images/api-authentication/copy-jwt.png)

**アクセストークンの生成**

JWT を生成したら、API 呼び出しで JWT を使用して `{ACCESS_TOKEN}` を生成できます。 `{API_KEY}` と `{ORG_ID}` の値とは異なり、Experience Platform API を引き続き使用するには、新しいトークンを 24 時間ごとに生成する必要があります。

**リクエスト**

次のリクエストは、ペイロードで指定された資格情報に基づいて新しい `{ACCESS_TOKEN}` を生成します。 このエンドポイントは、フォームデータのみをペイロードとして受け入れるので、`Content-Type` の `multipart/form-data` ヘッダーを指定する必要があります。

```shell
curl -X POST https://ims-na1.adobelogin.com/ims/exchange/jwt \
  -H 'Content-Type: multipart/form-data' \
  -F 'client_id={API_KEY}' \
  -F 'client_secret={SECRET}' \
  -F 'jwt_token={JWT}'
```

| プロパティ | 説明 |
| --- | --- |
| `{API_KEY}` | `{API_KEY}` 前の手順 [!UICONTROL &#x200B; で取得した &#x200B;] ール（[&#x200B; クライアント ID](#api-ims-secret)。 |
| `{SECRET}` | [&#x200B; 前の手順 &#x200B;](#api-ims-secret) で取得したクライアント秘密鍵。 |
| `{JWT}` | [&#x200B; 前の手順 &#x200B;](#jwt) で生成した JWT。 |

>[!NOTE]
>
>同じ API キー、クライアントの秘密鍵、JWT を使用して、セッションごとに新しいアクセストークンを生成できます。 これにより、アプリケーションでのアクセストークンの生成を自動化できます。

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
| `access_token` | 生成された `{ACCESS_TOKEN}`。 単語 `Bearer` のプレフィックスが付いたこの値は、すべてのExperience Platform API 呼び出しの `Authentication` ヘッダーとして必要です。 |
| `expires_in` | アクセストークンの有効期限が切れるまでの残り時間（ミリ秒）。 この値が 0 に達した場合、Experience Platform API の使用を継続するには、新しいアクセストークンを生成する必要があります。 |

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
>上記の呼び出しはアクセス資格情報をテストするには十分ですが、属性ベースの適切なアクセス制御権限がないと、複数のリソースにアクセスして変更することができません。 詳しくは、以下の **必要な属性ベースのアクセス制御権限の取得** 節を参照してください。

## 必要な属性ベースのアクセス制御権限の取得 {#get-abac-permissions}

Experience Platform内の複数のリソースにアクセスしたり、変更したりするには、適切なアクセス制御権限が必要です。 システム管理者から [&#x200B; 必要な権限 &#x200B;](/help/access-control/ui/permissions.md) を付与できます。 詳しくは、「役割の API 資格情報の管理 [&#x200B; に関する節を参照し &#x200B;](/help/access-control/abac/ui/permissions.md#manage-api-credentials-for-role) ください。

システム管理者が API を使用してExperience Platform リソースへのアクセスに必要な権限を付与する方法について詳しくは、以下のビデオチュートリアルも参照してください。

>[!VIDEO](https://video.tv.adobe.com/v/31656/?learn=on&t=159&captions=jpn)

## Postmanを使用した API 呼び出しの認証とテスト {#use-postman}

[Postman](https://www.postman.com/) は、開発者が RESTful API を参照およびテストできる一般的なツールです。 Experience Platform Postmanのコレクションと環境を使用すると、Experience Platform API の操作を高速化できます。 詳しくは、[Experience PlatformでのPostmanの使用 &#x200B;](/help/landing/postman.md) およびコレクションと環境の基本を学ぶを参照してください。

Experience Platform コレクションおよび環境でのPostmanの使用に関する詳細については、以下のビデオチュートリアルも参照してください。

**Experience Platform API で使用するPostman環境のダウンロードと読み込み**

>[!VIDEO](https://video.tv.adobe.com/v/31656/?learn=on&t=106&captions=jpn)

**Postman コレクションを使用してアクセストークンを生成する**

[Identity Management サービス Postman コレクションをダウンロードし &#x200B;](https://github.com/adobe/experience-platform-postman-samples/tree/master/apis/ims) 以下のビデオを視聴してアクセストークンの生成方法を確認してください。

>[!VIDEO](https://video.tv.adobe.com/v/34080/?learn=on&captions=jpn)

**Experience Platform API Postman コレクションをダウンロードし、API とやり取りする**

>[!VIDEO](https://video.tv.adobe.com/v/34079/?learn=on&captions=jpn)

<!--
This [Medium post](https://medium.com/adobetech/using-postman-for-jwt-authentication-on-adobe-i-o-7573428ffe7f) describes how you can set up Postman to automatically perform JWT authentication and use it to consume Experience Platform APIs.
-->

## システム管理者：Experience Platform権限を使用して開発者および API アクセス制御を付与します {#grant-developer-and-api-access-control}

Adobe Developer Consoleで統合を作成する前に、アカウントにExperience Platform製品プロファイルの開発者権限とユーザー権限が必要です。

>[!NOTE]
>
>システム管理者のみが、権限の API 資格情報を表示および管理できます。

### 製品プロファイルへの開発者の追加 {#add-developers-to-product-profile}

[Admin Consoleに移動し &#x200B;](https://adminconsole.adobe.com/)Adobe IDでログインします。

ナビゲーションバーの「**[!UICONTROL 製品]**」を選択して、製品のリストから「**[!UICONTROL Adobe Experience Platform]**」を選択します。

![Adobe Experience Platform製品がハイライト表示されたAdobe Admin Consoleの製品ページ。](././images/api-authentication/products.png)

「**[!UICONTROL 製品プロファイル]**」タブから、「**[!UICONTROL AEP-Default-All-Users]**」を選択します。 または、検索バーを使用して名前を入力して製品プロファイルを検索します。

![&#x200B; 検索バーとAEP-Default-All-Users 製品がハイライト表示された製品プロファイルページ。](././images/api-authentication/select-product-profile.png)

「**[!UICONTROL 開発者]**」タブを選択し、「**[!UICONTROL 開発者を追加]**」を選択します。

![&#x200B; 「開発者」タブが表示され、「開発者を追加」オプションがハイライト表示されます。](././images/api-authentication/add-developer1.png)

**[!UICONTROL 開発者を追加]** ダイアログが表示されます。 開発者の **[!UICONTROL メールまたはユーザー名]** を入力します。 有効な [!UICONTROL &#x200B; メールまたはユーザー名 &#x200B;] に、開発者の詳細が表示されます。 「**[!UICONTROL 保存]**」を選択します。

![&#x200B; 開発者情報が入力され、「保存」オプションがハイライト表示された開発者を追加ダイアログ &#x200B;](././images/api-authentication/add-developer-email.png)

開発者が正常に追加され、「**[!UICONTROL 開発者]**」タブに表示されます。

![&#x200B; 新しく追加された開発者がハイライト表示された、追加されたすべての開発者のリストを表示する「開発者」タブ。](././images/api-authentication/developer-added.png)

### API 資格情報の役割への割り当て

>[!NOTE]
>
> Experience Platform UI で API をロールに割り当てることができるのは、システム管理者のみです。

Experience Platform API を使用して操作を実行するには、システム管理者は、役割の特定の権限セットに加えて、API 資格情報を追加する必要があります。 詳しくは、「役割の API 資格情報の管理 [&#x200B; に関する節を参照し &#x200B;](../access-control/abac/ui/permissions.md#manage-api-credentials-for-a-role) ください。

製品プロファイルに開発者を追加し、API を役割に割り当てるための、上記の手順のチュートリアルを、次のビデオチュートリアルでも利用できます。

>[!VIDEO](https://video.tv.adobe.com/v/3446399/?learn=on&captions=jpn)

## その他のリソース {#additional-resources}

Experience Platform API の概要の詳細については、以下にリンクされている追加のリソースを参照してください

* [Experience Platform API の認証とアクセス &#x200B;](https://experienceleague.adobe.com/docs/platform-learn/tutorials/platform-api-authentication.html?lang=ja) ビデオチュートリアルページ
* アクセストークンを生成するための [Identity Management サービス Postman コレクション &#x200B;](https://github.com/adobe/experience-platform-postman-samples/tree/master/apis/ims)
* [Experience Platform API Postman コレクション &#x200B;](https://github.com/adobe/experience-platform-postman-samples/tree/master/apis/experience-platform)

## 次の手順 {#next-steps}

このドキュメントでは、Experience Platform API のアクセス資格情報を収集し、正常にテストしました。 [&#x200B; ドキュメント &#x200B;](../landing/documentation/overview.md) 全体を通して提供される API 呼び出しの例に従うことができるようになりました。

このチュートリアルで収集した認証値に加えて、多くのExperience Platform API では、有効な `{SANDBOX_NAME}` をヘッダーとして指定する必要もあります。 詳しくは、[サンドボックスの概要](../sandboxes/home.md)を参照してください。
