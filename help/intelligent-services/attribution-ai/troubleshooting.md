---
keywords: Experience Platform；はじめに；attribution ai；人気の高いトピック；attribution ai 入力；attribution ai 出力；attribution ai トラブルシューティング；attribution ai エラー
solution: Experience Platform, Real-time Customer Data Platform
feature: Attribution AI
title: Attribution AIエラーのトラブルシューティング
description: Attribution AIの一般的なエラーに対する回答を見つけます。
type: Documentation
exl-id: c2ff700a-1e36-4ba2-876c-9f8b56344241
source-git-commit: eae43834d1cd5931dd752b95023da7ac77668e56
workflow-type: tm+mt
source-wordcount: '167'
ht-degree: 0%

---

# Attribution AIエラーのトラブルシューティング

このドキュメントでは、Attribution AIに関するよくある質問に対する回答を示します。

## Chrome の匿名Attribution AIにアクセスできません

Google Chrome の匿名モードセキュリティ設定が更新されたので、Google Chrome の匿名モードでの読み込みエラーが発生します。 この問題は、experience.adobe.com を信頼されたドメインにするために、Chrome で積極的に作業を進めています。

<img src="./images/faq/error.PNG" width="500" /><br />

### 推奨される修正

この問題に対処するには、常に cookie を使用できるサイトとして experience.adobe.com を追加する必要があります。 最初に、 **chrome://settings/cookies**. 次に、 **カスタマイズされた動作** セクションで **追加** ボタンをクリックします。 表示されるポップオーバーで、コピーして貼り付けます。 `[*.]experience.adobe.com` 次に、 **サードパーティ Cookie を含める** （このサイトのチェックボックス）。 完了したら、「 」を選択します。 **追加** をクリックし、匿名でAttribution AIを再読み込みします。

![推奨修正](./images/faq/cookies2.gif)
