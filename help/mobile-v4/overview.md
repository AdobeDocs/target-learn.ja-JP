---
title: Android向けAdobe Mobile Services SDK v4とのAdobe Target
description: Adobe Mobile Services SDK v4 for AndroidとのAdobe Targetは、既にAdobe Mobile Services SDK v4を使用しており、Adobe Targetを使用してアプリのエクスペリエンスをパーソナライズする開始を考えているAndroid開発者にとって最適なスタートポイントです。
feature: mobile
kt: 3040
audience: developer
doc-type: tutorial
activity-type: implement
translation-type: tm+mt
source-git-commit: b331bb29c099bd91df27300ebe199cafa12516db
workflow-type: tm+mt
source-wordcount: '539'
ht-degree: 2%

---


# 概要

_Adobe Mobile Services SDK v4 for AndroidとのAdobe Targetは_ 、既にAdobe Mobile Services SDK v4を使用し、Adobe Targetを使用してアプリのエクスペリエンスをパーソナライズする開始を考えるAndroid開発者にとって、最適なスタートポイントです。

レッスンを完了するためのデモAndroidアプリが用意されています。 このチュートリアルを完了したら、お使いのAndroidアプリで実装する開始 [!DNL Target] の準備が整います。

このチュートリアルでは、以下の内容について学習します。

* Adobe Mobile Services SDK [](https://docs.adobe.com/content/help/en/mobile-services/android/getting-started-android/requirements.html) Setupの検証
* 次のタイプの [!DNL Target] リクエストを実装します。
   * コンテンツのプリフェッチ [!DNL Target]
   * 単一のリクエストでの複数の [!DNL Target] 場所(mbox)のバッチ処理
   * 要求のブロック（アプリの表示前に実行）
   * 非ブロック要求（バックグラウンドで実行）
   * リアルタイム（非キャッシュ）
   * キャッシュバスティングの再フェッチ
* パーソナライゼーションの追加強化を要求するパラメーター
* オーディエンスとオファーの作成
* レイアウトのパーソナライズ
* 機能に関するフラグを付けた新機能を公開する

## 前提条件

このレッスンでは、次の操作を行うと想定します。

* Adobe IDと承認者レベルでAdobe Targetインターフェイスにアクセスできる（以下の検証手順を参照）
* 自分のアカウントにリクエストできるように、Adobe Targetのクライアントコードを知っておく。 クライアントコードは、セットアップ/実装/at.js設定を編集画面のAdobe Targetインターフェイスに表示されます
* Mobile Servicesユーザーインターフェイスにアクセスし、 [Mobile Servicesユーザーインターフェイスに精通している](https://mobilemarketing.adobe.com)
* Androidモバイルアプリの開発用IDEを持っている。 このチュートリアルでは、様々な手順とスクリ [ーンショットで](https://developer.android.com/studio/install) Android Studioを使用します

必要なExperience Cloudソリューションへのアクセス権がない場合は、Experience Cloud管理者に問い合わせてください。

また、JavaのAndroid開発に精通していることを前提としています。 レッスンを完了するのにJavaのエキスパートである必要はありませんが、コードを問題なく読み、理解できれば、レッスンの内容をさらに詳しく理解できるはずです。

### Adobe Targetへのアクセスの確認

このレッスンでは、Adobe Targetにアクセスする必要があります。 次の手順に進む前に、次の手順を実行してAdobe Targetにアクセスできることを確認します。

1. [Adobe Experience Cloudへのログイン](https://experience.adobe.com/)
1. From the Experience Cloud home screen, click [!DNL Target]:
   ![Experience Cloudホーム画面](assets/aec_homeScreen_clickTarget.png)
1. 次の図に示すように、Adobe Targetのアクティビティリストに移動する必要があります。ユーザーに承認者レベルのアクセス権があることを確認してください。 承認者レベルのアクセスにアクセスできない場合、 [!DNL Target] または承認者レベルのアクセスを確認できない場合は、会社のExperience Cloud管理者に問い合わせて、このアクセスを要求し、許可された後でこのチュートリアルを再開してください。

   ![Adobe UI](assets/targetUI_approver.png)

## レッスンについて

このレッスンでは、独自のAdobe Targetアカウントを使用して、「We.Travel」というデモ旅行アプリにAdobe Targetを導入します。 チュートリアルを終えるまでに、アプリの使用状況に基づいて、パーソナライズされたメッセージをユーザーに配信します。 最終的なパーソナライゼーションエクスペリエンスは次のようになります。

![We.Travelアプリfinal](assets/overview_final_result.jpg)

Web.Travelアプリ内の実装を順を追って実行すると、独自のモバイルアプリでを使用して開始を行うこ [!DNL Target] とができます。

始めよう！

**[次へ： 「サンプルアプリのダウンロードと更新」>](download-and-update-the-sample-app.md)**
