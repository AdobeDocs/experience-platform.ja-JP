---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: フィールドをIDとしてラベル付けする
topic: api guide
translation-type: tm+mt
source-git-commit: 40ce232e39f62f1ee478ef05229dd2fc125ee4c0

---


# フィールドをIDとしてラベル付けする

個人を特定できる情報(PII)を含むフィールドは、IDフィールドとしてラベル付けできます。 IDフィールドに指定された値は、IDサービスによってIDとして解釈されます。 IDの名前空間は、フィールドのラベル付けの一部として指定されます。

フィールドをIDとしてラベル付けするには、次の条件を満たす必要があります。

- IDに使用できるのは、文字列型のフィールドのみです
- IDは、レコードと時系列のデータでのみ認識されます。
- PIIフィールドのみをIDとしてマークする必要があります。 より一般的なデータを表すフィールドを選択すると、関係の精度が低下し、IDグラフから関連IDにアクセスする際にエラーが発生する可能性があります。

スキーマレジストリAPIを使用してフィールドをIDとしてラベル付けする方法については、 [Create a descriptorを参照してください](../../xdm/api/descriptors.md)。

## 次の手順

次のチュートリアルに進み、 [リストクラスターID](./list-cluster-identites.md)