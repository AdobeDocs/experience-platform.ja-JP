---
title: （ベータ版） [!DNL Google Ad Manager 360] 接続
description: Google Ad Manager 360 は、媒体社がビデオやモバイルアプリを通じて web サイト上の広告の表示を管理できる、Google の広告配信プラットフォームです。
exl-id: 3251145a-3e4d-40aa-b120-d79c8c9c7cae
source-git-commit: c35b43654d31f0f112258e577a1bb95e72f0a971
workflow-type: tm+mt
source-wordcount: '1206'
ht-degree: 61%

---

# （ベータ版）[!DNL Google Ad Manager 360] 接続

>[!IMPORTANT]
>
> Googleは、欧州連合（EU）の [ デジタル市場法 ](https://developers.google.com/google-ads/api/docs/start) （DMA](https://digital-markets-act.ec.europa.eu/index_en)）（[EU ユーザー同意ポリシー ](https://www.google.com/about/company/user-consent-policy/)）で定義されているコンプライアンスおよび同意関連の要件をサポートするために、[Google Ads API](https://ads-developers.googleblog.com/2023/10/updates-to-customer-match-conversion.html)、[Customer Match および [Display &amp; Video 360 API](https://developers.google.com/display-video/api/guides/getting-started/overview) に対する変更内容をリリースしています。 同意要件に対するこれらの変更の適用は 2024 年 3 月 6 日（PT）から開始されます。
><br/>
>EU のユーザー同意ポリシーに準拠し、欧州経済領域（EEA）のユーザーに対するオーディエンスリストの作成を続行するには、広告主およびパートナーは、オーディエンスデータをアップロードする際にエンドユーザーの同意を渡していることを確認する必要があります。 Google パートナーとして、Adobeは、欧州連合の DMA に基づくこれらの同意要件に準拠するために必要なツールを提供します。
><br/>
>Adobeのプライバシーとセキュリティシールドを購入し、同意のないプロファイルを除外する [ 同意ポリシー ](../../../data-governance/enforcement/auto-enforcement.md#consent-policy-evaluation) を設定しているお客様は、何もする必要はありません。
><br/>
>Adobeのプライバシーとセキュリティシールドを購入されていないお客様が既存のReal-Time CDP Googleの宛先を引き続き中断なく使用するには、[ セグメントビルダー ](../../../segmentation/home.md#segment-definitions) 内の [ セグメント定義 ](../../../segmentation/ui/segment-builder.md) 機能を使用して、同意のないプロファイルを除外する必要があります。

[!DNL Google Ad Manager 360] 接続では、[!DNL Google Cloud Storage] を介して、[!DNL Google Ad Manager 360] への [!DNL publisher provided identifiers]（PPID）のバッチアップロードが可能です。

媒体社が提供した ID が Google Ad Manager 360 でどのように機能するかについて詳しくは、[公式 Google ドキュメント](https://support.google.com/admanager/answer/2880055?hl=ja)を参照してください。

>[!IMPORTANT]
>
>この宛先は現在ベータ版で、一部のお客様のみご利用いただけます。[!DNL Google Ad Manager 360] 接続へのアクセスをリクエストするには、アドビ担当者に連絡し、お使いの [!DNL organization ID] を提供します。

[!DNL Google Ad Manager 360] 宛先では、[!DNL CSV] ファイルをお使いの [!DNL Google Cloud Storage] バケットに書き出します。[!DNL CSV] ファイルを書き出したら、お使いの [!DNL Google Ad Manager 360] アカウントに読み込む必要があります。

## 宛先の詳細 {#specifics}

[!DNL Google Ad Manager 360] 宛先に固有の次の詳細事項に注意してください。

* アクティブ化されたオーディエンスは、Google プラットフォームでプログラムにより作成され、CSV ファイルに入力されます。

## サポートされる ID {#supported-identities}

[!DNL This integration] では、以下の表で説明する ID のアクティベーションをサポートしています。

| ターゲット ID | 説明 | 注意点 |
|---|---|---|
| PPID | [!DNL Publisher provided ID] | オーディエンスを [!DNL Google Ad Manager 360] に送信するには、このターゲット ID を選択します。 |

{style="table-layout:auto"}

## サポートされるオーディエンス {#supported-audiences}

この節では、この宛先に書き出すことができるオーディエンスのタイプについて説明します。

| オーディエンスオリジン | サポートあり | 説明 |
|---------|----------|----------|
| [!DNL Segmentation Service] | ✓ | Experience Platform[ セグメント化サービス ](../../../segmentation/home.md) を通じて生成されたオーディエンス。 |
| カスタムアップロード | ✓ | CSV ファイルから Experience Platform に[読み込まれた](../../../segmentation/ui/audience-portal.md#import-audience)オーディエンス。 |

{style="table-layout:auto"}

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL プロファイルベース]** | [宛先のアクティベーションワークフロー](/help/destinations/ui/activate-batch-profile-destinations.md#select-attributes)のプロファイル属性の選択画面で選択したように、該当するスキーマフィールド（例：PPID）と共に、セグメントのすべてのメンバーを書き出しています。 |
| 書き出し頻度 | **[!UICONTROL バッチ]** | バッチ宛先では、ファイルが 3 時間、6 時間、8 時間、12 時間、24 時間の単位でダウンストリームプラットフォームに書き出されます。 詳しくは、[バッチ（ファイルベース）宛先](/help/destinations/destination-types.md#file-based)を参照してください。 |

{style="table-layout:auto"}

## 前提条件 {#prerequisites}

### 許可リストへの登録 {#allow-listing}

Platform で最初の [!DNL Google Ad Manager 360] の宛先を設定する前に、許可リストへの登録は必須です。宛先を作成する前に、必ず以下に説明する許可リストへの登録プロセスを完了してください。

>[!NOTE]
>
>既存の [Audience Manager](https://experienceleague.adobe.com/docs/audience-manager/user-guide/aam-home.html?lang=ja) のお客様の場合は例外です。 この Google の宛先への接続を Audience Manager で既に作成している場合は、許可リストへの登録プロセスを再度実行する必要はありません。次の手順に進んでください。

1. Adobeをリンクされた Data Management Platform （DMP）として追加するには ](https://support.google.com/admanager/answer/3289669?hl=ja)[Google Ad Manager のドキュメント } に記載されている手順に従います。
2. [!DNL Google Ad Manager] インターフェイスで、**[!UICONTROL 管理者]** / **[!UICONTROL グローバル設定]** / **[!UICONTROL ネットワーク設定]** に移動し、**[!UICONTROL API アクセス]** スライダーを有効にします。


## 宛先への接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、**[!UICONTROL 宛先の表示]** および **[!UICONTROL 宛先の管理]**[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。 詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](https://experienceleague.adobe.com/docs/experience-platform/destinations/ui/connect-destination.html?lang=ja)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つの節でリストされているフィールドに入力します。

### 宛先に対する認証 {#authenticate}

宛先に対する認証を行うには、必須フィールドに入力し、「**[!UICONTROL 宛先に接続]**」を選択します。

* **[!UICONTROL アクセスキー ID]**：61 文字の英数字から成る文字列で、Platform に対する [!DNL Google Cloud Storage] アカウントの認証に使用します。
* **[!UICONTROL 秘密アクセスキー]**：Base64 でエンコードされた40 文字の文字列で、Platform に対する [!DNL Google Cloud Storage] アカウントの認証に使用します。

これらの値について詳しくは、[Google Cloud Storage の HMAC キー](https://cloud.google.com/storage/docs/authentication/hmackeys#overview)ガイドを参照してください。独自のアクセスキー ID と秘密アクセスキーを生成する手順については、[[!DNL Google Cloud Storage] ソースの概要](/help/sources/connectors/cloud-storage/google-cloud-storage.md)を参照してください。

### 宛先の詳細の入力 {#destination-details}

>[!CONTEXTUALHELP]
>id="platform_destinations_gam360_appendSegmentID"
>title="オーディエンス名へのオーディエンス ID の追加"
>abstract="この宛先のオーディエンス名に Experience Platform のオーディエンス ID を含める（`Audience Name (Audience ID)` など）には、このオプションを選択します。"

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横のアスタリスクは、そのフィールドが必須であることを示します。

* **[!UICONTROL 名前]**：この宛先に希望する名前を入力します。
* **[!UICONTROL 説明]**：オプション。例えば、この宛先を使用しているキャンペーンを指定できます。
* **[!UICONTROL フォルダーパス]**：書き出したファイルをホストする宛先フォルダーへのパス。
* **[!UICONTROL バケット名]**：この宛先が使用する [!DNL Google Cloud Storage] バケット名を入力します。
* **[!UICONTROL アカウント ID]**:[!DNL Google] アカウントから [!DNL Audience Link ID] を入力します。 これは、（[!DNL Network code] ではなく） [!DNL Google Ad Manager] ネットワークに関連付けられた特定の識別子です。 これは、[!DNL Google Ad Manager] インターフェイスの **[!UICONTROL 管理者/グローバル設定]** にあります。
* **[!UICONTROL アカウントタイプ]**:[!DNL Google] アカウントに応じて、次のいずれかのオプションを選択します。
   * [!DNL Google AdX] に `AdX buyer` を使用する
   * [!DNL DoubleClick] for Publishers に `DFP by Google` を使用する
* **[!UICONTROL オーディエンス ID をオーディエンス名に追加]**:Google Ad Manager 360 のオーディエンス名にExperience Platformのオーディエンス ID を含めるには、次のように、このオプションを選択します。`Audience Name (Audience ID)`。

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続の詳細の入力を終えたら「**[!UICONTROL 次へ]**」を選択します。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>* データをアクティブ化するには、**[!UICONTROL 宛先の表示]**、**[!UICONTROL 宛先のアクティブ化]**、**[!UICONTROL プロファイルの表示]** および **[!UICONTROL セグメントの表示]**[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。
>* *ID* を書き出すには、**[!UICONTROL ID グラフの表示]**[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。<br> ![ 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択します。](/help/destinations/assets/overview/export-identities-to-destination.png " 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択 "){width="100" zoomable="yes"}

この宛先に対してオーディエンスをアクティブ化する手順については、[ プロファイル書き出しのバッチ宛先に対するオーディエンスデータのアクティブ化 ](../../ui/activate-batch-profile-destinations.md) を参照してください。

ID マッピングステップでは、次の事前入力済みマッピングが表示されます。

| 事前入力済みマッピング | 説明 |
|---------|----------|
| `ECID` -> `ppid` | ユーザーによる編集が可能な事前入力済みのマッピングは、これだけです。Platform から任意の属性または ID 名前空間を選択し、それを `ppid` にマッピングすることができます。 |
| `metadata.segment.alias` -> `list_id` | Experience Platformオーディエンス名をGoogle Platform のオーディエンス ID にマッピングします。 |
| `iif(${segmentMembership.ups.seg_id.status}=="exited", "1","0")` -> `delete` | 不適格なユーザーをセグメントから削除するタイミングを Google プラットフォームに指示します。 |

これらのマッピングは [!DNL Google Ad Manager 360] で必要であり、すべての [!DNL Google Ad Manager 360] 接続に対してAdobe Experience Platform によって自動的に作成されます。

![Google Ad Manager 360 のマッピングステップを示す UI 画像](../../assets/catalog/advertising/google-ad-manager-360/ad-manager-360-mapping.png)

## 書き出したデータ {#exported-data}

データが正常に書き出されたかどうかを確認するには、[!DNL Google Cloud Storage] バケットを確認し、書き出されたファイルに想定どおりのプロファイル母集団が含まれていることを確認します。

## トラブルシューティング {#troubleshooting}

この宛先の使用中にエラーが発生し、AdobeまたはGoogleに連絡する必要がある場合は、次の ID を手元に用意しておいてください。

AdobeのGoogle アカウント ID です。

* **[!UICONTROL アカウント ID]**: 87933855
* **[!UICONTROL 顧客 ID]**: 89690775