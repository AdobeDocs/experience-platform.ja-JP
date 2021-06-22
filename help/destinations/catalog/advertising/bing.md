---
keywords: '広告；bing; '
title: Microsoft Bing接続
description: Microsoft Bingの接続先を使用すると、Microsoftディスプレイ広告全体で再ターゲティングとオーディエンスターゲティングされたデジタルキャンペーンを実行できます。
exl-id: e1c0273b-7e3c-4d77-ae14-d1e528ca0294
source-git-commit: 2931efa6f67a042255fb1d31c0683f73d817b55b
workflow-type: tm+mt
source-wordcount: '533'
ht-degree: 5%

---

# [!DNL Microsoft Bing] 接続  {#bing-destination}

## 概要 {#overview}

[!DNL Microsoft Bing]の宛先は、プロファイルデータを[!DNL Microsoft Display Advertising]に送信する際に役立ちます。

プロファイルデータを[!DNL Microsoft Bing]に送信するには、まず宛先に接続する必要があります。

## ユースケース {#use-cases}

マーケティング担当者の場合、[!DNL Microsoft Advertising IDs]で構築されたセグメントを使用して、[!DNL Microsoft Advertising]チャネルをまたいだディスプレイ広告を通じてユーザーをターゲットにできるようにしたいと考えています。

## サポートされているID{#supported-identities}

[!DNL Microsoft Bing] では、以下の表で説明するIDのアクティブ化をサポートしています。[ID](/help/identity-service/namespaces.md)の詳細を説明します。

| ターゲットID | 説明 |
|---|---|
| MAID | Microsoft Advertising ID |

## エクスポートタイプ{#export-type}

**[!DNL Segment Export]** ：セグメント（オーディエンス）のすべてのメンバーを宛先に書き出し [!DNL Microsoft Bing] ます。

## 前提条件 {#prerequisites}

[!DNL Microsoft Bing]で最初の宛先を作成したい場合で、(Adobe Audience Managerや他のアプリケーションで)以前にExperience CloudIDサービスで[ID同期機能](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/methods/idsync.html)を有効にしていない場合は、Adobeコンサルティングまたはカスタマーケアに連絡してID同期を有効にしてください。 以前にAudience Managerで[!DNL Microsoft Bing]統合を設定していた場合、設定したID同期はPlatformに引き継がれます。

宛先を設定する際は、次の情報を指定する必要があります。

* [!UICONTROL アカウントID]:これは、整 [!DNL Bing Ads CID]数形式のです。

## 宛先に接続 {#connect-destination}

**[!UICONTROL 接続]** > **[!UICONTROL 宛先]**&#x200B;で、「[!DNL Microsoft Bing]」を選択し、「**[!UICONTROL 設定]**」を選択します。

![Microsoft Bingの宛先の設定](../../assets/catalog/advertising/bing/configure.png)

この宛先との接続が既に存在する場合は、宛先カードに「**[!UICONTROL アクティブ化]**」ボタンが表示されます。 **[!UICONTROL アクティブ化]**&#x200B;と&#x200B;**[!UICONTROL 設定]**&#x200B;の違いについて詳しくは、宛先ワークスペースのドキュメントの[カタログ](../../ui/destinations-workspace.md#catalog)の節を参照してください。

![Microsoft Bingの宛先のアクティブ化](../../assets/catalog/advertising/bing/activate.png)

## 認証手順 {#authentication}

**[!UICONTROL 認証]**&#x200B;手順で、宛先接続の詳細を入力する必要があります。

* **[!UICONTROL 名前]**:将来この宛先を認識するための名前。
* **[!UICONTROL 説明]**:今後この宛先を識別するのに役立つ説明。
* **[!UICONTROL アカウントID]**:お使いの [!DNL Bing Ads CID].
* **[!UICONTROL マーケティングアクション]**:マーケティングアクションは、宛先にデータを書き出す目的を示します。Adobe定義のマーケティングアクションから選択することも、独自のマーケティングアクションを作成することもできます。 マーケティングアクションについて詳しくは、「Adobe Experience Platformの[データガバナンス](../../../data-governance/policies/overview.md) 」ページを参照してください。 個々のAdobe定義マーケティングアクションについて詳しくは、「[データ使用ポリシーの概要](../../../data-governance/policies/overview.md)」を参照してください。

![Microsoft Bingの宛先認証](../../assets/catalog/advertising/bing/authentication.png)

「**[!UICONTROL 宛先を作成]**」をクリックします。 これで宛先が作成されました。後でセグメントをアクティブにする場合は、「[!UICONTROL 保存して終了]」をクリックします。または、「[!UICONTROL 次へ]」をクリックしてワークフローを続行し、アクティブ化するセグメントを選択します。 どちらの場合も、残りのワークフローについては、次の「[セグメントのアクティブ化](#activate-segments)」の節を参照してください。

## セグメントのアクティブ化 {#activate-segments}

セグメントのアクティベーションワークフローについて詳しくは、「[宛先へのプロファイルとセグメントのアクティブ化](../../ui/activate-destinations.md#select-attributes)」を参照してください。

[セグメントスケジュール](../../ui/activate-destinations.md#segment-schedule)の手順で、宛先の対応するIDまたはわかりやすい名前にセグメントを手動でマッピングする必要があります。

セグメントをマッピングする場合は、使いやすくするために、[!DNL Platform]セグメント名または短い形式を使用することをお勧めします。 ただし、宛先のセグメントIDまたは名前が[!DNL Platform]アカウントのIDと一致する必要はありません。 マッピングフィールドに挿入した値は、宛先に反映されます。

![Segment Mapping ID](../../assets/common/segment-mapping-id.png)

## エクスポートされたデータ{#exported-data}

データが[!DNL Microsoft Bing]の宛先に正常に書き出されたかどうかを確認するには、[!DNL Microsoft Bing Ads]アカウントを確認します。 アクティブ化に成功した場合、オーディエンスがアカウントに入力されます。
