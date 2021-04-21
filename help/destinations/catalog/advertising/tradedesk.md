---
keywords: 広告；業者の机広告業務机
title: トレードデスクの接続
description: トレードデスクは、ディスプレイ、ビデオ、モバイル在庫のソースを対象としたデジタルキャンペーンのリターゲティングとオーディエンスを、広告購入者が実行するセルフサービスプラットフォームです。
exl-id: b8f638e8-dc45-4aeb-8b4b-b3fa2906816d
translation-type: tm+mt
source-git-commit: 5b72433fcf2318f98538278c6d2650b366e391a2
workflow-type: tm+mt
source-wordcount: '628'
ht-degree: 5%

---

# [!DNL The Trade Desk] connection

## 概要 {#overview}

[!DNL The Trade Desk] destinationは、プロファイルデータの送信先にな [!DNL The Trade Desk]ります。

[!DNL The Trade Desk] は、ディスプレイ、ビデオ、モバイル在庫ソースにわたって、デジタルキャンペーンをターゲットにした再ターゲット化やオーディエンスを広告購入者が実行するためのセルフサービスプラットフォームです。

プロファイルデータを[!DNL Trade Desk]に送信するには、まず宛先に接続する必要があります。

## 使用例 {#use-cases}

マーケティング担当者として、[!DNL Trade Desk IDs]やデバイスIDから構築されたセグメントを使用して、デジタルキャンペーンをターゲットにした再ターゲット化やオーディエンスを作成できるようにしたいと思います。

## サポートされるID{#supported-identities}

[!DNL The Trade Desk] は、次の表に示すIDのアクティベーションをサポートしています。[ID](/help/identity-service/namespaces.md)の詳細を表示します。

| ターゲットID | 説明 |
|---|---|
| GAID | [!DNL Google Advertising ID] |
| IDFA | [!DNL Apple ID for Advertisers] |
| トレードデスクID | Advertiser ID in the Trade Desk platform |

## エクスポートタイプ{#export-type}

**[!DNL Segment export]**  — セグメント(オーディエンス)のすべてのメンバーをエクスポート先にエクスポートします。

## 前提条件 {#prerequisites}

[!DNL The Trade Desk]で最初の宛先を作成したい場合で、以前(Adobe Audience Managerや他のアプリケーションで)Experience CloudIDサービスで[ID同期機能](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/methods/idsync.html)を有効にしていない場合は、Adobeコンサルティングかカスタマーケアにご連絡ください。 以前にAudience Managerで[!DNL The Trade Desk]統合を設定していた場合、設定したID同期はPlatformに持ち越します。

## 宛先に接続 {#connect-destination}

**[!UICONTROL 接続]**/**[!UICONTROL 宛先]**&#x200B;で、[!DNL The Trade Desk]を選択し、**[!UICONTROL 設定]**&#x200B;を選択します。

![トレードデスクの宛先の設定](../../assets/catalog/advertising/tradedesk/configure.png)

この宛先との接続が既に存在する場合は、宛先カードに「**[!UICONTROL Activate]**」ボタンが表示されます。 「**[!UICONTROL アクティブ化]**」と「**[!UICONTROL 設定]**」の違いについて詳しくは、保存先のワークスペースドキュメントの「[カタログ](../../ui/destinations-workspace.md#catalog)」の節を参照してください。

![トレードデスクの宛先を有効にする](../../assets/catalog/advertising/tradedesk/activate.png)

## 認証手順{#authentication}

**[!UICONTROL 認証]**&#x200B;手順で、[!DNL The Trade Desk]接続の詳細を入力する必要があります。

* **[!UICONTROL 名前]**:この宛先が将来認識される名前。
* **[!UICONTROL 説明]**:この宛先を将来特定するのに役立つ説明です。
* **[!UICONTROL アカウントID]**:アカウント [!DNL Trade Desk] [!UICONTROL ID]。
* **[!UICONTROL サーバーの場所]**:どの地域サーバーを使用する [!DNL Trade Desk] かを担当者に問い合わせてください。以下は、選択可能な地域サーバーです。

   * **[!UICONTROL ヨーロッパ]**
   * **[!UICONTROL シンガポール]**
   * **[!UICONTROL 東京]**
   * **[!UICONTROL 北米東部]**
   * **[!UICONTROL 北米西部]**
   * **[!UICONTROL ラテンアメリカ]**

* **[!UICONTROL マーケティングアクション]**:マーケティングアクションは、データをエクスポート先にエクスポートする意図を示します。Adobe定義のマーケティングアクションから選択するか、独自のマーケティングアクションを作成することができます。 マーケティング活動の詳細については、「[Adobe Experience Platform](../../../data-governance/policies/overview.md)のデータガバナンス」ページを参照してください。 Adobe定義の個々のマーケティングアクションについて詳しくは、[データ使用ポリシーの概要](../../../data-governance/policies/overview.md)を参照してください。

![トレードデスクの認証手順](../../assets/catalog/advertising/tradedesk/authenticate.png)

「**[!UICONTROL 宛先を作成]**」をクリックします。 これで宛先が作成されました。後でセグメントをアクティブにする場合は、「[!UICONTROL 保存して終了]」をクリックできます。または、「[!UICONTROL 次へ]」を選択してワークフローを続行し、アクティブにするセグメントを選択できます。 どちらの場合も、残りのワークフローについては、次の「[セグメントをアクティブにする](#activate-segments)」の節を参照してください。

## セグメントのアクティブ化 {#activate-segments}

セグメントのアクティベーションワークフローについて詳しくは、「[宛先へのプロファイルとセグメントのアクティブ化](../../ui/activate-destinations.md#select-attributes)」を参照してください。

[セグメントスケジュール](../../ui/activate-destinations.md#segment-schedule)の手順では、セグメントを宛先の対応するIDまたはフレンドリ名に手動でマップする必要があります。

セグメントをマッピングする際は、使いやすいように、[!DNL Platform]セグメント名を使用するか、それより短い形式を使用することをお勧めします。 ただし、宛先のセグメントIDまたは名前が[!DNL Platform]アカウントのセグメントIDまたは名前と一致している必要はありません。 マッピングフィールドに挿入した値は、すべて宛先に反映されます。

複数のデバイスマッピング(cookie ID、[!DNL IDFA]、[!DNL GAID])を使用する場合は、3つのマッピングすべてに同じマッピング値を使用するようにします。 [!DNL The Trade Desk] は、すべてのセグメントを1つのセグメントに集計し、デバイスレベルの分類を表示します。

![セグメントマッピングID](../../assets/common/segment-mapping-id.png)

## エクスポートされたデータ{#exported-data}

データが[!DNL The Trade Desk]宛先に正常にエクスポートされたかどうかを確認するには、[!DNL Trade Desk]アカウントを確認してください。 アクティベーションに成功すると、オーディエンスがアカウントに入力されます。
