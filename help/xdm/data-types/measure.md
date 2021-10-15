---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ；スキーマ；XDM；フィールド；スキーマ；スキーマ；メジャー；データ型；データ型；データ型；
solution: Experience Platform
title: 測定データタイプ
topic-legacy: overview
description: このドキュメントでは、エクスペリエンスデータモデル (XDM) の測定データタイプの概要を説明します。
exl-id: 5d6cc15d-63cf-4af5-9ae9-12c886dd6735
source-git-commit: 39d04cf482e862569277211d465bb2060a49224a
workflow-type: tm+mt
source-wordcount: '230'
ht-degree: 6%

---

#  Measureddata type

 測定は、特定の指標の具体的な定量化可能なデータポイントを含む標準エクスペリエンスデータモデル (XDM) データ型です。測定は、一意の識別子と値で構成されます。

<img src="../images/data-types/measure.PNG" width="500" /><br />

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `id` | 文字列 | このメジャーの一意の識別子。 モバイルアプリやオフライン機能を備えた Web サイトなど、非可逆通信チャネルを使用したデータ収集では、測定の送信が保証されない場合、このプロパティには、測定のクライアント生成の一意の ID が含まれます。 この時間を十分に長くして、十分なランダム性を確保することをお勧めします。 <br><br> の生成時にタイムスタンプ、デバイス ID、IP、MACアドレス、その他のユーザー識別値などの情報が組み込まれている場合は、結果をハッシュ化する必要がありま `id`す。これにより、PII が値にエンコードされなくなります。これは、ユーザーやデバイスを識別するのではなく、特定の測定時間が目標となるからです。 |
| `value` | Double | この測定の定量化可能値。 |

{style=&quot;table-layout:auto&quot;}

データタイプについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力例](https://github.com/adobe/xdm/blob/master/components/datatypes/data/measure.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/data/measure.schema.json)
