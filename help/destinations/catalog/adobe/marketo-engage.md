---
title: Marketo Engage の宛先
description: Marketo Engageは、マーケティング、広告、分析およびコマース用の唯一のエンドツーエンドのカスタマーエクスペリエンス管理（CXM）ソリューションです。 CRM リード管理と顧客エンゲージメントから、アカウントベースのマーケティングと収益属性に至るまで、アクティビティを自動化および管理できます。
exl-id: 5ae5f114-47ba-4ff6-8e42-f8f43eb079f7
source-git-commit: 891484b279d2521115c6b1edc58f45c594a55382
workflow-type: tm+mt
source-wordcount: '874'
ht-degree: 27%

---

# （従来の）（V2）Marketo Engageの宛先 {#beta-marketo-engage-destination}

## 宛先の変更ログ {#changelog}

<!--
>[!IMPORTANT]
>
>The **[!UICONTROL (Legacy) (V2) Marketo Engage]** will be deprecated in **March 2026**.
>
>To ensure a smooth transition to the new **[[!UICONTROL Marketo Engage]](marketo-engage-connection.md)** destination, review the following key points and required actions:
>
>* All users of the existing **[!UICONTROL (Legacy) (V2) Marketo Engage]** must migrate to the new **[!UICONTROL Marketo Engage]** destination by March 2026.
>* **Existing dataflows will not be migrated automatically.** You must [set up a new connection](../../ui/connect-destination.md) to the new **[!UICONTROL Marketo Engage]** destination and activate your audiences there.

-->

