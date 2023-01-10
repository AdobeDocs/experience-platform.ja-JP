---
keywords: Experience Platform；ホーム；人気のトピック；スキーマ；スキーマ；XDM；フィールド；スキーマ；fullName;xdm:fullName；ユーザー名；名前；データ型；データ型；
solution: Experience Platform
title: 担当者名データタイプ
description: このドキュメントでは、ユーザー名 XDM データタイプの概要を説明します。
exl-id: 5cf55fb1-b6b0-4d1c-93c3-7e2b7766599e
source-git-commit: 60c0bd62b4effaa161c61ab304718ab8c20a06e1
workflow-type: tm+mt
source-wordcount: '256'
ht-degree: 21%

---

# [!UICONTROL 人物名] データタイプ

[!UICONTROL 人物名] は、個人の完全名を表す標準的な XDM データ型です。 名前構造の規則は言語や文化によって大きく異なるので、名前は常にこのデータ型を使用してモデル化する必要があります。

また、データ型には、正式な挨拶や非公式の挨拶の作成など、完全な名前のフラグメントのみを使用する必要がある状況で使用できる、多数のオプションのプロパティが用意されています。

<img src="../images/data-types/person-name.png" width="500" /><br />

| プロパティ | 説明 |
| --- | --- |
| `courtesyTitle` | 人の肩書き、敬称、あいさつ文 ( 例： `Mr.`, `Miss.`または `Dr.`) をクリックします。 |
| `firstName` | 名前の言語で最も一般的に受け入れられる、書き込み順の名前の最初のセグメント。 |
| `fullName` | 名前の言語で最も一般的に受け入れられる書き込み順での人のフルネーム。 |
| `lastName` | 名前の言語で最も一般的に受け入れられる、書き込み順序の名前の最後のセグメント。 |
| `middleName` | 姓と名の間に付けられたミドルネーム、代替ネームまたは追加ネーム。 |
| `suffix` | 追加情報 ( 例えば、 `Jr.`, `Sr.`, `M.D.`, `PhD`, `I`, `II`, `III`など )。 |

{style=&quot;table-layout:auto&quot;}

人物名データタイプについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力された例](https://github.com/adobe/xdm/blob/master/components/datatypes/person/person-name.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/person/person-name.schema.json)
