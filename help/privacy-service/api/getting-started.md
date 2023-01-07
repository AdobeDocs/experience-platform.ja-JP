---
title: Privacy ServiceAPI の概要
description: Privacy ServiceAPI への認証方法とサンプル API 呼び出しの解釈方法については、ドキュメントを参照してください。
exl-id: c1d05e30-ef8f-4adf-87e0-1d6e3e9e9f9e
source-git-commit: 0f7ef438db5e7141197fb860a5814883d31ca545
workflow-type: tm+mt
source-wordcount: '919'
ht-degree: 19%

---

# Privacy ServiceAPI の概要

このガイドでは、Adobe Experience Platform Privacy Service API を呼び出す前に知っておく必要があるコア概念の概要を示します。

## 前提条件

このガイドでは、 [Privacy Service](../home.md) およびを使用すると、Adobe Experience Cloudアプリケーション全体でデータ主体（顧客）からのアクセス要求と削除要求を管理できます。

API のアクセス資格情報を作成するには、組織内の管理者がAdobe Admin Console内でPrivacy Service用の製品プロファイルを事前に設定しておく必要があります。 API 統合に割り当てる製品プロファイルによって、統合機能にアクセスする際に統合に付与されるPrivacy Serviceが決まります。 詳しくは、 [Privacy Service権限の管理](../permissions.md) を参照してください。

## 必須ヘッダーの値の収集

Privacy ServiceAPI を呼び出すには、まず、必要なヘッダーで使用するアクセス資格情報を収集する必要があります。

* `Authorization: Bearer {ACCESS_TOKEN}`
* `x-api-key: {API_KEY}`
* `x-gw-ims-org-id: {ORG_ID}`

