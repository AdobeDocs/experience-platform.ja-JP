---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ；スキーマ；XDM；フィールド；スキーマ；スキーマ；アプリケーション；データ型；データ型；データ型；
solution: Experience Platform
title: アプリケーションデータタイプ
topic-legacy: overview
description: このドキュメントでは、Application Experience Data Model(XDM) データタイプの概要を説明します。
exl-id: ac7d6761-7b58-4e0d-85e7-6f157fb2eea5
source-git-commit: 39d04cf482e862569277211d465bb2060a49224a
workflow-type: tm+mt
source-wordcount: '441'
ht-degree: 5%

---

#  Applicationdata type

 アプリケーションは、アプリケーションが生成するインタラクションに関連する詳細を記述する標準のエクスペリエンスデータモデル (XDM) データ型です。アプリケーションとは、エンドユーザーがインストール、実行、閉じる、またはアンインストールできるモバイルアプリケーションやデスクトップアプリケーションなどのソフトウェアエクスペリエンスを指します。 このデータ型のプロパティは、チャットボット、ブラウザーベースのプラグイン、またはアプリケーションに適用されないその他のエクスペリエンスなどのエージェントを記述する目的ではありません。

<img src="../images/data-types/application.PNG" width="500" /><br />

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `applicationCloses` | [[!UICONTROL 測定]](./measure.md) | アプリケーションの終了に関する詳細を説明します。 |
| `crashes` | [[!UICONTROL 測定]](./measure.md) | このプロパティは、アプリケーションが意図したとおりに終了しない場合にトリガーします。 |
| `featureUsages` | [[!UICONTROL 測定]](./measure.md) | 測定中のアプリケーション機能のアクティベーションからのデータを示します。 |
| `firstLaunches` | [[!UICONTROL 測定]](./measure.md) | 最初の起動時のデータが含まれます。 このプロパティは、インストール後の最初の起動時にトリガーされます。 |
| `installs` | [[!UICONTROL 測定]](./measure.md) | 特定のインストールイベントが使用可能な場合に、デバイスへのアプリケーションのインストールを記録します。 |
| `launches` | [[!UICONTROL 測定]](./measure.md) | アプリケーションの起動に関連付けられた値を示します。 これは、実行のたびに、セッションのタイムアウトを超えたときに、クラッシュ、インストール、バックグラウンドからの再開を含めてトリガーされます。 |
| `upgrades` | [[!UICONTROL 測定]](./measure.md) | 以前にインストールされたアプリケーションのアップグレードに関するデータが含まれます。 これは、アップグレード後の最初の起動時にトリガーされます。 |
| `id` | 文字列 | アプリケーションの一意の識別子。 |
| `name` | 文字列 |  アプリケーションの名前 |
| `userPerspective` | 文字列 | イベントが発生した時点でのユーザーとアプリまたはブランドとの間の視点または物理的な関係。 アプリに関するユーザーの視点を理解すると、「アクティブ」なセッションの一部として `background` イベントと `detached` イベントを含めないほとんどの場合、セッションを正確に生成できます。 このプロパティの値は、以下に示す列挙値の 1 つと等しい必要があります。 <li> `foreground`:ユーザーとアプリが直接お互いにやり取りしている。 </li> <li> `background`:アプリとユーザーは間接的に相互作用します。例えば、画面がロックされている間や別のアプリがフォアグラウンドで使用されている間に、アプリは値を測定して更新することができます。  </li> <li> `detached`:デタッチとは、イベントがアプリに関連していたが、外部システムからの電子メールの送信やプッシュ通知など、アプリから直接来ていなかったことを意味します。 |
| `version` | 文字列 | アプリケーションのバージョン。 |

{style=&quot;table-layout:auto&quot;}

データタイプについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力例](https://github.com/adobe/xdm/blob/master/components/datatypes/channels/application.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/channels/application.schema.json)
