---
keywords: Experience Platform；ホーム；人気の高いトピック；認証；アクセス
solution: Experience Platform
title: Experience Platform API の認証とアクセス
type: Tutorial
description: このドキュメントでは、Experience Platform API を呼び出すために Adobe Experience Platform 開発者アカウントにアクセスするための順を追ったチュートリアルを提供します。
exl-id: dfe8a7be-1b86-4d78-a27e-87e4ed8b3d42
source-git-commit: fa4786b081b46c8f3c0030282ae3900891fbd652
workflow-type: tm+mt
source-wordcount: '1581'
ht-degree: 14%

---


# Experience Platform API の認証とアクセス

このドキュメントでは、Experience Platform API を呼び出すために Adobe Experience Platform 開発者アカウントにアクセスするための順を追ったチュートリアルを提供します。このチュートリアルの最後に、すべての Platform API 呼び出しに必要な次の資格情報が生成されます。

* `{ACCESS_TOKEN}`
* `{API_KEY}`
* `{ORG_ID}`

アプリケーションとユーザーのセキュリティを維持するには、Adobe I/O API へのすべてのリクエストが、OAuth や JSON Web Tokens（JWT）などの標準を使用して認証され、承認される必要があります。JWT は、個人用アクセストークンを生成するために、クライアント固有の情報と共に使用されます。

このチュートリアルでは、次のフローチャートに示すように、Platform API 呼び出しを認証するために必要な資格情報を収集する方法について説明します。

![](./images/api-authentication/authentication-flowchart.png)

## 前提条件

Experience PlatformAPI を正しく呼び出すには、次が必要です。

* Adobe Experience Platformへのアクセス権を持つ組織。
* Admin Consoleプロファイルの開発者およびユーザーとして追加できる製品管理者。

また、このチュートリアルを完了するには、Adobe IDが必要です。 Adobe ID をお持ちでない場合は、次の手順で作成できます。

