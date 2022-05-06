---
title: Privacy ServiceAPI の概要
description: Privacy ServiceAPI への認証方法とサンプル API 呼び出しの解釈方法については、ドキュメントを参照してください。
topic-legacy: developer guide
exl-id: c1d05e30-ef8f-4adf-87e0-1d6e3e9e9f9e
source-git-commit: 47a94b00e141b24203b01dc93834aee13aa6113c
workflow-type: tm+mt
source-wordcount: '669'
ht-degree: 32%

---

# Privacy ServiceAPI の概要

このガイドでは、Privacy ServiceAPI を呼び出す前に知っておく必要があるコア概念の概要を示します。

## 前提条件

このガイドは、次の の機能を実際に利用および理解しているユーザーを対象としています。

* [Adobe Experience Platform Privacy Service](../home.md):Adobe Experience Cloudアプリケーション全体でデータ主体（顧客）からのアクセスリクエストと削除リクエストを管理できる RESTful API およびユーザーインターフェイスが用意されています。

## 必須ヘッダーの値の収集

Privacy ServiceAPI を呼び出すには、まず、必要なヘッダーで使用するアクセス資格情報を収集する必要があります。

* `Authorization: Bearer {ACCESS_TOKEN}`
* `x-api-key: {API_KEY}`
* `x-gw-ims-org-id: {ORG_ID}`

これには、Adobe Admin ConsoleでAdobe Experience Platformの開発者権限を取得し、Adobe Developerコンソールで資格情報を生成する必要があります。

###  Experience Platform への開発者のアクセス

に対する開発者アクセス権を取得するには [!DNL Platform]を使用する場合は、 [Experience Platform認証のチュートリアル](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja). 「Adobe Developerコンソールでアクセス資格情報を生成する」手順に達したら、このチュートリアルに戻って、Privacy Serviceに固有の資格情報を生成します。

### アクセス認証情報の生成

Adobe Developer Console を使用して、次の 3 つのアクセス認証情報を生成する必要があります。

* `{ORG_ID}`
* `{API_KEY}`
* `{ACCESS_TOKEN}`

お使いの `{ORG_ID}` および `{API_KEY}` は 1 回だけ生成する必要があり、今後の API 呼び出しで再利用できます。 ただし、 `{ACCESS_TOKEN}` は一時的で、24 時間ごとに再生成する必要があります。

これらの値を生成する手順については、以下で詳しく説明します。

#### 1 回限りのセットアップ

[Adobe Developer Console](https://www.adobe.com/go/devs_console_ui) に移動し 、Adobe ID を使用してログインします。次に、Adobe Developer Console のドキュメントの[空のプロジェクトの作成](https://www.adobe.io/apis/experienceplatform/console/docs.html#!AdobeDocs/adobeio-console/master/projects-empty.md)チュートリアルで概説されている手順に従います。

新しいプロジェクトを作成したら、「 **[!UICONTROL API を追加]** の **[!UICONTROL プロジェクトの概要]** 画面

![](../images/api/getting-started/add-api-button.png)

**[!UICONTROL API の追加]**&#x200B;画面が表示されます。 選択 **[!UICONTROL Privacy ServiceAPI]** を選択する前に、使用可能な API のリストから **[!UICONTROL 次へ]**.

![](../images/api/getting-started/add-privacy-service-api.png)

この **[!UICONTROL API の設定]** 画面が表示されます。 次のオプションを選択します。 **[!UICONTROL キーペアを生成]**&#x200B;を選択し、「 **[!UICONTROL キーペアを生成]** をクリックします。

![](../images/api/getting-started/generate-key-pair.png)

鍵のペアが自動的に生成され、秘密鍵と公開証明書を含む ZIP ファイルがローカルマシンにダウンロードされます（後の手順で使用できます）。 選択 **[!UICONTROL 設定済み API を保存]** 設定を完了します。

![](../images/api/getting-started/key-pair-generated.png)

API がプロジェクトに追加されると、プロジェクトページが **Privacy ServiceAPI の概要** ページ。 ここから、 **[!UICONTROL サービスアカウント (JWT)]** セクションに含まれます。Privacy ServiceAPI へのすべての呼び出しで必要な、以下のアクセス資格情報が提供されます。

* **[!UICONTROL クライアント ID]**:クライアント ID は必須です `{API_KEY}` の値は、x-api-key ヘッダーで指定する必要があります。
* **[!UICONTROL 組織 ID]**:組織 ID は `{ORG_ID}` x-gw-ims-org-id ヘッダーで使用する必要がある値です。

![](../images/api/getting-started/jwt-credentials.png)

#### 各セッションの認証

収集する必要がある最後の資格情報は、 `{ACCESS_TOKEN}`:Authorization ヘッダーで使用されます。 の値とは異なる `{API_KEY}` および `{ORG_ID}`を使用している場合、 [!DNL Platform] API

新しい `{ACCESS_TOKEN}`」をクリックし、以前にダウンロードした秘密鍵を開き、その内容をの横にあるテキストボックスに貼り付けます。 **[!UICONTROL アクセストークンを生成]** 選択する前に **[!UICONTROL トークンを生成]**.

![](../images/api/getting-started/paste-private-key.png)

新しいアクセストークンが生成され、トークンをクリップボードにコピーするためのボタンが提供されます。 この値は、必要な Authorization ヘッダーに使用され、の形式で指定する必要があります `Bearer {ACCESS_TOKEN}`.

![](../images/api/getting-started/generated-access-token.png)

## API 呼び出し例の読み取り

このチュートリアルでは、API 呼び出しの例を提供し、リクエストの形式を設定する方法を示します。この中には、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。サンプル API 呼び出しのドキュメントで使用されている規則については、 Platform API の開始ガイドの[API 呼び出し例の読み方](../../landing/api-guide.md#sample-api)に関する節を参照してください。

## 次の手順

使用するヘッダーを理解できたので、Privacy Service API への呼び出しを開始できます。開始するには、次のいずれかのエンドポイントガイドを選択します。

* [プライバシージョブ](./privacy-jobs.md)
* [同意](./consent.md)
