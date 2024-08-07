---
keywords: Experience Platform;ホーム;人気のトピック;
title: データ準備トラブルシューティングガイド
description: このドキュメントでは、Adobe Experience Platform データ準備に関するよくある質問に対する回答を示します。
exl-id: 810cfb2f-f80a-4aa7-ab3c-beb5de78708e
source-git-commit: d39ae3a31405b907f330f5d54c91b95c0f999eee
workflow-type: tm+mt
source-wordcount: '343'
ht-degree: 100%

---

# [!DNL Data Prep] トラブルシューティングガイド

このドキュメントでは、Adobe Experience Platform [!DNL Data Prep] に関するよくある質問に対する回答と、一般的なエラーのトラブルシューティングガイドを示します。Platform API 全般に関する質問とトラブルシューティング情報については、[Adobe Experience Platform API トラブルシューティング ガイド](../landing/troubleshooting.md)を参照してください。

## FAQ

以下は、[!DNL Data Prep] に関するよくある質問とその回答のリストです。

### 変換エラーを解決するにはどうすればよいですか？

[!DNL Data Prep] は、すべての変換エラーを、エラーが発生した列にローカライズします。その結果、その列は無効になり、残りの行は引き続き処理されます。これらの変換の問題は、**警告**&#x200B;として記録されます。警告を定期的に確認し、変換の問題を考慮して変換ロジックを調整することをお勧めします。これにより、Experience Platform に取り込まれるデータの品質が向上します。

**必須**&#x200B;とマークされた列が変換の問題により無効になっている場合、その行は取り込まれません。部分的なデータ取り込みが有効になっている場合、フロー全体が失敗する前に、そのような却下のしきい値を設定できます。無効化された属性がスキーマレベルの検証に影響しなかった場合、その行は引き続き取り込まれます。

変換エラーが発生しなくても無効な行もすべて却下されます。例えば、データ取り込みフローには、必須フィールドへのパススルーマッピング（変換ロジックなし）があり、その属性の入力値がない場合があります。この行は却下されます。

### フィールド内の特殊文字をエスケープするにはどうすればよいですか？

フィールド内の特殊文字をエスケープするには、`${...}` を使用します。ただし、ピリオド（`.`）を含むフィールドを含む JSON ファイルは、このメカニズムではサポートされていません。階層を操作する際に、子属性にピリオド（`.`）がある場合は、バックスラッシュ（`\`）を使用して特殊文字をエスケープする必要があります。例えば、`address` は属性 `street.name` を含むオブジェクトであり、これは `address.street.name` ではなく `address.street\.name` として参照できます。

### 計算フィールドの最大長はどれくらいですか？

計算フィールドの最大長は 4096 文字です。