1. に移動します。 [Adobe Developer Console](https://console.adobe.io).
2. 選択 **[!UICONTROL 新しいアカウントを作成]**.
3. サインアッププロセスを完了します。

## Experience Platform用の開発者とユーザーアクセスの獲得

Adobe Developer Console で統合を作成する前に、Adobe Admin ConsoleのExperience Platform製品プロファイルに対する開発者権限とユーザー権限を持つアカウントが必要です。

### 開発者アクセスの獲得

連絡先： [!DNL Admin Console] 組織の管理者に問い合わせて、 [[!DNL Admin Console]](https://adminconsole.adobe.com/). 詳しくは、 [!DNL Admin Console] 方法に関する具体的な手順に関するドキュメント [製品プロファイルの開発者アクセスの管理](https://helpx.adobe.com/jp/enterprise/admin-guide.html/enterprise/using/manage-developers.ug.html).

開発者として割り当てられたら、で統合の作成を開始できます。 [Adobe Developer Console](https://www.adobe.com/go/devs_console_ui). これらの統合は、外部のアプリやサービスからAdobeAPI へのパイプラインです。

### ユーザーアクセスの取得

お使いの [!DNL Admin Console] また、管理者はユーザーを同じ製品プロファイルに追加する必要があります。 詳しくは、 [ユーザーグループの管理 [!DNL Admin Console]](https://helpx.adobe.com/enterprise/admin-guide.html/enterprise/using/user-groups.ug.html) を参照してください。

## API キー、組織 ID、クライアントの秘密鍵を生成する {#api-ims-secret}

>[!NOTE]
>
>このドキュメントを [Privacy ServiceAPI ガイド](../privacy-service/api/getting-started.md)に戻り、固有のアクセス資格情報を生成できるようになりました。 [!DNL Privacy Service].

を通じて Platform への開発者およびユーザーアクセス権を付与されたら、 [!DNL Admin Console]次の手順は、 `{ORG_ID}` および `{API_KEY}` Adobe Developer Console の資格情報 これらの資格情報は 1 回だけ生成する必要があり、今後の Platform API 呼び出しで再利用できます。

### プロジェクトにExperience Platformを追加する

[Adobe Developer Console](https://www.adobe.com/go/devs_console_ui) に移動し 、Adobe ID を使用してログインします。次に、Adobe Developer Console のドキュメントの[空のプロジェクトの作成](https://developer.adobe.com/developer-console/docs/guides/projects/projects-empty/)チュートリアルで概説されている手順に従います。

新しいプロジェクトを作成したら、「 **[!UICONTROL API を追加]** の **[!UICONTROL プロジェクトの概要]** 画面

![](./images/api-authentication/add-api.png)

**[!UICONTROL API の追加]**&#x200B;画面が表示されます。 Adobe Experience Platformの製品アイコンを選択し、「 **[!UICONTROL Experience PlatformAPI]** 選択する前に **[!UICONTROL 次へ]**.

![](./images/api-authentication/platform-api.png)

ここから、 [サービスアカウント (JWT) を使用したプロジェクトへの API の追加](https://www.adobe.io/apis/experienceplatform/console/docs.html#!AdobeDocs/adobeio-console/master/services-add-api-jwt.md) （「API の設定」手順から開始）を実行して、プロセスを完了します。

>[!IMPORTANT]
>
>上記にリンクされたプロセス中の特定の手順で、ブラウザーは、秘密鍵と関連する公開証明書を自動的にダウンロードします。 この秘密鍵は、このチュートリアルの後の手順で必要になるので、お使いのコンピューター上のどこに保存されるかをメモしておきます。

###  資格情報の収集

API がプロジェクトに追加されると、 **[!UICONTROL Experience PlatformAPI]** プロジェクトのページには、Experience PlatformAPI へのすべての呼び出しで必要な次の資格情報が表示されます。

* `{API_KEY}` ([!UICONTROL クライアント ID])
* `{ORG_ID}` ([!UICONTROL 組織 ID])

![](././images/api-authentication/api-key-ims-org.png)

上記の資格情報に加えて、生成された **[!UICONTROL クライアント秘密鍵]** を参照してください。 選択 **[!UICONTROL クライアント秘密鍵を取得]** をクリックして値を表示し、後で使用するためにコピーします。

![](././images/api-authentication/client-secret.png)

## JSON Web トークン (JWT) の生成 {#jwt}

次の手順では、アカウントの資格情報に基づいて JSON Web トークン (JWT) を生成します。 この値は、 `{ACCESS_TOKEN}` Platform API 呼び出しで使用する資格情報。24 時間ごとに再生成する必要があります。

>[!IMPORTANT]
>
>このチュートリアルの目的上、以下の手順では、開発者コンソール内で JWT を生成する方法の概要を説明します。 ただし、この生成方法は、テストおよび評価の目的でのみ使用する必要があります。
>
>通常の使用では、JWT を自動的に生成する必要があります。 プログラムによる JWT の生成方法について詳しくは、 [サービスアカウント認証ガイド](https://www.adobe.io/developer-console/docs/guides/authentication/JWT/) Adobe Developer

選択 **[!UICONTROL サービスアカウント (JWT)]** 左側のナビゲーションで、「 **[!UICONTROL JWT を生成]**.

![](././images/api-authentication/generate-jwt.png)

の下に表示されるテキストボックス内 **[!UICONTROL カスタム JWT を生成]**、Platform API をサービスアカウントに追加する際に以前生成した秘密鍵の内容を貼り付けます。 次に、 **[!UICONTROL トークンを生成]**.

![](././images/api-authentication/paste-key.png)

ページが更新され、生成された JWT が表示されます。また、アクセストークンを生成できるサンプルの cURL コマンドも表示されます。 このチュートリアルの目的で、 **[!UICONTROL コピー]** 次の **[!UICONTROL 生成された JWT]** をクリックして、トークンをクリップボードにコピーします。

![](././images/api-authentication/copy-jwt.png)

## アクセストークンの生成

JWT を生成したら、API 呼び出しで使用して、 `{ACCESS_TOKEN}`. の値とは異なる `{API_KEY}` および `{ORG_ID}`に設定する場合、Platform API を使用し続けるには、24 時間ごとに新しいトークンを生成する必要があります。

**リクエスト**

次のリクエストでは、新しい `{ACCESS_TOKEN}` ペイロードで指定された資格情報に基づきます。 このエンドポイントは、フォームデータをペイロードとしてのみ受け入れるので、 `Content-Type` ヘッダー `multipart/form-data`.

```shell
curl -X POST https://ims-na1.adobelogin.com/ims/exchange/jwt \
  -H 'Content-Type: multipart/form-data' \
  -F 'client_id={API_KEY}' \
  -F 'client_secret={SECRET}' \
  -F 'jwt_token={JWT}'
```

| プロパティ | 説明 |
| --- | --- |
| `{API_KEY}` | この `{API_KEY}` ([!UICONTROL クライアント ID]) [前の手順](#api-ims-secret). |
| `{SECRET}` | で取得したクライアント秘密鍵 [前の手順](#api-ims-secret). |
| `{JWT}` | で生成した JWT [前の手順](#jwt). |

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
| `token_type` | 返されるトークンのタイプ。 アクセストークンの場合、この値は常に `bearer`. |
| `access_token` | 生成された `{ACCESS_TOKEN}`. この値の先頭には、「 `Bearer`は、 `Authentication` すべての Platform API 呼び出し用のヘッダー。 |
| `expires_in` | アクセストークンの有効期限が切れるまでの残り時間（ミリ秒）。 この値が 0 に達したら、Platform API を使用し続けるには、新しいアクセストークンを生成する必要があります。 |

## アクセス資格情報のテスト

3 つの必要な資格情報をすべて収集したら、次の API 呼び出しをおこないます。 この呼び出しでは、すべての標準 [!DNL Experience Data Model] (XDM) 組織で使用可能なクラス。

**リクエスト**

```SHELL
curl -X GET https://platform.adobe.io/data/foundation/schemaregistry/global/classes \
  -H 'Accept: application/vnd.adobe.xed-id+json' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}'
```

**応答**

応答が次に示すような場合は、資格情報が有効で機能しています。 （スペース節約のために応答は部分的に表示されています。）

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

## Postmanを使用した API 呼び出しの認証とテスト

[Postman](https://www.postman.com/) は、開発者が RESTful API を調べてテストできる一般的なツールです。 この [投稿（中）](https://medium.com/adobetech/using-postman-for-jwt-authentication-on-adobe-i-o-7573428ffe7f) JWT 認証を自動的に実行し、それを使用して Platform API を使用するようにPostmanを設定する方法について説明します。

## 開発者権限と API アクセス制御Experience Platform

>[!NOTE]
>
>権限で API 資格情報を表示および管理できるのは、システム管理者のみです。

Adobe Developer Console で統合を作成する前に、Adobe Admin ConsoleのExperience Platform製品プロファイルに対する開発者権限とユーザー権限を持つアカウントが必要です。

### 製品プロファイルへの開発者の追加

[[!DNL Admin Console]](https://adminconsole.adobe.com/) に移動し、Adobe ID でログインします。

選択 **[!UICONTROL 製品]**&#x200B;を選択し、「 **[!UICONTROL Adobe Experience Platform]** を製品のリストから削除します。

![Admin Consoleの製品リスト](././images/api-authentication/products.png)

次の **[!UICONTROL 製品プロファイル]** タブ、選択 **[!UICONTROL AEP-Default-All-Users]**. または、検索バーを使用して名前を入力し、製品プロファイルを検索します。

![製品プロファイルを検索](././images/api-authentication/select-product-profile.png)

を選択します。 **[!UICONTROL 開発者]** 「 」タブで、「 **[!UICONTROL 開発者を追加]**.

![「開発者」タブから開発者を追加する](././images/api-authentication/add-developer1.png)

開発者の **[!UICONTROL 電子メールまたはユーザー名]**. 有効な [!UICONTROL 電子メールまたはユーザー名] 開発者の詳細が表示されます。 「**[!UICONTROL 保存]**」を選択します。

![電子メールまたはユーザー名を使用して開発者を追加する](././images/api-authentication/add-developer-email.png)

開発者が正常に追加され、 [!UICONTROL 開発者] タブをクリックします。

![「デベロッパー」タブに表示されるデベロッパー](././images/api-authentication/developer-added.png)

### API の設定

開発者は、Adobe Developer Console で、プロジェクト内に API を追加して設定できます。

プロジェクトを選択し、「 」を選択します。 **[!UICONTROL API を追加]**.

![プロジェクトへの API の追加](././images/api-authentication/add-api-project.png)

内 **[!UICONTROL API を追加]** ダイアログボックスの選択 **[!UICONTROL Adobe Experience Platform]**&#x200B;を選択し、「 **[!UICONTROL Experience PlatformAPI]**.

![API をExperience Platformに追加](././images/api-authentication/add-api-platform.png)

内 **[!UICONTROL API の設定]** 画面、選択 **[!UICONTROL AEP-Default-All-Users]**.

### API をロールに割り当て

システム管理者は、システム UI で API をロールに割り当てることができますExperience Platform。

選択 **[!UICONTROL 権限]** と、API を追加するロールを定義します。 を選択します。 **[!UICONTROL API 資格情報]** 「 」タブで、「 **[!UICONTROL API 資格情報の追加]**.

![選択した役割の「 API 資格情報」タブ](././images/api-authentication/api-credentials.png)

ロールに追加する API を選択し、「 」を選択します。 **[!UICONTROL 保存]**.

![選択可能な API のリスト](././images/api-authentication/select-api.png)

次の場所に戻ります： [!UICONTROL API 資格情報] タブに追加します。新しく追加された API が表示されます。

![新しく追加された API を含む「 API 資格情報」タブ](././images/api-authentication/api-credentials-with-added-api.png)

## 次の手順

このドキュメントでは、Platform API のアクセス資格情報を収集し、正常にテストしました。 これで、 [ドキュメント](../landing/documentation/overview.md).

このチュートリアルで収集した認証値に加えて、多くの Platform API では有効な `{SANDBOX_NAME}` ヘッダーとして提供される。 詳しくは、[サンドボックスの概要](../sandboxes/home.md)を参照してください。