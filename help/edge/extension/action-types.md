---
title: Adobe Experience Platform Web SDK拡張機能のアクションタイプ
description: Adobe Experience Platform Web SDKタグ拡張機能で提供される様々なアクションタイプについて説明します。
solution: Experience Platform
feature: Web SDK
exl-id: a4bf0bb9-59b4-4c43-97e6-387768176517
source-git-commit: 67b73321b8e147b934ad4015f03c9a5364f2b9ea
workflow-type: tm+mt
source-wordcount: '533'
ht-degree: 3%

---

# アクションタイプ

[Adobe Experience Platform Web SDKタグ拡張](web-sdk-extension-configuration.md)を設定した後、アクションタイプを設定します。

このページでは、使用可能なアクションタイプについて説明します。


## イベントの送信

Adobe[!DNL Experience Platform]にイベントを送信し、Adobe Experience Platformが送信したデータを収集して、その情報に基づいて行動できるようにします。 インスタンスを選択します（複数ある場合）。 **[!UICONTROL XDM Data]**&#x200B;フィールドには、送信する任意のデータを送信できます。 XDMスキーマの構造に準拠するJSONオブジェクトを使用します。 このオブジェクトは、ページ上または&#x200B;**[!UICONTROL カスタムコード]** **[!UICONTROL データ要素]**&#x200B;を使用して作成できます。

「イベントを送信」アクションタイプには、実装によっては他にも便利なフィールドがいくつかあります。 これらのフィールドはすべてオプションです。

- **タイプ：** このフィールドでは、XDMスキーマに記録されるイベントタイプを指定できます。デフォルトのイベントタイプについて詳しくは、[ドキュメント](https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/tracking-events.html?lang=en#using-the-sendbeacon-api)を参照してください。
- **データ：** XDMスキーマと一致しないデータは、このフィールドを使用して送信できます。このフィールドは、Adobe Targetプロファイルを更新しようとする場合や、Target Recommendations属性を送信しようとする場合に役立ちます。 例については、[ドキュメント](https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/tracking-events.html?lang=en)を参照してください。
<!--- **Merge ID:** If you would like to specify a merge ID for your event, you can do so in this field. Please note that the solutions downstream are not able to merge your event data at this time. -->
- **データセットID:** データストリームで指定したデータセット以外のデータセットにデータを送信する必要がある場合は、ここでそのデータセットIDを指定できます。
- **ドキュメントのアンロード：** ユーザーがページから離れた場合でもイベントがサーバーに到達することを確認するには、「ドキュメントはアンロードします」チェックボッ **[!UICONTROL クスをオン]** にします。これにより、イベントはサーバーに到達できますが、応答は無視されます。
- **視覚的なパーソナライゼーションの決定をレンダリングする：** ページ上でパーソナライズされたコンテンツをレンダリングする場合は、「視覚的なパーソナライゼーションの決定 **[!UICONTROL をレンダリング」チェックボ]** ックスをオンにします。必要に応じて、判定範囲を指定することもできます。 パーソナライズされたコンテンツのレンダリングについて詳しくは、[パーソナライゼーションのドキュメント](https://experienceleague.adobe.com/docs/experience-platform/edge/personalization/rendering-personalization-content.html?lang=en#automatically-rendering-content)を参照してください。

## 同意の設定

ユーザーの同意を得たら、「同意を設定」アクションタイプを使用して、この同意をAdobe Experience Platform Web SDKに伝える必要があります。 現在、「Adobe」と「IAB TCF」の 2 種類の標準がサポートされています。[顧客の同意設定のサポート](../consent/supporting-consent.md)を参照してください。 Adobeバージョン2.0を使用する場合、データ要素の値のみがサポートされます。 同意オブジェクトに解決されるデータ要素を作成する必要があります。

このアクションでは、IDマップを含めるためのオプションのフィールドも提供され、同意を受けた後にIDを同期できます。 同期は、同意の呼び出しが最初に実行される可能性が高いので、同意が「保留」または「送信」と設定されている場合に役立ちます。

## イベント結合 ID のリセット

ページ上のイベント結合IDをリセットする場合は、このアクションを使用します。 IDをリセットするには、リセットする結合IDを選択し、必要に応じてアクションを実行します。

## 次の作業

アクションを設定した後、[データ要素のタイプ](data-element-types.md)を設定します。
