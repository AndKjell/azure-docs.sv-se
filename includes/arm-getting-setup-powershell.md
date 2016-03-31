## Einrichten von PowerShell für Ressourcen-Manager-Vorlagen

Bevor Sie Azure PowerShell mit dem Ressourcen-Manager verwenden können, benötigen Sie die richtigen Power Shell- und Azure PowerShell-Versionen.

### Überprüfen der PowerShell-Versionen

Überprüfen Sie, ob Sie über die PowerShell-Version 3.0 oder 4.0 verfügen. Um die Version von Windows PowerShell zu finden, geben Sie diesen Befehl in eine Windows PowerShell-Eingabeaufforderung ein.

    $PSVersionTable

Sie erhalten den folgenden Informationstyp:

    Name                           Value
    ----                           -----
    PSVersion                      3.0
    WSManStackVersion              3.0
    SerializationVersion           1.1.0.1
    CLRVersion                     4.0.30319.18444
    BuildVersion                   6.2.9200.16481
    PSCompatibleVersions           {1.0, 2.0, 3.0}
    PSRemotingProtocolVersion      2.2


Überprüfen Sie, ob der Wert der **PSVersion** 3.0 oder 4.0. Falls nicht, finden Sie unter [Windows Management Framework 3.0](http://www.microsoft.com/download/details.aspx?id=34595) oder [Windows Management Framework 4.0](http://www.microsoft.com/download/details.aspx?id=40855).

### Festlegen Ihres Azure-Kontos und -Abonnements

Wenn Sie bereits über ein Azure-Abonnement haben, können Sie aktivieren die [Vorteile für MSDN-Abonnenten](http://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) oder registrieren Sie sich für eine [kostenlose Testversion](http://azure.microsoft.com/pricing/free-trial/).

Öffnen Sie eine Azure PowerShell-Eingabeaufforderung und melden Sie sich mit diesem Befehl bei Azure an.

    Login-AzureRmAccount

Wenn Sie mehrere Azure-Abonnements haben, können Sie Ihre Azure-Abonnements mit diesem Befehl auflisten.

    Get-AzureRmSubscription

Sie erhalten den folgenden Informationstyp:

    SubscriptionId            : fd22919d-eaca-4f2b-841a-e4ac6770g92e
    SubscriptionName          : Visual Studio Ultimate with MSDN
    Environment               : AzureCloud
    SupportedModes            : AzureServiceManagement,AzureResourceManager
    DefaultAccount            : johndoe@contoso.com
    Accounts                  : {johndoe@contoso.com}
    IsDefault                 : True
    IsCurrent                 : True
    CurrentStorageAccountName :
    TenantId                  : 32fa88b4-86f1-419f-93ab-2d7ce016dba7

Sie können das aktuelle Azure-Abonnement durch Ausführen dieser Befehle in der Azure PowerShell-Eingabeaufforderung festlegen. Ersetzen Sie alles innerhalb der Anführungszeichen, einschließlich der Zeichen, mit dem richtigen Namen < und >.

    $subscr="<SubscriptionName from the display of Get-AzureRmSubscription>"
    Select-AzureRmSubscription -SubscriptionName $subscr -Current

Weitere Informationen zu Azure-Abonnements und-Konten finden Sie unter [Gewusst wie: Verbinden mit Ihrem Abonnement](powershell-install-configure.md#Connect).


