---
keywords: Experience Platform；ホーム；人気のトピック；onetrust;OneTrust
solution: Experience Platform
title: UI での OneTrust Source接続の作成
type: Tutorial
description: Adobe Experience Platform UI を使用して OneTrust ソース接続を作成する方法を説明します。
exl-id: 6af0604d-cbb6-4c8e-b017-3eb82ec6ee1c
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '502'
ht-degree: 23%

---

# UI での [!DNL OneTrust Integration] ソース接続の作成

>[!NOTE]
>
>[!DNL OneTrust Integration] ソースは、同意および環境設定データの取り込みのみをサポートし、cookie はサポートしていません。

このチュートリアルでは、Experience Platform ユーザーインターフェイスを使用して [[!DNL OneTrust Integration]](https://my.onetrust.com/s/contactsupport?language=en_US) ソース接続を作成し、履歴およびスケジュールされた同意データの両方をAdobe Experience Platformに取り込む手順について説明します。

## 前提条件

>[!IMPORTANT]
>
>[!DNL OneTrust Integration] ソースコネクタとドキュメントは、[!DNL OneTrust Integration] チームによって作成されました。 お問い合わせや更新のリクエストについては、[[!DNL OneTrust]  チーム ](https://my.onetrust.com/s/contactsupport?language=en_US) に直接お問い合わせください。

[!DNL OneTrust Integration] をExperience Platformに接続する前に、まずアクセストークンを取得する必要があります。 アクセストークンを見つける手順について詳しくは、[[!DNL OneTrust Integration] OAuth 2 ガイド ](https://developer.onetrust.com/docs/api-docs-v3/b3A6MjI4OTUyOTc-generate-access-token) を参照してください。

システム間の更新トークンは [!DNL OneTrust] でサポートされていないため、有効期限が切れた後は、アクセストークンは自動的に更新されません。 そのため、有効期限が切れる前に、接続でアクセストークンが更新されていることを確認する必要があります。 アクセストークンの設定可能な最長有効期間は 1 年です。 アクセストークンの更新について詳しくは、[[!DNL OneTrust] OAuth 2.0 クライアント資格情報の管理のドキュメント ](https://developer.onetrust.com/docs/documentation/ZG9jOjIyODk1MTUw-managing-o-auth-2-0-client-credentials) を参照してください。

### 必要な資格情報の収集

[!DNL OneTrust Integration] をExperience Platformに接続するには、次の認証資格情報の値を指定する必要があります。

| 資格情報 | 説明 | 例 |
| --- | --- | --- |
| ホスト名 | [!DNL OneTrust Integration] データの取り出し元となる環境。 | `app.onetrust.com` |
| 認証テスト URL | （オプション）認証テスト URL は、ベース接続の作成時に資格情報を検証するために使用されます。指定しない場合、代わりにソース接続の作成時に資格情報が自動的にチェックされます。 | |
| アクセストークン | [!DNL OneTrust Integration] アカウントに対応するアクセストークン。 | `ZGFkZDMyMjFhMmEyNDQ2ZGFhNTdkZjNkZjFmM2IyOWE6QjlUSERVUTNjOFVsRmpEZTJ6Vk9oRnF3Sk8xNlNtcm4=` |

これらの資格情報について詳しくは、[[!DNL OneTrust Integration]  認証ドキュメント ](https://developer.onetrust.com/docs/api-docs-v3/b3A6MjI4OTUyOTc-generate-access-token) を参照してください。

## [!DNL OneTrust Integration] アカウントを接続

>[!NOTE]
>
>[!DNL OneTrust Integration] API の仕様は、データ取得用にAdobeと共有されています。

Experience Platform UI の左側のナビゲーションから「**[!UICONTROL ソース]**」を選択し、Experience Platformで使用可能なソースのカタログの [!UICONTROL  ソース ] ワークスペースにアクセスします。

*[!UICONTROL カテゴリ]* メニューを使用して、カテゴリ別にソースをフィルタリングします。 または、検索バーにソース名を入力して、カタログから特定のソースを検索します。

[!DNL OneTrust Integration] ソースカードの [!UICONTROL  同意および環境設定 ] カテゴリに移動します。 開始するには、「**[!UICONTROL データを追加]**」を選択します。

![Experience Platform UI ソースカタログ ](../../../../images/tutorials/create/onetrust/catalog.png)

**[!UICONTROL OneTrust 統合アカウントの接続]** ページが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

### 既存のアカウント

既存のアカウントを使用するには、新しいデータフローを作成する [!DNL OneTrust Integration] アカウントを選択し、「**[!UICONTROL 次へ]**」を選択して続行します。

![ ソースワークフローの既存のアカウント認証手順。](../../../../images/tutorials/create/onetrust/existing.png)

### 新規アカウント

新しいアカウントを作成する場合は、「**[!UICONTROL 新しいアカウント]**」を選択し、続けて名前、説明（オプション）、の認証情報を指定します。 終了したら「**[!UICONTROL ソースに接続]**」を選択し、新しい接続が確立されるまでしばらく待ちます。

![ ソースワークフローの新しいアカウント認証手順。](../../../../images/tutorials/create/onetrust/new.png)

## 次の手順

このチュートリアルでは、[!DNL OneTrust Integration] アカウントとの接続を確立しました。次のチュートリアルに進み、[ 同意データをExperience Platformに取り込むためのデータフローの設定 ](../../dataflow/consent-and-preferences.md) を行いましょう。
