---
keywords: Experience Platform；ホーム；人気の高いトピック；Pinterest広告；
title: Pinterest Ads ソースの概要
description: API またはユーザーインターフェイスを使用してPinterest Ads をAdobe Experience Platformに接続する方法を説明します。
badge: ベータ
hide: true
hidefromtoc: true
exl-id: 8edbcb26-0a18-47f1-8012-ca209d99d7a6
source-git-commit: e37c00863249e677f1645266859bf40fe6451827
workflow-type: tm+mt
source-wordcount: '984'
ht-degree: 13%

---

# [!DNL Pinterest Ads]

>[!NOTE]
>
>[!DNL Pinterest Ads] ソースはベータ版です。詳しくは、 [ソースの概要](../../home.md#terms-and-conditions) ベータ版のコネクタの使用に関する詳細

Adobe Experience Platform を使用すると、外部ソースからデータを取り込みながら、Platform サービスを使用して受信データの構造化、ラベル付けおよび拡張を行うことができます。アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取り込むことができます。

Adobe Experience Platform には、サードパーティの広告システムからデータを取り込む機能が用意されています。広告プロバイダーのサポートは次のとおりです。 [!DNL Pinterest Ads].

[[!DNL Pinterest]](https://www.pinterest.com) は、Web 上でレシピ、ホームデコア、スタイルインスピレーション、その他のアイデアを見つけるための視覚的発見エンジンです。 画像、アニメーションGIF、ビデオをピンボード形式で使用して、小規模に表示します。 [[!DNL Pinterest Ads]](https://ads.pinterest.com/) ビジネスを成長させ、 [!DNL Pinterest].

を使用 [!DNL Pinterest Ads]を使用すると、ターゲット広告を通じてユーザーにリーチし、製品を検出して購入することができます。 ピン元 [!DNL Pinterest Ads] は、関連する検索結果で余分な露出を受けるためにスポンサー提供されています。 購読しているユーザー [!DNL Pinterest Business] では、既存の最もパフォーマンスの高いピンの昇格、新しい画像またはビデオの作成、Web サイトから固定された画像の昇格を選択できます。 [!DNL Pinterest Ads] では、特定のキャンペーン目標を満たすのに役立つ様々な広告形式を提供しています。

## [!DNL Pinterest] API {#pinterest-apis}

この [!DNL Pinterest Ads] ソースは [!DNL Pinterest] 取得する API [!DNL Pinterest Ads] データに加えて、すべてのパフォーマンスと指標も含まれます。 サポートされる API エンドポイントは次のとおりです。

* [キャンペーン分析](https://developers.pinterest.com/docs/api/v5/#operation/campaigns/analytics)
* [広告グループ分析](https://developers.pinterest.com/docs/api/v5/#operation/ad_groups/analytics)
* [広告分析](https://developers.pinterest.com/docs/api/v5/#operation/ads/analytics)

以下を使用： [!DNL Pinterest Ads] データを取り込むソース [!DNL Pinterest] をExperience Platformに追加し、そこでデータ分析を実行できます。 データは、取り込み日から 90 日の日付を遡った範囲で返されます。 [!DNL Pinterest Ads] は、bearer トークンを認証メカニズムとして使用し、 [!DNL Pinterest] API

## 前提条件 {#prerequisites}

の作成の最初の手順 [!DNL Pinterest Ads] ソース接続は、Pinterest開発者アカウントを持っていることを確認するためです。 まだない場合は、 [サインアップ](https://www.pinterest.com/business/create/?next=https://developers.pinterest.com/account-setup/) 登録してアカウントを作成するページ。

### 設定 [!DNL Pinterest] アプリケーションとアクセストークンの生成 {#create-app-and-generate-token}

>[!IMPORTANT]
>
>次を使用することをお勧めします。 [!DNL Pinterest] UI でアクセストークンを生成するとアクセスが制限されるので、アクセストークンを生成する API。 UI を使用してアクセスできるのは、次のスコープのみです。 `pins:read`, `boards:read` および `user_accounts:read`. この制限は、Analytics エンドポイントでの使用には適していません。 [!DNL Pinterest] API

アクセストークンを生成するには、 [!DNL Pinterest] ～に関するガイド [アプリの設定](https://developers.pinterest.com/docs/getting-started/set-up-app/) および [OAuth 2.0 を使用した認証](https://developers.pinterest.com/docs/getting-started/authentication/).

### 必要な認証情報の収集 {#gather-required-credentials}

[!DNL Pinterest Ads] を Platform に接続するには、次の接続プロパティの値を指定する必要があります。

| 認証情報 | 説明 |
| --- | --- |
| アクセストークン | この [!DNL Pinterest Ads] ユーザーアカウントのアクセストークン。 トークンのユーザーアカウントは、指定した [!DNL Pinterest Ad] アカウントに割り当てるか、ビジネスアクセスを通じて必要な役割の 1 つを割り当てます。管理者、アナリストまたはキャンペーンマネージャー。 アクセストークンについて詳しくは、 [[!DNL Pinterest] アクセストークン生成のガイド](https://developers.pinterest.com/docs/getting-started/set-up-app/). |
| 広告アカウント ID | 関連する [!DNL Pinterest Ads] ビジネスユニットの広告アカウント ID。 広告アカウント ID の取得に関する情報を参照してください。 次にアクセス： [[!DNL Pinterest] Ads Manager での ID の検索に関するガイド](https://help.pinterest.com/en/business/article/find-ids-in-ads-manager). |
| キャンペーン、広告グループ、広告 ID | この `campaign`, `ad group`または `ad` 広告アカウント ID に対応する ID。 必要な ID を取得するには、 [!DNL Pinterest] ページ **Pinterest Business Hub** > **広告アカウントの概要** > **キャンペーン** / **広告グループ** / **広告** とで、それぞれの名前のすぐ下に記載されている必要な ID をコピーします。 |

>[!NOTE]
>
>この [!DNL Pinterest] API は、各 ID に関連付けられたデータを取得するための個々の API を提供します。 したがって、目的の ID タイプに対応する ID のみを渡す必要があります。

## ガードレール {#guardrails}

次の節では、のデータガードレールに関する情報を示します。 [!DNL Pinterest].

### [!DNL Pinterest] 日付範囲 {#pinterest-date-range}

この [!DNL Pinterest] API は、 `start_date` および `end_date` パラメーターを使用して特定の日付範囲の分析データを取得する必要があります。

* この `start_date` は、現在の日付の 90 日前より前に設定することはできません。
* この `end_date` 次の日から 90 日を超えてはなりません： `start_date`.

データフローをスケジュールする場合、次の頻度と間隔の設定のいずれかを設定する必要があります。

| 頻度 | 間隔 |
| --- | --- |
| `Day` | 1 |
| `Hour` | 24 |

例えば、取り込みが 2023 年 3 月 15 日に設定され、頻度と間隔の設定が `Day=1` または `Hour=24`、 [!DNL Pinterest] 計算の日付が 90 日間遡られるので、API では 2022 年 12 月 15 日までのデータのみを取得します。

### [!DNL Pinterest] 時間範囲 {#pinterest-time-range}

この [!DNL Pinterest] API は、データを取得する方法に関して、様々な種類の時間の精度をサポートしています。

| 時間の精度 | 説明 |
| --- | --- |
| **合計** | データ指標は、指定した日付範囲にわたって集計されます。 |
| **日** | データ指標は、毎日分類されます。 |
| **時間** | データ指標は 1 時間ごとに分類されます。 |
| **毎週** | データ指標は週単位で分類されます。 |
| **毎月** | データ指標は月単位で分類されます。 |

プラットフォームの場合、 [!DNL Pinterest Ads] ソースは内部的に `Day`：データは毎日集計されます。 例えば、 `impressions recorded` を指標として使用する場合は、精度が `DAY`、 `xx` インプレッション数 `day 1`, `yy` インプレッション数 `day 2` など。

>[!IMPORTANT]
>
>Pinterestは、広告、広告グループ、広告キャンペーンから情報を読み取るために、API に 1 日に 1000 個の API 呼び出しのレート制限を課しています。 基になる API 呼び出しに適用されるレート制限について詳しくは、 [[!DNL Pinterest] レート制限に関するドキュメント](https://developers.pinterest.com/docs/reference/ratelimits/).

## [!DNL Pinterest Ads] を Platform に接続 {#connect-to-platform}

以下のドキュメントでは、API やユーザーインターフェイスを使用して [!DNL Pinterest Ads] と Platform を接続する方法について説明します。

### API を使用して [!DNL Pinterest Ads] と Platform を接続する {#connect-to-platform-using-api}

* [フローサービス API を使用したPinterestベース接続の作成](../../tutorials/api/create/advertising/pinterest-ads.md)
* [Flow Service API を使用したデータテーブルの探索](../../tutorials/api/explore/tabular.md)
* [フローサービス API を使用した広告ソースのデータフローの作成](../../tutorials/api/collect/advertising.md)

### UI を使用した [!DNL Pinterest Ads] の Platform への接続 {#connect-to-platform-using-ui}

* [UI でのPinterestソース接続の作成](../../tutorials/ui/create/advertising/pinterest-ads.md)
* [UI での広告ソース接続のデータフローの作成](../../tutorials/ui/dataflow/advertising.md)
