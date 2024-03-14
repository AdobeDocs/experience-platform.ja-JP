---
title: ECID へのアクセス
description: データ準備またはExperience Cloudからタグ ID にアクセスする方法を説明します
exl-id: 8e63a873-d7b5-4c6c-b14d-3c3fbc82b62f
source-git-commit: e01dfcf3cccea589083a23171f4b8d9ecad58233
workflow-type: tm+mt
source-wordcount: '253'
ht-degree: 3%

---


# ECID へのアクセス

The [!DNL Experience Cloud Identity (ECID)] は、ユーザーが Web サイトを訪問した際にユーザーに割り当てられる永続的な識別子です。 特定の状況で、 [!DNL ECID] （サードパーティに送信する場合など）。 別の使用例として、 [!DNL ECID] カスタム XDM フィールドに追加する必要があります。

ECID には、 [データ収集用のデータ準備](../../../../datastreams/data-prep.md) （推奨）またはタグを使用します。

## データ準備を使用した ECID へのアクセス（推奨される方法） {#accessing-ecid-data-prep}

ECID をカスタム XDM フィールドに設定したい場合は、ID マップに追加で、ECID を設定することができます。それには、 `source` を次のパスに追加します。

```js
xdm.identityMap.ECID[0].id
```

次に、フィールドのタイプがの XDM パスにターゲットを設定します `string`.

![](./assets/access-ecid-data-prep.png)

## タグ

次にアクセスする必要がある場合、 [!DNL ECID] クライアント側では、次に説明するタグアプローチを使用します。

1. プロパティがで設定されていることを確認します。 [ルールコンポーネントの順序付け](../../../ui/managing-resources/rules.md#sequencing) 有効。
1. 新しいルールを作成します。 このルールは、 [!DNL ECID] 他の重要な行動もなく
1. を追加します。 [!UICONTROL Library Loaded] イベントをルールに追加します。
1. を追加します。 [!UICONTROL カスタムコード] 次のコードを使用してルールにアクションを追加します（SDK インスタンスに設定した名前がの場合）。 `alloy` 同じ名前のデータ要素がまだ存在していません )。

   ```js
    return alloy("getIdentity")
      .then(function(result) {
        _satellite.setVar("ECID", result.identity.ECID);
      });
   ```

1. ルールを保存します。

その後、 [!DNL ECID] 以降のルールでは `%ECID%` または `_satellite.getVar("ECID")`他のデータ要素にアクセスする場合と同様に、
