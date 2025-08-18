---
title: Adobe Audience Manager DIL バージョン 8.0 （またはそれ以降）への更新
description: この記事では、Adobe Audience Manager（AAM）Data Integration Library（DIL）コードをバージョン 8.0 以降に更新する手順と推奨事項を説明します。 これは、Adobe Analytics データのサーバーサイド転送ではなく、「クライアントサイド」のDIL実装を指し、DTM、アドビによる Launch、Adobe タグマネージャーソリューションを使用しない実装について説明します。
feature: DIL Implementation
topics: null
activity: implement
doc-type: technical video
team: Technical Marketing
kt: 1841
role: Developer, Data Engineer
level: Intermediate
exl-id: 8c1e6ed5-0f21-427b-a681-0ecb020a0e60
source-git-commit: 62b43b5627dabf754cf821f974a56c60989ef7ef
workflow-type: tm+mt
source-wordcount: '1074'
ht-degree: 1%

---

# Adobe Audience ManagerのDIL バージョン 8.0 （またはそれ以降）への更新 {#updating-to-adobe-audience-manager-s-dil-version-or-greater}

この記事では、Adobe Audience Manager（AAM） [!DNL Data Integration Library] （DIL）コードをバージョン 8.0 以降に更新する手順と推奨事項を説明します。 これは、Adobe Analytics データのサーバーサイド転送ではなく、「クライアントサイド」のDIL実装を指し、DTM、アドビによる Launch、Adobe タグマネージャーソリューションを使用しない実装について説明します。

## 概要 {#overview}

Audience Manager [!DNL Data Integration Library] （DIL）コードを使用すると、web サイトにAAMを実装できます*。 以前のバージョンのDILを実装する場合、AdobeのExperience Cloud ID サービス（ECID）も実装する必要はありませんでした（ただし、非常に良いアイデアでした）。 DIL バージョン 8.0 以降、ECID バージョン 3.3 以降には強い依存関係があります。 ECID 3.3 を使用せずに、または以前のバージョンでDIL 8.0 以降を実装した場合、エラーが発生し、機能しません。 AAMを実装する方法は複数あるので、このページを作成して、いくつかの手順と推奨事項を説明しました。 以下に、これらの手順と推奨事項をプラットフォーム/実装方法別に示します。 DILについて詳しくは、[ ドキュメント ](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-overview.html?lang=en) を参照してください。

* このページの説明に記載されているように、ここでは、Adobe Analyticsを持たないAAMのお客様が使用する「クライアントサイド」のDIL実装についてのみ説明します。 Adobe Analyticsがある場合は、AAMを実装するサーバーサイド転送方式を使用する必要があります。 このメソッドについては、[ ドキュメント ](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/server-side-forwarding/ssf.html?lang=ja) を参照してください。

## 重複する非推奨（廃止予定）の要素とメソッド {#duplicate-and-deprecated-elements-and-methods}

以前のバージョンのDILと ECID には、DILと ECID の両方で同じ機能を実行する手法という重複があり、どちらを使用するかで混乱が生じていました。 通常は、両方を使用してマッチングする必要があり、そのメッセージはお客様によく伝えられていませんでした。 DIL 8.0 以降、これらの重複するメソッドおよび要素はDILで非推奨になりました。ECID バージョンを使用することをお勧めします。

例：

* [!DNL DIL.create] を使用する場合、いくつかの要素は非推奨となっており、代わりに ECID 要素を使用する必要があります。 これらの要素は、[[!DNL DIL.create]  ドキュメント ](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/class-level-dil-methods/dil-create.html) で呼び出されます。
* [!DNL idSync] インスタンスレベルのメソッドも非推奨（廃止予定）となり、メソッドの [ ドキュメント ](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-instance-methods.html) で呼び出されています。

## 顧客 ID との ID 同期 {#id-syncing-with-a-customer-id}

AAMでは、マシン上の UUID （匿名の一意のユーザー ID）を顧客 ID と同期させることができます。これにより、その顧客に関するオフラインデータをアップロードし、オンライン行動と結び付けて、顧客を深く理解できます。 これまでは、次の 2 つの方法のいずれかでおこなわれてきました。

* [!DNL idSync] インスタンスレベルのメソッド
* [!DNL declaredId] の [!DNL DIL.create] 要素

これらの古い方法のいずれかを使用して顧客 ID と同期している場合は、ECID サービスの一部である [!DNL setCustomerIDs] メソッドを使用して、に更新することを強くお勧めします。 [!DNL setCustomerIDs] について詳しくは、メソッドの [ ドキュメント ](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/methods/setcustomerids.html?lang=ja) を参照してください。

