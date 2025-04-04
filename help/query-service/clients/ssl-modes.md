---
keywords: Experience Platform；ホーム；人気のトピック；Query Service;Query Service；接続；Query Service への接続；SSL;ssl;sslmode;
title: クエリサービス SSL オプション
description: Adobe Experience Platform クエリサービスへのサードパーティ接続での SSL サポートと、完全検証 SSL モードを使用した接続方法について説明します。
exl-id: 41b0a71f-165e-49a2-8a7d-d809f5f683ae
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '1011'
ht-degree: 1%

---

# [!DNL Query Service] SSL オプション

セキュリティを強化するために、Adobe Experience Platform [!DNL Query Service] では、SSL 接続のネイティブサポートを提供して、クライアントとサーバーの通信を暗号化します。 このドキュメントでは、[!DNL Query Service] へのサードパーティクライアント接続に使用できる SSL オプションと、`verify-full` の SSL パラメーター値を使用して接続する方法について説明します。

## 前提条件

このドキュメントは、Experience Platform データで使用するサードパーティのデスクトップクライアントアプリケーションを既にダウンロードしていることを前提としています。 サードパーティのクライアントと接続する際に SSL セキュリティを組み込む方法については、それぞれの接続ガイドのドキュメントを参照してください。 サポートされているすべてのクライアント [!DNL Query Service] リストについては、[ クライアント接続の概要 ](./overview.md) を参照してください。

## 使用可能な SSL オプション {#available-ssl-options}

Experience Platformでは、データセキュリティのニーズに合わせ、暗号化と鍵交換の処理オーバーヘッドのバランスを取るために、様々な SSL オプションをサポートしています。

`sslmode` パラメーターの値が異なれば、保護レベルも異なります。 SSL 証明書を使用してデータを暗号化することで、「中間者」（MITM）攻撃、盗聴、なりすましを防ぐのに役立ちます。 次の表に、使用可能な様々な SSL モードの分類と、提供される保護レベルを示します。

>[!NOTE]
>
> 必要なデータ保護コンプライアンスにより、SSL 値 `disable` はAdobe Experience Platformでサポートされていません。

