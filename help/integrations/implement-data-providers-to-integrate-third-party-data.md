---
title: データプロバイダーを導入してサードパーティデータを統合する方法
description: このチュートリアルでは、Adobe Targetのデータプロバイダー機能を使用して、サードパーティのデータプロバイダーからデータを取得し、ターゲットリクエストに渡す方法の詳細と例を示します。
role: Developer
level: Experienced
topic: Personalization, Integrations
feature: Implementation, Integrations, APIs/SDKs
doc-type: technical video
kt: null
thumbnail: null
author: Daniel Wright
translation-type: tm+mt
source-git-commit: b89732fcca0be8bffc6e580e4ae0e62df3c3655d
workflow-type: tm+mt
source-wordcount: '308'
ht-degree: 0%

---


# [!UICONTROL データプロバイダー]を導入し、サードパーティのデータをAdobe Targetに統合

Adobe Targetの[!UICONTROL データプロバイダー]機能を使用して、サードパーティのデータプロバイダーからデータを取得し、ターゲットリクエストに渡す方法の実装の詳細と例です。

>[!NOTE]
>
>[!UICONTROL データ] プロバイダーには1.3以 `at.js` 上が必要

## データプロバイダーの基本コンポーネントの実装

>[!VIDEO](https://video.tv.adobe.com/v/22348/?quality=12)

`dataProvider`の基本的な要素の概要と、適切な順序でコードを取得する方法を簡単に説明します。\
このビデオで使用されているコードの実際の例は、次を参照してください。
[https://target.enablementadobe.com/data-providers/simple.html](https://target.enablementadobe.com/data-providers/simple.html)

## サードパーティAPIとの統合

>[!VIDEO](https://video.tv.adobe.com/v/22345/)

より現実的な例で、weather APIの統合を参照してください。\
このビデオで使用されているコードの実際の例は、次を参照してください。
[https://target.enablementadobe.com/data-providers/3rdparty.html](https://target.enablementadobe.com/data-providers/3rdparty.html)

## 複数のプロバイダーとの統合

>[!VIDEO](https://video.tv.adobe.com/v/22346/)

複数のプロバイダーのデータをグローバル[!DNL Target]リクエストに組み込む方法。\
このビデオで使用されているコードの実際の例は、次を参照してください。
[https://target.enablementadobe.com/data-providers/combined.html](https://target.enablementadobe.com/data-providers/combined.html)

## ページロードへの影響を最小化

>[!VIDEO](https://video.tv.adobe.com/v/22347/)

データをセッションストレージオブジェクトに格納することで、ページ読み込み時間への影響を最小限に抑えます。 または、`profile.`プレフィックスを使用して値をプロファイルパラメーターとして渡し、セッションの最初の[!DNL Target]リクエストに渡すだけです。 ただし、リクエスト1回につき50個のプロファイルパラメーターを渡すことに制限されます。

このビデオで使用されているコードの実際の例は、次を参照してください。[https://target.enablementadobe.com/data-providers/reducedCalls.html](https://target.enablementadobe.com/data-providers/reducedCalls.html)

## サポート資料

* [Adobe Targetでのデータプロバイダーの使用](use-data-providers-to-integrate-third-party-data.md)

* [データプロバイダードキュメント](https://docs.adobe.com/content/help/en/target/using/implement-target/client-side/functions-overview/targetgobalsettings.html#data-providers)