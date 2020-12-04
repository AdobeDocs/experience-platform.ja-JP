---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Privacy Service 開発者ガイド
description: RESTful API を使用して、Adobe Experience Cloud アプリケーション全体でデータサブジェクトの個人データを管理します。
topic: developer guide
translation-type: tm+mt
source-git-commit: 28b733a16b067f951a885c299d59e079f0074df8
workflow-type: tm+mt
source-wordcount: '759'
ht-degree: 30%

---


# [!DNL Privacy Service] 開発ガイド

Adobe Experience Platform [!DNL Privacy Service] provides a RESTful API and user interface that allow you to manage (access and delete) the personal data of your data subjects (customers) across Adobe Experience Cloud applications. [!DNL Privacy Service]また、 は、 アプリケーションに関連するジョブのステータスと結果にアクセスできる、中央監査とログのメカニズムも提供します。[!DNL Experience Cloud]

This guide covers how to use the [!DNL Privacy Service] API. UI の使用方法について詳しくは、「[Privacy Service サービス UI の概要](../ui/overview.md)」を参照してください。For a comprehensive list of all available endpoints in the [!DNL Privacy Service] API, please see the [API reference](https://www.adobe.io/apis/experiencecloud/gdpr/api-reference.html).

## はじめに {#getting-started}

This guide requires a working understanding the following [!DNL Experience Platform] features:

* [[!DNL Privacy Service]](../home.md)：Adobe Experience Cloud アプリケーション全体でデータサブジェクト（顧客）からのアクセスリクエストと削除リクエストを管理するための RESTful API とユーザーインターフェイスが用意されていまます。

以下の節では、Privacy Service API への呼び出しを正常に実行するために必要な追加情報を示しています。

### API 呼び出し例の読み取り

このチュートリアルでは、API 呼び出しの例を提供し、リクエストの形式を設定する方法を示します。この中には、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、 トラブルシューテングガイドの[API 呼び出し例の読み方](../../landing/troubleshooting.md)に関する節を参照してください。[!DNL Experience Platform]

## 必須ヘッダーの値の収集

APIを呼び出すには、最初にアクセス資格情報を収集して、必要なヘッダーで使用する必要があります。 [!DNL Privacy Service]

* Authorization: Bearer `{ACCESS_TOKEN}`
* x-api-key: `{API_KEY}`
* x-gw-ims-org-id: `{IMS_ORG}`

これには、Adobe Admin Consoleでの開発者権限を取得し [!DNL Experience Platform] 、Adobeデベロッパーコンソールで資格情報を生成する必要があります。

### 開発者向けのアクセス権の取得 [!DNL Experience Platform]

開発者がにアクセスできるようにするには [!DNL Platform]、 [Experience Platform認証チュートリアルの最初の手順に従い](../../tutorials/authentication.md)ます。 「Adobe開発者コンソールでアクセス資格情報を生成する」の手順に進んだら、このチュートリアルに戻って、に固有の資格情報を生成し [!DNL Privacy Service]ます。

### アクセス資格情報の生成

AdobeDeveloper Consoleを使用して、次の3つのアクセス資格情報を生成する必要があります。

* `{IMS_ORG}`
* `{API_KEY}`
* `{ACCESS_TOKEN}`

およ `{IMS_ORG}``{API_KEY}` びは1回だけ生成する必要があり、今後のAPI呼び出しで再利用できます。 ただし、一時的な `{ACCESS_TOKEN}` ので、24時間ごとに再生成する必要があります。

これらの値の生成手順の詳細は以下のとおりです。

#### 1回限りのセットアップ

[Adobeデベロッパーコンソールに移動し](https://www.adobe.com/go/devs_console_ui) 、Adobe IDでサインインします。 次に、Adobe開発者コンソールのドキュメントで、空のプロジェクトの [作成に関するチュートリアルに説明されている手順に従います](https://www.adobe.io/apis/experienceplatform/console/docs.html#!AdobeDocs/adobeio-console/master/projects-empty.md) 。

新しいプロジェクトを作成したら、プ **[!UICONTROL ロジェクト概要]** 画面の「 **[!UICONTROL API]** 」をクリックします。

![](../images/api/getting-started/add-api-button.png)

API **** 追加画面が表示されます。 「 **[!UICONTROL 次へ]** 」をクリックする前に、利用可能なAPIのリストから **[!UICONTROL Privacy ServiceAPIを選択します]**。

![](../images/api/getting-started/add-privacy-service-api.png)

The **[!UICONTROL Configure API]** screen appears. 「キーペアを **[!UICONTROL 生成する」オプションを選択し]**、右下隅の「キーペアを **[!UICONTROL 生成]** 」をクリックします。

![](../images/api/getting-started/generate-key-pair.png)

キーペアが自動的に生成され、秘密鍵と公開証明書を含むZIPファイルがローカルマシンにダウンロードされます（後の手順で使用します）。 「設定済みAPI **[!UICONTROL を保存]** 」を選択して設定を完了します。

![](../images/api/getting-started/key-pair-generated.png)

APIがプロジェクトに追加されると、 **Privacy ServiceAPIの概要** ページにプロジェクトページが再び表示されます。 ここから、「 **[!UICONTROL サービスアカウント(JWT)]** 」セクションまで下にスクロールします。このセクションには、 [!DNL Privacy Service] APIへのすべての呼び出しに必要な次のアクセス資格情報が表示されます。

* **[!UICONTROL クライアントID]**:クライアントIDは、x-api-keyヘッダー `{API_KEY}` で指定する必要があるクライアントIDです。
* **[!UICONTROL 組織ID]**:組織IDは、x-gw-ims-org-idヘッダーで使用する必要がある `{IMS_ORG}` 値です。

![](../images/api/getting-started/jwt-credentials.png)

#### 各セッションの認証

収集する必要がある最後の必要な秘密鍵証明書 `{ACCESS_TOKEN}`は、「認証」ヘッダーで使用される、自分の秘密鍵証明書です。 との値とは異なり、APIを使用し続け `{API_KEY}` るに `{IMS_ORG}`は、新しいトークンを24時間ごとに生成する必要があり [!DNL Platform] ます。

新しい秘密鍵を生成するに `{ACCESS_TOKEN}`は、「トークンの生成」をクリックする前に、以前にダウンロードした秘密鍵を開き、「 **[!UICONTROL アクセストークンの]** 生成 **[!UICONTROL 」の横のテキストボックスにその内容を貼り付けます]**。

![](../images/api/getting-started/paste-private-key.png)

新しいアクセストークンが生成され、トークンをクリップボードにコピーするためのボタンが提供されます。 この値は、必要な認証ヘッダーに使用され、形式で指定する必要があり `Bearer {ACCESS_TOKEN}`ます。

![](../images/api/getting-started/generated-access-token.png)

## 次の手順

Now that you understand what headers to use, you are ready to begin making calls to the [!DNL Privacy Service] API. [プライバシージョブ](privacy-jobs.md)に関するドキュメントでは、 API を使用して実行できる様々な API 呼び出しについて説明します。[!DNL Privacy Service]各呼び出し例では一般的な API 形式、必須ヘッダーを示すサンプルリクエストおよびサンプルレスポンスが示されています。
