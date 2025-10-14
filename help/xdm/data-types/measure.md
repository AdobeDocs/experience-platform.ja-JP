---
keywords: Experience Platform；ホーム；人気のトピック；スキーマ；XDM；フィールド；スキーマ；スキーマ；メジャー；データタイプ；データタイプ；データタイプ；
solution: Experience Platform
title: 測定データタイプ
description: メジャーエクスペリエンスデータモデル（XDM）データタイプについて説明します。
exl-id: 5d6cc15d-63cf-4af5-9ae9-12c886dd6735
source-git-commit: e028fbb82b37b3940b308a860c26f8b5f9884d3a
workflow-type: tm+mt
source-wordcount: '208'
ht-degree: 5%

---

# [!UICONTROL &#x200B; 測定 &#x200B;] データタイプ

[!UICONTROL &#x200B; 測定 &#x200B;] は、特定の指標の具体的な定量化可能なデータポイントを含んだ、標準の Experience Data Model （XDM）データタイプです。 測定は、一意の ID と値で構成されます。

![&#x200B; 画像を測定 &#x200B;](../images/data-types/measure.PNG){ 幅=500}

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `id` | 文字列 | この測定の一意の ID。 モバイルアプリやオフライン機能を備えた web サイトなど、手段の送信が確保できない非可逆の通信チャネルを使用したデータ収集の場合、このプロパティには、クライアントで生成される、実行された手段の一意の ID が含まれます。 ランダム性を十分に確保するには、これを十分に長くすることをお勧めします。 <br><br> タイムスタンプ、デバイス ID、IP、MAC アドレス、その他のユーザーが識別できる可能性のある値などの情報が `id` の生成に含まれる場合、結果はハッシュ化される必要があります。 目標はユーザーやデバイスを識別することではなく、特定の指標を時間内に特定することなので、これにより、PII が値にエンコードされなくなります。 |
| `value` | Double | この測定の定量化可能値。 |

{style="table-layout:auto"}

データタイプについて詳しくは、公開 XDM リポジトリを参照してください。

* [&#x200B; 入力された例 &#x200B;](https://github.com/adobe/xdm/blob/master/components/datatypes/data/measure.example.1.json)
* [&#x200B; 完全なスキーマ &#x200B;](https://github.com/adobe/xdm/blob/master/components/datatypes/data/measure.schema.json)
