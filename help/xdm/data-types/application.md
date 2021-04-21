---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ;スキーマ;XDM；フィールド；スキーマ;スキーマ；アプリケーション；データ型；データ型；
solution: Experience Platform
title: Application Data Type
topic-legacy: overview
description: このドキュメントでは、Application Experience Data Model(XDM)データ型の概要を説明します。
exl-id: ac7d6761-7b58-4e0d-85e7-6f157fb2eea5
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '438'
ht-degree: 4%

---

#  Applicationdata型

 Applicationは、アプリケーションで生成されたインタラクションに関連する詳細を説明する、標準のExperience Data Model(XDM)データ型です。アプリケーションとは、モバイルやデスクトップアプリケーションなど、エンドユーザーがインストール、実行、閉じる、アンインストールできるソフトウェアの使用体験のことです。 このデータ型のプロパティは、チャットボット、ブラウザーベースのプラグインなど、アプリケーションに適用されない他のエクスペリエンスを記述する目的ではありません。

<img src="../images/data-types/application.PNG" width="500" /><br />

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `applicationCloses` | [[!UICONTROL 測定]](./measure.md) | アプリケーションの終了の詳細を説明します。 |
| `crashes` | [[!UICONTROL 測定]](./measure.md) | このプロパティは、アプリケーションが意図したとおりに終了しない場合にトリガーします。 |
| `featureUsages` | [[!UICONTROL 測定]](./measure.md) | 測定するアプリケーション機能のアクティベーションのデータを示します。 |
| `firstLaunches` | [[!UICONTROL 測定]](./measure.md) | 最初の起動時のデータが含まれます。 このプロパティは、インストール後の最初の起動時にトリガーされます。 |
| `installs` | [[!UICONTROL 測定]](./measure.md) | 特定のインストールイベントが使用可能な場合に、デバイスにアプリケーションをインストールしたことを記録します。 |
| `launches` | [[!UICONTROL 測定]](./measure.md) | アプリケーションの起動に関連付けられた値を示します。 これは、セッションのタイムアウトを超えた場合に、クラッシュ、インストールおよびバックグラウンドからの再開を含む、実行のたびにトリガーされます。 |
| `upgrades` | [[!UICONTROL 測定]](./measure.md) | 以前にインストールされたアプリケーションのアップグレードに関するデータが含まれます。 これは、アップグレード後の最初の起動時にトリガーされます。 |
| `id` | 文字列 | アプリケーションの一意の識別子。 |
| `name` | 文字列 |  アプリケーションの名前 |
| `userPerspective` | 文字列 | イベントが発生した時点でのユーザーとアプリまたはブランドの間の視点または物理的な関係。 アプリに関するユーザーの視点を理解すると、セッションを正確に生成できます。これは、`background`と`detached`のイベントを「アクティブ」セッションの一部として含めたくない場合が多いからです。 このプロパティの値は、次に示す列挙値の1つと等しくなければなりません。 <li> `foreground`:ユーザーとアプリは、直接やり取りを行います。 </li> <li> `background`:アプリとユーザーは間接的に相互作用します。例えば、画面がロックされている間や、別のアプリがフォアグラウンドで使用されている間に、アプリが値を測定して更新する場合があります。  </li> <li> `detached`:「デタッチ」は、イベントがアプリに関連付けられていたが、電子メールの送信や外部システムからのプッシュ通知など、アプリから直接アクセスされていないことを意味します。 |
| `version` | 文字列 | アプリケーションのバージョン。 |

データ型の詳細については、パブリックXDMリポジトリを参照してください。

* [入力済みの例](https://github.com/adobe/xdm/blob/master/components/datatypes/channels/application.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/channels/application.schema.json)
