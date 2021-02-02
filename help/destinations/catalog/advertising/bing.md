---
keywords: '広告；bing; '
title: Microsoft Bingの宛先
seo-title: Microsoft Bingの送信先は、プロファイルデータをMicrosoftディスプレイ広告に送信する際に役立ちます。
description: Microsoft Bingのリンク先では、Microsoftディスプレイ広告全体で、ターゲットを設定したデジタルキャンペーンの再ターゲット化とオーディエンスを実行できます。
seo-description: Microsoft Bingのリンク先では、Microsoftディスプレイ広告全体で、ターゲットを設定したデジタルキャンペーンの再ターゲット化とオーディエンスを実行できます。
translation-type: tm+mt
source-git-commit: 95f57f9d1b3eeb0b16ba209b9774bd94f5758009
workflow-type: tm+mt
source-wordcount: '491'
ht-degree: 6%

---


# [!DNL Microsoft Bing] destination

## 概要 {#overview}

[!DNL Microsoft Bing]宛先は、プロファイルデータを[!DNL Microsoft Display Advertising]に送信するのに役立ちます。

プロファイルデータを[!DNL Microsoft Bing]に送信するには、まず宛先に接続する必要があります。

## 宛先の仕様 {#destination-specs}

[!DNL Microsoft Bing]宛先に固有の次の詳細をメモしておきます。

* 次の[ID](../../../identity-service/namespaces.md)を[!DNL Microsoft Bing]宛先に送信できます。[!DNL Microsoft ID]。

## 使用例 {#use-cases}

マーケティング担当者として、[!DNL Microsoft Advertising IDs]から構築されたセグメントを[!DNL Microsoft Advertising]チャネルのディスプレイ広告を通じてターゲットに使用できるようにしたいと思います。

## エクスポートタイプ{#export-type}

**[!DNL Segment Export]**  — セグメント(オーディエンス)のすべてのメンバーを [!DNL Microsoft Bing] 宛先にエクスポートします。

## 前提条件 {#prerequisites}

宛先を設定する際に、次の情報を入力するように求められます。

* [!UICONTROL アカウントID]:これは、整数形式 [!DNL Bing Ads CID]のユーザーのものです。

## 宛先に接続 {#connect-destination}

**[!UICONTROL 接続]**/**[!UICONTROL 宛先]**&#x200B;で、[!DNL Microsoft Bing]を選択し、**[!UICONTROL 設定]**&#x200B;を選択します。

![Microsoft Bingの宛先の設定](../../assets/catalog/advertising/bing/configure.png)

>[!NOTE]
>
>この宛先との接続が既に存在する場合は、宛先カードに「**[!UICONTROL Activate]**」ボタンが表示されます。 「**[!UICONTROL アクティブ化]**」と「**[!UICONTROL 設定]**」の違いについて詳しくは、保存先のワークスペースドキュメントの「[カタログ](../../ui/destinations-workspace.md#catalog)」の節を参照してください。
>
>![Microsoft Bingの宛先のアクティブ化](../../assets/catalog/advertising/bing/activate.png)

[!UICONTROL 認証]手順で、宛先接続の詳細を入力する必要があります。

* **[!UICONTROL 名前]**:この宛先が将来認識される名前。
* **[!UICONTROL 説明]**:この宛先を将来特定するのに役立つ説明です。
* **[!UICONTROL アカウントID]**:あなたの [!DNL Bing Ads CID]。
* **[!UICONTROL マーケティングの使用例]**:マーケティングの使用例は、データがエクスポート先にエクスポートされる意図を示します。Adobe定義のマーケティングの使用例から選択するか、独自のマーケティングの使用例を作成することができます。 マーケティングの使用例について詳しくは、[Adobe Experience Platform](../../../data-governance/policies/overview.md)のデータガバナンスのページを参照してください。 個々のAdobe定義マーケティングの使用例について詳しくは、[データ使用ポリシーの概要](../../../data-governance/policies/overview.md)を参照してください。

![Microsoft Bing宛先認証](../../assets/catalog/advertising/bing/authentication.png)

「**[!UICONTROL 宛先を作成]**」をクリックします。 これで宛先が作成されました。後でセグメントをアクティブにする場合は、[[!UICONTROL 保存して終了]]をクリックします。または、[[!UICONTROL 次へ]]をクリックしてワークフローを続行し、アクティブにするセグメントを選択します。 どちらの場合も、残りのワークフローについては、次の「[セグメントをアクティブにする](#activate-segments)」の節を参照してください。

## セグメントのアクティブ化 {#activate-segments}

セグメントのアクティベーションワークフローについて詳しくは、「[宛先へのプロファイルとセグメントのアクティブ化](../../ui/activate-destinations.md#select-attributes)」を参照してください。

[セグメントスケジュール](../../ui/activate-destinations.md#segment-schedule)の手順では、セグメントを宛先の対応するIDまたはフレンドリ名に手動でマップする必要があります。

セグメントをマッピングする際は、使いやすいように、[!DNL Platform]セグメント名を使用するか、それより短い形式を使用することをお勧めします。 ただし、宛先のセグメントIDまたは名前が[!DNL Platform]アカウントのセグメントIDまたは名前と一致している必要はありません。 マッピングフィールドに挿入した値は、すべて宛先に反映されます。

![セグメントマッピングID](../../assets/common/segment-mapping-id.png)

## エクスポートされたデータ{#exported-data}

データが[!DNL Microsoft Bing]宛先に正常にエクスポートされたかどうかを確認するには、[!DNL Microsoft Bing Ads]アカウントを確認してください。 アクティベーションに成功すると、オーディエンスがアカウントに入力されます。