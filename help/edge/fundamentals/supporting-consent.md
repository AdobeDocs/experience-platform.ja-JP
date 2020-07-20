---
title: 同意のサポート
seo-title: Adobe Experience Platform Web SDK：同意設定のサポート
description: Experience Platform Web SDK を使用して同意設定をサポートする方法について説明します
seo-description: Experience Platform Web SDK を使用して同意設定をサポートする方法について説明します
translation-type: tm+mt
source-git-commit: 7b07a974e29334cde2dee7027b9780a296db7b20
workflow-type: tm+mt
source-wordcount: '516'
ht-degree: 96%

---


# 同意の支援

ユーザーのプライバシーを尊重するため、SDK に対して特定の目的でユーザー固有のデータを使用することを許可する前に、ユーザーの同意を求めることができます。現在、SDK はユーザーに対し、あらゆる目的に対するオプトインまたはオプトアウトのみを許可していますが、アドビでは、将来的に、特定の目的でより詳細な制御を提供したいと考えています。

ユーザーがすべての目的をオプトインした場合、SDK は次のタスクを実行できます。

* アドビのサーバーとの間でデータを送信する。
* Cookie または Web ストレージ項目の読み取りと書き込み（ユーザーのオプトイン設定を保持する場合を除く）。

ユーザーがすべての目的をオプトアウトした場合、SDK は次のタスクを実行しません。

## 同意の設定

デフォルトでは、ユーザーはすべての目的に対してオプトインします。ユーザーがオプトインするまで SDK が上記のタスクを実行しないようにするには、次のように、SDK の設定時に `"defaultConsent": { "general": "pending" }` を渡します。

```javascript
alloy("configure", {
  "edgeConfigId": "ebebf826-a01f-4458-8cec-ef61de241c93",
  "imsOrgId": "ADB3LETTERSANDNUMBERS@AdobeOrg",
  "defaultConsent": { "general": "pending" }
});
```

一般的な目的のデフォルトの同意が「保留」に設定されている場合、ユーザーのオプトイン設定に依存するコマンド（`event` コマンドなど）を実行しようとすると、そのコマンドは SDK 内のキューに追加されます。これらのコマンドは、ユーザーのオプトイン設定を SDK に通知するまで処理されません。

この時点で、ユーザーインターフェイス内のどこかでユーザーにオプトインを求めることをお勧めします。ユーザーの環境設定を収集した後、これらの環境設定を SDK に伝えます。

## 同意設定の連絡

ユーザーがオプトインした場合は、次のように、`general` オプションを `in` に設定して `setConsent` コマンドを実行します。

```javascript
alloy("setConsent", {
    consent: [{ 
      standard: "Adobe",
      version: "1.0",
      value: { 
        general: "in" 
      }
    }]
});
```

ユーザーがオプトインしたので、SDK はその前にキューに追加されたすべてのコマンドを実行します。ユーザーのオプトインに依存する以降のコマンドは、キューに追加&#x200B;_されず_、即座に実行されます。

ユーザーがオプトアウトを選択した場合は、次のように、`general` オプションを `out` に設定して `setConsent` コマンドを実行します。

```javascript
alloy("setConsent", {
    consent: [{ 
      standard: "Adobe",
      version: "1.0",
      value: { 
        general: "out" 
      }
    }]
});
```

>[!NOTE]
>
>ユーザーがオプトアウトすると、SDK では `in` に対するユーザーの同意を設定できません。

ユーザーがオプトアウトを選択したので、以前にキューに追加されたコマンドから返された promise は拒否されます。以降のコマンドで、ユーザーのオプトインに依存するものは、同様に拒否された prormise を返します。エラーの処理や抑制の詳細については、[コマンドの実行](executing-commands.md)を参照してください。

>[!NOTE]
>
>現在、SDK は `general` 目的のみをサポートしています。アドビでは、様々な機能や製品に対応する、さらに堅牢な目的やカテゴリのセットを構築する予定ですが、現在の実装アプリーチでは、すべてをオプトインするか、すべてをオプトインしないかのいずれかです。This only applies to the Adobe Experience Platform [!DNL Web SDK] and NOT other Adobe JavaScript libraries.

## 同意設定の保持

`setConsent` コマンドを使用してユーザー設定を SDK に伝えた後、SDK はユーザー設定を Cookie に保持します。次回ユーザーがブラウザーに We bサイトを読み込むと、SDK はこれらの永続的な環境設定を取得して使用します。`setConsent` コマンドを再び実行する必要はありません。ただし、ユーザーの環境設定に変更を加えた場合は伝える必要があります（変更はいつでも加えることができます）。