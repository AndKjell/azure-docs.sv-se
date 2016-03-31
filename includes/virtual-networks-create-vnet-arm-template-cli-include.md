## Bereitstellen der ARM-Vorlage mithilfe der Azure-Befehlszeilenschnittstelle

Führen Sie die folgenden Schritte aus, um die ARM-Vorlage bereitzustellen, die Sie mithilfe der Azure-Befehlszeilenschnittstelle heruntergeladen haben.

1. Wenn Sie Azure-Befehlszeilenschnittstelle noch nie verwendet haben, finden Sie unter [Installieren und Konfigurieren der Azure-Befehlszeilenschnittstelle](xplat-cli-install.md) und führen Sie die Schritte bis zu dem Zeitpunkt, in dem Sie Ihr Azure-Konto und Ihr Abonnement auswählen.
2. Führen Sie die **Azure Config Mode** Befehl aus, um die Ressourcen-Manager-Modus wechseln, wie unten dargestellt.

        azure config mode arm

    Hier ist die erwartete Ausgabe des obigen Befehls:

        info:    New mode is arm

3. Führen Sie bei Bedarf die **Azure Gruppe erstellen** eine neue Ressourcengruppe erstellen, wie unten dargestellt. Beachten Sie die Ausgabe des Befehls. In der nach der Ausgabe angezeigten Liste werden die verwendeten Parameter erläutert. Weitere Informationen zu Ressourcengruppen finden Sie auf [Übersicht über Azure Resource Manager](resource-group-overview.md).

        azure group create -n TestRG -l centralus

    Hier ist die erwartete Ausgabe des obigen Befehls:

        info:    Executing command group create
        + Getting resource group TestRG
        + Creating resource group TestRG
        info:    Created resource group TestRG
        data:    Id:                  /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG
        data:    Name:                TestRG
        data:    Location:            centralus
        data:    Provisioning State:  Succeeded
        data:    Tags: null
        data:
        info:    group create command OK

    - **-n (oder--Name)**. Name für die neue Ressourcengruppe. In diesem Szenario *TestRG*.
    - **-l (oder--Location)**. Azure-Region, in der die neue Ressourcengruppe erstellt wird. In diesem Szenario *Centralus*.

4. Führen Sie die **Azure Bereitstellung erstellen** Cmdlet, um das neue VNet bereitstellen, indem Sie mithilfe der Vorlage und Parameter-Dateien heruntergeladen und über geändert. In der nach der Ausgabe angezeigten Liste werden die verwendeten Parameter erläutert.

        azure group deployment create -g TestRG -n TestVNetDeployment -f C:\ARM\azuredeploy.json -e C:\ARM\azuredeploy-parameters.json

    Hier ist die erwartete Ausgabe des obigen Befehls:

        info:    Executing command group deployment create
        + Initializing template configurations and parameters
        + Creating a deployment
        info:    Created template deployment "TestVNetDeployment"
        + Registering providers
        info:    Registering provider microsoft.network
        + Waiting for deployment to complete
        data:    DeploymentName     : TestVNetDeployment
        data:    ResourceGroupName  : TestRG
        data:    ProvisioningState  : Succeeded
        data:    Timestamp          : 2015-08-14T21:56:11.152759Z
        data:    Mode               : Incremental
        data:    Name           Type    Value
        data:    -------------  ------  --------------
        data:    location       String  Central US
        data:    vnetName       String  TestVNet
        data:    addressPrefix  String  192.168.0.0/16
        data:    subnet1Prefix  String  192.168.1.0/24
        data:    subnet1Name    String  FrontEnd
        data:    subnet2Prefix  String  192.168.2.0/24
        data:    subnet2Name    String  BackEnd
        info:    group deployment create command OK

    - **-g (oder--Resource-Group)**. Name der Ressourcengruppe, in der das neue VNet erstellt wird.
    - **-f (oder--Vorlagendatei)**. Pfad zu Ihrer ARM-Vorlagendatei.
    - **-e (oder -Parameter-Datei)**. Pfad zu Ihrer ARM-Parameterdatei.

5. Führen Sie die **Azure Network Vnet Show** Befehl aus, um die Eigenschaften der neuen Vnet anzeigen, wie unten dargestellt.

        azure network vnet show -g TestRG -n TestVNet

    Hier ist die erwartete Ausgabe des obigen Befehls:

        info:    Executing command network vnet show
        + Looking up virtual network "TestVNet"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet
        data:    Name                            : TestVNet
        data:    Type                            : Microsoft.Network/virtualNetworks
        data:    Location                        : centralus
        data:    ProvisioningState               : Succeeded
        data:    Address prefixes:
        data:      192.168.0.0/16
        data:    Subnets:
        data:      Name                          : FrontEnd
        data:      Address prefix                : 192.168.1.0/24
        data:
        data:      Name                          : BackEnd
        data:      Address prefix                : 192.168.2.0/24
        data:
        info:    network vnet show command OK


