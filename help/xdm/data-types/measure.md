---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ；スキーマ；XDM；フィールド；スキーマ；スキーマ；測定；データ型；データ型；データ型；
solution: Experience Platform
title: 測定データタイプ
topic-legacy: overview
description: このドキュメントでは、エクスペリエンスデータモデル(XDM)の測定データタイプの概要を説明します。
exl-id: 5d6cc15d-63cf-4af5-9ae9-12c886dd6735
source-git-commit: 39d04cf482e862569277211d465bb2060a49224a
workflow-type: tm+mt
source-wordcount: '230'
ht-degree: 6%

---

#  Measureddata type

 測定は、特定の指標の具体的な定量化可能なデータポイントを含む、標準のエクスペリエンスデータモデル(XDM)データ型です。測定は、一意の識別子と値で構成されます。

<img src="../images/data-types/measure.PNG" width="500" /><br />

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `id` | 文字列 | このメジャーの一意の識別子。 モバイルアプリやオフライン機能を備えたWebサイトなど、非可逆通信チャネルを使用したデータ収集では、測定の送信が保証されない場合、このプロパティには、測定のクライアント生成の一意のIDが含まれます。 十分な長さに設定してランダム性を確保することをお勧めします。 <br><br> の生成時にタイムスタンプ、デバイスID、IP、MACアドレス、その他の潜在的なユーザー識別値などの情報が組み込まれる場合は、結果をハッシュ化する必要がありま `id`す。これにより、PIIが値にエンコードされることがなくなります。これは、目標はユーザーやデバイスを識別することではなく、特定の時間計測を行うことです。 |
| `value` | Double | この測定の定量化可能値。 |

{style=&quot;table-layout:auto&quot;}

データタイプについて詳しくは、パブリックXDMリポジトリを参照してください。

* [入力済みの例](https://github.com/adobe/xdm/blob/master/components/datatypes/data/measure.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/data/measure.schema.json)
