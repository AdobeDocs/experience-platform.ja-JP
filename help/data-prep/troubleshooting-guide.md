---
keywords: Experience Platform;ホーム;人気のトピック;
title: データ準備トラブルシューティングガイド
description: このドキュメントでは、Adobe Experience Platform データ準備に関するよくある質問に対する回答を示します。
exl-id: 810cfb2f-f80a-4aa7-ab3c-beb5de78708e
source-git-commit: fded2f25f76e396cd49702431fa40e8e4521ebf8
workflow-type: tm+mt
source-wordcount: '1257'
ht-degree: 28%

---

# [!DNL Data Prep] トラブルシューティングガイド

このドキュメントでは、Adobe Experience Platform [!DNL Data Prep] に関するよくある質問に対する回答と、一般的なエラーのトラブルシューティングガイドを示します。Experience Platform API 全般に関する質問とトラブルシューティングについては、[Adobe Experience Platform API トラブルシューティングガイド ](../landing/troubleshooting.md) を参照してください。

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

### 属性の検証が原因で取り込みに失敗しましたが、ファイルにその属性が正しく含まれています。 正確に何が間違っていますか？

各フィールドのデータタイプが、スキーマで定義されたタイプと一致することを確認します。 さらに、「必須」、「列挙」、「形式」などの制約に従う必要があります。

取り込まれるデータは、Experience Platformで定義されたエクスペリエンスデータモデル（XDM）スキーマに従っている必要があります。 属性がスキーマで指定された期待されるタイプまたは形式に一致しない場合、取り込みは失敗します。

データ準備関数を使用している場合は、変換によって適切な属性が得られることを確認します。 属性は、ソースワークフローの設定プロセス中に確認できます。 マッピング手順で、「**[!UICONTROL 新しいフィールドタイプ]**」を選択したあと、「**[!UICONTROL 計算フィールドを追加]**」を選択します。 次に、計算フィールドインターフェイスを使用して、各関数をプレビューします。

### ストリーミングまたはバッチ取り込みレコードから不正なデータ値を削除するにはどうすればよいですか？

データ準備マッピングインターフェイスを使用して、必要なデータを持つ列のみをマッピングして、列レベルのフィルタリングを実行できます。 また、計算フィールドを使用して、サポート関数を使用してデータを変換することもできます。

