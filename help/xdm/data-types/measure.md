---
keywords: Experience Platform；ホーム；人気のトピック；スキーマ；スキーマ；XDM；フィールド；スキーマ；スキーマ；測定；データ型；データ型；
solution: Experience Platform
title: 測定データタイプ
description: このドキュメントでは、エクスペリエンスデータモデル (XDM) データタイプの測定の概要を説明します。
exl-id: 5d6cc15d-63cf-4af5-9ae9-12c886dd6735
source-git-commit: 60c0bd62b4effaa161c61ab304718ab8c20a06e1
workflow-type: tm+mt
source-wordcount: '227'
ht-degree: 5%

---

# [!UICONTROL 測定] データタイプ

[!UICONTROL 測定] は、特定の指標の具体的な定量化可能なデータポイントを含む標準的なエクスペリエンスデータモデル (XDM) データタイプです。 測定は、一意の識別子と値で構成されます。

<img src="../images/data-types/measure.PNG" width="500" /><br />

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `id` | 文字列 | この測定の一意の ID。 モバイルアプリやオフライン機能を備えた Web サイトなど、非可逆通信チャネルを使用したデータ収集で測定の送信が保証されない場合、このプロパティには、実行された測定のクライアント生成の一意の ID が含まれます。 この時間を十分に長くして、十分なランダム性を確保することをお勧めします。 <br><br> タイムスタンプ、デバイス ID、IP、MACアドレスなどのユーザー識別値の情報が、 `id`の場合、結果はハッシュ化する必要があります。 これにより、ユーザーやデバイスを識別するのではなく、特定の測定を時間的に行うことが目的なので、値に PII がエンコードされなくなります。 |
| `value` | Double | この測定の定量化可能値。 |

{style="table-layout:auto"}

データタイプについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力された例](https://github.com/adobe/xdm/blob/master/components/datatypes/data/measure.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/data/measure.schema.json)
