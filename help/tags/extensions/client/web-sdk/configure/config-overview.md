---
title: 設定の概要
description: Web SDK タグ拡張機能の設定時に使用できるオプションについて説明します。
source-git-commit: 5f0203cfff3cb5c8b892142ff9b1c121925c3c46
workflow-type: tm+mt
source-wordcount: '214'
ht-degree: 1%

---

# 設定の概要

Adobe Experience Platform Web SDK タグ拡張機能には、カスタマイズ可能なオプションがいくつか用意されています。 これらの設定は、JavaScript ライブラリで [`configure`](/help/collection/js/commands/configure/overview.md) コマンドを使用する場合のタグと同等です。

1. Adobe IDの資格情報を使用して [experience.adobe.com](https://experience.adobe.com) にログインします。
1. **[!UICONTROL Data Collection]**／**[!UICONTROL Tags]**&#x200B;に移動します。
1. 目的のタグプロパティを選択します。
1. **[!UICONTROL Extensions]** に移動し、**[!UICONTROL Configure]** カードで [!UICONTROL Adobe Experience Platform Web SDK] を選択します。

## カスタムビルドコンポーネント

組織にとってビルドサイズの最適化が優先される場合、拡張機能のビルドサイズを小さくするために使用しない特定の機能を無効にすることができます。 詳しくは、[&#x200B; カスタムビルドコンポーネント &#x200B;](custom-build-components.md) を参照してください。

## SDK インスタンス

ほとんどの実装には、通常、1 つのSDK インスタンスが必要です。 ただし、組織で複数の web SDK トラッキングインスタンスが必要な場合は、「**[!UICONTROL Add instance]**」ボタンを使用できます。 各 Web SDK タグインスタンスの設定では、次の包括的なセクションを使用できます。

* [**[!UICONTROL SDK instance]**](general.md): インスタンスの一般設定。 このセクションのフィールドはすべて必須です。
* [**[!UICONTROL Datastreams]**](datastreams.md)：各タグ環境のデータの移動先。
* [**[!UICONTROL Consent]**](consent.md)：拡張機能のデフォルトの同意設定。
* [**[!UICONTROL Identity]**](identity.md):Cookie と訪問者の移行設定。
* [**[!UICONTROL Personalization]**](personalization.md)：個々のレベルで訪問者のエクスペリエンスをカスタマイズします。
* [**[!UICONTROL Data collection]**](data-collection.md)：自動的に収集される内容を含めるか除外します。
* [**[!UICONTROL Streaming media]**](streaming-media.md)：ストリーミングメディアコレクションに固有の設定。
* [**[!UICONTROL Datastream configuration overrides]**](configuration-overrides.md)：特定の条件が満たされた場合に設定を変更します。
* [**[!UICONTROL Advanced settings]**](advanced-settings.md):Edge Networkのベースパスを指定します。