| sslmode | 盗聴保護 | MITM 保護 | 説明 |
|---|---|---|---|
| `allow` | ○ | × | 暗号化はすべての通信で必要です。 ネットワークは、正しいサーバーに接続するために信頼されています。 |
| `prefer` | ○ | × | 暗号化はすべての通信で必要です。 ネットワークは、正しいサーバーに接続するために信頼されています。 |
| `require` | ○ | × | 暗号化はすべての通信で必要です。 ネットワークは、正しいサーバーに接続するために信頼されています。 サーバー SSL 証明書の検証は不要です。 |
| `verify-ca` | ○ | CA-policy に依存 | 暗号化はすべての通信で必要です。 データを共有するには、サーバーの検証が必要です。 それには、[!DNL PostgreSQL] ホームディレクトリにルート証明書を設定する必要があります。 [ 詳細は以下のとおりです ](#instructions) |
| `verify-full` | ○ | ○ | 暗号化はすべての通信で必要です。 データを共有するには、サーバーの検証が必要です。 それには、[!DNL PostgreSQL] ホームディレクトリにルート証明書を設定する必要があります。 [ 詳細は以下のとおりです ](#instructions)。 |

>[!NOTE]
>
>`verify-ca` と `verify-full` の違いは、ルート証明機関（CA）のポリシーによって異なります。 アプリケーション用にプライベート証明書を発行する独自のローカル CA を作成している場合は、`verify-ca` を使用すると十分な保護が得られることがよくあります。 パブリック CA を使用する場合、`verify-ca` は、他のユーザーが CA に登録している可能性のあるサーバーへの接続を許可します。 `verify-full` は、常にパブリックルート CA で使用する必要があります。

Experience Platform データベースへのサードパーティ接続を確立する場合、少なくとも `sslmode=require` を使用して、移動中のデータの安全な接続を確保することをお勧めします。 セキュリティが重視されるほとんどの環境では、`verify-full` SSL モードを使用することをお勧めします。

## サーバー検証用のルート証明書を設定します {#root-certificate}

>[!IMPORTANT]
>
>Query Service Interactive Postgres API の実稼動環境の TLS/SSL 証明書は、2024 年 1 月 24 日水曜日（PT）に更新されました。<br> これは年間要件ですが、この機会に、Adobeの TLS/SSL 証明書プロバイダーが証明書階層を更新したので、チェーンのルート証明書も変更されました。 これは、特定の Postgres クライアントで、その認証機関のリストにルート証明書がない場合に影響を与える可能性があります。 例えば、PSQL CLI クライアントでは、ルート証明書を明示的なファイル `~/postgresql/root.crt` に追加する必要がある場合があります。そうしないと、エラーが発生する可能性があります。 たとえば、`psql: error: SSL error: certificate verify failed` のように設定します。この問題について詳しくは、[ 公式の PostgreSQL ドキュメント ](https://www.postgresql.org/docs/current/libpq-ssl.html#LIBQ-SSL-CERTIFICATES) を参照してください。<br> 追加するルート証明書は、[https://cacerts.digicert.com/DigiCertGlobalRootG2.crt.pemからダウンロードできます ](https://cacerts.digicert.com/DigiCertGlobalRootG2.crt.pem)。

安全な接続を確保するには、接続を行う前に、クライアントとサーバーの両方で SSL 使用を設定する必要があります。 SSL がサーバー上でのみ設定されている場合、クライアントは、サーバーが高いセキュリティを必要とすることが確立される前に、パスワードなどの機密情報を送信する可能性があります。

デフォルトでは、[!DNL PostgreSQL] はサーバー証明書の検証を実行しません。 サーバーの ID を検証し、（SSL `verify-full` モードの一部として）機密データが送信される前に安全な接続を確保するには、ローカル マシン（`root.crt`）にルート（自己署名）証明書を、そしてルート証明書によって署名されたリーフ証明書を、サーバーに配置する必要があります。

`sslmode` パラメータが `verify-full` に設定されている場合、libpq はクライアントに格納されているルート証明書までの証明書チェーンをチェックして、サーバーの信頼性を検証します。 次に、ホスト名がサーバー証明書に保存されている名前と一致することを確認します。

サーバー証明書の検証を許可するには、ホーム ディレクトリの [!DNL PostgreSQL] ファイルに 1 つ以上のルート証明書（`root.crt`）を配置する必要があります。 ファイルパスは `~/.postgresql/root.crt` のようになります。

## サードパーティ [!DNL Query Service] 接続で使用する完全検証 SSL モードを有効にする {#instructions}

`sslmode=require` よりも厳しいセキュリティ制御が必要な場合は、ハイライト表示された手順に従って、SSL モードを使用してサードパーティクライアントを [!DNL Query Service] に接続 `verify-full` きます。

>[!NOTE]
>
>SSL 証明書を取得するために使用できるオプションは多数あります。 不正な証明書の増加傾向により、DigiCert は、高保証 TLS/SSL、PKI、IoT、署名ソリューションの信頼できるグローバルプロバイダーであるため、このガイドで使用されています。

1. [ 使用可能な DigiCert ルート証明書のリスト ](https://www.digicert.com/kb/digicert-root-certificates.htm) に移動します。
1. 使用可能な証明書のリストから「[!DNL DigiCert Global Root G2]」を検索します。
1. 「[!DNL **PEM をダウンロード**]」を選択して、ファイルをローカルマシンにダウンロードします。
   ![ 「PEM をダウンロード」がハイライト表示された、使用可能な DigiCert ルート証明書のリスト ](../images/clients/ssl-modes/digicert.png)
1. セキュリティ証明書ファイルの名前を `root.crt` に変更します。
1. ファイルを [!DNL PostgreSQL] フォルダーにコピーします。 必要なファイルパスは、オペレーティングシステムによって異なります。 フォルダーがまだ存在しない場合は作成します。
   - macOSを使用している場合、パスは `/Users/<username>/.postgresql` になります
   - Windows を使用している場合、パスは `%appdata%\postgresql` です

>[!TIP]
>
>Windows オペレーティング・システム上で `%appdata%` ファイルの場所を見つけるには、⊞**Win + R** を押して、検索フィールドに `%appdata%` を入力します。

[!DNL DigiCert Global Root G2] CRT ファイルを [!DNL PostgreSQL] フォルダーで使用できるようになったら、`sslmode=verify-full` または `sslmode=verify-ca` オプションを使用して、[!DNL Query Service] に接続できます。

## 次の手順

このドキュメントでは、サードパーティクライアントを [!DNL Query Service] に接続するための使用可能な SSL オプションと、`verify-full` SSL オプションを有効にしてデータを移動しながら暗号化する方法について詳しく説明します。

まだ行っていない場合は、[ サードパーティのクライアントの接続  [!DNL Query Service]](./overview.md) に関するガイダンスに従ってください。
