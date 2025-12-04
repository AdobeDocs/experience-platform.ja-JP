---
title: data
description: データオブジェクトを介して XDM 以外のデータをAdobeに送信する方法を説明します。
exl-id: 537fc34e-3cda-4aa7-ae0d-0d3ef4b89848
source-git-commit: f4a2778c71ad6a48621212f3ece1776d1b3ac643
workflow-type: tm+mt
source-wordcount: '311'
ht-degree: 0%

---

# `data`

`data` オブジェクトを使用すると、XDM スキーマに一致しないAdobeにペイロードを送信できます。 Adobe Analytics、Adobe Target、Adobe Audience Managerに直接データを送信するなど、XDM 以外のシナリオで役立ちます。 データがデータストリームに到達したら、[&#x200B; データ準備のマッピング &#x200B;](/help/data-prep/ui/mapping.md) を使用して、XDM フィールドを `data` オブジェクトの各フィールドに割り当てることができます。 商品が `data` オブジェクト内のフィールドを検出するようにAdobeによって既に設定されている場合は、そのデータをそのままデータストリームに送信できます。

>[!IMPORTANT]
>
>このオブジェクト内のデータには、次のアクションの少なくとも 1 つが必要です：
>
>* データストリームのサービスは、`data` オブジェクトの指定されたプロパティからデータを取得するように設定する必要があります。
>* 指定されたプロパティは、データ準備を使用して XDM フィールドにマッピングする必要があります。
>
>特定のプロパティが XDM フィールドにマッピングされていない場合や、設定済みのサービスで使用されていない場合、そのデータは永続的に失われます。

`data` コマンドのパラメーター内で、`sendEvent` オブジェクトを JSON オブジェクトの一部として設定します。 データストリームでマッピングするデータの場合、このオブジェクトを好きなように構造化できます。 特定のサービスで使用されるデータの場合は、オブジェクト階層がサービスの想定と一致していることを確認します。 `data` オブジェクトと [`xdm`](xdm.md) オブジェクトの両方を同じ `sendEvent` コマンドに含めることができます。

```javascript
alloy("sendEvent", {
  "data": dataObject
});
```

## Adobe Analyticsで `data` オブジェクトを使用します {#analytics}

Adobe Analyticsで `data` オブジェクトを使用すると、XDM スキーマを使用せずにレポートスイートにデータを送信できます。 変数は、AppMeasurement変数と同じ構文を使用するように設定されているので、web SDKへのアップグレードプロセスが簡単になります。 詳しくは、Adobe Analytics導入ガイドの [Adobe Analyticsへのデータオブジェクト変数のマッピング &#x200B;](https://experienceleague.adobe.com/en/docs/analytics/implementation/aep-edge/data-var-mapping) を参照してください。

## Web SDK タグ拡張機能を使用して `data` オブジェクトを使用します

`data` オブジェクトは、Web SDK タグ拡張機能を使用する際に [&#x200B; 変数データ要素 &#x200B;](/help/tags/extensions/client/web-sdk/data-element-types.md#variable) として使用できます。
