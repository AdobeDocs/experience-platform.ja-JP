---
keywords: Experience Platform;ホーム;人気のトピック;ソース;取り込み;トラブルシューティング;ソースのトラブルシューティング;ソースの FAQ;FAQ;ソースコネクタ;ソースコネクタの FAQ;ソースコネクタのトラブルシューティング;
solution: Experience Platform
title: ソースのトラブルシューティング
description: このドキュメントでは、Adobe Experience Platform でのソースに関するよくある質問に対する回答を示します。
exl-id: 94875121-7d4d-4eb2-8760-aa795933dd7e
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '749'
ht-degree: 97%

---

# ソースのトラブルシューティングガイド

このドキュメントでは、Adobe Experience Platform でのソースに関するよくある質問に対する回答を示します。すべての [!DNL Experience Platform] API で発生する問題を含め、他の [!DNL Experience Platform] サービスに関する質問とトラブルシューティングについては、[Experience Platform トラブルシューティングガイド](../landing/troubleshooting.md)を参照してください。

## よくある質問

ソースに関するよくある質問に対する回答の一覧を以下に示します。

### ソースを有効にするには、ネットワークのセキュリティ設定を変更する必要がありますか？

場合によっては、ソースを有効にするために、特定の IP アドレスを許可リストに登録する必要があります。詳しくは、特定のソースコネクタに関するドキュメントを参照してください。

### ソースでサポートされている認証タイプは何ですか？

ソースの認証は、接続文字列、ユーザー名とパスワード、アクセストークンとキーのいずれかを使用して行えます。サポートされている認証タイプについて詳しくは、指定されたソースコネクタのドキュメントを参照してください。

### 最近のフロー実行がすべて失敗するのはなぜですか？

最近のフロー実行がすべて失敗している場合は、資格情報が変更されたか、期限切れになった可能性があります。 この問題を解決するには、最新の資格情報を使用して接続を更新してみてください。

### サポートされているファイルタイプは何ですか？

現在サポートされているファイルタイプは、区切りファイル、JSON および Parquet です。

### ファイルの名前とサイズに関する制約は何ですか？

ソース内のファイルについて考慮する必要がある制約のリストを以下に示します。

- ディレクトリ名とファイルコンポーネント名は 255 文字を超えてはなりません。
- ディレクトリ名とファイル名の末尾にスラッシュ（`/`）は使用できません。使用した場合、自動的に削除されます。
- 次の予約 URL 文字は、適切にエスケープする必要があります。`! ' ( ) ; @ & = + $ , % # [ ]`
- 次の文字は使用できません。`" \ / : | < > * ?`
- 無効な URL パス文字は使用できません。`\uE000` のようなコードポイントは、NTFS ファイル名では有効ですが、有効な Unicode 文字ではありません。また、一部の ASCII 文字や Unicode 文字、例えば制御文字（0x00 ～ 0x1F、\u0081 など）も使用できません。HTTP/1.1 で Unicode 文字列を規定するルールについては、[RFC 2616、セクション 2.2：基本ルール](https://www.ietf.org/rfc/rfc2616.txt)および [RFC 3987](https://www.ietf.org/rfc/rfc3987.txt) を参照してください。
- 次のファイル名は使用できません：LPT1、LPT2、LPT3、LPT4、LPT5、LPT6、LPT7、LPT8、LPT9、COM1、COM2、COM3、COM4、COM5、COM6、COM7、COM8、COM9、PRN、AUX、NUL、CON、CLOCK$、ドット文字（.）、2 つのドット文字（..）。
- 1 回のバッチあたりの最大ファイル数は 1500 で、最大バッチサイズは 100 GB です。
- 1 行あたりのプロパティまたはフィールドの最大数は 10,000 個です。
- 1 分間にユーザーごとに送信できるバッチの最大数は 2000 個です。

### サポートされているデータタイプは何ですか？

サポートされているデータタイプには、整数、文字列、ブール値、日時オブジェクト、配列およびオブジェクトがあります。

### サポートされている日時形式は何ですか？

ソースでは、データの取り込み時に、様々な日時形式をサポートしています。 サポートされている日時形式について詳しくは、データ準備ドキュメントで[データ形式処理ガイド](../data-prep/data-handling.md#dates)の日付の節を参照してください。

### CSV、JSON、Parquet の各ファイルにおける配列の形式を教えてください。

JSON ファイルと Parquet ファイルでは、配列をネイティブにサポートしています。 CSV などのフラットな構造の場合、配列はサポートされていません。ただし、explode や join などのデータ準備関数を使用して、複数の値を持つ文字列を配列に分割することができます。これらのデータ準備関数について詳しくは、[データ準備関数ガイド](../data-prep/functions.md#string)を参照してください

### 部分取り込みをサポートしているソースは何ですか？

すべてのバッチ取り込みソースで、部分取り込みをサポートしています。 ただし、ストリーミング取り込みソースでは、部分取り込みをサポートしていません。

### 部分取り込みを使用するのは、どのような場合ですか？

部分取り込みを使用するのは、ファイル全体をExperience Platformに取り込むなどの制約がある **ない** 場合です。 または、エラーが含まれている可能性のあるデータを取り込んでも構わない場合は、部分取り込みを使用してください。

### 部分取り込みエラーの一般的なしきい値は何ですか？

部分取り込みの「一般的なエラーしきい値」というものはありません。 むしろ、この値はユースケースによって異なる可能性があります。 デフォルトでは、エラーしきい値は 5％に設定されています。

### 新しいデータフローの作成後、フロー実行のステータスが更新されるまでに、どれくらいの時間がかかりますか？

フロー実行は即座には生成されず、指定された `startTime` の後、更新されるまでに約 2～3 分かかる場合があります。新しいデータフローの作成直後にフロー実行のステータスを確認しても、まだ実行されていないので、フロー実行の `lastRunDetails` に関する情報は返されません。データフローが生成されるまで数分間待ってからフロー実行のステータスを確認することをお勧めします。