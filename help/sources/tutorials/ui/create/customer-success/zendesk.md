---
keywords: Experience Platform;Zendesk；ソース；コネクタ；ソースコネクタ；ソース sdk;SDK;zendesk;Zendesk
title: UI での Zendesk Source 接続の作成
description: Adobe Experience Platform UI を使用して Zendesk ソース接続を作成する方法を説明します。
exl-id: 75d303b0-2dcd-4202-987c-fe3400398d90
source-git-commit: e92c2386d9f4a4709f0a749d3ed97e033f066610
workflow-type: tm+mt
source-wordcount: '797'
ht-degree: 32%

---

# （ベータ版） [!DNL Zendesk] UI のソース接続

>[!NOTE]
>
>この [!DNL Zendesk] ソースはベータ版です。 詳しくは、 [ソースの概要](../../../../home.md#terms-and-conditions) ベータラベル付きのソースの使用に関する詳細

このチュートリアルでは、 [!DNL Zendesk] Adobe Experience Platformユーザーインターフェイスを使用したソース接続

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)]  システム](../../../../../xdm/home.md)：[!DNL Experience Platform] が顧客体験データの整理に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

### 必要な認証情報の収集

次の項目にアクセスするには、 [!DNL Zendesk] プラットフォームのアカウントで、次の資格情報の値を指定する必要があります。

| 認証情報 | 説明 | 例 |
| --- | --- | --- |
| サブドメイン | 登録プロセス中に作成されたアカウントに固有の一意のドメイン。 | `https://yoursubdomain.zendesk.com` |
| アクセストークン | Zendesk API トークン。 | `0lZnClEvkJSTQ7olGLl7PMhVq99gu26GTbJtf` |

認証の詳細については、 [!DNL Zendesk] ソース、 [[!DNL Zendesk] ソースの概要](../../../../connectors/customer-success/zendesk.md).

![Zendesk API トークン](../../../../images/tutorials/create/zendesk/zendesk-api-tokens.png)

### 用の Platform スキーマの作成 [!DNL Zendesk]

を作成する前に [!DNL Zendesk] ソース接続の場合は、まず、ソースに使用する Platform スキーマを作成する必要もあります。 に関するチュートリアルを参照してください。 [Platform スキーマの作成](../../../../../xdm/schema/composition.md) スキーマの作成方法に関する包括的な手順を参照してください。

