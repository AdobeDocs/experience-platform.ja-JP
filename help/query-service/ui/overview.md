---
keywords: Experience Platform；ホーム；人気のあるトピック；クエリサービス；クエリサービス；クエリ;クエリエディタ；クエリエディタ；クエリエディタ；
solution: Experience Platform
title: クエリサービスUIガイド
topic-legacy: guide
description: Adobe Experience Platformクエリサービスは、クエリの書き込みと実行、表示が以前に実行したクエリ、IMS組織内でユーザーによって保存されたクエリへのアクセスに使用できるユーザーインターフェイスを提供します。
exl-id: 99ad25e4-0ca4-4bd1-b701-ab463197930b
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '629'
ht-degree: 53%

---

# [!DNL Query Service] ガイド

Adobe Experience Platform[!DNL Query Service]は、クエリの書き込みと実行、以前に実行した表示、IMS組織内のユーザーが保存したクエリへのアクセスに使用できるユーザーインターフェイスを提供します。 [Adobe Experience Platform][platform-ui] 内の UI にアクセスするには、左側のナビゲーションで「**[!UICONTROL クエリ]**」を選択します。

## [!DNL Query Editor]

[!DNL Query Editor]を使用すると、外部クライアントを使用せずにクエリを書き込み、実行できます。 **[!UICONTROL クエリを作成]**&#x200B;をクリックして[!DNL Query Editor]を開き、新しいクエリを作成します。 「**[!UICONTROL ログ]**」タブまたは「**[!UICONTROL 参照]**」タブからクエリを選択して、[!DNL Query Editor]にアクセスすることもできます。 以前に実行または保存したクエリを選択すると、[!DNL Query Editor]が開き、選択したクエリのSQLが表示されます。

![画像](../images/queries/ui-overview/overview.png)

[!DNL Query Editor] クエリの入力を開始できる編集領域を提供します。ユーザーが入力すると、エディターによって、テーブル内の SQL 予約語、テーブル、およびフィールド名が自動入力されます。クエリの書き込みが完了したら、「**再生**」ボタンをクリックしてクエリを実行します。 エディターの下の&#x200B;**[!UICONTROL コンソール]**&#x200B;タブには、[!DNL Query Service]が現在行っている処理が表示され、クエリがいつ返されたかが示されます。 コンソールの横にある「**[!UICONTROL 結果]**」タブには、クエリ結果が表示されます。[!DNL Query Editor]の使用方法の詳細については、『[クエリエディタガイド][query-editor]』を参照してください。

![画像](../images/queries/ui-overview/query-editor.png)

## 参照

「**[!UICONTROL 参照]**」タブには、組織のユーザーによって保存されたクエリが表示されます。これらをクエリプロジェクトと考えると便利です。ここで保存したクエリは、まだ作成中の可能性があります。**[!UICONTROL 「参照]**」タブに表示されるクエリも、[!DNL Query Service]によって以前に実行されている場合は、「**[!UICONTROL ログ]**」タブに実行クエリとして表示されます。

![画像](../images/queries/ui-overview/browse.png)

| 列 | 説明 |
| --- | --- |
| 名前 | ユーザーが作成したクエリ名。名前をクリックすると、[!DNL Query Editor]内のクエリが開きます。 検索バーを使用して、クエリの名前で検索することもできます。検索では大文字と小文字が区別されます。 |
| SQL | SQL クエリの最初の数文字。コードの上にカーソルを置くと、完全なクエリが表示されます。 |
| 変更者 | 最後にクエリを変更したユーザー。[!DNL Query Service]へのアクセス権を持つ組織内のユーザーは、クエリを変更できます。 |
| 最終変更日 | ブラウザーのタイムゾーンでの、クエリが最後に編集された日付と時間。 |

## ログ

「**[!UICONTROL ログ]**」タブには、以前に実行されたクエリのリストが表示されます。デフォルトでは、ログはクエリを逆年代順にリストします。

![画像](../images/queries/ui-overview/log.png)

| 列 | 説明 |
| --- | --- |
| **[!UICONTROL 名前]** | クエリ名。SQL クエリの最初の数文字で構成されます。名前をクリックすると[!DNL Query Editor]が開き、クエリを編集できます。 検索バーを使用して、クエリの名前で検索できます。検索では大文字と小文字が区別されます。 |
| **[!UICONTROL 作成者]** | クエリを作成した人物の名前。 |
| **[!UICONTROL クライアント]** | クエリに使用されるクライアント。 |
| **[!UICONTROL データセット]** | クエリが使用する入力データセット。データセットをクリックして、入力データセットの詳細画面に移動します。 |
| **[!UICONTROL ステータス]** | クエリの現在の状態。 |
| **[!UICONTROL 前回の実行]** | 最後にクエリが実行された日時。この列の上にある矢印をクリックして、リストを昇順または降順で並べ替えることができます。 |
| **[!UICONTROL 実行時]** | クエリの実行に要した時間。 |

## 資格情報

「**[!UICONTROL 資格情報]**」タブには、 の資格情報が表示されます。[!DNL Postgres]任意のフィールド 横にある「**[!UICONTROL コピー]**」アイコンをクリックして、その内容をキーボードバッファに保存します。これらの資格情報を使用して外部クライアントと接続する方法の詳細については、[クライアントとの接続ガイド][connect-clients]を参照してください。

![画像](../images/queries/ui-overview/credentials.png)

## 次の手順

[!DNL Platform]の[!DNL Query Service]ユーザーインターフェイスに慣れたので、[!DNL Query Editor]にアクセスして独自のクエリプロジェクトを作成し、組織内の他の開始と共有することができます。 [!DNL Query Editor]でのクエリのオーサリングと実行について詳しくは、[クエリエディターユーザーガイド][query-editor]を参照してください。

[platform-ui]: https://platform.adobe.com
[query-editor]: user-guide.md
[connect-clients]: ../clients/overview.md
