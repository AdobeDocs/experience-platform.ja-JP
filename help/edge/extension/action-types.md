---
title: Adobe Experience PlatformWeb SDK Extensionのアクションタイプ
description: Adobe Experience Platform LaunchのAdobe Experience PlatformWeb SDK Extensionが提供する様々なアクションタイプについて説明します。
solution: Experience Platform
feature: Web SDK
exl-id: a4bf0bb9-59b4-4c43-97e6-387768176517
source-git-commit: 7e87f5b29d388b34681217e392c3f1ae8f2b67ee
workflow-type: tm+mt
source-wordcount: '547'
ht-degree: 4%

---

# アクションタイプ

[Adobe Experience Platform Launch](https://experienceleague.adobe.com/docs/launch.html)用の[Adobe Experience PlatformWeb SDK拡張機能](web-sdk-extension.md)を設定した後、アクションタイプを設定します。

このページでは、使用可能なアクションのタイプについて説明します。

## イベントの送信

Adobe Experience Platformが送信したデータを収集し、その情報に基づいて行動できるように、Adobe[!DNL Experience Platform]にイベントを送信します。 インスタンスを選択します（複数ある場合）。 **[!UICONTROL XDM Data]**&#x200B;フィールドには、任意の送信データを格納できます。 XDMスキーマの構造に準拠するJSONオブジェクトを使用します。 このオブジェクトは、ページ上に作成するか、**[!UICONTROL カスタムコード]** **[!UICONTROL データ要素]**&#x200B;を使用して作成できます。

「イベントを送信」アクションタイプには、実装に応じて役立つフィールドがいくつかあります。 これらのフィールドはすべてオプションです。

- **Type:** このフィールドでは、XDMスキーマに記録されるイベントタイプを指定できます。デフォルトのイベントタイプの詳細については、[ドキュメント](https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/tracking-events.html?lang=en#using-the-sendbeacon-api)を参照してください。
- **マージID:** イベントの [マージ](https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/merging-event-data.html?lang=en#fundamentals) IDを指定する場合は、このフィールドで指定します。現時点では、ダウンストリームのソリューションはイベントデータをマージできません。
- **データセットID：データストリームで指定したデータセット以外のデータセットにデータを送信する** 必要がある場合は、ここでそのデータセットIDを指定できます。
- **ドキュメントがアンロードします：** ユーザーがページから離れた場所に移動した場合でもイベントがサーバーに到達することを確認するには、 **[!UICONTROL ドキュメントがアンロードする]** チェックボックスをオンにします。これにより、イベントはサーバーに到達できますが、応答は無視されます。
- **視覚的なパーソナライゼーションの意思決定のレンダリング：** ページ上でパーソナライズされたコンテンツをレンダリングする場合は、「視覚的なパーソナライゼーションのデシジョンを **[!UICONTROL レンダリング」]** チェックボックスをオンにします。必要に応じて、決定範囲を指定することもできます。 パーソナライズされたコンテンツのレンダリングについて詳しくは、[パーソナライゼーションドキュメント](https://experienceleague.adobe.com/docs/experience-platform/edge/personalization/rendering-personalization-content.html?lang=en#automatically-rendering-content)を参照してください。

## 同意の設定

ユーザーから同意を受けた後、この同意は、「同意の設定」アクションタイプを使用して、Adobe Experience PlatformWeb SDKに伝える必要があります。 現在、「Adobe」と「IAB TCF」の 2 種類の標準がサポートされています。「[顧客の同意に関する基本設定のサポート](../consent/supporting-consent.md)」を参照してください。 Adobeバージョン2.0を使用する場合は、データ要素の値のみがサポートされます。 同意オブジェクトに解決されるデータ要素を作成する必要があります。

この操作では、承認を受けた後でIDを同期できるように、IDマップを含めるオプションのフィールドも提供されます。 同期は、同意が「保留」または「送信」として設定されている場合に便利です。同意の呼び出しが最初に実行される呼び出しである可能性が高いからです。

## イベント結合 ID のリセット

ページ上のイベント結合IDをリセットする場合は、この操作を行って行います。 IDをリセットするには、リセットするマージIDを選択し、必要に応じてアクションを実行します。

## 次の作業

アクションを設定した後、[データ要素の型を設定](data-element-types.md)します。
