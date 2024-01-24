---
keywords: Experience Platform；ホーム；人気の高いトピック；クエリサービス；クエリサービス；接続；クエリサービスへの接続；SSL;ssl;sslmode;
title: クエリサービスの SSL オプション
description: Adobe Experience Platform Query Service へのサードパーティ接続の SSL サポートと、verify-full SSL モードを使用した接続方法について説明します。
exl-id: 41b0a71f-165e-49a2-8a7d-d809f5f683ae
source-git-commit: 229ce98da8f1c97e421ef413826b0d23754d16df
workflow-type: tm+mt
source-wordcount: '1017'
ht-degree: 1%

---

# [!DNL Query Service] SSL オプション

セキュリティの向上のため、Adobe Experience Platform [!DNL Query Service] は、クライアント/サーバー通信を暗号化するための SSL 接続をネイティブでサポートします。 このドキュメントでは、に対するサードパーティクライアント接続で使用可能な SSL オプションについて説明します。 [!DNL Query Service] および `verify-full` SSL パラメーター値。

## 前提条件

このドキュメントでは、お使いの Platform データで使用するサードパーティのデスクトップクライアントアプリケーションを既にダウンロード済みであることを前提としています。 サードパーティのクライアントとの接続時に SSL セキュリティを組み込む方法に関する具体的な手順は、それぞれの接続ガイドのドキュメントに記載されています。 すべての [!DNL Query Service] サポートされるクライアント ( [クライアント接続の概要](./overview.md).

## 使用可能な SSL オプション {#available-ssl-options}

Platform は、データセキュリティのニーズに合わせて様々な SSL オプションをサポートし、暗号化と鍵交換の処理オーバーヘッドのバランスを取ります。

異なる `sslmode` パラメータ値は、異なるレベルの保護を提供します。 SSL 証明書を使用してデータを動作中に暗号化することで、「中間者」(MITM) 攻撃、盗聴、偽装を防ぐのに役立ちます。 次の表に、使用可能な様々な SSL モードの分類と保護レベルを示します。

>[!NOTE]
>
> SSL 値 `disable` は、必要なデータ保護コンプライアンスのため、Adobe Experience Platformでサポートされていません。

| sslmode | 盗聴防止 | MITM 保護 | 説明 |
|---|---|---|---|
| `allow` | 部分的 | × | セキュリティは優先事項ではありません。処理の速度と低いオーバーヘッドがより重要です。 このモードでは、サーバーが暗号化を要求した場合にのみ、暗号化がオプトされます。 |
| `prefer` | 部分的 | × | 暗号化は必要ありませんが、サーバーが暗号化をサポートしている場合は、通信が暗号化されます。 |
| `require` | ○ | × | すべての通信で暗号化が必要です。 ネットワークは、正しいサーバーに接続するために信頼されています。 サーバー SSL 証明書の検証は不要です。 |
| `verify-ca` | ○ | CA ポリシーに依存 | すべての通信で暗号化が必要です。 データを共有する前に、サーバーの検証が必要です。 この場合は、 [!DNL PostgreSQL] ホームディレクトリ。 [詳細は以下のとおりです](#instructions) |
| `verify-full` | ○ | ○ | すべての通信で暗号化が必要です。 データを共有する前に、サーバーの検証が必要です。 この場合は、 [!DNL PostgreSQL] ホームディレクトリ。 [詳細は以下のとおりです](#instructions). |

>[!NOTE]
>
>違いは `verify-ca` および `verify-full` ルート証明機関 (CA) のポリシーに依存します。 独自のローカル CA を作成し、アプリケーションに対してプライベート証明書を発行した場合は、 `verify-ca` 多くの場合、十分な保護を提供します。 パブリック CA を使用する場合、 `verify-ca` は、他のユーザーが CA に登録している可能性のあるサーバーへの接続を許可します。 `verify-full` は常にパブリックルート CA と共に使用する必要があります。

Platform データベースへのサードパーティ接続を確立する場合は、 `sslmode=require` 少なくとも、動作中のデータに対して安全な接続を確保するために必要です。 The `verify-full` ほとんどのセキュリティ上の問題を区別する環境では、SSL モードを使用することをお勧めします。

## サーバーの検証用にルート証明書を設定する {#root-certificate}

>[!IMPORTANT]
>
>クエリサービス Interactive Postgres API の実稼動環境の TLS/SSL 証明書が、2024 年 1 月 24 日（水）に更新されました。<br>これは年間要件ですが、Adobeの TLS/SSL 証明書プロバイダーが証明書階層を更新したので、この時点で、チェーン内のルート証明書も変更されました。 これは、特定の Postgres クライアントに影響を及ぼす可能性があります。 たとえば、PSQL CLI クライアントでは、ルート証明書を明示的なファイルに追加する必要がある場合があります。 `~/postgresql/root.crt`を含めない場合は、エラーが発生する可能性があります。 たとえば、`psql: error: SSL error: certificate verify failed` のように設定します。詳しくは、 [PostgreSQL の公式ドキュメント](https://www.postgresql.org/docs/current/libpq-ssl.html#LIBQ-SSL-CERTIFICATES) 詳しくは、この問題を参照してください。<br>追加するルート証明書は、からダウンロードできます。 [https://cacerts.digicert.com/DigiCertGlobalRootG2.crt.pem](https://cacerts.digicert.com/DigiCertGlobalRootG2.crt.pem).

安全な接続を確保するには、接続を確立する前に、クライアントとサーバーの両方で SSL 使用を設定する必要があります。 SSL がサーバー上でのみ設定されている場合、クライアントは、サーバーのセキュリティが高い必要があるという確立前に、パスワードなどの機密情報を送信する場合があります。

デフォルトでは、 [!DNL PostgreSQL] は、サーバー証明書の検証を実行しません。 （SSL の一部として）機密データが送信される前に、サーバーの ID を検証し、安全な接続を確保するため `verify-full` モード ) の場合は、ローカルマシン（自己署名済み）にルート（自己署名済み）証明書を配置する必要があります。`root.crt`) と、サーバー上のルート証明書によって署名されたリーフ証明書。

次の場合、 `sslmode` パラメーターがに設定されている `verify-full`を指定した場合、libpq は、クライアントに格納されているルート証明書まで証明書チェーンをチェックすることで、サーバーが信頼できることを検証します。 次に、ホスト名がサーバーの証明書に保存された名前と一致していることを確認します。

サーバー証明書の検証を許可するには、1 つ以上のルート証明書 (`root.crt`) を [!DNL PostgreSQL] ファイルをホームディレクトリに保存します。 ファイルパスは次のようになります。 `~/.postgresql/root.crt`.

## サードパーティで使用するための verify-full SSL モードの有効化 [!DNL Query Service] 接続 {#instructions}

より厳しいセキュリティ制御が必要な場合 `sslmode=require`を使用すると、ハイライト表示されている手順に従って、サードパーティのクライアントを [!DNL Query Service] using `verify-full` SSL モード。

>[!NOTE]
>
>SSL 証明書を取得するには、多くのオプションを使用できます。 不正な証明書の傾向が高まっているので、DigiCert は、TLS/SSL、PKI、IoT、および署名ソリューションの信頼できるグローバルプロバイダーであるため、このガイドで使用されています。

1. に移動します。 [使用可能な DigiCert ルート証明書のリスト](https://www.digicert.com/kb/digicert-root-certificates.htm)
1. 「」を検索します。[!DNL DigiCert Global Root G2]」をクリックします。
1. 選択 [!DNL **PEM をダウンロード**] をクリックして、ローカルマシンにファイルをダウンロードします。
   ![「Download PEM」がハイライト表示された、使用可能な DigiCert ルート証明書のリスト。](../images/clients/ssl-modes/digicert.png)
1. セキュリティ証明書ファイルの名前をに変更します。 `root.crt`.
1. ファイルを [!DNL PostgreSQL] フォルダー。 必要なファイルパスは、オペレーティングシステムによって異なります。 フォルダーが存在しない場合は、フォルダーを作成します。
   - macOSを使用している場合、パスは `/Users/<username>/.postgresql`
   - Windows を使用している場合、パスは `%appdata%\postgresql`

>[!TIP]
>
>次を検索： `%appdata%` Windows オペレーティングシステム上のファイルの場所を指定するには、⊞を押します。 **Win + R** と入力 `%appdata%` を検索フィールドに入力します。

次の期間の後に [!DNL DigiCert Global Root G2] CRT ファイルは、 [!DNL PostgreSQL] フォルダー、次の場所に接続できます： [!DNL Query Service] の使用 `sslmode=verify-full` または `sslmode=verify-ca` オプション。

## 次の手順

このドキュメントでは、サードパーティクライアントをに接続する際に使用できる SSL オプションについてより深く理解しています。 [!DNL Query Service]、および有効にする方法 `verify-full` SSL オプションを使用して、データを移行中に暗号化できます。

まだおこなっていない場合は、 [サードパーティクライアントの接続先 [!DNL Query Service]](./overview.md).
