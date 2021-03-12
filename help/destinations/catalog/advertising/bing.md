---
keywords: '広告；bing; '
title: Microsoft Bing接続
description: Microsoft Bingの接続先で、Microsoftディスプレイ広告全体に対して、ターゲットを設定したデジタルキャンペーンの再ターゲット化とオーディエンスを実行できます。
translation-type: tm+mt
source-git-commit: 0ef107963f7da377070eb845fd7c24218a99464b
workflow-type: tm+mt
source-wordcount: '527'
ht-degree: 5%

---


# [!DNL Microsoft Bing] connection  {#bing-destination}

[!DNL Microsoft Bing]宛先は、プロファイルデータを[!DNL Microsoft Display Advertising]に送信するのに役立ちます。

プロファイルデータを[!DNL Microsoft Bing]に送信するには、まず宛先に接続する必要があります。

## 宛先の仕様 {#destination-specs}

[!DNL Microsoft Bing]宛先に固有の次の詳細をメモしておきます。

* 次の[ID](../../../identity-service/namespaces.md)を[!DNL Microsoft Bing]宛先に送信できます。[!DNL Microsoft ID]。

>[!IMPORTANT]
>
>[!DNL Microsoft Bing]で最初の宛先を作成したい場合で、以前(Adobe Audience Managerや他のアプリケーションで)Experience CloudIDサービスで[ID同期機能](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/methods/idsync.html)を有効にしていない場合は、Adobeコンサルティングかカスタマーケアにご連絡ください。 以前にAudience Managerで[!DNL Microsoft Bing]統合を設定していた場合、設定したID同期はPlatformに持ち越します。

## 使用例 {#use-cases}

マーケティング担当者として、[!DNL Microsoft Advertising IDs]から構築されたセグメントを[!DNL Microsoft Advertising]チャネルのディスプレイ広告を通じてターゲットに使用できるようにしたいと思います。

## エクスポートタイプ{#export-type}

**[!DNL Segment Export]**  — セグメント(オーディエンス)のすべてのメンバーを [!DNL Microsoft Bing] 宛先にエクスポートします。

## 前提条件 {#prerequisites}

宛先を設定する際は、次の情報を入力する必要があります。

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
* **[!UICONTROL マーケティングアクション]**:マーケティングアクションは、データをエクスポート先にエクスポートする意図を示します。Adobe定義のマーケティングアクションから選択するか、独自のマーケティングアクションを作成することができます。 マーケティング活動の詳細については、「[Adobe Experience Platform](../../../data-governance/policies/overview.md)のデータガバナンス」ページを参照してください。 Adobe定義の個々のマーケティングアクションについて詳しくは、[データ使用ポリシーの概要](../../../data-governance/policies/overview.md)を参照してください。

![Microsoft Bing宛先認証](../../assets/catalog/advertising/bing/authentication.png)

「**[!UICONTROL 宛先を作成]**」をクリックします。 これで宛先が作成されました。後でセグメントをアクティブにする場合は、[[!UICONTROL 保存して終了]]をクリックします。または、[[!UICONTROL 次へ]]をクリックしてワークフローを続行し、アクティブにするセグメントを選択します。 どちらの場合も、残りのワークフローについては、次の「[セグメントをアクティブにする](#activate-segments)」の節を参照してください。

## セグメントのアクティブ化 {#activate-segments}

セグメントのアクティベーションワークフローについて詳しくは、「[宛先へのプロファイルとセグメントのアクティブ化](../../ui/activate-destinations.md#select-attributes)」を参照してください。

[セグメントスケジュール](../../ui/activate-destinations.md#segment-schedule)の手順では、セグメントを宛先の対応するIDまたはフレンドリ名に手動でマップする必要があります。

セグメントをマッピングする際は、使いやすいように、[!DNL Platform]セグメント名を使用するか、それより短い形式を使用することをお勧めします。 ただし、宛先のセグメントIDまたは名前が[!DNL Platform]アカウントのセグメントIDまたは名前と一致している必要はありません。 マッピングフィールドに挿入した値は、すべて宛先に反映されます。

![セグメントマッピングID](../../assets/common/segment-mapping-id.png)

## エクスポートされたデータ{#exported-data}

データが[!DNL Microsoft Bing]宛先に正常にエクスポートされたかどうかを確認するには、[!DNL Microsoft Bing Ads]アカウントを確認してください。 アクティベーションに成功すると、オーディエンスがアカウントに入力されます。