**クイックヒント：** 上記のいずれかの方法を以前に使用した際には、[!UICONTROL Data Source] ID （別名「DPID」）を持つAAM [!UICONTROL Data Source] を参照していました。 [!DNL setCustomerIDs] に更新する場合は、代わりにAAM [!UICONTROL Data Source] の「[!UICONTROL Integration Code]」を使用する必要があります。 同じ [!UICONTROL Data Source] を指していますが、単に別の識別子です。 これを以下のビデオで示します。

>[!VIDEO](https://video.tv.adobe.com/v/23873/?quality=12)

次の節では、実装方法に基づいてDIL 8.0 にアップデートする際の手順と推奨事項を説明します。

## Adobe Experience Platform タグのDIL 8.0 への更新 {#updating-to-dil-in-experience-platform-launch}

DIL 8.0 への更新の基本手順

1. 8.0 より前のDILを使用している場合は、アップグレードする前にAAM拡張機能のDIL設定に移動し、使用している詳細設定オプションをメモします（その後の手順で使用します）。
1. AAM拡張機能をバージョン 8.0 以降に更新します。
1. お使いのExperience Cloud ID サービス拡張機能がバージョン 3.3.0 以降であることを確認します。
1. 8.0 より前のAAM拡張機能またはDILのカスタムコードに含まれていた非推奨のメソッドや要素（`disableIDSyncs` など）については、ECID 拡張機能で ECID メソッドを有効にします。

   1. （DIL） `disableDestinationPublishingIframe` -> （ECID） `disableIdSyncs`
   1. （DIL） `disableIDSyncs` -> （ECID） `disableIdSyncs`
   1. （DIL） `iframeAkamaiHTTPS` -> （ECID） `dSyncSSLUseAkamai`
   1. （DIL） `declaredId` -> （ECID） `setCustomerIDs`

1. 変更内容を公開します。

>[!VIDEO](https://video.tv.adobe.com/v/23874/?quality=12)

## Adobe DTM のDIL 8.0 への更新 {#updating-to-dil-in-adobe-dtm}

1. AAM ツールをバージョン 8.0 以降に更新します。 このバージョン設定は、AAM ツールの「一般」セクションにあります。
1. 8.0 より前のAAM ツールのDIL用カスタムコードに含まれていた非推奨のメソッドや要素（`disableIDSyncs` など）については、それらをメモして（ECID ツールに追加できるように）、AAM ツールのカスタムコ [!DNL DIL code] ドから削除します。
1. Experience Cloud ID サービス拡張機能をバージョン 3.3.0 以降に更新します
1. AAM ツールのカスタムコードから削除した ECID ツールに詳細オプションを追加します。
1. 変更を公開する

## DIL Tag Management ソリューションを使用しない場合のAdobe 8.0 への更新 {#additional-resources}

ページ上で直接コードを更新する場合は、上記のようにDILから ECID にメソッドや要素を移動する必要がある場合を除き、古い項目を新しい項目に置き換えるだけです。 その場合は、DILの場所の古いメソッドや要素を、ECID の場所の ECID メソッドや要素に置き換えるだけです。

Adobe以外のタグマネージャーについても同じことが言えます。 そのタグ管理ソリューションに古いバージョンがある場合は、次の手順に従って、そのバージョンを新しいコードに置き換えます。

1. DIL ライブラリを最新バージョン（8.0 以降）に更新 – 現在、公開されている場所では利用できないので、最新のDIL コードをAdobe ConsultingまたはAdobe カスタマーケアから取得する必要があります。 その後、古いDIL ライブラリコードを新しいDIL ライブラリコードに置き換え、次の手順に進みます（今すぐ停止しないでください。そうしないと、問題が発生します）。
1. [!DNL ECID Service] をインストールするか、既存のバージョンを 3.3.0 以降に更新します。 最新のExperience Cloud ID サービスリリースを [GitHub ページから ](https://github.com/Adobe-Marketing-Cloud/id-service/releases) ダウンロードできます。 これに関するヘルプが必要な場合は、[ ドキュメント ](https://experienceleague.adobe.com/docs/id-service/using/home.html?lang=ja) を参照するか、Adobe コンサルタントにお問い合わせください。

1. DILのカスタムコードにある非推奨のメソッドや要素が、ECID メソッドに移動されることを確認します。

   1. （DIL） `disableDestinationPublishingIframe` -> （ECID） `disableIdSyncs`

      [ドキュメント](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/configurations/disableidsync.html?lang=ja)

   1. （DIL） `disableIDSyncs` -> （ECID） `disableIdSyncs`

      [ドキュメント](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/configurations/disableidsync.html?lang=ja)

   1. （DIL） `iframeAkamaiHTTPS` -> （ECID） `idSyncSSLUseAkamai`

      [ドキュメント](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/class-level-dil-methods/dil-create.html)

   1. （DIL） `declaredId` -> （ECID） `setCustomerIDs`

      [ドキュメント](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/methods/setcustomerids.html?lang=ja)
