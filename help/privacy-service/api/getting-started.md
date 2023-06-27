---
title: Privacy ServiceAPI の認証とアクセス
description: Privacy ServiceAPI への認証方法とサンプル API 呼び出しの解釈方法については、ドキュメントを参照してください。
exl-id: c1d05e30-ef8f-4adf-87e0-1d6e3e9e9f9e
source-git-commit: 2c8ac35e9bf72c91743714da1591c3414db5c5e9
workflow-type: tm+mt
source-wordcount: '862'
ht-degree: 21%

---

# Privacy ServiceAPI の認証とアクセス

このガイドでは、Adobe Experience Platform Privacy Service API を呼び出す前に知っておく必要があるコア概念の概要を提供します。

## 前提条件 {#prerequisites}

このガイドでは、 [Privacy Service](../home.md) およびを使用すると、Adobe Experience Cloudアプリケーション全体でデータ主体（顧客）からのアクセス要求と削除要求を管理できます。

API のアクセス資格情報を作成するには、組織内の管理者がAdobe Admin Console内でPrivacy Service用の製品プロファイルを事前に設定しておく必要があります。 API 統合に割り当てる製品プロファイルによって、統合機能にアクセスする際に統合に付与されるPrivacy Serviceが決まります。 詳しくは、 [Privacy Service権限の管理](../permissions.md) を参照してください。

## 必須ヘッダーの値の収集 {#gather-values-required-headers}

Privacy ServiceAPI を呼び出すには、まず、必要なヘッダーで使用するアクセス資格情報を収集する必要があります。

* `Authorization: Bearer {ACCESS_TOKEN}`
* `x-api-key: {API_KEY}`
* `x-gw-ims-org-id: {ORG_ID}`

