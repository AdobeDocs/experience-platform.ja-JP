---
title: Reactor API の認証とアクセス
description: Reactor API の使用を開始する方法（必要なアクセス資格情報を生成する手順など）について説明します。
exl-id: fc1acc1d-6cfb-43c1-9ba9-00b2730cad5a
source-git-commit: 2c8ac35e9bf72c91743714da1591c3414db5c5e9
workflow-type: tm+mt
source-wordcount: '921'
ht-degree: 53%

---

# Reactor API の認証とアクセス

を使用するには、 [Reactor API](https://developer.adobe.com/experience-platform-apis/references/reactor/) タグ拡張機能を作成および管理するには、各リクエストに次の認証ヘッダーを含める必要があります。

* `Authorization: Bearer {ACCESS_TOKEN}`
* `x-api-key: {API_KEY}`
* `x-gw-ims-org-id: {ORG_ID}`

このガイドでは、Adobe Developer Console を使用してこれらの各ヘッダーの値を収集し、Reactor API の呼び出しを開始する方法について説明します。

## Adobe Experience Platform への開発者のアクセス {#gain-developer-access}

Reactor API の認証値を生成する前に、開発者が Experience Platform にアクセスできる必要があります。開発者アクセス権を取得するには、[Experience Platform 認証チュートリアル](/help/landing/api-authentication.md)の最初の手順に従ってください。完了したら、 [ユーザーアクセスの取得](/help/landing/api-authentication.md#gain-user-access) 手順を実行するには、このチュートリアルに戻り、Reactor API に固有の資格情報を生成します。

## アクセス認証情報の生成 {#generate-access-credentials}

Adobe Developer Console を使用して、次の 3 つのアクセス認証情報を生成する必要があります。

* `{ORG_ID}`
* `{API_KEY}`
* `{ACCESS_TOKEN}`

組織の ID (`{ORG_ID}`) および API キー (`{API_KEY}`) は、最初に生成された後、今後の API 呼び出しで再利用できます。 ただし、アクセストークン（`{ACCESS_TOKEN}`）は一時的なもので、24 時間ごとに再生成する必要があります。

これらの値を生成する手順については、以下で詳しく説明します。

### 1 回限りのセットアップ {#one-time-setup}

[Adobe Developer Console](https://www.adobe.com/go/devs_console_ui) に移動し 、Adobe ID を使用してログインします。次に、Developer Console のドキュメントにある[空のプロジェクトを作成](https://developer.adobe.com/developer-console/docs/guides/projects/projects-empty/)に関するチュートリアルで説明されている手順に従います。

プロジェクトを作成したら、**プロジェクトの概要**&#x200B;画面で「**API を追加**」を選択します。

![](../images/api/getting-started/add-api-button.png)

**API の追加**&#x200B;画面が表示されます。 選択 **Experience Platform LaunchAPI** を選択する前に、使用可能な API のリストから **次へ**.

![](../images/api/getting-started/add-launch-api.png)

次に、アクセストークンを生成する認証タイプを選択し、Experience PlatformAPI にアクセスします。

>[!IMPORTANT]
>
>を選択します。 **[!UICONTROL OAuth サーバー間]** メソッドのみを使用します。これは、今後の移行がサポートされる唯一のメソッドです。 この **[!UICONTROL サービスアカウント (JWT)]** メソッドは非推奨です。 JWT 認証方式を使用した統合は 2025 年 1 月 1 日まで引き続き機能しますが、Adobeでは、その日以前に既存の統合を新しい OAuth サーバー間方式に移行することを強くお勧めします。 詳しくは、の節を参照してください。 [!BADGE 非推奨]{type=negative}[JSON Web トークン (JWT) の生成](/help/landing/api-authentication.md#jwt) （「 Platform API 認証に関するチュートリアル」）を参照してください。

「**次へ**」をクリックして続行します。

![「 OAuth Server-to-Server 」認証方式を選択します。](/help/tags/images/api/getting-started/oauth-authentication-method.png)

次の画面では、API 統合に関連付ける 1 つ以上の製品プロファイルを選択します。

>[!NOTE]
>
製品プロファイルは、Adobe Admin Console を通じて組織によって管理され、詳細な機能に対する特定の権限セットが含まれています。製品プロファイルとその権限は、組織内の管理者権限を持つユーザーのみが管理できます。API 用に選択する製品プロファイルが不明な場合は、管理者に問い合わせてください。

リストから目的の製品プロファイルを選択し、「**設定済み API を保存**」を選択して API の登録を完了します。

![](../images/api/getting-started/select-product-profile.png)

###  資格情報の収集 {#gather-credentials}

API がプロジェクトに追加されると、 **[!UICONTROL Experience PlatformAPI]** プロジェクトのページには、Experience PlatformAPI へのすべての呼び出しで必要な次の資格情報が表示されます。

* `{API_KEY}` ([!UICONTROL クライアント ID])
* `{ORG_ID}` ([!UICONTROL 組織 ID])

![開発者コンソールで API を追加した後の統合情報。](/help/tags/images/api/getting-started/api-integration-information.png)

### アクセストークンの生成 {#generate-access-token}

次の手順では、 `{ACCESS_TOKEN}` Platform API 呼び出しで使用する資格情報。 の値とは異なる `{API_KEY}` および `{ORG_ID}`に設定する場合、Platform API を使用し続けるには、24 時間ごとに新しいトークンを生成する必要があります。

>[!TIP]
>
これらのトークンは、24 時間後に期限切れになります。この統合をアプリケーションに使用している場合は、アプリケーション内からプログラムでベアラートークンを取得することをお勧めします。

使用例に応じて、アクセストークンを生成するための 2 つのオプションがあります。

* [トークンの手動生成](#manual)
* [トークン生成の自動化](#auto-token)

#### アクセストークンの手動生成 {#manual}

新しい `{ACCESS_TOKEN}`に移動します。 **[!UICONTROL 資格情報]** > **[!UICONTROL OAuth サーバー間]** を選択し、 **[!UICONTROL アクセストークンを生成]**、以下に示すように。

![開発者コンソールの UI で、およびアクセストークンが生成された方法を示す画面記録。](/help/tags/images/api/getting-started/generate-access-token.gif)

新しいアクセストークンが生成され、トークンをクリップボードにコピーするためのボタンが表示されます。この値は、必要な Authorization ヘッダーに使用され、の形式で指定する必要があります `Bearer {ACCESS_TOKEN}`.

#### トークン生成の自動化 {#auto-token}

また、Postman環境とコレクションを使用してアクセストークンを生成することもできます。 詳しくは、 [Postmanを使用した API 呼び出しの認証とテスト](/help/landing/api-authentication.md#use-postman) (Experience PlatformAPI 認証ガイド ) を参照してください。

## API 資格情報のテスト {#test-api-credentials}

このチュートリアルの手順に従うことで、の有効な値を取得できます。 `{ORG_ID}`, `{API_KEY}`、および `{ACCESS_TOKEN}`. これらの値を、Reactor API への簡単な cURL リクエストで使用してテストできるようになりました。

まず、[すべての会社をリスト](./endpoints/companies.md#list)する API 呼び出しを試みます。

>[!NOTE]
>
組織に会社が存在しない場合、応答は HTTP ステータス 404（未検出）になります。403（禁止）エラーが発生しない限り、アクセス資格情報は有効となり、機能します。

アクセス資格情報が機能していることを確認したら、引き続き他の API リファレンスドキュメントを参照して、API の多くの機能を確認します。

## API 呼び出し例の読み取り {#read-sample-api-calls}

各エンドポイントガイドには、API 呼び出しの例が用意されており、リクエストの形式を設定する方法を示しています。 これには、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。サンプル API 呼び出しのドキュメントで使用されている規則については、 Platform API の開始ガイドの[API 呼び出し例の読み方](../../landing/api-guide.md#sample-api)に関する節を参照してください。

## 次の手順 {#next-steps}

これで、使用するヘッダーを理解できたので、Reactor API への呼び出しを開始する準備が整いました。 開始するには、次のいずれかのエンドポイントガイドを選択します。

* [Reactor API リファレンスドキュメント](https://developer.adobe.com/experience-platform-apis/references/reactor/)
* [Reactor API ガイドの概要](/help/tags/api/overview.md)
