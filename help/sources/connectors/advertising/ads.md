---
title: Google Ads Sourceの概要
description: API またはユーザーインターフェイスを使用してGoogle Ads をAdobe Experience Platformに接続する方法について説明します。
exl-id: 1f6257e0-213c-4723-a240-511c11c5833c
source-git-commit: ac90eea69f493bf944a8f9920426a48d62faaa6c
workflow-type: tm+mt
source-wordcount: '562'
ht-degree: 22%

---

# [!DNL Google Ads] ソース

>[!NOTE]
>
>[!DNL Google Ads] ソースはベータ版です。ベータ版のコネクタの使用に関して詳しくは、[ ソースの概要 ](../../home.md#terms-and-conditions) を参照してください。

Adobe Experience Platformを使用すると、データを外部ソースから取得しながら、Experience Platform サービスを使用して、受信データの構造化、ラベル付け、拡張を行うことができます。 アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取り込むことができます。

Adobe Experience Platform には、サードパーティの広告システムからデータを取り込む機能が用意されています。広告プロバイダーのサポートには、[!DNL Google Ads] が含まれます。

## 前提条件 {#prerequisites}

### IP アドレス許可リスト

ソースコネクタを操作する前に、IP アドレスのリストを許可リストに追加する必要があります。 地域固有の IP アドレスを許可リストに追加しないと、ソースを使用する際にエラーが発生したり、パフォーマンスが低下する場合があります。 詳しくは、[IP アドレスの許可リスト](../../ip-address-allow-list.md)ページを参照してください。

### Experience Platformに対する権限の設定

[!DNL Google Ads] アカウントをExperience Platformに接続するには、アカウントで **[!UICONTROL ソースの表示]** および **[!UICONTROL ソースの管理]** 権限の両方が有効になっている必要があります。 必要な権限を取得するには、製品管理者にお問い合わせください。 詳しくは、[ アクセス制御 UI ガイド ](../../../access-control/ui/overview.md) を参照してください。

### 必要な資格情報の収集

[!DNL Google Ads] アカウントをExperience Platformに正常に接続するには、次の資格情報に適切な値を指定する必要があります。

| 資格情報 | 説明 |
| --- | --- |
| `clientCustomerId` | クライアントカスタマー ID は、[!DNL Google Ads] API で管理する [!DNL Google Ads] クライアントアカウントに対応するアカウント番号です。 この ID は `123-456-7890` のテンプレートに従います。 |
| `loginCustomerId` | ログインカスタマー ID は、[!DNL Google Ads] manager アカウントに対応するアカウント番号であり、特定の運用顧客からレポートデータを取得するために使用されます。 ログインカスタマー ID について詳しくは、[[!DNL Google Ads] API ドキュメント ](https://developers.google.com/search-ads/reporting/concepts/login-customer-id) を参照してください。 |
| `developerToken` | 開発者トークンを使用すると、[!DNL Google Ads] API にアクセスできます。 同じ開発者トークンを使用して、すべての [!DNL Google Ads] アカウントに対してリクエストを行うことができます。 [ マネージャーアカウントにログイン ](https://ads.google.com/home/tools/manager-accounts/) してデベロッパートークンを取得し、[!DNL API Center] のページに移動します。 |
| `refreshToken` | 更新トークンは認証の一部 [!DNL OAuth2] す。 このトークンを使用すると、有効期限が切れた後にアクセストークンを再生成できます。 |
| `clientId` | クライアント ID は、クライアント秘密鍵と並行して、認証の一部として使用 [!DNL OAuth2] れます。 クライアント ID とクライアント秘密鍵を組み合わせると、[!DNL Google] ーザー先のアプリケーションを識別することにより、お客様のアカウントに代わってアプリケーションを動作させることができます。 |
| `clientSecret` | クライアントの秘密鍵は、クライアント ID と並行して、認証の一部として使用 [!DNL OAuth2] れます。 クライアント ID とクライアント秘密鍵を組み合わせると、[!DNL Google] ーザー先のアプリケーションを識別することにより、お客様のアカウントに代わってアプリケーションを動作させることができます。 |
| `googleAdsApiVersion` | [!DNL Google Ads] でサポートされている現在の API バージョン。 最新バージョンは `v18` ですが、Experience Platformでサポートされている最新バージョンは `v17` です。 |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様などの、ソースのコネクタプロパティを返します。[!DNL Google Ads] の接続仕様 ID は `d771e9c1-4f26-40dc-8617-ce58c4b53702` です。 [!DNL Flow Service] API を使用して [!DNL Google Ads] アカウントに接続する場合、この値は必須です。 |

## [!DNL Google Ads] をExperience Platformに接続

以下のドキュメントでは、API やユーザーインターフェイスを使用して [!DNL Google Ads] をExperience Platformに接続する方法について説明しています。

### API の使用

* [Flow Service API を使用したGoogle Ads ベース接続の作成](../../tutorials/api/create/advertising/ads.md)
* [Flow Service API を使用したデータテーブルの探索](../../tutorials/api/explore/tabular.md)
* [Flow Service API を使用した広告ソースのデータフローの作成](../../tutorials/api/collect/advertising.md)

### UI の使用

* [UI でのGoogle Ads ソースコネクタの作成](../../tutorials/ui/create/advertising/ads.md)
* [UI での広告ソース接続のデータフローの作成](../../tutorials/ui/dataflow/advertising.md)
