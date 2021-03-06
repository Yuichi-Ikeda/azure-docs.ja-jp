---
title: 中速から高速のネットワーク帯域幅での、大規模なデータセットの Azure データ転送のオプション | Microsoft Docs
description: ご利用の環境に中速から高速のネットワーク帯域幅があり、大規模なデータセットを転送する予定の場合に、データ転送に関する Azure ソリューションを選択する方法について説明します。
services: storage
author: alkohli
ms.service: storage
ms.subservice: blob
ms.topic: article
ms.date: 12/07/2018
ms.author: alkohli
ms.openlocfilehash: bc5668d826395fb71ee70907f095303a43f1ec7f
ms.sourcegitcommit: e7312c5653693041f3cbfda5d784f034a7a1a8f1
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/11/2019
ms.locfileid: "54214319"
---
# <a name="data-transfer-for-large-datasets-with-moderate-to-high-network-bandwidth"></a>中速から高速のネットワーク帯域幅での大規模なデータセットのデータ転送
 
この記事では、ご利用の環境に中速から高速のネットワーク帯域幅があり、大規模なデータセットを転送する予定の場合の、データ転送ソリューションの概要を示します。 また、この記事では、推奨されるデータ転送と、このシナリオのそれぞれの主要な機能のマトリックスについても説明します。

利用可能なすべてのデータ転送オプションの概要を理解するには、[Azure データ転送ソリューションの選択](storage-choose-data-transfer-solution.md)に関するページに移動します。

## <a name="scenario-description"></a>シナリオの説明

大規模なデータセットでは、TB から PB の順にデータ サイズを参照します。 中速から高速のネットワーク帯域幅では、100 Mbps から 10 Gbps の順に参照します。

## <a name="recommended-options"></a>推奨されるオプション

このシナリオで推奨されるオプションは、中速ネットワーク帯域幅であるか、高速ネットワーク帯域幅であるかによって異なります。

### <a name="moderate-network-bandwidth-100-mbps---1-gbps"></a>ネットワーク帯域幅 - 中 (100 Mbps から 1 Gbps)

中速ネットワーク帯域幅では、ネットワーク経由のデータ転送時間をプロジェクションする必要があります。

以下の表を使用して時間を予測し、それに基づいて、オフライン転送かネットワーク経由の転送を選びます。 表には、さまざまなネットワーク帯域幅に対する、ネットワーク データ転送の予想時間が示されています (90% の使用率と仮定)。  

![ネットワーク転送またはオフライン転送](media/storage-solution-large-dataset-low-network/storage-network-or-offline-transfer.png)

- ネットワーク転送に時間がかかりすぎると予想される場合は、物理デバイスを使用する必要があります。 この場合に推奨されるオプションは、Azure Data Box ファミリのデバイスを使用するオフライン転送、または独自のディスクを使用する Azure Import/Export です。

    - **オフライン転送用の azure Data Box ファミリ** – 時間、ネットワークの可用性、またはコストが限られている場合は、Microsoft によって提供される Data Box デバイスのデバイスを使用して、大量のデータを Azure に移動します。 Robocopy などのツールを使って、オンプレミスのデータをコピーします。 転送対象のデータのサイズに応じて、Data Box Disk、Data Box、Data Box Heavy から選択できます。
    - **Azure Import/Export** – 大量のデータを安全に Azure Blob Storage と Azure Files にインポートするために、独自のディスク ドライブを送付して Azure Import/Export サービスを使用します。 また、このサービスでは、Azure Blob Storage からディスク ドライブにデータを転送し、オンプレミスのサイトに送付できます。