詳しいガイダンス [!DNL Zendesk] に必要なスキーマ [!DNL Zendesk Search API]（を参照） [制限](#limits) 」の節を参照してください。

![スキーマを作成](../../../../images/tutorials/create/zendesk/schema.png)

## [!DNL Zendesk] アカウントの接続

Platform UI の左側のナビゲーションバーで「**[!UICONTROL ソース]**」を選択し、[!UICONTROL ソース]ワークスペースにアクセスします。[!UICONTROL カタログ]画面には、アカウントを作成できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択することができます。または、使用する特定のソースを検索オプションを使用して探すこともできます。

以下 *顧客の成功* カテゴリ、選択 **[!UICONTROL Zendesk]**&#x200B;を選択し、 **[!UICONTROL データを追加]**.

![カタログ](../../../../images/tutorials/create/zendesk/catalog.png)

この **[!UICONTROL Zendesk アカウントに接続]** ページが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

### 既存のアカウント

既存のアカウントを使用するには、 *Zendesk* 新しいデータフローを作成するアカウントを選択し、 **[!UICONTROL 次へ]** をクリックして続行します。

![既存](../../../../images/tutorials/create/zendesk/existing.png)

### 新しいアカウント

新しいアカウントを作成する場合は、「**[!UICONTROL 新規アカウント]**」を選択し、続けて名前、説明（オプション）、 の認証情報を指定します。終了したら「**[!UICONTROL ソースに接続]**」を選択し、新しい接続が確立されるまでしばらく待ちます。

![新規](../../../../images/tutorials/create/zendesk/new.png)

### データの選択

ソースが認証されると、ページはインタラクティブスキーマツリーに更新され、データの階層を調べたり調べたりできます。 「**[!UICONTROL 次へ]**」を選択して次に進みます。

![select-data](../../../../images/tutorials/create/zendesk/select-data.png)

## 次の手順

このチュートリアルに従うことで、 [!DNL Zendesk] アカウントとプラットフォーム。 次のチュートリアルに進み、 [データフローを作成して、顧客の成功データを Platform に取り込む](../../dataflow/customer-success.md).

## その他のリソース

以下の節では、 [!DNL Zendesk] ソース。

### 検証 {#validation}

次に、 [!DNL Zendesk] ソースとそれ [!DNL Zendesk] プロファイルが Platform に取り込まれています。

Platform UI で、「 **[!UICONTROL データセット]** 左側のナビゲーションから [!UICONTROL データセット] ワークスペース。 この [!UICONTROL データセットアクティビティ] screen には、実行の詳細が表示されます。

![アクティビティページ](../../../../images/tutorials/create/zendesk/dataset-activity.png)

次に、表示するデータフローのデータフロー実行 ID を選択して、そのデータフロー実行に関する具体的な詳細を確認します。

![データフローページ](../../../../images/tutorials/create/zendesk/dataflow-monitoring.png)

最後に、 **[!UICONTROL データセットをプレビュー]** をクリックして、取り込まれたデータを表示します。

![Zendesk データセット](../../../../images/tutorials/create/zendesk/preview-dataset.png)

また、Platform データを [!DNL Zendesk] > [!DNL Customers] ページ。

![zendesk-customers](../../../../images/tutorials/create/zendesk/zendesk-customers.png)

### Zendesk スキーマ

次の表に、Zendesk 用に設定する必要がある、サポートされているマッピングを示します。

>[!TIP]
>
>詳しくは、 [Zendesk Search API > 検索結果の書き出し](https://developer.zendesk.com/api-reference/ticketing/ticket-management/search/#export-search-results) を参照してください。

| ソース | タイプ |
|---|---|
| `results.active` | ブール値 |
| `results.alias` | 文字列 |
| `results.created_at` | 文字列 |
| `results.custom_role_id` | 整数 |
| `results.default_group_id` | 整数 |
| `results.details` | 文字列 |
| `results.email` | 文字列 |
| `results.external_id` | 整数 |
| `results.iana_time_zone` | 文字列 |
| `results.id` | 整数 |
| `results.last_login_at` | 文字列 |
| `results.locale` | 文字列 |
| `results.locale_id` | 整数 |
| `results.moderator` | ブール値 |
| `results.name` | 文字列 |
| `results.notes` | 文字列 |
| `results.only_private_comments` | ブール値 |
| `results.organization_id` | 整数 |
| `results.phone` | 文字列 |
| `results.photo` | 文字列 |
| `results.report_csv` | ブール値 |
| `results.restricted_agent` | ブール値 |
| `results.result_type` | 文字列 |
| `results.role` | 文字列 |
| `results.role_type` | 整数 |
| `results.shared` | ブール値 |
| `results.shared_agent` | ブール値 |
| `results.shared_phone_number` | ブール値 |
| `results.signature` | 文字列 |
| `results.suspended` | ブール値 |
| `results.ticket_restriction` | 文字列 |
| `results.time_zone` | 文字列 |
| `results.two_factor_auth_enabled` | ブール値 |
| `results.updated_at` | 文字列 |
| `results.url` | 文字列 |
| `results.verified` | ブール値 |

{style=&quot;table-layout:auto&quot;}

### 制限 {#limits}

* この [Zendesk Search API > 検索結果の書き出し](https://developer.zendesk.com/api-reference/ticketing/ticket-management/search/#export-search-results) は、1 ページにつき最大 1,000 個のレコードを返します。
   * の値 ``filter[type]`` パラメーターが ``user`` したがって、Zendesk 接続はユーザーを返すだけです。
   * 1 ページあたりの結果の数は、 ``page[size]`` パラメーター。 値は ``100``. これは、Zendesk が設定した減速拘束の影響を軽減するために行われます。
   * 詳しくは、 [制限](https://developer.zendesk.com/api-reference/ticketing/ticket-management/search/#limits) および [ページ編集](https://developer.zendesk.com/api-reference/ticketing/ticket-management/search/#pagination-1).
   * また、 [カーソルのページネーションを使用したリスト間のページ分割](https://developer.zendesk.com/documentation/developer-tools/pagination/paginating-through-lists-using-cursor-pagination/).
