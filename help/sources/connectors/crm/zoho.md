---
keywords: Experience Platform；ホーム；人気のあるトピック；Zoho CRM;zoho crm;Zoho;zoho
solution: Experience Platform
title: Zoho CRM ソースコネクタの概要
topic-legacy: overview
description: API またはユーザーインターフェイスを使用して Zoho CRM をAdobe Experience Platformに接続する方法を説明します。
source-git-commit: 030789af0a049b54d6e271410836c08456a83441
workflow-type: tm+mt
source-wordcount: '549'
ht-degree: 3%

---

# (ベータ) [!DNL Zoho CRM]

>[!NOTE]
>
>10. [!DNL Zoho CRM] ソースはベータ版です。 詳しくは、 [ソースの概要](../../home.md#terms-and-conditions) ベータラベルのコネクタの使用に関する詳細

Adobe Experience Platformを使用すると、データを外部ソースから取り込みながら、 [!DNL Platform] サービス アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取得することができます。

Experience Platformは、サードパーティの CRM システムからデータを取り込む機能を提供します。 CRM プロバイダーのサポートには以下が含まれます。 [!DNL Zoho CRM].

## IP アドレス許可リスト

ソースコネクタを操作する前に、IP アドレスのリストを許可リストに追加する必要があります。 地域固有の IP アドレスを許可リストに追加しないと、ソースを使用する際にエラーが発生したり、パフォーマンスが低下する可能性があります。 詳しくは、 [IP アドレス許可リスト](../../ip-address-allow-list.md) ページを参照してください。

## の認証資格情報の取得 [!DNL Zoho CRM]

データを [!DNL Zoho CRM] アカウントから Platform にアクセスする場合、まず資格情報を取得して、 [!DNL Zoho CRM] ソース。 次の手順に従って、クライアント ID、クライアントの秘密鍵、アクセストークン、更新トークンを取得します。

### アプリの登録

認証資格情報を取得する最初の手順は、 [[!DNL Zoho CRM] 開発者コンソール](https://accounts.zoho.com/). アプリケーションを登録するには、次の中からクライアントタイプを選択する必要があります。Java Script、Web ベース、モバイル、ブラウザー以外のモバイルアプリケーション、またはセルフクライアント。 次に、アプリケーションの名前、Web ページの URL、認証済みのリダイレクト URI の値を指定します。 [!DNL Zoho CRM] その後、を使用して、付与トークンでリダイレクトできます。

登録が成功すると、クライアント ID とクライアントの秘密鍵が返されます。

### 認証リクエストの作成

次に、 [認証要求](https://www.zoho.com/crm/developer/docs/api/v2/auth-request.html) web ベースのアプリケーションまたはセルフクライアントを使用する。 認証リクエストは付与トークンを返し、アクセストークンを取得できます。

承認リクエストを作成する場合、 **スコープ** および **アクセスタイプ**. 詳しくは、 [[!DNL Zoho CRM] 文書](https://www.zoho.com/crm/developer/docs/api/v2/scopes.html) スコープの詳細は、 **アクセスタイプ** は常に `offline`.

### アクセスおよび更新トークンの生成

付与トークンを取得したら、 [アクセスおよび更新トークン](https://www.zoho.com/crm/developer/docs/api/v2/access-refresh.html) ～にPOSTを要求して `{ACCOUNTS_URL}/oauth/v2/token` クライアント ID、クライアントの秘密鍵、付与トークン、リダイレクト URI を指定する際に使用します。 この手順の間、 `grant_type` をパラメーターとして指定し、値を `"authorization_code"`.

リクエストが成功すると、アクセストークンと更新トークンが返され、これを使用して認証できます。

資格情報の取得に関する詳細な手順については、 [[!DNL Zoho CRM] 認証ガイド](https://www.zoho.com/crm/developer/docs/api/v2/oauth-overview.html).

## 接続 [!DNL Zoho CRM] を [!DNL Platform] API の使用

以下のドキュメントでは、接続方法に関する情報を提供します [!DNL Zoho CRM] API またはユーザーインターフェイスを使用して Platform に送信するには：

- [の作成 [!DNL Zoho CRM] フローサービス API を使用したベース接続](../../tutorials/api/create/crm/zoho.md)
- [フローサービス API を使用した CRM ソースのデータ構造とコンテンツの調査](../../tutorials/api/explore/crm.md)
- [フローサービス API を使用した CRM ソースのデータフローの作成](../../tutorials/api/collect/crm.md)

## 接続 [!DNL Zoho CRM] を [!DNL Platform] UI の使用

- [の作成 [!DNL Zoho CRM] UI でのソース接続](../../tutorials/ui/create/crm/zoho.md)
- [UI での CRM ソース接続のデータフローの作成](../../tutorials/ui/dataflow/crm.md)
