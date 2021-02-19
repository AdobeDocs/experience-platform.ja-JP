---
keywords: Experience Platform；ホーム；人気の高いトピック
solution: Experience Platform
title: Privacy ServiceAPIガイド
description: Privacy ServiceAPIを使用すると、開発者は、法的なプライバシー規制に準拠して、Experience Cloudアプリケーション全体で個人データにアクセスしたり削除したりするための顧客リクエストを作成および管理できます。 このガイドに従って、APIを使用した主な操作の実行方法を学習します。
topic: developer guide
translation-type: tm+mt
source-git-commit: e649ab3da077cdd8e98562199b8bdece6108a572
workflow-type: tm+mt
source-wordcount: '795'
ht-degree: 26%

---


# [!DNL Privacy Service] APIガイド

Adobe Experience Platform[!DNL Privacy Service]は、RESTful APIとユーザーインターフェイスを提供し、Adobe Experience Cloudのアプリケーションを介してデータサブジェクト（お客様）の個人データを管理（アクセスおよび削除）できます。 [!DNL Privacy Service]また、 は、 アプリケーションに関連するジョブのステータスと結果にアクセスできる、中央監査とログのメカニズムも提供します。[!DNL Experience Cloud]

このガイドは[!DNL Privacy Service] APIの使用方法を説明しています。 UI の使用方法について詳しくは、「[Privacy Service サービス UI の概要](../ui/overview.md)」を参照してください。[!DNL Privacy Service] APIで使用可能なすべてのエンドポイントの包括的なリストについては、[APIリファレンス](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/privacy-service.yaml)を参照してください。

## はじめに {#getting-started}

このガイドでは、次の[!DNL Experience Platform]機能を理解している必要があります。

* [[!DNL Privacy Service]](../home.md)：Adobe Experience Cloud アプリケーション全体でデータサブジェクト（顧客）からのアクセスリクエストと削除リクエストを管理するための RESTful API とユーザーインターフェイスが用意されていまます。

以下の節では、Privacy Service API への呼び出しを正常に実行するために必要な追加情報を示しています。

### API 呼び出し例の読み取り

このチュートリアルでは、API 呼び出しの例を提供し、リクエストの形式を設定する方法を示します。この中には、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、 トラブルシューテングガイドの[API 呼び出し例の読み方](../../landing/troubleshooting.md)に関する節を参照してください。[!DNL Experience Platform]

## 必須ヘッダーの値の収集

[!DNL Privacy Service] APIを呼び出すには、まずアクセス資格情報を収集して、必要なヘッダーで使用する必要があります。

* Authorization: Bearer `{ACCESS_TOKEN}`
* x-api-key: `{API_KEY}`
* x-gw-ims-org-id: `{IMS_ORG}`

これには、Adobe Admin Consoleで[!DNL Experience Platform]の開発者権限を取得し、Adobeデベロッパーコンソールで資格情報を生成する必要があります。

### [!DNL Experience Platform]への開発者アクセスを得る

[!DNL Platform]への開発者アクセスを得るには、[Experience Platform認証チュートリアル](https://www.adobe.com/go/platform-api-authentication-en)の最初の手順に従ってください。 「Adobe開発者コンソールでアクセス資格情報を生成する」の手順に進んだら、このチュートリアルに戻って[!DNL Privacy Service]専用の資格情報を生成します。

### アクセス資格情報の生成

AdobeDeveloper Consoleを使用して、次の3つのアクセス資格情報を生成する必要があります。

* `{IMS_ORG}`
* `{API_KEY}`
* `{ACCESS_TOKEN}`

`{IMS_ORG}`と`{API_KEY}`は1回だけ生成する必要があり、今後のAPI呼び出しで再利用できます。 ただし、`{ACCESS_TOKEN}`は一時的で、24時間ごとに再生成する必要があります。

これらの値の生成手順の詳細は以下のとおりです。

#### 1回限りのセットアップ

[Adobeデベロッパーコンソール](https://www.adobe.com/go/devs_console_ui)に移動し、Adobe IDでサインインします。 次に、Adobe開発者コンソールドキュメントの[空のプロジェクト](https://www.adobe.io/apis/experienceplatform/console/docs.html#!AdobeDocs/adobeio-console/master/projects-empty.md)の作成に関するチュートリアルに説明されている手順に従います。

新しいプロジェクトを作成したら、**[!UICONTROL プロジェクトの概要]**&#x200B;画面で追加&#x200B;**[!UICONTROL API]**&#x200B;を選択します。

![](../images/api/getting-started/add-api-button.png)

**[!UICONTROL 追加API]**&#x200B;画面が表示されます。 **[!UICONTROL 次へ]**&#x200B;を選択する前に、使用可能なAPIのリストから&#x200B;**[!UICONTROL Privacy ServiceAPI]**&#x200B;を選択します。

![](../images/api/getting-started/add-privacy-service-api.png)

**[!UICONTROL APIを設定]**&#x200B;画面が表示されます。 「**[!UICONTROL キーペアを生成]**」オプションを選択し、右下隅の「**[!UICONTROL キーペアを生成]**」を選択します。

![](../images/api/getting-started/generate-key-pair.png)

キーペアが自動的に生成され、秘密鍵と公開証明書を含むZIPファイルがローカルマシンにダウンロードされます（後の手順で使用します）。 「**[!UICONTROL 設定済みAPI]**&#x200B;を保存」を選択して、設定を完了します。

![](../images/api/getting-started/key-pair-generated.png)

APIがプロジェクトに追加されると、**Privacy ServiceAPIの概要**&#x200B;ページにプロジェクトページが再表示されます。 ここから、**[!UICONTROL サービスアカウント(JWT)]**&#x200B;セクションまで下にスクロールします。このセクションでは、[!DNL Privacy Service] APIのすべての呼び出しで必要な次のアクセス資格情報が提供されます。

* **[!UICONTROL クライアントID]**:クライアントIDは、x-api-keyヘッダー `{API_KEY}` で指定する必要があるクライアントIDです。
* **[!UICONTROL 組織ID]**:組織IDは、x-gw-ims-org-idヘッダーで使用する必要がある `{IMS_ORG}` 値です。

![](../images/api/getting-started/jwt-credentials.png)

#### 各セッションの認証

最後に必要な資格情報は`{ACCESS_TOKEN}`です。これは、認証ヘッダーで使用されます。 `{API_KEY}`および`{IMS_ORG}`の値と異なり、[!DNL Platform] APIを使用し続けるには、24時間ごとに新しいトークンを生成する必要があります。

新しい`{ACCESS_TOKEN}`を生成するには、以前にダウンロードした秘密鍵を開き、**[!UICONTROL 「アクセストークン]**&#x200B;を生成」の横のテキストボックスにその内容を貼り付けてから、**[!UICONTROL トークン]**&#x200B;を生成します。

![](../images/api/getting-started/paste-private-key.png)

新しいアクセストークンが生成され、トークンをクリップボードにコピーするためのボタンが提供されます。 この値は、必要な認証ヘッダーに使用され、`Bearer {ACCESS_TOKEN}`の形式で指定する必要があります。

![](../images/api/getting-started/generated-access-token.png)

## 次の手順

これで、使用するヘッダーがわかったので、[!DNL Privacy Service] APIを呼び出す準備が整いました。 [プライバシージョブ](privacy-jobs.md)に関するドキュメントでは、 API を使用して実行できる様々な API 呼び出しについて説明します。[!DNL Privacy Service]各呼び出し例では一般的な API 形式、必須ヘッダーを示すサンプルリクエストおよびサンプルレスポンスが示されています。
