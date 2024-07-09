---
title: Marketo Engage の宛先
description: Marketo Engageは、マーケティング、広告、分析およびコマース用の唯一のエンドツーエンドのカスタマーエクスペリエンス管理（CXM）ソリューションです。 CRM リード管理と顧客エンゲージメントから、アカウントベースのマーケティングと収益属性に至るまで、アクティビティを自動化および管理できます。
exl-id: 5ae5f114-47ba-4ff6-8e42-f8f43eb079f7
source-git-commit: c35b43654d31f0f112258e577a1bb95e72f0a971
workflow-type: tm+mt
source-wordcount: '885'
ht-degree: 26%

---

# Marketo Engage先 {#beta-marketo-engage-destination}

## 宛先の変更ログ {#changelog}

>[!IMPORTANT]
>
>のリリースとともに [拡張されたMarketo V2 宛先コネクタ](/help/release-notes/2022/july-2022.md#destinations)に設定すると、宛先カタログに 2 つのMarketo カードが表示されます。
>* に対して既にデータをアクティブ化している場合 **[!UICONTROL Marketo V1]** 宛先：への新しいデータフローを作成してください **[!UICONTROL Marketo V2]** 宛先との既存のデータフローを削除 **[!UICONTROL Marketo V1]** 宛先（2023 年 2 月まで）。 この日において、 **[!UICONTROL Marketo V1]** 宛先カードが削除されます。
>* へのデータフローをまだ作成していない場合は次のようにします。 **[!UICONTROL Marketo V1]** 宛先。新しいを使用してください **[!UICONTROL Marketo V2]** Marketoに接続してデータを書き出すためのカード。

![2 つのMarketo宛先カードを並べて表示した画像](../..//assets/catalog/adobe/marketo-side-by-side-view.png)

Marketo V2 の宛先の改善点は次のとおりです。

* Marketo V1 では、アクティベーションワークフローの&#x200B;**[!UICONTROL セグメントをスケジュール]**&#x200B;手順で、**マッピング ID** を手動で追加して、Marketo にデータを正常に書き出す必要がありました。この手動の手順は、Marketo V2 では不要になりました。
* Marketo V1 では、アクティベーションワークフローの&#x200B;**[!UICONTROL マッピング]**&#x200B;手順で、XDM フィールドを Marketo の 3 つのターゲットフィールド（`firstName`、`lastName`、および `companyName`）にのみマッピングできました。  Marketo V2 リリースで、XDM フィールドを Marketo の多数のフィールドにマッピングできるようになりました。 詳しくは、 [サポートされる属性](#supported-attributes) この後の節を参照してください。

## 概要 {#overview}

[!DNL Marketo Engage] は、マーケティング、広告、分析、コマース用の唯一のエンドツーエンドの顧客体験管理（CXM）ソリューションです。 CRM リード管理と顧客エンゲージメントから、アカウントベースのマーケティングと収益属性に至るまで、アクティビティを自動化および管理できます。

宛先により、マーケターは、Adobe Experience Platformで作成されたオーディエンスをMarketoにプッシュできます。それらのオーディエンスは静的リストとして表示されます。

## サポートされる ID と属性 {#supported-identities-attributes}

>[!NOTE]
>
>が含まれる [マッピングステップ](/help/destinations/ui/activate-segment-streaming-destinations.md#mapping) 宛先をアクティブ化ワークフローでは、次のようになります *mandatory* id とをマッピングする場合 *optional* をクリックして属性をマッピングします。 「ID 名前空間」タブでのメールや ECID のマッピングは、Marketoでユーザーが一致していることを確認するために行う最も重要な操作です。 マッピングメールは、最も高い一致率を保証します。

### サポートされている ID {#supported-identities}

| ターゲット ID | 説明 |
|---|---|
| ECID | ECID を表す名前空間。 この名前空間は、「Adobe Marketing Cloud ID」、「Adobe Experience Cloud ID」、「Adobe Experience Platform ID」という別名で呼ばれることもあります。次のドキュメントを参照してください： [ECID](/help/identity-service/features/ecid.md) を参照してください。 |
| メール | メールアドレスを表す名前空間。 このタイプの名前空間は多くの場合、1 人の人物に関連付けられているので、様々なチャネルでその人物を識別するために使用できます。 |

{style="table-layout:auto"}

### サポートされる属性 {#supported-attributes}

Experience Platformの属性を、Marketoでアクセスできる任意の属性にマッピングできます。 Marketoでは、を使用できます [API リクエストの説明](https://developers.marketo.com/rest-api/lead-database/leads/#describe) 組織がアクセスできる属性フィールドを取得する場合

## サポートされるオーディエンス {#supported-audiences}

この節では、この宛先に書き出すことができるオーディエンスのタイプについて説明します。

| オーディエンスオリジン | サポートあり | 説明 |
|---------|----------|----------|
| [!DNL Segmentation Service] | ✓ | Experience Platformを通じて生成されたオーディエンス [セグメント化サービス](../../../segmentation/home.md). |
| カスタムアップロード | ✓ | CSV ファイルから Experience Platform に[読み込まれた](../../../segmentation/ui/audience-portal.md#import-audience)オーディエンス。 |

{style="table-layout:auto"}

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL オーディエンスの書き出し]** | で使用される識別子（メール、ECID）を使用して、オーディエンスのすべてのメンバーを書き出します [!DNL Marketo Engage] の宛先。 |
| 書き出し頻度 | **[!UICONTROL ストリーミング]** | ストリーミングの宛先は常に、API ベースの接続です。オーディエンス評価に基づいて Experience Platform 内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。[ストリーミングの宛先](/help/destinations/destination-types.md#streaming-destinations)の詳細についてはこちらを参照してください。 |

{style="table-layout:auto"}

## 宛先の設定とオーディエンスのアクティブ化 {#set-up}

>[!IMPORTANT]
> 
>* 宛先に接続するには、 **[!UICONTROL 宛先の表示]** および **[!UICONTROL 宛先の管理]** [アクセス制御権限](/help/access-control/home.md#permissions).
>* データをアクティブ化するには、 **[!UICONTROL 宛先の表示]**, **[!UICONTROL 宛先のアクティブ化]**, **[!UICONTROL プロファイルの表示]**、および **[!UICONTROL セグメントの表示]** [アクセス制御権限](/help/access-control/home.md#permissions). [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。

宛先を設定しオーディエンスをアクティブ化する方法について詳しくは、以下を参照してください [Adobe Experience Platform オーディエンスのMarketo静的リストへのプッシュ](https://experienceleague.adobe.com/docs/marketo/using/product-docs/core-marketo-concepts/smart-lists-and-static-lists/static-lists/push-an-adobe-experience-cloud-segment-to-a-marketo-static-list.html?lang=ja) Marketoのドキュメントで説明しています。

次のビデオでは、Marketoの宛先を設定し、オーディエンスをアクティブ化する手順も示します。

>[!IMPORTANT]
>
>ビデオには、現在の機能がすべて反映されているわけではありません。 最新の情報については、上記にリンクされているガイドを参照してください。 ビデオの次の部分は古くなっています。
> 
>* Experience PlatformUI で使用する必要がある宛先カードは、です **[!UICONTROL Marketo V2]**.
>* ビデオに新しいが表示されない **[!UICONTROL 人物の作成]** 宛先に接続ワークフローのセレクターフィールド。
>* ビデオで言及されている 2 つの制限は、もはや適用されません。 ビデオの記録時にサポートされていたオーディエンスメンバーシップ情報に加えて、他の多くのプロファイル属性フィールドをマッピングできるようになりました。 Marketoの静的リストにまだ存在しないMarketoにオーディエンスメンバーを書き出すこともできます。これらはリストに追加されます。
>* が含まれる **[!UICONTROL オーディエンスをスケジュール手順]** アクティベーションワークフローのMarketo V1 では、を手動で追加する必要がありました。 **[!UICONTROL マッピング ID]** をクリックして、Marketoにデータを正常に書き出します。 この手動の手順は、Marketo V2 では不要になりました。

>[!VIDEO](https://video.tv.adobe.com/v/338248?quality=12)

<!--

## Connect to the destination {#connect}

To connect to this destination, follow the steps described in the [destination configuration tutorial](../../ui/connect-destination.md).

-->

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。方法について詳しくは、 [!DNL Adobe Experience Platform] データガバナンスを実施します。を参照してください。 [データガバナンスの概要](https://experienceleague.adobe.com/docs/experience-platform/data-governance/home.html?lang=ja).

<!--

## Activate audiences to this destination {#activate}

See [Activate audience data to streaming audience export destinations](../../ui/activate-segment-streaming-destinations.md) for instructions on activating audiences to this destination.

-->
