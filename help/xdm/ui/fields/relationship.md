---
keywords: Experience Platform；ホーム；人気のあるトピック；API;API;XDM;XDMシステム；エクスペリエンスデータモデル；データモデル；ui；ワークスペース；リレーションシップ；フィールド；
solution: Experience Platform
title: UIでの関係フィールドの定義
description: Experience Platformユーザーインターフェイスで関係フィールドを定義する方法を説明します。
topic-legacy: user guide
exl-id: 8a6be545-0edb-4b9c-b164-e44a7a5f54f5
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '249'
ht-degree: 0%

---

# UIでの関係フィールドの定義

エクスペリエンスデータモデル(XDM)では、[和集合スキーマ](../../schema/composition.md#union)は、[リアルタイム顧客プロファイル](../../../profile/home.md)に対しても有効になった同じクラスに属するすべてのスキーマの統合表示です。 和集合スキーマは、個別のエクスペリエンスデータから顧客の完全な表現を構築するためにプロファイルが利用します。

場合によっては、プロファイルの一部ではなく、プロファイルに関連したデータを取り込むことがあります。 このようなデータの例としては、顧客の「お気に入りのホテル」フィールドが挙げられます。 人のお気に入りのホテルの属性は、その人の属性ではないので、ホテルは、[!DNL XDM Individual Profile]ではなく、カスタムクラスに基づく別のスキーマで表すのが最適です。

和集合スキーマは同じクラスを共有するスキーマに基づいているので、プロファイルで使用する「Hotels」スキーマを有効にした場合、[!DNL XDM Individual Profile]のフィールド和集合スキーマは含まれません。 代わりに、「Hotels」と和集合に属する別のスキーマとの関係を定義する必要があります。 これには、宛先スキーマのプライマリIDを参照するソーススキーマに&#x200B;**関係フィールド**&#x200B;を定義する必要があります。

Adobe Experience PlatformUIで2つのスキーマ間の関係を定義する手順について詳しくは、[関係UIチュートリアル](../../tutorials/relationship-ui.md)を参照してください。
