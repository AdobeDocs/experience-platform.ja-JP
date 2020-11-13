---
keywords: advertising; the trade desk;
title: The Trade Desk Destination
seo-title: The Trade Desk Destination
description: 'トレードデスクは、ディスプレイ、ビデオ、モバイル在庫のソースを対象としたデジタルキャンペーンのリターゲティングとオーディエンスを、広告購入者が実行するセルフサービスプラットフォームです。 '
seo-description: トレードデスクは、ディスプレイ、ビデオ、モバイル在庫のソースを対象としたデジタルキャンペーンのリターゲティングとオーディエンスを、広告購入者が実行するセルフサービスプラットフォームです。
translation-type: tm+mt
source-git-commit: c9fb63b390d4c7dddcdb35a85710ff664614ad63
workflow-type: tm+mt
source-wordcount: '575'
ht-degree: 5%

---


# [!DNL The Trade Desk] destination

## 概要 {#overview}

[!DNL The Trade Desk] destinationは、プロファイルデータの送信先にな [!DNL The Trade Desk]ります。

[!DNL The Trade Desk] は、ディスプレイ、ビデオ、モバイル在庫ソースにわたって、デジタルキャンペーンをターゲットにした再ターゲット化やオーディエンスを広告購入者が実行するためのセルフサービスプラットフォームです。

にプロファイルデータを送信するに [!DNL The Trade Desk]は、まず宛先に接続する必要があります。

## 宛先の仕様 {#destination-specs}

Note the following details that are specific to the [!DNL The Trade Desk] destination:

* You can send the following [identities](../../identity-service/namespaces.md) to [!DNL The Trade Desk] destinations: [!DNL The Trade Desk ID], [!DNL IDFA], [!DNL GAID].

## 使用例 {#use-cases}

マーケターとして、またはデバイスIDから構築されたセグメントを使用して、デジタルキャンペーンをターゲットにした再ターゲット化 [!DNL Trade Desk IDs] やオーディエンスを作成できるようにしたいと思います。

## 書き出しタイプ {#export-type}

**[!DNL Segment export]**  — セグメント(オーディエンス)のすべてのメンバーをエクスポート先にエクスポートします。

## 宛先に接続 {#connect-destination}

1. **[!UICONTROL 接続]** / **[!UICONTROL 宛先]**、を選択し、「 [!DNL The Trade Desk]設定 ****」を選択します。

   ![トレードデスクの宛先の設定](assets/tradedesk-destination-configure.png)

   >[!NOTE]
   >
   >この宛先との接続が既に存在する場合は、宛先カードに **[!UICONTROL 「アクティブ化]** 」ボタンが表示されます。 「 **[!UICONTROL アクティブ化]** 」と「 **[!UICONTROL 設定]**」の違いについて詳しくは、表示先ワークスペースのドキュメントの「 [カタログ](../destinations/destinations-workspace.md#catalog) 」セクションを参照してください。
   >
   >![トレードデスクの宛先を有効にする](assets/tradedesk-destination-activate.png)

1. 「 [!UICONTROL 認証] 」の手順で、 [!DNL The Trade Desk] 接続の詳細を入力する必要があります。

   * **[!UICONTROL 名前]**:この宛先が将来認識される名前。
   * **[!UICONTROL 説明]**:この宛先を将来特定するのに役立つ説明です。
   * **[!UICONTROL アカウントID]**:アカウント [!DNL Trade Desk] ID 。
   * **[!UICONTROL クライアントシークレット]**:クライアント資格情報で使用される `clientSecret` パラメーター [!DNL OAuth2] 。
   * **[!UICONTROL サーバーの場所]**:どの地域サーバーを使用すべきかを [!DNL The Trade Desk] 担当者に問い合わせてください。 以下は、選択可能な地域サーバーです。

      * **[!UICONTROL ヨーロッパ]**
      * **[!UICONTROL シンガポール]**
      * **[!UICONTROL 東京]**
      * **[!UICONTROL 北米東部]**
      * **[!UICONTROL 北米西部]**
      * **[!UICONTROL ラテンアメリカ]**
   * **[!UICONTROL マーケティングの使用例]**:マーケティングの使用例は、データがエクスポート先にエクスポートされる意図を示します。 Adobe定義のマーケティングの使用例から選択するか、独自のマーケティングの使用例を作成することができます。 マーケティングの使用例について詳しくは、「 [データガバナンス(Adobe Experience Platform](../privacy/data-governance-overview.md#destinations) )」ページを参照してください。 個々のAdobe定義マーケティングの使用例について詳しくは、 [データ使用ポリシーの概要を参照してください](../../data-governance/policies/overview.md#core-actions)。

   ![トレードデスクの認証手順](assets/tradedesk-destination-authentication.png)

1. 「 **[!UICONTROL 作成先]**」をクリックします。 これで宛先が作成されました。You can click [!UICONTROL Save &amp; Exit] if you want to activate segments later, or you can select [!UICONTROL Next] to continue the workflow and select segments to activate. In either case, see the next section, [Activate Segments](#activate-segments), for the rest of the workflow.

## セグメントのアクティブ化 {#activate-segments}

セグメントのアクティベーションワークフローについて詳しくは、「[宛先へのプロファイルとセグメントのアクティブ化](activate-destinations.md#select-attributes)」を参照してください。

セグメ [ントのスケジュール](activate-destinations.md#segment-schedule) ：手順で、セグメントを宛先の対応するIDまたはフレンドリ名に手動でマップする必要があります。

セグメントをマッピングする場合は、使いやすいように、セグメント名を短くすることをお勧めします。 [!DNL Platform] ただし、宛先のセグメントIDまたは名前が、アカウントのセグメントIDまたは名前と一致している必要はありません [!DNL Platform] 。 マッピングフィールドに挿入した値は、すべて宛先に反映されます。

複数のデバイスマッピング(cookie ID、 [!DNL IDFA]、 [!DNL GAID])を使用する場合は、3つのマッピングすべてに同じマッピング値を使用する必要があります。 [!DNL The Trade Desk] は、すべてのセグメントを1つのセグメントに集計し、デバイスレベルの分類を表示します。

![セグメントマッピングID](assets/segment-mapping-id.png)


## 書き出されたデータ {#exported-data}

データが宛先に正常にエクスポートされたかどうかを確認するには、ア [!DNL The Trade Desk][!DNL The Trade Desk] カウントを確認してください。 アクティベーションに成功すると、オーディエンスがアカウントに入力されます。
