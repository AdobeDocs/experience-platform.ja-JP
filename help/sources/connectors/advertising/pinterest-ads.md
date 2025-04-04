---
keywords: Experience Platform；ホーム；人気のトピック；Pinterest Ads;
title: Pinterest Ads Sourceの概要
description: API またはユーザーインターフェイスを使用してPinterest Ads をAdobe Experience Platformに接続する方法について説明します。
badge: ベータ版
hide: true
hidefromtoc: true
exl-id: 8edbcb26-0a18-47f1-8012-ca209d99d7a6
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '946'
ht-degree: 6%

---

# [!DNL Pinterest Ads]

>[!NOTE]
>
>[!DNL Pinterest Ads] ソースはベータ版です。ベータ版のコネクタの使用に関して詳しくは、[ ソースの概要 ](../../home.md#terms-and-conditions) を参照してください。

Adobe Experience Platformを使用すると、データを外部ソースから取得しながら、Experience Platform サービスを使用して、受信データの構造化、ラベル付け、拡張を行うことができます。 アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取り込むことができます。

Adobe Experience Platform には、サードパーティの広告システムからデータを取り込む機能が用意されています。広告プロバイダーのサポートには、[!DNL Pinterest Ads] が含まれます。

[[!DNL Pinterest]](https://www.pinterest.com) は、レシピ、家庭装飾、スタイルのインスピレーション、その他のアイデアを web 全体で見つけるための視覚的な発見エンジンです。 これらは、画像、アニメーション GIF、ビデオをピンボード形式で使用して、小規模で提示されます。 [[!DNL Pinterest Ads]](https://ads.pinterest.com/) を使用すると、ビジネスを成長させ、[!DNL Pinterest] を使用して 4 億人に到達できます。

[!DNL Pinterest Ads] を使用すると、ターゲットを絞った広告を通じてユーザーにリーチし、製品を検出して購入できます。 [!DNL Pinterest Ads] からのピンは、関連する検索結果で追加の露出を受け取るためにスポンサーされています。 [!DNL Pinterest Business] を購読しているユーザーは、既存の最もパフォーマンスの高いピンの昇格、新しい画像やビデオの作成、web サイトにピン留めされた画像の昇格を選択できます。 [!DNL Pinterest Ads] では、特定のキャンペーンの目標を達成するのに役立つ複数の広告形式を提供しています。

## [!DNL Pinterest] API {#pinterest-apis}

[!DNL Pinterest Ads] ソースは、[!DNL Pinterest] API を活用して、すべてのパフォーマンスと指標と共に [!DNL Pinterest Ads] データを取得します。 サポートされている API エンドポイントは次のとおりです。

* [Campaign 分析 ](https://developers.pinterest.com/docs/api/v5/#operation/campaigns/analytics)
* [ 広告グループ分析 ](https://developers.pinterest.com/docs/api/v5/#operation/ad_groups/analytics)
* [ 広告分析 ](https://developers.pinterest.com/docs/api/v5/#operation/ads/analytics)

[!DNL Pinterest Ads] ソースを使用して、[!DNL Pinterest] からExperience Platformにデータを取り込み、データ分析を実行できます。 データは、過去 90 日間の取得日から返されます。 [!DNL Pinterest Ads] は、[!DNL Pinterest] API と通信するための認証メカニズムとして bearer トークンを使用します。

## 前提条件 {#prerequisites}

[!DNL Pinterest Ads] ソース接続を作成する最初の手順は、Pinterest開発者アカウントを持っていることを確認することです。 アカウントをまだお持ちでない場合は、[ 新規登録 ](https://www.pinterest.com/business/create/?next=https://developers.pinterest.com/account-setup/) ページにアクセスし、アカウントを登録および作成してください。

### アプリ [!DNL Pinterest] 設定し、アクセストークンを生成する {#create-app-and-generate-token}

>[!IMPORTANT]
>
>UI でアクセストークンを生成するとアクセスが制限されるので、[!DNL Pinterest] API を使用してアクセストークンを生成することをお勧めします。 UI を使用してアクセスできるのは、`pins:read`、`boards:read` および `user_accounts:read` のスコープのみです。 この制限は、[!DNL Pinterest] API の分析エンドポイントでの使用には適切ではありません。

アクセストークンを生成するには、[!DNL Pinterest] のガイド [ アプリの設定 ](https://developers.pinterest.com/docs/getting-started/set-up-app/) および [OAuth 2.0 を使用した認証 ](https://developers.pinterest.com/docs/getting-started/authentication/) を参照してください。

### 必要な資格情報の収集 {#gather-required-credentials}

[!DNL Pinterest Ads] をExperience Platformに接続するには、次の接続プロパティの値を指定する必要があります。

| 資格情報 | 説明 |
| --- | --- |
| アクセストークン | ユーザーアカウントの [!DNL Pinterest Ads] アクセストークン。 トークンのユーザーアカウントは、指定された [!DNL Pinterest Ad] アカウントの所有者であるか、ビジネスアクセスで必要な役割（管理者、アナリスト、キャンペーンマネージャー）の 1 つが付与されている必要があります。 アクセストークンについて詳しくは、[[!DNL Pinterest]  アクセストークンの生成に関するガイド ](https://developers.pinterest.com/docs/getting-started/set-up-app/) を参照してください。 |
| 広告アカウント ID | ビジネスユニットの関連する [!DNL Pinterest Ads] 広告アカウント ID。 広告アカウント ID の取得については、こちらを参照してください。 [[!DNL Pinterest]  広告マネージャーでの ID の検索に関するガイド ](https://help.pinterest.com/en/business/article/find-ids-in-ads-manager) を参照してください。 |
| キャンペーン、広告グループまたは広告 ID | 広告アカウント ID に対応する `campaign`、`ad group` または `ad` の ID。 必要な ID を取得するには、**Pinterest Business Hub**/**広告アカウントの概要**/**キャンペーン**/**広告グループ**/**広告** の [!DNL Pinterest] ページに移動し、それぞれの名前の下に記載されている必須 ID をコピーします。 |

>[!NOTE]
>
>[!DNL Pinterest] API は、各 ID に関連付けられたデータを取得するための個々の API を提供します。 したがって、目的の ID タイプに対応する ID のみを渡す必要があります。

## ガードレール {#guardrails}

次の節では、[!DNL Pinterest] のデータガードレールに関する情報を示します。

### [!DNL Pinterest] 日付範囲 {#pinterest-date-range}

[!DNL Pinterest] API は、指定された日付範囲の分析データを取得するための `start_date` および `end_date` パラメーターの両方をサポートしています。

* `start_date` は、現在の日付より 90 日以上前にすることはできません。
* `end_date` は、`start_date` 後 90 日を超えることはできません。

データフローをスケジュールする場合、次のいずれかの頻度および間隔の設定を行う必要があります。

| 頻度 | 間隔 |
| --- | --- |
| `Day` | 1 |
| `Hour` | 24 |

例えば、取り込みが 2023 年 3 月 15 日（PT）に設定され、頻度と間隔の設定が `Day=1` または `Hour=24` に設定されている場合、計算が 90 日間遡るので、[!DNL Pinterest] API は 2022 年 12 月 15 日までのデータのみを取得します。

### [!DNL Pinterest] 時間範囲 {#pinterest-time-range}

[!DNL Pinterest] API は、データの取得方法に関して、次のような様々な種類の時間精度をサポートしています。

| 時間の精度 | 説明 |
| --- | --- |
| **合計** | データ指標は、指定した日付範囲にわたって集計されます。 |
| **日** | データ指標は毎日分類されます。 |
| **時間** | データ指標は 1 時間ごとに分類されます。 |
| **毎週** | データ指標は毎週分類されます。 |
| **毎月** | データ指標は月単位で分類されます。 |

Experience Platformの場合、[!DNL Pinterest Ads] ソースは内部で `Day` に設定されています。つまり、データは毎日集計されます。 例えば、`impressions recorded` を指標として使用すると、精度は `DAY` として設定されるので、`day 1` に対するインプレッション数は `xx`、`day 2` に対するインプレッション数 `yy` どが得られます。

>[!IMPORTANT]
>
>Pinterestでは、広告、広告グループまたは広告キャンペーンから情報を読み取るために、API に対して毎日 1,000 回の API 呼び出しのレート制限が課されています。 基になる API 呼び出しに適用されるレート制限について詳しくは、[[!DNL Pinterest]  レート制限に関するドキュメント ](https://developers.pinterest.com/docs/reference/ratelimits/) を参照してください。

## [!DNL Pinterest Ads] をExperience Platformに接続 {#connect-to-platform}

以下のドキュメントでは、API やユーザーインターフェイスを使用して [!DNL Pinterest Ads] をExperience Platformに接続する方法について説明しています。

### API を使用した [!DNL Pinterest Ads] のExperience Platformへの接続 {#connect-to-platform-using-api}

* [Flow Service API を使用したPinterest ベース接続の作成](../../tutorials/api/create/advertising/pinterest-ads.md)
* [Flow Service API を使用したデータテーブルの探索](../../tutorials/api/explore/tabular.md)
* [Flow Service API を使用した広告ソースのデータフローの作成](../../tutorials/api/collect/advertising.md)

### UI を使用した [!DNL Pinterest Ads] のExperience Platformへの接続 {#connect-to-platform-using-ui}

* [UI でのPinterest ソースコネクタの作成](../../tutorials/ui/create/advertising/pinterest-ads.md)
* [UI での広告ソース接続のデータフローの作成](../../tutorials/ui/dataflow/advertising.md)
