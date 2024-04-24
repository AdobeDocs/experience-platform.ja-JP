---
title: Google Display & Video 360 接続
description: ディスプレイ&ビデオ 360 （旧称：DoubleClick Bid Manager）は、ディスプレイ、ビデオ、モバイルの在庫ソース全体でリターゲティングやオーディエンスをターゲットにしたデジタルキャンペーンを実行するために使用されるツールです。
exl-id: bdd3b3fd-891f-44ec-bd47-daf7f3289f92
source-git-commit: 0db22ba2993012357cf65daaeffb5676193fba23
workflow-type: tm+mt
source-wordcount: '1155'
ht-degree: 55%

---

# [!DNL Google Display & Video 360] 接続

>[!IMPORTANT]
>
> Googleは、に対する変更をリリースしています [Google広告 API](https://developers.google.com/google-ads/api/docs/start), [カスタマーマッチ](https://ads-developers.googleblog.com/2023/10/updates-to-customer-match-conversion.html)、および [ディスプレイおよびビデオ 360 API](https://developers.google.com/display-video/api/guides/getting-started/overview) に基づいて定義されるコンプライアンスおよび同意関連の要件をサポートするため [デジタル市場法](https://digital-markets-act.ec.europa.eu/index_en) （DMA）の適用（[EU ユーザー同意ポリシー](https://www.google.com/about/company/user-consent-policy/)）に設定します。 同意要件に対するこれらの変更の適用は 2024 年 3 月 6 日（PT）から開始されます。
><br/>
>EU のユーザー同意ポリシーに準拠し、欧州経済領域（EEA）のユーザーに対するオーディエンスリストの作成を続行するには、広告主およびパートナーは、オーディエンスデータをアップロードする際にエンドユーザーの同意を渡していることを確認する必要があります。 Google パートナーとして、Adobeは、欧州連合の DMA に基づくこれらの同意要件に準拠するために必要なツールを提供します。
><br/>
>Adobeのプライバシーとセキュリティシールドを購入し、 [同意ポリシー](../../../data-governance/enforcement/auto-enforcement.md#consent-policy-evaluation) 同意されていないプロファイルを除外するには、何もする必要はありません。
><br/>
>Adobeプライバシーおよびセキュリティシールドを購入していないお客様は、を使用する必要があります [セグメント定義](../../../segmentation/home.md#segment-definitions) 内の機能 [セグメントビルダー](../../../segmentation/ui/segment-builder.md) 中断することなく既存のReal-Time CDP Google宛先を引き続き使用するために、同意されていないプロファイルを除外します。

[!DNL Display & Video 360]（旧称：） [!DNL DoubleClick Bid Manager]は、ディスプレイ、ビデオ、モバイルの各インベントリソースをまたいでリターゲティングやオーディエンスターゲティングしたデジタルキャンペーンを実行するために使用されるツールです。

## 宛先の詳細 {#specifics}

[!DNL Google Display & Video 360] の宛先に固有な次の詳細に注意ください。

* アクティブ化されたオーディエンスは、Google プラットフォームでプログラムを使用して作成されます。
* へのオーディエンスバックフィルのアクティブ化 [!DNL Google Display & Video 360] 宛先は、オーディエンスが最初に宛先接続にマッピングされてから 24～48 時間後に発生するようにスケジュールされています。 この更新は、データの取り込みまで 24 時間待機するというGoogleのポリシーに対応するものであり、Real-Time CDPとの間の一致率を向上させることを目的としています [!DNL Google Display & Video 360]. これは、この宛先にのみ適用されるバックエンド設定で、UI でお客様が設定可能なスケジュールオプションとは無関係です。

>[!IMPORTANT]
>
>Google ディスプレイ&amp;ビデオ 360 で最初の宛先を作成しようとしており、を有効にしていない場合は、 [ID 同期機能](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/methods/idsync.html?lang=ja) 過去のExperience CloudID サービス（Adobe Audience Managerなどを使用）で ID 同期を有効にするには、Adobeのコンサルティングまたはカスタマーケアにお問い合わせください。 以前に Audience Manager で Google 統合を設定していた場合、設定した ID 同期は Platform に引き継がれます。

## サポートされている ID {#supported-identities}

[!DNL Google Display & Video 360] では、以下の表に示す ID に基づいたオーディエンスのアクティベーションがサポートされています。 [ID](/help/identity-service/features/namespaces.md) についての詳細情報。

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
---------|----------|----------|
| [!DNL Segmentation Service] | ✓ | Experience Platformを通じて生成されたオーディエンス [セグメント化サービス](../../../segmentation/home.md). |
| カスタムアップロード | ✓ | CSV ファイルから Experience Platform に[読み込まれた](../../../segmentation/ui/overview.md#import-audience)オーディエンス。 |

{style="table-layout:auto"}

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL オーディエンスの書き出し]** | オーディエンスのすべてのメンバーを Google の宛先に書き出します。 |
| 書き出し頻度 | **[!UICONTROL ストリーミング]** | ストリーミングの宛先は常に、API ベースの接続です。オーディエンス評価に基づいて Experience Platform 内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。[ストリーミングの宛先](/help/destinations/destination-types.md#streaming-destinations)の詳細についてはこちらを参照してください。 |

## 前提条件 {#prerequisites}

### 許可リストへの登録 {#allow-listing}

>[!NOTE]
>
>Platform で最初の [!DNL Google Display & Video 360] の宛先を設定する前に、許可リストへの登録は必須です。が、以下に説明する許可リストへの登録プロセスを完了していることを確認します。 [!DNL Google] 宛先を作成する前に以下を行います。
>このルールの例外は、[Audience Manager](https://experienceleague.adobe.com/docs/audience-manager/user-guide/aam-home.html?lang=ja) 顧客の場合です。この Google の宛先への接続を Audience Manager で既に作成している場合は、許可リストへの登録プロセスを再度実行する必要はありません。次の手順に進んでください。

を作成する前に [!DNL Google Display & Video 360] platform の宛先ユーザーは、Googleに連絡して、許可されたデータプロバイダーのリストに含めるAdobeと、アカウントを許可リストに追加する権限を依頼する必要があります。 Google に連絡し、次の情報を提供します。

* **アカウント ID**：アドビの Google アカウント ID です。アカウント ID：87933855。
* **顧客 ID**：アドビの Google 顧客アカウント ID です。顧客 ID：89690775。
* **アカウントの種類**：**[!DNL Invite advertiser]** を使用して、Display &amp; Video 360 アカウントにある特定のブランドにのみオーディエンスを共有するか、**[!DNL Invite partner]** を使用して Display &amp; Video 360 アカウントのすべてのブランドにオーディエンスを共有します。

## 宛先への接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、 **[!UICONTROL 宛先の表示]** および **[!UICONTROL 宛先の管理]** [アクセス制御権限](/help/access-control/home.md#permissions). [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。

### 接続パラメーター {#parameters}

この宛先を[設定](../../ui/connect-destination.md)するとき、次の情報を指定する必要があります。

* **[!UICONTROL 名前]**：この宛先に希望する名前を入力します。
* **[!UICONTROL 説明]**：オプション。例えば、この宛先を使用しているキャンペーンを指定できます。
* **[!UICONTROL アカウントの種類]**：Google のアカウントに応じて、次のオプションを選択します。
   * Display &amp; Video 360 アカウントにある特定のブランドにのみオーディエンスを共有するには、`Invite Advertiser` を使用します。
   * Display &amp; Video 360 アカウントのすべてのブランドにオーディエンスを共有するには、`Invite Partner` を使用します。
* **[!UICONTROL アカウント ID]**：**[!DNL Invite partner]** または **[!DNL Invite advertiser]** のアカウント ID に Google アカウントの ID を入力します。通常、これは 6 桁または 7 桁の ID です。

>[!NOTE]
>
>の設定時 [!DNL Google Display & Video 360] 宛先、との連携 [!DNL Google Account Manager] またはAdobe担当者：お持ちのアカウントの種類を把握します。

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続の詳細の入力を終えたら「**[!UICONTROL 次へ]**」を選択します。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>データをアクティブ化するには、 **[!UICONTROL 宛先の表示]**, **[!UICONTROL 宛先のアクティブ化]**, **[!UICONTROL プロファイルの表示]**、および **[!UICONTROL セグメントの表示]** [アクセス制御権限](/help/access-control/home.md#permissions). [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。

この宛先にオーディエンスをアクティブ化する手順については、[ストリーミングオーディエンス書き出し宛先に対するオーディエンスデータのアクティブ化](../../ui/activate-segment-streaming-destinations.md)を参照してください。

## 書き出したデータ

データがに正常に [!DNL Google Display & Video 360] の宛先に書き出されたかどうかを確認するには、[!DNL Google Display & Video 360] アカウントを確認します。 アクティベーションに成功すると、オーディエンスがお使いのアカウントに入力されます。

## トラブルシューティング {#troubleshooting}

### 400 Bad Request エラーメッセージ {#bad-request}

この宛先を設定する際に、次のエラーが発生する場合があります。

`{"message":"Google Error: AuthorizationError.USER_PERMISSION_DENIED","code":"400 BAD_REQUEST"}`

このエラーは、カスタマーアカウントが準拠していない場合に発生します [前提条件](#prerequisites). この問題を修正するには、Googleに連絡して、自分のアカウントが許可リストに登録されていることを確認してください。