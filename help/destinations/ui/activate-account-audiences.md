---
title: 宛先へのアカウントオーディエンスのアクティブ化
type: Tutorial
description: 宛先に対してアカウントオーディエンスをアクティブ化する方法を説明します。
badgeLimitedAvailability: label="限定提供（LA）" type="Caution"
badgeB2B: label="B2B エディション" type="Informative" url="https://helpx.adobe.com/legal/product-descriptions/real-time-customer-data-platform-b2b-edition-prime-and-ultimate-packages.html newtab=true"
exl-id: ad69d0a8-bf5b-42ac-97a3-401eadda62cd
source-git-commit: 0a572c5fe612b8e0cc866b4e2287ea53a4022b1a
workflow-type: tm+mt
source-wordcount: '834'
ht-degree: 10%

---

# アカウントオーディエンスを有効化

>[!AVAILABILITY]
>
>宛先に対してアカウントオーディエンスをアクティブ化する機能は、 [B2B エディションオブReal-time Customer Data Platform](../../rtcdp/b2b-overview.md). また、アカウントオーディエンス機能は、現在、 **限られた可用性**. AdobeカスタマーケアまたはAdobe担当者に問い合わせて、この機能へのアクセスをリクエストしてください。

この記事では、エクスポートに必要なワークフローについて説明します。 [アカウントオーディエンス](/help/segmentation/ui/account-audiences.md) Adobe Experience Platformから目的の宛先に移動します。

## サポートされる宛先 {#supported-destinations}

**[!UICONTROL 接続]**／**[!UICONTROL 宛先]**&#x200B;に移動し、「**[!UICONTROL カタログ]**」タブを選択します。以下を使用します。 **[!UICONTROL データタイプ]** フィルターと選択 **[!UICONTROL アカウント]** アカウントオーディエンスのアクティブ化をサポートする宛先を表示する。 現在、アカウントオーディエンスの書き出しは、特定のクラウドストレージの宛先 ([Amazon S3](/help/destinations/catalog/cloud-storage/amazon-s3.md), [ADLS Gen 2](/help/destinations/catalog/cloud-storage/adls-gen2.md), [Azure Blob ストレージ](/help/destinations/catalog/cloud-storage/azure-blob.md), [データランディングゾーン](/help/destinations/catalog/cloud-storage/data-landing-zone.md)、および [SFTP](/help/destinations/catalog/cloud-storage/sftp.md)) および [（会社） LinkedIn Matched Audiences](/help/destinations/catalog/social/linkedin.md) 宛先。

![アカウントオーディエンスをサポートする宛先。](/help/destinations/assets/ui/activate-account-audiences/data-types-filter.png)

## 前提条件 {#prerequisites}

* 最初に取り込む必要があります [アカウントプロファイル](/help/rtcdp/accounts/account-profile-overview.md) を作成します。 [アカウントオーディエンス](/help/segmentation/ui/account-audiences.md) これらをダウンストリームの宛先に対してアクティブ化する前に、次の手順を実行します。
* アカウントオーディエンスを宛先に対してアクティブ化するには、宛先に正常に接続している必要があります。 まだ接続していない場合は、[宛先カタログ](../catalog/overview.md)に移動し、サポートされている宛先を参照し、使用する宛先を設定します。に関する UI チュートリアルをお読みください。 [宛先への接続](./connect-destination.md) を参照してください。

### 必要な権限 {#permissions}

