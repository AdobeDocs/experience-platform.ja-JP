---
keywords: Experience Platform;ホーム;人気のトピック;
title: Data Prep トラブルシューティングガイド
topic-legacy: troubleshooting
description: このドキュメントでは、Adobe Experience Platform Data Prep に関するよくある質問に対する回答を示します。
source-git-commit: e96263847f53ea2c884c273fd7986855d4c478c1
workflow-type: tm+mt
source-wordcount: '254'
ht-degree: 3%

---

# [!DNL Data Prep] トラブルシューティングガイド

このドキュメントでは、Adobe Experience Platformに関するよくある質問に対する回答を示します [!DNL Data Prep]に加えて、一般的なエラーのトラブルシューティングガイドを示します。 Platform API に関する一般的な質問とトラブルシューティング情報については、 [Adobe Experience Platform API トラブルシューティングガイド](../landing/troubleshooting.md).

## FAQ

次に、 [!DNL Data Prep] そして彼らの答えを

### 変換エラーはどのように解決されますか？

[!DNL Data Prep] すべての変換エラーを、発生した列にローカライズします。 その結果、その列は無効になり、残りの行は引き続き処理されます。 これらの変換の問題は、 **警告**. 警告を定期的に確認し、変換ロジックを調整して、変換の問題を考慮することをお勧めします。 これにより、Experience Platformに取り込まれるデータの品質が向上します。

列が **必須** は、変換の問題により無効にされた場合、行は取り込まれません。 部分的なデータ取り込みが有効な場合、フロー全体が失敗する前に、このような却下のしきい値を設定できます。 指定されていない属性がスキーマレベルの検証に影響を与えなかった場合、行は引き続き取り込まれます。

変換エラーが発生しなくても無効な行も拒否されます。 例えば、データ取り込みフローでは、必須フィールドへのマッピング（変換ロジックなし）がパススルーされ、その属性に対して受け取る値がない場合があります。 この行は却下されます。