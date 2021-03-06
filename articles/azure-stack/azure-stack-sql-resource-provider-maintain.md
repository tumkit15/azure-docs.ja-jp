---
title: Azure Stack 上の SQL リソース プロバイダーの保守 | Microsoft Docs
description: Azure Stack 上の SQL リソース プロバイダー サービスを保守する方法について説明します。
services: azure-stack
documentationCenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/16/2018
ms.author: jeffgilb
ms.reviewer: quying
ms.openlocfilehash: 360661402289ab9b06eb01be447dc98942c93302
ms.sourcegitcommit: 6361a3d20ac1b902d22119b640909c3a002185b3
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/17/2018
ms.locfileid: "49364097"
---
# <a name="sql-resource-provider-maintenance-operations"></a>SQL リソース プロバイダーの保守操作

SQL リソース プロバイダーは、ロック ダウンされた仮想マシン上で実行されます。 保守操作を有効にするには、仮想マシンのセキュリティを更新する必要があります。 最小権限の原則を使用してそれを行うには、[PowerShell Just Enough Administration (JEA)](https://docs.microsoft.com/powershell/jea/overview) エンドポイント *DBAdapterMaintenance* を使用できます。 リソース プロバイダーのインストール パッケージには、この操作のためのスクリプトが含まれています。

## <a name="patching-and-updating"></a>修正プログラム適用と更新

SQL リソース プロバイダーはアドオン コンポーネントであるため、Azure Stack の一部としては提供されません。 必要に応じて、SQL リソース プロバイダーの更新プログラムが提供されます。 更新された SQL アダプターがリリースされると、更新プログラムを適用するためのスクリプトが提供されます。 このスクリプトによって新しいリソース プロバイダー VM が作成され、古い方のプロバイダーの状態が新しい方の VM に移行されます。 詳細については、「[SQL リソース プロバイダーの更新](azure-stack-sql-resource-provider-update.md)」を参照してください。

### <a name="provider-virtual-machine"></a>プロバイダー仮想マシン

リソース プロバイダーは "*ユーザー*" 仮想マシン上で実行されているので、必要な修正プログラムおよび更新プログラムがリリースされたら、それらを適用する必要があります。 修正プログラム適用と更新のサイクルの一環で提供される Windows 更新プログラム パッケージを使用して、VM に更新プログラムを適用できます。

## <a name="backuprestoredisaster-recovery"></a>バックアップ/復元/ディザスター リカバリー

 SQL リソース プロバイダーはアドオン コンポーネントであるため、Azure Stack ビジネス継続性ディザスター リカバリー (BCDR) プロセスの一環として、そのバックアップは行われません。 次の操作に対してスクリプトが提供されます。

- 状態情報のバックアップ (Azure Stack ストレージ アカウントに格納されている)。
- スタックをすべて回復する必要がある場合のリソース プロバイダーの復元。

>[!NOTE]
>回復を行う必要がある場合は、データベース サーバーを回復してから、リソース プロバイダーを復元する必要があります。

## <a name="updating-sql-credentials"></a>SQL 資格情報の更新

管理者は、SQL Server の sysadmin アカウントの作成と保守を担当します。 リソース プロバイダーには、ユーザーに代わってデータベースを管理するためにこれらの権限があるアカウントが必要となりますが、ユーザーのデータにアクセスする必要はありません。 SQL Server で sysadmin パスワードを更新する必要がある場合、リソース プロバイダーの管理者インターフェイスを使用して、保存済みパスワードを変更できます。 これらのパスワードは、Azure Stack インスタンス上の Key Vault に格納されています。

設定を変更するには、**[参照]** &gt; **[管理リソース]** &gt; **[SQL ホスティング サーバー]** &gt; **[SQL ログイン]** の順に選択し、ユーザー名を選択します。 変更は、最初に SQL インスタンス (および必要な場合はレプリカ) で行う必要があります。**[設定]** で **[パスワード]** を選択します。

![管理パスワードの更新](./media/azure-stack-sql-rp-deploy/sqlrp-update-password.PNG)

## <a name="secrets-rotation"></a>シークレットのローテーション

*この説明は、Azure Stack 統合システムに対してのみ適用されます。*

Azure Stack 統合システムで SQL および MySQL リソース プロバイダーを使用する場合、以下のリソース プロバイダーのインフラストラクチャ シークレットが期限切れにならないようにローテーションする責任は Azure Stack オペレーターにあります。

- [デプロイ時に提供](azure-stack-pki-certs.md)された外部 SSL 証明書。
- デプロイ時に提供されたリソース プロバイダー VM のローカル管理者アカウントのパスワード。
- リソース プロバイダーの診断ユーザー (dbadapterdiag) のパスワード。

### <a name="powershell-examples-for-rotating-secrets"></a>PowerShell のシークレットのローテーション例

**すべてのシークレットを同時に変更する。**

```powershell
.\SecretRotationSQLProvider.ps1 `
    -Privilegedendpoint $Privilegedendpoint `
    -CloudAdminCredential $cloudCreds `
    -AzCredential $adminCreds `
    –DiagnosticsUserPassword $passwd `
    -DependencyFilesLocalPath $certPath `
    -DefaultSSLCertificatePassword $certPasswd  `
    -VMLocalCredential $localCreds
```

**診断ユーザーのパスワードを変更する。**

```powershell
.\SecretRotationSQLProvider.ps1 `
    -Privilegedendpoint $Privilegedendpoint `
    -CloudAdminCredential $cloudCreds `
    -AzCredential $adminCreds `
    –DiagnosticsUserPassword  $passwd
```

**VM ローカル管理者アカウントのパスワードを変更する。**

```powershell
.\SecretRotationSQLProvider.ps1 `
    -Privilegedendpoint $Privilegedendpoint `
    -CloudAdminCredential $cloudCreds `
    -AzCredential $adminCreds `
    -VMLocalCredential $localCreds
```

**SSL 証明書のパスワードを変更する。**

```powershell
.\SecretRotationSQLProvider.ps1 `
    -Privilegedendpoint $Privilegedendpoint `
    -CloudAdminCredential $cloudCreds `
    -AzCredential $adminCreds `
    -DependencyFilesLocalPath $certPath `
    -DefaultSSLCertificatePassword $certPasswd
```

### <a name="secretrotationsqlproviderps1-parameters"></a>SecretRotationSQLProvider.ps1 のパラメーター

|パラメーター|説明|
|-----|-----|
|AzCredential|Azure Stack サービス管理者アカウントの資格情報。|
|CloudAdminCredential|Azure Stack クラウド管理者ドメイン アカウントの資格情報。|
|PrivilegedEndpoint|Get-AzureStackStampInformation にアクセスするための特権エンドポイント。|
|DiagnosticsUserPassword|診断ユーザー アカウントのパスワード。|
|VMLocalCredential|MySQLAdapter VM でのローカル管理者アカウント。|
|DefaultSSLCertificatePassword|既定の SSL 証明書 (* pfx) のパスワード。|
|DependencyFilesLocalPath|依存関係ファイルのローカル パス。|
|     |     |

### <a name="known-issues"></a>既知の問題

**問題**: シークレット ローテーション ログ。<br>
シークレット ローテーションのカスタム スクリプトが実行され、失敗した場合、シークレット ローテーションのログは自動的に収集されません。

**対処法**: <br>
Get-AzsDBAdapterLogs コマンドレットを使用して、C:\Logs に保存されているすべてのリソース プロバイダーのログ (AzureStack.DatabaseAdapter.SecretRotation.ps1_*.log など) を収集します。

## <a name="update-the-virtual-machine-operating-system"></a>仮想マシンのオペレーティング システムの更新

仮想マシンのオペレーティング システムを更新するには、次のいずれかの方法を使用します。

- 現在パッチが適用されている Windows Server 2016 Core イメージを使用して最新のリソース プロバイダーのパッケージをインストールする。
- リソース プロバイダーのインストールまたは更新中に Windows 更新プログラム パッケージをインストールする。

## <a name="update-the-virtual-machine-windows-defender-definitions"></a>仮想マシンの Windows Defender の定義を更新する

Windows Defender の定義を更新するには: 

1. [Windows Defender の定義](https://www.microsoft.com/en-us/wdsi/definitions)から Windows Defender 定義の更新をダウンロードします。

   定義の更新に関するページ上で下方にスクロールして「Manually download and install the definitions」 (定義を手動でダウンロードしてインストールする) を見つけます。 "Windows Defender Antivirus for Windows 10 および Windows 8.1" 64 ビット ファイルをダウンロードします。

   あるいは、[こちらのダイレクト リンク](https://go.microsoft.com/fwlink/?LinkID=121721&arch=x64)を使用して、fpam-fe.exe ファイルをダウンロードして実行します。

2. SQL リソース プロバイダー アダプター仮想マシンのメンテナンス エンドポイントに対し PowerShell セッションを作成します。

3. メンテナンス エンドポイントのセッションを使用して仮想マシンに定義更新ファイルをコピーします。

4. メンテナンス PowerShell セッションで *Update-DBAdapterWindowsDefenderDefinitions* コマンドを実行します。

5. 定義をインストールしたら、*Remove-ItemOnUserDrive* コマンドを使用することにより、定義更新ファイルを削除することをお勧めします。

**定義を更新するための PowerShell スクリプトの例**

次のスクリプトを編集して実行することにより、Defender の定義を更新することができます。 スクリプト内の値を、実際の環境の値に置き換えます。

```powershell
# Set credentials for local admin on the resource provider VM.
$vmLocalAdminPass = ConvertTo-SecureString "<local admin user password>" -AsPlainText -Force
$vmLocalAdminUser = "<local admin user name>"
$vmLocalAdminCreds = New-Object System.Management.Automation.PSCredential `
    ($vmLocalAdminUser, $vmLocalAdminPass)

# Provide the public IP address for the adapter VM.
$databaseRPMachine  = "<RP VM IP address>"
$localPathToDefenderUpdate = "C:\DefenderUpdates\mpam-fe.exe"

# Download the Windows Defender update definitions file from https://www.microsoft.com/en-us/wdsi/definitions.
Invoke-WebRequest -Uri 'https://go.microsoft.com/fwlink/?LinkID=121721&arch=x64' `
    -Outfile $localPathToDefenderUpdate

# Create a session to the maintenance endpoint.
$session = New-PSSession -ComputerName $databaseRPMachine `
    -Credential $vmLocalAdminCreds -ConfigurationName DBAdapterMaintenance
# Copy the defender update file to the adapter virtual machine.
Copy-Item -ToSession $session -Path $localPathToDefenderUpdate `
     -Destination "User:\"
# Install the update definitions.
Invoke-Command -Session $session -ScriptBlock `
    {Update-AzSDBAdapterWindowsDefenderDefinition -DefinitionsUpdatePackageFile "User:\mpam-fe.exe"}
# Cleanup the definitions package file and session.
Invoke-Command -Session $session -ScriptBlock `
    {Remove-AzSItemOnUserDrive -ItemPath "User:\mpam-fe.exe"}
$session | Remove-PSSession
```

## <a name="collect-diagnostic-logs"></a>診断ログの収集

ロックダウンされた仮想マシンからログを収集するには、PowerShell Just Enough Administration (JEA) エンドポイント *DBAdapterDiagnostics* を使用します。 このエンドポイントでは、次のコマンドが提供されます。

- **Get-AzsDBAdapterLog**。 このコマンドは、リソース プロバイダー診断ログの zip パッケージを作成し、そのファイルをセッションのユーザー ドライブ上に保存します。 このコマンドはパラメーターなしで実行することができ、過去 4 時間のログが収集されます。
- **Remove-AzsDBAdapterLog**。 このコマンドは、リソース プロバイダー VM に対する既存のログ パッケージを削除します。

### <a name="endpoint-requirements-and-process"></a>エンドポイントの要件とプロセス

リソース プロバイダーをインストールまたは更新すると、**dbadapterdiag** ユーザー アカウントが作成されます。 このアカウントを使用することで、診断ログを収集できます。

>[!NOTE]
>dbadapterdiag アカウント パスワードは、プロバイダーのデプロイ中または更新中に作成される仮想マシンのローカル管理者用に使用されるパスワードと同じです。

*DBAdapterDiagnostics* コマンドを使用するには、リソース プロバイダーの仮想マシンに対するリモート PowerShell セッションを作成し、**Get-AzsDBAdapterLog** コマンドを実行します。

**FromDate** および **ToDate** パラメーターを使用して、ログ収集の期間を設定します。 これらのパラメーターの一方または両方を指定しない場合は、次の既定値が使用されます。

- FromDate は現在の時刻の 4 時間前です。
- ToDate は現在の時刻です。

**ログを収集する PowerShell スクリプトの例**

次のスクリプトでは、リソース プロバイダー VM から診断ログを収集する方法を示します。

```powershell
# Create a new diagnostics endpoint session.
$databaseRPMachineIP = '<RP VM IP address>'
$diagnosticsUserName = 'dbadapterdiag'
$diagnosticsUserPassword = '<Enter Diagnostic password>'

$diagCreds = New-Object System.Management.Automation.PSCredential `
        ($diagnosticsUserName, (ConvertTo-SecureString -String $diagnosticsUserPassword -AsPlainText -Force))
$session = New-PSSession -ComputerName $databaseRPMachineIP -Credential $diagCreds `
        -ConfigurationName DBAdapterDiagnostics

# Sample that captures logs from the previous hour.
$fromDate = (Get-Date).AddHours(-1)
$dateNow = Get-Date
$sb = {param($d1,$d2) Get-AzSDBAdapterLog -FromDate $d1 -ToDate $d2}
$logs = Invoke-Command -Session $session -ScriptBlock $sb -ArgumentList $fromDate,$dateNow

# Copy the logs to the user drive.
$sourcePath = "User:\{0}" -f $logs
$destinationPackage = Join-Path -Path (Convert-Path '.') -ChildPath $logs
Copy-Item -FromSession $session -Path $sourcePath -Destination $destinationPackage

# Cleanup the logs.
$cleanup = Invoke-Command -Session $session -ScriptBlock {Remove- AzsDBAdapterLog }
# Close the session.
$session | Remove-PSSession
```

## <a name="next-steps"></a>次の手順

[サーバーをホストする SQL Server を追加する](azure-stack-sql-resource-provider-hosting-servers.md)
