---
title: 宛先へのアカウントオーディエンスの有効化
type: Tutorial
description: 宛先に対してアカウントオーディエンスをアクティブ化する方法を学ぶ
badgeB2B: label="B2B edition" type="Informative" url=" https://experienceleague.adobe.com/docs/experience-platform/rtcdp/intro/rtcdp-intro/overview.html?lang=en#rtcdp-editions newtab=true"
badgeB2P: label="B2P エディション" type="Positive" url=" https://experienceleague.adobe.com/docs/experience-platform/rtcdp/intro/rtcdp-intro/overview.html?lang=en#rtcdp-editions newtab=true"
exl-id: ad69d0a8-bf5b-42ac-97a3-401eadda62cd
source-git-commit: 9f4ce2a3a8af72342683c859caa270662b161b7d
workflow-type: tm+mt
source-wordcount: '782'
ht-degree: 6%

---

# アカウントオーディエンスの有効化

>[!AVAILABILITY]
>
>宛先に対してアカウントオーディエンスをアクティブ化する機能は、Real-Time Customer Data Platformの [B2B](/help/rtcdp/overview.md#rtcdp-b2b) および [B2Person](/help/rtcdp/overview.md#rtcdp-b2p) エディションを購入する企業で利用できます。

この記事では、Adobe Experience Platformから目的の宛先に [&#x200B; アカウントオーディエンス &#x200B;](/help/segmentation/types/account-audiences.md) を書き出すために必要なワークフローについて説明します。

## サポートされる宛先 {#supported-destinations}

**[!UICONTROL Connections]**/**[!UICONTROL Destinations]** に移動し、「**[!UICONTROL Catalog]**」タブを選択します。 **[!UICONTROL Data types]** フィルターを使用し、「**[!UICONTROL Accounts]**」を選択して、アカウントオーディエンスのアクティブ化をサポートする宛先を確認します。 現在、アカウントオーディエンスの書き出しを使用できるのは、特定のクラウドストレージの宛先（[Amazon S3](/help/destinations/catalog/cloud-storage/amazon-s3.md)、[ADLS Gen 2](/help/destinations/catalog/cloud-storage/adls-gen2.md)、[Azure Blob Storage](/help/destinations/catalog/cloud-storage/azure-blob.md)、[Data Landing Zone](/help/destinations/catalog/cloud-storage/data-landing-zone.md) および [SFTP](/help/destinations/catalog/cloud-storage/sftp.md)）と、[Demandbase](/help/destinations/catalog/advertising/demandbase.md) および [&#x200B; （Companies） LinkedIn Matched Audiences](/help/destinations/catalog/social/linkedin-b2b.md) ストリーミング宛先のみです。

![&#x200B; アカウントオーディエンスをサポートする宛先。](/help/destinations/assets/ui/activate-account-audiences/data-types-filter.png)

## ビデオの概要

アカウントオーディエンスの作成とアクティブ化の概要、およびアカウントオーディエンスをアクティブ化する際にサポートされるユースケースについては、以下のビデオをご覧ください。

>[!VIDEO](https://video.tv.adobe.com/v/338252/?learn=on)

## 前提条件 {#prerequisites}

* ダウンストリームの宛先に対してアクティブ化するには、まず [&#x200B; アカウントプロファイル &#x200B;](/help/rtcdp/accounts/account-profile-overview.md) を取り込み [&#x200B; アカウントオーディエンス &#x200B;](/help/segmentation/types/account-audiences.md) を作成する必要があります。
* 宛先へのアカウントオーディエンスをアクティブ化するには、正常に宛先に接続する必要があります。 まだ接続していない場合は、[&#x200B; 宛先カタログ &#x200B;](../catalog/overview.md) に移動し、サポートされている宛先を参照し、使用する宛先を設定します。 詳しくは、[&#x200B; 宛先への接続 &#x200B;](./connect-destination.md) に関する UI チュートリアルを参照してください。

### 必要な権限 {#permissions}

アカウントオーディエンスをアクティブ化するには、**[!UICONTROL View Destinations]** および **[!UICONTROL Activate Destinations]** [&#x200B; アクセス制御権限 &#x200B;](/help/access-control/home.md#permissions) が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。

アカウントオーディエンスをアクティブ化するために必要な権限があることを確認するには、宛先カタログを参照します。 宛先に **[!UICONTROL Activate]** コントロールがある場合、適切な権限を持っています。

## 宛先の選択 {#select-destination}

データセットを書き出すことができる宛先を選択するには、次の手順に従います。

1. **[!UICONTROL Connections > Destinations]** に移動して、「**[!UICONTROL Catalog]**」タブを選択します。

   ![カタログコントロールがハイライト表示された「宛先カタログ」タブ](/help/destinations/assets/ui/export-datasets/catalog-tab.png)

1. データセットを書き出す宛先に対応するカードで「**[!UICONTROL Activate]**」を選択します。

>[!TIP]
>
>アカウントオーディエンスを書き出すことができる宛先は、カードの右上隅にあるアイコンで示されます。これは、以下で強調表示されている宛先と同様です。または、データタイプフィルターを使用して、アカウントオーディエンスを書き出すことができる宛先のみを表示することもできます [&#x200B; ページの上部に表示されます &#x200B;](#supported-destinations)。

![&#x200B; プロファイルオーディエンスを書き出すことができる Demandbase 宛先ページがハイライト表示されています。](/help/destinations/assets/ui/activate-account-audiences/demandbase-icon-activate-account-audiences.png)

1. **[!UICONTROL Data type Accounts]** を選択し、その後にデータセットを書き出す宛先接続を選択して、「**[!UICONTROL Next]**」を選択します。

>[!TIP]
> 
>アカウントオーディエンスをアクティブ化する新しい宛先を設定する場合は、「**[!UICONTROL Configure new destination]**」を選択して [&#x200B; 宛先に接続 &#x200B;](/help/destinations/ui/connect-destination.md) ワークフローをトリガーし、「[&#x200B; データタイプとしてアカウントを選択 &#x200B;](/help/destinations/ui/connect-destination.md#segment-activation-or-dataset-exports) を選択します。

![&#x200B; アカウントコントロールがハイライト表示された宛先アクティベーションワークフロー &#x200B;](/help/destinations/assets/ui/activate-account-audiences/activate-account-audiences-highlighted.png)

1. 次の節に進んで、書き出し用に [&#x200B; アカウントオーディエンスを選択 &#x200B;](#select-profile-audiences) します。

## アカウントオーディエンスの選択 {#select-account-audiences}

アカウントオーディエンス名の左側にあるチェックボックスを使用して、宛先に書き出すオーディエンスを選択し、「**[!UICONTROL Next]**」を選択します。 このビューには *アカウントオーディエンス* のみが表示され、他のオーディエンスタイプは表示されません。

![&#x200B; 書き出すアカウントオーディエンスを選択できる「オーディエンスを選択」ステップが表示されているデータセット書き出しワークフロー &#x200B;](/help/destinations/assets/ui/activate-account-audiences/select-account-audiences.png)

## スケジュール設定と次の手順

アカウントオーディエンスを書き出すための残りのアクティベーションワークフローについては、ファイルベースの宛先に対するデータのアクティブ化に関するチュートリアルを参照してください。 [&#x200B; オーディエンスの書き出しをスケジュール手順 &#x200B;](/help/destinations/ui/activate-batch-profile-destinations.md#scheduling) から続行します。 **[!UICONTROL (Companies) LinkedIn Matched Audiences]** の宛先に対してアカウントオーディエンスをアクティブ化する場合は、ストリーミング宛先のアクティブ化に関するチュートリアルを参照してください。 [&#x200B; マッピング手順 &#x200B;](/help/destinations/ui/activate-segment-streaming-destinations.md#mapping) から続行します。

>[!NOTE]
>
>アカウントオーディエンスをクラウドストレージの宛先に書き出す際のスケジュール設定ステップでは、アカウントオーディエンスをアクティブ化するワークフローでのみ [&#x200B; 完全なファイル &#x200B;](/help/destinations/ui/activate-batch-profile-destinations.md#export-full-files) および [&#x200B; 増分ファイル &#x200B;](/help/destinations/ui/activate-batch-profile-destinations.md#export-incremental-files) を毎日のスケジュールで _書き出すことができ_ す。 1 時間ごとの書き出しはサポートされていません。 また、サポートされている評価タイプは **[!UICONTROL After audience evaluation]** のみです。

## 重要なコールアウトと既知の制限事項 {#important-callouts-known-limitations}

アカウントオーディエンスをアクティブ化する機能の一般リリースについては、次の重要なコールアウトと既知の制限事項に注意してください。

### アカウントオーディエンスを **[!UICONTROL (Companies) LinkedIn Matched Audiences]** の宛先に対してアクティブ化する際に、マッピング手順で必要なマッピングペア {#required-mappings}

**[!UICONTROL (Companies) LinkedIn Matched Audiences]** の宛先に対してアカウントオーディエンスをアクティブ化する場合、データを正常に書き出すには、次の 2 つのマッピングペアが必須です。

![LinkedIn マッピングの必須フィールド。](/help/destinations/assets/ui/activate-account-audiences/linkedin-mapping-required-fields.png)

| ソースフィールド | ターゲットフィールド |
|---------|----------|
| `accountName` | `companyName` |
| `accountKey.sourceKey` | `primaryId` （**[!UICONTROL Select Identity namespace]** を選択する際に、**[!UICONTROL Target Field]** ビューでこのフィールドを選択します）。<br> ![&#x200B; 宛先に対してアカウントオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択。](/help/destinations/assets/ui/activate-account-audiences/identity-namespace-highlighted.png " 宛先に対してアカウントオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択 "){width="100" zoomable="yes"} |

### データガバナンスの施行 {#data-governance-enforcement}

同意は、（顧客および見込み客オーディエンス *の個人またはプロファイルレベルで適用さ* ます。 したがって、アカウントオーディエンスを宛先に対してアクティブ化する場合、[&#x200B; 同意ポリシーの評価 &#x200B;](/help/data-governance/enforcement/auto-enforcement.md#consent-policy-evaluation) は現在、サポートされていません。 アクティベーションワークフローのレビュー手順では、アク **[!UICONTROL View applicable consent policies]** ィベーション用のコントロールがグレー表示されています。

![&#x200B; 同意適用コントロールを含んだアカウントオーディエンスをアクティベート ワークフローのレビュー手順がグレー表示されている様子。](/help/destinations/assets/ui/activate-account-audiences/consent-checks-greyed-out.png)

[&#x200B; データ使用ポリシーチェック &#x200B;](/help/data-governance/enforcement/auto-enforcement.md#consent-policy-evaluation) および [&#x200B; 属性ベースのアクセス制御 &#x200B;](/help/destinations/home.md#attribute-based-access) など、Real-Time CDPのその他のデータガバナンスメカニズムもサポートされています。
