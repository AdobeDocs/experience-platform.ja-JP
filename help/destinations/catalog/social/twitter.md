---
title: Twitterカスタムオーディエンス接続
description: twitterで既存のフォロワーと顧客をターゲットに設定し、Adobe Experience Platform内に構築したオーディエンスをアクティブ化して、関連するリマーケティングキャンペーンを作成します
exl-id: fd244e58-cd94-4de7-81e4-c321eb673b65
source-git-commit: c35b43654d31f0f112258e577a1bb95e72f0a971
workflow-type: tm+mt
source-wordcount: '862'
ht-degree: 43%

---

# [!DNL Twitter Custom Audiences] 接続

## 概要 {#overview}

Twitter で既存のフォロワーと顧客をターゲットに設定し、Adobe Experience Platform 内に構築したオーディエンスをアクティブ化して、関連するリマーケティングキャンペーンを作成します。

## 前提条件 {#prerequisites}

設定する前に [!DNL Twitter Custom Audiences] 出力先で、満たす必要のある次のTwitterの前提条件を確認してください。

1. あなたの [!DNL Twitter Ads] アカウントは広告の対象となる必要があります。 新規 [!DNL Twitter Ads] アカウントは、作成後の最初の 2 週間は広告の対象になりません。
2. でのアクセスを許可したTwitterユーザーアカウント [!DNL Twitter Audience Manager] に該当する必要があります。 *[!DNL Partner Audience Manager]* 権限が有効になっています。

## サポートされている ID {#supported-identities}

