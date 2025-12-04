---
title: ストリーミングプロファイルの書き出し宛先に対してオーディエンスをアクティブ化する
type: Tutorial
description: オーディエンスをストリーミングプロファイルベースの宛先に送信して、Adobe Experience Platformでのオーディエンスデータをアクティブ化する方法を説明します。
badgeUltimate: label="Ultimate" type="Positive"
exl-id: bc0f781e-60de-44a5-93cb-06b4a3148591
source-git-commit: 9f4ce2a3a8af72342683c859caa270662b161b7d
workflow-type: tm+mt
source-wordcount: '717'
ht-degree: 13%

---


# ストリーミングプロファイルの書き出し宛先に対してオーディエンスをアクティブ化する

>[!IMPORTANT]
> 
> * データをアクティブ化し、ワークフローの [&#x200B; マッピングステップ &#x200B;](#mapping) を有効にするには、**[!UICONTROL View Destinations]**、**[!UICONTROL Activate Destinations]**、**[!UICONTROL View Profiles]** および **[!UICONTROL View Segments]** [&#x200B; アクセス制御権限 &#x200B;](/help/access-control/home.md#permissions) が必要です。
> * ワークフローの [&#x200B; マッピングステップ &#x200B;](#mapping) を実行せずにデータをアクティブ化するには、**[!UICONTROL View Destinations]**、**[!UICONTROL Activate Segment without Mapping]**、**[!UICONTROL View Profiles]** および **[!UICONTROL View Segments]** [&#x200B; アクセス制御権限 &#x200B;](/help/access-control/home.md#permissions) が必要です。
> 
> 詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

## 概要 {#overview}

この記事では、Adobe Experience Platformのオーディエンスデータをストリーミングプロファイルベースの宛先（「エンタープライズ宛先 [&#x200B; とも呼ばれます）に対してアクティブ化するために必要なワークフロー &#x200B;](/help/destinations/destination-types.md#advanced-enterprise-destinations) ついて説明します。

この記事は、次の 3 つの宛先に適用されます。

* [Amazon Kinesis](/help/destinations/catalog/cloud-storage/amazon-kinesis.md)
* [Azure Event Hubs](/help/destinations/catalog/cloud-storage/azure-event-hubs.md)
* [HTTP API 宛先 &#x200B;](/help/destinations/catalog/streaming/http-destination.md)。

## 前提条件 {#prerequisites}

宛先へのデータをアクティベートするには、正常に[宛先に接続する](./connect-destination.md)必要があります。まだ接続していない場合は、[宛先カタログ](../catalog/overview.md)に移動し、サポートされている宛先を参照し、使用する宛先を設定します。

## 宛先の選択 {#select-destination}

1. **[!UICONTROL Connections > Destinations]** に移動して、「**[!UICONTROL Catalog]**」タブを選択します。

   ![&#x200B; 「宛先カタログ」タブを示す画像 &#x200B;](../assets/ui/activate-streaming-profile-destinations/catalog-tab.png)

1. 次の画像に示すように、オーディエンスをアクティベートする宛先に対応するカードで **[!UICONTROL Activate audiences]** を選択します。

   ![&#x200B; 「宛先のカタログ」タブの「オーディエンスをアクティブ化」コントロールをハイライト表示した画像。](../assets/ui/activate-streaming-profile-destinations/activate-audiences-button.png)

1. オーディエンスをアクティベートするために使用する宛先接続を選択し、「**[!UICONTROL Next]**」を選択します。

   ![&#x200B; 接続可能な 2 つの宛先の選択を示す画像 &#x200B;](../assets/ui/activate-streaming-profile-destinations/select-destination.png)

1. 次の節の「オーディエンスを選択 [&#x200B; に移動し &#x200B;](#select-audiences) す。

## オーディエンスを選択 {#select-audiences}

宛先に対してアクティブ化するオーディエンスを選択するには、オーディエンス名の左側にあるチェックボックスを使用したあと、「**[!UICONTROL Next]**」を選択します。

接触チャネルに応じて、複数のタイプのオーディエンスから選択できます。

* **[!UICONTROL Segmentation Service]**: Segmentation Service によってExperience Platform内で生成されたオーディエンス。 詳しくは、[&#x200B; オーディエンスポータルドキュメント &#x200B;](../../segmentation/ui/audience-portal.md) を参照してください。
* **[!UICONTROL Custom upload]**:Experience Platform以外で生成され、CSV ファイルとしてExperience Platformにアップロードされたオーディエンス。 外部オーディエンスについて詳しくは、[&#x200B; オーディエンスの読み込み &#x200B;](../../segmentation/ui/audience-portal.md#import-audience) に関するドキュメントを参照してください。
* その他のタイプのオーディエンス。他のAdobe ソリューション（[!DNL Audience Manager] など）から派生します。

![&#x200B; アクティベーションワークフローのオーディエンスを選択手順で選択されたチェックボックス選択を強調表示した画像。](../assets/ui/activate-streaming-profile-destinations/select-audiences.png)

## プロファイル属性の選択 {#select-attributes}

**[!UICONTROL Mapping]** の手順では、ターゲット宛先に送信するプロファイル属性を選択します。

1. **[!UICONTROL Select attributes]** ページで「**[!UICONTROL Add new field]**」を選択します。

   ![&#x200B; マッピング手順の「新しいフィールドを追加」コントロールを強調表示した画像。](../assets/ui/activate-streaming-profile-destinations/add-new-field.png)

1. **[!UICONTROL Schema field]** エントリの右側の矢印を選択します。

   ![&#x200B; マッピングステップでのソースフィールドの選択方法をハイライト表示した画像。](../assets/ui/activate-streaming-profile-destinations/select-schema-field.png)

1. **[!UICONTROL Select field]** ページで、宛先に送信する XDM 属性を選択してから「**[!UICONTROL Select]**」を選択します。

   ![&#x200B; ソースフィールドとして選択できる XDM フィールドの選択を示す画像 &#x200B;](../assets/ui/activate-streaming-profile-destinations/target-field-page.png)

1. さらにフィールドを追加するには、手順 1 ～ 3 を繰り返してから「**[!UICONTROL Next]**」を選択します。

## レビュー {#review}

**[!UICONTROL Review]** のページには、選択内容の概要が表示されます。 **[!UICONTROL Cancel]** を選択してフローを中断する **[!UICONTROL Back]**、設定を変更する、または **[!UICONTROL Finish]** を選択して選択内容を確認し、宛先へのデータの送信を開始します。

![&#x200B; レビュー手順の選択の概要。](../assets/ui/activate-streaming-profile-destinations/review.png)

### 同意ポリシーの評価 {#consent-policy-evaluation}

[&#x200B; 同意ポリシーの評価 &#x200B;](/help/data-governance/enforcement/auto-enforcement.md#consent-policy-evaluation) は、現在、Amazon Kinesis、Azure Event Hubs および HTTP API の 3 つのエンタープライズ宛先への書き出しではサポートされていません。

つまり、ターゲット化に同意しないプロファイル *含まれる*、これら 3 つの宛先への書き出しに含まれます。

<!--

If your organization purchased **Adobe Healthcare Shield** or **Adobe Privacy & Security Shield**, select **[!UICONTROL View applicable consent policies]** to see which consent policies are applied and how many profiles are included in the activation as a result of them. Read about [consent policy evaluation](/help/data-governance/enforcement/auto-enforcement.md#consent-policy-evaluation) for more information.

-->

### データ使用ポリシーのチェック {#data-usage-policy-checks}

**[!UICONTROL Review]** の手順では、Experience Platformはデータ使用ポリシーの違反もチェックします。 ポリシーに違反した場合の例を次に示します。違反を解決するまで、Audience Activation ワークフローを完了することはできません。 ポリシー違反の解決方法については、データガバナンスに関するドキュメントの [&#x200B; データ使用ポリシー違反 &#x200B;](/help/data-governance/enforcement/auto-enforcement.md#data-usage-violation) を参照してください。

![データポリシー違反](../assets/common/data-policy-violation.png)

### オーディエンスのフィルタリング {#filter-audiences}

また、この手順では、ページで使用可能なフィルターを使用して、このワークフローの一環としてスケジュールまたはマッピングが更新されたオーディエンスのみを表示できます。

![&#x200B; レビューステップで使用可能なオーディエンスフィルターを示す画面録画。](../assets/ui/activate-streaming-profile-destinations/filter-audiences-review-step.gif)

選択内容に満足し、ポリシー違反が検出されていない場合は、「**[!UICONTROL Finish]**」を選択して選択内容を確定し、宛先へのデータの送信を開始します。

## Audience Activation の検証 {#verify}

書き出された [!DNL Experience Platform] データは、JSON 形式でターゲットの宛先に格納されます。 例えば、以下のイベントには、特定のオーディエンスに対して選定され、別のオーディエンスから退出したプロファイルのメールアドレス属性が含まれています。 この見込み客の ID は `ECID` と `email_lc_sha256` です。

```json
{
  "person": {
    "email": "yourstruly@adobe.com"
  },
  "segmentMembership": {
    "ups": {
      "7841ba61-23c1-4bb3-a495-00d3g5fe1e93": {
        "lastQualificationTime": "2020-05-25T21:24:39Z",
        "status": "exited"
      },
      "59bd2fkd-3c48-4b18-bf56-4f5c5e6967ae": {
        "lastQualificationTime": "2020-05-25T23:37:33Z",
        "status": "realized"
      }
    }
  },
  "identityMap": {
    "ecid": [
      {
        "id": "14575006536349286404619648085736425115"
      },
      {
        "id": "66478888669296734530114754794777368480"
      }
    ],
    "email_lc_sha256": [
      {
        "id": "655332b5fa2aea4498bf7a290cff017cb4"
      },
      {
        "id": "66baf76ef9de8b42df8903f00e0e3dc0b7"
      }
    ]
  }
}
```
