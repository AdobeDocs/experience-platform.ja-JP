---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ;スキーマ;XDM；フィールド；スキーマ;スキーマ；測定；データ型；データ型；
solution: Experience Platform
title: 測定データタイプ
topic-legacy: overview
description: このドキュメントでは、Measure Experience Data Model(XDM)データタイプの概要を説明します。
exl-id: 5d6cc15d-63cf-4af5-9ae9-12c886dd6735
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '227'
ht-degree: 5%

---

#  Measuredata型

 Measureisは、特定の指標の具体的な定量化データポイントを含む、標準のExperience Data Model(XDM)データ型です。メジャーは、一意の識別子と値で構成されます。

<img src="../images/data-types/measure.PNG" width="500" /><br />

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `id` | 文字列 | このメジャーの一意の識別子。 モバイルアプリやオフライン機能を備えたWebサイトなど、非可逆の通信チャネルを使用したデータ収集の場合、測定の送信が保証されないとき、このプロパティには、実行された測定のクライアント生成の固有IDが含まれます。 ランダム性を十分に確保するために、十分に長くすることをお勧めします。 <br><br> の生成時にタイムスタンプ、デバイスID、IP、MACアドレスなどの情報や、ユーザー識別の可能性があるその他の値が組み込まれている場合、結果 `id`はハッシュされる必要があります。これにより、ユーザーやデバイスを識別するのではなく、特定の時間の測定を行うので、値にPIIがエンコードされなくなります。 |
| `value` | Double | この測定の定量化可能値。 |

データ型の詳細については、パブリックXDMリポジトリを参照してください。

* [入力済みの例](https://github.com/adobe/xdm/blob/master/components/datatypes/data/measure.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/data/measure.schema.json)