[!DNL Twitter Custom Audiences] では、以下の表で説明する ID のアクティブ化をサポートしています。 [ID](https://experienceleague.adobe.com/docs/experience-platform/identity/namespaces.html#getting-started) についての詳細情報。

| ターゲット ID | 説明 | 注意点 |
|---|---|---|
| device_id | IDFA/AdID/Android ID | Adobe Experience Platformでは、広告主のGoogle Advertising ID （GAID）とApple ID （IDFA）がサポートされています。 で、ソーススキーマからこれらの名前空間や属性を適切にマッピングしてください。 [マッピングステップ](/help/destinations/ui/activate-segment-streaming-destinations.md#mapping) ：宛先のアクティベーションワークフロー |
| メール | ユーザーのメールアドレス | プレーンテキストのメールアドレスと SHA256 でハッシュ化されたメールアドレスをこのフィールドにマッピングしてください。 ハッシュ化されていない属性がソースフィールドに含まれている場合は、 **[!UICONTROL 変換を適用]** 選択肢、持つこと [!DNL Platform] アクティブ化時にデータを自動的にハッシュ化します。 お客様のメールアドレスをAdobe Experience Platformにアップロードする前にハッシュ化する場合、これらの ID は、ソルトを使用せずに、SHA256 を使用してハッシュ化する必要があることに注意してください。 |

{style="table-layout:auto"}

## サポートされるオーディエンス {#supported-audiences}

この節では、この宛先に書き出すことができるオーディエンスのタイプについて説明します。

| オーディエンスオリジン | サポートあり | 説明 |
|---------|----------|----------|
| [!DNL Segmentation Service] | ✓ | Experience Platformを通じて生成されたオーディエンス [セグメント化サービス](../../../segmentation/home.md). |
| カスタムアップロード | ✓ | CSV ファイルから Experience Platform に[読み込まれた](../../../segmentation/ui/audience-portal.md#import-audience)オーディエンス。 |

{style="table-layout:auto"}

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL オーディエンスの書き出し]** | twitterのカスタムオーディエンスの宛先で使用された識別子を使用して、オーディエンスのすべてのメンバーを書き出します。 |
| 書き出し頻度 | **[!UICONTROL ストリーミング]** | ストリーミングの宛先は常に、API ベースの接続です。オーディエンス評価に基づいて Experience Platform 内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。[ストリーミングの宛先](/help/destinations/destination-types.md#streaming-destinations)の詳細についてはこちらを参照してください。 |

{style="table-layout:auto"}

## ユースケース {#use-cases}

を使用する方法とタイミングをより深く理解するために、 [!DNL Twitter Custom Audiences] 宛先の場合、Adobe Experience Platformのお客様がこの宛先を使用して解決できるユースケースのサンプルを以下に示します。

### ユースケース 1

twitterで既存のフォロワーと顧客をターゲットに設定し、Adobe Experience Platform内に構築したオーディエンスをとしてアクティブ化して、関連するリマーケティングキャンペーンを作成します。 [!DNL List Custom Audiences] twitterで。

## 宛先に接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、 **[!UICONTROL 宛先の表示]** および **[!UICONTROL 宛先の管理]** [アクセス制御権限](/help/access-control/home.md#permissions). 詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つのセクションにリストされているフィールドに入力します。

### 宛先に対する認証 {#authenticate}

1. の検索 [!DNL Twitter Custom Audiences] 宛先カタログでの宛先と選択 **[!UICONTROL 設定]**.
2. を選択 **[!UICONTROL 宛先への接続]**.
   ![linkedInに対する認証](/help/destinations/assets/catalog/social/twitter/authenticate-twitter-destination.png)
3. twitter資格情報を入力し、を選択します。 **ログイン**.

### 宛先の詳細の入力 {#destination-details}

>[!CONTEXTUALHELP]
>id="platform_destinations_connect_twitter_accountid"
>title="アカウント ID"
>abstract="Twitter 広告アカウント ID。これは、Twitter 広告設定で確認できます。"

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横のアスタリスクは、そのフィールドが必須であることを示します。

* **[!UICONTROL 名前]**：今後この宛先を認識するための名前。
* **[!UICONTROL 説明]**：今後この宛先を識別するのに役立つ説明。
* **[!UICONTROL アカウント ID]**：あなたの [!DNL Twitter Ads] アカウント ID。 これは [!DNL Twitter Ads] 設定。

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続の詳細の入力を終えたら「**[!UICONTROL 次へ]**」を選択します。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>* データをアクティブ化するには、 **[!UICONTROL 宛先の表示]**, **[!UICONTROL 宛先のアクティブ化]**, **[!UICONTROL プロファイルの表示]**、および **[!UICONTROL セグメントの表示]** [アクセス制御権限](/help/access-control/home.md#permissions). [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。
>* エクスポートする *id*、が必要です **[!UICONTROL ID グラフの表示]** [アクセス制御権限](/help/access-control/home.md#permissions). <br> ![宛先に対してオーディエンスをアクティブ化するために、ワークフローで強調表示されている ID 名前空間を選択します。](/help/destinations/assets/overview/export-identities-to-destination.png "宛先に対してオーディエンスをアクティブ化するために、ワークフローで強調表示されている ID 名前空間を選択します。"){width="100" zoomable="yes"}

この宛先にオーディエンスをアクティベートする手順は、[ストリーミングオーディエンスの書き出し宛先へのプロファイルとオーディエンスのアクティベート](/help/destinations/ui/activate-segment-streaming-destinations.md)を参照してください。

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。[!DNL Adobe Experience Platform] がどのようにデータガバナンスを実施するかについて詳しくは、[データガバナンスの概要](https://experienceleague.adobe.com/docs/experience-platform/data-governance/home.html?lang=ja)を参照してください。

## その他のリソース {#additional-resources}

オーディエンスをTwitterにマッピングする場合は、次のオーディエンスの命名要件を必ず満たすようにします。

1. 人間が読み取れるオーディエンスマッピング名を指定します。 Experience Platformセグメントと同じ名前を使用することをお勧めします。
2. 特殊文字（+ &amp;、% : ; @ / = ? $）を使用してオーディエンスとオーディエンスマッピングの名前を定義します。 Experience Platformオーディエンス名にこれらの文字が含まれている場合は、オーディエンスをTwitterの宛先にマッピングする前に、これらの文字を削除してください。

に関する詳細情報 [!DNL List Custom Audiences] twitterー内のを見つけることができます。 [Twitterドキュメント](https://business.twitter.com/en/help/campaign-setup/campaign-targeting/custom-audiences/lists.html).
