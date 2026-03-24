---
title: 配信先でアカウントオーディエンスを活用
type: Tutorial
description: 宛先にアカウントオーディエンスをアクティブ化する方法について説明します
exl-id: ad69d0a8-bf5b-42ac-97a3-401eadda62cd
source-git-commit: d946d3dbb09c1fe0163fba3a892b4c0f1b331f87
workflow-type: tm+mt
source-wordcount: '762'
ht-degree: 6%

---

# アカウントオーディエンスのアクティベーション

>[!AVAILABILITY]
>
>宛先にアカウントオーディエンスをアクティブ化する機能は、[の](/help/rtcdp/overview.md#rtcdp-b2b)Business-to-Business[および](/help/rtcdp/overview.md#rtcdp-b2p)Business-to-Person[!DNL Real-Time Customer Data Platform] エディションを購入する企業で使用できます。

この記事では、[ アカウントオーディエンス ](/help/segmentation/types/account-audiences.md)を[!DNL Adobe Experience Platform]から好みの宛先にエクスポートするために必要なワークフローについて説明します。

## サポートされる宛先 {#supported-destinations}

**[!UICONTROL Connections]** > **[!UICONTROL Destinations]**&#x200B;に移動し、「**[!UICONTROL Catalog]**」タブを選択します。 **[!UICONTROL Data types]** フィルターを使用して&#x200B;**[!UICONTROL Accounts]**&#x200B;を選択し、アカウントオーディエンスのアクティブ化をサポートする宛先を表示します。 現在、アカウントオーディエンスの書き出しは、特定のクラウドストレージ宛先（[Amazon S3](/help/destinations/catalog/cloud-storage/amazon-s3.md)、[ADLS Gen 2](/help/destinations/catalog/cloud-storage/adls-gen2.md)、[Azure Blob Storage](/help/destinations/catalog/cloud-storage/azure-blob.md)、[ データランディングゾーン ](/help/destinations/catalog/cloud-storage/data-landing-zone.md)、[SFTP](/help/destinations/catalog/cloud-storage/sftp.md)）および[Bombora](/help/destinations/catalog/advertising/bombora.md)、[Demandbase](/help/destinations/catalog/advertising/demandbase.md)、および[ （Companies） LinkedIn Matched Audiences](/help/destinations/catalog/social/linkedin-b2b.md) ストリーミング宛先でのみ使用できます。

![ アカウントオーディエンスをサポートする宛先。](/help/destinations/assets/ui/activate-account-audiences/data-types-filter.png)

## ビデオの概要 {#video-overview}

アカウントオーディエンスの作成とアクティブ化の概要と、アカウントオーディエンスのアクティブ化でサポートされるユースケースについては、次のビデオをご覧ください。

>[!VIDEO](https://video.tv.adobe.com/v/338252/?learn=on)

## 前提条件 {#prerequisites}

* ダウンストリームの宛先にアクティブ化する前に、最初に[ アカウントプロファイル ](/help/rtcdp/accounts/account-profile-overview.md)を取り込み、[ アカウントオーディエンス ](/help/segmentation/types/account-audiences.md)を作成する必要があります。
* 宛先に対してアカウントオーディエンスをアクティブ化するには、宛先に正常に接続している必要があります。 まだ実行していない場合は、[宛先カタログ ](../catalog/overview.md)に移動し、サポートされている宛先を参照して、使用する宛先を設定します。 詳しくは、[宛先への接続](./connect-destination.md)に関するUI チュートリアルを参照してください。

### 必要な権限 {#permissions}

アカウント オーディエンスをアクティブ化するには、**[!UICONTROL View Destinations]**&#x200B;および&#x200B;**[!UICONTROL Activate Destinations]** [ アクセス制御権限](/help/access-control/home.md#permissions)が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。

アカウントオーディエンスをアクティブ化するために必要な権限を持っていることを確認するには、宛先カタログを参照します。 宛先に&#x200B;**[!UICONTROL Activate]** コントロールがある場合は、適切な権限を持っています。

## 宛先の選択 {#select-destination}

データセットを書き出すことができる宛先を選択するには、次の手順に従います。

1. **[!UICONTROL Connections > Destinations]**&#x200B;に移動し、「**[!UICONTROL Catalog]**」タブを選択します。

   ![カタログコントロールがハイライト表示された「宛先カタログ」タブ](/help/destinations/assets/ui/export-datasets/catalog-tab.png)

1. データセットの書き出し先に対応するカードで&#x200B;**[!UICONTROL Activate]**&#x200B;を選択します。

>[!TIP]
>
>アカウントオーディエンスを書き出すことができる宛先は、カードの右上隅に、下に強調表示されている宛先と同様のアイコンで示されます。または、データタイプフィルターを使用して、アカウントオーディエンスを書き出すことができる宛先のみを表示できます。例えば、ページ [の上位](#supported-destinations)に表示されます。

ハイライト表示されたプロファイルオーディエンスを書き出すことができる![Demandbase宛先ページ。](/help/destinations/assets/ui/activate-account-audiences/demandbase-icon-activate-account-audiences.png)

1. **[!UICONTROL Data type Accounts]**&#x200B;を選択し、データセットを書き出す宛先接続を選択してから、**[!UICONTROL Next]**&#x200B;を選択します。

>[!TIP]
> 
>アカウント オーディエンスをアクティブ化する新しい宛先を設定する場合は、**[!UICONTROL Configure new destination]**&#x200B;を選択して[宛先への接続](/help/destinations/ui/connect-destination.md) ワークフローをトリガーにし、[ アカウントをデータ型](/help/destinations/ui/connect-destination.md#segment-activation-or-dataset-exports)として選択します。

![ アカウント管理がハイライト表示された宛先アクティベーションのワークフロー。](/help/destinations/assets/ui/activate-account-audiences/activate-account-audiences-highlighted.png)

1. 次のセクションに進み、[書き出すアカウントオーディエンスを選択](#select-profile-audiences)します。

## アカウントオーディエンスの選択 {#select-account-audiences}

アカウントオーディエンス名の左側にあるチェックボックスを使用して、宛先に書き出すオーディエンスを選択し、**[!UICONTROL Next]**&#x200B;を選択します。

>[!NOTE]
>
>このビューには&#x200B;*アカウントオーディエンス*&#x200B;のみが表示され、他のオーディエンスタイプは表示されません。

![ データセットの書き出しワークフローで、書き出すアカウントオーディエンスを選択できる「オーディエンスを選択」ステップが表示されます。](/help/destinations/assets/ui/activate-account-audiences/select-account-audiences.png)

## スケジュール設定と次のステップ {#scheduling-and-next-steps}

アカウントオーディエンスを書き出すアクティベーションワークフローの残りの部分については、ファイルベースの宛先へのデータのアクティベーションに関するチュートリアルを参照してください。 [ オーディエンスの書き出しスケジュール手順](/help/destinations/ui/activate-batch-profile-destinations.md#scheduling)から続行します。 **[!UICONTROL (Companies) LinkedIn Matched Audiences]**&#x200B;宛先に対してアカウントオーディエンスをアクティブ化する場合は、ストリーミング宛先のアクティブ化に関するチュートリアルを参照してください。 [ マッピング手順](/help/destinations/ui/activate-segment-streaming-destinations.md#mapping)から続行します。

>[!NOTE]
>
>アカウントオーディエンスをクラウドストレージの宛先に書き出すスケジュール手順では、アカウントオーディエンスをアクティブ化するワークフローで、毎日のスケジュール [で](/help/destinations/ui/activate-batch-profile-destinations.md#export-full-files)完全ファイル [と](/help/destinations/ui/activate-batch-profile-destinations.md#export-incremental-files)増分ファイル __&#x200B;のみを書き出すことができます。 時間単位の書き出しはサポートされていません。 **[!UICONTROL After audience evaluation]**&#x200B;は、サポートされている評価タイプです。

## 重要なコールアウトと既知の制限事項 {#important-callouts-known-limitations}

アカウントオーディエンスをアクティブ化する機能の一般公開リリースに関する次の重要な注意事項と既知の制限事項に注意してください。

### **[!UICONTROL (Companies) LinkedIn Matched Audiences]**&#x200B;宛先にアカウントオーディエンスをアクティブ化する際に、マッピングステップで必須のマッピングペア {#required-mappings}

**[!UICONTROL (Companies) LinkedIn Matched Audiences]**&#x200B;宛先に対してアカウントオーディエンスをアクティブ化する場合、データを正常にエクスポートするには、次の2つのマッピングペアが必須であることに注意してください。

![LinkedInが必須フィールドをマッピングしています。](/help/destinations/assets/ui/activate-account-audiences/linkedin-mapping-required-fields.png)

| ソースフィールド | ターゲットフィールド |
|---------|----------|
| `accountName` | `companyName` |
| `accountKey.sourceKey` | `primaryId` （**[!UICONTROL Select Identity namespace]**&#x200B;を選択する際に&#x200B;**[!UICONTROL Target Field]** ビューでこのフィールドを選択します）。<br> ![ ワークフローで強調表示されているID名前空間を選択して、アカウントオーディエンスを宛先にアクティブ化します。](/help/destinations/assets/ui/activate-account-audiences/identity-namespace-highlighted.png " ワークフローで強調表示されたID名前空間を選択して、アカウントオーディエンスを宛先にアクティブ化します。"){width="100" zoomable="yes"} |

{style="table-layout:auto"}

### データガバナンスの実施 {#data-governance-enforcement}

*顧客および見込み顧客*&#x200B;の個人レベルまたはプロファイルレベルで同意が適用されます。 したがって、宛先に対してアカウントオーディエンスをアクティブ化する場合、[同意ポリシー評価](/help/data-governance/enforcement/auto-enforcement.md#consent-policy-evaluation)は現在サポートされていません。 アクティブ化ワークフローのレビュー手順では、**[!UICONTROL View applicable consent policies]**&#x200B;のコントロールがグレー表示されています。

![同意の適用制御がグレー表示されたアカウント オーディエンスのアクティブ化ワークフローのレビュー手順。](/help/destinations/assets/ui/activate-account-audiences/consent-checks-greyed-out.png)

[!DNL Real-Time CDP] データ使用ポリシーチェック [や](/help/data-governance/enforcement/auto-enforcement.md#consent-policy-evaluation)属性ベースのアクセス制御[など、](/help/destinations/home.md#attribute-based-access)の他のデータガバナンスメカニズムがサポートされています。
