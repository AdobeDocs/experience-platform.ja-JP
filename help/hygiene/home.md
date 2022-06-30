---
title: データハイジーンの概要
description: Adobe Experience Platform のデータハイジーンを使用すると、古くなったレコードや不正確なレコードを更新またはパージして、データのライフサイクルを管理できます。
exl-id: 104a2bb8-3242-4a20-b98d-ad6df8071a16
source-git-commit: 22da9e39e168d9a995c7c134733aa7a1b3587749
workflow-type: tm+mt
source-wordcount: '220'
ht-degree: 100%

---

# Adobe Experience Platform のデータハイジーン

>[!IMPORTANT]
>
>Adobe Experience Platform のデータハイジーン機能は、現在、Adobe Shield for Healthcare を購入した組織でのみ利用できます。

Adobe Experience Platform では、カスタマーエクスペリエンスを調整するために、大規模で複雑なデータ操作を管理するための堅牢なツールのセットを提供しています。長い期間をかけてデータがシステムに取り込まれるにつれて、データが期待通りに使用され、間違ったデータを修正する必要がある場合は更新され、組織のポリシーで必要と判断された場合は削除されるように、データストアを管理することがますます重要になります。

Platform のデータハイジーン機能を使用すると、プログラムによるデータセット削除を通じて、保存済みの消費者データを管理できます。

## [!UICONTROL データハイジーン] UI ワークスペース

Platform UI の[!UICONTROL データハイジーン]ワークスペースを使用すると、データハイジーン操作の設定とスケジュール設定ができ、レコードが期待どおりに維持されていることを確認するのに便利です。

UI でのデータハイジーンタスクの管理手順については、[データハイジーン UI ガイド](./ui/overview.md)を参照してください。

## Data Hygiene API

[!UICONTROL データハイジーン] UI は、Data Hygiene API をベースに構築されており、そのエンドポイントは、データハイジーン活動を自動化したい場合に、直接使用できます。詳しくは、[Data Hygiene API ガイド](./api/overview.md)を参照してください。
