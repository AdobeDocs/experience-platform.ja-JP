---
keywords: Experience Platform；ホーム；人気のトピック；Query service;Query Service；接続；クエリサービスへの接続；aqua Data Studio;Aqua Data Studio;Looker;looker;Postico;postico;Power BI;power bi;psql;rstudio;PSQL;RStudio;Tableau;
solution: Experience Platform
title: クエリサービスへのクライアントの接続
description: このドキュメントでは、様々なデスクトップクライアントアプリケーションからクエリサービスに接続する方法と、それらの接続を検証する方法について説明します。
exl-id: 2ba20179-5adb-4259-a120-231a40e78054
source-git-commit: 26f0725f0f239707bd719ed46929648f8d557155
workflow-type: tm+mt
source-wordcount: '269'
ht-degree: 3%

---

# クライアントの接続先 [!DNL Query Service]

この節では、をに接続する方法について説明します。 [!DNL Query Service] 様々なデスクトップクライアントアプリケーションとそれらの接続の検証方法から。 [!DNL Query Service] はを使用します [!DNL PostgreSQL] プロトコルなので、この節の手順では、の使用方法について説明します。 [!DNL PostgreSQL] クエリを接続して書き込むためのツールとドライバー。

>[!IMPORTANT]
>
>Query Service Interactive Postgres API の実稼動環境の TLS/SSL 証明書は、2024 年 1 月 24 日水曜日（PT）に更新されました。<br>これは年間要件ですが、この機会に、Adobeの TLS/SSL 証明書プロバイダーが証明書階層を更新したので、チェーンのルート証明書も変更されました。 これは、特定の Postgres クライアントで、その認証機関のリストにルート証明書がない場合に影響を与える可能性があります。 たとえば、PSQL CLI クライアントでは、ルート証明書を明示的なファイルに追加する必要がある場合があります `~/postgresql/root.crt`。そうでない場合、エラーが発生する可能性があります。 たとえば、`psql: error: SSL error: certificate verify failed` のように設定します。を参照してください。 [postgreSQL の公式ドキュメント](https://www.postgresql.org/docs/current/libpq-ssl.html#LIBQ-SSL-CERTIFICATES) この問題に関する詳細情報。<br>追加するルート証明書は、次の場所からダウンロードできます。 [https://cacerts.digicert.com/DigiCertGlobalRootG2.crt.pem](https://cacerts.digicert.com/DigiCertGlobalRootG2.crt.pem).

手順は、次のクライアントに対して提供されます。

- [[!DNL Aqua Data Studio]](./aqua-data-studio.md)
- [[!DNL DbVisualizer]](./dbvisulaizer.md)
- [[!DNL Looker]](./looker.md)
- [[!DNL Postico (Mac)]](./postico.md)
- [[!DNL Power BI (PC)]](./power-bi.md)
- [[!DNL PSQL]](./psql.md)
- [[!DNL RStudio]](./rstudio.md)
- [[!DNL Tableau]](./tableau.md)

>[!IMPORTANT]
>
>Power BIユーザーと Tableau ユーザーは、「クエリサービスの資格情報」タブからCustomer Journey Analyticsを BI ツールに接続できます。 の方法については、資格情報ドキュメントを参照してください。 [bi ツールをCustomer Journey Analyticsに接続](../ui/credentials.md#connect-to-customer-journey-analytics).