これらの値は、 [Adobe Developer Console](https://developer.adobe.com/console). お使いの `{ORG_ID}` および `{API_KEY}` は 1 回だけ生成する必要があり、今後の API 呼び出しで再利用できます。 ただし、 `{ACCESS_TOKEN}` は一時的で、24 時間ごとに再生成する必要があります。

これらの値を生成する手順については、以下で詳しく説明します。

### 1 回限りのセットアップ

[Adobe Developer Console](https://developer.adobe.com/console) に移動し 、Adobe ID を使用してログインします。次に、Developer Console のドキュメントにある[空のプロジェクトを作成](https://developer.adobe.com/developer-console/docs/guides/projects/projects-empty/)に関するチュートリアルで説明されている手順に従います。

新しいプロジェクトを作成したら、「 **[!UICONTROL プロジェクトに追加]** を選択します。 **[!UICONTROL API]** をドロップダウンメニューから選択します。

![API オプションが [!UICONTROL プロジェクトに追加] 開発者コンソールのプロジェクトの詳細ページからのドロップダウン](../images/api/getting-started/add-api-button.png)

#### API を選択し、鍵のペアを生成します {#keypair}

**[!UICONTROL API の追加]**&#x200B;画面が表示されます。 選択 **[!UICONTROL Experience Cloud]** 使用可能な API のリストを絞り込むには、 **[!UICONTROL Privacy ServiceAPI]** 選択する前に **[!UICONTROL 次へ]**.

![使用可能な API のリストから選択されるPrivacy ServiceAPI カード](../images/api/getting-started/add-privacy-service-api.png)

この **[!UICONTROL API の設定]** 画面が表示されます。 次のオプションを選択します。 **[!UICONTROL キーペアを生成]**&#x200B;を選択し、「 **[!UICONTROL キーペアを生成]**.

![この [!UICONTROL キーペアを生成] オプションが [!UICONTROL API の設定] screen](../images/api/getting-started/generate-key-pair.png)

キーペアが自動的に生成され、秘密鍵と公開証明書を含む ZIP ファイルがブラウザーによってダウンロードされます（後の手順で使用します）。 「**[!UICONTROL 次へ]**」をクリックして続行します。

![UI に表示される生成されたキーペア。この値はブラウザーによって自動的にダウンロードされます](../images/api/getting-started/key-pair-generated.png)

#### 製品プロファイルを使用した権限の割り当て {#product-profiles}

最後の設定手順では、この統合の権限を継承する製品プロファイルを選択します。 複数のプロファイルを選択した場合、その権限セットは統合用に結合されます。

>[!NOTE]
>
>製品プロファイルとそれらが提供する詳細な権限は、管理者がAdobe Admin Consoleを通じて作成および管理します。 詳しくは、 [Privacy Service権限](../permissions.md) を参照してください。

終了したら、「 」を選択します。 **[!UICONTROL 設定済み API を保存]**.

![設定を保存する前にリストから 1 つの製品プロファイルが選択されている](../images/api/getting-started/select-product-profiles.png)

API がプロジェクトに追加されると、プロジェクトページが **Privacy ServiceAPI の概要** ページ。 ここから、 **[!UICONTROL サービスアカウント (JWT)]** セクションに含まれます。Privacy ServiceAPI へのすべての呼び出しで必要な、以下のアクセス資格情報が提供されます。

* **[!UICONTROL クライアント ID]**:クライアント ID は必須です `{API_KEY}` を `x-api-key` ヘッダー。
* **[!UICONTROL 組織 ID]**：組織 ID は、`x-gw-ims-org-id` ヘッダーで使用する必要がある `{ORG_ID}` 値です。

![API を設定した後、プロジェクトの概要ページに表示されるクライアント ID と組織 ID の値](../images/api/getting-started/jwt-credentials.png)

### 各セッションの認証

収集する必要がある最後の資格情報は、 `{ACCESS_TOKEN}`:Authorization ヘッダーで使用されます。 の値とは異なる `{API_KEY}` および `{ORG_ID}`に値を指定する場合、API を引き続き使用するには、24 時間ごとに新しいトークンを生成する必要があります。

一般に、アクセストークンを生成する方法は 2 つあります。

* [トークンを手動で生成](#manual-token) テストおよび開発用。
* [トークン生成の自動化](#auto-token) （API 統合用）

#### トークンを手動で生成 {#manual-token}

新しい `{ACCESS_TOKEN}`」をクリックし、以前にダウンロードした秘密鍵を開き、その内容をの横にあるテキストボックスに貼り付けます。 **[!UICONTROL アクセストークンを生成]** 選択する前に **[!UICONTROL トークンを生成]**.

![以前に生成されたアクセストークンが、プロジェクトの概要ページに貼り付けられ、 [!UICONTROL トークンを生成] 次の後に選択されたボタン](../images/api/getting-started/paste-private-key.png)

新しいアクセストークンが生成され、トークンをクリップボードにコピーするためのボタンが提供されます。 この値は、必要な Authorization ヘッダーに使用され、の形式で指定する必要があります `Bearer {ACCESS_TOKEN}`.

![UI からコピーされる生成されたアクセストークン](../images/api/getting-started/generated-access-token.png)

#### トークン生成の自動化 {#auto-token}

AdobeIdentity Managementサービス (IMS) にPOSTリクエストを通じて JSON Web トークン (JWT) を送信することで、自動プロセス用の新しいアクセストークンを生成できます。 次のドキュメントにある開発者コンソールのドキュメントを参照してください。 [JWT 認証](https://developer.adobe.com/developer-console/docs/guides/authentication/JWT/) を参照してください。

## API 呼び出し例の読み取り

各エンドポイントガイドには、API 呼び出しの例が用意されており、リクエストの形式を設定する方法を示しています。 これには、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。サンプル API 呼び出しのドキュメントで使用されている規則については、 Platform API の開始ガイドの[API 呼び出し例の読み方](../../landing/api-guide.md#sample-api)に関する節を参照してください。

## 次の手順

使用するヘッダーを理解できたので、Privacy Service API への呼び出しを開始できます。開始するには、次のいずれかのエンドポイントガイドを選択します。

* [プライバシージョブ](./privacy-jobs.md)
* [同意](./consent.md)
