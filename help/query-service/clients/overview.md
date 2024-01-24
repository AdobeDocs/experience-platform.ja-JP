---
keywords: Experience Platform；ホーム；人気のトピック；クエリサービス；クエリサービス；接続；クエリサービスへの接続；Aqua Data Studio;Looker;Postico;postico;Power BI;power bi;psql;PSQL;RStudio;Tableau;Tableau;
solution: Experience Platform
title: クエリサービスにクライアントを接続
description: このドキュメントでは、様々なデスクトップクライアントアプリケーションからクエリサービスに接続する方法と、それらの接続を検証する方法について説明します。
exl-id: 2ba20179-5adb-4259-a120-231a40e78054
source-git-commit: 778c65c6310ed4a627be0fd3ae076784cfc8495b
workflow-type: tm+mt
source-wordcount: '229'
ht-degree: 3%

---

# クライアントの接続先 [!DNL Query Service]

この節では、 [!DNL Query Service] 様々なデスクトップクライアントアプリケーションから、およびこれらの接続を検証する方法を参照してください。 [!DNL Query Service] は [!DNL PostgreSQL] プロトコルの場合、この節の説明は、 [!DNL PostgreSQL] 接続してクエリを書き込むためのツールとドライバ。

>[!IMPORTANT]
>
>クエリサービス Interactive Postgres API の実稼動環境の TLS/SSL 証明書が、2024 年 1 月 24 日（水）に更新されました。<br>これは年間要件ですが、Adobeの TLS/SSL 証明書プロバイダーが証明書階層を更新したので、この時点で、チェーン内のルート証明書も変更されました。 これは、特定の Postgres クライアントに影響を及ぼす可能性があります。 たとえば、PSQL CLI クライアントでは、ルート証明書を明示的なファイルに追加する必要がある場合があります。 `~/postgresql/root.crt`を含めない場合は、エラーが発生する可能性があります。 たとえば、`psql: error: SSL error: certificate verify failed` のように設定します。詳しくは、 [PostgreSQL の公式ドキュメント](https://www.postgresql.org/docs/current/libpq-ssl.html#LIBQ-SSL-CERTIFICATES) 詳しくは、この問題を参照してください。<br>追加するルート証明書は、からダウンロードできます。 [https://cacerts.digicert.com/DigiCertGlobalRootG2.crt.pem](https://cacerts.digicert.com/DigiCertGlobalRootG2.crt.pem).

手順は、次のクライアントに対して提供されます。

- [[!DNL Aqua Data Studio]](./aqua-data-studio.md)
- [[!DNL DbVisualizer]](./dbvisulaizer.md)
- [[!DNL Looker]](./looker.md)
- [[!DNL Postico (Mac)]](./postico.md)
- [[!DNL Power BI (PC)]](./power-bi.md)
- [[!DNL PSQL]](./psql.md)
- [[!DNL RStudio]](./rstudio.md)
- [[!DNL Tableau]](./tableau.md)
