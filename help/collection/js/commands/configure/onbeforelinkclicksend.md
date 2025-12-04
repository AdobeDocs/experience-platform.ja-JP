---
title: onBeforeLinkClickSend
description: リンクトラッキングデータが送信される直前に実行されるコールバック。
exl-id: 8c73cb25-2648-4cf7-b160-3d06aecde9b4
source-git-commit: c6a2b9700f0a688f65fec9febf5622c6c7b6aafa
workflow-type: tm+mt
source-wordcount: '172'
ht-degree: 0%

---

# `onBeforeLinkClickSend`

>[!IMPORTANT]
>
>このコールバックは非推奨（廃止予定）です。 代わりに [`clickCollection.filterClickDetails`](clickcollection.md) を使用します。

`onBeforeLinkClickSend` コールバックを使用すると、JavaScript関数を登録して、Adobeに送信する直前に送信したリンクトラッキングデータを変更できます。 要素の追加、編集、削除の機能を含め、`xdm` オブジェクトまたは `data` オブジェクトを操作できます。 クライアントサイドのボットトラフィックが検出された場合など、データの送信を完全に条件付きでキャンセルすることもできます。

このコールバックは、[`clickCollectionEnabled`](clickcollectionenabled.md) が有効で、登録済みの関数が含まれ `filterClickDetails` いない場合にのみ実行されます。

[`onBeforeEventSend`](onbeforeeventsend.md) と `onBeforeLinkClickSend` の両方に登録済みの関数が含まれている場合、`onBeforeLinkClickSend` が最初に実行されます。

>[!WARNING]
>
>このコールバックを使用すると、カスタムコードを使用できます。 コールバックに含めるコードがキャッチされない例外をスローした場合、イベントの処理が停止します。 データはAdobeに送信されません。

## `onBeforeLinkClickSend` および `filterClickDetails`

[`clickCollection.filterClickDetails`](clickcollection.md) コールバックは、`onBeforeLinkClickSend` を置き換えるように設計されています。 Adobeでは、コールバック関数を両方に同時に割り当てることを強くお勧めします。 コールバック関数を `filterClickDetails` と `onBeforeLinkClickSend` の両方に割り当てる場合、ライブラリは次のロジックを使用します。

* `filterClickDetails` のみが実行され、`onBeforeLinkClickSend` は実行されません。
* イベントのグループ化 `clickCollection.eventGroupingEnabled` は機能しません。
