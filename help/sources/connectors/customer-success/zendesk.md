---
title: Zendesk Source コネクタの概要
description: API またはユーザーインターフェイスを使用して Zendesk をAdobe Experience Platformに接続する方法を説明します。
last-substantial-update: 2023-06-21T00:00:00Z
exl-id: 9f245783-949d-4f40-9cf3-8991b4b6d780
source-git-commit: e300e57df998836a8c388511b446e90499185705
workflow-type: tm+mt
source-wordcount: '365'
ht-degree: 47%

---

# [!DNL Zendesk]

Adobe Experience Platform を使用すると、外部ソースからデータを取り込みながら、Platform サービスを使用して受信データの構造化、ラベル付けおよび拡張を行うことができます。アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取り込むことができます。

Experience Platform は、サードパーティの顧客成功アプリケーションからデータを取得する機能を備えています。顧客成功プロバイダーのサポートには以下が含まれます。 [!DNL Zendesk].

このAdobe Experience Platform [ソース](https://experienceleague.adobe.com/docs/experience-platform/sources/home.html?lang=ja) は、 [Zendesk Search API > 検索結果の書き出し](https://developer.zendesk.com/api-reference/ticketing/ticket-management/search/#export-search-results) これにより、Zendesk からExperience Platformにユーザー情報が返され、さらに処理が行われます。

## IP アドレス許可リスト

ソースコネクタを操作する前に、IP アドレスのリストを許可リストに追加する必要があります。 地域固有の IP アドレスを許可リストに追加しないと、ソースを使用する際にエラーが発生したり、パフォーマンスが低下する場合があります。 詳しくは、[IP アドレスの許可リスト](../../ip-address-allow-list.md)ページを参照してください。

## の認証 [!DNL Zendesk] アカウント

[!DNL Zendesk] は、 API と通信するための認証メカニズムとして Bearer トークンを使用します。 [!DNL Zendesk]

この節では、を認証するために完了する必要がある前提条件の手順について説明します。 [!DNL Zendesk] アカウント。

* の認証の最初の手順 [!DNL Zendesk] アカウントは、以下を確実にお持ちであることを確認するために使用されます。 [!DNL Zendesk] サポートアカウント。 まだ [[!DNL Zendesk] 登録ページ](https://www.zendesk.co.jp/register/) 登録して Zendesk アカウントを作成します。
* 登録が完了したら、 [[!DNL Zendesk] web サイト](https://www.zendesk.com/login/) を選択し、 **サブドメイン**.
* 次に、「 **[!DNL Settings]** > **[!DNL Apps and Integrations]** > **[!DNL Zendesk API]**.
* 最後に、 **[!DNL API token]** 」セクションに入力します。

![Zendesk API トークン](../../images/tutorials/create/zendesk/zendesk-api-tokens.png)

詳しくは、 [[!DNL Zendesk documentation on subdomains]](<https://support.zendesk.com/hc/en-us/articles/4409381383578-Where-can-I-find-my-Zendesk-subdomain->) サブドメインの取得方法に関する情報を参照してください。 API トークンの生成について詳しくは、 [[!DNL Zendesk] 新しい API トークンの生成に関するガイド](<https://support.zendesk.com/hc/en-us/articles/4408889192858-Generating-a-new-API-token>).

以下のドキュメントでは、API やユーザーインターフェイスを使用して [!DNL Zendesk] と Platform を接続する方法について説明します。

## API を使用して [!DNL Zendesk] と Platform を接続する

* [のソース接続とデータフローの作成 [!DNL Zendesk] フローサービス API の使用](../../tutorials/api/create/customer-success/zendesk.md)

## UI を使用した [!DNL Zendesk] の Platform への接続

* [UI での  [!DNL Zendesk ] ソース接続の作成](../../tutorials/ui/create/customer-success/zendesk.md)
* [UI での顧客成功ソース接続のデータフローの作成](../../tutorials/ui/dataflow/customer-success.md)
