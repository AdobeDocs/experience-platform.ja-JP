---
keywords: Experience Platform；ホーム；人気の高いトピック；onetrust;OneTrust
solution: Experience Platform
title: UI での OneTrust Source 接続の作成
type: Tutorial
description: Adobe Experience Platform UI を使用して OneTrust ソース接続を作成する方法を説明します。
exl-id: 6af0604d-cbb6-4c8e-b017-3eb82ec6ee1c
source-git-commit: 35095ec8c22106ba0a8f11e0a970ed7989a7f06c
workflow-type: tm+mt
source-wordcount: '528'
ht-degree: 25%

---

# UI での [!DNL OneTrust Integration] ソース接続の作成

>[!NOTE]
>
>この [!DNL OneTrust Integration] ソースは、同意および環境設定のデータの取り込みのみをサポートし、cookie はサポートしません。

このチュートリアルでは、 [[!DNL OneTrust Integration]](https://my.onetrust.com/s/contactsupport?language=en_US) Platform ユーザーインターフェイスを使用して、履歴データとスケジュールされた同意データの両方をAdobe Experience Platformに取り込むためのソース接続。

## 前提条件

>[!IMPORTANT]
>
>この [!DNL OneTrust Integration] ソースコネクタとドキュメントは [!DNL OneTrust Integration] チーム。 お問い合わせや更新のご依頼については、 [[!DNL OneTrust] チーム](https://my.onetrust.com/s/contactsupport?language=en_US) 直接

接続する前に [!DNL OneTrust Integration] Platform に接続する場合は、最初にアクセストークンを取得する必要があります。 アクセストークンの検索方法について詳しくは、 [[!DNL OneTrust Integration] OAuth 2 ガイド](https://developer.onetrust.com/docs/api-docs-v3/b3A6MjI4OTUyOTc-generate-access-token).

システム間の更新トークンはではサポートされていないので、有効期限が切れた後、アクセストークンは自動的に更新されません。 [!DNL OneTrust]. したがって、アクセストークンの有効期限が切れる前に、接続でアクセストークンを必ず更新しておく必要があります。 アクセストークンの設定可能な最大有効期間は 1 年です。 アクセストークンの更新について詳しくは、 [[!DNL OneTrust] OAuth 2.0 クライアント資格情報の管理に関するドキュメント](https://developer.onetrust.com/docs/documentation/ZG9jOjIyODk1MTUw-managing-o-auth-2-0-client-credentials).

### 必要な資格情報の収集

接続するには [!DNL OneTrust Integration] を Platform に設定するには、次の認証資格情報の値を指定する必要があります。

| 資格情報 | 説明 | 例 |
| --- | --- | --- |
| ホスト名 | 環境 [!DNL OneTrust Integration] からデータを取り込む必要があります。 | `app.onetrust.com` |
| 認証テスト URL | （オプション）認証テスト URL は、ベース接続の作成時に認証情報を検証するために使用されます。指定しない場合、代わりにソース接続の作成時に資格情報が自動的にチェックされます。 |  |
| アクセストークン | に対応するアクセストークン [!DNL OneTrust Integration] アカウント | `ZGFkZDMyMjFhMmEyNDQ2ZGFhNTdkZjNkZjFmM2IyOWE6QjlUSERVUTNjOFVsRmpEZTJ6Vk9oRnF3Sk8xNlNtcm4=` |

これらの資格情報について詳しくは、 [[!DNL OneTrust Integration] 認証ドキュメント](https://developer.onetrust.com/docs/api-docs-v3/b3A6MjI4OTUyOTc-generate-access-token).

## [!DNL OneTrust Integration] アカウントを接続

>[!NOTE]
>
>この [!DNL OneTrust Integration] API の仕様は、データ取り込み用にAdobeと共有されています。

Platform UI で、「 **[!UICONTROL ソース]** 左側のナビゲーションから [!UICONTROL ソース] workspace :Experience Platformで使用可能なソースのカタログ。

以下を使用： *[!UICONTROL カテゴリ]* メニューを使用して、ソースをカテゴリでフィルタリングできます。 または、検索バーにソース名を入力して、カタログから特定のソースを検索します。

次に移動： [!UICONTROL 同意と環境設定] カテゴリ [!DNL OneTrust Integration] ソースカード。 最初に、 **[!UICONTROL データを追加]**.

![Experience PlatformUI ソースカタログ。](../../../../images/tutorials/create/onetrust/catalog.png)

この **[!UICONTROL OneTrust 統合アカウントに接続]** ページが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

### 既存のアカウント

既存のアカウントを使用するには、新しいデータフローを作成する [!DNL OneTrust Integration] アカウントを選択し、「**[!UICONTROL 次へ]**」を選択して続行します。

![ソースワークフローの既存のアカウント認証手順。](../../../../images/tutorials/create/onetrust/existing.png)

### 新規アカウント

新しいアカウントを作成する場合は、「**[!UICONTROL 新規アカウント]**」を選択し、続けて名前、説明（オプション）、 の認証情報を指定します。終了したら「**[!UICONTROL ソースに接続]**」を選択し、新しい接続が確立されるまでしばらく待ちます。

![ソースワークフローの新しいアカウント認証手順。](../../../../images/tutorials/create/onetrust/new.png)

## 次の手順

このチュートリアルでは、[!DNL OneTrust Integration] アカウントとの接続を確立しました。次のチュートリアルに進み、 [データフローを設定して、同意データを Platform に取り込む](../../dataflow/consent-and-preferences.md).
