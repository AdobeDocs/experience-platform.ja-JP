---
title: Adobe Experience Platform Web SDK 拡張機能のアクションタイプ
description: Adobe Experience Platform Web SDK タグ拡張機能で提供される様々なアクションタイプについて説明します。
solution: Experience Platform
exl-id: a4bf0bb9-59b4-4c43-97e6-387768176517
source-git-commit: db7700d5c504e484f9571bbb82ff096497d0c96e
workflow-type: tm+mt
source-wordcount: '821'
ht-degree: 4%

---


# アクションタイプ

設定後、 [Adobe Experience Platform Web SDK タグ拡張機能](web-sdk-extension-configuration.md)を使用する場合は、アクションタイプを設定する必要があります。

このページでは、 [Adobe Experience Platform Web SDK タグ拡張機能](web-sdk-extension-configuration.md).

## イベントを送信 {#send-event}

イベントをAdobeに送信 [!DNL Experience Platform] Adobe Experience Platformが送信したデータを収集し、その情報に基づいて行動できるようにする。 インスタンスを選択します（複数ある場合）。 送信する任意のデータを **[!UICONTROL XDM データ]** フィールドに入力します。 XDM スキーマの構造に準拠する JSON オブジェクトを使用します。 このオブジェクトは、ページ上または **[!UICONTROL カスタムコード]** **[!UICONTROL データ要素]**.

「イベントを送信」アクションタイプには、実装に応じて役立つその他のフィールドがいくつかあります。 これらのフィールドはすべてオプションです。

- **タイプ：** このフィールドでは、XDM スキーマに記録されるイベントタイプを指定できます。 詳しくは、 [ドキュメント](https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/tracking-events.html?lang=ja#using-the-sendbeacon-api) を参照してください。
- **データ：** XDM スキーマと一致しないデータは、このフィールドを使用して送信できます。 このフィールドは、Adobe Targetプロファイルを更新しようとする場合や、Target Recommendations属性を送信しようとする場合に役立ちます。 例については、 [ドキュメント](https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/tracking-events.html?lang=ja).<!--- **Merge ID:** If you would like to specify a merge ID for your event, you can do so in this field. Please note that the solutions downstream are not able to merge your event data at this time. -->
- **データセット ID:** データストリームで指定したデータセット以外のデータセットにデータを送信する必要がある場合は、ここでそのデータセット ID を指定できます。
- **ドキュメントをアンロードします：** ユーザーがページから離れてもイベントがサーバーに到達することを確認するには、 **[!UICONTROL ドキュメントはアンロードされます]** チェックボックス。 これにより、イベントがサーバーに到達できますが、応答は無視されます。
- **視覚的なパーソナライゼーションの決定のレンダリング：** ページ上でパーソナライズされたコンテンツをレンダリングする場合は、 **[!UICONTROL 視覚的なパーソナライゼーションの決定をレンダリング]** チェックボックス。 必要に応じて、決定範囲やサーフェスを指定することもできます。 詳しくは、 [パーソナライゼーションドキュメント](../personalization/rendering-personalization-content.md#automatically-rendering-content) パーソナライズされたコンテンツのレンダリングについて詳しくは、を参照してください。

## 同意の設定 {#set-consent}

ユーザーから同意を得たら、「同意の設定」アクションタイプを使用して、この同意をAdobe Experience Platform Web SDK に伝える必要があります。 現在、「Adobe」と「IAB TCF」の 2 種類の標準がサポートされています。詳しくは、 [顧客の同意設定のサポート](../consent/supporting-consent.md). Adobeバージョン 2.0 を使用する場合、データ要素の値のみがサポートされます。 同意オブジェクトに解決するデータ要素を作成する必要があります。

このアクションには、ID マップを含めるためのオプションのフィールドも用意されています。これにより、同意を受け取った後に ID を同期することができます。 同期は、同意の呼び出しが最初に実行される可能性が高いので、同意が「保留」または「送信」として設定されている場合に役立ちます。

## イベント結合 ID をリセット {#reset-event-merge-id}

ページ上のイベント結合 ID をリセットする場合は、このアクションで実行できます。 ID をリセットするには、リセットする結合 ID を選択し、必要に応じてアクションを実行します。

## （ベータ版）変数を更新 {#update-variable}

>[!IMPORTANT]
>
>現在はベータ版機能で、変更される可能性があります。 将来のバージョンでは、重大な変更が含まれる可能性があります。

イベントの結果としての XDM オブジェクトを変更するには、このアクションを使用します。 このアクションは、後でから参照できるオブジェクトを構築するために作成されます。 **[!UICONTROL イベントを送信]** アクションを使用して、イベント XDM オブジェクトを記録します。

このアクションタイプを使用するには、 [変数](data-element-types.md#variable) データ要素。 変更する変数データ要素を選択すると、 [XDM オブジェクト](data-element-types.md#xdm-object) データ要素。

![](./assets/update-variable.png)

エディターに使用される XDM スキーマは、で選択されたスキーマです [!UICONTROL 変数] データ要素。 左側のツリーのプロパティの 1 つをクリックし、右側の値を変更することで、オブジェクトの 1 つ以上のプロパティを設定できます。例えば、以下のスクリーンショットでは、produsedBy プロパティがデータ要素「Produced by data element」に設定されています。

![](./assets/update-variable-set-property.png)

変数更新アクションのエディターと XDM オブジェクトデータ要素のエディターでは、いくつか違いがあります。 まず、変数の更新アクションに、「xdm」というラベルの付いたルートレベル項目があります。 この項目をクリックすると、オブジェクト全体の設定に使用するデータ要素を指定できます。 次に、変数の更新アクションには、xdm オブジェクトからデータをクリアするチェックボックスがあります。 左側のプロパティの 1 つをクリックし、右側のチェックボックスをオンにして値をクリアします。 これにより、変数に値を設定する前に現在の値がクリアされます。

## 次の手順 {#next-steps}

この記事を読むと、アクションの設定方法をより深く理解できるはずです。 次に、 [データ要素タイプの設定](data-element-types.md).
