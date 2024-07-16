---
title: 宛先へのアカウントオーディエンスの有効化
type: Tutorial
description: 宛先に対してアカウントオーディエンスをアクティブ化する方法を学ぶ
badgeB2B: label="B2B エディション" type="Informative" url=" https://experienceleague.adobe.com/docs/experience-platform/rtcdp/intro/rtcdp-intro/overview.html?lang=en#rtcdp-editions newtab=true"
badgeB2P: label="B2P エディション" type="Positive" url=" https://experienceleague.adobe.com/docs/experience-platform/rtcdp/intro/rtcdp-intro/overview.html?lang=en#rtcdp-editions newtab=true"
exl-id: ad69d0a8-bf5b-42ac-97a3-401eadda62cd
source-git-commit: dff460f0b0d365d3d643744544642d9f9488e18a
workflow-type: tm+mt
source-wordcount: '834'
ht-degree: 7%

---

# アカウントオーディエンスの有効化

>[!AVAILABILITY]
>
>宛先に対してアカウントオーディエンスをアクティブ化する機能は、Real-time Customer Data Platformの [B2B](/help/rtcdp/overview.md#rtcdp-b2b) および [B2Person](/help/rtcdp/overview.md#rtcdp-b2p) エディションを購入する企業で利用できます。

この記事では、Adobe Experience Platformから目的の宛先に [ アカウントオーディエンス ](/help/segmentation/ui/account-audiences.md) を書き出すために必要なワークフローについて説明します。

## サポートされる宛先 {#supported-destinations}

**[!UICONTROL 接続]**／**[!UICONTROL 宛先]**&#x200B;に移動し、「**[!UICONTROL カタログ]**」タブを選択します。**[!UICONTROL データタイプ]** フィルターを使用し、**[!UICONTROL アカウント]** を選択して、アカウントオーディエンスのアクティブ化をサポートする宛先を確認します。 現在、アカウントオーディエンスの書き出しは、特定のクラウドストレージの宛先（[Amazon S3](/help/destinations/catalog/cloud-storage/amazon-s3.md)、[ADLS Gen 2](/help/destinations/catalog/cloud-storage/adls-gen2.md)、[Azure Blob Storage](/help/destinations/catalog/cloud-storage/azure-blob.md)、[Data Landing Zone](/help/destinations/catalog/cloud-storage/data-landing-zone.md)、および [SFTP](/help/destinations/catalog/cloud-storage/sftp.md)）と、[ （企業）LinkedInでマッチしたオーディエンス ](/help/destinations/catalog/social/linkedin.md) の宛先でのみ使用できます。

![ アカウントオーディエンスをサポートする宛先。](/help/destinations/assets/ui/activate-account-audiences/data-types-filter.png)

## ビデオの概要

アカウントオーディエンスの作成とアクティブ化の概要、およびアカウントオーディエンスをアクティブ化する際にサポートされるユースケースについては、以下のビデオをご覧ください。

>[!VIDEO](https://video.tv.adobe.com/v/338252/?learn=on)

## 前提条件 {#prerequisites}

* ダウンストリームの宛先に対してアクティブ化するには、まず [ アカウントプロファイル ](/help/rtcdp/accounts/account-profile-overview.md) を取り込み [ アカウントオーディエンス ](/help/segmentation/ui/account-audiences.md) を作成する必要があります。
* 宛先へのアカウントオーディエンスをアクティブ化するには、正常に宛先に接続する必要があります。 まだ接続していない場合は、[ 宛先カタログ ](../catalog/overview.md) に移動し、サポートされている宛先を参照し、使用する宛先を設定します。 詳しくは、[ 宛先への接続 ](./connect-destination.md) に関する UI チュートリアルを参照してください。

### 必要な権限 {#permissions}

アカウントオーディエンスをアクティブ化するには、**[!UICONTROL 宛先の表示]** および **[!UICONTROL 宛先のアクティブ化]**[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。

アカウントオーディエンスをアクティブ化するために必要な権限があることを確認するには、宛先カタログを参照します。 宛先に **[!UICONTROL アクティブ化]** コントロールがある場合、適切な権限を持っています。

## 宛先の選択 {#select-destination}

データセットを書き出すことができる宛先を選択するには、次の手順に従います。

1. **[!UICONTROL 接続／宛先]**&#x200B;に移動し、「**[!UICONTROL カタログ]**」タブを選択します。

   ![カタログコントロールがハイライト表示された「宛先カタログ」タブ](/help/destinations/assets/ui/export-datasets/catalog-tab.png)

1. データセットを書き出す宛先に対応するカードで「**[!UICONTROL アクティブ化]**」を選択します。

>[!TIP]
>
>アカウントオーディエンスを書き出すことができる宛先は、カードの右上隅にあるアイコンで示されます。これは、以下で強調表示されている宛先と同様です。または、データタイプフィルターを使用して、アカウントオーディエンスを書き出すことができる宛先のみを表示することもできます [ ページの上部に表示されます ](#supported-destinations)。

![ ハイライト表示されたプロファイルオーディエンスを書き出すことができるAmazon S3 の宛先ページ ](/help/destinations/assets/ui/activate-account-audiences/amazon-s3-icon-activate-account-audiences.png)

1. **[!UICONTROL データタイプ アカウント]** を選択し、続いてデータセットを書き出す宛先接続を選択して、「**[!UICONTROL 次へ]**」を選択します。

>[!TIP]
> 
>アカウントオーディエンスをアクティブ化する新しい宛先を設定する場合は、「**[!UICONTROL 新しい宛先を設定]**」を選択して [ 宛先に接続 ](/help/destinations/ui/connect-destination.md) ワークフローをトリガーにし、[ データタイプとしてアカウントを選択 ](/help/destinations/ui/connect-destination.md#segment-activation-or-dataset-exports) します。

![ アカウントコントロールがハイライト表示された宛先アクティベーションワークフロー ](/help/destinations/assets/ui/activate-account-audiences/activate-account-audiences-highlighted.png)

1. 次の節に進んで、書き出し用に [ アカウントオーディエンスを選択 ](#select-profile-audiences) します。

## アカウントオーディエンスの選択 {#select-account-audiences}

アカウントオーディエンス名の左側にあるチェックボックスを使用して、宛先に書き出すオーディエンスを選択し、「**[!UICONTROL 次へ]**」を選択します。 このビューには *アカウントオーディエンス* のみが表示され、他のオーディエンスタイプは表示されません。

![ 書き出すアカウントオーディエンスを選択できる「オーディエンスを選択」ステップが表示されているデータセット書き出しワークフロー ](/help/destinations/assets/ui/activate-account-audiences/select-account-audiences.png)

## スケジュール設定と次の手順

アカウントオーディエンスを書き出すための残りのアクティベーションワークフローについては、ファイルベースの宛先に対するデータのアクティブ化に関するチュートリアルを参照してください。 [ オーディエンスの書き出しをスケジュール手順 ](/help/destinations/ui/activate-batch-profile-destinations.md#scheduling) から続行します。 **[!UICONTROL （会社）LinkedInでマッチしたオーディエンスの宛先に対してアカウントオーディエンスをアクティブ化する場合は]** ストリーミングの宛先のアクティブ化に関するチュートリアルを参照してください。 [ マッピング手順 ](/help/destinations/ui/activate-segment-streaming-destinations.md#mapping) から続行します。

>[!NOTE]
>
>アカウントオーディエンスをクラウドストレージの宛先に書き出す際のスケジュール設定ステップでは、アカウントオーディエンスをアクティブ化するワークフローでのみ [ 完全なファイル ](/help/destinations/ui/activate-batch-profile-destinations.md#export-full-files) および [ 増分ファイル ](/help/destinations/ui/activate-batch-profile-destinations.md#export-incremental-files) を毎日のスケジュールで _書き出すことができ_ す。 1 時間ごとの書き出しはサポートされていません。 また、サポートされている評価タイプは **[!UICONTROL オーディエンスの評価後]** のみです。

## 重要なコールアウトと既知の制限事項 {#important-callouts-known-limitations}

アカウントオーディエンスをアクティブ化する機能の一般リリースについては、次の重要なコールアウトと既知の制限事項に注意してください。

### **[!UICONTROL （会社）LinkedIn Matched Audiences]** 宛先に対してアカウントオーディエンスをアクティブ化する際に、マッピングステップで必要なマッピングペア {#required-mappings}

**[!UICONTROL （会社）LinkedIn一致オーディエンス]** 宛先に対してアカウントオーディエンスをアクティブ化する場合、データを正常に書き出すには、次の 2 つのマッピングペアが必須です。

![LinkedIn マッピングの必須フィールド。](/help/destinations/assets/ui/activate-account-audiences/linkedin-mapping-required-fields.png)

| ソースフィールド | ターゲットフィールド |
|---------|----------|
| `accountName` | `companyName` |
| `accountKey.sourceKey` | `primaryId` （**[!UICONTROL ターゲットフィールド]** を選択する際に、**[!UICONTROL ID 名前空間を選択]** ビューでこのフィールドを選択します）。<br> ![ 宛先に対してアカウントオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択。](/help/destinations/assets/ui/activate-account-audiences/identity-namespace-highlighted.png " 宛先に対してアカウントオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択 "){width="100" zoomable="yes"} |

### データガバナンスの施行 {#data-governance-enforcement}

同意は、（顧客および見込み客オーディエンス *の個人またはプロファイルレベルで適用さ* ます。 したがって、アカウントオーディエンスを宛先に対してアクティブ化する場合、[ 同意ポリシーの評価 ](/help/data-governance/enforcement/auto-enforcement.md#consent-policy-evaluation) は現在、サポートされていません。 アクティベーションワークフローのレビュー手順で、「適用可能な同意ポリシーを表示 **[!UICONTROL に関するコントロールがグレー表示され]** います。

![ 同意適用コントロールを含んだアカウントオーディエンスをアクティベート ワークフローのレビュー手順がグレー表示されている様子。](/help/destinations/assets/ui/activate-account-audiences/consent-checks-greyed-out.png)

[ データ使用ポリシーチェック ](/help/data-governance/enforcement/auto-enforcement.md#consent-policy-evaluation) および [ 属性ベースのアクセス制御 ](/help/destinations/home.md#attribute-based-access) など、Real-Time CDPのその他のデータガバナンスメカニズムもサポートされています。
