---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: フィールドをIDとしてラベル付けする
topic: api guide
translation-type: tm+mt
source-git-commit: 40ce232e39f62f1ee478ef05229dd2fc125ee4c0
workflow-type: tm+mt
source-wordcount: '147'
ht-degree: 1%

---


# フィールドをIDとしてラベル付けする

個人識別情報(PII)を含むフィールドは、識別情報フィールドとしてラベル付けできます。 IDフィールドに指定された値は、IDサービスによってIDとして解釈されます。 IDの名前空間は、フィールドのラベル付けの一部として指定されます。

フィールドにIDのラベルを付けるには、次の条件を満たす必要があります。

- IDに使用できるのは、文字列タイプのフィールドのみです
- IDは、レコードと時系列データでのみ認識されます。
- PIIフィールドのみをIDとしてマークする必要があります。 より一般的なデータを表すフィールドを選択すると、関係があまり正確でなくなり、IDグラフから関連IDにアクセスする際にエラーが発生する可能性があります

スキーマレジストリAPIを使用してフィールドにIDのラベルを付ける方法については、 [Create a descriptor](../../xdm/api/descriptors.md).を参照してください。

## 次の手順

次のチュートリアルに進み、 [リストクラスタIDを確認します。](./list-cluster-identites.md)