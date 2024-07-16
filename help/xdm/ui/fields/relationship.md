---
keywords: Experience Platform；ホーム；人気のトピック；api;API;XDM;XDM システム；エクスペリエンスデータモデル；データモデル；ui；ワークスペース；関係；フィールド；
solution: Experience Platform
title: UI での関係フィールドの定義
description: Experience Platformユーザーインターフェイスで関係フィールドを定義する方法を説明します。
exl-id: 8a6be545-0edb-4b9c-b164-e44a7a5f54f5
source-git-commit: 7021725e011a1e1d95195c6c7318ecb5afe05ac6
workflow-type: tm+mt
source-wordcount: '249'
ht-degree: 0%

---

# UI での関係フィールドの定義

エクスペリエンスデータモデル（XDM）では、[ 結合スキーマ ](../../schema/composition.md#union) は、（リアルタイム顧客プロファイル [ に対しても有効になっている、同じクラスに属するすべてのスキーマの統合ビュー ](../../../profile/home.md) です。 結合スキーマは、異なるエクスペリエンスデータから顧客の完全な表現を構築するために、プロファイルで活用されます。

場合によっては、必ずしもプロファイルの一部ではなく、プロファイルに関連付けられたデータを取り込んでいる可能性があります。 このようなデータの例として、顧客の「お気に入りのホテル」フィールドがあります。 お気に入りのホテルの属性は、その人自身の属性ではないので、ホテルは [!DNL XDM Individual Profile] ではなく、カスタムクラスに基づく別のスキーマで表すのが最善です。

結合スキーマは同じクラスを共有するスキーマにのみ基づいているので、プロファイルで使用する「Hotels」スキーマを有効にするだけで、[!DNL XDM Individual Profile] 用のフィールド結合スキーマは含まれません。 代わりに、「ホテル」と和集合に属する別のスキーマとの間の関係を定義する必要があります。 これには、参照スキーマのプライマリ ID を参照するソーススキーマでの **関係フィールド** の定義が含まれます。

Adobe Experience Platform UI で 2 つのスキーマ間の関係を定義する手順について詳しくは、[ 関係 UI チュートリアル ](../../tutorials/relationship-ui.md) を参照してください。
