---
keywords: Experience Platform;home;popular topics;schema;Schema;XDM;individual profile;fields;schemas;Schemas;identityMap;identity map;Identity map;Schema design;map;Map;union schema;union
solution: Experience Platform
title: IdentityMapミックスイン
topic: overview
description: このドキュメントでは、XDM Individualプロファイルクラスの概要を説明します。
translation-type: tm+mt
source-git-commit: fd5bd4134bd35d5d87c79bf1e75bc88c67511b2b
workflow-type: tm+mt
source-wordcount: '96'
ht-degree: 0%

---


# [!UICONTROL IdentityMap] mixin

[!UICONTROL IdentityMap] は、 [[!DNL XDM Individual Profile] クラスの標準ミックスインです](../../classes/individual-profile.md)。 ミックスインは単一のマップフィールドを提供します。このフィールドには、名前空間がキーを設定した一連のユーザIDが含まれます。

>[!WARNING]
>
>本発明の `IdentityMap` フィールドは、アイデンティティデータが取り込まれると、システムによって自動的に更新される。 このフィールドを [リアルタイム顧客プロファイルに適切に活用するため](../../../profile/home.md)、データ操作で手動でフィールドの内容を更新しないでください。

<img src="../../images/mixins/identitymap.png" width="600" /><br />

使用事例について詳しくは、スキーマ構成の [基本のIDマップに関する節を参照してください](../../schema/composition.md#identityMap) 。