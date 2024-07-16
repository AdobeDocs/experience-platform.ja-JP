---
title: Privacy Service API の認証とアクセス
description: Privacy ServiceAPI への認証方法とサンプル API 呼び出しの解釈方法については、ドキュメントを参照してください。
role: Developer
exl-id: c1d05e30-ef8f-4adf-87e0-1d6e3e9e9f9e
source-git-commit: c16ce1020670065ecc5415bc3e9ca428adbbd50c
workflow-type: tm+mt
source-wordcount: '851'
ht-degree: 18%

---

# Privacy Service API の認証とアクセス

このガイドでは、Adobe Experience Platform Privacy Service API を呼び出す前に知っておく必要があるコア概念の概要を説明します。

## 前提条件 {#prerequisites}

このガイドでは、[Privacy Service](../home.md) およびAdobe Experience Cloud アプリケーション全体のデータ主体（顧客）からのアクセスリクエストと削除リクエストを管理する方法について実際に理解している必要があります。

API のアクセス資格情報を作成するには、組織内の管理者がAdobe Admin Console内でPrivacy Serviceするための製品プロファイルを事前に設定している必要があります。 API 統合に割り当てる製品プロファイルによって、その統合がPrivacy Service機能にアクセスする際に持つ権限が決まります。 詳しくは、[Privacy Service権限の管理 ](../permissions.md) のガイドを参照してください。

## 必須ヘッダーの値の収集 {#gather-values-required-headers}

ヘッダー API を呼び出すには、まず必要なPrivacy Serviceで使用されるアクセス資格情報を収集する必要があります。

* `Authorization: Bearer {ACCESS_TOKEN}`
* `x-api-key: {API_KEY}`
* `x-gw-ims-org-id: {ORG_ID}`

