---
keywords: Experience Platform；ホーム；人気のあるトピック；ラベルのID
solution: Experience Platform
title: フィールドを ID としてラベル付け
topic: api guide
description: 個人を特定できる情報（PII）を含むフィールドは、ID フィールドとしてラベル付けできます。ID フィールドに指定された値は、ID サービスによって ID として解釈されます。ID の名前空間は、フィールドのラベル付けの一部として指定されます。
translation-type: tm+mt
source-git-commit: 0af537e965605e6c3e02963889acd85b9d780654
workflow-type: tm+mt
source-wordcount: '194'
ht-degree: 89%

---


# フィールドを ID としてラベル付け

個人を特定できる情報（PII）を含むフィールドは、ID フィールドとしてラベル付けできます。IDフィールドに指定された値は、[!DNL Identity Service]によってIDとして解釈されます。 ID の名前空間は、フィールドのラベル付けの一部として指定されます。

フィールドを ID としてラベル付けするには、次の条件が満たされている必要があります。

- ID に使用できるのは、文字列型のフィールドのみです。
- ID は、レコードと時系列データのみで認識されます。
- PII フィールドのみを ID としてマークする必要があります。より一般的なデータを表すフィールドを選択すると、関係の精度が低下し、ID グラフから関連 ID にアクセスする際にエラーが発生する可能性があります。

スキーマレジストリ API を使用してフィールドを ID としてラベル付けする方法については、[記述子の作成](../../xdm/api/descriptors.md)を参照してください。

## 次の手順

次のチュートリアルに進み、[クラスターの ID をリスト](./list-cluster-identites.md)します。