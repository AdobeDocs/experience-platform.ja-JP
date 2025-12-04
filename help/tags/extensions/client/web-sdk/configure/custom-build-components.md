---
title: カスタムビルドコンポーネント
description: 機能を無効にしてビルドサイズを小さくするカスタム web SDK ビルドを作成します。
source-git-commit: d6aea91d6989775ff5b6038b216ed2518f4a7d98
workflow-type: tm+mt
source-wordcount: '257'
ht-degree: 1%

---

# カスタムビルドコンポーネント

Web SDK ライブラリには、パーソナライゼーション、ID、リンクトラッキングなど、様々な機能のための複数のモジュールが含まれています。 ユースケースによっては、ライブラリ全体ではなく、特定の機能のみが必要になる場合があります。 ビルドコンポーネントを無効にすると、必要なモジュールのみを使用して、ライブラリのサイズを小さくし、パフォーマンスを向上させることができます。

コンポーネントを無効にすると、そのコンポーネントの設定を編集できなくなります。 複数の Web SDK インスタンスを使用する場合、選択したビルドコンポーネントがすべてのインスタンスに適用されます。

1. Adobe IDの資格情報を使用して [experience.adobe.com](https://experience.adobe.com) にログインします。
1. **[!UICONTROL Data Collection]**／**[!UICONTROL Tags]**&#x200B;に移動します。
1. 目的のタグプロパティを選択します。
1. **[!UICONTROL Extensions]** に移動し、**[!UICONTROL Configure]** カードで [!UICONTROL Adobe Experience Platform Web SDK] を選択します。
1. 上部の「**[!UICONTROL Custom build components]**」アコーディオンを展開します。

>[!WARNING]
>
>これらの設定を誤って変更すると、データ損失など、望ましくない結果が生じる可能性があります。 これらの変更を実稼動環境に公開する前に、開発環境で実装を十分にテストしてください。

Adobeでは、次の web SDK ビルドコンポーネントを無効にすることができます。

| コンポーネントを作成 | 説明 | 依存機能 |
| --- | --- | --- |
| **[!UICONTROL Activity collector]** | リンクの自動収集とActivity Mapのトラッキングを可能にします。 | |
| **[!UICONTROL Advertising]** | Adobe AdvertisingとCustomer Journey Analyticsの統合を有効にします。 | |
| **[!UICONTROL Audiences]** | ID 同期など、Adobe Audience Managerとの統合をサポートします。 | |
| **[!UICONTROL Consent]** | 同意機能を使用する機能を許可します。 | [[!UICONTROL Set consent]](../actions/set-consent.md) アクション |
| **[!UICONTROL Event merge]** | 非推奨。 | [[!UICONTROL Event merge ID]](../data-element-types.md) データ要素（非推奨） <br>[[!UICONTROL Reset event merge ID]](../actions/reset-event-merge-id.md) アクション（非推奨） |
| **[!UICONTROL Media Analytics bridge]** | では、従来の Media Analytics との統合をサポートしています。 | [[!UICONTROL Get media analytics tracker]](../actions/get-media-analytics-tracker.md) アクション |
| **[!UICONTROL Personalization]** | Adobe TargetおよびAdobe Journey Optimizerとの統合をサポートします。 | [[!UICONTROL Apply propositions]](../actions/apply-propositions.md) アクション |
| **[!UICONTROL Push notifications]** | Adobe Journey Optimizerの web プッシュ通知を有効にします。 | [[!UICONTROL Send push subscription]](../actions/send-push-subscription.md) アクション |
| **[!UICONTROL Rules engine]** | Adobe Journey Optimizerでデバイス判定を有効にします。 | アクショ [[!UICONTROL Evaluate rulsets]](../actions/evaluate-rulesets.md)<br> [[!UICONTROL Subscribe ruleset items]](../event-types.md#subscribe-ruleset-items) イベント |
| **[!UICONTROL Streaming media]** | ストリーミングメディアコレクションとの統合をサポートします。 | [[!UICONTROL Send media event]](../actions/send-media-event.md) アクション |
