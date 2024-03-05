---
title: data
description: XDM 以外のデータをAdobeに送信する方法を説明します。
source-git-commit: f75dcfc945be2f45c1638bdd4d670288aef6e1e6
workflow-type: tm+mt
source-wordcount: '298'
ht-degree: 1%

---

# data

The `data` プロパティを使用すると、XDMAdobeと一致しないスキーマにデータを送信できます。 これは、XDM 以外のシナリオ（例えば、の更新）で役立ちます。 [Adobe Targetプロファイル](/help/web-sdk/personalization/adobe-target/target-overview.md). データがAdobeに到達したら、データストリームマッピングツールを使用して、XDM フィールドを `data` プロパティ。

>[!IMPORTANT]
>
>このプロパティ内のデータには、次のアクションの少なくとも 1 つが必要です。
>
>* データストリーム内のサービスは、 `data` object
>* 各プロパティは、XDM フィールドにマッピングする必要があります
>
>特定のフィールドが XDM フィールドにマッピングされていない場合や、設定されたサービスで使用されている場合、そのデータは永久的に失われます。

## Web SDK タグ拡張機能を使用して、データプロパティを使用する

データ要素を **[!UICONTROL データ]** フィールドを使用して、タグルールのアクション内に配置できます。

1. にログインします。 [experience.adobe.com](https://experience.adobe.com) Adobe ID資格情報を使用して。
1. に移動します。 **[!UICONTROL データ収集]** > **[!UICONTROL タグ]**.
1. 目的のタグプロパティを選択します。
1. に移動します。 **[!UICONTROL ルール]**&#x200B;を選択し、目的のルールを選択します。
1. の下 [!UICONTROL アクション]、既存のアクションを選択するか、アクションを作成します。
1. を設定します。 [!UICONTROL 拡張] ドロップダウンフィールド **[!UICONTROL Adobe Experience Platform Web SDK]**&#x200B;をクリックし、 [!UICONTROL アクションタイプ] から **[!UICONTROL イベントを送信]**.
1. 目的のオブジェクトを含むデータ要素を **[!UICONTROL データ]** フィールドに入力します。
1. クリック **[!UICONTROL 変更を保持]**&#x200B;を開き、パブリッシュワークフローを実行します。

## Web SDK JavaScript ライブラリを使用した data プロパティの使用

を設定します。 `data` プロパティを JSON オブジェクトの一部として、 `sendEvent` コマンドを使用します。 データストリームでマッピングする予定のデータに対しては、このプロパティを好きなだけ構造にすることができます。 特定のサービスで使用されるデータの場合は、オブジェクト階層がサービスで想定されるものと一致していることを確認します。 次の `data` オブジェクトと [`xdm`](xdm.md) 同じ `sendEvent` コマンドを使用します。

```javascript
alloy("sendEvent", {
  "data": dataObject
});
```
