---
title: PathFactory Sourceの概要
description: API またはユーザーインターフェイスを使用して PathFactory をAdobe Experience Platformに接続する方法について説明します。
last-substantial-update: 2024-04-30T00:00:00Z
exl-id: befb73c4-fd6a-4512-9124-d23a1c27e0e0
source-git-commit: 06b2108715ce368ff4ecf5c6c7dd3a327d9f61b1
workflow-type: tm+mt
source-wordcount: '429'
ht-degree: 4%

---

# [!DNL PathFactory]

[[!DNL PathFactory]](https://www.pathfactory.com/) は、企業がコンテンツジャーニーを管理し、インテリジェントなコンテンツインサイトを通じてエンゲージメントを促進するのに役立つ、クラウドベースのプラットフォームを提供します。 このガイドでは、最適なデータ取り込みのために PathFactory のコネクタを使用して、PathFactory からExperience Platformにデータを統合する方法について詳しく説明します。

次の 3 つのプライマリソースを使用して、[[!DNL PathFactory]](https://www.pathfactory.com/) からデータを取り込むことができます。

* **[!DNL Visitors]**：顧客データと連絡先データをレコードとして取り込み、オーディエンスをより深く理解します。
* **[!DNL Sessions]**：プラットフォーム上の個々のユーザーセッションアクティビティを追跡する時系列イベント。
* **[!DNL Page Views]**：表示されているページに関するインサイトを提供し、コンテンツのパフォーマンスの分析に役立つ時系列イベント。

[!DNL PathFactory] ソースアカウントの設定方法については、以下のドキュメントを参照してください。

## IP アドレスの許可リスト {#ip-allow-list}

ソースをExperience Platformに接続する前に、地域固有の IP アドレスを許可リストに追加する必要があります。 詳しくは、[Experience Platformへの接続に対する IP アドレスの許可リストに加える](../../ip-address-allow-list.md) に関するガイドを参照してください。

## 前提条件 {#prerequisites}

[[!DNL PathFactory]](https://www.pathfactory.com/) コネクタとExperience Platformの統合を開始する前に、次の前提条件を満たしていることを確認してください。

* **[PathFactory アカウント]**。
   * 有効なアカウントをお持ちでない場合は、[[!DNL PathFactory]](https://www.pathfactory.com/portal/company/contactus.shtml) にお問い合わせください。
* 任意の **製品に対する** アクティブな購読 [!DNL PathFactory]。
* **ユーザー名、パスワード、ドメイン**。
   * これらの資格情報は、[!DNL PathFactory] アカウントとそのデータにアクセスするために必要です。
* **アクセストークン** と **API エンドポイント**。
   * これらは、[!DNL PathFactory] API との接続に必要です。

### 資格情報とアクセストークンの取得方法 {#gather-credentials}

[!DNL PathFactory] をExperience Platformに接続するには、次の資格情報を指定する必要があります。

| 資格情報 | 説明 | エンドポイント |
| --- | --- | --- |
| ユーザー名 | [!DNL PathFactory] アカウントのユーザー名。 | 該当なし |
| パスワード | [!DNL PathFactory] アカウントのパスワード。 | 該当なし |
| ドメイン | [!DNL PathFactory] アカウントに関連付けられたドメイン。 | 該当なし |
| アクセストークン | API 認証に使用される一意のトークン。 | 該当なし |
| 訪問者エンドポイント | 訪問者データの API エンドポイント。 | `/api/public/v3/data_lake_apis/visitors.json` |
| セッションエンドポイント | セッションデータの API エンドポイント。 | `/api/public/v3/data_lake_apis/sessions.json` |
| ページビューエンドポイント | ページビューデータの API エンドポイント。 | `/api/public/v3/data_lake_apis/page_views.json` |

ユーザー名、パスワード、ドメインおよびアクセストークンの取得方法について詳しくは、[[!DNL PathFactory]  サポートセンター ](https://support.pathfactory.com/categories/adobe/) を参照してください。 このリソースでは、資格情報の取得と管理に関する包括的なガイドを提供します。

### Experience Platformに対する権限の設定

**[!UICONTROL View Sources]** アカウントをExperience Platformに接続するには、アカウントで **[!UICONTROL Manage Sources]** と [!DNL PathFactory] の両方の権限が有効になっている必要があります。 必要な権限を取得するには、製品管理者にお問い合わせください。 詳しくは、[ アクセス制御 UI ガイド ](../../../access-control/ui/overview.md) を参照してください。

## [!DNL PathFactory] をExperience Platformに接続 {#pathfactory-connect}

以下のドキュメントでは、API やユーザーインターフェイスを使用して [!DNL PathFactory] をExperience Platformに接続する方法について説明しています。

* [ ソース接続とデータフローを作成し、API を使用してExperience Platformに  [!DNL PathFactory]  ータを取り込みます ](../../tutorials/api/create/marketing-automation/pathfactory.md)。
* [UI を使用してアカウ  [!DNL PathFactory]  トをExperience Platformに接続します ](../../tutorials/ui/create/marketing-automation/pathfactory.md)。
* [UI を使用したソース接続のデータフローの作成 ](../../tutorials/ui/dataflow/marketing-automation.md)。
