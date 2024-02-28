---
title: Transport Layer Security(TLS) 情報
description: TLS バージョンおよび暗号に関する情報
source-git-commit: 35ee2aca2b92cb8abe1fc69ad6cbc66b0e241e89
workflow-type: tm+mt
source-wordcount: '209'
ht-degree: 24%

---

# Transport Layer Security(TLS) 情報

>[!NOTE]
>
>Adobe Experience Platform Launch は、Adobe Experience Platform のデータ収集テクノロジースイートとしてリブランドされています。 その結果、製品ドキュメント全体でいくつかの用語が変更されました。用語の変更の統合参照については、 [用語の更新](../../term-updates.md) 文書。

Transport Layer Security(TLS) は、インターネットを介してアプリケーション間で送信されるデータに対してエンドツーエンドのセキュリティを提供する暗号化プロトコルです。 TLS の詳細については、 [TLS の基本](https://www.internetsociety.org/deploy360/tls/basics/) ドキュメント。

Adobe Experience Platform のタグは、Web サイトにスクリプトを動的に読み込むように設計されたタグ管理システムです。TLS は、Adobe・ホスト間の通信を保護 `assets.adobedtm.com` および Web サイトに書き込まれます。

複数の TLS バージョンが利用可能で、様々な暗号をサポートしています。 一部のバージョンと暗号が、他のバージョンと同じでも、他のバージョンよりも安全でもないと考えられる場合もあります。

## サポートされる TLS バージョンと暗号

「Adobeホスト」オプションは現在、以下の TLS バージョンと暗号をサポートしています。

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

次の場合、 [自己ホスト型](../publishing/hosts/self-hosting-libraries.md) ライブラリの場合、サポートされる TLS バージョンは、独自のホスティングサービスによって決定されます。

## TLS 暗号は 2024 年 5 月 1 日に削除予定

```
PORT    STATE SERVICE
443/tcp open  https
| ssl-enum-ciphers:
|   TLSv1.2:
|     ciphers:
|       TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384 (secp256r1) - A
|       TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256 (secp256r1) - A
|       TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384 (secp256r1) - A
|       TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256 (secp256r1) - A
|       TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA (secp256r1) - A
|       TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA (secp256r1) - A
|       TLS_RSA_WITH_AES_256_GCM_SHA384 (rsa 2048) - A
|       TLS_RSA_WITH_AES_128_GCM_SHA256 (rsa 2048) - A
|       TLS_RSA_WITH_AES_256_CBC_SHA256 (rsa 2048) - A
|       TLS_RSA_WITH_AES_128_CBC_SHA256 (rsa 2048) - A
|       TLS_RSA_WITH_AES_256_CBC_SHA (rsa 2048) - A
|       TLS_RSA_WITH_AES_128_CBC_SHA (rsa 2048) - A
|     compressors:
|       NULL
|     cipher preference: server
|   TLSv1.3:
|     ciphers:
|       TLS_AKE_WITH_AES_128_CCM_8_SHA256 (secp256r1) - A
|       TLS_AKE_WITH_AES_128_CCM_SHA256 (secp256r1) - A
|     cipher preference: client
|_  least strength: A
```
