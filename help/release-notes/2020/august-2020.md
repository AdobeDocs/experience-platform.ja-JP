---
title: Adobe Experience Platform リリースノート
description: Experience Platformリリースノート（2020年8月11日）
doc-type: release notes
last-update: August 10, 2020
author: crhoades, ens28527
translation-type: tm+mt
source-git-commit: f881c1365684b1ca9e6bf9a8ce866d234dc54128
workflow-type: tm+mt
source-wordcount: '302'
ht-degree: 16%

---


# Adobe Experience Platform リリースノート

**リリース日：2020 年 6 月 10 日**

Adobe Experience Platformの新機能：

- [!DNL Access control](#access-control)
- [!DNL Sandboxes](#sandboxes)

## [!DNL Access control] {#access-control}

[!DNL Experience Platform] ア [ドビのAdmin Console](https://adminconsole.adobe.com) 製品プロファイルを活用して、ユーザーを権限およびサンドボックスにリンクします。 権限は、データモデリング、プロファイル管理、Sandbox管理など、様々なPlatform機能へのアクセスを制御します。

**主な特長**

| 機能 | 説明 |
|--- | ---|
| 権限 | で [!DNL Admin Console]は、 [!DNL Platform] 製品プロファイル内のタブを使用して、そのプロファイルに関連付けられたユーザが使用できる [!DNL Platform] 機能をカスタマイズできます。 使用できる権限カテゴリは次のとおりです。 [!UICONTROL データモデリング，]データ管理 [!UICONTROL ,][!UICONTROL プロファイル管理], ID, [!UICONTROL ID, ID], ID, Data Data Data Data Adata Administration, SANDBOX, DESTINATIONS, DESTINATIONS, DESTINATIONS, DE, SOU , |
| サンドボックスへのアクセス | [!UICONTROL _製品プロファイル内の「_]権限[!DNL Platform]」タブで、ユーザーに特定のサンドボックスへのアクセス権を付与できます。 詳しくは、[以下のサンドボックスの節を参照してください](#sandboxes)。 |

詳しくは、 [アクセス制御の概要を参照してください](../../access-control/home.md)。

## [!DNL Sandboxes] {#sandboxes}

[!DNL Experience Platform] は、デジタルエクスペリエンスアプリケーションをグローバルに拡張するために設計されています。 企業ではしばしば複数のデジタルエクスペリエンスアプリケーションを並行して運用し、運用コンプライアンスを確保しながら、アプリケーションの開発、テスト、導入に注力する必要があります。In order to address this need, [!DNL Experience Platform] provides sandboxes which partition a single [!DNL Platform] instance into separate virtual environments to help develop and evolve digital experience applications.

**主な特長**

| 機能 | 説明 |
|--- | ---|
| 実稼働用サンドボックス | [!DNL Experience Platform] 削除やリセットができない、単一の実稼働用サンドボックスを提供します。 |
| 非実稼働用サンドボックス | 複数の非実稼働サンドボックスを1つの [!DNL Platform] インスタンスに対して作成できるので、実稼働サンドボックスに影響を与えることなく、機能のテスト、実験の実行、カスタム設定を行うことができます。 |
| サンドボックス切り替え | ユーザーインターフェイスでは、画面の左上隅にあるサンドボックス切り替えボタンを使用すると、ドロップダウンメニューから使用可能なサンドボックスを切り替えることができます。 [!DNL Experience Platform] |
| `x-sandbox-name` header | APIへの呼び出しには、新しい [!DNL Experience Platform] ヘッダーが含まれている必要があります。このヘッダーの値は、操作が実行されるサンドボックスの `x-sandbox-name``name` 属性を参照します。 |

詳しくは、 [サンドボックスの概要を参照してください](../../sandboxes/home.md)。