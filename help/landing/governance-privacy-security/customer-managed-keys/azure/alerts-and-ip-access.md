---
title: Azure CMK のアラートと IP許可リストの設定
description: Azure Key Vault でAdobeの静的 IP アドレスを許可リストする方法と、Experience Platform アラートが顧客管理キーアクセスの問題を検出し解決する方法について説明します。
source-git-commit: 719273798954b70460c77711ef5eda5f9d22cdb0
workflow-type: tm+mt
source-wordcount: '452'
ht-degree: 0%

---

# [!DNL Azure] CMK のアラートと IP許可リストの設定

透明性を高めるために、Adobeでは [ モニタリングサービス ](../../../../observability/alerts/ui.md){target="_blank"} を提供しています。このサービスは、Key Vault のアクセスステータスを確認し、問題が発生した場合にトリガーアラートを送信します。 これらのアラートは、迅速な対応とサービスの中断の回避に役立ちます。 このサービスを有効にするには、Adobeの静的 IP アドレスを許可リストに加えるします。

>[!IMPORTANT]
>
>パブリックネットワークアクセスを無効にしている場合、または [!DNL Azure] Key Vault で選択したネットワークのみを許可するように設定している場合は、Adobeの静的 IP アドレスを許可リストに追加する必要があります。 そうしないと、Experience Platform インスタンスに影響を与える可能性のあるアクセスの問題が通知されない場合があります。

## [!DNL Azure] Key Vault のAdobe静的 IP の許可リスト {#add-adobe-static-ip}

ネットワーク制限を維持しながら、これらのアラートを有効にするには、**[!DNL Azure Key Vault]** / **[!DNL Networking settings]** に移動します。 「**[!DNL Firewalls and virtual networks]**」タブで、「**[!DNL Allow public access from specific virtual networks and IP addresses]**」を選択します。

Adobe![[!DNL Azure] 静的 IP アドレスを追加する場所と、「次からアクセスを許可」オプションがハイライト表示された状態を示す、Key Vault ネットワーク設定画面 ](../../../images/governance-privacy-security/customer-managed-keys/key-vault-networking-settings.png)

### Adobeの静的 IP アドレス

>[!IMPORTANT]
>
>Adobeが提供する静的 IP アドレスは `20.88.123.53` です。

次に、「**[!DNL Firewall]**」セクションで「**[!DNL Add your current IP address]**」を選択し、Adobeの静的 IP アドレスに置き換えます。 すべての送信接続は実稼動環境として扱われるので、この静的 IP アドレスを許可リストに加えるして、制限されたネットワーク設定の Key Vault へのアクセスが中断されないようにする必要があります。

>[!NOTE]
>
>[!DNL Azure] Key Vault 設定で静的 IP アドレスを追加または更新した後、変更が有効になるまで最大 10 分間待ちます。 IP が追加されると、CMK アプリは Key Vault にアクセスして権限を検証します。

Adobeの静的 IP を許可リストに加えるすると、Experience Platformは、問題が発生した場合に Key Vault とトリガーアラートへのアクセスを監視できます。 これらのアラートは初期の警告を提供するので、サービスに影響が及ぶ前に対処できます。 次の節では、受信する可能性のあるアラートのタイプと応答方法について詳しく説明します。

## アラートの監視 {#monitor-alerts}

Platform アラートは、**[!UICONTROL キーアクセスの失敗]** や **[!UICONTROL キーの無効化]** など、キーアクセスを中断させる可能性のある問題を通知します。 これらのアラートを使用すると、静的 IP の削除やファイアウォールの設定の誤りなどの問題をすばやく特定できます。 アクセスを復元するには、[!DNL Azure] ファイアウォールの設定を確認し、必要な IP アドレスを再追加します。

<!-- For a complete list of alert types and recommended resolutions, see the [CMK alert resolution reference](../alert-resolution-reference.md). -->

Adobe I/O イベント通知を購読すると、モニタリングツールでリアルタイムアラートを受け取ることができます。 設定手順については、[Adobe I/O イベント通知の購読 ](../../../../observability/alerts/subscribe.md) を参照してください。 また、Experience Platform内でアラートを表示および管理する方法については、[ アラート UI ガイド ](../../../../observability/alerts/ui.md) を参照してください。

## 次の手順

これで、[!DNL Azure] Key Vault の IP許可リストに加えるとアラートモニタリングが設定されました。 [!DNL Azure] での顧客管理キーの設定を完了するには、次の設定ガイドに従います。

- [Key Vault [!DNL Azure]  設定 ](./azure-key-vault-config.md)
- [API を使用した CMK の設定 ](./api-set-up.md)
- [UI を使用した CMK の設定](./ui-set-up.md)
