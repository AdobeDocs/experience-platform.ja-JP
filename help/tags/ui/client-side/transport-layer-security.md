---
title: Transport Layer Security （TLS）情報
description: 使用される TLS バージョンおよび暗号に関する情報
exl-id: 04948cd8-6cf0-4159-a9d3-3130b97af106
source-git-commit: 236c5a11f40490fc7ee536358fb146027fe64545
workflow-type: tm+mt
source-wordcount: '201'
ht-degree: 25%

---

# Transport Layer Security （TLS）情報

>[!NOTE]
>
>Adobe Experience Platform Launch は、Adobe Experience Platform のデータ収集テクノロジースイートとしてリブランドされています。 その結果、製品ドキュメント全体でいくつかの用語が変更されました。用語の変更点の一覧については、[&#x200B; 用語の更新 &#x200B;](../../term-updates.md) ドキュメントを参照してください。

Transport Layer Security （TLS）は、インターネット経由でアプリケーション間で送信されるデータのエンドツーエンドのセキュリティを提供する暗号化プロトコルです。 TLS について詳しくは、[TLS の基本 &#x200B;](https://www.internetsociety.org/deploy360/tls/basics/) ドキュメントを参照してください。

Adobe Experience Platform のタグは、Web サイトにスクリプトを動的に読み込むように設計されたタグ管理システムです。TLS は、これらのスクリプトが読み込まれる際にAdobe ホスト `assets.adobedtm.com` と web サイト間の通信を保護します。

複数の TLS バージョンが使用可能で、サポートする暗号は多数あります。 すべてのバージョンと暗号が同じとは限らず、一部のバージョンは他のバージョンよりも安全であると見なされます。

## サポートされる TLS のバージョンと暗号

Adobe ホストオプションは、現在、次の TLS バージョンと暗号をサポートしています。

```
PORT    STATE SERVICE
443/tcp open  https
| ssl-enum-ciphers:
|   TLSv1.2:
|     ciphers:
|       TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384 (secp256r1) - A
|       TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256 (secp256r1) - A
|       TLS_ECDHE_ECDSA_WITH_CHACHA20_POLY1305_SHA256 (secp256r1) - A
|       TLS_ECDHE_RSA_WITH_CHACHA20_POLY1305_SHA256 (secp256r1) - A
|       TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384 (secp256r1) - A
|       TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256 (secp256r1) - A
|     compressors:
|       NULL
|     cipher preference: server
|   TLSv1.3:
|     ciphers:
|       TLS_AKE_WITH_AES_128_GCM_SHA256 (secp256r1) - A
|       TLS_AKE_WITH_AES_256_GCM_SHA384 (secp256r1) - A
|       TLS_AKE_WITH_CHACHA20_POLY1305_SHA256 (secp256r1) - A
|     cipher preference: client
|_  least strength: A
```

### 自己ホスト

ライブラリを [&#x200B; セルフホスティング &#x200B;](../publishing/hosts/self-hosting-libraries.md) している場合、サポートされる TLS バージョンは、独自のホスティングサービスによって決定されます。
