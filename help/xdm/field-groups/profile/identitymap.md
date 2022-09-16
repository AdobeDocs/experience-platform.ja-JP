---
keywords: Experience Platform；ホーム；人気のトピック；スキーマ；スキーマ；スキーマ；XDM；個々のプロファイル；フィールド；スキーマ；スキーマ；identityMap;ID マップ；ID マップ；スキーマデザイン；マップ；和集合スキーマ；和集合
solution: Experience Platform
title: IdentityMap スキーマフィールドグループ
topic-legacy: overview
description: このドキュメントでは、XDM Individual Profile クラスの概要を説明します。
exl-id: c9928e85-ef1e-4739-ba1d-80505a9e60c3
source-git-commit: d425dcd9caf8fccd0cb35e1bac73950a6042a0f8
workflow-type: tm+mt
source-wordcount: '148'
ht-degree: 36%

---


# [!UICONTROL IdentityMap] スキーマフィールドグループ

>[!NOTE]
>
>複数のスキーマフィールドグループの名前が変更されました。 詳しくは、[フィールドグループ名の更新](../name-updates.md)のドキュメントを参照してください。

[!UICONTROL IdentityMap] は、 [[!DNL XDM Individual Profile] クラス](../../classes/individual-profile.md). フィールドグループは、名前空間でキー設定された一連のユーザー ID を含む、単一のマップフィールドを提供します。

>[!WARNING]
>
>この `IdentityMap` フィールドは、id データが取り込まれると自動的に更新されます。 このフィールドを適切に利用するため、[リアルタイム顧客プロファイル](../../../profile/home.md)の場合は、データ操作でフィールドの内容を手動で更新しようとしないでください。

<img src="../../images/field-groups/identitymap.png" width="600" /><br />

そのユースケースについては、[スキーマ構成の基本](../../schema/composition.md#identityMap) の ID マップの節を参照してください。
