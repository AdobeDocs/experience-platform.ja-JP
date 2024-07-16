---
keywords: Experience Platform；ホーム；人気のトピック；ラベル ID
solution: Experience Platform
title: フィールドを ID としてラベル付け
description: 個人を特定できる情報（PII）を含むフィールドは、ID フィールドとしてラベル付けできます。ID フィールドに指定された値は、ID サービスによって ID として解釈されます。ID の名前空間は、フィールドのラベル付けの一部として指定されます。
role: Developer
exl-id: f0b3f18b-7302-4a0b-b444-2d4b59787681
source-git-commit: c16ce1020670065ecc5415bc3e9ca428adbbd50c
workflow-type: tm+mt
source-wordcount: '196'
ht-degree: 73%

---

# フィールドを ID としてラベル付け

個人を特定できる情報（PII）を含むフィールドは、ID フィールドとしてラベル付けできます。ID フィールドに指定された値は、[!DNL Identity Service] によって ID として解釈されます。 ID の名前空間は、フィールドのラベル付けの一部として指定されます。

フィールドを ID としてラベル付けするには、次の条件が満たされている必要があります。

- ID に使用できるのは、文字列型のフィールドのみです。
- ID は、レコードと時系列データのみで認識されます。
- PII フィールドのみを ID としてマークする必要があります。より一般的なデータを表すフィールドを選択すると、関係の精度が低下し、ID グラフから関連 ID にアクセスする際にエラーが発生する可能性があります。

スキーマレジストリ API を使用してフィールドを ID としてラベル付けする方法については、[ 記述子エンドポイントガイド ](../../xdm/api/descriptors.md#create) を参照してください。

## 次の手順

次のチュートリアルに進み、[クラスターの ID をリスト](./list-cluster-identites.md)します。
