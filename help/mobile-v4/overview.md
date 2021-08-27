---
title: Android用AdobeMobile Services SDK v4を使用したAdobe Target
description: Android向けAdobeMobile Services SDK v4を使用したAdobe Targetは、既にAdobeMobile Services SDK v4を使用し、Adobe Targetでのアプリエクスペリエンスのパーソナライズを開始したいAndroid開発者にとって最適な出発点です。
role: Developer
level: Intermediate
topic: Mobile, Personalization
feature: Implement Mobile, Overview
doc-type: tutorial
kt: 3040
thumbnail: null
exl-id: 20f8ed4f-a86d-4c5e-9296-71a93724caa3
source-git-commit: ee58c7c85708722cf040cd9b039a2855dd390a16
workflow-type: tm+mt
source-wordcount: '549'
ht-degree: 2%

---

# Android用AdobeMobile Services SDK v4を使用したAdobe Target — 概要

_Android向けAdobeMobile Services SDK v4を使用したAdobe Targetは、既にAdobeMobile Services SDK v4を使用しており、Adobe Targetでのアプリエクスペリエンスのパーソナライズを開始したいAndroid開発者にとって最適な出発点です。_ 

レッスンを完了するためのデモ用Androidアプリが用意されています。 このチュートリアルを完了すると、独自のAndroidアプリに[!DNL Target]を実装する準備が整います。

このチュートリアルでは、以下の内容について学習します。

* [AdobeMobile Services SDK](https://experienceleague.adobe.com/docs/mobile-services/android/getting-started-android/requirements.html?lang=en)の設定を検証します
* 次のタイプの[!DNL Target]リクエストを実装します。
   * [!DNL Target]コンテンツのプリフェッチ
   * 1つのリクエストで複数の[!DNL Target]場所(mbox)をバッチ処理
   * リクエストのブロック（アプリの表示前に実行）
   * 非ブロックリクエスト（バックグラウンドで実行）
   * リアルタイム（非キャッシュ）
   * キャッシュバスティングのリフェッチ
* 拡張パーソナライゼーションのリクエストへのパラメーターの追加
* オーディエンスとオファーの作成
* レイアウトのパーソナライズ
* 機能フラグ付けを使用した新機能のロールアウト

## 前提条件

このレッスンでは、次の操作を想定しています。

* Adobe TargetインターフェイスにAdobeIDと承認者レベルでアクセスできる（以下の検証手順を参照）
* 独自のアカウントにリクエストを送信できるAdobe Targetクライアントコードを把握する。 クライアントコードは、   セットアップ/実装/ at.js設定を編集画面
* にアクセスでき、[Mobile Servicesユーザーインターフェイス](https://mobilemarketing.adobe.com/)に精通している
* Androidモバイルアプリ開発用のIDEを持っている。 このチュートリアルは、様々な手順とスクリーンショットで[Android Studio](https://developer.android.com/studio/install)を備えています。

必要なExperience Cloudソリューションへのアクセス権がない場合は、Experience Cloud管理者に問い合わせてください。

また、JavaでのAndroidの開発に精通していることを前提としています。 Javaの専門家でなくてもレッスンを完了できますが、コードを快適に読んで理解できれば、レッスンを最大限活用できます。

### Adobe Targetへのアクセスの検証

このレッスンでは、Adobe Targetにアクセスする必要があります。 次の手順に進む前に、次の手順を実行して、Adobe Targetにアクセスできることを確認してください。

1. [Adobe Experience Cloud](https://experience.adobe.com/)にログインします。
1. Experience Cloudのホーム画面で、[!DNL Target]をクリックします。
   ![Experience Cloudのホーム画面](assets/aec_homeScreen_clickTarget.png)
1. 次の図に示すように、Adobe Targetのアクティビティリストに移動し、ユーザーが承認者レベルのアクセス権を持っていることを確認します。 [!DNL Target]にアクセスできない場合や、承認者レベルのアクセス権を確認できない場合は、会社のExperience Cloud管理者に連絡し、このアクセス権を要求して、許可された後でこのチュートリアルを再開してください。

   ![AdobeUI](assets/targetUI_approver.png)

## レッスンについて

このレッスンでは、独自のAdobe Targetアカウントを使用して、Adobe Targetを「We.Travel」と呼ばれるデモ用旅行アプリに実装します。 チュートリアルの最後までに、アプリの使用状況に基づいてパーソナライズされたメッセージをユーザーに配信します。 最終的なパーソナライゼーションエクスペリエンスは次のようになります。

![We.Travelアプリの最終版](assets/overview_final_result.jpg)

We.Travelアプリ内での実装を順を追った後、独自のモバイルアプリで[!DNL Target]の使用を開始できます。

始めましょう！

**[次へ：「サンプルアプリのダウンロードと更新」>](download-and-update-the-sample-app.md)**
