---
title: PathFactory ソースの概要
description: API またはユーザーインターフェイスを使用して PathFactory をAdobe Experience Platformに接続する方法について説明します。
last-substantial-update: 2024-04-30T00:00:00Z
hide: true
hidefromtoc: true
badge: ベータ版
source-git-commit: 18f6c253aec6815cf84272cbce340a9aa7ed8ab9
workflow-type: tm+mt
source-wordcount: '464'
ht-degree: 16%

---

# [!DNL PathFactory]

>[!NOTE]
>
>[!DNL PathFactory] ソースはベータ版です。読んでください。 [ソースの概要](../../home.md#terms-and-conditions) ベータラベル付きソースの使用の詳細については、を参照してください。

[[!DNL PathFactory]](https://www.pathfactory.com/) は、企業がコンテンツジャーニーを管理し、インテリジェントなコンテンツインサイトを通じてエンゲージメントを促進するのに役立つ、クラウドベースのプラットフォームを提供します。 このガイドでは、PathFactory のコネクタを使用して、PathFactory からExperience Platformにデータを統合して最適なデータ取得を行う方法について詳しく説明します。

からデータを取り込むことができます [[!DNL PathFactory]](https://www.pathfactory.com/) 3 つの主要なソースを使用：

* **[!DNL Visitors]**：顧客データと連絡先データをレコードとして取り込み、オーディエンスをより深く理解します。
* **[!DNL Sessions]**：プラットフォーム上の個々のユーザーセッションアクティビティを追跡する時系列イベント。
* **[!DNL Page Views]**：表示されているページに関するインサイトを提供し、コンテンツのパフォーマンスの分析に役立つ時系列イベント。

の設定方法については、以下のドキュメントを参照してください [!DNL PathFactory] ソースアカウント。

## IP アドレス許可リスト {#ip-allow-list}

ソースコネクタを操作する前に、IP アドレスのリストを許可リストに追加する必要がある場合があります。 地域固有の IP アドレスを許可リストに追加しないと、ソースを使用する際にエラーが発生したり、パフォーマンスが低下する場合があります。 詳しくは、[IP アドレスの許可リスト](../../ip-address-allow-list.md)ページを参照してください。

## 前提条件 {#prerequisites}

統合を開始する前に [[!DNL PathFactory]](https://www.pathfactory.com/) Experience Platformを備えたコネクタは、次の前提条件を満たしていることを確認します。

* **A [PathFactory アカウント]**.
   * 連絡先 [[!DNL PathFactory]](https://www.pathfactory.com/portal/company/contactus.shtml) 有効なアカウントをまだ持っていない場合は。
* **アクティブな購読** 任意の [!DNL PathFactory] 製品。
* **ユーザー名、パスワード、ドメイン**.
   * これらの資格情報は、にアクセスするために必要です。 [!DNL PathFactory] アカウントとそのデータ。
* **アクセストークン** および **API エンドポイント**.
   * これらは、に接続するために必要です [!DNL PathFactory] API です。

### 資格情報とアクセストークンの取得方法 {#gather-credentials}

接続する [!DNL PathFactory] Experience Platformするには、次の資格情報を指定する必要があります。

| 資格情報 | 説明 | エンドポイント |
| --- | --- | --- |
| ユーザー名 | あなたの [!DNL PathFactory] アカウントのユーザー名。 | 適用なし |
| パスワード | あなたの [!DNL PathFactory] アカウントのパスワード。 | 適用なし |
| ドメイン | に関連付けられたドメイン [!DNL PathFactory] アカウント。 | 適用なし |
| アクセストークン | API 認証に使用される一意のトークン。 | 適用なし |
| 訪問者エンドポイント | 訪問者データの API エンドポイント。 | `/api/public/v3/data_lake_apis/visitors.json` |
| セッションエンドポイント | セッションデータの API エンドポイント。 | `/api/public/v3/data_lake_apis/sessions.json` |
| ページビューエンドポイント | ページビューデータの API エンドポイント。 | `/api/public/v3/data_lake_apis/page_views.json` |

ユーザー名、パスワード、ドメインおよびアクセストークンの取得方法について詳しくは、を参照してください [[!DNL PathFactory] サポートセンター](https://support.pathfactory.com/categories/adobe/). このリソースでは、資格情報の取得と管理に関する包括的なガイドを提供します。

### Experience Platformーに対する権限の設定

両方ともある必要があります **[!UICONTROL ソースの表示]** および **[!UICONTROL ソースの管理]** を接続するために、お使いのアカウントで権限が有効になっています [!DNL PathFactory] Experience Platformするアカウント。 必要な権限を取得するには、製品管理者にお問い合わせください。 詳しくは、 [アクセス制御 UI ガイド](../../../access-control/ui/overview.md).

## [!DNL PathFactory] を Platform に接続 {#pathfactory-connect}

以下のドキュメントでは、API やユーザーインターフェイスを使用して [!DNL PathFactory] と Platform を接続する方法について説明します。

* [取り込むソース接続とデータフローを作成 [!DNL PathFactory] api を使用した Platform へのデータ送信](../../tutorials/api/create/marketing-automation/pathfactory.md).
* [を接続 [!DNL PathFactory] ui を使用したExperience Platformへのアカウント設定](../../tutorials/ui/create/marketing-automation/pathfactory.md).
* [UI を使用したソース接続のデータフローの作成](../../tutorials/ui/dataflow/marketing-automation.md).
