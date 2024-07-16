---
keywords: Experience Platform；ホーム；人気のトピック；スキーマ；スキーマ；XDM；フィールド；スキーマ；スキーマ；fullName;xdm:fullName；ユーザー名；名前；データタイプ；データタイプ；データタイプ；
solution: Experience Platform
title: 人物名データタイプ
description: ユーザー名 XDM データタイプについて説明します。
exl-id: 5cf55fb1-b6b0-4d1c-93c3-7e2b7766599e
source-git-commit: de8e944cfec3b52d25bb02bcfebe57d6a2a35e39
workflow-type: tm+mt
source-wordcount: '230'
ht-degree: 6%

---

# [!UICONTROL  ユーザー名 ] データタイプ

[!UICONTROL  ユーザー名 ] は、ユーザーのフルネームを記述する標準の XDM データタイプです。 名前構造の規則は言語や文化によって異なるので、名前は常にこのデータタイプを使用してモデル化する必要があります。

さらに、データタイプにはオプションのプロパティが多数用意されており、正式または非公式の挨拶の作成など、フルネームのフラグメントのみを使用する必要がある状況で使用できます。

<img src="../images/data-types/person-name.png" width="500" /><br />

| プロパティ | 説明 |
| --- | --- |
| `courtesyTitle` | 個人の肩書き、敬称、あいさつ文の略称（`Mr.`、`Miss.`、`Dr.` など）。 |
| `firstName` | 名前の言語で最も一般的に受け入れられている、書き順の名前の最初のセグメント。 |
| `fullName` | 名前の言語で最も一般的に受け入れられている書き順での、人物の氏名。 |
| `lastName` | 名前の言語で最も一般的に受け入れられている、書き順の名前の最後のセグメント。 |
| `middleName` | 姓と名の間に付けられたミドルネーム、代替ネームまたは追加ネーム。 |
| `suffix` | 追加情報（`Jr.`、`Sr.`、`M.D.`、`PhD`、`I`、`II`、`III` など）を提供するために人物の名前の後に提供される文字のグループ。 |

{style="table-layout:auto"}

人物名データタイプについて詳しくは、公開 XDM リポジトリを参照してください。

* [ 入力された例 ](https://github.com/adobe/xdm/blob/master/components/datatypes/person/person-name.example.1.json)
* [ 完全なスキーマ ](https://github.com/adobe/xdm/blob/master/components/datatypes/person/person-name.schema.json)
