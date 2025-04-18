---
keywords: Experience Platform;はじめに;アトリビューション AI;人気のトピック;アトリビューション AI の入力;アトリビューション AI の出力;アトリビューション AI のトラブルシューティング;顧客 AI のエラー
solution: Experience Platform, Real-Time Customer Data Platform
feature: Attribution AI
title: アトリビューション AI エラーのトラブルシューティング
description: アトリビューション AI の一般的なエラーに対する回答を示します。
type: Documentation
exl-id: c2ff700a-1e36-4ba2-876c-9f8b56344241
source-git-commit: e028fbb82b37b3940b308a860c26f8b5f9884d3a
workflow-type: tm+mt
source-wordcount: '171'
ht-degree: 97%

---

# アトリビューション AI エラーのトラブルシューティング

このドキュメントでは、アトリビューション AI に関するよくある質問への回答を示します。

## Chrome のシークレットモードでアトリビューション AI にアクセスできない

Google Chrome の匿名モードセキュリティ設定が更新されたので、Google Chrome の匿名モードでの読み込みエラーが発生します。アドビでは、experience.adobe.com を信頼できるドメインにするために、Chrome でこの問題に鋭意取り組んでいるところです。

![ エラー画像 ](./images/faq/error.PNG){width=500}

### 推奨される修正

この問題を回避するには、experience.adobe.com を、常に Cookie を使用できるサイトとして追加する必要があります。まず、**chrome://settings/cookies** に移動します。次に、「**カスタマイズされた動作**」セクションまでスクロールしたあと、「常に Cookie を使用できるサイト」の隣にある「**追加**」ボタンを選択します。表示されたポップオーバーで `[*.]experience.adobe.com` をコピー＆ペーストし、「**このサイトでサードパーティの Cookie を許可する**」のチェックボックスをオンにします。完了したら、「**追加**」を選択し、アトリビューション AI をシークレットモードでリロードします。

![推奨される修正](./images/faq/cookies2.gif)
