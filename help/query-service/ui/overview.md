---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Adobe Experience PlatformクエリサービスのUIガイド
topic: guide
translation-type: tm+mt
source-git-commit: bfbf2074a9dcadd809de043d62f7d2ddaa7c7b31
workflow-type: tm+mt
source-wordcount: '584'
ht-degree: 2%

---


# [!DNL Query Service] ガイド

このAdobe Experience Platformは、クエリの書き込みと実行、以前に実行された表示、およびIMS組織内のユーザーによって保存されたクエリへのアクセスクエリに使用できるユーザーインターフェイスを提供します。 [!DNL Query Service] [Adobe Experience Platform内のUIにアクセスするには][platform-ui]、左側のナビゲーションで **[!UICONTROL クエリ]** （「オプション」）を選択します。

## [!DNL Query Editor]

を [!DNL Query Editor] 使用すると、外部クライアントを使用せずに、クエリを書き込みおよび実行できます。 「 **[!UICONTROL クエリを作成]** 」をクリックしてクエリを開き、新しいを作成し [!DNL Query Editor] ます。 「ログ [!DNL Query Editor] 」タブまたは「 *[!UICONTROL 参照]* 」タブからクエリを選択して、アクセスすることもで ** きます。 以前に実行または保存したクエリを選択すると、が開き、選択したクエリのSQLが表示され [!DNL Query Editor] ます。

![画像](../images/queries/ui-overview/overview.png)

[!DNL Query Editor] クエリの入力を開始できる編集領域を提供します。 入力すると、SQL予約語、テーブル、およびテーブル内のフィールド名がエディタによって自動入力されます。 クエリの書き込みが完了したら、 **再生** ボタンをクリックしてクエリを実行します。 エディタの下の *[!UICONTROL コンソール]* (Console [!DNL Query Service] )タブに、クエリがいつ返されたかを示す、現在何を行っているかが表示されます。 コンソールの横にある *[!UICONTROL 「Result]* 」タブには、クエリの結果が表示されます。 を使用する方法の詳細については、『 [クエリエディタ][query-editor] 』ガイドを参照してくだ [!DNL Query Editor]さい。

![画像](../images/queries/ui-overview/query-editor.png)

## 参照

「 *[!UICONTROL 参照]* 」タブには、組織内のユーザーが保存したクエリが表示されます。 これらをクエリプロジェクトと考えると便利です。ここで保存されたクエリは、まだ建設中の可能性があります。 「 *[!UICONTROL 参照]* 」タブに表示されるクエリは、以前にが実行した場合は、「 *[!UICONTROL ログ]* 」タブに実行クエリとしても表示され [!DNL Query Service]ます。

![画像](../images/queries/ui-overview/browse.png)

| 列 | 説明 |
| --- | --- |
| 名前 | ユーザーが作成したクエリ名。 名前をクリックすると、のクエリが開き [!DNL Query Editor]ます。 検索バーを使用して、クエリの名前で検索することもできます。 検索では大文字と小文字が区別されます。 |
| SQL | SQLクエリの最初の数文字。 コードの上にカーソルを置くと、完全なクエリが表示されます。 |
| 変更者 | クエリを最後に変更したユーザー。 にアクセスできる組織内のユーザーは誰でも、クエリを変更 [!DNL Query Service] できます。 |
| 最終変更日 | ブラウザーのタイムゾーンでの、クエリーに対する最後の変更の日時。 |

## ログ

「 *[!UICONTROL ログ]* 」タブには、以前に実行されたクエリのリストが表示されます。 デフォルトでは、ログリストはクエリを逆年代順にします。

![画像](../images/queries/ui-overview/log.png)

| 列 | 説明 |
| --- | --- |
| **[!UICONTROL 名前]** | クエリ名。SQLクエリの最初の数文字で構成されます。 名前をクリックすると、が開き [!DNL Query Editor]、クエリを編集できます。 検索バーを使用して、クエリの名前で検索できます。 検索では大文字と小文字が区別されます。 |
| **[!UICONTROL 作成者]** | クエリの作成者の名前。 |
| **[!UICONTROL クライアント]** | クエリに使用するクライアント。 |
| **[!UICONTROL データセット]** | クエリが使用する入力データセット。 データセットをクリックして、入力データセットの詳細画面に移動します。 |
| **[!UICONTROL ステータス]** | クエリの現在のステータス。 |
| **[!UICONTROL 前回の実行]** | クエリが最後に実行された日。 この列の上にある矢印をクリックして、リストを昇順または降順で並べ替えることができます。 |
| **[!UICONTROL 実行時]** | クエリの実行に要した時間。 |

## 資格情報

「 *[!UICONTROL 資格情報]* 」タブには、資格情報が表示され [!DNL Postgres] ます。 任意のフィールドの横にある **[!UICONTROL コピー]** アイコンをクリックして、その内容をキーボードバッファに保存します。 これらの資格情報を使用して外部クライアントと接続する方法の詳細については、『 [connect with clients』ガイドを参照してください][connect-clients]。

![画像](../images/queries/ui-overview/credentials.png)

## 次の手順

の [!DNL Query Service] ユーザインターフェイスに慣れたので、独自のクエリプロジェクトを作成する開始 [!DNL Platform][!DNL Query Editor] にアクセスして、組織内の他のユーザと共有できます。 でのクエリのオーサリングと実行について詳し [!DNL Query Editor]くは、『 [クエリエディタユーザガイド][query-editor]』を参照してください。

[platform-ui]: https://platform.adobe.com
[query-editor]: user-guide.md
[connect-clients]: ../clients/overview.md
