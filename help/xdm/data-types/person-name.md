---
keywords: Experience Platform；ホーム；人気のトピック；スキーマ；スキーマ；XDM；フィールド；スキーマ；スキーマ；fullName;xdm:fullName；ユーザー名；名前；データタイプ；データタイプ；データタイプ；
solution: Experience Platform
title: 人物名データタイプ
description: ユーザー名 XDM データタイプについて説明します。
exl-id: 5cf55fb1-b6b0-4d1c-93c3-7e2b7766599e
source-git-commit: 1d1224b263b55b290d2cac9c07dfd1b852c4cef5
workflow-type: tm+mt
source-wordcount: '232'
ht-degree: 6%

---

# [!UICONTROL &#x200B; ユーザー名 &#x200B;] データタイプ

[!UICONTROL &#x200B; ユーザー名 &#x200B;] は、ユーザーのフルネームを記述する標準の XDM データタイプです。 名前構造の規則は言語や文化によって異なるので、名前は常にこのデータタイプを使用してモデル化する必要があります。

さらに、データタイプにはオプションのプロパティが多数用意されており、正式または非公式の挨拶の作成など、フルネームのフラグメントのみを使用する必要がある状況で使用できます。

![](../images/data-types/person-name.png){width=500}

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

* [&#x200B; 入力された例 &#x200B;](https://github.com/adobe/xdm/blob/master/components/datatypes/person/person-name.example.1.json)
* [&#x200B; 完全なスキーマ &#x200B;](https://github.com/adobe/xdm/blob/master/components/datatypes/person/person-name.schema.json)
