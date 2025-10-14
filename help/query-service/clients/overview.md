---
keywords: Experience Platform；ホーム；人気のトピック；Query service;Query Service；接続；クエリサービスへの接続；aqua Data Studio;Aqua Data Studio;Looker;looker;Postico;postico;Power BI;power bi;psql;rstudio;PSQL;RStudio;Tableau;
solution: Experience Platform
title: クエリーサービスにクライアントを接続
description: このドキュメントでは、様々なデスクトップクライアントアプリケーションからクエリサービスに接続する方法と、それらの接続を検証する方法について説明します。
exl-id: 2ba20179-5adb-4259-a120-231a40e78054
source-git-commit: 26f0725f0f239707bd719ed46929648f8d557155
workflow-type: tm+mt
source-wordcount: '269'
ht-degree: 5%

---

# [!DNL Query Service] へのクライアントの接続

この節では、様々なデスクトップクライアントアプリケーションから [!DNL Query Service] に接続する方法と、それらの接続を検証する方法について説明します。 [!DNL Query Service] は [!DNL PostgreSQL] プロトコルを使用するので、この節の手順では、[!DNL PostgreSQL] ツールとドライバーを使用してクエリを接続し、書き込む方法について説明します。

>[!IMPORTANT]
>
>Query Service Interactive Postgres API の実稼動環境の TLS/SSL 証明書は、2024 年 1 月 24 日水曜日（PT）に更新されました。<br> これは年間要件ですが、この機会に、Adobeの TLS/SSL 証明書プロバイダーが証明書階層を更新したので、チェーンのルート証明書も変更されました。 これは、特定の Postgres クライアントで、その認証機関のリストにルート証明書がない場合に影響を与える可能性があります。 例えば、PSQL CLI クライアントでは、ルート証明書を明示的なファイル `~/postgresql/root.crt` に追加する必要がある場合があります。そうしないと、エラーが発生する可能性があります。 たとえば、`psql: error: SSL error: certificate verify failed` のように設定します。この問題について詳しくは、[&#x200B; 公式の PostgreSQL ドキュメント &#x200B;](https://www.postgresql.org/docs/current/libpq-ssl.html#LIBQ-SSL-CERTIFICATES) を参照してください。<br> 追加するルート証明書は、[https://cacerts.digicert.com/DigiCertGlobalRootG2.crt.pemからダウンロードできます &#x200B;](https://cacerts.digicert.com/DigiCertGlobalRootG2.crt.pem)。

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
>Power BIユーザーと Tableau ユーザーは、「クエリサービスの資格情報」タブからCustomer Journey Analyticsを BI ツールに接続できます。 [BI ツールをCustomer Journey Analyticsに接続 &#x200B;](../ui/credentials.md#connect-to-customer-journey-analytics) する手順については、資格情報ドキュメントを参照してください。
