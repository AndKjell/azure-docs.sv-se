
## <a name="start-your-powershell-session"></a>Starta din PowerShell-session
Först måste du ha senast [Azure PowerShell](http://msdn.microsoft.com/library/mt619274.aspx) installerade och körs. Mer information finns i [Så här installerar och konfigurerar du Azure PowerShell](/powershell/azureps-cmdlets-docs).

> [!NOTE]
> Exemplen i det här avsnittet använder [Azure Resource Manager-distributionsmodellen](../articles/azure-resource-manager/resource-group-overview.md), så exemplen använder den [Azure Resource Manager-cmdletar](http://msdn.microsoft.com/library/azure/mt125356.aspx). 
> 
> 

Kör den [ **Connect-AzureRmAccount** ](https://docs.microsoft.com/powershell/module/azurerm.profile/connect-azurermaccount) cmdlet och du kommer att visas en skärm att ange dina autentiseringsuppgifter. Använd samma autentiseringsuppgifter som du använder för att logga in på Azure Portal.

    Connect-AzureRmAccount

Om du har flera prenumerationer använder den [ **Set-AzureRmContext** ](https://docs.microsoft.com/powershell/module/azurerm.profile/set-azurermcontext) cmdlet för att välja vilken prenumeration som din PowerShell-session ska använda. För att se vilken prenumeration den nuvarande PowerShell-sessionen använder kör du cmdleten [**Get-AzureRmContext**](https://docs.microsoft.com/powershell/module/azurerm.profile/get-azurermcontext). För att se alla dina prenumerationer kör du [**Get-AzureRmSubscription**](https://docs.microsoft.com/powershell/module/servicemanagement/azurerm.profile/get-azurermsubscription).

    Set-AzureRmContext -SubscriptionId '4cac86b0-1e56-bbbb-aaaa-000000000000'

