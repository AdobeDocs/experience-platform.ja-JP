---
title: Marketo Engage の宛先
description: Marketo Engageは、マーケティング、広告、分析、コマースに対する唯一のエンドツーエンドのカスタマーエクスペリエンス管理 (CXM) ソリューションです。 CRM リード管理や顧客エンゲージメントから、アカウントベースのマーケティングや売上高属性に至るアクティビティを自動化および管理できます。
exl-id: 5ae5f114-47ba-4ff6-8e42-f8f43eb079f7
source-git-commit: 6dc4a93b46d6111637e0024da574d605e0d2b986
workflow-type: tm+mt
source-wordcount: '740'
ht-degree: 21%

---

# Marketo Engage先 {#beta-marketo-engage-destination}

## 宛先の変更ログ {#changelog}

>[!IMPORTANT]
>
>のリリースに伴い、 [Marketo V2 宛先コネクタの機能強化](/help/release-notes/2022/july-2022.md#destinations)に設定すると、宛先カタログに 2 つのMarketoカードが表示されます。
>* 既に **[!UICONTROL Marketo V1]** 宛先：新しいデータフローを **[!UICONTROL Marketo V2]** 宛先への既存のデータフローの削除 **[!UICONTROL Marketo V1]** 2023 年 2 月までに宛先に追加されました。 この日付以降、 **[!UICONTROL Marketo V1]** 宛先カードが削除されます。
>* まだ **[!UICONTROL Marketo V1]** 宛先、新しい **[!UICONTROL Marketo V2]** Marketoに接続してデータを書き出すためのカード。


![2 つのMarketo宛先カードの横並び表示の画像。](/help/destinations/assets/catalog/adobe/marketo-side-by-side-view.png)

Marketo V2 の宛先の改善点を次に示します。

* Marketo V1 では、アクティベーションワークフローの&#x200B;**[!UICONTROL セグメントをスケジュール]**&#x200B;手順で、**マッピング ID** を手動で追加して、Marketo にデータを正常に書き出す必要がありました。この手動の手順は、Marketo V2 では不要になりました。
* Marketo V1 では、アクティベーションワークフローの&#x200B;**[!UICONTROL マッピング]**&#x200B;手順で、XDM フィールドを Marketo の 3 つのターゲットフィールド（`firstName`、`lastName`、および `companyName`）にのみマッピングできました。  Marketo V2 リリースで、XDM フィールドを Marketo の多数のフィールドにマッピングできるようになりました。 詳しくは、 [サポートされている属性](#supported-attributes) の節を参照してください。

## 概要 {#overview}

[!DNL Marketo Engage] は、マーケティング、広告、分析、コマースに対応する、唯一のエンドツーエンドのカスタマーエクスペリエンス管理 (CXM) ソリューションです。 CRM リード管理や顧客エンゲージメントから、アカウントベースのマーケティングや売上高属性に至るアクティビティを自動化および管理できます。

 の宛先を利用することで、マーケターは、Adobe Experience Platform で作成したセグメントを Marketo にプッシュできます。それらのセグメントは静的リストとして表示されます。

## サポートされる ID と属性 {#supported-identities-attributes}

>[!NOTE]
>
>内 [マッピング手順](/help/destinations/ui/activate-segment-streaming-destinations.md#mapping) 「宛先のアクティブ化」ワークフローの「 *必須* ID をマッピングするには、および *オプション* 属性をマッピングします。 「 ID 名前空間」タブから電子メールや ECID をマッピングすることは、Marketoで人物が一致するようにするために最も重要な作業です。 「E メールのマッピング」を選択すると、最も高い一致率が確保されます。

### サポートされる ID {#supported-identities}

| ターゲット ID | 説明 |
|---|---|
| ECID | ECID を表す名前空間。 この名前空間は、次のエイリアスからも参照できます。&quot;Adobe Marketing Cloud ID&quot;、&quot;Adobe Experience Cloud ID&quot;、&quot;Adobe Experience Platform ID&quot;。 次のドキュメントを参照してください： [ECID](/help/identity-service/ecid.md) を参照してください。 |
| メール | 電子メールアドレスを表す名前空間。 このタイプの名前空間は、多くの場合、1 人のユーザーに関連付けられているので、様々なチャネルをまたいでそのユーザーを識別するために使用できます。 |

{style=&quot;table-layout:auto&quot;}

### サポートされている属性 {#supported-attributes}

Experience Platformの属性を、組織がMarketoでアクセスできる任意の属性にマッピングできます。 Marketoでは、 [API リクエストの説明](https://developers.marketo.com/rest-api/lead-database/leads/#describe) ：組織がアクセス権を持つ属性フィールドを取得します。

## エクスポートのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、次の表を参照してください。

| 項目 | タイプ | 備考 |
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL セグメントエクスポート]** | セグメント（オーディエンス）のすべてのメンバーを、 [!DNL Marketo Engage] 宛先。 |
| 書き出し頻度 | **[!UICONTROL ストリーミング]** | ストリーミングの宛先は、API ベースの接続です。 セグメント評価に基づいてExperience Platform内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。 詳細を表示 [ストリーミング先](/help/destinations/destination-types.md#streaming-destinations). |

{style=&quot;table-layout:auto&quot;}

## 宛先の設定とセグメントのアクティブ化 {#set-up}

>[!IMPORTANT]
> 
>* 宛先に接続するには、 **[!UICONTROL 宛先の管理]** [アクセス制御権限](/help/access-control/home.md#permissions).
>* データをアクティブ化するには、 **[!UICONTROL 宛先の管理]**, **[!UICONTROL 宛先のアクティブ化]**, **[!UICONTROL プロファイルの表示]**、および **[!UICONTROL セグメントを表示]** [アクセス制御権限](/help/access-control/home.md#permissions). 詳しくは、 [アクセス制御の概要](/help/access-control/ui/overview.md) または製品管理者に問い合わせて、必要な権限を取得してください。


宛先の設定方法とセグメントのアクティブ化方法について詳しくは、 [Adobe Experience PlatformセグメントをMarketo静的リストにプッシュ](https://experienceleague.adobe.com/docs/marketo/using/product-docs/core-marketo-concepts/smart-lists-and-static-lists/static-lists/push-an-adobe-experience-cloud-segment-to-a-marketo-static-list.html?lang=en) (Marketoドキュメント ) を参照してください。

次のビデオでは、Marketoの宛先を設定し、セグメントをアクティブ化する手順についても説明します。

>[!NOTE]
>
>Adobe Experience Platform のユーザーインターフェイスは頻繁に更新され、このビデオが録画された後に変更されている可能性があります。 最新情報については、上記にリンクされたガイドを参照してください。

>[!VIDEO](https://video.tv.adobe.com/v/338248?quality=12)

<!--

## Connect to the destination {#connect}

To connect to this destination, follow the steps described in the [destination configuration tutorial](../../ui/connect-destination.md).

-->

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。詳しくは、 [!DNL Adobe Experience Platform] データガバナンスを強制します。詳しくは、 [データガバナンスの概要](https://experienceleague.adobe.com/docs/experience-platform/data-governance/home.html?lang=ja).

<!--

## Activate segments to this destination {#activate}

See [Activate audience data to streaming segment export destinations](../../ui/activate-segment-streaming-destinations.md) for instructions on activating audience segments to this destination.

-->
