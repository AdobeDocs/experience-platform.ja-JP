---
keywords: Experience Platform；ホーム；人気のあるトピック；api;API;XDM;XDM システム；エクスペリエンスデータモデル；データモデル；ui；ワークスペース；関係；フィールド；
solution: Experience Platform
title: UI での関係フィールドの定義
description: 関係ユーザーインターフェイスで関係フィールドを定義するExperience Platformを説明します。
topic-legacy: user guide
exl-id: 8a6be545-0edb-4b9c-b164-e44a7a5f54f5
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '249'
ht-degree: 0%

---

# UI での関係フィールドの定義

エクスペリエンスデータモデル (XDM) では、[ 和集合スキーマ ](../../schema/composition.md#union) は、[ リアルタイム顧客プロファイル ](../../../profile/home.md) に対しても有効になった、同じクラスに属するすべてのスキーマの統合ビューです。 和集合スキーマは、様々なエクスペリエンスデータから顧客の完全な表現を構築するために、プロファイルで利用されます。

場合によっては、必ずしもプロファイルの一部ではなく、プロファイルに関連するデータを取り込むことがあります。 この種のデータの例として、顧客の「お気に入りのホテル」フィールドが挙げられます。 人のお気に入りのホテルの属性は人の属性ではないので、ホテルは [!DNL XDM Individual Profile] ではなく、カスタムクラスに基づく別のスキーマで表すのが最適です。

和集合スキーマは同じクラスを共有するスキーマにのみ基づいているので、プロファイルで使用する「Hotels」スキーマを有効にしても、[!DNL XDM Individual Profile] のフィールド和集合スキーマは含まれません。 代わりに、「Hotels」と和集合に属する別のスキーマとの関係を定義する必要があります。 これには、宛先スキーマのプライマリ ID を参照するソーススキーマに **関係フィールド** を定義する必要があります。

Adobe Experience Platform UI で 2 つのスキーマ間の関係を定義する手順について詳しくは、[ 関係 UI のチュートリアル ](../../tutorials/relationship-ui.md) を参照してください。
