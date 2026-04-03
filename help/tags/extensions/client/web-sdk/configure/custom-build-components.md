---
title: ビルドオプション
description: ビルドサイズを小さくする機能を無効にするカスタム Web SDK ビルドを作成します。
exl-id: 853e0a6c-0953-4e08-9a7d-334aab022583
source-git-commit: 82e41af32468febeda2dce6b471d72ef74359ea9
workflow-type: tm+mt
source-wordcount: '277'
ht-degree: 9%

---

# ビルドオプション {#build-options}

>[!CONTEXTUALHELP]
>id="platform_tags_websdk_buildoptions"
>title="ビルドオプション"
>abstract="JavaScript ライブラリからモジュールを選択的に含めたり除外したりして、ライブラリサイズを減らし、パフォーマンスを向上させます。"

Web SDKには、パーソナライゼーション、ID、リンクトラッキングなど、さまざまな機能を実行するための複数のモジュールが用意されています。 ユースケースによっては、ライブラリ全体ではなく、特定の機能だけが必要な場合があります。 ビルドコンポーネントを無効にすると、必要なモジュールのみを使用できるため、ライブラリサイズを削減し、パフォーマンスを向上できます。

コンポーネントを無効にすると、そのコンポーネントの設定を編集できなくなります。 複数のWeb SDK インスタンスを使用する場合、選択したビルドコンポーネントはすべてのインスタンスに適用されます。

1. Adobe IDの資格情報を使用して[experience.adobe.com](https://experience.adobe.com)にログインします。
1. **[!UICONTROL Data Collection]**／**[!UICONTROL Tags]**&#x200B;に移動します。
1. 目的のタグプロパティを選択します。
1. **[!UICONTROL Extensions]**&#x200B;に移動し、**[!UICONTROL Configure]** カードの[!UICONTROL Adobe Experience Platform Web SDK]を選択します。
1. 上部の&#x200B;**[!UICONTROL Custom build components]** アコーディオンを展開します。

>[!WARNING]
>
>これらの設定を誤って変更すると、データの損失など、望ましくない結果が生じる可能性があります。 本番環境に変更を公開する前に、開発環境での実装を徹底的にテストします。

Adobeでは、次のWeb SDK ビルドコンポーネントを無効にすることができます。

| ビルドコンポーネント | 説明 | 依存機能 |
| --- | --- | --- |
| **[!UICONTROL Activity collector]** | 自動リンク収集とActivity Map トラッキングを可能にします。 | |
| **[!UICONTROL Advertising]** | Customer Journey AnalyticsとAdobe Advertisingの統合を有効にします。 | |
| **[!UICONTROL Audiences]** | IDの同期など、Adobe Audience Managerとの統合をサポートします。 | |
| **[!UICONTROL Brand concierge]** | Brand Conciergeとの統合が可能です。 | |
| **[!UICONTROL Consent]** | 同意機能を使用できます。 | [[!UICONTROL Set consent]](../actions/set-consent.md) アクション |
| **[!UICONTROL Event merge]** | 非推奨（廃止予定）: | [[!UICONTROL Event merge ID]](../data-element-types.md) データ要素（非推奨） <br>[[!UICONTROL Reset event merge ID]](../actions/reset-event-merge-id.md) アクション（非推奨） |
| **[!UICONTROL Media Analytics bridge]** | 従来のMedia Analyticsとの統合をサポートしています。 | [[!UICONTROL Get media analytics tracker]](../actions/get-media-analytics-tracker.md) アクション |
| **[!UICONTROL Personalization]** | Adobe TargetおよびAdobe Journey Optimizerとの統合をサポートしています。 | [[!UICONTROL Apply propositions]](../actions/apply-propositions.md) アクション |
| **[!UICONTROL Push notifications]** | Adobe Journey Optimizerのweb プッシュ通知を有効にします。 | [[!UICONTROL Send push subscription]](../actions/send-push-subscription.md) アクション |
| **[!UICONTROL Rules engine]** | Adobe Journey Optimizerでデバイス決定を有効にします。 | [[!UICONTROL Evaluate rulsets]](../actions/evaluate-rulesets.md) アクション <br> [[!UICONTROL Subscribe ruleset items]](../event-types.md#subscribe-ruleset-items) イベント |
| **[!UICONTROL Streaming media]** | ストリーミングメディア収集との統合をサポートしています。 | [[!UICONTROL Send media event]](../actions/send-media-event.md) アクション |
