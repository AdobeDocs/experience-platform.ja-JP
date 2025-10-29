---
keywords: google アドマネージャー;google 広告;ダブルクリック;DoubleClick AdX;DoubleClick;Google アドマネージャー;Google ad manager;DFP
title: Google Ad Manager の接続
description: Google Ad Manager（以前は DoubleClick for Publishers または DoubleClick AdX と呼ばれていました）は Google の広告提供プラットフォームです。パブリッシャーはビデオやモバイルアプリを通じて、web サイト上の広告の表示を管理することができます。
exl-id: e93f1bd5-9d29-43a1-a9a6-8933f9d85150
source-git-commit: 1b507e9846a74b7ac2d046c89fd7c27a818035ba
workflow-type: tm+mt
source-wordcount: '1077'
ht-degree: 56%

---

# [!DNL Google Ad Manager] 接続

>[!IMPORTANT]
>
> Googleは、欧州連合（EU）の [ デジタル市場法 ](https://developers.google.com/google-ads/api/docs/start) （DMA[）（](https://ads-developers.googleblog.com/2023/10/updates-to-customer-match-conversion.html)EU ユーザー同意ポリシー [）で定義されているコンプライアンスおよび同意関連の要件をサポートするために、](https://developers.google.com/display-video/api/guides/getting-started/overview)Google Ads API[、](https://digital-markets-act.ec.europa.eu/index_en)Customer Match および [Display &amp; Video 360 API](https://www.google.com/about/company/user-consent-policy/) に対する変更内容をリリースしています。 同意要件に対するこれらの変更の適用は 2024 年 3 月 6 日（PT）から開始されます。
> ><br/>
> >EU のユーザー同意ポリシーに準拠し、欧州経済領域（EEA）のユーザーに対するオーディエンスリストの作成を続行するには、広告主およびパートナーは、オーディエンスデータをアップロードする際にエンドユーザーの同意を渡していることを確認する必要があります。 Google パートナーであるAdobeは、欧州連合の DMA に基づく同意要件に準拠するために必要なツールを提供します。
> ><br/>
> >Adobe Privacy &amp; Security Shield を購入し、同意のないプロファイルを除外する [ 同意ポリシー ](../../../data-governance/enforcement/auto-enforcement.md#consent-policy-evaluation) を設定している場合は、何もする必要はありません。
> ><br/>
> >Adobe Privacy &amp; Security Shield を購入されていないお客様が既存のReal-Time CDP Googleの宛先を引き続き中断することなく使用するには、[ セグメントビルダー ](../../../segmentation/home.md#segment-definitions) 内の [ セグメント定義 ](../../../segmentation/ui/segment-builder.md) 機能を使用して、同意のないプロファイルを除外する必要があります。


[!DNL Google Ad Manager]（以前は [!DNL DoubleClick for Publishers]（DFP）または [!DNL DoubleClick AdX] と呼ばれていました）は、[!DNL Google] の広告提供プラットフォームです。パブリッシャーはビデオやモバイルアプリを通じて、web サイト上の広告の表示を管理できます。

## 宛先の詳細 {#specifics}

[!DNL Google Ad Manager] の宛先に固有な次の詳細に注意ください。

* アクティブ化されたオーディエンスは、[!DNL Google] プラットフォームでプログラム的に作成されます。
* [!DNL Experience Platform] には、現在、アクティベーションの成功を検証するための測定指標は含まれていません。統合を検証し、オーディエンスのターゲティングサイズについて理解するには、Google でのオーディエンス数を参照します。
* オーディエンスを [!DNL Google Ad Manager] の宛先にマッピングすると、オーディエンス名が即座に [!DNL Google Ad Manager] ユーザーインターフェイスに表示されます。
* セグメント母集団が [!DNL Google Ad Manager] で表示されるまで、24～48 時間かかります。また、オーディエンスを [!DNL Google Ad Manager] で表示するには、オーディエンスサイズが 50 以上のプロファイルにする必要があります。 サイズが 50 未満のプロファイルを含むオーディエンスは、[!DNL Google Ad Manager] に入力されません。

## サポートされる ID {#supported-identities}

[!DNL Google Ad Manager] では、以下の表に示す ID に基づいたオーディエンスのアクティベーションをサポートしています。 [ID](/help/identity-service/features/namespaces.md) についての詳細情報。

| ID | 説明 | 注意点 |
|---|---|---|
| GAID | [!DNL Google Advertising ID] |  |
| IDFA | [!DNL Apple ID for Advertisers] |  |
| AAM UUID | [Adobe Audience Manager [!DNL Unique User ID]](https://experienceleague.adobe.com/docs/audience-manager/user-guide/reference/ids-in-aam.html?lang=ja)、別名 [!DNL Device ID]。数値型で 38 桁のデバイス ID。Audience Manager はこの値を、操作するデバイスのそれぞれに関連付けます。 | Google は [AAM UUID](https://experienceleague.adobe.com/docs/audience-manager/user-guide/reference/ids-in-aam.html?lang=ja) を使用して、カリフォルニア州のユーザーをターゲット設定し、他のすべてのユーザーに対して Google Cookie ID を使用します。 |
| [!DNL Google] Cookie ID | [!DNL Google] Cookie ID | [!DNL Google] は、この ID を使用してカリフォルニア州以外のユーザーをターゲットします。 |
| RIDA | 広告用 Roku ID。 この ID は、Roku デバイスを一意に識別します。 |  |
| MAID | Microsoft Advertising ID。この ID は、Windows 10 を実行しているデバイスを一意に識別します。 |  |
| Amazon Fire TV ID | この ID は、Amazon Fire TV を一意に識別します。 |  |

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
|---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL Audience export]** | オーディエンスのすべてのメンバーを Google の宛先に書き出します。 |
| 書き出し頻度 | **[!UICONTROL Streaming]** | ストリーミングの宛先は常に、API ベースの接続です。オーディエンス評価に基づいて Experience Platform 内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。[ストリーミングの宛先](/help/destinations/destination-types.md#streaming-destinations)の詳細についてはこちらを参照してください。 |

{style="table-layout:auto"}

## 前提条件 {#prerequisites}

[!DNL Google Ad Manager] での最初の宛先を作成しようとしており、これまで（Audience Manager などのアプリケーションを使用して）Experience Cloud ID サービスで [ID 同期機能](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/methods/idsync.html?lang=ja)を有効にしたことがない場合は、アドビのコンサルティングまたはカスタマーケアに連絡して ID 同期を有効にしてもらってください。以前にAudience Managerで [!DNL Google] 統合を設定していた場合、設定した ID 同期はExperience Platformに引き継がれます。

### 許可リストへの登録 {#allow-listing}

Experience Platformで最初の [!DNL Google Ad Manager] ール先を設定する前に、許可リストへの登録は必須です。 宛先を作成する前に、必ず以下に説明する許可リストへの登録プロセスを完了してください。

1. Adobeをリンクされた Data Management Platform （DMP）として追加するには [](https://support.google.com/admanager/answer/3289669?hl=ja)Google Ad Manager のドキュメント } に記載されている手順に従います。
2. [!DNL Google Ad Manager] インターフェイスで、**[!UICONTROL Admin]**/**[!UICONTROL Global Settings]**/**[!UICONTROL Network Settings]** に移動し、**[!UICONTROL API Access]** スライダーを有効にします。

## 宛先への接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、**[!UICONTROL View Destinations]** および **[!UICONTROL Manage Destinations]**[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。

### 接続パラメーター {#parameters}

>[!CONTEXTUALHELP]
>id="platform_destinations_gam_appendSegmentID"
>title="オーディエンス名へのオーディエンス ID の追加"
>abstract="Google アドマネージャーのオーディエンス名に Experience Platform のオーディエンス ID を含めるには、`Audience Name (Audience ID)` のように、このオプションを選択します。"

この宛先を[設定](../../ui/connect-destination.md)するとき、次の情報を指定する必要があります。

* **[!UICONTROL Name]**：この宛先に希望する名前を入力します。
* **[!UICONTROL Description]**：オプション。例えば、この宛先を使用しているキャンペーンを指定できます。
* **[!UICONTROL Account ID]**: [!DNL Audience Link ID] アカウントから [!DNL Google] を入力してください。 これは、（[!DNL Google Ad Manager] ではなく） [!DNL Network code] ネットワークに関連付けられた特定の識別子です。 これは、**[!UICONTROL Admin > Global settings]** インターフェイスの [!DNL Google Ad Manager] の下にあります。
* **[!UICONTROL Account Type]**:Googleのアカウントに応じて、次のいずれかのオプションを選択します。
   * [!DNL DoubleClick] for Publishers に `DFP by Google` を使用する
   * [!DNL Google AdX] に `AdX buyer` を使用する
* **[!UICONTROL Append audience ID to audience name]**: Google Ad Manager のオーディエンス名にExperience Platformのオーディエンス ID を含めるには、次のように、このオプションを選択します。`Audience Name (Audience ID)`

>[!NOTE]
>
>[!DNL Google Ad Manager] の宛先を設定する際は、[!DNL Google Account Manager] またはアドビの担当者にお問い合わせの上、お持ちのアカウントの種類をご確認ください。

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続への詳細の入力を終えたら「**[!UICONTROL Next]**」を選択します。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>データをアクティブ化するには、**[!UICONTROL View Destinations]**、**[!UICONTROL Activate Destinations]**、**[!UICONTROL View Profiles]**、**[!UICONTROL View Segments]** [ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。

この宛先にオーディエンスをアクティブ化する手順については、[ストリーミングオーディエンス書き出し宛先に対するオーディエンスデータのアクティブ化](../../ui/activate-segment-streaming-destinations.md)を参照してください。

## 書き出したデータ {#exported-data}

データがに正常に [!DNL Google Ad Manager] の宛先に書き出されたかどうかを確認するには、[!DNL Google Ad Manager] アカウントを確認します。 アクティベーションに成功すると、オーディエンスがお使いのアカウントに入力されます。

## トラブルシューティング {#troubleshooting}

この宛先の使用中にエラーが発生し、AdobeまたはGoogleに連絡する必要がある場合は、次の ID を手元に用意しておいてください。

以下は、AdobeのGoogle アカウント ID です。

* **[!UICONTROL Account ID]**: 87933855
* **[!UICONTROL Customer ID]**: 89690775