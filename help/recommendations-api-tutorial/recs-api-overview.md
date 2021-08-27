---
title: Adobe Recommendations APIとは
description: このチュートリアルでは、Adobe Target Recommendations APIを使用してRecommendationsのカタログとカスタム条件の設定と管理を行う実践、およびDelivery APIを使用してRecommendationsのコンテンツを取得する実践を、開発者に対して順を追って説明します。
role: Developer
level: Intermediate
topic: Personalization, Administration, Integrations, Development
feature: APIs/SDKs, Recommendations, Administration & Configuration, Overview
doc-type: tutorial
kt: 3815
thumbnail: null
author: Judy Kim
exl-id: 10f80056-fb71-4362-86bc-d161f596cb91
source-git-commit: d1517f0763290eb61a9e4eef4f2eb215a9cdd667
workflow-type: tm+mt
source-wordcount: '359'
ht-degree: 2%

---

# Adobe Recommendations APIの概要

[!DNL Recommendations]に関連するAPIには、以下を実行できる[管理API](https://experienceleague.adobe.com/docs/target/using/apis/api-overview.html?lang=en)が含まれます。

* レコメンデーション可能な製品やコンテンツのカタログの管理
* [!DNL Recommendations]アルゴリズムとアクティビティの管理

Recommendationsで[!DNL Target] [delivery API](https://experienceleague.adobe.com/docs/target/using/apis/api-overview.html?lang=en)を使用すると、次のこともできます。

* JSON、HTML、またはXMLオブジェクトでレコメンデーションを取得して、Web、モバイル、電子メール、Internet of Things(IOT)、その他のチャネルで表示できるようにします。

## チュートリアルの説明

このチュートリアルでは、[!DNL Recommendations] APIを使用して[!DNL Recommendations]カタログとカスタム条件を設定および管理し、Delivery APIを使用してRecommendationsコンテンツを取得する、実践的な方法について説明します。 このチュートリアルを最後まで学習すると、次の内容を習得できます。

* Recommendations APIを使用したエンティティの設定と管理
* Recommendations APIを使用したカスタム条件の設定と管理
* RecommendationsをDelivery APIと共に使用して、HTML以外のデバイスでレコメンデーションの結果を使用する方法を理解する

## オーディエンス

このチュートリアルは、Target APIまたはRecommendations APIを初めて使用する開発者を対象としています。

## 前提条件

Target管理APIを使用するには、[Adobe認証の設定](../apis/configure-io-target-integration.md)が必要です。 このチュートリアルを開始する前に、この設定が完了していることを確認してください。

## リソース

このチュートリアルを理解し、このチュートリアルに正しく従うために必要な、次のリソースに注意してください。

| リソース | 詳細 |
| --- | --- |
| 郵便配達人 | お使いのオペレーティングシステム用の[Postmanアプリ](https://www.postman.com/downloads/)を入手します。 Postman basicは、アカウントの作成を無料で行います。 Adobe Target APIを一般的に使用するために必須ではありませんが、PostmanはAPIワークフローを容易にします。Adobe Targetは、APIの実行とその動作の学習に役立つ、複数のPostmanコレクションを提供します。 このチュートリアルの残りの部分は、Postmanに関する実務知識を前提としています。 サポートが必要な場合は、[Postmanのドキュメント](https://learning.getpostman.com/)を参照してください。 |
| 参照 | このチュートリアルの残りの部分では、次のリソースに関する知識が前提となります。<UL><li>[Adobe I/OGithub](https://github.com/adobeio)</li><li>[TargetAdobe I/Oのドキュメント](https://developers.adobetarget.com/api/#introduction)</li><li>[Recommendations APIドキュメント](https://developers.adobetarget.com/api/recommendations/)</li></ul> |

[次：「Recommendationsカタログの管理」>](manage-catalog.md)
