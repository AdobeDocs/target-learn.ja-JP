---
title: データプロバイダーを導入してサードパーティデータを統合する方法
description: このチュートリアルでは、Adobe Targetのデータプロバイダー機能を使用してサードパーティデータプロバイダーからデータを取得し、Target リクエストに渡す方法について詳しく説明します。
role: Developer
level: Experienced
topic: Personalization, Integrations
feature: Implementation, Integrations, APIs/SDKs
doc-type: technical video
kt: null
author: Daniel Wright
exl-id: fcf6d1a8-e2a7-41ce-9c1c-02985b7afb5a
TQID: https://experienceleague.adobe.com/Oh0ngUGA-ZfpPHnQnN0VRgUG1ta4yqg8Pfm8mhCZTN0
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2:
  - id: c93393a4-e558-47e1-992e-c91ed4d480ce
  - id: f7c7de77-382f-4f48-8b36-61a170f06d3d
subfeature_v2:
  - id: fd0ff162-b6d3-4a11-8aeb-e165a01c0f0a
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
  - id: e0eb8757-182f-49f3-94a4-1587d16f5094
source-git-commit: c0b4abf2d4ead4d58a8db6e8970857b7b50dbe5c
workflow-type: tm+mt
source-wordcount: 293
ht-degree: 0%

---

# [!UICONTROL Data Providers]を実装してサードパーティデータをAdobe Targetに統合

実装の詳細と、Adobe Targetの[!UICONTROL Data Providers]機能を使用してサードパーティのデータプロバイダーからデータを取得し、Target リクエストに渡す方法の例。

>[!NOTE]
>
>[!UICONTROL Data Providers]には`at.js` 1.3以降が必要です

## データプロバイダーの基本コンポーネントの実装

>[!VIDEO](https://video.tv.adobe.com/v/34061/?captions=jpn&quality=12)

`dataProvider`の基本的なコンポーネントの概要と、コードを正しい順序で取得する方法について説明します。\
ビデオで使用されているコードの実際の例は、次のとおりです。
[https://target.enablementadobe.com/data-providers/simple.html](https://target.enablementadobe.com/data-providers/simple.html)

## サードパーティ APIとの統合

>[!VIDEO](https://video.tv.adobe.com/v/34062?captions=jpn)

より現実的な例としては、weather APIの統合があります。\
ビデオで使用されているコードの実際の例は、次のとおりです。
[https://target.enablementadobe.com/data-providers/3rdparty.html](https://target.enablementadobe.com/data-providers/3rdparty.html)

## 複数のプロバイダーとの統合

>[!VIDEO](https://video.tv.adobe.com/v/36804?captions=jpn)

複数のプロバイダーからのデータをグローバル [!DNL Target] リクエストに組み込む方法。\
ビデオで使用されているコードの実際の例は、次のとおりです。
[https://target.enablementadobe.com/data-providers/combined.html](https://target.enablementadobe.com/data-providers/combined.html)

## ページ読み込み効果を最小化

>[!VIDEO](https://video.tv.adobe.com/v/36805?captions=jpn)

セッションストレージオブジェクトにデータを保存することで、ページ読み込み時間への影響を最小限に抑えることができます。 または、値を`profile.`接頭辞を使用してプロファイルパラメーターとして渡し、セッションの最初の[!DNL Target] リクエストでそれらを渡すだけです。 ただし、リクエストごとに50個のプロファイルパラメーターを渡すことに制限されます。

ビデオで使用されているコードの実際の例は、[https://target.enablementadobe.com/data-providers/reducedCalls.html](https://target.enablementadobe.com/data-providers/reducedCalls.html)にあります。

## サポートマテリアル

* [Adobe Targetでデータプロバイダーを使用](use-data-providers-to-integrate-third-party-data.md)
