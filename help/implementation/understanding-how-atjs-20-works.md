---
title: at.js 2.0 の仕組み
description: at.js 2.0 で、シングルページアプリケーション（SPA）のAdobe Targetのサポートを強化し、他のExperience Cloud ソリューションと統合する方法を説明します。
role: Developer
level: Intermediate
topic: SPA, Architecture, Development
feature: Implementation
doc-type: technical video
kt: null
author: Daniel Wright
exl-id: 7f037665-88a7-469c-8df5-c82cb0f65382
source-git-commit: fcd2273ba373dc2b3bc59a77f1925cdb7b2ed3ee
workflow-type: tm+mt
source-wordcount: '392'
ht-degree: 0%

---

# Adobe Targetの at.js 2.0 の仕組みを理解する

`at.js` 2.0 は、シングルページアプリケーション（SPA）に対するAdobe Targetのサポートを強化し、他のExperience Cloud ソリューションと統合されています。 このビデオと付属の図では、すべてがどのように統合されるかを説明します。

>[!VIDEO](https://video.tv.adobe.com/v/26250?quality=12)

## アーキテクチャ図

![ ページ読み込み時の at.js 2.0 の動作 ](assets/pageload.png)

1. 呼び出しによってExperience Cloud ID （ECID）が返されます。 ユーザーが認証されると、別の呼び出しでその顧客 ID が同期されます。

1. ライブラリ `at.js` 同期して読み込まれ、ドキュメントの本文を非表示にします（ページに実装され `at.js` オプションの事前非表示スニペットを使用して非同期で読み込むこともできます）。

1. すべての設定済みパラメーター、ECID、SDID、顧客 ID を含むページ読み込みリクエストが行われます。

1. プロファイルスクリプトは、を実行して [!UICONTROL Profile Store] に入力します。 ストアは、[!UICONTROL Audience Library] から選定オーディエンス（[!DNL Analytics]、Audience Managerなどから共有されたオーディエンスなど）をリクエストします。 [!UICONTROL Customer Attributes] は、バッチ処理で [!UICONTROL Profile Store] に送信されます。
1. URL、リクエストパラメーター、プロファイルデータに基づいて、現在のページと今後の表示で訪問者に返すアクティビティとエクスペリエンスを [!DNL Target] 定します

1. ターゲットコンテンツがページに送り返されます（オプションで、パーソナライゼーションを追加するためのプロファイル値も含む）。

   現在のページ上のターゲットコンテンツは、デフォルトコンテンツのちらつきなしでできるだけ早く表示されます。

   単一ページアプリケーションの今後のビュー用のターゲットコンテンツは、ブラウザーにキャッシュされます。そのため、ビューがトリガーされたときに追加のサーバー呼び出しをおこなわずに即座にターゲットコンテンツを適用できます。 （`triggerView()` の動作については、次の図を参照してください）。

1. ページから [!DNL Analytics] サーバーに送信された [!UICONTROL Data Collection] データ
1. [!DNL Target] データは、SDID を介して Analytics データと照合され、[!DNL Analytics] レポートストレージに処理されます。 A4T レポートを使用して、[!DNL Analytics] データが [!DNL Analytics] と [!DNL Target] の両方に表示できるようになります。

![triggerView （）関数が使用された場合の at.js 2.0 の動作 ](assets/triggerview.png)

1. `adobe.target.triggerView()` は、単一ページアプリケーションで呼び出されます
1. ビューのターゲットコンテンツがキャッシュから読み取られる

1. ターゲットコンテンツは、デフォルトコンテンツのちらつきなしでできるだけ早く表示されます

1. アクティビティ内の訪問者をカウントして指標を増分するための通知リクエストが [!DNL Target] [!UICONTROL Profile Store] ーザーに送信されます
1. [!DNL Analytics] データは SPA から [!UICONTROL Data Collection] サーバーに送信されます

1. データ [!DNL Target]、[!DNL Target] バックエンドから [!UICONTROL Data Collection] Server に送信されます。 [!DNL Target] データは、SDID を介して [!DNL Analytics] データと照合され、[!DNL Analytics] レポートストレージに処理されます。 A4T レポートを使用して、[!DNL Analytics] データが [!DNL Analytics] と [!DNL Target] の両方に表示できるようになります。

## その他のリソース

* [at.js 2.0 のシングルページアプリケーションへの実装](implement-atjs-20-in-a-single-page-application.md)
* [Adobe Targetのシングルページアプリケーション用 Visual Experience Composer （SPA VEC）の使用](../experiences/use-the-visual-experience-composer-for-single-page-applications.md)
