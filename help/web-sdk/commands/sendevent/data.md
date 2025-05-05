---
title: data
description: データオブジェクトを介して XDM 以外のデータをAdobeに送信する方法を説明します。
exl-id: 537fc34e-3cda-4aa7-ae0d-0d3ef4b89848
source-git-commit: 8c652e96fa79b587c7387a4053719605df012908
workflow-type: tm+mt
source-wordcount: '372'
ht-degree: 0%

---


# `data`

`data` オブジェクトを使用すると、XDM スキーマに一致しないAdobeにペイロードを送信できます。 Adobe Analytics、Adobe Target、Adobe Audience Managerに直接データを送信するなど、XDM 以外のシナリオで役立ちます。 データがデータストリームに到達したら、[ データ準備のマッピング ](/help/data-prep/ui/mapping.md) を使用して、XDM フィールドを `data` オブジェクトの各フィールドに割り当てることができます。

>[!IMPORTANT]
>
>このオブジェクト内のデータには、次のアクションの少なくとも 1 つが必要です：
>
>* データストリームのサービスは、`data` オブジェクトの指定されたプロパティからデータを取得するように設定する必要があります。
>* 指定されたプロパティは、データ準備を使用して XDM フィールドにマッピングする必要があります。
>
>特定のプロパティが XDM フィールドにマッピングされていない場合や、設定済みのサービスで使用されていない場合、そのデータは永続的に失われます。

## Web SDK タグ拡張機能を介して `data` オブジェクトを使用します {#tag-extension}

タグルールのアクション内の「**[!UICONTROL データ]**」フィールドにデータ要素を指定します。

1. Adobe IDの資格情報を使用して [experience.adobe.com](https://experience.adobe.com) にログインします。
1. **[!UICONTROL データ収集]**/**[!UICONTROL タグ]** に移動します。
1. 目的のタグプロパティを選択します。
1. **[!UICONTROL ルール]** に移動し、目的のルールを選択します。
1. [!UICONTROL &#x200B; アクション &#x200B;] で、既存のアクションを選択するか、アクションを作成します。
1. 「[!UICONTROL &#x200B; 拡張機能 &#x200B;]」ドロップダウンフィールドを **[!UICONTROL Adobe Experience Platform Web SDK]** に設定し、「[!UICONTROL &#x200B; アクションタイプ &#x200B;] を **[!UICONTROL イベントを送信]** に設定します。
1. 目的のオブジェクトを含むデータ要素を **[!UICONTROL データ]** フィールドに指定します。
1. 「**[!UICONTROL 変更を保持]**」をクリックして、公開ワークフローを実行します。

## Web SDK JavaScript ライブラリを介して `data` オブジェクトを使用します {#library}

`sendEvent` コマンドのパラメーター内で、`data` オブジェクトを JSON オブジェクトの一部として設定します。 データストリームでマッピングするデータの場合、このオブジェクトを好きなように構造化できます。 特定のサービスで使用されるデータの場合は、オブジェクト階層がサービスの想定と一致していることを確認します。 `data` オブジェクトと [`xdm`](xdm.md) オブジェクトの両方を同じ `sendEvent` コマンドに含めることができます。

```javascript
alloy("sendEvent", {
  "data": dataObject
});
```

## Adobe Analyticsで `data` オブジェクトを使用します {#analytics}

Adobe Analyticsで `data` オブジェクトを使用すると、XDM スキーマを使用せずにレポートスイートにデータを送信できます。 変数は、[!DNL AppMeasurement] 変数と同じ構文を使用するように設定されているので、Web SDK へのアップグレードプロセスが簡素化されます。 詳しくは、Adobe Analytics導入ガイドの [Adobe Analyticsへのデータオブジェクト変数のマッピング ](https://experienceleague.adobe.com/ja/docs/analytics/implementation/aep-edge/data-var-mapping) を参照してください。
