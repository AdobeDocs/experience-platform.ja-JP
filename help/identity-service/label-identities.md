---
keywords: Experience Platform；ホーム；人気のトピック；ラベル ID
title: UI でのフィールドの ID としてのラベル付け
description: 個人を特定できる情報（PII）を含むフィールドは、ID フィールドとしてラベル付けできます。ID フィールドに指定された値は、ID サービスによって ID として解釈されます。ID の名前空間は、フィールドのラベル付けの一部として指定されます。
hide: true
hidefromtoc: true
exl-id: c3097030-0242-404f-9e4c-72a7fa574011
source-git-commit: 0d111241658b4014d1ca2e6013d21a4782d81be9
workflow-type: tm+mt
source-wordcount: '201'
ht-degree: 70%

---

# UI でのフィールドの ID としてのラベル付け

個人を特定できる情報（PII）を含むフィールドは、ID フィールドとしてラベル付けできます。ID フィールドに指定された値は、[!DNL Identity Service] によって ID として解釈されます。 ID の名前空間は、フィールドのラベル付けの一部として指定されます。

フィールドを ID としてラベル付けするには、次の条件が満たされている必要があります。

* ID に使用できるのは、文字列型のフィールドのみです。
* ID は、レコードと時系列データのみで認識されます。
* PII フィールドのみを ID としてマークする必要があります。より一般的なデータを表すフィールドを選択すると、関係の精度が低下し、ID グラフから関連 ID にアクセスする際にエラーが発生する可能性があります。

UI で ID フィールドにラベルを付ける方法については、[UI での ID フィールドの定義 ](../xdm/ui/fields/identity.md) に関するガイドを参照してください。

## 次の手順

[!DNL Identity Service] について詳しくは、[[!DNL Identity Service] 概要](./home.md)を参照してください。
