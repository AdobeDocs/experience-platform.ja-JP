---
title: Adobe Advertising 設定
description: デマンドサイドプラットフォーム機能を有効または無効にします。
exl-id: 594fd75d-bb13-4146-9105-1398e24c4c16
source-git-commit: 6c05d8abde0e4d6b07fe37d6e3eacd5d3dd67ec2
workflow-type: tm+mt
source-wordcount: '219'
ht-degree: 31%

---

# Adobe Advertising 設定 {#advertising}

>[!AVAILABILITY]
>
>Web SDK用Adobe Advertisingは現在&#x200B;**ベータ版**&#x200B;です。 機能とドキュメントは変更される場合があります。

>[!CONTEXTUALHELP]
>id="platform_tags_websdk_advertising"
>title="Adobe Advertising"
>abstract="Adobe Advertising 統合の設定を指定します。 クリックスルー測定を有効にするために、広告設定は不要です。 検索、ソーシャル、コマースの各クライアントは、これ以上のアクションは必要ありません。ただし、ディスプレイ広告（DSP）のユーザーは、ビュースルーコンバージョンを測定するために、このセクションで広告主を設定する必要があります。"

**[!UICONTROL Adobe Advertising]** セクションでは、実装で使用する場合、デマンドサイドプラットフォーム （DSP）機能を有効または無効にできます。 このフィールドを設定する必要があるのは、実装でDSPを使用している場合のみです。

1. Adobe IDの資格情報を使用して[experience.adobe.com](https://experience.adobe.com)にログインします。
1. **[!UICONTROL Data Collection]**／**[!UICONTROL Tags]**&#x200B;に移動します。
1. 目的のタグプロパティを選択します。
1. **[!UICONTROL Extensions]**&#x200B;に移動し、[!UICONTROL Adobe Experience Platform Web SDK] カードの&#x200B;**[!UICONTROL Configure]**&#x200B;を選択します。
1. **[!UICONTROL Adobe Advertising]** セクションまでスクロールします。

現在、利用可能なオプションが1つあります。

## [!UICONTROL Adobe Advertising DSP]

Adobe AdvertisingのDSP機能を有効または無効にするドロップダウンメニュー。

* **[!UICONTROL Enabled]**: ビュースルートラッキングを有効にします。
* **[!UICONTROL Disabled]**: ビュースルートラッキングを無効にします。

有効にすると、次の設定を使用できます。

* **[!UICONTROL Advertisers]**: ビュースルートラッキングを有効にする広告主。
* **[!UICONTROL ID5 partner ID]**：オプション。 組織のID5 パートナーID。 この設定を使用すると、Web SDKでID5のユニバーサル IDを収集できます。
* **[!UICONTROL RampID JavaScript path]**：オプション。 組織の[!DNL LiveRamp RampID] JavaScript コード （`ats.js`）へのパス。  この設定を使用すると、Web SDKで[!DNL RampID]個のユニバーサル IDを収集できます。