これらの値は、[Adobe Developer Console](https://developer.adobe.com/console) を使用して生成されます。 `{ORG_ID}` と `{API_KEY}` は 1 回だけ生成する必要があり、今後の API 呼び出しで再利用できます。 ただし、`{ACCESS_TOKEN}` は一時的なもので、24 時間ごとに再生成する必要があります。

これらの値を生成する手順については、以下で詳しく説明します。

### 1 回限りのセットアップ {#one-time-setup}

[Adobe Developer Console](https://developer.adobe.com/console) に移動し 、Adobe ID を使用してログインします。次に、Developer Console のドキュメントにある[空のプロジェクトを作成](https://developer.adobe.com/developer-console/docs/guides/projects/projects-empty/)に関するチュートリアルで説明されている手順に従います。

新しいプロジェクトを作成したら、「**[!UICONTROL プロジェクトに追加]**」を選択し、ドロップダウンメニューから「**[!UICONTROL API]**」を選択します。

![Developer Consoleのプロジェクトの詳細ページの [!UICONTROL  プロジェクトに追加 ] ドロップダウンから選択されている「API」オプション ](../images/api/getting-started/add-api-button.png)

#### Privacy ServiceAPI を選択します。 {#select-privacy-service-api}

**[!UICONTROL API の追加]**&#x200B;画面が表示されます。 「**[!UICONTROL Experience Cloud]**」を選択して使用可能な API のリストを絞り込み、「**[!UICONTROL Privacy ServiceAPI]**」のカードを選択して **[!UICONTROL 「次へ]**」を選択します。

![ 使用可能な API のリストから選択されているPrivacy ServiceAPI カード ](../images/api/getting-started/add-privacy-service-api.png)

>[!TIP]
>
>**[!UICONTROL ドキュメントを表示]** オプションを選択して、別のブラウザーウィンドウに移動し、[Privacy ServiceAPI リファレンスドキュメント ](https://developer.adobe.com/experience-platform-apis/references/privacy-service/) を参照します。

次に、認証タイプを選択してアクセストークンを生成し、Privacy ServiceAPI にアクセスします。

>[!IMPORTANT]
>
>今後は **[!UICONTROL OAuth サーバー間]** の方法のみがサポートされるので、この方法を選択します。 **[!UICONTROL サービスアカウント（JWT）]** メソッドは非推奨（廃止予定）となりました。 JWT 認証方法を使用した統合は 2025 年 1 月 1 日（PT）まで引き続き機能しますが、Adobeでは、その日までに既存の統合を新しい OAuth サーバー間認証方法に移行することを強くお勧めします。 詳しくは、「[!BADGE  非推奨 ]」の節を参照してください。{type=negative}[JSON web トークン（JWT）を生成 ](/help/landing/api-authentication.md#jwt) ます。

![Oauth サーバー間認証方法を選択します。](/help/privacy-service/images/api/getting-started/select-oauth-authentication.png)

#### 製品プロファイルを使用した権限の割り当て {#product-profiles}

最後の設定手順では、この統合の権限継承元となる製品プロファイルを選択します。 複数のプロファイルを選択すると、それらの権限セットが統合のために組み合わされます。

>[!NOTE]
>
製品プロファイルとそれらが提供する詳細な権限は、Adobe Admin Consoleを通じて管理者が作成および管理します。 詳しくは、[Privacy Serviceの権限 ](../permissions.md) に関するガイドを参照してください。

終了したら、「**[!UICONTROL 設定済み API を保存]**」を選択します。

![ 設定を保存する前に、リストから 1 つの製品プロファイルが選択されている ](../images/api/getting-started/select-product-profiles.png)

API がプロジェクトに追加されると、プロジェクトの **[!UICONTROL Privacy ServiceAPI]** ページに、Privacy ServiceAPI へのすべての呼び出しで必要な次の資格情報が表示されます。

* `{API_KEY}` （[!UICONTROL  クライアント ID]）
* `{ORG_ID}` （[!UICONTROL  組織 ID]）

![Developer Consoleに API を追加した後の統合情報 ](/help/privacy-service/images/api/getting-started/api-integration-information.png)

### 各セッションの認証 {#authentication-each-session}

収集する必要がある最終的な必須資格情報は、`{ACCESS_TOKEN}` です。これは、Authorization ヘッダーで使用されます。 `{API_KEY}` や `{ORG_ID}` の値とは異なり、API を引き続き使用するには、24 時間ごとに新しいトークンを生成する必要があります。

一般に、アクセストークンは次の 2 とおりの方法で生成されます。

* テストおよび開発用に [ 手動でトークンを生成 ](#manual-token) します。
* API 統合用の [ トークン生成の自動化 ](#auto-token)。

#### トークンの手動生成 {#manual-token}

新しい `{ACCESS_TOKEN}` を手動で生成するには、次に示すように、**[!UICONTROL 資格情報]**/**[!UICONTROL OAuth サーバー間]** に移動し、「**[!UICONTROL アクセストークンを生成]**」を選択します。

![Developer Console UI でのアクセストークンの生成方法を示す画面記録。](/help/privacy-service/images/api/getting-started/generate-access-token.gif)

新しいアクセストークンが生成され、トークンをクリップボードにコピーするためのボタンが表示されます。この値は、必要な [!DNL Authorization] ヘッダーに使用され、`Bearer {ACCESS_TOKEN}` の形式で指定する必要があります。

#### トークン生成の自動化 {#auto-token}

また、Postman環境とコレクションを使用してアクセストークンを生成することもできます。 詳しくは、Experience PlatformAPI 認証ガイドの [Postmanを使用した API 呼び出しの認証およびテスト ](/help/landing/api-authentication.md#use-postman) に関する節を参照してください。

## API 呼び出し例の読み取り {#read-sample-api-calls}

各エンドポイントガイドには、API 呼び出しの例とリクエストの形式を指定する方法が示されています。 これには、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。サンプル API 呼び出しのドキュメントで使用されている規則については、Platform API の開始ガイドの [ サンプル API 呼び出しの読み方 ](../../landing/api-guide.md#sample-api) に関する節を参照してください。

## 次の手順 {#next-steps}

使用するヘッダーを理解できたので、Privacy Service API への呼び出しを開始できます。開始するには、エンドポイントガイドの 1 つを選択します。

* [プライバシージョブ](./privacy-jobs.md)
* [同意](./consent.md)
