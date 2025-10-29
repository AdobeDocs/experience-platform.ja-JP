---
title: オーディエンス分析の宛先
description: お客様がCustomer Journey Analyticsで認定されるオーディエンスを表示します。
badgeLimitedAvailability: label="限定提供" type="Informative"
exl-id: 81437237-d746-4ce9-b938-7d2541f0ed32
hide: true
hidefromtoc: true
source-git-commit: 1b507e9846a74b7ac2d046c89fd7c27a818035ba
workflow-type: tm+mt
source-wordcount: '758'
ht-degree: 33%

---

# オーディエンス分析の宛先

[!UICONTROL Audience Analysis] の宛先を使用すると、Adobe Experience Platform オーディエンスデータを [Customer Journey Analytics](https://experienceleague.adobe.com/docs/analytics-platform/using/cja-overview/cja-overview.html?lang=ja) にエンリッチメントできます。 結果のエンリッチメントされたデータに含めるオーディエンスを選択できます。 オーディエンスの選定は、[Analysis Workspace](https://experienceleague.adobe.com/docs/analytics-platform/using/cja-workspace/home.html) レポートでディメンションとして使用できるようになります。

>[!AVAILABILITY]
>
>この宛先は、限定的なテストフェーズにあります。 この宛先の使用に興味がある場合は、Adobe アカウントチームにお問い合わせください。

## 前提条件

この宛先を使用するには、次が必要です。

* Audience Analysis の宛先を使用するようにプロビジョニングする必要があります。 この宛先を使用するためのプロビジョニングが未完了の場合は、Adobe アカウントチームにお問い合わせください。
* Customer Journey Analyticsを使用するようにプロビジョニングする必要があります。
* Adobe Experience Platformで 1 つ以上のオーディエンスを作成する必要があります。

## サポートされている ID

Audience Analysis では、以下の表に示す ID のアクティベーションをサポートしています。 ID の詳細は[こちら](/help/identity-service/features/namespaces.md)から。通常、Experience Cloud ID （ECID）が使用されます。

| ターゲット ID | 説明 | 注意点 |
|---|---|---|
| GAID | GOOGLE ADVERTISING ID | ソース ID が GAID 名前空間の場合は、GAID ターゲット ID を選択します。 |
| IDFA | Apple の広告主 ID | ソース ID が IDFA 名前空間の場合は、IDFA ターゲット ID を選択します。 |
| ECID | Experience Cloud ID | ECID を表す名前空間。 この名前空間は、「Adobe Marketing Cloud ID」、「Adobe Experience Cloud ID」、「Adobe Experience Platform ID」という別名で呼ばれることもあります。詳しくは、[ECID](/help/identity-service/features/ecid.md) に関する次のドキュメントを参照してください。 |
| phone_sha256 | SHA256 アルゴリズムでハッシュ化された電話番号 | Adobe Experience Platform では、プレーンテキストと SHA256 でハッシュ化された電話番号の両方がサポートされています。ハッシュ化されていない属性がソースフィールドに含まれている場合は、「**[!UICONTROL Apply transformation]**」オプションをオンにして、アクティブ化時にがデータ [!DNL Experience Platform] 自動的にハッシュ化するように設定します。 |
| email_lc_sha256 | SHA256 アルゴリズムでハッシュ化されたメールアドレス | Adobe Experience Platform では、プレーンテキストと SHA256 でハッシュ化されたメールアドレスの両方がサポートされています。ハッシュ化されていない属性がソースフィールドに含まれている場合は、「**[!UICONTROL Apply transformation]**」オプションをオンにして、アクティブ化時にがデータ [!DNL Experience Platform] 自動的にハッシュ化するように設定します。 |
| extern_id | カスタムユーザー ID | ソース ID がカスタム名前空間の場合は、このターゲット ID を選択します。 |

{style="table-layout:auto"}

## サポートされるオーディエンス

この宛先を使用する場合、次のタイプのオーディエンスがサポートされます。

| オーディエンスオリジン | サポートあり | 説明 |
|---------|----------|----------|
| [!DNL Segmentation Service] | ✓ | Experience Platform [ セグメント化サービス ](../../../segmentation/home.md) を通じて生成されたオーディエンス。 |
| カスタムアップロード | ✓ | CSV ファイルから Experience Platform に[読み込まれた](../../../segmentation/ui/audience-portal.md#import-audience)オーディエンス。 |

{style="table-layout:auto"}

## 書き出しのタイプと頻度

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
|---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL Audience export]** | Audience Analysis の宛先で使用される識別子（名前、電話番号など）を使用して、オーディエンスのすべてのメンバーを書き出します。 |
| 書き出し頻度 | **[!UICONTROL Streaming]** | ストリーミングの宛先は常に、API ベースの接続です。オーディエンスの評価に基づいてExperience Platform内でプロファイルが更新されると、コネクタは更新を宛先プラットフォームに送信します。 [ストリーミングの宛先](/help/destinations/destination-types.md#streaming-destinations)の詳細についてはこちらを参照してください。 |

{style="table-layout:auto"}

## 新しい宛先を設定

>[!IMPORTANT]
> 
>宛先を作成するには、**[!UICONTROL View Destinations]** および **[!UICONTROL Manage Destinations]**[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。

この宛先を作成するには、[ 宛先設定のチュートリアル ](../../ui/connect-destination.md) に示されている手順に従います。

### 宛先の詳細

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横のアスタリスクは、そのフィールドが必須であることを示します。

* **[!UICONTROL Name]**：宛先名。
* **[!UICONTROL Description]**：宛先の説明。
* **[!UICONTROL Datastream ID]**：選定オーディエンスでエンリッチメントするデータストリーム ID。 この ID は、[ データストリームマネージャー ](/help/datastreams/overview.md) で取得できます。
* **[!UICONTROL Integration alias]**：統合エイリアス。

### アラート

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

* **[!UICONTROL Activation Skipped Rate Exceed]**: アクティブ化のスキップ率がしきい値を超えると通知されます。

宛先接続への詳細の入力を終えたら「**[!UICONTROL Next]**」を選択します。

### ガバナンスポリシーと実施アクション

このオプションのセクションを使用すると、データガバナンスポリシーを定義し、オーディエンスが送信されてアクティブな場合に使用するデータが準拠していることを確認できます。

宛先に対する目的のマーケティングアクションの選択が終了したら、「**[!UICONTROL Create]**」を選択します。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>データをアクティブ化するには、**[!UICONTROL View Destinations]**、**[!UICONTROL Activate Destinations]**、**[!UICONTROL View Profiles]**、**[!UICONTROL View Segments]** [ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。

宛先を作成したら、宛先に必要なオーディエンスをアクティブ化できます。

1. 作成した宛先にまだ移動していない場合は、**[!UICONTROL Destinations]**/**[!UICONTROL Browse]** に移動して、もう一度見つけることができます。
1. **[!UICONTROL Activate audiences]** を選択します。
1. 選定を分析する対象オーディエンスを選択します。 終了したら「**[!UICONTROL Next]**」を選択します。
1. 宛先設定とオーディエンス設定を確認し、「**[!UICONTROL Finish]**」を選択します。

**[!UICONTROL Activate audiences]** のページに戻ると、将来の分析の対象となるオーディエンスを追加できます。 アクティベートしたオーディエンスを削除することはできません。
