---
title: Adobe Recommendations API とは
description: このチュートリアルでは、Adobe Target Recommendations API を使用して、Recommendationsのカタログとカスタム条件の設定と管理を行う実践、および Delivery API を使用して Recommendations のコンテンツを取得する実践を、開発者に対して順を追って説明します。
role: Developer
level: Intermediate
topic: Personalization, Administration, Integrations, Development
feature: APIs/SDKs, Recommendations, Administration & Configuration, Overview
doc-type: tutorial
kt: 3815
author: Judy Kim
exl-id: 10f80056-fb71-4362-86bc-d161f596cb91
source-git-commit: 342e02562b5296871638c1120114214df6115809
workflow-type: tm+mt
source-wordcount: '359'
ht-degree: 3%

---

# Adobe Recommendations API の概要

[!DNL Recommendations] に関連する API には、以下をおこなうための [ 管理 API](https://experienceleague.adobe.com/docs/target/using/apis/api-overview.html?lang=en) が含まれています。

* レコメンデーション可能な商品またはコンテンツのカタログの管理
* [!DNL Recommendations] アルゴリズムとアクティビティの管理

Recommendationsで [!DNL Target] [delivery API](https://experienceleague.adobe.com/docs/target/using/apis/api-overview.html?lang=en) を使用すると、次のこともできます。

* JSON、HTML、または XML オブジェクトでレコメンデーションを取得して、Web、モバイル、電子メール、Internet of Things(IOT)、その他のチャネルで表示できるようにします。

## チュートリアルの説明

このチュートリアルでは、[!DNL Recommendations] API を使用して [!DNL Recommendations] カタログとカスタム条件の設定と管理を行う実践、および Delivery API を使用して Recommendations コンテンツを取得する実践を、開発者に対して順を追って説明します。 このチュートリアルを最後まで学習すると、次の内容を習得できます。

* Recommendations API を使用したエンティティの設定と管理
* Recommendations API を使用したカスタム条件の設定と管理
* Recommendationsを Delivery API と共に使用して、レコメンデーションの結果を非HTMLデバイスで使用する方法を理解する

## オーディエンス

このチュートリアルは、Target API またはRecommendations API を初めて使用する開発者を対象としています。

## 前提条件

Target 管理 API を使用するには、[Adobe認証の設定 ](../apis/configure-io-target-integration.md) が必要です。 このチュートリアルを開始する前に、この設定が完了していることを確認してください。

## リソース

このチュートリアルを理解し、このチュートリアルに正しく従うには、次のリソースに注意してください。

| リソース | 詳細 |
| --- | --- |
| 郵便配達員 | お使いのオペレーティングシステム用の [Postman アプリ ](https://www.postman.com/downloads/) を入手します。 Postman 基本はアカウントの作成で無料です。 Adobe Target API を一般的に使用するために必須ではありませんが、Postman は API ワークフローを容易にします。Adobe Targetは、API の実行とその動作の学習に役立つ、複数の Postman コレクションを提供します。 このチュートリアルの残りの部分は、Postman に関する実務知識を前提としています。 サポートが必要な場合は、[Postman のドキュメント ](https://learning.getpostman.com/) を参照してください。 |
| 参照 | このチュートリアルの残りの部分では、次のリソースに関する知識が必要です。<UL><li>[Adobe I/OGithub](https://github.com/adobeio)</li><li>[TargetAdobe I/Oドキュメント](https://developers.adobetarget.com/api/#introduction)</li><li>[Recommendations API ドキュメント](https://developers.adobetarget.com/api/recommendations/)</li></ul> |

[次：「Recommendationsカタログの管理」>](manage-catalog.md)
