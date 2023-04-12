---
description: このページでは、Adobe Experience Platform Destination SDK を認証および使用開始する方法について説明します。 これには、Adobe I/O 認証資格情報、サンドボックス名および宛先オーサリングのアクセス制御権限を取得する方法についての説明も含まれています。
title: Destination SDK の概要
exl-id: f22c37a8-202d-49ac-9af0-545dfa9af8fd
source-git-commit: 81f48de908b274d836f551bec5693de13c5edaf1
workflow-type: tm+mt
source-wordcount: '610'
ht-degree: 95%

---

# はじめに

## 概要 {#overview}

このページでは、Adobe Experience Platform Destination SDK を認証および使用開始する方法について説明します。 これには、Adobe I/O 認証資格情報、サンドボックス名および宛先オーサリングのアクセス制御権限を取得する方法についての説明も含まれています。

## 用語 {#terminology}

このガイドでは、組織やサンドボックスなど、Platform 固有の概念を使用します。 これらを始めとする用語の定義については、 [Experience Platform 用語集](https://experienceleague.adobe.com/docs/experience-platform/landing/glossary.html?lang=ja)を参照してください。

## 必要な認証資格情報の取得 {#obtain-authentication-credentials}

Destination SDK では、認証に [Adobe I/O](https://www.adobe.io/) ゲートウェイを使用します。 Destination SDK エンドポイントへの API 呼び出しを行うには、API 呼び出しで特定のヘッダーを指定する必要があります。 Adobe Exchange チームと協力して、[Adobe デベロッパーコンソール](https://developer.adobe.com/console)への認証を設定してください。

Destination SDK API エンドポイントへの呼び出しを成功させるには、 [Experience Platform 認証のチュートリアル](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja)に従います。チュートリアルを「[API キー、組織 ID、クライアントの秘密鍵を生成する](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja#api-ims-secret)&quot;ステップ。 Adobe Exchange チームが、それ以前の手順を代行します。 認証チュートリアルを完了すると、以下に示すような Destination SDK API 呼び出しで必要な各ヘッダーの値が提供されます。

* `x-api-key: {API_KEY}`（クライアント ID とも呼ばれます）
* `x-gw-ims-org-id: {ORG_ID}`（組織 ID とも呼ばれます）
* `Authorization: Bearer {ACCESS_TOKEN}`。アクセストークンの有効期間は 24 時間（ミリ秒単位で表現）なので、更新する必要があります。 アクセストークンを更新するには、認証に関するチュートリアルで説明されている手順を繰り返します。

<!--

### Obtain `Authorization: Bearer {ACCESS_TOKEN}`

To obtain the `{ACCESS_TOKEN}`, you must generate a JWT token and exchange it for the access token. Follow the steps below:

1. Follow the instructions in the [Generate JWT section](https://www.adobe.io/apis/experienceplatform/console/docs.html#!AdobeDocs/adobeio-console/master/credentials.md) in the credentials guide.
2. Follow the instructions in [Step 3: try it](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/AuthenticationOverview/ServiceAccountIntegration.md) in the Service account connection guide.

You now have the required authentication headers `x-api-key: {API_KEY}`, `x-gw-ims-org-id: {ORG_ID}`, and `Authorization: Bearer {ACCESS_TOKEN}`.

>[!NOTE]
>
>The access token has an expiration time of 24 hours, expressed in milliseconds, so you will have to refresh it. To refresh the access token, repeat the steps outlined in this section.

-->

## 宛先の所有権とサンドボックス {#destination-ownership}

Experience Platform のすべてのリソースは、特定の仮想サンドボックスに分離されています。Destination SDK へのリクエストには、操作が行われるサンドボックスの名前を指定するヘッダーが必要です。

* `x-sandbox-name: {SANDBOX_NAME}`

Adobe Exchange チームが提供するサンドボックス名は、Destination SDK API エンドポイントへの呼び出しで使用する必要があります。

## 役割ベースのアクセス制御（RBAC） {#rbac}

[リファレンスドキュメント](./configuration-options.md)に記載されている Destination SDK API エンドポイントを使用するには、**[!UICONTROL 宛先オーサリング]**&#x200B;のアクセス制御権限が必要です。 Adobe Exchange チームに依頼して、[Adobe Admin Console](https://adminconsole.adobe.com/) でこの権限を割り当ててもらいます。

![宛先オーサリング権限](./assets/destination-authoring-permission.png)

詳しくは、次の Experience Platform アクセス制御ドキュメントを参照してください。

* [製品プロファイルに対する権限の管理](/help/access-control/ui/permissions.md)
* [Experience Platform で使用可能な権限](/help/access-control/home.md#permissions)
* [Adobe Admin Console ドキュメント](https://helpx.adobe.com/jp/enterprise/using/admin-console.html)

## その他の留意事項 {#additional-considerations}

* 宛先設定を作成する場合も編集する場合も、宛先設定に加える変更はすべて、アドビによる審査と承認が必要です。変更内容は、審査が完了した後にのみ、宛先に反映されます。
* 宛先設定を編集できるのは、同じ組織に所属し、サンドボックスにアクセスできるユーザーのみです。

## 次の手順 {#next-steps}

この記事では手順に従って、Adobe I/Oに対する認証資格情報、サンドボックス名、宛先オーサリングのアクセス制御権限を取得しました。 次は、Destination SDK を使用して宛先を設定します。

* 宛先のタイプに応じて、以下の設定ガイドをお読みください。

   * [Destination SDK を使用したストリーミングの宛先の設定](./configure-destination-instructions.md)
   * [Destination SDK を使用したファイルベースの宛先の設定](./configure-file-based-destination-instructions.md)

* すべての操作については、 [宛先オーサリング API ドキュメント](https://www.adobe.io/experience-platform-apis/references/destination-authoring/)を参照してください。
* Destination SDK API エンドポイントを使用して使用して宛名を設定するには、 [宛先オーサリング API Postman コレクション](https://github.com/adobe/experience-platform-postman-samples/blob/master/apis/experience-platform/Destination%20Authoring%20API.postman_collection.json)を使用します。Postman の使用を開始するには、 [環境とコレクションのインポート手順](https://learning.postman.com/docs/getting-started/importing-and-exporting-data/)および [Postman 環境を作成するためのビデオガイド](https://video.tv.adobe.com/v/28832)を参照してください。
