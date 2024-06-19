---
solution: Experience Platform
title: 同意および環境設定スキーマフィールドグループ
description: 同意および環境設定スキーマフィールドグループについて説明します。
exl-id: ec592102-a9d3-4cac-8b94-58296a138573
source-git-commit: b08c6cf12a38f79e019544dea91913a77bd6490a
workflow-type: tm+mt
source-wordcount: '933'
ht-degree: 0%

---

# [!UICONTROL 同意および環境設定] フィールドグループ

[!UICONTROL 同意および環境設定] は、の標準フィールドグループです [[!DNL XDM Individual Profile] クラス](../../classes/individual-profile.md) 個々の顧客の同意および好みに関する情報を取得します。

>[!NOTE]
>
>このフィールドグループはと互換性があるだけです [!DNL XDM Individual Profile]。に使用することはできません。 [!DNL XDM ExperienceEvent] スキーマ。 エクスペリエンスイベントスキーマに同意データと環境設定データを含める場合は、 [[!UICONTROL プライバシー、パーソナライゼーションおよびマーケティング環境設定に関する同意] データタイプ](../../data-types/consents.md) を使用してスキーマに [カスタムフィールドグループ](../../ui/resources/field-groups.md#create) その代わり。

## フィールドグループ構造 {#structure}

この [!UICONTROL 同意および環境設定] フィールドグループは、オブジェクトタイプの単一フィールドを提供します。 `consents`同意および環境設定に関する情報を取り込みます。 このフィールドは、 [[!UICONTROL プライバシー、パーソナライゼーションおよびマーケティング環境設定に関する同意] データタイプ](../../data-types/consents.md)、を削除しています `adID` フィールドと追加 `idSpecific` マップフィールド。

![](../../images/field-groups/consent.png)

>[!TIP]
>
>のガイドを参照してください [xdm リソースの調査](../../ui/explore.md) xdm リソースを検索し、Platform UI でその構造を検査する手順については、を参照してください。

次の JSON は、 [!UICONTROL 同意および環境設定] フィールドグループは処理できます。 フィールドグループが提供するほとんどのフィールドの使用方法については、のガイドを参照してください。 [同意および環境設定データタイプ](../../data-types/consents.md). 以下のサブセクションでは、フィールドグループがデータタイプに追加する一意の属性に焦点を当てています。

```json
{
  "consents": {
    "collect": {
      "val": "VI"
    },
    "share": {
      "val": "y"
    },
    "personalize": {
      "content": {
        "val": "y"
      }
    },
    "marketing": {
      "preferred": "email",
      "any": {
        "val": "y"
      },
      "email": {
        "val": "y"
      }
    },
    "idSpecific": {
      "ECID": {
        "37784337855396895622558625508046772577": {
          "adID": {
            "val": "n",
          },
          "share": {
            "val": "n"
          },
          "marketing": {
            "push": {
              "val": "n",
              "time": "2020-09-30T01:02:33+00:00",
              "reason": "not relevant"
            }
          }
        }
      },
      "email": {
        "john@xyz.com": {
          "marketing": {
            "email": {
              "val": "y"
            }
          }
        }
      }
    },
    "metadata": {
      "time": "2019-01-01T15:52:25+00:00"
    }
  }
}
```

>[!TIP]
>
>顧客の同意データと環境設定データのマッピング方法を視覚化するために、Experience Platformで定義した XDM スキーマのサンプル JSON データを生成できます。 詳しくは、次のドキュメントを参照してください。
>
>* [UI でのサンプルデータの生成](../../ui/sample.md)
>* [API でのサンプルデータの生成](../../api/sample-data.md)

### `idSpecific`

`idSpecific` 特定の同意または環境設定が顧客に普遍的に適用されるのではなく、単一のデバイスまたは ID に制限されている場合に使用できます。 例えば、ある顧客が、あるアドレスへのメールの受信をオプトアウトしながら、別のアドレスへのメールを許可できる可能性があります。

>[!IMPORTANT]
>
>チャネルレベルの同意および環境設定（で提供される同意や環境設定など） `consents` 外に `idSpecific`）は、そのチャネル内のすべての ID に適用されます。 したがって、すべてのチャネルレベルの同意と環境設定は、同等の ID 固有の設定またはデバイス固有の設定に従うかどうかに直接影響します。
>
>* お客様がチャネルレベルでオプトアウトしている場合、同等の同意または環境設定はにあります。 `idSpecific` は無視されます。
>* チャネルレベルの同意または環境設定が設定されていない場合、または顧客がオプトインした場合、同等の同意または環境設定がで表示されます `idSpecific` 光栄です。

内の各キー `idSpecific` オブジェクトは、Adobe Experience Platform ID サービスによって認識される特定の ID 名前空間を表します。 独自のカスタム名前空間を定義して様々な識別子を分類できますが、ID サービスが提供する標準の名前空間の 1 つを使用して、リアルタイム顧客プロファイルのストレージサイズを小さくすることをお勧めします。 ID 名前空間について詳しくは、 [id 名前空間の概要](../../../identity-service/features/namespaces.md) ID サービスに関するドキュメント

各名前空間オブジェクトのキーは、顧客が環境設定を指定した一意の ID 値を表します。 各 ID 値には、と同じ形式の同意および環境設定の完全なセットを含めることができます `consents`.

```json
"idSpecific": {
  "email": {
    "jdoe@example.com": {
      "marketing": {
        "email": {
          "val": "n"
        }
      }
    }
  },
  "ECID": {
    "37784337855396895622558625508046772577": {
      "collect": {
        "val": "y"
      },
      "adID": {
        "val": "n"
      },
      "marketing": {
        "push": {
          "val": "n"
        }
      }
    }
  }
}
```

内 `marketing` で提供されるオブジェクト `idSpecific` セクション、 `any` および `preferred` フィールドはサポートされていません。 これらのフィールドは、ユーザーレベルでのみ設定できます。 さらに、 `idSpecific` のマーケティング環境設定 `email`, `sms`、および `push` サポートしない `subscriptions` フィールド。

また、次の場合にのみ提供できる同意もあります `idSpecific` セクション： `adID`. このフィールドについては、次のサブセクションで説明します。

#### `adID`

この `adID` 同意は、広告主 ID （IDFA または GAID）を使用してこのデバイス上のアプリ間で顧客をリンクできるかどうかに対する顧客の同意を表します。 この値は、 `ECID` の ID 名前空間 `idSpecific` セクション。他の名前空間に設定したり、このフィールドグループのユーザーレベルで設定したりすることはできません。

```json
"idSpecific": {
  "ECID": {
    "37784337855396895622558625508046772577": {
      "collect": {
        "val": "y"
      },
      "adID": {
        "val": "n"
      },
      "marketing": {
        "push": {
          "val": "n"
        }
      }
    }
  }
}
```

>[!NOTE]
>
>この値は、必要に応じてAdobe Experience Platform Mobile SDK が自動的に設定するので、直接設定する必要はありません。

## フィールドグループを使用したデータの取り込み {#ingest}

を使用するには [!UICONTROL 同意および環境設定] フィールドグループ顧客から同意データを取り込むには、そのフィールドグループを含むスキーマに基づいてデータセットを作成する必要があります。

チュートリアルを参照してください。 [ui でのスキーマの作成](https://www.adobe.com/go/xdm-schema-editor-tutorial-en) フィールドグループをフィールドに割り当てる手順については、を参照してください。 を使用したフィールドを含むスキーマを作成したら、 [!UICONTROL 同意および環境設定] フィールドグループ。の節を参照してください。 [データセットの作成](../../../catalog/datasets/user-guide.md#create) データセットユーザーガイドでは、既存のスキーマを使用してデータセットを作成する手順に従います。

>[!IMPORTANT]
>
>に同意データを送信する場合 [!DNL Real-Time Customer Profile]を作成する必要があります。 [!DNL Profile]に基づいた、対応スキーマ [!DNL XDM Individual Profile] を含むクラス [!UICONTROL 同意および環境設定] フィールドグループ。 そのスキーマに基づいて作成するデータセットも、用に有効にする必要があります [!DNL Profile]. に関連する特定の手順については、上記にリンクされたチュートリアルを参照してください [!DNL Real-Time Customer Profile] スキーマおよびデータセットの要件。
>
>また、顧客プロファイルを正しく更新するには、最新の同意データと環境設定データを含むデータセットに優先順位を付けるように結合ポリシーが設定されていることを確認する必要があります。 概要を参照してください [結合ポリシー](../../../rtcdp/profile/merge-policies.md) を参照してください。

## 同意および環境設定の変更の処理

顧客が web サイトに対する同意や環境設定を変更した場合は、その変更を収集し、を使用して直ちに適用する必要があります [Adobe Experience Platform Web SDK](../../../web-sdk/commands/setconsent.md). お客様がデータ収集をオプトアウトした場合、すべてのデータ収集は直ちに停止する必要があります。 顧客がパーソナライゼーションをオプトアウトする場合、訪問する次のページにはパーソナライゼーションが存在しないはずです。

## 次の手順

このドキュメントでは、の構造と使用について説明しました [!UICONTROL 同意および環境設定] フィールドグループ。 フィールドグループで提供される他のフィールドについて詳しくは、のドキュメントを参照してください [[!UICONTROL プライバシー、パーソナライゼーションおよびマーケティング環境設定に関する同意] データタイプ](../../data-types/consents.md).
