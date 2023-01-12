---
keywords: Experience Platform；ホーム；人気のトピック；API;API;XDM;XDM システム；エクスペリエンスデータモデル；データモデル；ui；ワークスペース；関係；フィールド；
solution: Experience Platform
title: UI での関係フィールドの定義
description: 関係ユーザーインターフェイスで関係フィールドを定義するExperience Platformを説明します。
exl-id: 8a6be545-0edb-4b9c-b164-e44a7a5f54f5
source-git-commit: 7021725e011a1e1d95195c6c7318ecb5afe05ac6
workflow-type: tm+mt
source-wordcount: '249'
ht-degree: 0%

---

# UI での関係フィールドの定義

エクスペリエンスデータモデル (XDM) で、 [和集合スキーマ](../../schema/composition.md#union) は、同じクラスに属するすべてのスキーマの統合ビューで、次に対しても有効になっています [リアルタイム顧客プロファイル](../../../profile/home.md). 和集合スキーマは、異なるエクスペリエンスデータから顧客を完全に表現するために、プロファイルで利用されます。

場合によっては、必ずしもプロファイルの一部ではなく、プロファイルに関連するデータを取り込むことがあります。 この種のデータの例としては、顧客の「お気に入りのホテル」フィールドが挙げられます。 人物のお気に入りのホテルの属性は、人物自身の属性ではないので、ホテルは、カスタムクラスに基づく別のスキーマで表されるのが最適です。 [!DNL XDM Individual Profile].

和集合スキーマは同じクラスを共有するスキーマにのみ基づいているので、プロファイルで使用する「Hotels」スキーマを有効にしても、 [!DNL XDM Individual Profile]. 代わりに、「Hotels」と和集合に属する別のスキーマとの関係を定義する必要があります。 これには、 **関係フィールド** 参照スキーマのプライマリ id を参照するソーススキーマ内。

Adobe Experience Platform UI で 2 つのスキーマ間の関係を定義する手順について詳しくは、 [relationship UI チュートリアル](../../tutorials/relationship-ui.md).
