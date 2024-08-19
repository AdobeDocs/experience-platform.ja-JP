---
title: ECID へのアクセス
description: Data Prep または Tags からExperience CloudID にアクセスする方法を説明します
exl-id: 8e63a873-d7b5-4c6c-b14d-3c3fbc82b62f
source-git-commit: e53ae6053a4b00e7e75242b95496c6795953005a
workflow-type: tm+mt
source-wordcount: '283'
ht-degree: 3%

---


# ECID へのアクセス

[!DNL Experience Cloud Identity (ECID)] は、ユーザーが web サイトを訪問したときにユーザーに割り当てられる永続的な識別子です。 状況によっては、（例えば、サードパーティに送信するために） [!DNL ECID] ールにアクセスする方が良い場合があります。 別の使用例としては、ID マップに含めるだけでなく、カスタム XDM フィールドに [!DNL ECID] を設定する場合があります。

[ データ収集のためのデータ準備 ](../../../../datastreams/data-prep.md) （推奨）またはタグを使用して、ECID にアクセスできます。

## データ準備を使用した ECID へのアクセス（推奨される方法） {#accessing-ecid-data-prep}

このメソッドは、[ データ収集のためのデータ準備 ](../../../../datastreams/data-prep.md) を使用して、`ECID` ータのカスタムマッピングを設定します。

この機能の使用方法については、[ データ収集のためのデータ準備 ](../../../../datastreams/data-prep.md) のドキュメントを参照してください。

ECID をカスタム XDM フィールドに設定する場合、ID マップに含める以外に、`source` を次のパスに設定することでこれを実行できます。

```js
xdm.identityMap.ECID[0].id
```

次に、フィールドのタイプが `string` の XDM パスにターゲットを設定します。

![](./assets/access-ecid-data-prep.png)

## タグ

クライアントサイドで [!DNL ECID] にアクセスする必要がある場合は、以下に説明するようにタグアプローチを使用します。

1. [ ルールコンポーネントの優先順位 ](../../../ui/managing-resources/rules.md#sequencing) が有効になっているプロパティが設定されていることを確認します。
1. 新しいルールを作成します。 このルールは、他の重要なアクションを実行せずに [!DNL ECID] をキャプチャする場合にのみ使用してください。
1. [!UICONTROL  ライブラリの読み込み ] イベントをルールに追加します。
1. 次のコードを使用して、ルールに [!UICONTROL  カスタムコード ] アクションを追加します（SDK インスタンスに対して設定した名前が `alloy` で、同じ名前のデータ要素がまだない場合）。

   ```js
    return alloy("getIdentity")
      .then(function(result) {
        _satellite.setVar("ECID", result.identity.ECID);
      });
   ```

1. ルールを保存します。

その後、他のデータ要素にアクセスする場合と同様に、`%ECID%` または `_satellite.getVar("ECID")` を使用して、後続のルールで [!DNL ECID] にアクセスできます。
