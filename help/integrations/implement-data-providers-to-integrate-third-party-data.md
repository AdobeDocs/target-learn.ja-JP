---
title: データプロバイダーを実装してサードパーティデータを統合する方法
description: このチュートリアルでは、Adobe Targetのデータプロバイダー機能を使用してサードパーティのデータプロバイダーからデータを取得し、Target リクエストで渡す方法の実装の詳細と例を示します。
role: Developer
level: Experienced
topic: Personalization, Integrations
feature: Implementation, Integrations, APIs/SDKs
doc-type: technical video
kt: null
author: Daniel Wright
exl-id: fcf6d1a8-e2a7-41ce-9c1c-02985b7afb5a
source-git-commit: 342e02562b5296871638c1120114214df6115809
workflow-type: tm+mt
source-wordcount: '301'
ht-degree: 0%

---

# [!UICONTROL  データプロバイダー ] を実装して、サードパーティデータをAdobe Targetに統合する

Adobe Targetの [!UICONTROL  データプロバイダー ] 機能を使用してサードパーティのデータプロバイダーからデータを取得し、Target リクエストで渡す実装の詳細と例。

>[!NOTE]
>
>[!UICONTROL データ] プロバイダ `at.js` ーには 1.3 以降が必要です。

## データプロバイダーの基本コンポーネントの実装

>[!VIDEO](https://video.tv.adobe.com/v/22348/?quality=12)

`dataProvider` の基本的な構成要素と、適切な順序でコードを取得する方法の概要を簡単に説明します。\
ビデオで使用されているコードの実際の例は、次の場所にあります。
[https://target.enablementadobe.com/data-providers/simple.html](https://target.enablementadobe.com/data-providers/simple.html)

## サードパーティ API との統合

>[!VIDEO](https://video.tv.adobe.com/v/22345/)

より現実的な例です。天気 API の統合などです。\
ビデオで使用されているコードの実際の例は、次の場所にあります。
[https://target.enablementadobe.com/data-providers/3rdparty.html](https://target.enablementadobe.com/data-providers/3rdparty.html)

## 複数のプロバイダーとの統合

>[!VIDEO](https://video.tv.adobe.com/v/22346/)

複数のプロバイダーのデータをグローバルな [!DNL Target] リクエストに組み込む方法。\
ビデオで使用されているコードの実際の例は、次の場所にあります。
[https://target.enablementadobe.com/data-providers/combined.html](https://target.enablementadobe.com/data-providers/combined.html)

## ページ読み込みの影響を最小化

>[!VIDEO](https://video.tv.adobe.com/v/22347/)

データをセッションストレージオブジェクトに保存することで、ページ読み込み時間への影響を最小限に抑えます。 または、 `profile.` プレフィックスを使用して値をプロファイルパラメーターとして渡し、セッションの最初の [!DNL Target] リクエストで値を渡すこともできます。 ただし、リクエストごとに 50 個のプロファイルパラメーターを渡すことに制限されます。

ビデオで使用されているコードの実際の例は、次の場所にあります。[https://target.enablementadobe.com/data-providers/reducedCalls.html](https://target.enablementadobe.com/data-providers/reducedCalls.html)

## サポート資料

* [Adobe Targetでのデータプロバイダーの使用](use-data-providers-to-integrate-third-party-data.md)

* [データプロバイダーのドキュメント](https://experienceleague.adobe.com/docs/target/using/implement-target/client-side/at-js-implementation/functions-overview/targetgobalsettings.html?lang=en#data-providers)