これらの値は、 [Adobe Developer Console](https://developer.adobe.com/console). お使いの `{ORG_ID}` および `{API_KEY}` は 1 回だけ生成する必要があり、今後の API 呼び出しで再利用できます。 ただし、 `{ACCESS_TOKEN}` は一時的で、24 時間ごとに再生成する必要があります。

これらの値を生成する手順については、以下で詳しく説明します。

### 1 回限りのセットアップ {#one-time-setup}

[Adobe Developer Console](https://developer.adobe.com/console) に移動し 、Adobe ID を使用してログインします。次に、Developer Console のドキュメントにある[空のプロジェクトを作成](https://developer.adobe.com/developer-console/docs/guides/projects/projects-empty/)に関するチュートリアルで説明されている手順に従います。

新しいプロジェクトを作成したら、「 **[!UICONTROL プロジェクトに追加]** を選択します。 **[!UICONTROL API]** をドロップダウンメニューから選択します。

![API オプションが [!UICONTROL プロジェクトに追加] 開発者コンソールのプロジェクトの詳細ページからのドロップダウン](../images/api/getting-started/add-api-button.png)

#### Privacy ServiceAPI を選択 {#select-privacy-service-api}

**[!UICONTROL API の追加]**&#x200B;画面が表示されます。 選択 **[!UICONTROL Experience Cloud]** 使用可能な API のリストを絞り込むには、 **[!UICONTROL Privacy ServiceAPI]** 選択する前に **[!UICONTROL 次へ]**.

![使用可能な API のリストから選択されるPrivacy ServiceAPI カード](../images/api/getting-started/add-privacy-service-api.png)

>[!TIP]
>
>を選択します。 **[!UICONTROL ドキュメントを表示]** 別のブラウザーウィンドウで [Privacy ServiceAPI リファレンスドキュメント](https://developer.adobe.com/experience-platform-apis/references/privacy-service/).

次に、アクセストークンを生成する認証タイプを選択し、Privacy ServiceAPI にアクセスします。

>[!IMPORTANT]
>
>を選択します。 **[!UICONTROL OAuth サーバー間]** メソッドのみを使用します。これは、今後の移行がサポートされる唯一のメソッドです。 この **[!UICONTROL サービスアカウント (JWT)]** メソッドは非推奨です。 JWT 認証方式を使用した統合は 2025 年 1 月 1 日まで引き続き機能しますが、Adobeでは、その日以前に既存の統合を新しい OAuth サーバー間方式に移行することを強くお勧めします。 詳しくは、の節を参照してください。 [!BADGE 非推奨]{type=negative}[JSON Web トークン (JWT) の生成](/help/landing/api-authentication.md#jwt).

![「Oauth Server-to-Server」認証方式を選択します。](/help/privacy-service/images/api/getting-started/select-oauth-authentication.png)

#### 製品プロファイルを使用した権限の割り当て {#product-profiles}

最後の設定手順では、この統合の権限を継承する製品プロファイルを選択します。 複数のプロファイルを選択した場合、その権限セットは統合用に結合されます。

>[!NOTE]
>
製品プロファイルとそれらが提供する詳細な権限は、管理者がAdobe Admin Consoleを通じて作成および管理します。 詳しくは、 [Privacy Service権限](../permissions.md) を参照してください。

終了したら、「 」を選択します。 **[!UICONTROL 設定済み API を保存]**.

![設定を保存する前にリストから 1 つの製品プロファイルが選択されている](../images/api/getting-started/select-product-profiles.png)

API がプロジェクトに追加されると、 **[!UICONTROL Privacy ServiceAPI]** プロジェクトのページには、Privacy ServiceAPI へのすべての呼び出しで必要な次の資格情報が表示されます。

* `{API_KEY}` ([!UICONTROL クライアント ID])
* `{ORG_ID}` ([!UICONTROL 組織 ID])

![開発者コンソールで API を追加した後の統合情報。](/help/privacy-service/images/api/getting-started/api-integration-information.png)

### 各セッションの認証 {#authentication-each-session}

収集する必要がある最後の資格情報は、 `{ACCESS_TOKEN}`:Authorization ヘッダーで使用されます。 の値とは異なる `{API_KEY}` および `{ORG_ID}`に値を指定する場合、API を引き続き使用するには、24 時間ごとに新しいトークンを生成する必要があります。

一般に、アクセストークンを生成する方法は 2 つあります。

* [トークンを手動で生成](#manual-token) テストおよび開発用。
* [トークン生成の自動化](#auto-token) （API 統合用）

#### トークンを手動で生成 {#manual-token}

新しい `{ACCESS_TOKEN}`に移動します。 **[!UICONTROL 資格情報]** > **[!UICONTROL OAuth サーバー間]** を選択し、 **[!UICONTROL アクセストークンを生成]**、以下に示すように。

![開発者コンソールの UI でのアクセストークンの生成方法を記録する画面です。](/help/privacy-service/images/api/getting-started/generate-access-token.gif)

新しいアクセストークンが生成され、トークンをクリップボードにコピーするためのボタンが表示されます。この値は、必要な [!DNL Authorization] ヘッダーに使用され、`Bearer {ACCESS_TOKEN}` の形式で指定する必要があります。

#### トークン生成の自動化 {#auto-token}

また、Postman環境とコレクションを使用してアクセストークンを生成することもできます。 詳しくは、 [Postmanを使用した API 呼び出しの認証とテスト](/help/landing/api-authentication.md#use-postman) (Experience PlatformAPI 認証ガイド ) を参照してください。

## API 呼び出し例の読み取り {#read-sample-api-calls}

各エンドポイントガイドには、API 呼び出しの例が用意されており、リクエストの形式を設定する方法を示しています。 これには、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。サンプル API 呼び出しのドキュメントで使用されている規則については、 Platform API の開始ガイドの[API 呼び出し例の読み方](../../landing/api-guide.md#sample-api)に関する節を参照してください。

## 次の手順 {#next-steps}

使用するヘッダーを理解できたので、Privacy Service API への呼び出しを開始できます。開始するには、次のいずれかのエンドポイントガイドを選択します。

* [プライバシージョブ](./privacy-jobs.md)
* [同意](./consent.md)
