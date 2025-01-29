---
keywords: Experience Platform；ホーム；人気のトピック；スキーマ；XDM；フィールド；スキーマ；スキーマ；アプリケーション；データタイプ；データタイプ；データタイプ；
solution: Experience Platform
title: アプリケーション データ タイプ
description: アプリケーションエクスペリエンスデータモデル（XDM）データタイプについて説明します。
exl-id: ac7d6761-7b58-4e0d-85e7-6f157fb2eea5
source-git-commit: e028fbb82b37b3940b308a860c26f8b5f9884d3a
workflow-type: tm+mt
source-wordcount: '420'
ht-degree: 3%

---

# [!UICONTROL  アプリケーション ] データタイプ

[!UICONTROL  アプリケーション ] は、アプリケーションによって生成されるインタラクションに関連する詳細を記述する、標準のエクスペリエンスデータモデル（XDM）データタイプです。 アプリケーションとは、エンドユーザーがインストール、実行、閉じる、またはアンインストールできるモバイルアプリケーションやデスクトップアプリケーションなどのソフトウェアエクスペリエンスを指します。 このデータタイプのプロパティは、チャットボット、ブラウザーベースのプラグイン、アプリケーションに適用されないその他のエクスペリエンスなどのエージェントを記述するためのものではありません。

![ アプリケーション画像 ](../images/data-types/application.PNG){width=500}

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `applicationCloses` | [[!UICONTROL 測定]](./measure.md) | アプリケーションの終了に関する詳細を示します。 |
| `crashes` | [[!UICONTROL 測定]](./measure.md) | このプロパティは、アプリケーションが意図したとおりに終了しない場合にトリガーします。 |
| `featureUsages` | [[!UICONTROL 測定]](./measure.md) | 測定されるアプリケーション機能のアクティブ化からのデータを表します。 |
| `firstLaunches` | [[!UICONTROL 測定]](./measure.md) | 最初のローンチに関するデータが含まれます。 このプロパティは、インストール後の最初の起動時にトリガーされます。 |
| `installs` | [[!UICONTROL 測定]](./measure.md) | 特定のインストールイベントが使用可能な場合に、デバイスへのアプリケーションのインストールを記録します。 |
| `launches` | [[!UICONTROL 測定]](./measure.md) | アプリケーションのローンチに関連付けられた値を表します。 これは、クラッシュ、インストール、セッションタイムアウトを超えた場合のバックグラウンドからの再開など、実行のたびにトリガーされます。 |
| `upgrades` | [[!UICONTROL 測定]](./measure.md) | 以前にインストールされたアプリケーションのアップグレードに関するデータが含まれています。 これは、アップグレード後の最初の起動時にトリガーされます。 |
| `id` | 文字列 | アプリケーションの一意の ID。 |
| `name` | 文字列 | アプリケーションの名前。 |
| `userPerspective` | 文字列 | イベントが発生した時点でのユーザーとアプリまたはブランドの間の視点または物理的関係。 アプリに対するユーザーの視点を理解することで、`background` や `detached` のイベントを「アクティブ」セッションの一部に含めない場合がほとんどなので、セッションを正確に生成するのに役立ちます。 このプロパティの値は、以下に示す列挙値のいずれかに等しい必要があります。 <li> `foreground`：ユーザーとアプリは直接相互にやり取りしています。 </li> <li> `background`: アプリとユーザーは間接的に相互にやり取りしています。 例えば、アプリは、画面がロックされている間、または別のアプリがフォアグラウンドで使用されている間に、値を測定して更新できます。  </li> <li> `detached`: デタッチとは、イベントがアプリに関連しているものの、外部システムからのメールやプッシュ通知の送信など、アプリから直接送信されたのではないイベントを意味します。 |
| `version` | 文字列 | アプリケーションのバージョン。 |

{style="table-layout:auto"}

データタイプについて詳しくは、公開 XDM リポジトリを参照してください。

* [ 入力された例 ](https://github.com/adobe/xdm/blob/master/components/datatypes/channels/application.example.1.json)
* [ 完全なスキーマ ](https://github.com/adobe/xdm/blob/master/components/datatypes/channels/application.schema.json)
