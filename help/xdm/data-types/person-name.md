---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ;スキーマ;XDM；フィールド；スキーマ;スキーマ;fullName;xdm:fullName；人物名；名前；データ型；データ型；
solution: Experience Platform
title: 個人名データタイプ
topic-legacy: overview
description: このドキュメントでは、Person Name XDMデータ型の概要を説明します。
exl-id: 5cf55fb1-b6b0-4d1c-93c3-7e2b7766599e
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '251'
ht-degree: 21%

---

# [!UICONTROL 個人] 名データ型

[!UICONTROL 個人] 名は、個人のフルネームを記述する標準のXDMデータ型です。名前構造の表記は言語や文化によって大きく異なるので、名前は常にこのデータ型を使ってモデル化する必要があります。

また、データ型には、正式な挨拶や非公式のあいさつ文の作成など、フルネームのフラグメントのみを使用する必要がある状況で使用できる多数のオプションのプロパティが用意されています。

<img src="../images/data-types/person-name.png" width="500" /><br />

| プロパティ | 説明 |
| --- | --- |
| `courtesyTitle` | 敬称、敬称の省略形（`Mr.`、`Miss.`、`Dr.`など）。 |
| `firstName` | 名前の言語で最も一般的に受け入れられる、書き込み順の名前の最初のセグメント。 |
| `fullName` | 名前の言語で最も一般的に受け入れられる、書き方の順での人のフルネーム。 |
| `lastName` | 名前の言語で最も一般的に受け入れられる、書き込み順序の名前の最後のセグメント。 |
| `middleName` | 姓と名の間に付けられたミドルネーム、代替ネームまたは追加ネーム。 |
| `suffix` | 追加情報を提供するために、人の名前の後に提供される文字のグループ（`Jr.`、`Sr.`、`M.D.`、`PhD`、`I`、`II`、`III`など）。 |

個人名データ型の詳細については、パブリックXDMリポジトリを参照してください。

* [入力済みの例](https://github.com/adobe/xdm/blob/master/components/datatypes/person-name.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/person-name.schema.json)
