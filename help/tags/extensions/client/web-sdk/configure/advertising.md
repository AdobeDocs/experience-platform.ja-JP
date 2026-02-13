---
title: Adobe Advertising設定
description: デマンドサイド Platform 機能を有効または無効にします。
exl-id: 594fd75d-bb13-4146-9105-1398e24c4c16
source-git-commit: 6c05d8abde0e4d6b07fe37d6e3eacd5d3dd67ec2
workflow-type: tm+mt
source-wordcount: '209'
ht-degree: 7%

---

# Adobe Advertising設定 {#advertising}

>[!AVAILABILITY]
>
>Web SDK用のAdobe Advertisingは現在 **ベータ版** です。 機能とドキュメントは変更される場合があります。

>[!CONTEXTUALHELP]
>id="platform_tags_websdk_advertising"
>title="Adobe Advertising"
>abstract="Adobe Advertising統合の設定を行います。 クリックスルー測定を有効にするために、広告の設定は必要ないことに注意してください。 検索、ソーシャル、Commerceの各クライアントでは、それ以上のアクションは必要ありません。ただし、Demand-side Platform （DSP）を使用するユーザーは、このセクションで広告主を設定して、ビュースルーコンバージョンを測定する必要があります。"

「**[!UICONTROL Adobe Advertising]**」セクションでは、実装で使用される場合にデマンドサイド Platform （DSP）機能を有効または無効にできます。 実装でDSPを使用している場合にのみ、このフィールドを設定する必要があります。

1. Adobe IDの資格情報を使用して [experience.adobe.com](https://experience.adobe.com) にログインします。
1. **[!UICONTROL Data Collection]**／**[!UICONTROL Tags]**&#x200B;に移動します。
1. 目的のタグプロパティを選択します。
1. **[!UICONTROL Extensions]** に移動し、**[!UICONTROL Configure]** カードで [!UICONTROL Adobe Experience Platform Web SDK] を選択します。
1. **[!UICONTROL Adobe Advertising]** セクションまで下にスクロールします。

現在、利用可能なオプションが 1 つあります。

## [!UICONTROL Adobe Advertising DSP]

Adobe AdvertisingのDSP機能を有効または無効にするドロップダウンメニューです。

* **[!UICONTROL Enabled]**：ビュースルートラッキングを有効にします。
* **[!UICONTROL Disabled]**：ビュースルートラッキングを無効にします。

有効になっている場合は、次の設定を使用できます。

* **[!UICONTROL Advertisers]**：ビュースルートラッキングを有効にする広告主。
* **[!UICONTROL ID5 partner ID]**：オプション。組織の ID5 パートナー ID。 この設定を使用すると、Web SDKで ID5 ユニバーサル ID を収集できます。
* **[!UICONTROL RampID JavaScript path]**：オプション。組織の [!DNL LiveRamp RampID] JavaScript コード（`ats.js`）へのパス。  この設定を使用すると、Web SDKでユニバーサル ID[!DNL RampID] 収集できます。