![2 つのMarketoの宛先カードを並べて表示した画像 ](../..//assets/catalog/adobe/marketo-side-by-side-view.png)

Marketo V2 の宛先の改善点は次のとおりです。

* Marketo V1 では、アクティベーションワークフローの&#x200B;**[!UICONTROL セグメントをスケジュール]**&#x200B;手順で、**マッピング ID** を手動で追加して、Marketo にデータを正常に書き出す必要がありました。この手動の手順は、Marketo V2 では不要になりました。
* Marketo V1 では、アクティベーションワークフローの&#x200B;**[!UICONTROL マッピング]**&#x200B;手順で、XDM フィールドを Marketo の 3 つのターゲットフィールド（`firstName`、`lastName`、および `companyName`）にのみマッピングできました。  Marketo V2 リリースで、XDM フィールドを Marketo の多数のフィールドにマッピングできるようになりました。 詳しくは、後述の [ サポートされる属性 ](#supported-attributes) の節を参照してください。

## 概要 {#overview}

[!DNL Marketo Engage] は、マーケティング、広告、分析およびコマース用の唯一のエンドツーエンドの顧客体験管理（CXM）ソリューションです。 CRM リード管理と顧客エンゲージメントから、アカウントベースのマーケティングと収益属性に至るまで、アクティビティを自動化および管理できます。

宛先により、マーケターは、Adobe Experience Platformで作成されたオーディエンスをMarketoにプッシュできます。それらのオーディエンスは静的リストとして表示されます。

## サポートされる ID と属性 {#supported-identities-attributes}

>[!NOTE]
>
>宛先のアクティブ化ワークフローの [ マッピング手順 ](/help/destinations/ui/activate-segment-streaming-destinations.md#mapping) では、ID をマッピングする *必須* と属性をマッピングする *オプション* です。 「ID 名前空間」タブでのメールや ECID のマッピングは、Marketoでユーザーが一致していることを確認するために行う最も重要な操作です。 マッピングメールは、最も高い一致率を保証します。

### サポートされている ID {#supported-identities}

| ターゲット ID | 説明 |
|---|---|
| ECID | ECID を表す名前空間。 この名前空間は、「Adobe Marketing Cloud ID」、「Adobe Experience Cloud ID」、「Adobe Experience Platform ID」という別名で呼ばれることもあります。詳しくは、[ECID](/help/identity-service/features/ecid.md) に関する次のドキュメントを参照してください。 |
| メール | メールアドレスを表す名前空間。 このタイプの名前空間は多くの場合、1 人の人物に関連付けられているので、様々なチャネルでその人物を識別するために使用できます。 |

{style="table-layout:auto"}

### サポートされる属性 {#supported-attributes}

Experience Platformから、組織がMarketoでアクセス権を持つ任意の属性に属性をマッピングできます。 Marketoでは、[Describe API request](https://developers.marketo.com/rest-api/lead-database/leads/#describe) を使用して、組織がアクセスできる属性フィールドを取得できます。

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
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL オーディエンスの書き出し]** | オーディ [!DNL Marketo Engage] ンスの宛先で使用される識別子（メール、ECID）を使用して、オーディエンスのすべてのメンバーを書き出します。 |
| 書き出し頻度 | **[!UICONTROL ストリーミング]** | ストリーミングの宛先は常に、API ベースの接続です。オーディエンス評価に基づいて Experience Platform 内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。[ストリーミングの宛先](/help/destinations/destination-types.md#streaming-destinations)の詳細についてはこちらを参照してください。 |

{style="table-layout:auto"}

## 宛先の設定とオーディエンスのアクティブ化 {#set-up}

>[!IMPORTANT]
> 
>* 宛先に接続するには、**[!UICONTROL 宛先の表示]** および **[!UICONTROL 宛先の管理]**[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。
>* データをアクティブ化するには、**[!UICONTROL 宛先の表示]**、**[!UICONTROL 宛先のアクティブ化]**、**[!UICONTROL プロファイルの表示]** および **[!UICONTROL セグメントの表示]**[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。

宛先を設定しオーディエンスをアクティブ化する方法について詳しくは、Marketo ドキュメントの [Marketo オーディエンスをAdobe Experience Platform静的リストにプッシュする ](https://experienceleague.adobe.com/docs/marketo/using/product-docs/core-marketo-concepts/smart-lists-and-static-lists/static-lists/push-an-adobe-experience-cloud-segment-to-a-marketo-static-list.html?lang=ja) を参照してください。

次のビデオでは、Marketoの宛先を設定し、オーディエンスをアクティブ化する手順も示します。

>[!IMPORTANT]
>
>ビデオには、現在の機能がすべて反映されているわけではありません。 最新の情報については、上記にリンクされているガイドを参照してください。 ビデオの次の部分は古くなっています。
> 
>* Experience Platform UI で使用する必要がある宛先カードは、**[!UICONTROL Marketo V2]** です。
>* このビデオでは、宛先に接続ワークフローの新しい **[!UICONTROL ユーザー作成]** セレクターフィールドが表示されません。 このフィールドを使用するには、属性マッピング手順で名と姓の両方をマッピングする必要があります。
>* ビデオで言及されている 2 つの制限は、もはや適用されません。 ビデオの記録時にサポートされていたオーディエンスメンバーシップ情報に加えて、他の多くのプロファイル属性フィールドをマッピングできるようになりました。 Marketoの静的リストにまだ存在しないMarketoにオーディエンスメンバーを書き出すこともできます。これらはリストに追加されます。
>* Marketo V1 では、アクティベーションワークフローの **[!UICONTROL オーディエンスをスケジュール設定ステップ]** で、**[!UICONTROL マッピング ID]** を手動で追加して、Marketoにデータを正常に書き出す必要がありました。 この手動の手順は、Marketo V2 では不要になりました。

>[!VIDEO](https://video.tv.adobe.com/v/338248?quality=12)

## 宛先の監視 {#monitor-destination}

宛先に接続し、宛先データフローを確立したら、Real-Time CDPの [ モニタリング機能 ](/help/dataflows/ui/monitor-destinations.md) を使用して、各データフロー実行で宛先に対してアクティブ化されたプロファイルレコードに関する詳細な情報を取得できます。

[!DNL Marketo Engage] 接続の監視情報には、各データフローおよびデータフロー実行のアクティブ化、除外、失敗した ID に関するオーディエンスレベルの情報が含まれます。 機能について [ 詳細を参照 ](/help/dataflows/ui/monitor-destinations.md#segment-level-view)。

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。[!DNL Adobe Experience Platform] がどのようにデータガバナンスを実施するかについて詳しくは、[ データガバナンスの概要 ](https://experienceleague.adobe.com/docs/experience-platform/data-governance/home.html?lang=ja) を参照してください。
