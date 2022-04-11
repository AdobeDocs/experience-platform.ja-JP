---
keywords: Experience Platform;ホーム;人気のトピック;Zoho CRM;Zoho crm;Zoho;zoho
solution: Experience Platform
title: Zoho CRM ソースコネクタの概要
topic-legacy: overview
description: API またはユーザーインターフェイスを使用して Zoho CRM を Adobe Experience Platform に接続する方法を説明します。
exl-id: 4a010453-3d09-4a47-b04e-5789ae4af48c
source-git-commit: 46b2fd6bc715bf1d8ccfeed576a2a2d193f92edd
workflow-type: ht
source-wordcount: '532'
ht-degree: 100%

---

# [!DNL Zoho CRM]

Adobe Experience Platform では、外部ソースからデータを取り込むと同時に、[!DNL Platform] サービスを使用して受信データの構造化、ラベル付け、および拡張を行うことができます。アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取り込むことができます。

Experience Platform は、サードパーティの CRM システムからのデータ取り込みをサポートしています。CRM プロバイダーのサポートは [!DNL Zoho CRM] を含みます。

## IP アドレス許可リスト

ソースコネクタを操作する前に、IP アドレスのリストを許可リストに追加する必要があります。 地域固有の IP アドレスを許可リストに追加しないと、ソースを使用する際にエラーが発生したり、パフォーマンスが低下する場合があります。 詳しくは、[IP アドレスの許可リスト](../../ip-address-allow-list.md)のページを参照してください。

## [!DNL Zoho CRM] の認証情報を取得します。

データを [!DNL Zoho CRM] アカウントを Platform にを持ち込む前に、まず [!DNL Zoho CRM] ソースを認証するための認証情報を取得する必要があります。次の手順に従って、クライアント ID、クライアントの秘密鍵、アクセストークン、更新トークンを取得します。

### アプリケーションを登録

認証資格情報を取得する最初の手順は、[[!DNL Zoho CRM] 開発者コンソール](https://accounts.zoho.com/)を使用してアプリケーションを登録することです。アプリケーションを登録するには、次の中からクライアントの種類を選択する必要があります。Java Script、Web ベース、モバイル、ブラウザー以外のモバイルアプリケーション、またはセルフクライアント。 次に、アプリケーションの名前、web ページの URL、および認証済みリダイレクト URI （[!DNL Zoho CRM] が付与トークンを使用してユーザーをリダイレクトする際に使用できる）の値を指定します。

登録が成功すると、クライアント ID とクライアント秘密鍵が返されます。

### 認証リクエストの作成

次に、webベースのアプリケーションまたはセルフクライアントを使用して、[認証リクエスト](https://www.zoho.com/crm/developer/docs/api/v2/auth-request.html)を作成する必要があります。認証リクエストは付与トークンを返し、その結果、アクセストークンを取得できます。

認証リクエストを作成する際は、**スコープ**&#x200B;および&#x200B;**アクセスタイプ**&#x200B;の両方の値を記入する必要があります。スコープに関する詳細はこちらの [[!DNL Zoho CRM]  ドキュメント](https://www.zoho.com/crm/developer/docs/api/v2/scopes.html)を参照してください。一方、**アクセスタイプ**&#x200B;は常に `offline` に設定する必要があります。

### アクセスおよび更新トークンの生成

付与トークンを取得したら、クライアント ID、クライアント秘密鍵、付与トークン、リダイレクト URI を指定すると同時に、`{ACCOUNTS_URL}/oauth/v2/token` へ POST リクエストを行うことで[アクセスと更新トークン](https://www.zoho.com/crm/developer/docs/api/v2/access-refresh.html)を生成できます。この手順では、パラメーターとして `grant_type` を含め、その値を `"authorization_code"` に設定する必要があります。

リクエストが成功すると、アクセストークンと更新トークンが返され、これを使用して認証することができます。

認証情報の取得に関する詳細な手順については、[[!DNL Zoho CRM] 認証ガイド](https://www.zoho.com/crm/developer/docs/api/v2/oauth-overview.html)を参照してください。

## API を使用して [!DNL Zoho CRM] と [!DNL Platform] を接続する

以下のドキュメントでは、API やユーザーインターフェイスを使用して [!DNL Zoho CRM] と Platform を接続する方法について説明しています。

- [Flow Service API を使用した [!DNL Zoho CRM] ベース接続の作成](../../tutorials/api/create/crm/zoho.md)
- [フローサービス API を使用して CRM ソースのデータ構造と内容を調べる](../../tutorials/api/explore/crm.md)
- [Flow Service API を使用して、CRM ソースのデータフローを作成する](../../tutorials/api/collect/crm.md)

## UIを使用して [!DNL Zoho CRM] と [!DNL Platform] を接続する

- [UI での [!DNL Zoho CRM] ソースコネクタの作成](../../tutorials/ui/create/crm/zoho.md)
- [UI での CRM ソース接続のデータフローの作成](../../tutorials/ui/dataflow/crm.md)
