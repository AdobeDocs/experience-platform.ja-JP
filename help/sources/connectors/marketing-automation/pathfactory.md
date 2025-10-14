---
title: PathFactory Sourceの概要
description: API またはユーザーインターフェイスを使用して PathFactory をAdobe Experience Platformに接続する方法について説明します。
last-substantial-update: 2024-04-30T00:00:00Z
exl-id: befb73c4-fd6a-4512-9124-d23a1c27e0e0
source-git-commit: 40c3745920204983f5388de6cba1402d87eda71c
workflow-type: tm+mt
source-wordcount: '449'
ht-degree: 11%

---

# [!DNL PathFactory]

[[!DNL PathFactory]](https://www.pathfactory.com/) は、企業がコンテンツジャーニーを管理し、インテリジェントなコンテンツインサイトを通じてエンゲージメントを促進するのに役立つ、クラウドベースのプラットフォームを提供します。 このガイドでは、最適なデータ取り込みのために PathFactory のコネクタを使用して、PathFactory からExperience Platformにデータを統合する方法について詳しく説明します。

次の 3 つのプライマリソースを使用して、[[!DNL PathFactory]](https://www.pathfactory.com/) からデータを取り込むことができます。

* **[!DNL Visitors]**：顧客データと連絡先データをレコードとして取り込み、オーディエンスをより深く理解します。
* **[!DNL Sessions]**：プラットフォーム上の個々のユーザーセッションアクティビティを追跡する時系列イベント。
* **[!DNL Page Views]**：表示されているページに関するインサイトを提供し、コンテンツのパフォーマンスの分析に役立つ時系列イベント。

[!DNL PathFactory] ソースアカウントの設定方法については、以下のドキュメントを参照してください。

## IP アドレス許可リスト {#ip-allow-list}

ソースコネクタを操作する前に、IP アドレスのリストを許可リストに追加する必要がある場合があります。 地域固有の IP アドレスを許可リストに追加しないと、ソースを使用する際にエラーが発生したり、パフォーマンスが低下する場合があります。 詳しくは、[IP アドレスの許可リスト](../../ip-address-allow-list.md)ページを参照してください。

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

ユーザー名、パスワード、ドメインおよびアクセストークンの取得方法について詳しくは、[[!DNL PathFactory]  サポートセンター &#x200B;](https://support.pathfactory.com/categories/adobe/) を参照してください。 このリソースでは、資格情報の取得と管理に関する包括的なガイドを提供します。

### Experience Platformに対する権限の設定

**[!UICONTROL アカウントをExperience Platformに接続するには、アカウントで]** ソースの表示 **[!UICONTROL および]** ソースの管理 [!DNL PathFactory] 権限の両方が有効になっている必要があります。 必要な権限を取得するには、製品管理者にお問い合わせください。 詳しくは、[&#x200B; アクセス制御 UI ガイド &#x200B;](../../../access-control/ui/overview.md) を参照してください。

## [!DNL PathFactory] をExperience Platformに接続 {#pathfactory-connect}

以下のドキュメントでは、API やユーザーインターフェイスを使用して [!DNL PathFactory] をExperience Platformに接続する方法について説明しています。

* [&#x200B; ソース接続とデータフローを作成し、API を使用してExperience Platformに  [!DNL PathFactory]  ータを取り込みます &#x200B;](../../tutorials/api/create/marketing-automation/pathfactory.md)。
* [UI を使用してアカウ  [!DNL PathFactory]  トをExperience Platformに接続します &#x200B;](../../tutorials/ui/create/marketing-automation/pathfactory.md)。
* [UI を使用したソース接続のデータフローの作成 &#x200B;](../../tutorials/ui/dataflow/marketing-automation.md)。
