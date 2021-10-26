---
keywords: エクスペリエンス Platform、home、人気のある話題。Zoho CRM; Zoho crm;Zoho;zoho
solution: Experience Platform
title: Zoho CRM ソースコネクタの概要
topic-legacy: overview
description: Api を使用して、またはユーザーインターフェイスを使用して、Zoho CRM を Adobe エクスペリエンスプラットフォームに接続する方法について説明します。
exl-id: fcd7af48-e66a-4313-bbfe-73301d335c67
source-git-commit: 7a15090d8ed2c1016d7dc4d7d3d0656640c4785c
workflow-type: tm+mt
source-wordcount: '549'
ht-degree: 3%

---

# (ベータ) [!DNL Zoho CRM]

>[!NOTE]
>
>[!DNL Zoho CRM]ソースはベータ版です。[ ](../../home.md#terms-and-conditions) ベータ版のコネクタの使用について詳しくは、ソースの概要を参照してください。

Adobe エクスペリエンスプラットフォームでは、ingested を使用して、外部ソースからデータを取得することができます。また、サービスを使用した受信データを構造化、ラベル付け、拡張する機能が提供され [!DNL Platform] ます。 アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取得することができます。

エクスペリエンスプラットフォームは、サードパーティの CRM システムからの ingesting データのサポートを提供します。 CRM プロバイダーのサポートが含ま [!DNL Zoho CRM] れています。

## IP アドレス許可リスト

ソースコネクタを使用する前に、許可リストに IP アドレスのリストが追加されている必要があります。 使用している地域に固有の IP アドレスを許可リストに追加していないと、ソースを使用しているときにエラーが発生したり、パフォーマンスが低下することがあります。 [詳細については、IP アドレス許可リストページを参照してください ](../../ip-address-allow-list.md) 。

## の認証資格情報を取得します。 [!DNL Zoho CRM]

アカウントからプラットフォームにデータを取り込む前に [!DNL Zoho CRM] 、ソースを認証するために、まず資格情報を取得する必要があり [!DNL Zoho CRM] ます。 クライアント ID、クライアントシークレット、アクセストークン、および更新トークンを取得するには、次の手順を実行します。

### アプリケーションの登録

認証資格情報を取得する最初の手順は、developer console を使用してアプリケーションを登録することです [[!DNL Zoho CRM]  ](https://accounts.zoho.com/) 。 アプリケーションを登録するには、次のいずれかのクライアントタイプを選択する必要があります。 Java Script、web ベース、mobile、非ブラウザーモバイルアプリケーション、または自己クライアントです。 次に、アプリケーションの名前に値、web ページの URL、および許可されたリダイレクト URI を指定し [!DNL Zoho CRM] ます。これを使用して、グラントトークンを使ったリダイレクトを行うことができます。

登録が成功した場合、クライアント ID とクライアントシークレットが返されます。

### 認証要求の作成

次に、 [ ](https://www.zoho.com/crm/developer/docs/api/v2/auth-request.html) web ベースのアプリケーションまたはセルフクライアントを使用して認証要求を作成する必要があります。 承認要求によって許可トークンが返されます。これにより、アクセストークンを取得することができます。

認証要求を作成する場合は、 **スコープとアクセスタイプの両方の値を入力する必要があり** **** ます。 スコープについて詳しくは、このドキュメントを参照してください [[!DNL Zoho CRM]  ](https://www.zoho.com/crm/developer/docs/api/v2/scopes.html) **。アクセスタイプは、** 常にに設定されている必要があり `offline` ます。

### アクセストークンと更新トークンの生成

許可トークンを取得したら、 [ ](https://www.zoho.com/crm/developer/docs/api/v2/access-refresh.html) `{ACCOUNTS_URL}/oauth/v2/token` クライアント ID、クライアントシークレット、許可トークン、およびリダイレクト URI を提供するために POST 要求を行うことによって、アクセスと更新トークンを生成できます。 この手順では、パラメーターとしてを指定し、その値をに設定する必要もあり `grant_type` `"authorization_code"` ます。

要求を完了すると、アクセスと更新トークンが返されます。これにより認証に使用することができます。

資格情報を取得するための詳細な手順については、認証ガイドを参照してください [[!DNL Zoho CRM]  ](https://www.zoho.com/crm/developer/docs/api/v2/oauth-overview.html) 。

## [!DNL Zoho CRM]Api を [!DNL Platform] 使用したへの接続

以下は、 [!DNL Zoho CRM] api またはユーザーインターフェイスを使用してプラットフォームに接続する方法を示しています。

- [ [!DNL Zoho CRM] フローサービス API を使用したベース接続の作成](../../tutorials/api/create/crm/zoho.md)
- [フローサービス API を使用して、CRM ソースのデータ構造とコンテンツを表示します。](../../tutorials/api/explore/crm.md)
- [フローサービス API を使用した CRM ソース用の流れ図の作成](../../tutorials/api/collect/crm.md)

## [!DNL Zoho CRM]UI を使用してに接続し [!DNL Platform] ます。

- [ [!DNL Zoho CRM] UI でのソース接続の作成](../../tutorials/ui/create/crm/zoho.md)
- [UI での CRM ソース接続用の流れ図の作成](../../tutorials/ui/dataflow/crm.md)
