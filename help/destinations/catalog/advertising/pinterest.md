---
title: Pinterest Customer List 接続
description: 顧客リスト、サイトを訪問した人、またはPinterest上のコンテンツとインタラクションを既に経験した人からオーディエンスを作成します。
exl-id: e601f75f-0d40-4cd0-93ca-54d7439f1db7
source-git-commit: 1b35687350dbbcebfc86acc90852d86870292142
workflow-type: tm+mt
source-wordcount: '829'
ht-degree: 37%

---

# [!DNL Pinterest Customer List] 接続

## 概要 {#overview}

顧客リスト、サイトを訪問した人、またはPinterest上のコンテンツとインタラクションを既に経験した人からオーディエンスを作成します。

>[!IMPORTANT]
>
>この目的地はPinterestチームによって作成されました。 お問い合わせや更新のリクエストについては、https://help.pinterest.com/en/contactまで直接ご連絡ください。

## 前提条件 {#prerequisites}

* ユーザーは、オーディエンスを追加する広告主アカウントにアクセスできるPinterest アカウントで認証する必要があります。 広告主アカウントの共有について詳しくは [&#x200B; こちら &#x200B;](https://help.pinterest.com/en/business/article/share-and-manage-access-to-your-ad-accounts) を参照してください。 特に、ユーザーには「オーディエンス」アクセスレベルが必要になります。
* 顧客リストの ID 形式について詳しくは [&#x200B; こちら &#x200B;](https://help.pinterest.com/en/business/article/audience-targeting) を参照してください。

## サポートされている ID {#supported-identities}

[!DNL Pinterest Customer List] の宛先では、以下の表で説明する ID のアクティブ化をサポートしています。 [ID](https://experienceleague.adobe.com/docs/experience-platform/identity/namespaces.html?lang=ja#getting-started) についての詳細情報。

宛先アクティベーションワークフローの [&#x200B; マッピング手順 &#x200B;](/help/destinations/ui/activate-segment-streaming-destinations.md#mapping) で、目的の ID をターゲットフィールド *pinterest_audience* にマッピングします。 ID は、Pinterestへのデータ取り込み時に区別されて解決されます。

| ターゲット ID | 説明 | 注意点 |
|---|---|---|
| GAID | [!DNL Google Advertising ID] | *GAID* ソース ID 名前空間をターゲット ID フィールド *pinterest_audience* にマッピングします。 |
| IDFA | [!DNL Apple ID for Advertisers] | *IDFA* ソース ID 名前空間をターゲット ID フィールド *pinterest_audience* にマッピングします。 |
| EMAIL | メールアドレス（クリアテキストまたは SHA256 アルゴリズムでハッシュ化） | Adobe Experience Platform では、プレーンテキストと SHA256 でハッシュ化されたメールアドレスの両方がサポートされています。<br> *Email* または *Email_LC_SHA256* ソース ID 名前空間をターゲット ID フィールド *pinterest_audience* にマッピングします。 |

{style="table-layout:auto"}

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL オーディエンスの書き出し]** | Pinterest Customer List の宛先で使用される識別子（名前、電話番号など）を使用して、オーディエンスのすべてのメンバーを書き出します。 |
| 書き出し頻度 | **[!UICONTROL ストリーミング]** | ストリーミングの宛先は常に、API ベースの接続です。オーディエンス評価に基づいて Experience Platform 内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。[ストリーミングの宛先](/help/destinations/destination-types.md#streaming-destinations)の詳細についてはこちらを参照してください。 |

{style="table-layout:auto"}

## ユースケース {#use-cases}

[!DNL Pinterest Customer List] の宛先を使用する方法とタイミングをより深く理解するために、Adobe Experience Platformのお客様がこの宛先を使用して解決できるユースケースのサンプルを以下に示します。

### ユースケース 1

顧客リスト、サイトを訪問した人、またはPinterest上のコンテンツとインタラクションを既に経験した人からオーディエンスを作成します。

## 宛先に接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、**[!UICONTROL 宛先の表示]** および **[!UICONTROL 宛先の管理]**&#x200B;[&#x200B; アクセス制御権限 &#x200B;](/help/access-control/home.md#permissions) が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。

### 接続パラメーター {#parameters}

この宛先を [&#x200B; 設定 &#x200B;](../../ui/connect-destination.md) する際は、次の情報を指定する必要があります。

* **[!UICONTROL 名前]**：今後この宛先を認識するための名前。
* **[!UICONTROL 説明]**：今後この宛先を識別するのに役立つ説明。
* **[!UICONTROL 広告アカウント ID]**：お使いのPinterest広告主 ID。

### 認証資格情報を更新 {#refresh-authentication-credentials}

Pinterest トークンは 30 日ごとに期限切れになります。 トークンの有効期限は、「**[!UICONTROL アカウント]**&#x200B;**[[!UICONTROL または]](../../ui/destinations-workspace.md#accounts)** 参照 **[[!UICONTROL タブの「アカウントの有効期限]](../../ui/destinations-workspace.md#browse)** 列から監視できます。

トークンの有効期限が切れると、宛先へのデータの書き出しは機能しなくなります。 この状況を回避するには、次の手順を実行して再認証を行います。

1. **[!UICONTROL 宛先]**/**[!UICONTROL アカウント]** に移動します。
2. （任意）ページで使用可能なフィルターを使用して、Pinterest アカウントのみを表示します。
   ![Pinterest アカウントのみを表示するようにフィルター &#x200B;](/help/destinations/assets/catalog/advertising/pinterest-customer-list/refresh-oauth-filters.png)
3. 更新するアカウントを選択し、省略記号を選択して **[!UICONTROL 詳細を編集]** を選択します。
   ![&#x200B; 詳細コントロールの編集を選択 &#x200B;](/help/destinations/assets/catalog/advertising/pinterest-customer-list/refresh-oauth-edit-details.png)
4. モーダルウィンドウで、「**[!UICONTROL OAuth に再接続]**」を選択し、Pinterest資格情報を使用して再認証します。
   ![&#x200B; 「OAuth に再接続」オプションを使用したモーダルウィンドウ &#x200B;](/help/destinations/assets/catalog/advertising/pinterest-customer-list/reconnect-oauth-control.png)

>[!SUCCESS]
> 
>認証資格情報が更新され、有効期限が 30 日にリセットされます。

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続の詳細の入力を終えたら「**[!UICONTROL 次へ]**」を選択します。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>* データをアクティブ化するには、**[!UICONTROL 宛先の表示]**、**[!UICONTROL 宛先のアクティブ化]**、**[!UICONTROL プロファイルの表示]** および **[!UICONTROL セグメントの表示]**&#x200B;[&#x200B; アクセス制御権限 &#x200B;](/help/access-control/home.md#permissions) が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。
>* *ID* を書き出すには、**[!UICONTROL ID グラフの表示]**&#x200B;[&#x200B; アクセス制御権限 &#x200B;](/help/access-control/home.md#permissions) が必要です。<br> ![&#x200B; 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択します。](/help/destinations/assets/overview/export-identities-to-destination.png " 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択 "){width="100" zoomable="yes"}

この宛先にオーディエンスをアクティベートする手順は、[ストリーミングオーディエンスの書き出し宛先へのプロファイルとオーディエンスのアクティベート](/help/destinations/ui/activate-segment-streaming-destinations.md)を参照してください。

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。[!DNL Adobe Experience Platform] がどのようにデータガバナンスを実施するかについて詳しくは、[データガバナンスの概要](https://experienceleague.adobe.com/docs/experience-platform/data-governance/home.html?lang=ja)を参照してください。

## その他のリソース {#additional-resources}

詳しくは、[Pinterest ヘルプセンター &#x200B;](https://help.pinterest.com/en/business/article/audience-targeting) ページを参照してください。

+++ 変更ログを表示


| リリース月 | 更新タイプ | 説明 |
|---|---|---|
| 2023年11月 | 機能とドキュメントの更新 | Real-Time CDPのPinterest宛先で v5 Advertiser API が使用されるようになりました。 |

{style="table-layout:auto"}


+++
