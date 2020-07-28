---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Audience Manager のマッピングフィールド
topic: overview
translation-type: tm+mt
source-git-commit: bd9884a24c5301121f30090946ab24d9c394db1b
workflow-type: tm+mt
source-wordcount: '148'
ht-degree: 97%

---


# Audience Managerマッピングフィールド

次の表には、Adobe Audience Manager データのフィールド（リアルタイム、オンボード、プロファイルデータ）と対応する XDM フィールド間のマッピングが含まれています。

各 XDM フィールドについて詳しくは、『[XDM フィールドディクショナリ](../../../../xdm/schema/field-dictionary.md)』を参照してください。

## リアルタイムデータ

タイプ：リアルタイムデータ

| リアルタイムデータフィールド | XDM フィールド |
| --- | --- |
| `requestIds[]` | `ExperienceEvent.identityMap["ECID"]` |
| `requestIds[]` | `ExperienceEvent.endUserIds` - *endUserIds に存在する名前空間と最初の値のみ。* |
| `primaryDeviceId` | `ExperienceEvent.identityMap["CORE"]` |
| `primaryDeviceId` | ExperienceEvent.endUserIds - *endUserIds に存在する名前空間と最初の値のみ。* |
| `trait[] ` | `ExperienceEvent.segmentMemberships["AAMTraits"]` |
| `segments[]` | `ExperienceEvent.segmentMemberships["AAMSegments"]` |
| `mergeRules[]` | `ExperienceEvent.profileStitch[]` |
| `timestamps` | `ExperienceEvent.timeStamp` |
| `deviceMetadata` | `ExperienceEvent.device` <ul><li>primaryHardwareType → type</li><li>manufacturer → manufacturer</li><li>marketingName → model</li><li>modelNumber → model</li></ul> |
| `location` | `ExperienceEvent.placeContext.geo` <ul><li>d_country → countryCode</li><li>d_state → stateProvince</li><li>d_city → city</li><li>d_postal → postalCode</li><li>d_lat → latitude</li><li>d_longitude → longitude</li></ul> |
| `request_user_agent` | `ExperienceEvent.environment.browserDetails` <ul><li>h_user-agent → userAgent</li><li>h_accept-language → acceptLanguage</li></ul> |
| `client_ip` | `ExperienceEvent.environment` <ul><li>d_os_name → os name </li><li>d_os_version → os_version</li></ul> |
| `Signals` | ExperienceEvent.signals |

## 受信データ&#x200B;**（廃止）**

タイプ：ExperienceEvent

| 受信フィールド | XDM フィールド |
| --- | --- |
| `uuid` | `ExperienceEvent.identityMap[<ID Type>]` |
| `deviceIds` | `ExperienceEvent.identityMap["CORE"] And calculated ECIDs  ExperienceEvent.identityMap["ECID"]` |
| `signals` | `ExperienceEvent.signals` |
| `b_time` | `ExperienceEvent.timeStamp` |
| `overwrite` | `overwriteTraits` |

>[!NOTE]
>
> 受信フィールドは、今後のリリースで廃止される予定です。

## プロファイルデータ

タイプ：プロファイル XDM

| プロファイルフィールド | XDM フィールド |
| --- | --- |
| `ids` | `identityMap` |
| `smem` | `ExperienceEvent.segmentMemberships["AAMSegments"]` |
| `tmem` | `ExperienceEvent.segmentMemberships["AAMTraits"]` |
