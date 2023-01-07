---
keywords: Experience Platform；ホーム；人気の高いトピック；PSQL;psqlconnect to query service；クエリサービス；クエリサービス；
solution: Experience Platform
title: PSQL をクエリサービスに接続
description: PSQL は、PostgreSQL をマシンにインストールする際に提供されるコマンドラインインターフェイスです。 次の手順に従ってインストールできます。
exl-id: ceb07128-409e-42be-8143-0cf681d435de
source-git-commit: 58eadaaf461ecd9598f3f508fab0c192cf058916
workflow-type: tm+mt
source-wordcount: '294'
ht-degree: 11%

---

# PSQL をクエリサービスに接続

PSQL は、をインストールする際にインストールされるコマンドラインインターフェイスです [!DNL PostgreSQL] を設定します。 このドキュメントでは、PSQL をAdobe Experience Platformに接続する手順を説明します [!DNL Query Service].

>[!NOTE]
>
> このガイドは、ユーザーが既に [!DNL PSQL] そしてその使い方に精通している 詳細情報： [!DNL PSQL] は [公式 [!DNL PSQL] ドキュメント](https://www.postgresql.org/docs/current/app-psql.html).

コンピューターに PSQL をインストールした後、PSQL をクエリサービスに接続する準備が整いました。 に戻る [!DNL Platform] UI を選択し、 **[!UICONTROL クエリ]**&#x200B;に続いて **[!UICONTROL 資格情報]**.

以下 **[!UICONTROL PSQL コマンド]** セクションで、 **[!UICONTROL クリップボードにコピー]** アイコン (![コピーアイコン](../images/clients/psql/copy-icon.png)) をクリックして、コマンド文字列をコピーします。

![コピーアイコンがハイライトされた「クエリ」ダッシュボードの「資格情報」タブ](../images/clients/psql/connect-bi.png)

コマンド文字列をターミナルまたはコマンドラインウィンドウに貼り付け、 **入力** キーボードで

>[!IMPORTANT]
>
>PC の場合は、テキストエディタを使用してコマンド文字列の改行を削除し、文字列をコピーします。 バージョン 12.0 以降を使用している場合は、 `PGGSSENCMODE=disable` を接続文字列に追加します。 また、有効期限のない資格情報を使用している場合は、「パスワード」フィールドを、有効期限のない資格情報パスワードに置き換えます。 有効期限のない資格情報の詳細については、 [資格情報ガイド](../ui/credentials.md).

次のような結果が表示されます。

```shell
psql (10.5, server 0.1.0)
SSL connection (protocol: TLSv1.2, cipher: ECDHE-RSA-AES256-GCM-SHA384, bits: 256, compression: off)
Type "help" for help.
all=>
```

バージョン 10.5 以降が表示されない場合は、バージョン 10.5 以降をダウンロードする必要があります。

## 次の手順

これで、 [!DNL Query Service]を使用すると、PSQL を使用してクエリを記述できます。 クエリの書き込みと実行の方法について詳しくは、 [クエリの実行](../best-practices/writing-queries.md).