アカウントオーディエンスをアクティブ化するには、 **[!UICONTROL 宛先の管理]**, **[!UICONTROL 宛先の表示]**, **[!UICONTROL 宛先のアクティブ化]** [アクセス制御権限](/help/access-control/home.md#permissions). [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。

アカウントオーディエンスをアクティブ化するために必要な権限を持っていることを確認するには、宛先カタログを参照します。 宛先に **[!UICONTROL 有効化]** コントロールを使用する場合、適切な権限を持っています。

## 宛先の選択 {#select-destination}

データセットを書き出すことができる宛先を選択するには、次の手順に従います。

1. **[!UICONTROL 接続／宛先]**&#x200B;に移動し、「**[!UICONTROL カタログ]**」タブを選択します。

   ![カタログコントロールがハイライト表示された「宛先カタログ」タブ](/help/destinations/assets/ui/export-datasets/catalog-tab.png)

1. 選択 **[!UICONTROL 有効化]** を、データセットの書き出し先の宛先に対応するカードに貼り付けます。

>[!TIP]
>
>アカウントオーディエンスをエクスポートできる宛先は、カードの右上隅にアイコンで示されます。例えば、以下で強調表示されている宛先に似ています。また、データタイプフィルターを使用して、アカウントオーディエンスをエクスポートできる宛先のみを表示できます。 [ページの上に表示される](#supported-destinations).

![ハイライト表示されたプロファイルオーディエンスをエクスポートできるAmazon S3 の宛先ページ。](/help/destinations/assets/ui/activate-account-audiences/amazon-s3-icon-activate-account-audiences.png)

1. 選択 **[!UICONTROL データタイプアカウント]**&#x200B;に続いて、データセットの書き出し先の接続を選択し、「 」を選択します。 **[!UICONTROL 次へ]**.

>[!TIP]
> 
>アカウントオーディエンスをアクティブ化する新しい宛先を設定する場合は、「 **[!UICONTROL 新しい宛先の設定]** トリガーする [宛先に接続](/help/destinations/ui/connect-destination.md) ワークフローと [データタイプとしてアカウントを選択](/help/destinations/ui/connect-destination.md#segment-activation-or-dataset-exports).

![アカウント制御が強調表示された宛先のアクティベーションワークフロー。](/help/destinations/assets/ui/activate-account-audiences/activate-account-audiences-highlighted.png)

1. 次のセクションに進み、 [アカウントオーディエンスを選択](#select-profile-audiences) エクスポート用。

## アカウントオーディエンスを選択 {#select-account-audiences}

アカウントオーディエンス名の左側にあるチェックボックスを使用して、宛先にエクスポートするオーディエンスを選択してから、「 」を選択します。 **[!UICONTROL 次へ]**. 注意： *アカウントオーディエンス* がこのビューに表示され、他のオーディエンスタイプは表示されません。

![オーディエンスの選択手順を表示するデータセットエクスポートワークフロー。この手順では、エクスポートするアカウントオーディエンスを選択できます。](/help/destinations/assets/ui/activate-account-audiences/select-account-audiences.png)

## スケジュールおよび次の手順

残りのアクティベーションワークフローでアカウントオーディエンスをエクスポートする方法については、ファイルベースの宛先へのデータのアクティブ化に関するチュートリアルを参照してください。 次から続行： [オーディエンスの書き出し手順をスケジュール](/help/destinations/ui/activate-batch-profile-destinations.md#scheduling). アカウントオーディエンスを **[!UICONTROL （会社） LinkedIn Matched Audiences]** の宛先については、ストリーミングの宛先のアクティブ化に関するチュートリアルを参照してください。 次から続行： [マッピング手順](/help/destinations/ui/activate-segment-streaming-destinations.md#mapping).

>[!NOTE]
>
>アカウントオーディエンスをクラウドストレージの宛先にエクスポートする場合のスケジュール設定手順では、アカウントオーディエンスをアクティブ化するワークフローでのみ、 [完全なファイル](/help/destinations/ui/activate-batch-profile-destinations.md#export-full-files) および [増分ファイル](/help/destinations/ui/activate-batch-profile-destinations.md#export-incremental-files) _毎日のスケジュールで_. 時間別の書き出しはサポートされていません。 また、 **[!UICONTROL オーディエンス評価後]** は、サポートされる唯一の評価タイプです。

## 重要な注意事項と既知の制限事項 {#important-callouts-known-limitations}

アカウントオーディエンスをアクティブ化する機能の限定提供リリースについては、次の重要な注意事項と既知の制限事項に注意してください。

### アカウントオーディエンスを **[!UICONTROL （会社） LinkedIn Matched Audiences]** 宛先 {#required-mappings}

アカウントオーディエンスを **[!UICONTROL （会社） LinkedIn Matched Audiences]** の宛先では、データを正常に書き出すには、次の 2 つのマッピングペアが必須です。

![LinkedInの必須フィールドのマッピング。](/help/destinations/assets/ui/activate-account-audiences/linkedin-mapping-required-fields.png)

| ソースフィールド | ターゲットフィールド |
|---------|----------|
| `accountName` | `companyName` |
| `accountKey.sourceKey` | `primaryId` ( このフィールドを **[!UICONTROL ID 名前空間を選択]** 表示、選択時 **[!UICONTROL ターゲットフィールド]**) をクリックします。 <br> ![ワークフローでハイライト表示された ID 名前空間を選択して、宛先に対するアカウントオーディエンスをアクティブ化します。](/help/destinations/assets/ui/activate-account-audiences/identity-namespace-highlighted.png "ワークフローでハイライト表示された ID 名前空間を選択して、宛先に対するアカウントオーディエンスをアクティブ化します。"){width="100" zoomable="yes"} |

### データガバナンスの実施 {#data-governance-enforcement}

同意は、次の個人レベルまたはプロファイルレベルで適用されます： *顧客および見込み客のオーディエンス*. したがって  [同意ポリシーの評価](/help/data-governance/enforcement/auto-enforcement.md#consent-policy-evaluation) は、現在、宛先に対してアカウントオーディエンスをアクティブ化する際にはサポートされません。 アクティベーションワークフローのレビューステップで、次の項目に対して灰色表示になっているコントロールを表示できます。 **[!UICONTROL 該当する同意ポリシーの表示]**.

![同意実施コントロールが灰色表示になっているアカウントオーディエンス有効化ワークフローの手順を確認します。](/help/destinations/assets/ui/activate-account-audiences/consent-checks-greyed-out.png)

Real-Time CDPのその他のデータガバナンスメカニズム ( [データ使用ポリシーのチェック](/help/data-governance/enforcement/auto-enforcement.md#consent-policy-evaluation) および [属性ベースのアクセス制御](/help/destinations/home.md#attribute-based-access) はサポートされています。
