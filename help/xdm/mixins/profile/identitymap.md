---
keywords: Experience Platform;home;popular topics;schema;Schema;XDM;individual profile;fields;schemas;Schemas;identityMap;identity map;Identity map;Schema design;map;Map;union schema;union
solution: Experience Platform
title: IdentityMapミックスイン
topic: overview
description: このドキュメントでは、XDM Individualプロファイルクラスの概要を説明します。
translation-type: tm+mt
source-git-commit: f9d8021643e72e3fbb5315b54a19815dcdaaa702
workflow-type: tm+mt
source-wordcount: '113'
ht-degree: 0%

---


# [!UICONTROL IdentityMap] mixin

>[!NOTE]
>
>いくつかのミックスインの名前が変更されました。 詳しくは、 [mixin名の更新に関するドキュメントを参照してください](../name-updates.md) 。

[!UICONTROL IdentityMap] は、 [[!DNL XDM Individual Profile] クラスの標準ミックスインです](../../classes/individual-profile.md)。 ミックスインは単一のマップフィールドを提供します。このフィールドには、名前空間がキーを設定した一連のユーザIDが含まれます。

>[!WARNING]
>
>本発明の `IdentityMap` フィールドは、アイデンティティデータが取り込まれると、システムによって自動的に更新される。 このフィールドを [リアルタイム顧客プロファイルに適切に活用するため](../../../profile/home.md)、データ操作で手動でフィールドの内容を更新しないでください。

<img src="../../images/mixins/identitymap.png" width="600" /><br />

使用事例について詳しくは、スキーマ構成の [基本のIDマップに関する節を参照してください](../../schema/composition.md#identityMap) 。