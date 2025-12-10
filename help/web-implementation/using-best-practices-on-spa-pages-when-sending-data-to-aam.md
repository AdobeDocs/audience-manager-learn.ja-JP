---
title: AAMにデータを送信する際の SPA ページでのベストプラクティスの使用
description: 単一ページアプリケーション（SPA）からAdobe Audience Manager（AAM）にデータを送信するためのベストプラクティスについて説明します。 この記事では、推奨される実装方法であるExperience Platform タグの使用に焦点を当てています。
feature: Implementation Basics
topics: spa
activity: implement
doc-type: technical video
team: Technical Marketing
kt: 1390
topic: SPA
role: Developer
level: Experienced
exl-id: 99ec723a-dd56-4355-a29f-bd6d2356b402
source-git-commit: d47848370e7bf7617f2b706041c911161a6479cd
workflow-type: tm+mt
source-wordcount: '547'
ht-degree: 0%

---

# AAMにデータを送信する際の SPA ページでのベストプラクティスの使用 {#using-best-practices-on-spa-pages-when-sending-data-to-aam}

このドキュメントでは、単一ページアプリケーション（SPA）からAdobe Audience Manager（AAM）にデータを送信するためのベストプラクティスをいくつか説明します。 この記事では、推奨される実装方法である [!UICONTROL Experience Platform tags] の使用に焦点を当てています。

## 最初のメモ

* 以下の項目は、Platform タグを使用してサイトに実装していることを前提としています。 Platform タグを使用していない場合でも考慮事項は存在しますが、実装方法に合わせて調整する必要があります。
* SPA はそれぞれ異なるため、要件に合わせて次の項目を微調整する必要が生じる場合がありますが、Adobeでは、SPA ページからAudience Managerにデータを送信する際に考慮する必要がある、いくつかのベストプラクティスについて説明します。

## Experience Platform タグ（以前の Launch）での SPA およびAAMの操作に関する簡単な図{#simple-diagram-of-working-with-spas-and-aam-in-experience-platform-launch}

![ タグでの aam 用 spa](assets/spa_for_aam_in_launch.png)

>[!NOTE]
>前述のように、これは Platform タグを使用したAdobe Audience Manager実装（Adobe Analyticsなし）で SPA ページがどのように処理されるかを簡単に示した図です。 ご覧のように、これは非常に簡単です。大きな決定は、ビューの変更（またはアクション）を Platform タグにどのように伝えるかです。

## SPA ページからのタグのトリガー {#triggering-launch-from-the-spa-page}

Platform タグでルールをトリガーする（したがってAudience Managerにデータを送信する）最も一般的な方法は、次の 2 つです。

* JavaScript カスタムイベントの設定（Adobe Analyticsを使用した例 [ こちら ](https://helpx.adobe.com/analytics/kt/using/spa-analytics-best-practices-feature-video-use.html) を参照）
* [!UICONTROL Direct Call Rule] の使用

このAudience Managerの例では、Platform タグで [!UICONTROL Direct Call rule] を使用して、Audience Managerに送信されるヒットをトリガーします。 次の節で説明するように、[!UICONTROL Data Layer] を新しい値に設定すると、Platform タグの [!UICONTROL Data Element] で取得できるので便利です。

## デモページ {#demo-page}

次に、SPA ページの場合と同様に、データレイヤーの値を変更してAudience Managerに送信する方法を示す小さなページを示します。 この機能をモデル化して、より詳細な変更が必要になるようにすることができます。 このデモページは [ こちら ](https://aam.enablementadobe.com/SPA-Launch.html) にあります。

## データレイヤーの設定 {#setting-the-data-layer}

前述のように、新しいコンテンツがページに読み込まれるとき、またはユーザーがサイトでアクションを実行するとき、Platform タグが呼び出されて [!UICONTROL rules] を実行する前に、データレイヤーをページの先頭に動的に設定して、Platform タグがデータレイヤーから新しい値を取得して、Audience Managerにプッシュできるようにする必要があります。

上記のデモサイトに移動し、ページソースを確認すると、次のように表示されます。

* データレイヤーが、Platform タグを呼び出す前の、ページの先頭にあります
* シミュレートされた SPA リンクのJavaScriptが [!UICONTROL Data Layer] を変更してから、Platform タグを呼び出します（`_satellite.track()` 呼び出し）。 この [!UICONTROL Direct Call Rule] の代わりにJavaScript カスタムイベントを使用した場合、レッスンは同じです。 最初に [!DNL data layer] を変更し、次に Platform タグを呼び出します。

>[!VIDEO](https://video.tv.adobe.com/v/23322/?quality=12)

## その他のリソース {#additional-resources}

* [Adobe フォーラムでの SPA ディスカッション ](https://forums.adobe.com/thread/2451022)
* [Platform タグでの SPA の実装方法を示すリファレンスアーキテクチャのサイト ](https://helpx.adobe.com/experience-manager/kt/integration/using/launch-reference-architecture-SPA-tutorial-implement.html)
* [Adobe Analyticsでの SPA トラッキング時のベストプラクティスの使用 ](https://helpx.adobe.com/analytics/kt/using/spa-analytics-best-practices-feature-video-use.html)
* [ この記事で使用するデモサイト ](https://aam.enablementadobe.com/SPA-Launch.html)
