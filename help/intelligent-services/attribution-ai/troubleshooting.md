---
keywords: Experience Platform；はじめに；attribution ai；人気の高いトピック；attribution ai 入力；attribution ai 出力；attribution ai トラブルシューティング；attribution ai エラー
solution: Experience Platform, Real-Time Customer Data Platform
feature: Attribution AI
title: Attribution AIエラーのトラブルシューティング
description: Attribution AIの一般的なエラーに対する回答を見つけます。
type: Documentation
exl-id: c2ff700a-1e36-4ba2-876c-9f8b56344241
source-git-commit: 07a110f6d293abff38804b939014e28f308e3b30
workflow-type: tm+mt
source-wordcount: '167'
ht-degree: 61%

---

# Attribution AIエラーのトラブルシューティング

このドキュメントでは、Attribution AIに関するよくある質問に対する回答を示します。

## Chrome の匿名Attribution AIにアクセスできません

Google Chrome の匿名モードセキュリティ設定が更新されたので、Google Chrome の匿名モードでの読み込みエラーが発生します。アドビでは、experience.adobe.com を信頼できるドメインにするために、Chrome でこの問題に鋭意取り組んでいるところです。

<img src="./images/faq/error.PNG" width="500" /><br />

### 推奨される修正

この問題を回避するには、experience.adobe.com を、常に Cookie を使用できるサイトとして追加する必要があります。まず、**chrome://settings/cookies** に移動します。次に、「**カスタマイズされた動作**」セクションまでスクロールしたあと、「常に Cookie を使用できるサイト」の隣にある「**追加**」ボタンを選択します。表示されたポップオーバーで `[*.]experience.adobe.com` をコピー＆ペーストし、「**このサイトでサードパーティの Cookie を許可する**」のチェックボックスをオンにします。完了したら、「 」を選択します。 **追加** をクリックし、匿名でAttribution AIを再読み込みします。

![推奨される修正](./images/faq/cookies2.gif)
