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

この `data` オブジェクトを使用すると、XDM スキーマに一致しないAdobeにペイロードを送信できます。 Adobe Analytics、Adobe Target、Adobe Audience Managerに直接データを送信するなど、XDM 以外のシナリオで役立ちます。 データストリームにデータが到達した場合、を使用できます [データ準備のマッピング](/help/data-prep/ui/mapping.md) XDM フィールドを内の各フィールドに割り当てるには `data` オブジェクト。

>[!IMPORTANT]
>
>このオブジェクト内のデータには、次のアクションの少なくとも 1 つが必要です：
>
>* データストリーム内のサービスは、内の指定のプロパティからデータを取得するように設定する必要があります。 `data` オブジェクト。
>* 指定されたプロパティは、データ準備を使用して XDM フィールドにマッピングする必要があります。
>
>特定のプロパティが XDM フィールドにマッピングされていない場合や、設定済みのサービスで使用されていない場合、そのデータは永続的に失われます。

## の使用 `data` web SDK タグ拡張機能を使用したオブジェクト {#tag-extension}

にデータ要素を指定します。 **[!UICONTROL データ]** タグルールのアクション内のフィールド。

1. へのログイン [experience.adobe.com](https://experience.adobe.com) Adobe IDの資格情報を使用します。
1. に移動します。 **[!UICONTROL データ収集]** > **[!UICONTROL タグ]**.
1. 目的のタグプロパティを選択します。
1. に移動します。 **[!UICONTROL ルール]**&#x200B;を選択してから、目的のルールを選択します。
1. 次の下 [!UICONTROL アクション]、既存のアクションを選択するか、アクションを作成します。
1. を [!UICONTROL 拡張機能] ドロップダウンフィールドの移動先 **[!UICONTROL Adobe Experience Platform Web SDK]**、を設定します。 [!UICONTROL アクションタイプ] 対象： **[!UICONTROL イベントを送信]**.
1. 目的のオブジェクトを含むデータ要素を **[!UICONTROL データ]** フィールド。
1. クリック **[!UICONTROL 変更を保持]**&#x200B;次に、公開ワークフローを実行します。

## の使用 `data` web SDK JavaScript ライブラリを介したオブジェクト {#library}

を `data` のパラメーター内の JSON オブジェクトの一部としてのオブジェクト `sendEvent` コマンド。 データストリームでマッピングするデータの場合、このオブジェクトを好きなように構造化できます。 特定のサービスで使用されるデータの場合は、オブジェクト階層がサービスの想定と一致していることを確認します。 次の両方を含めることができます `data` オブジェクトと [`xdm`](xdm.md) 同じオブジェクト `sendEvent` コマンド。

```javascript
alloy("sendEvent", {
  "data": dataObject
});
```

## の使用 `data` Adobe Analyticsを使用したオブジェクト {#analytics}

を使用できます `data` Adobe Analyticsを使用したオブジェクトで、XDM スキーマを使用せずにレポートスイートにデータを送信します。 変数は、と同じ構文を使用するように設定されます [!DNL AppMeasurement] 変数を使用して、Web SDK へのアップグレードプロセスを簡素化します。 参照： [Adobe Analyticsへのデータオブジェクト変数のマッピング](https://experienceleague.adobe.com/en/docs/analytics/implementation/aep-edge/data-var-mapping) を参照してください。Adobe Analytics実装ガイド