- ネットワーク転送が妥当であると予想される場合は、「[ネットワーク帯域幅 - 高](#high-network-bandwidth)」で詳しく説明されている以下のツールのいずれかを使用できます。


### <a name="high-network-bandwidth-1-gbps---100-gbps"></a>ネットワーク帯域幅 - 高 (1 Gbps から 100 Gbps)

利用可能なネットワーク帯域幅が高い場合は、次のツールのいずれかを使用します。

- **AzCopy** - このコマンドライン ツールを使用すると、最適なパフォーマンスで Azure BLOB、Files、および Table ストレージとの間で相互に簡単にデータをコピーできます。 AzCopy はコンカレンシーと並列処理をサポートし、中断された場合にコピー操作を再開することができます。
- **Azure Storage REST API/SDK** – アプリケーションの構築時に、Azure Storage REST API に対するアプリケーションを開発し、複数の言語で提供される Azure SDK を使用できます。
- **オンライン転送用の Azure Data Box** – Data Box Edge と Data Box Gateway はオンラインのネットワークデバイスであり、Azure の内外へのデータの移動が可能です。 継続的な取り込みと、アップロード前のデータの前処理を同時に行う必要がある場合は、Data Box Edge の物理デバイスを使用します。 Data Box Gateway は、同じデータ転送機能を備えた、デバイスの仮想バージョンです。 いずれの場合も、データ転送はデバイスによって管理されます。
- **Azure Data Factory** – 転送操作をスケールアウトする場合や、オーケストレーションとエンタープライズ レベルの監視機能が必要な場合は、Data Factory を使用する必要があります。 Data Factory を使用し、複数の Azure サービス間またはオンプレミスで、あるいは 2 つを組み合わせて、ファイルを定期的に転送します。 Data Factory では、さまざまなデータ ストアからデータを取り込み、データ移動とデータ転送を自動化するデータ駆動型ワークフロー (パイプラインと呼ばれる) を作成し、スケジュールを設定できます。

## <a name="comparison-of-key-capabilities"></a>主な機能の比較

次の表は、推奨されるオプションの主な機能の違いをまとめたものです。

### <a name="moderate-network-bandwidth"></a>ネットワーク帯域幅 - 中

オフライン データ転送を使用する場合は、以下の表を使用して、主な機能の違いを理解します。

|                                     |    Data Box Disk (プレビュー)    |    Data Box                                      |    Data Box Heavy (プレビュー)              |    Import/Export                       |
|-------------------------------------|---------------------------------|--------------------------------------------------|------------------------------------------|----------------------------------------|
|    データ サイズ                        |    最大 35 TB                 |    デバイスあたり最大 80 TB                       |    デバイスあたり最大 800 TB               |    可変                            |
|    データ型                        |    Azure BLOB                  |    Azure BLOB<br>Azure Files                    |    Azure BLOB<br>Azure Files            |    Azure BLOB<br>Azure Files          |
|    フォーム ファクター                      |    注文ごとに 5 つの SSD             |    1 X 50 lbs。 1 回の注文ごとにデスクトップ サイズのデバイス    |    1 X 最大 500-lbs。 1 回の注文ごとに大容量のデバイス    |    1 回の注文ごとに最大 10 台の HDD/SSD        |
|    初期セットアップ時間               |    低 <br>(15 分)            |    低から中 <br> (30 分未満)               |    中<br>(1 ～ 2 時間)               |    中から難<br>(可変) |
|    データを Azure に送信する               |    [はい]                          |    はい                                           |    はい                                   |    [はい]                                 |
|    Azure からデータをエクスポートする           |    いいえ                            |    いいえ                                             |    いいえ                                     |    [はい]                                 |
|    暗号化                       |    AES 128 ビット                  |    AES 256 ビット                                   |    AES 256 ビット                           |    AES 128 ビット                         |
|    ハードウェア                         |     Microsoft による提供          |    Microsoft による提供                            |    Microsoft による提供                    |    お客様による提供                   |
|    Linux                |    USB 3.1/SATA                 |    RJ 45、SFP+                                   |    RJ45、QSFP+                           |    SATA II/SATA III                    |
|    パートナー統合              |    一部                         |    [高](https://azuremarketplace.microsoft.com/campaigns/databox/azure-data-box)                                          |    [高](https://azuremarketplace.microsoft.com/campaigns/databox/azure-data-box)                                  |    一部                                |
|    発送                         |    Microsoft による管理            |    Microsoft による管理                             |    Microsoft による管理                     |    お客様による管理                    |
| データの移動時に使用する         |コマース境界内|コマース境界内|コマース境界内|地理的境界を越える (US から EU など)|
|    価格                          |    [料金](https://azure.microsoft.com/pricing/details/storage/databox/disk/)                    |   [料金](https://azure.microsoft.com/pricing/details/storage/databox/)                                      |  [料金](https://azure.microsoft.com/pricing/details/storage/databox/heavy/)                               |   [料金](https://azure.microsoft.com/pricing/details/storage-import-export/)                            |


オンラインのデータ転送を使用する場合は、高速ネットワーク帯域幅に対して、次のセクションの表を使用します。

### <a name="high-network-bandwidth"></a>ネットワーク帯域幅 - 高

|                                     |    ツール AzCopy、 <br>Azure PowerShell、 <br>Azure CLI             |    Azure Storage REST API、SDK                   |    Data Box Gateway または Data Box Edge (プレビュー)           |    Azure Data Factory                                            |
|-------------------------------------|------------------------------------|----------------------------------------------|----------------------------------|-----------------------------------------------------------------------|
|    データ型                  |    Azure BLOB、Azure Files、Azure Tables    |    Azure BLOB、Azure Files、Azure Tables    |    Azure BLOB、Azure Files                           |   データ ストアと形式について、70 個を超えるデータ コネクタがサポートされます    |
|    フォーム ファクター                |    コマンドライン ツール                        |    プログラマティック インターフェイス                    |    Microsoft では仮想 <br>または物理デバイスが提供されます     |    Azure ポータルのサービス                                            |
|    最初の 1 回限りのセットアップ     |    簡単               |    中                       |    簡単 (30 分未満) から中 (1 から 2 時間)            |    広範                                                          |
|    データの前処理              |    いいえ                                         |    いいえ                                         |    はい (Edge コンピューティングを使用)                               |    [はい]                                                                |
|    その他のクラウドからの転送       |    いいえ                                         |    いいえ                                         |    いいえ                                                     |    [はい]                                                                |
|    ユーザー タイプ                        |    IT プロフェッショナルまたは開発者                                       |    Dev                                       |    IT プロフェッショナル                                                |    IT プロフェッショナル                                                             |
|    価格                          |    無料、データ エグレス料金を適用         |    無料、データ エグレス料金を適用         |    [料金](https://azure.microsoft.com/pricing/details/storage/databox/edge/)                                               |    [料金](https://azure.microsoft.com/pricing/details/data-factory/)                                                            |

## <a name="next-steps"></a>次の手順

- [Import/Export でデータを転送する方法を学習します](/azure/storage/common/storage-import-export-data-to-blobs)。
- 以下の方法を理解します。

    - [Data Box Edge を使用してデータを転送する](https://docs.microsoft.com/azure/databox/data-box-disk-quickstart-portal)。
    - [Data Box を使用してデータを転送する](https://docs.microsoft.com/azure/databox/data-box-quickstart-portal)。
- [AzCopy を使用してデータを転送する](/azure/storage/common/storage-use-azcopy-v10)。
- 以下の方法を理解します。
    - [Data Box Gateway を使用してデータを転送する](https://docs.microsoft.com/azure/databox-online/data-box-gateway-deploy-add-shares)します。
    - [Azure に送信する前に Data Box Edge を使用してデータを変換する](https://docs.microsoft.com/azure/databox-online/data-box-edge-deploy-configure-compute)。
- [Azure Data Factory を使用してデータを転送する方法を学習します](https://docs.microsoft.com/azure/data-factory/quickstart-create-data-factory-portal)。
- REST API を使用してデータ転送する

    - [.NET の場合](https://docs.microsoft.com/dotnet/api/overview/azure/storage)
    - [Java の場合](https://docs.microsoft.com/java/api/overview/azure/storage/client)