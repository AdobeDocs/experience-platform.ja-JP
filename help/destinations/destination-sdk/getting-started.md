---
description: このページでは、Adobe Experience Platform Destination SDKを認証し、使用を開始する方法について説明します。 これには、Adobe I/O認証資格情報、サンドボックス名、宛先オーサリングのアクセス制御権限の取得方法に関する手順が含まれています。
title: Destination SDKの概要
exl-id: f22c37a8-202d-49ac-9af0-545dfa9af8fd
source-git-commit: 468b9309c5184684c0b25c2656a9eef37715af53
workflow-type: tm+mt
source-wordcount: '597'
ht-degree: 7%

---

# はじめに

## 概要 {#overview}

このページでは、Adobe Experience Platform Destination SDKを認証し、使用を開始する方法について説明します。 これには、Adobe I/O認証資格情報、サンドボックス名、宛先オーサリングのアクセス制御権限の取得方法に関する手順が含まれています。

## 用語 {#terminology}

このガイドでは、IMS 組織やサンドボックスなど、Platform 固有の概念を使用します。 詳しくは、 [Experience Platform用語集](https://experienceleague.adobe.com/docs/experience-platform/landing/glossary.html) を参照してください。

## 必要な認証資格情報の取得 {#obtain-authentication-credentials}

Destination SDKは [Adobe I/O](https://www.adobe.io/) 認証用のゲートウェイ。 Destination SDKエンドポイントに対する API 呼び出しをおこなうには、API 呼び出しに特定のヘッダーを指定する必要があります。 Exchange のAdobeチームと協力して、 [Adobe開発者コンソール](https://developer.adobe.com/console).

Destination SDK API エンドポイントを正常に呼び出すには、 [Experience Platform認証のチュートリアル](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja). チュートリアルを「[API キー、IMS Org ID およびクライアントの秘密鍵を生成します](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html#api-ims-secret)&quot;ステップ。 Adobe交換チームが前の手順を処理します。 次に示すように、Destination SDK API 呼び出しの各必要なヘッダーの値は、認証に関するチュートリアルで説明されています。

* `x-api-key: {API_KEY}`（クライアント ID とも呼ばれます）
* `x-gw-ims-org-id: {IMS_ORG}`（組織 ID とも呼ばれます）
* `Authorization: Bearer {ACCESS_TOKEN}`に対する質問に回答します。アクセストークンの有効期限は 24 時間で、ミリ秒単位で指定されているので、更新する必要があります。 アクセストークンを更新するには、認証に関するチュートリアルで説明されている手順を繰り返します。

<!--

### Obtain `Authorization: Bearer {ACCESS_TOKEN}`

To obtain the `{ACCESS_TOKEN}`, you must generate a JWT token and exchange it for the access token. Follow the steps below:

1. Follow the instructions in the [Generate JWT section](https://www.adobe.io/apis/experienceplatform/console/docs.html#!AdobeDocs/adobeio-console/master/credentials.md) in the credentials guide.
2. Follow the instructions in [Step 3: try it](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/AuthenticationOverview/ServiceAccountIntegration.md) in the Service account connection guide.

You now have the required authentication headers `x-api-key: {API_KEY}`, `x-gw-ims-org-id: {IMS_ORG}`, and `Authorization: Bearer {ACCESS_TOKEN}`.

>[!NOTE]
>
>The access token has an expiration time of 24 hours, expressed in milliseconds, so you will have to refresh it. To refresh the access token, repeat the steps outlined in this section.

-->

## 宛先の所有権とサンドボックス {#destination-ownership}

Experience Platform のすべてのリソースは、特定の仮想サンドボックスに分離されています。Destination SDKへのリクエストには、操作がおこなわれるサンドボックスの名前を指定するヘッダーが必要です。

* `x-sandbox-name: {SANDBOX_NAME}`

Sandbox Exchange チームが提供するAdobe名は、Destination SDK API エンドポイントへの呼び出しで使用する必要があります。

## ロールベースのアクセス制御 (RBAC) {#rbac}

Destination SDK API エンドポイントを使用するには、 [リファレンスドキュメント](./configuration-options.md)、 **[!UICONTROL 宛先のオーサリング]** アクセス制御権限。 Exchange チームと連携して、Adobeに割り当てられた権限を取得する [Adobe Admin Console](https://adminconsole.adobe.com/).

![宛先オーサリング権限](./assets/destination-authoring-permission.png)

詳細については、次のアクセス制御Experience Platformを参照してください。

* [製品プロファイルに対する権限の管理](/help/access-control/ui/permissions.md)
* [Experience Platformに使用可能な権限](/help/access-control/home.md#permissions)
* [Adobe Admin Consoleドキュメント](https://helpx.adobe.com/jp/enterprise/using/admin-console.html)

## その他の考慮事項 {#additional-considerations}

* 宛先の設定に対して行った変更は、宛先の設定を作成または編集する場合でも、Adobeが確認して承認する必要があります。 変更は、レビューが完了した後でのみ、宛先に反映されます。
* 同じ組織に属し、サンドボックスにアクセスできるユーザーのみが、宛先設定を編集できます。

## 次の手順 {#next-steps}

この記事の手順に従って、Adobe I/Oに対する認証資格情報、サンドボックス名、宛先オーサリングのアクセス制御権限を取得しました。 次に、Destination SDKを使用して宛先を設定できます。
* 読み取り [Destination SDKを使用した宛先の設定](./configure-destination-instructions.md) を参照してください。
* すべての操作については、 [宛先オーサリング API ドキュメント](https://www.adobe.io/experience-platform-apis/references/destination-authoring/).
* 以下を使用： [宛先オーサリング API Postman コレクション](https://github.com/adobe/experience-platform-postman-samples/blob/master/apis/experience-platform/Destination%20Authoring%20API.postman_collection.json) Destination SDK API エンドポイントを使用して宛先を設定する場合。 Postman の使用を開始するには、 [環境とコレクションの読み込み手順](https://learning.postman.com/docs/getting-started/importing-and-exporting-data/) および [Postman 環境を作成するためのビデオガイド](https://video.tv.adobe.com/v/28832).
