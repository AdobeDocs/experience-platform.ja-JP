---
keywords: Experience Platform；ホーム；人気の高いトピック；ラベル ID
title: UI でフィールドを ID としてラベル付けする
description: 個人を特定できる情報（PII）を含むフィールドは、ID フィールドとしてラベル付けできます。ID フィールドに指定された値は、ID サービスによって ID として解釈されます。ID の名前空間は、フィールドのラベル付けの一部として指定されます。
source-git-commit: ae51c9bd07944f26be2809a6d15f9d9e8c2fd5a1
workflow-type: tm+mt
source-wordcount: '201'
ht-degree: 67%

---

# UI でフィールドを ID としてラベル付け

個人を特定できる情報（PII）を含むフィールドは、ID フィールドとしてラベル付けできます。ID フィールドに指定された値は、 [!DNL Identity Service]. ID の名前空間は、フィールドのラベル付けの一部として指定されます。

フィールドを ID としてラベル付けするには、次の条件が満たされている必要があります。

* ID に使用できるのは、文字列型のフィールドのみです。
* ID は、レコードと時系列データのみで認識されます。
* PII フィールドのみを ID としてマークする必要があります。より一般的なデータを表すフィールドを選択すると、関係の精度が低下し、ID グラフから関連 ID にアクセスする際にエラーが発生する可能性があります。

UI で ID フィールドにラベルを付ける手順については、 [UI での id フィールドの定義](../../xdm/ui/fields/identity.md).

## 次の手順

詳しくは、 [!DNL Identity Service]を参照し、 [[!DNL Identity Service] 概要](../home.md).
