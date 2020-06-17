---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Privacy Service開発ガイド
description: RESTful APIを使用して、Adobe Experience Cloudアプリケーション全体でデータサブジェクトの個人データを管理します
topic: developer guide
translation-type: tm+mt
source-git-commit: 6f93191defad6a79a3f6623da3492ab405787b5c
workflow-type: tm+mt
source-wordcount: '793'
ht-degree: 2%

---


# Privacy Service開発ガイド

Adobe Experience Platform Privacy Serviceは、Adobe Experience Cloudアプリケーション全体でデータサブジェクト（お客様）の個人データを管理（アクセスおよび削除）できるRESTful APIおよびユーザーインターフェイスを提供します。 Privacy Serviceは、Experience Cloudアプリケーションに関連するジョブのステータスと結果にアクセスできる、中央の監査とログのメカニズムも提供します。

このガイドはPrivacy ServiceAPIの使用方法をカバーしています。 UIの使用方法について詳しくは、 [Privacy ServiceUIの概要を参照してください](../ui/overview.md)。 Privacy ServiceAPIで使用可能なすべてのエンドポイントの包括的なリストについては、 [APIリファレンスを参照してください](https://www.adobe.io/apis/experiencecloud/gdpr/api-reference.html)。

## はじめに

このガイドでは、次のExperience Platform機能を理解している必要があります。

* [Privacy Service](../home.md): RESTful APIとユーザーインターフェイスを提供します。このインターフェイスを使用すると、Adobe Experience Cloudアプリケーション全体で、データサブジェクト（顧客）からのアクセス要求と削除要求を管理できます。

以下の節では、Privacy ServiceAPIを正しく呼び出すために知る必要がある追加情報について説明します。

### サンプルAPI呼び出しの読み取り

このチュートリアルでは、リクエストをフォーマットする方法を示すAPI呼び出しの例を提供します。 例えば、パス、必須のヘッダー、適切にフォーマットされた要求ペイロードなどです。 API応答で返されるサンプルJSONも提供されます。 サンプルAPI呼び出しのドキュメントで使用される規則について詳しくは、Experience PlatformトラブルシューティングガイドのAPI呼び出し例 [の読み方に関する節](../../landing/troubleshooting.md) を参照してください。

## 必要なヘッダーの値の収集

Privacy ServiceAPIを呼び出すには、まずアクセス資格情報を収集し、必要なヘッダーで使用する必要があります。

* 認証： 無記名 `{ACCESS_TOKEN}`
* x-api-key: `{API_KEY}`
* x-gw-ims-org-id: `{IMS_ORG}`

これには、アドビAdmin ConsoleでExperience Platformの開発者権限を取得し、Adobe Developer Consoleで資格情報を生成する必要があります。

### Experience Platformへの開発者のアクセス権を取得

開発者がPlatformにアクセスするには、 [Experience Platform認証チュートリアルの最初の手順に従い](../../tutorials/authentication.md)ます。 「Generate access credentials in Adobe Developer Console」の手順に到達したら、このチュートリアルに戻って、Privacy Serviceに固有の秘密鍵証明書を生成します。

### アクセス資格情報の生成

Adobe Developer Consoleを使用して、次の3つのアクセス資格情報を生成する必要があります。

* `{IMS_ORG}`
* `{API_KEY}`
* `{ACCESS_TOKEN}`

およ `{IMS_ORG}``{API_KEY}` びは1回だけ生成する必要があり、今後のAPI呼び出しで再利用できます。 ただし、一時的な `{ACCESS_TOKEN}` ので、24時間ごとに再生成する必要があります。

これらの値の生成手順の詳細は以下のとおりです。

#### 1回限りのセットアップ

Adobe Developer Consoleに移動し、 [Adobe IDでサインインします](https://www.adobe.com/go/devs_console_ui) 。 次に、Adobe Developer Consoleドキュメントで空のプロジェクトの [作成に関するチュートリアルに概要を説明している手順に従い](https://www.adobe.io/apis/experienceplatform/console/docs.html#!AdobeDocs/adobeio-console/master/projects-empty.md) ます。

新しいプロジェクトを作成したら、プ **[!UICONTROL ロジェクト概要]** 画面の「 _[!UICONTROL API]_」をクリックします。

![](../images/api/getting-started/add-api-button.png)

API __追加画面が表示されます。 「**[!UICONTROL &#x200B;次へ&#x200B;]**」をクリックする前に、利用可能なAPIのリストから**[!UICONTROL  Privacy ServiceAPIを選択します&#x200B;]**。

![](../images/api/getting-started/add-privacy-service-api.png)

API _[!UICONTROL を設定]_画面が表示されます。 「キーペアを**[!UICONTROL &#x200B;生成する」オプションを選択し&#x200B;]**、右下隅の「キーペアを**[!UICONTROL &#x200B;生成&#x200B;]**」をクリックします。

![](../images/api/getting-started/generate-key-pair.png)

キーペアが自動的に生成され、秘密鍵と公開証明書を含むZIPファイルがローカルマシンにダウンロードされます（後の手順で使用します）。 「設定済みAPI **[!UICONTROL を保存]** 」を選択して設定を完了します。

![](../images/api/getting-started/key-pair-generated.png)

APIがプロジェクトに追加されると、 _Privacy ServiceAPIの概要_ ページにプロジェクトページが再び表示されます。 ここから、「 _[!UICONTROL サービスアカウント(JWT)]_」セクションまで下にスクロールします。このセクションには、Privacy ServiceAPIのすべての呼び出しで必要な次のアクセス資格情報が含まれます。

* **[!UICONTROL クライアントID]**: クライアントIDは、x-api-keyヘッダー `{API_KEY}` で指定する必要があるクライアントIDです。
* **[!UICONTROL 組織ID]**: 組織IDは、x-gw-ims-org-idヘッダーで使用する必要がある `{IMS_ORG}` 値です。

![](../images/api/getting-started/jwt-credentials.png)

#### 各セッションの認証

収集する必要がある最後の必要な秘密鍵証明書 `{ACCESS_TOKEN}`は、「認証」ヘッダーで使用される、自分の秘密鍵証明書です。 との値とは異なり、新しいトークン `{API_KEY}``{IMS_ORG}`は、PlatformAPIを引き続き使用するために24時間ごとに生成する必要があります。

新しい秘密鍵を生成するに `{ACCESS_TOKEN}`は、「トークンの生成」をクリックする前に、以前にダウンロードした秘密鍵を開き、「 _[!UICONTROL アクセストークンの]_生成**[!UICONTROL 」の横のテキストボックスにその内容を貼り付けます&#x200B;]**。

![](../images/api/getting-started/paste-private-key.png)

新しいアクセストークンが生成され、トークンをクリップボードにコピーするためのボタンが提供されます。 この値は、必要な認証ヘッダーに使用され、形式で指定する必要があり `Bearer {ACCESS_TOKEN}`ます。

![](../images/api/getting-started/generated-access-token.png)

## 次の手順

これで、使用するヘッダーが分かったので、Privacy ServiceAPIの呼び出しを開始する準備が整いました。 プ [ライバシージョブに関するドキュメントでは](privacy-jobs.md) 、Privacy ServiceAPIを使用して実行できる様々なAPI呼び出しについて詳しく説明します。 各サンプル呼び出しには、一般的なAPI形式、必要なヘッダーを表示するサンプルリクエスト、サンプルレスポンスが含まれます。
