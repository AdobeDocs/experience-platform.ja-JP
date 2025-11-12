---
title: Zendesk Source コネクタの概要
description: API またはユーザーインターフェイスを使用して Zendesk をAdobe Experience Platformに接続する方法について説明します。
last-substantial-update: 2023-06-21T00:00:00Z
exl-id: 9f245783-949d-4f40-9cf3-8991b4b6d780
source-git-commit: 06b2108715ce368ff4ecf5c6c7dd3a327d9f61b1
workflow-type: tm+mt
source-wordcount: '330'
ht-degree: 22%

---

# [!DNL Zendesk]

Adobe Experience Platform を使用すると、データを外部ソースから取得しながら、Experience Platform サービスを使用して、受信データの構造化、ラベル付け、拡張を行うことができます。アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取り込むことができます。

Experience Platform は、サードパーティの顧客成功アプリケーションからデータを取得する機能を備えています。カスタマーサクセスプロバイダーのサポートには、[!DNL Zendesk] が含まれます。

このAdobe Experience Platform[ ソース ](https://experienceleague.adobe.com/docs/experience-platform/sources/home.html?lang=ja) は、[Zendesk Search API/検索結果を書き出し ](https://developer.zendesk.com/api-reference/ticketing/ticket-management/search/#export-search-results) を利用して、ユーザー情報を Zendesk からExperience Platformに返し、さらに処理できるようにします。

## IP アドレスの許可リスト

ソースをExperience Platformに接続する前に、地域固有の IP アドレスを許可リストに追加する必要があります。 詳しくは、[Experience Platformへの接続に対する IP アドレスの許可リストに加える](../../ip-address-allow-list.md) に関するガイドを参照してください。

## [!DNL Zendesk] アカウントの認証

[!DNL Zendesk] は、認証メカニズムとして bearer トークンを使用して、[!DNL Zendesk] API と通信します。

この節では、[!DNL Zendesk] アカウントを認証するために完了すべき前提条件の手順について説明します。

* [!DNL Zendesk] アカウントを認証する最初の手順は、サポートアカウントを確実に持つこ [!DNL Zendesk] です。 [[!DNL Zendesk]  登録ページ ](https://www.zendesk.co.jp/register/) が表示されていない場合は、Zendesk アカウントを登録して作成します。
* 正常に登録したら、[[!DNL Zendesk] web サイト ](https://www.zendesk.com/login/) に移動し、**サブドメイン** を指定します。
* 次に、**[!DNL Settings]**/**[!DNL Apps and Integrations]**/**[!DNL Zendesk API]** を選択します。
* 最後に、**[!DNL API token]** セクションから API トークンを取得します。

![Zendesk API トークン ](../../images/tutorials/create/zendesk/zendesk-api-tokens.png)

サブドメインの取得方法については、[[!DNL Zendesk documentation on subdomains]](<https://support.zendesk.com/hc/en-us/articles/4409381383578-Where-can-I-find-my-Zendesk-subdomain->) を参照してください。 API トークンの生成について詳しくは、[[!DNL Zendesk]  新しい API トークンの生成に関するガイド ](<https://support.zendesk.com/hc/en-us/articles/4408889192858-Generating-a-new-API-token>) を参照してください。

以下のドキュメントでは、API やユーザーインターフェイスを使用して [!DNL Zendesk] をExperience Platformに接続する方法について説明しています。

## API を使用した [!DNL Zendesk] のExperience Platformへの接続

* [Flow Service API を使用したソース接続とデータフロ  [!DNL Zendesk]  の作成](../../tutorials/api/create/customer-success/zendesk.md)

## UI を使用した [!DNL Zendesk] のExperience Platformへの接続

* [UI での  [!DNL Zendesk ] ソース接続の作成](../../tutorials/ui/create/customer-success/zendesk.md)
* [UI でのカスタマーサクセスソース接続のデータフローの作成](../../tutorials/ui/dataflow/customer-success.md)
