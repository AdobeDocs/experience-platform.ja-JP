---
title: Twitter Custom Audiences 接続
description: twitterで既存のフォロワーや顧客をターゲットに設定し、Adobe Experience Platform内で作成したオーディエンスをアクティブ化して、関連するリマーケティングキャンペーンを作成します
exl-id: fd244e58-cd94-4de7-81e4-c321eb673b65
source-git-commit: 16365865e349f8805b8346ec98cdab89cd027363
workflow-type: tm+mt
source-wordcount: '861'
ht-degree: 51%

---

# [!DNL Twitter Custom Audiences] 接続

## 概要 {#overview}

Twitter で既存のフォロワーと顧客をターゲットに設定し、Adobe Experience Platform 内に構築したオーディエンスをアクティブ化して、関連するリマーケティングキャンペーンを作成します。

## 前提条件 {#prerequisites}

[!DNL Twitter Custom Audiences] の宛先を設定する前に満たす必要がある、以下の Twitter の前提条件を確認してください。

1. [!DNL Twitter Ads] アカウントは広告を利用する資格を持っている必要があります。新規 [!DNL Twitter Ads] アカウントは、作成後最初の 2 週間は、広告を利用する資格がありません。
2. でアクセスを承認したTwitterユーザーアカウント [!DNL Twitter Audience Manager] は、 *[!DNL Partner Audience Manager]* 権限が有効です。

## サポートされている ID {#supported-identities}

[!DNL Twitter Custom Audiences] では、以下の表で説明する ID のアクティベーションをサポートしています。[ID](https://experienceleague.adobe.com/docs/experience-platform/identity/namespaces.html?lang=ja#getting-started) についての詳細情報。

| ターゲット ID | 説明 | 注意点 |
|---|---|---|
| device_id | IDFA/AdID/Android ID | Google Advertising ID(GAID) とApple Advertisers(IDFA) の両方がAdobe Experience Platformでサポートされています。 これらの名前空間や属性は、ソーススキーマから適切にマッピングしてください。 [マッピング手順](/help/destinations/ui/activate-segment-streaming-destinations.md#mapping) 宛先のアクティベーションワークフローの |
| メール | ユーザーの電子メールアドレス | プレーンテキストの電子メールアドレスと SHA256 ハッシュ化された電子メールアドレスを、このフィールドにマッピングしてください。 ハッシュ化されていない属性がソースフィールドに含まれている場合は、「**[!UICONTROL 変換を適用]**」オプションをオンにして、アクティブ化時に [!DNL Platform] がデータを自動的にハッシュ化するように設定します。顧客の電子メールアドレスをAdobe Experience Platformにアップロードする前にハッシュ化する場合、これらの ID は、SHA256 を使用して、ソルトなしでハッシュ化する必要があります。 |

{style="table-layout:auto"}

## サポートされるオーディエンス {#supported-audiences}

この節では、この宛先に書き出すことができるすべてのオーディエンスについて説明します。

この宛先では、Experience Platform [セグメント化サービス](../../../segmentation/home.md).

*さらに*&#x200B;の場合、この宛先では、以下の表で説明するオーディエンスのアクティブ化もサポートされます。

| オーディエンスタイプ | 説明 |
---------|----------|
| カスタムアップロード | オーディエンス [インポート済み](../../../segmentation/ui/overview.md#import-audience) を CSV ファイルからExperience Platformに追加します。 |

{style="table-layout:auto"}

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL オーディエンスの書き出し]** | twitter Custom Audiences の宛先で使用される識別子を使用して、オーディエンスのすべてのメンバーを書き出します。 |
| 書き出し頻度 | **[!UICONTROL ストリーミング]** | ストリーミングの宛先は常に、API ベースの接続です。オーディエンス評価に基づいてExperience Platform内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。 [ストリーミングの宛先](/help/destinations/destination-types.md#streaming-destinations)の詳細についてはこちらを参照してください。 |

{style="table-layout:auto"}

## ユースケース {#use-cases}

[!DNL Twitter Custom Audiences] 宛先を使用する方法とタイミングをより深く理解するために、Adobe Experience Platform のお客様がこの宛先を使用して解決できるサンプルユースケースを以下に示します。

### ユースケース 1

twitterで既存のフォロワーや顧客をターゲットに設定し、Adobe Experience Platform内で作成されたオーディエンスを [!DNL List Custom Audiences] twitterで

## 宛先に接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、**[!UICONTROL 宛先の管理]** [アクセス制御権限](/help/access-control/home.md#permissions)が必要です。詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つのセクションにリストされているフィールドに入力します。

### 宛先に対する認証 {#authenticate}

1. 次を検索： [!DNL Twitter Custom Audiences] 宛先カタログの宛先および「 」を選択します。 **[!UICONTROL 設定]**.
2. 選択 **[!UICONTROL 宛先に接続]**.
   ![linkedInへの認証](/help/destinations/assets/catalog/social/twitter/authenticate-twitter-destination.png)
3. twitter資格情報を入力し、「 」を選択します。 **ログイン**.

### 宛先の詳細の入力 {#destination-details}

>[!CONTEXTUALHELP]
>id="platform_destinations_connect_twitter_accountid"
>title="アカウント ID"
>abstract="Twitter 広告アカウント ID。これは、Twitter 広告設定で確認できます。"

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横のアスタリスクは、そのフィールドが必須であることを示します。

* **[!UICONTROL 名前]**：今後この宛先を認識するための名前。
* **[!UICONTROL 説明]**：今後この宛先を識別するのに役立つ説明。
* **[!UICONTROL アカウント ID]**: [!DNL Twitter Ads] アカウント ID。 これは、 [!DNL Twitter Ads] 設定。

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続への詳細の入力を終えたら「**[!UICONTROL 次へ]**」を選択します。

## この宛先に対するオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>データをアクティブ化するには、**[!UICONTROL 宛先の管理]**、**[!UICONTROL 宛先のアクティブ化]**、**[!UICONTROL プロファイルの表示]**&#x200B;および&#x200B;**[!UICONTROL セグメントの表示]** [に対するアクセス制御権限](/help/access-control/home.md#permissions)が必要です。詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

読み取り [ストリーミングオーディエンスの書き出し先に対するプロファイルとオーディエンスのアクティブ化](/help/destinations/ui/activate-segment-streaming-destinations.md) を参照してください。

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。[!DNL Adobe Experience Platform] がどのようにデータガバナンスを実施するかについて詳しくは、[データガバナンスの概要](https://experienceleague.adobe.com/docs/experience-platform/data-governance/home.html?lang=ja)を参照してください。

## その他のリソース {#additional-resources}

オーディエンスをTwitterにマッピングする場合は、次のオーディエンスの命名要件を満たしていることを確認してください。

1. 人間が読み取り可能なオーディエンスマッピング名を指定する。 Experience Platformセグメントに使用したのと同じ名前を使用することをお勧めします。
2. 特殊文字 (+ &amp; 、 % : ; @ / = ? $) がオーディエンスおよびオーディエンスマッピング名に含まれています。 Experience Platformのオーディエンス名にこれらの文字が含まれている場合は、オーディエンスをTwitterの宛先にマッピングする前に、それらの文字を削除してください。

詳細情報： [!DNL List Custom Audiences] twitterの [Twitterドキュメント](https://business.twitter.com/en/help/campaign-setup/campaign-targeting/custom-audiences/lists.html).
