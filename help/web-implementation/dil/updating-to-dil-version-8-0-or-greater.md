---
title: Adobe Audience ManagerのDILバージョン8.0（またはそれ以降）へのアップデート
description: この記事では、Adobe Audience Manager(AAM)Data Integration Library(DIL)コードをバージョン8.0以降に更新する手順と推奨事項を示します。 これは、Adobe Analyticsデータのサーバー側転送ではなく、「クライアント側」のDIL実装に関するもので、Adobeタグマネージャーソリューションを使用しないDTM、Launch by Adobe、実装に関するものです。
feature: dil implementation
topics: null
audience: implementer
activity: implement
doc-type: technical video
team: Technical Marketing
kt: 1841
translation-type: tm+mt
source-git-commit: dfd549508cc223714bdb07ac6fd2aa31e6ca5586
workflow-type: tm+mt
source-wordcount: '1149'
ht-degree: 3%

---


# Adobe Audience ManagerのDILバージョン8.0（またはそれ以降）へのアップデート {#updating-to-adobe-audience-manager-s-dil-version-or-greater}

この記事では、Adobe Audience Manager(AAM) [!DNL Data Integration Library] (DIL)コードをバージョン8.0以降に更新する手順と推奨事項を示します。 これは、Adobe Analyticsデータのサーバー側転送ではなく、「クライアント側」のDIL実装に関するもので、Adobeタグマネージャーソリューションを使用しないDTM、Launch by Adobe、実装に関するものです。

## 概要 {#overview}

