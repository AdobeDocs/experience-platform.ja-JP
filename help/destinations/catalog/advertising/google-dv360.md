---
title: Google Display & Video 360 接続
description: ディスプレイ&ビデオ 360 （旧DoubleClick Bid Manager）は、ディスプレイ、ビデオ、モバイルの在庫ソースをまたいで、リターゲティングとオーディエンスをターゲットにしたデジタルキャンペーンを実行するために使用されるツールです。
exl-id: bdd3b3fd-891f-44ec-bd47-daf7f3289f92
source-git-commit: 58f69a78fb3c622c8741d7a1618f15509c160a5b
workflow-type: tm+mt
source-wordcount: '1254'
ht-degree: 44%

---

# [!DNL Google Display & Video 360] 接続

>[!IMPORTANT]
>
> Googleは、欧州連合（[EU ユーザーの同意ポリシー](https://developers.google.com/google-ads/api/docs/start)）の[ デジタル市場法](https://ads-developers.googleblog.com/2023/10/updates-to-customer-match-conversion.html) （DMA）で定義されているコンプライアンスと同意に関する要件をサポートするために、[Google Ads API](https://developers.google.com/display-video/api/guides/getting-started/overview)、[Customer Match](https://digital-markets-act.ec.europa.eu/index_en)、および[Display &amp; Video 360 API](https://www.google.com/about/company/user-consent-policy/)に対する変更をリリースしています。 これらの変更の同意要件への適用は、2024年3月6日現在で有効です。
><br/>
>EUのユーザー同意方針に準拠し、欧州経済地域（EEA）のユーザーに対してオーディエンスリストの作成を継続するには、広告主とパートナーは、オーディエンスデータをアップロードする際に、エンドユーザーの同意を確実に渡す必要があります。 Adobeは、Googleパートナーとして、欧州連合のDMAに基づく同意要件に準拠するために必要なツールを提供します。
><br/>
>Adobe Privacy &amp; Security Shieldを購入し、同意のないプロファイルを除外するように[同意ポリシー](../../../data-governance/enforcement/auto-enforcement.md#consent-policy-evaluation)を設定しているお客様は、何らかの操作を行う必要はありません。
><br/>
>Adobe Privacy &amp; Security Shieldを購入していないお客様は、既存の[ Google宛先を中断なく引き続き使用するために、同意のないプロファイルを除外するために、](../../../segmentation/home.md#segment-definitions) セグメントビルダー[内の](../../../segmentation/ui/segment-builder.md) セグメント定義[!DNL Real-Time CDP]機能を使用する必要があります。

[!DNL Display & Video 360]は、ディスプレイ、ビデオ、モバイルの在庫ソースをまたいで、リターゲティングとオーディエンスをターゲットにしたデジタルキャンペーンを実行するために使用されるツールです。以前は[!DNL DoubleClick Bid Manager]と呼ばれていました。

## 宛先の詳細 {#specifics}

[!DNL Google Display & Video 360] の宛先に固有な次の詳細に注意ください。

* アクティブ化されたオーディエンスは、Google プラットフォームでプログラムを使用して作成されます。
* [!DNL Google Display & Video 360]宛先へのオーディエンスバックフィルのアクティブ化は、オーディエンスが最初に宛先接続にマッピングされてから24 ～ 48時間後に行われるようにスケジュールされています。 この更新は、データの取り込みまで24時間待機するGoogleのポリシーに対応しており、[!DNL Real-Time CDP]と[!DNL Google Display & Video 360]の間の一致率を向上させることを目的としています。 これは、この宛先にのみ適用できるバックエンド設定であり、UIの顧客設定可能なスケジューリングオプションとは関係ありません。

>[!IMPORTANT]
>
>Google Display &amp; Video 360を使用して最初の宛先を作成する場合で、過去に（Adobe Audience Managerまたはその他のアプリケーションで）Experience Cloud ID サービスで[ID同期機能](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/methods/idsync.html?lang=ja)を有効にしていない場合は、Adobe Consultingまたはカスタマーケアに連絡してID同期を有効にしてください。 以前にAudience ManagerでGoogle統合を設定していた場合、設定したID同期はExperience Platformに引き継がれます。

## サポートされている ID {#supported-identities}

[!DNL Google Display & Video 360]は、次の表に示すIDに基づいてオーディエンスをアクティブ化することをサポートしています。 [ID](/help/identity-service/features/namespaces.md) についての詳細情報。

| ID | 説明 | 注意点 |
|---|---|---|
| GAID | [!DNL Google Advertising ID] |  |
| IDFA | [!DNL Apple ID for Advertisers] |  |
| AAM UUID | [Adobe Audience Manager [!DNL Unique User ID]](https://experienceleague.adobe.com/docs/audience-manager/user-guide/reference/ids-in-aam.html?lang=ja)、別名 [!DNL Device ID]。数値型で 38 桁のデバイス ID。Audience Manager はこの値を、操作するデバイスのそれぞれに関連付けます。 | Google は [AAM UUID](https://experienceleague.adobe.com/docs/audience-manager/user-guide/reference/ids-in-aam.html?lang=ja) を使用して、カリフォルニア州のユーザーをターゲット設定し、他のすべてのユーザーに対して Google Cookie ID を使用します。 |
| [!DNL Google] Cookie ID | [!DNL Google] Cookie ID | [!DNL Google] は、この ID を使用してカリフォルニア州以外のユーザーをターゲットします。 |
| RIDA | 広告用 Roku ID。 この ID は、Roku デバイスを一意に識別します。 |  |
| MAID | Microsoft Advertising ID。この ID は、Windows 10 を実行しているデバイスを一意に識別します。 |  |
| Amazon Fire TV ID | この ID は、Amazon Fire TV を一意に識別します。 |  |

## サポートされるオーディエンス {#supported-audiences}

この節では、この宛先に書き出すことができるオーディエンスのタイプについて説明します。

| オーディエンスの由来 | サポートあり | 説明 |
|---------|----------|----------|
| [!DNL Segmentation Service] | ○ | Experience Platform [ セグメント化サービス ](../../../segmentation/home.md)を通じて生成されたオーディエンス。 |
| その他すべてのオーディエンスの生成元 | ○ | このカテゴリには、[!DNL Segmentation Service]を通じて生成されたオーディエンス以外のすべてのオーディエンスのオリジンが含まれます。 [様々なオーディエンスの起源](/help/segmentation/ui/audience-portal.md#customize)について読みます。 次に例を示します。 <ul><li> カスタムアップロードオーディエンス [がCSV ファイルからExperience Platformに](../../../segmentation/ui/audience-portal.md#import-audience)をインポートしました。</li><li> 類似オーディエンス， </li><li> 連合オーディエンス， </li><li> [!DNL Adobe Journey Optimizer]などの他のExperience Platform アプリで生成されたオーディエンス </li><li> その他。 </li></ul> |

{style="table-layout:auto"}



オーディエンスのデータタイプ別にサポートされるオーディエンス：

| オーディエンスのデータタイプ | サポートあり | 説明 | ユースケース |
|--------------------|-----------|-------------|-----------|
| [人物オーディエンス ](/help/segmentation/types/people-audiences.md) | ○ | 顧客プロファイルにもとづいて、マーケティング施策の特定のグループをターゲットにすることができます。 | 買い物客やカートの放棄が多い |
| [ アカウントオーディエンス ](/help/segmentation/types/account-audiences.md) | × | アカウントベースドマーケティング戦略のために、特定の組織内の個人をターゲットにします。 | B2B マーケティング |
| [見込みオーディエンス ](/help/segmentation/types/prospect-audiences.md) | × | まだ顧客ではないが、ターゲットオーディエンスと特徴を共有する個人をターゲットにします。 | サードパーティデータによる見込み顧客の開拓 |
| [ データセットの書き出し](/help/catalog/datasets/overview.md) | × | [!DNL Adobe Experience Platform] データ レイクに保存されている構造化データのコレクション。 | レポート，データサイエンスワークフロー |

{style="table-layout:auto"}


## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
|---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL Audience export]** | オーディエンスのすべてのメンバーを Google の宛先に書き出します。 |
| 書き出し頻度 | **[!UICONTROL Streaming]** | ストリーミングの宛先は常に、API ベースの接続です。オーディエンス評価に基づいて Experience Platform 内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。[ストリーミングの宛先](/help/destinations/destination-types.md#streaming-destinations)の詳細についてはこちらを参照してください。 |

## 前提条件 {#prerequisites}

### 許可リストへの登録 {#allow-listing}

>[!NOTE]
>
>許可リストへの登録は、Experience Platformで最初の[!DNL Google Display & Video 360]の宛先を設定する前に必須です。 宛先を作成する前に、以下に説明する許可リストへの登録プロセスが[!DNL Google]までに完了していることを確認してください。
>このルールの例外は、[Audience Manager](https://experienceleague.adobe.com/docs/audience-manager/user-guide/aam-home.html?lang=ja) 顧客の場合です。この Google の宛先への接続を Audience Manager で既に作成している場合は、許可リストへの登録プロセスを再度実行する必要はありません。次の手順に進んでください。

Experience Platformで[!DNL Google Display & Video 360]の宛先を作成する前に、Googleに連絡して、Adobeを許可されたデータプロバイダーのリストに追加し、アカウントを許可リストに追加するように求める必要があります。 Google に連絡し、次の情報を提供します。

* **アカウント ID**：アドビの Google アカウント ID です。アカウント ID：87933855。
* **顧客 ID**：アドビの Google 顧客アカウント ID です。顧客 ID：89690775。
* **アカウントの種類**：**[!DNL Invite advertiser]** を使用して、Display &amp; Video 360 アカウントにある特定のブランドにのみオーディエンスを共有するか、**[!DNL Invite partner]** を使用して Display &amp; Video 360 アカウントのすべてのブランドにオーディエンスを共有します。

## 宛先への接続 {#connect}

>[!IMPORTANT]
>
>宛先に接続するには、**[!UICONTROL View Destinations]**&#x200B;および&#x200B;**[!UICONTROL Manage Destinations]** [ アクセス制御権限](/help/access-control/home.md#permissions)が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。

### 接続パラメーター {#parameters}

この宛先を[設定](../../ui/connect-destination.md)するとき、次の情報を指定する必要があります。

* **[!UICONTROL Name]**：この宛先の優先名を入力します。
* **[!UICONTROL Description]**：オプション。例えば、この宛先を使用しているキャンペーンを指定できます。
* **[!UICONTROL Account Type]**: Googleのアカウントに応じて、次のオプションを選択します。
   * Display &amp; Video 360 アカウントにある特定のブランドにのみオーディエンスを共有するには、`Invite Advertiser` を使用します。
   * Display &amp; Video 360 アカウントのすべてのブランドにオーディエンスを共有するには、`Invite Partner` を使用します。
* **[!UICONTROL Account ID]**: **[!DNL Invite partner]**&#x200B;または&#x200B;**[!DNL Invite advertiser]** アカウント IDをGoogleで入力します。 通常、これは 6 桁または 7 桁の ID です。

>[!NOTE]
>
>[!DNL Google Display & Video 360]宛先を設定する際は、[!DNL Google Account Manager]またはAdobeの担当者と協力して、どのアカウントタイプを持っているかを把握してください。

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続の詳細の提供が完了したら、**[!UICONTROL Next]**&#x200B;を選択します。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
>
>データをアクティブ化するには、**[!UICONTROL View Destinations]**、**[!UICONTROL Activate Destinations]**、**[!UICONTROL View Profiles]**&#x200B;および&#x200B;**[!UICONTROL View Segments]** [ アクセス制御権限](/help/access-control/home.md#permissions)が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。

この宛先にオーディエンスをアクティブ化する手順については、[ストリーミングオーディエンス書き出し宛先に対するオーディエンスデータのアクティブ化](../../ui/activate-segment-streaming-destinations.md)を参照してください。

## 書き出したデータ {#exported-data}

データがに正常に [!DNL Google Display & Video 360] の宛先に書き出されたかどうかを確認するには、[!DNL Google Display & Video 360] アカウントを確認します。 アクティベーションに成功すると、オーディエンスがお使いのアカウントに入力されます。

## トラブルシューティング {#troubleshooting}

### 400 Bad Request エラーメッセージ {#bad-request}

この宛先を設定する際に、次のエラーが発生する場合があります。

`{"message":"Google Error: AuthorizationError.USER_PERMISSION_DENIED","code":"400 BAD_REQUEST"}`

このエラーは、顧客アカウントが[の前提条件](#prerequisites)に準拠していない場合に発生します。 この問題を解決するには、Googleに連絡し、アカウントが許可リストに登録されていることを確認してください。