行レベルのフィルタリングは、現在、[Adobe Analytics ソースコネクタ ](../sources/tutorials/ui/create/adobe-applications/analytics.md#row-level-filtering) でのみ使用できます。

取り込み後に、Data Distiller を使用して、SQL を使用してデータを消去、整形、操作できます。 ただし、このプロセスでは、不正なレコードを含むバッチを削除し、SQL の結果から作成された新しいバッチを再度取り込む必要があります。

>[!IMPORTANT]
>
>* データレイク：レコードが含まれているバッチを削除して再取り込みすることで、既に取り込まれているレコードのみを削除できます。
>
>* リアルタイム顧客プロファイル：新しいレコードを取り込むことで属性ベースのレコードを上書きできますが、エクスペリエンスイベントレコードは削除できません。
>
>* ID サービス：ID サービスでレコードを完全に削除することはできません。 プロファイル全体を削除し、プロファイル削除 API を使用して、正しいレコードでプロファイルを再度アップロードする必要があります。

### GIF データで計算フィールドを使用するためのベストプラクティスは何ですか？

ソースデータの XDM スキーマへのマッピング手順でデータ準備のマッピング関数を使用して、新しい計算フィールドを作成できます。

### Adobe Analytics データをソースとして取り込む場合、スキーマはプロファイルに対して自動的に作成されますか？

Analytics データは、プロファイル用に自動的に設定されません。 ソースコネクタを設定したら、データセットとスキーマに移動し、それらをプロファイル取り込みに対して有効にする必要があります。

実稼動サンドボックスで Analytics ソースデータフローを作成すると、次の 2 つのデータフローが作成されます。

* データレイクへの履歴レポートスイートデータの 13 か月のバックフィルを行うデータフロー。 このデータフローは、バックフィルが完了すると終了します。
* データレイクとプロファイルにライブデータを送信するデータフロー。 このデータフローは継続的に実行されます。

### データ準備関数を使用してマップオブジェクト内の 1 つの値を小文字にするにはどうすればよいですか？

`map_get_values` 関数を使用して値を取得し、lower 関数を使用して小文字にすることができます。

```shell
lower(map_get_values(mapObject, 'keyName'))
```

同じ関数を使用して、マップオブジェクトを小文字にすることができます。 ただし、マップ全体をループ処理して、すべての項目を小文字にすることはできません。

### データ準備関数をネストされた方法で使用できますか？

はい。あるデータ準備関数を別の関数内で使用すると、データ取り込み中に複雑なデータ準備機能を解決できます。

例えば、特定の条件に基づいてフィールドを null として定義する場合、「iif」関数を使用してそのフィールドを確認できます。 関数が `true` を返す場合は「nullify （）」を使用し、`false` を返す場合は、それぞれのフィールドを使用します。

marketing_type がフィールドの場合、「.equals」を使用して marketing_type フィールドの値を確認できます。これは、「iif」関数内にネストできます。 `true` が返された場合、以下に示すように、「nullify （）」関数を使用できます。

```shell
iif(marketing_type.equals("phyMail"), nullify(), marketing_type)
```

次に、if、equals、nullify を使用してデータ準備関数をネストする方法の例の概要を示します。

| 関数 | 説明 | パラメーター | 構文 | 式 | サンプル出力 |
| --- | --- | --- | --- | --- | --- |
| iif | 指定されたブール式を評価し、結果に基づいて指定された値を返します。 | <ul><li>EXPRESSION: **必須** 評価されるブール式。</li><li>TRUE_VALUE: **必須** 式が true と評価された場合に返される値。</li><li>FALSE_VALUE: **必須** 式が false と評価された場合に返される値。</li></ul> | iif （EXPRESSION, TRUE_VALUE, FALSE_VALUE） | iif(&quot;s&quot;.equalsIgnoreCase(&quot;S&quot;), &quot;True&quot;, &quot;False&quot;) | &quot;True&quot; |
| 次と等しい | 2 つの文字列を比較して、等しいかどうかを確認します。 この関数では大文字と小文字が区別されます。 | <ul><li>STRING1: **必須** 比較する最初の文字列。</li><li>STRING2: **必須** 比較する 2 番目の文字列。 | STRING1.&#x200B;equals （&#x200B;STRING2） | &quot;string1&quot;..&#x200B;equals&#x200B;（&quot;STRING1&quot;） | false |
| 無効にする | 属性の値を null に設定します。 これは、フィールドをターゲットスキーマにコピーしない場合に使用する必要があります。 | | nullify （） | nullify （） | null |

{style="table-layout:auto"}

次に、評価されるフィールドが「marketing_type」であると仮定して、関数をネストする方法の例を示します。

```shell
iif(marketing_type.equals("phyMail"), nullify(), marketing_type)
```

次に、次の 3 つのフィールドがあるとします。

* marketing_type: （email, phyMail, push, sms, phone）
* total_consents:4000 ～ 5500 の範囲の数値
* 開催日：2024 年 2 月～3 月

上記の 3 つの関数を使用またはネストして、次の 3 つのフィールドを操作できます。

* iif （marketing_type.equals （&quot;email&quot;）, nullify （）, iif （marketing_type.equals （&quot;push&quot;）, &quot;push-notification&quot;, marketing_type））
* iif （marketing_type.equals （&quot;phyMail&quot;）, nullify （）, iif （marketing_type.equals （&quot;sms&quot;）, &quot;text-message&quot;, marketing_type））
* iif （total_consents > 5000, iif （marketing_type.equals （&quot;phone&quot;）, nullify （）, marketing_type）, &quot;insufficient-consents&quot;）
* iif （date.equals （&quot;3/21/24&quot;）, iif （marketing_type.equals （&quot;push&quot;）, nullify （）, marketing_type）, &quot;not-March&quot;）
* iif （total_consents &lt; 4500, iif （marketing_type.equals （&quot;sms&quot;）, &quot;low-consent-sms&quot;, marketing_type）, &quot;high-consents&quot;）
* iif （marketing_type.equals （&quot;email&quot;）, iif （total_consents > 5000, nullify （）, &quot;email-low-consents&quot;）, marketing_type）
* iif （marketing_type.equals （&quot;push&quot;）, iif （total_consents &lt; 4500, &quot;low-consent-push&quot;, nullify （））, marketing_type）
* iif （total_consents >= 5500, iif （marketing_type.equals （&quot;phyMail&quot;）, nullify （）, &quot;high-consents&quot;）, marketing_type）