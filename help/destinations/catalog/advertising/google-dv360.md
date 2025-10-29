---
title: Google Display & Video 360 接続
description: ディスプレイ&ビデオ 360 （旧称：DoubleClick Bid Manager）は、ディスプレイ、ビデオ、モバイルの在庫ソース全体でリターゲティングやオーディエンスをターゲットにしたデジタルキャンペーンを実行するために使用されるツールです。
exl-id: bdd3b3fd-891f-44ec-bd47-daf7f3289f92
source-git-commit: 1b507e9846a74b7ac2d046c89fd7c27a818035ba
workflow-type: tm+mt
source-wordcount: '1134'
ht-degree: 49%

---

# [!DNL Google Display & Video 360] 接続

>[!IMPORTANT]
>
> Googleは、欧州連合（EU）の [&#x200B; デジタル市場法 &#x200B;](https://developers.google.com/google-ads/api/docs/start) （DMA[）（](https://ads-developers.googleblog.com/2023/10/updates-to-customer-match-conversion.html)EU ユーザー同意ポリシー [）で定義されているコンプライアンスおよび同意関連の要件をサポートするために、](https://developers.google.com/display-video/api/guides/getting-started/overview)Google Ads API[、](https://digital-markets-act.ec.europa.eu/index_en)Customer Match および [Display &amp; Video 360 API](https://www.google.com/about/company/user-consent-policy/) に対する変更内容をリリースしています。 同意要件に対するこれらの変更の適用は 2024 年 3 月 6 日（PT）から開始されます。
> &#x200B;><br/>
> &#x200B;>EU のユーザー同意ポリシーに準拠し、欧州経済領域（EEA）のユーザーに対するオーディエンスリストの作成を続行するには、広告主およびパートナーは、オーディエンスデータをアップロードする際にエンドユーザーの同意を渡していることを確認する必要があります。 Google パートナーであるAdobeは、欧州連合の DMA に基づく同意要件に準拠するために必要なツールを提供します。
> &#x200B;><br/>
> &#x200B;>Adobe Privacy &amp; Security Shield を購入し、同意のないプロファイルを除外する [&#x200B; 同意ポリシー &#x200B;](../../../data-governance/enforcement/auto-enforcement.md#consent-policy-evaluation) を設定している場合は、何もする必要はありません。
> &#x200B;><br/>
> &#x200B;>Adobe Privacy &amp; Security Shield を購入されていないお客様が既存のReal-Time CDP Googleの宛先を引き続き中断することなく使用するには、[&#x200B; セグメントビルダー &#x200B;](../../../segmentation/home.md#segment-definitions) 内の [&#x200B; セグメント定義 &#x200B;](../../../segmentation/ui/segment-builder.md) 機能を使用して、同意のないプロファイルを除外する必要があります。

[!DNL Display & Video 360] （以前の [!DNL DoubleClick Bid Manager]）は、ディスプレイ、ビデオ、モバイルの各インベントリソースをまたいでリターゲティングやオーディエンスをターゲットにしたデジタルキャンペーンを実行するために使用されるツールです。

## 宛先の詳細 {#specifics}

[!DNL Google Display & Video 360] の宛先に固有な次の詳細に注意ください。

* アクティブ化されたオーディエンスは、Google プラットフォームでプログラムを使用して作成されます。
* [!DNL Google Display & Video 360] の宛先へのオーディエンスバックフィルのアクティベーションは、オーディエンスが最初に宛先接続にマッピングされてから 24～48 時間後に行われるようにスケジュールされています。 この更新は、データの取り込みまで 24 時間待機するというGoogleのポリシーに対応するものであり、Real-Time CDPと [!DNL Google Display & Video 360] の間の一致率を向上させることを目的としています。 これは、この宛先にのみ適用されるバックエンド設定で、UI でお客様が設定可能なスケジュールオプションとは無関係です。

>[!IMPORTANT]
>
>Google ディスプレイ&amp;ビデオ 360 を使用して最初の宛先を作成しようとしており、これまで（Adobe Audience Managerなどのアプリケーションを使用して）Experience Cloud ID サービスで [ID 同期機能 &#x200B;](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/methods/idsync.html?lang=ja) を有効にしたことがない場合は、Adobe Consultingまたはカスタマーケアに連絡して ID 同期を有効にしてもらってください。 以前にAudience ManagerでGoogle統合を設定していた場合、設定した ID 同期はExperience Platformに引き継がれます。

## サポートされている ID {#supported-identities}

[!DNL Google Display & Video 360] では、以下の表に示す ID に基づいたオーディエンスのアクティベーションをサポートしています。 [ID](/help/identity-service/features/namespaces.md) についての詳細情報。

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

| オーディエンスオリジン | サポートあり | 説明 |
|---------|----------|----------|
| [!DNL Segmentation Service] | ✓ | Experience Platform [&#x200B; セグメント化サービス &#x200B;](../../../segmentation/home.md) を通じて生成されたオーディエンス。 |
| カスタムアップロード | ✓ | CSV ファイルから Experience Platform に[読み込まれた](../../../segmentation/ui/audience-portal.md#import-audience)オーディエンス。 |

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
>Experience Platformで最初の [!DNL Google Display & Video 360] ール先を設定する前に、許可リストへの登録は必須です。 宛先を作成する前に、[!DNL Google] が以下に説明する許可リストへの登録プロセスを完了していることを確認してください。
>&#x200B;>このルールの例外は、[Audience Manager](https://experienceleague.adobe.com/docs/audience-manager/user-guide/aam-home.html?lang=ja) 顧客の場合です。この Google の宛先への接続を Audience Manager で既に作成している場合は、許可リストへの登録プロセスを再度実行する必要はありません。次の手順に進んでください。

Experience Platformで [!DNL Google Display & Video 360] の宛先を作成する前に、Googleに連絡して、許可されたデータプロバイダーのリストにAdobeを追加し、お使いのアカウントを許可リストに追加することを求める必要があります。 Google に連絡し、次の情報を提供します。

* **アカウント ID**：アドビの Google アカウント ID です。アカウント ID：87933855。
* **顧客 ID**：アドビの Google 顧客アカウント ID です。顧客 ID：89690775。
* **アカウントの種類**：**[!DNL Invite advertiser]** を使用して、Display &amp; Video 360 アカウントにある特定のブランドにのみオーディエンスを共有するか、**[!DNL Invite partner]** を使用して Display &amp; Video 360 アカウントのすべてのブランドにオーディエンスを共有します。

## 宛先への接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、**[!UICONTROL View Destinations]** および **[!UICONTROL Manage Destinations]**&#x200B;[&#x200B; アクセス制御権限 &#x200B;](/help/access-control/home.md#permissions) が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。

### 接続パラメーター {#parameters}

この宛先を[設定](../../ui/connect-destination.md)するとき、次の情報を指定する必要があります。

* **[!UICONTROL Name]**：この宛先に希望する名前を入力します。
* **[!UICONTROL Description]**：オプション。例えば、この宛先を使用しているキャンペーンを指定できます。
* **[!UICONTROL Account Type]**:Googleのアカウントに応じて、次のいずれかのオプションを選択します。
   * Display &amp; Video 360 アカウントにある特定のブランドにのみオーディエンスを共有するには、`Invite Advertiser` を使用します。
   * Display &amp; Video 360 アカウントのすべてのブランドにオーディエンスを共有するには、`Invite Partner` を使用します。
* **[!UICONTROL Account ID]**:Googleを使用して、**[!DNL Invite partner]** または **[!DNL Invite advertiser]** アカウント ID を入力します。 通常、これは 6 桁または 7 桁の ID です。

>[!NOTE]
>
>[!DNL Google Display & Video 360] の宛先を設定する場合は、[!DNL Google Account Manager] またはAdobeの担当者にお問い合わせの上、お持ちのアカウントの種類をご確認ください。

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続への詳細の入力を終えたら「**[!UICONTROL Next]**」を選択します。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>データをアクティブ化するには、**[!UICONTROL View Destinations]**、**[!UICONTROL Activate Destinations]**、**[!UICONTROL View Profiles]**、**[!UICONTROL View Segments]** [&#x200B; アクセス制御権限 &#x200B;](/help/access-control/home.md#permissions) が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。

この宛先にオーディエンスをアクティブ化する手順については、[ストリーミングオーディエンス書き出し宛先に対するオーディエンスデータのアクティブ化](../../ui/activate-segment-streaming-destinations.md)を参照してください。

## 書き出したデータ

データがに正常に [!DNL Google Display & Video 360] の宛先に書き出されたかどうかを確認するには、[!DNL Google Display & Video 360] アカウントを確認します。 アクティベーションに成功すると、オーディエンスがお使いのアカウントに入力されます。

## トラブルシューティング {#troubleshooting}

### 400 Bad Request エラーメッセージ {#bad-request}

この宛先を設定する際に、次のエラーが発生する場合があります。

`{"message":"Google Error: AuthorizationError.USER_PERMISSION_DENIED","code":"400 BAD_REQUEST"}`

このエラーは、顧客アカウントが [&#x200B; 前提条件 &#x200B;](#prerequisites) に準拠していない場合に発生します。 この問題を修正するには、Googleに連絡して、自分のアカウントが許可リストに登録されていることを確認してください。