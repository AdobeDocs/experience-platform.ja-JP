---
title: Twitter カスタムオーディエンス接続
description: Twitter で既存のフォロワーと顧客をターゲットに設定し、Adobe Experience Platform内に構築したオーディエンスをアクティブ化して、関連するリマーケティングキャンペーンを作成します
exl-id: fd244e58-cd94-4de7-81e4-c321eb673b65
source-git-commit: 5b529a1af62c2745c3226029de1a1ff508bddfc7
workflow-type: tm+mt
source-wordcount: '971'
ht-degree: 39%

---

# [!DNL Twitter Custom Audiences] 接続

## 概要 {#overview}

>[!IMPORTANT]
>
>* 2025 年 9 月 9 日（PT）以降、宛先カタログに 2 つの **[!DNL Twitter Custom Audiences]** カードが並んで表示されるようになります。 これは、宛先サービスの内部アップグレードが原因です。 既存の **[!DNL Twitter Custom Audiences]** 宛先コネクタの名前は、**[!UICONTROL （非推奨） Twitter カスタムオーディエンスに変更され]** 名前が **[!UICONTROL Twitter カスタムオーディエンス]** の新しいカードが使用できるようになりました。
>* 新しいアクティベーションデータフローには、カタログの新しい **[!UICONTROL Twitter カスタムオーディエンス]** 接続を使用します。 **[!UICONTROL （非推奨） Twitter カスタムオーディエンス]** の宛先へのアクティブなデータフローがある場合、それらは自動的に更新されるので、ユーザーからのアクションは必要ありません。
>* [Flow Service API](https://developer.adobe.com/experience-platform-apis/references/destinations/) を使用してデータフローを作成する場合は、[!DNL flow spec ID] を更新し、次の値に [!DNL connection spec ID] す必要があります。
>   * フロー仕様 ID: `903da9e4-7cf5-442a-9498-a237e4f064f9`
>   * 接続仕様 ID: `9eb18875-a095-4b89-854e-39b9e29ccd41`

Twitter で既存のフォロワーと顧客をターゲットに設定し、Adobe Experience Platform 内に構築したオーディエンスをアクティブ化して、関連するリマーケティングキャンペーンを作成します。

## 前提条件 {#prerequisites}

[!DNL Twitter Custom Audiences] の宛先を設定する前に、満たす必要のある次の Twitter の前提条件を確認してください。

1. [!DNL Twitter Ads] アカウントは広告の対象となる必要があります。 新しい [!DNL Twitter Ads] アカウントは、作成後の最初の 2 週間は広告の対象になりません。
2. [!DNL Twitter Audience Manager] でのアクセスを許可した Twitter ユーザーアカウントで、*[!DNL Partner Audience Manager]* 権限が有効になっている必要があります。

## サポートされている ID {#supported-identities}

[!DNL Twitter Custom Audiences] では、以下の表で説明する ID のアクティブ化をサポートしています。 [ID](https://experienceleague.adobe.com/docs/experience-platform/identity/namespaces.html#getting-started) についての詳細情報。

| ターゲット ID | 説明 | 注意点 |
|---|---|---|
| device_id | IDFA/AdID/Android ID | Adobe Experience Platformでは、広告主のGoogle Advertising ID （GAID）とApple ID （IDFA）がサポートされています。 宛先のアクティベーションワークフローの [ マッピング手順 ](/help/destinations/ui/activate-segment-streaming-destinations.md#mapping) で、ソーススキーマからこれらの名前空間や属性を適切にマッピングしてください。 |
| メール | ユーザーのメールアドレス | プレーンテキストのメールアドレスと SHA256 でハッシュ化されたメールアドレスをこのフィールドにマッピングしてください。 ハッシュ化されていない属性がソースフィールドに含まれている場合は、「**[!UICONTROL 変換を適用]**」オプションをオンにして、アクティブ化時にがデータ [!DNL Experience Platform] 自動的にハッシュ化するように設定します。 お客様のメールアドレスをAdobe Experience Platformにアップロードする前にハッシュ化する場合、これらの ID は、ソルトを使用せずに、SHA256 を使用してハッシュ化する必要があることに注意してください。 |

{style="table-layout:auto"}

## サポートされるオーディエンス {#supported-audiences}

この節では、この宛先に書き出すことができるオーディエンスのタイプについて説明します。

| オーディエンスオリジン | サポートあり | 説明 |
|---------|----------|----------|
| [!DNL Segmentation Service] | ✓ | Experience Platform [ セグメント化サービス ](../../../segmentation/home.md) を通じて生成されたオーディエンス。 |
| カスタムアップロード | ✓ | CSV ファイルから Experience Platform に[読み込まれた](../../../segmentation/ui/audience-portal.md#import-audience)オーディエンス。 |

{style="table-layout:auto"}

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL オーディエンスの書き出し]** | Twitter カスタムオーディエンスの宛先で使用される識別子を使用して、オーディエンスのすべてのメンバーを書き出します。 |
| 書き出し頻度 | **[!UICONTROL ストリーミング]** | ストリーミングの宛先は常に、API ベースの接続です。オーディエンス評価に基づいて Experience Platform 内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。[ストリーミングの宛先](/help/destinations/destination-types.md#streaming-destinations)の詳細についてはこちらを参照してください。 |

{style="table-layout:auto"}

## ユースケース {#use-cases}

[!DNL Twitter Custom Audiences] の宛先を使用する方法とタイミングをより深く理解するために、Adobe Experience Platformのお客様がこの宛先を使用して解決できるユースケースのサンプルを以下に示します。

### ユースケース 1

Twitter で既存のフォロワーと顧客をターゲットに設定し、Adobe Experience Platform内に構築したオーディエンスをアクティブ化して、関連するリマーケティングキャンペーンを作成します。Twitter の [!DNL List Custom Audiences] を使用してください。

## 宛先に接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、**[!UICONTROL 宛先の表示]** および **[!UICONTROL 宛先の管理]**&#x200B;[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。 詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つのセクションにリストされているフィールドに入力します。

### 宛先に対する認証 {#authenticate}

1. 宛先カタログで [!DNL Twitter Custom Audiences] の宛先を見つけて、「**[!UICONTROL 設定]**」を選択します。
2. **[!UICONTROL 宛先に接続]** を選択します。
   ![LinkedIn への認証 ](/help/destinations/assets/catalog/social/twitter/authenticate-twitter-destination.png)
3. Twitter 資格情報を入力し、「**ログイン**」を選択します。

### 宛先の詳細の入力 {#destination-details}

>[!CONTEXTUALHELP]
>id="platform_destinations_connect_twitter_accountid"
>title="アカウント ID"
>abstract="Twitter 広告アカウント ID。これは、Twitter 広告設定で確認できます。"

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横のアスタリスクは、そのフィールドが必須であることを示します。

* **[!UICONTROL 名前]**：今後この宛先を認識するための名前。
* **[!UICONTROL 説明]**：今後この宛先を識別するのに役立つ説明。
* **[!UICONTROL アカウント ID]**：お使いの [!DNL Twitter Ads] アカウント ID。 これは、[!DNL Twitter Ads] の設定で確認できます。

>[!IMPORTANT]
>
>特殊文字（+ &amp;、% : ; @ / = ? $ \n）を使用して、オーディエンス、説明およびオーディエンスマッピング名を定義します。 Experience Platform オーディエンス名にこれらの文字が含まれている場合は、それらを削除してから、オーディエンスを Twitter の宛先にマッピングしてください。

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続の詳細の入力を終えたら「**[!UICONTROL 次へ]**」を選択します。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>* データをアクティブ化するには、**[!UICONTROL 宛先の表示]**、**[!UICONTROL 宛先のアクティブ化]**、**[!UICONTROL プロファイルの表示]** および **[!UICONTROL セグメントの表示]**&#x200B;[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。
>* *ID* を書き出すには、**[!UICONTROL ID グラフの表示]**&#x200B;[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。<br> ![ 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択します。](/help/destinations/assets/overview/export-identities-to-destination.png " 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択 "){width="100" zoomable="yes"}

この宛先にオーディエンスをアクティベートする手順は、[ストリーミングオーディエンスの書き出し宛先へのプロファイルとオーディエンスのアクティベート](/help/destinations/ui/activate-segment-streaming-destinations.md)を参照してください。

### マッピングに関する考慮事項 {#mapping-considerations}

オーディエンスを Twitter にマッピングする場合は、人間が読み取れるオーディエンスマッピング名を指定します。 Experience Platform セグメントと同じ名前を使用することをお勧めします。

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。[!DNL Adobe Experience Platform] がどのようにデータガバナンスを実施するかについて詳しくは、[データガバナンスの概要](https://experienceleague.adobe.com/docs/experience-platform/data-governance/home.html?lang=ja)を参照してください。

## その他のリソース {#additional-resources}

Twitter での [!DNL List Custom Audiences] の使用について詳しくは、[Twitter ドキュメント ](https://business.twitter.com/en/help/campaign-setup/campaign-targeting/custom-audiences/lists.html) を参照してください。
