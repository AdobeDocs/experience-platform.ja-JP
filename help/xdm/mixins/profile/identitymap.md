---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ;スキーマ;XDM；個々のプロファイル；フィールド；スキーマ;スキーマ;identityMap；アイデンティティマップ；スキーマ設計；マップ；和集合スキーマ;和集合
solution: Experience Platform
title: IdentityMap Mixin
topic: overview
description: このドキュメントでは、XDM Individualプロファイルクラスの概要を説明します。
translation-type: tm+mt
source-git-commit: f2238d35f3e2a279fbe8ef8b581282102039e932
workflow-type: tm+mt
source-wordcount: '138'
ht-degree: 0%

---


#  IdentityMapmixin

>[!NOTE]
>
>いくつかのミックスインの名前が変更されました。 詳しくは、[mixin name updates](../name-updates.md)のドキュメントを参照してください。

 IdentityMapは、ク [[!DNL XDM Individual Profile] ラスの標準ミックスインです](../../classes/individual-profile.md)。ミックスインは単一のマップフィールドを提供します。このフィールドには、名前空間がキーを設定した一連のユーザIDが含まれます。

>[!WARNING]
>
>`IdentityMap`フィールドは、IDデータが取り込まれると自動的にシステムによって更新されます。 このフィールドを[リアルタイム顧客プロファイル](../../../profile/home.md)に適切に利用するため、データ操作でフィールドの内容を手動で更新しないでください。

<img src="../../images/mixins/identitymap.png" width="600" /><br />

使用事例の詳細については、[スキーマ構成の基本](../../schema/composition.md#identityMap)のIDマップに関する節を参照してください。