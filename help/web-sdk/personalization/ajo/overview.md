---
title: Experience Platform Web SDKでのAdobe Journey Optimizerの使用
description: Adobe Journey Optimizerを使用してExperience Platform web SDKでパーソナライズされたコンテンツをレンダリングする方法を説明します
keywords: ajo;ajo web;adobe journey optimizer;renderDecisions；サーフェス；決定；提案；範囲；スキーマ
exl-id: 3f28e2bc-2c4b-4400-8f69-c7316449ff4f
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '391'
ht-degree: 2%

---

# [!DNL Experience Platform Web SDK] での [!DNL Adobe Journey Optimizer] の使用

[!DNL Adobe Experience Platform] [!DNL Web SDK] は、web チャネルに [!DNL Adobe Journey Optimizer] して管理され、パーソナライズされたエクスペリエンスを配信およびレンダリングできます。 WYSIWYG エディター（[!DNL Adobe Journey Optimizer]Web チャネル [、非ビジュアルインターフェイス &#x200B;](https://experienceleague.adobe.com/docs/journey-optimizer/using/web/create-web.html?lang=ja) コードベースのエクスペリエンスチャネル [&#x200B; を使用して、[!DNL Journey Optimizer Web] しいキャンペーンとパーソナライゼーションエクスペリエンスを作成、アクティベートおよび配信で &#x200B;](https://experienceleague.adobe.com/ja/docs/journey-optimizer/using/code-based-experience/get-started-code-based) ます。

>[!IMPORTANT]
>
>エクスペリエンスのオーサリングとレポートの概要については [&#128279;](https://experienceleague.adobe.com/docs/journey-optimizer/using/web/get-started-web.html?lang=ja)Adobe Journey Optimizer web チャネルのドキュメ [!DNL Journey Optimizer Web] トをお読みください。

## 用語 {#terminology}

**[!UICONTROL サーフェス]**:Web サーフェスは、[!DNL Adobe Journey Optimizer] エクスペリエンスコンテンツが配信される Web ページまたはページ上の場所を URI で特定したものです。

**[!UICONTROL 提案]**:[!DNL Adobe Journey Optimizer] では、提案は、[!DNL Journey Optimizer Campaign] ールから選択されたエクスペリエンスに関連付けられます。

## [!DNL Adobe Journey Optimizer] の有効化 {#enable-ajo}

[!DNL Adobe Journey Optimizer] の使用を開始するには、次の手順に従います。

1. [!DNL Adobe Journey Optimizer] の [web エクスペリエンスガイド &#x200B;](https://experienceleague.adobe.com/docs/journey-optimizer/using/web/create-web.html?lang=ja#prerequesites) から [&#x200B; 前提条件 &#x200B;](https://experienceleague.adobe.com/docs/journey-optimizer/using/web/create-web.html?lang=ja) を確認します。具体的には次のとおりです。
   * [!DNL Adobe Experience Cloud Visual Editing Helper] を設定します。
   * [&#x200B; データストリーム &#x200B;](../../../datastreams/overview.md) で [!DNL Adobe Journey Optimizer] を有効にします。
   * 「[!UICONTROL Edge上でアクティブ化結合ポリシー &#x200B;]」オプションを有効にします。

2. イベントに「`renderDecisions`」オプションを追加します。 Web ページサーフェス上に配信されたJourney Optimizer コンテンツの提案を自動レンダリングするには、`renderDecisions` を `true` に設定します。

   ```javascript
   alloy("sendEvent", {
       ...,
       "renderDecisions": true
   })
   ```

3. オプションで、イベントに追加のサーフェスを指定します。 デフォルトでは、Web SDKは現在の web ページの web サーフェスを自動的に生成し、Edge Networkに対するリクエストに含めます。 必要に応じて、`sendEvent` コマンドの `personalization.surfaces` オプション、または Web SDK拡張機能の対応する **[!UICONTROL サーフェス]** [[!UICONTROL &#x200B; イベントを送信 &#x200B;] アクション &#x200B;](../../../tags/extensions/client/web-sdk/action-types.md#send-event) 設定で追加のサーフェスを指定することで、リクエストに追加のサーフェスを含めることができます。

   ```javascript
   alloy("sendEvent", {
       ...
       "personalization": {
           "surfaces": [ "web://my.site.com/about.html", "web://my.site.com/contact.html" ]
       }
   })
   ```

   ![extension-add-surface](./assets/extension-add-surface.png)

   イベントサーフェスは、`query.personalization.surfaces` リクエストフィールドに含まれています。

   ```json
   {
   "events": [
       {
           "query": {
               "personalization": {
               "schemas": [
                   ...
               ],
               "decisionScopes": [
                   "__view__"
               ],
               "surfaces": [
                   "web://ajostage.weebly.com/"
               ]
               }
           },
           ...
       }
   ]
   }
   ```

4. 他のパーソナライゼーション機能と同様に、**[事前非表示スニペット](../manage-flicker.md)** を追加して、エクスペリエンスを取得する際に、ページの特定の部分のみを非表示にすることができます。

## Adobe Journey Optimizer Web エクスペリエンスの作成 {#create-ajo-web-experiences}

[!DNL Adobe Journey Optimizer] の [Web エクスペリエンスガイド &#x200B;](https://experienceleague.adobe.com/docs/journey-optimizer/using/web/create-web.html?lang=ja#create-web-campaign) の [Web キャンペーンオーサリング &#x200B;](https://experienceleague.adobe.com/docs/journey-optimizer/using/web/create-web.html?lang=ja) 手順に従って、キャンペーンとエクスペリエンス [!DNL Journey Optimizer Web] 作成します。

## パーソナライズされたコンテンツのレンダリング {#rendering-personalized-content}

詳しくは、[&#x200B; パーソナライゼーションコンテンツのレンダリング &#x200B;](../rendering-personalization-content.md) に関するドキュメントを参照してください。

web サーフェスに対するAdobe Journey Optimizerの提案は、`__view__` の決定範囲の提案と同様の方法で処理されます。 特に、`sendEvent` コマンド `renderDecisions` オプションが `true` に設定されている場合、これらは web SDKによって自動的にレンダリングされます。

Journey Optimizer コンテンツ提案のサンプル：

```json
{
    "scope": "web://ajostage.weebly.com/",
    "scopeDetails": {
        "correlationID": "ccfaf19c-6360-4aea-b464-0cf924db5da7",
        "characteristics": {
            "eventToken": "eyJtZXNzYWdlRXhlY3V0aW9uIjp7Im1lc3NhZ2VFeGVjdXRpb25JRCI6ImEzNDYxYTMzLTc5MjktNGQyNS1hNmMxLTVkYzM2YWY1NzRmMyIsIm1lc3NhZ2VJRCI6ImNjZmFmMTljLTYzNjAtNGFlYS1iNDY0LTBjZjkyNGRiNWRhNyIsIm1lc3NhZ2VUeXBlIjoibWFya2V0aW5nIiwiY2FtcGFpZ25JRCI6IjEzN2JmMzllLWM1ODgtNGI1My1iODQxLTJiMWZiZDYxM2JkYiIsImNhbXBhaWduVmVyc2lvbklEIjoiMTA1NzY1MmEtZWYwNS00YjE3LWExMmUtY2FlOTQyOTFhMWFjIiwiY2FtcGFpZ25BY3Rpb25JRCI6ImViNTlmODQ4LTk5ZDYtNGE1OC05YmU4LTk4MjIxODU0NmYzNiIsIm1lc3NhZ2VQdWJsaWNhdGlvbklEIjoiYzg2NzFjZmItNDdjYS00YTVjLTg4Y2YtNzYwZDFlZjU1MzQyIn0sIm1lc3NhZ2VQcm9maWxlIjp7ImNoYW5uZWwiOnsiX2lkIjoiaHR0cHM6Ly9ucy5hZG9iZS5jb20veGRtL2NoYW5uZWxzL3dlYiIsIl90eXBlIjoiaHR0cHM6Ly9ucy5hZG9iZS5jb20veGRtL2NoYW5uZWwtdHlwZXMvd2ViIn0sIm1lc3NhZ2VQcm9maWxlSUQiOiI2YTViY2I3ZC02MmYxLTQ5NDItODRkMC02MzE5ZjM5Zjk1ZGUifX0="
        },
        "decisionProvider": "AJO",
        "activity": {
            "id": "137bf39e-c588-4b53-b841-2b1fbd613bdb#eb59f848-99d6-4a58-9be8-982218546f36"
        }
    },
    "id": "002321c0-dff5-4153-b171-a9dfb70b9750",
    "items": [
        {
            "schema": "https://ns.adobe.com/personalization/dom-action",
            "data": {
                "uiData": {
                    "tagType": "Text",
                    "actionType": "changed"
                },
                "content": "Welcome AJO!",
                "prehidingSelector": "#wsite-content > DIV:nth-of-type(2) > DIV:nth-of-type(1) > DIV:nth-of-type(1) > DIV:nth-of-type(1) > DIV:nth-of-type(1) > DIV:nth-of-type(3) > FONT:nth-of-type(1) > SPAN:nth-of-type(1)",
                "type": "setHtml",
                "selector": "#wsite-content > DIV.wsite-section-wrap:eq(1) > DIV.wsite-section:eq(0) > DIV.wsite-section-content:eq(0) > DIV.container:eq(0) > DIV.wsite-section-elements:eq(0) > DIV.paragraph:eq(0) > FONT:nth-of-type(1) > SPAN:nth-of-type(1)"
            },
            "id": "0a522f66-9e6a-4ded-b1d0-e9167f103290"
        },
        {
            "schema": "https://ns.adobe.com/personalization/dom-action",
            "data": {
                "uiData": {
                    "tagType": "Text",
                    "actionType": "changed"
                },
                "content": {
                    "font-weight": "bold"
                },
                "prehidingSelector": "#wsite-content > DIV:nth-of-type(2) > DIV:nth-of-type(1) > DIV:nth-of-type(1) > DIV:nth-of-type(1) > DIV:nth-of-type(1) > DIV:nth-of-type(3) > FONT:nth-of-type(1) > SPAN:nth-of-type(1)",
                "type": "setStyle",
                "selector": "#wsite-content > DIV.wsite-section-wrap:eq(1) > DIV.wsite-section:eq(0) > DIV.wsite-section-content:eq(0) > DIV.container:eq(0) > DIV.wsite-section-elements:eq(0) > DIV.paragraph:eq(0) > FONT:nth-of-type(1) > SPAN:nth-of-type(1)"
            },
            "id": "66216ca5-5d0f-4239-a8c8-6bc4a5a7cbdb"
        }
    ]
}
```

## デバッグ {#debugging}

Adobe Journey Optimizer パーソナライゼーション実装をデバッグするには、[web SDKのデバッグ &#x200B;](/help/web-sdk/use-cases/debugging.md) を使用してください。 [[!DNL Adobe Experience Platform Assurance]](https://developer.adobe.com/client-sdks/documentation/platform-assurance/) を使用したトラブルシューティングの際には、[!DNL Adobe Journey Optimizer] のデバッグトレースを利用できます。 `AJO:` プレフィックスが付いたイベントを確認します。

![assurance-ajo-trace](./assets/assurance-ajo-trace.png)