Audience Managerの [!DNL Data Integration Library] (DIL)コードを使用して、貴社のウェブサイト*にAAMを導入できます。 以前のバージョンのDILを実装する場合、AdobeのExperience CloudIDサービス(ECID)も実装する必要はありませんでした（ただし、実装することは非常に良い方法でした）。 DILバージョン8.0以降では、ECIDバージョン3.3以降に対する依存関係が厳しくなりました。 ECID 3.3を使用せずにDIL8.0以降を実装した場合、またはそれより前のバージョンを使用した場合は、エラーが発生し、機能しません。 AAMの実装には複数の方法があるので、このページを作成し、実行する手順と推奨事項を示します。 以下の表に、プラットフォーム/実装方法別のこれらの手順と推奨事項を示します。 DILの詳細については、 [ドキュメントを参照してください](https://marketing.adobe.com/resources/help/en_US/aam/c_dil.html)。

* このページの説明に記載されているように、これは、Adobe Analyticsを持たないAAMのお客様が使用する「クライアント側」のDIL導入に限定されます。 Adobe Analyticsをお持ちの場合は、AAMを実装する際に、サーバ側転送方式を使用する必要があります。 This method is described in the [documentation](https://marketing.adobe.com/resources/help/ja_JP/reference/ssf.html).

## 重複および非推奨の要素とメソッド {#duplicate-and-deprecated-elements-and-methods}

DILとECIDの以前のバージョンでは、重複するメソッド(DILとECIDの両方で同じ機能を果たすメソッド)があったため、どちらを使用するかに関して混乱が生じていました。 通常は、両方を使用してそれらを一致させる必要があり、そのメッセージがアドビのお客様によく伝えられませんでした。 DIL8.0以降、これらの重複メソッドと要素はDILで非推奨となっており、ECIDバージョンを使用することをお勧めします。

例：

* を使用する場合 [!DNL DIL.create]、いくつかの要素が廃止されたので、代わりにECID要素を使用する必要があります。 これらの要素は、 [[!DNL DIL.create] ドキュメントで呼び出されます](https://marketing.adobe.com/resources/help/en_US/aam/r_dil_create.html)。
* インスタンス [!DNL idSync] レベルのメソッドも非推奨となりました。このメソッドは、メソッドの [ドキュメントで取り上げています](https://marketing.adobe.com/resources/help/en_US/aam/r_dil_idsync.html)。

## 顧客IDとのID同期 {#id-syncing-with-a-customer-id}

AAMでは、マシン上のUUID（匿名の一意のユーザーID）を顧客IDと同期できるので、その顧客に関するオフラインデータをアップロードし、その顧客のオンライン動作と関連付けて、顧客の理解を深めることができます。 これまでは、次の2つの方法のいずれかで行われてきました。

* インスタンス [!DNL idSync] レベルのメソッド
* の [!DNL declaredId] 要素 [!DNL DIL.create]

これらの古い方法のいずれかを使用して顧客IDと同期している場合は、ECIDサービスの一部である [!DNL setCustomerIDs] 方法を使用するように更新することを強くお勧めします。 の詳細につ [!DNL setCustomerIDs] いては、メソッドの [ドキュメントを参照してください](https://marketing.adobe.com/resources/help/en_US/mcvid/mcvid_setcustomerids.html)。

**クイックヒント：** 以前に上記のいずれかのメソッドを使用していた場合は、AAM [!UICONTROL Data Source] を [!UICONTROL Data Source] ID(「DPID」)で参照していました。 に更新する場合 [!DNL setCustomerIDs]、代わりにAAMの「」 [!UICONTROL Data Source]を使用する必要があり[!UICONTROL Integration Code]ます。 引き続き同じIDを指し [!UICONTROL Data Source] ますが、単に別のIDを指します。 以下のビデオで示します。

>[!VIDEO](https://video.tv.adobe.com/v/23873/?quality=12)

以下の節では、お使いの実装方法に基づいてDIL8.0に更新するためのリスト手順と推奨事項を示します。

## Adobe Experience Platform LaunchのDIL8.0へのアップデート {#updating-to-dil-in-experience-platform-launch}

DIL8.0への更新の基本手順

1. 8.0より前のDILを使用している場合は、アップグレードの前にAAM拡張機能のDIL設定に進み、使用している高度なオプション（以降の手順で使用する場合）を控えておきます
1. AAM拡張機能をバージョン8.0以降に更新する
1. Experience CloudIDサービス拡張機能がバージョン3.3.0以降であることを確認します。
1. 8.0より前のAAM Extensionにあった非推奨のメソッド/エレメント(「[!DNL disableIDSyncs]」など)や、DIL用のカスタムコードにあった場合は、ECID拡張でECIDメソッドを有効にします。

   1. (DIL) disableDestinationPublishingIframe -> (ECID) disableIdSyncs
   1. (DIL) disableIDSyncs -> (ECID) disableIdSyncs
   1. (DIL)iframeAkamaiHTTPS -> (ECID) dSyncSSLUseAkamai
   1. (DIL) declaredId -> (ECID) setCustomerIDs

1. 変更を公開する

>[!VIDEO](https://video.tv.adobe.com/v/23874/?quality=12)

## AdobeDTMのDIL8.0へのアップデート {#updating-to-dil-in-adobe-dtm}

1. AAMツールをバージョン8.0以降に更新します。 このバージョン設定は、AAMツールの「一般」セクションにあります。
1. 8.0より前のAAM Toolのカスタムコードに含まれていた非推奨のメソッド/要素（「disableIDSyncs」など）をDILする場合は、それらをメモし（ECIDツールに追加できるように）、AAMツールのカスタムから削除し [!DNL DIL code] ます。
1. Experience CloudIDサービス拡張機能をバージョン3.3.0以降に更新する
1. AAMツ追加ールのカスタムコードから削除したECIDツールの高度なオプション。
1. 変更を公開する

## Tag Management Solutionを使用しないDIL8.0への更新 {#additional-resources}

ページ上でコードを直接更新する場合は、前述のようにメソッド/エレメントをDILからECIDに移動する必要がある場合を除き、古いアイテムを新しいアイテムに置き換えることができます。 その場合は、DILーの場所にある古いメソッドまたはエレメントを、ECIDの場所にあるECIDメソッドまたはエレメントに置き換えます。

Adobe以外のタグマネージャーにも同じことが言えます。 tag managementソリューションに古いバージョンがある場合は、次の手順に従って、新しいコードに置き換えます。

1. DILライブラリを最新バージョン（8.0以降）に更新する — 最新のDILコードは、現在公開場所では利用できないので、AdobeコンサルティングまたはAdobeカスタマーケアから入手する必要があります。 その後、古いDILライブラリコードを新しいDILライブラリコードに置き換えて、次の手順に進みます（今すぐ停止しないでください。問題が発生する場合は、ha）。
1. 既存のバージョン [!DNL ECID Service] を3.3.0以降にインストールまたは更新します。 You can download the latest Experience Cloud ID Service release [from our GitHub page](https://github.com/Adobe-Marketing-Cloud/id-service/releases). この問題に関するヘルプが必要な場合は、 [ドキュメントを参照するか](https://marketing.adobe.com/resources/help/ja_JP/mcvid/) 、Adobeコンサルタントにお問い合わせください。

1. 非推奨のメソッドまたはDIL用のカスタムコード内にある要素が、ECIDメソッドに移動されていることを確認します。

   1. (DIL) disableDestinationPublishingIframe -> (ECID) disableIdSyncs

      [ドキュメント](https://marketing.adobe.com/resources/help/ja_JP/mcvid/mcvid-disableidsync.html)

   1. (DIL) disableIDSyncs -> (ECID) disableIdSyncs

      [ドキュメント](https://marketing.adobe.com/resources/help/ja_JP/mcvid/mcvid-disableidsync.html)

   1. (DIL)iframeAkamaiHTTPS -> (ECID) idSyncSSLUseAkamai

      [ドキュメント](https://marketing.adobe.com/resources/help/en_US/aam/r_dil_create.html)

   1. (DIL) declaredId -> (ECID) setCustomerIDs

      [ドキュメント](https://marketing.adobe.com/resources/help/en_US/mcvid/mcvid_setcustomerids.html)