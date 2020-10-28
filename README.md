Add a new key to a Key Vault
============================

            

This runbook shows how to add a new key to an existing Key Vault. It requires an Azure Run As account and additional modules, see the PREREQUISITE sections in the runbook for details. The runbook will use the Run As account to authenticate against Key
 Vault. The Key Vault's access policy needs to allow adding new keys. If successful, the runbook output will display information about the new key.





PowerShell
Edit|Remove
powershell
<#
.SYNOPSIS
  This runbook will create a key in the given key vault.
  
.DESCRIPTION
  This runbook will create a key in the given key vault.

  PREREQUISITE
  The automation account has to be created with a 'Run As' account (Service Principal). Automation accounts with a run as account 
  can be identified by verifying that they contain the two assets: 'AzureRunAsConnection' and 'AzureRunAsCertificate'. 
  See https://azure.microsoft.com/en-us/documentation/articles/automation-sec-configure-azure-runas-account/ for more information.

  PREREQUISITE
  The modules 'AzureRM.profile' and 'AzureRM.keyvault' have to be imported into the automation account. They can be imported from
  the Module Gallery. See https://azure.microsoft.com/en-us/blog/announcing-azure-resource-manager-support-azure-automation-runbooks/
  for more information.

.PARAMETER VaultName 
  Mandatory
  This is the name of the key vault.
  
.PARAMETER ResourceGroupName 
  Mandatory
  This is the name of the resource group that the key vault is in.
  
.PARAMETER KeyName  
  Mandatory
  This is the name of the key.
  
.NOTES
  AUTHOR: Andreas Wieberneit
  LASTEDIT: April 15, 2016
#>

param
(
	[Parameter (Mandatory=$true)]
	[string] $VaultName,
	[Parameter (Mandatory=$true)]
	[string] $ResourceGroupName,
	[Parameter (Mandatory=$true)]
	[string] $KeyName
)

# Authenticate to Azure with service principal and certificate, and set subscription
$connectionAssetName = 'AzureRunAsConnection'
$conn = Get-AutomationConnection -Name $ConnectionAssetName
if ($conn -eq $null)
{
	throw 'Could not retrieve $connectionAssetName connection asset. Check that this asset exists in the automation account.'
}
Add-AzureRmAccount -ServicePrincipal -Tenant $conn.TenantID -ApplicationId $conn.ApplicationId -CertificateThumbprint $conn.CertificateThumbprint -ErrorAction Stop | Write-Verbose
Set-AzureRmContext -SubscriptionId $conn.SubscriptionId -ErrorAction Stop | Write-Verbose

# Try to retrieve the key vault.
$keyVault = Get-AzureRMKeyVault -VaultName $VaultName -ResourceGroupName $ResourceGroupName 
if ($keyVault -eq $null)
{
	throw 'Could not retrieve key vault $VaultName. Check that a key vault with this name exists in the resource group $ResourceGroupName.'
}

# Try to create the key.
$key = Add-AzureKeyVaultKey -VaultName $keyVault[0].VaultName -Name $KeyName -Destination 'Software' 
if ($key -eq $null)
{
	throw 'Could not create key $KeyName.'
}


<# 
.SYNOPSIS 
  This runbook will create a key in the given key vault. 
   
.DESCRIPTION 
  This runbook will create a key in the given key vault. 
 
  PREREQUISITE 
  The automation account has to be created with a 'Run As' account (Service Principal). Automation accounts with a run as account  
  can be identified by verifying that they contain the two assets: 'AzureRunAsConnection' and 'AzureRunAsCertificate'.  
  See https://azure.microsoft.com/en-us/documentation/articles/automation-sec-configure-azure-runas-account/ for more information. 
 
  PREREQUISITE 
  The modules 'AzureRM.profile' and 'AzureRM.keyvault' have to be imported into the automation account. They can be imported from 
  the Module Gallery. See https://azure.microsoft.com/en-us/blog/announcing-azure-resource-manager-support-azure-automation-runbooks/ 
  for more information. 
 
.PARAMETER VaultName  
  Mandatory 
  This is the name of the key vault. 
   
.PARAMETER ResourceGroupName  
  Mandatory 
  This is the name of the resource group that the key vault is in. 
   
.PARAMETER KeyName   
  Mandatory 
  This is the name of the key. 
   
.NOTES 
  AUTHOR: Andreas Wieberneit 
  LASTEDIT: April 15, 2016 
#> 
 
param 
( 
    [Parameter (Mandatory=$true)] 
    [string] $VaultName, 
    [Parameter (Mandatory=$true)] 
    [string] $ResourceGroupName, 
    [Parameter (Mandatory=$true)] 
    [string] $KeyName 
) 
 
# Authenticate to Azure with service principal and certificate, and set subscription 
$connectionAssetName = 'AzureRunAsConnection' 
$conn = Get-AutomationConnection -Name $ConnectionAssetName 
if ($conn -eq $null) 
{ 
    throw 'Could not retrieve $connectionAssetName connection asset. Check that this asset exists in the automation account.' 
} 
Add-AzureRmAccount -ServicePrincipal -Tenant $conn.TenantID -ApplicationId $conn.ApplicationId -CertificateThumbprint $conn.CertificateThumbprint -ErrorAction Stop | Write-Verbose 
Set-AzureRmContext -SubscriptionId $conn.SubscriptionId -ErrorAction Stop | Write-Verbose 
 
# Try to retrieve the key vault. 
$keyVault = Get-AzureRMKeyVault -VaultName $VaultName -ResourceGroupName $ResourceGroupName  
if ($keyVault -eq $null) 
{ 
    throw 'Could not retrieve key vault $VaultName. Check that a key vault with this name exists in the resource group $ResourceGroupName.' 
} 
 
# Try to create the key. 
$key = Add-AzureKeyVaultKey -VaultName $keyVault[0].VaultName -Name $KeyName -Destination 'Software'  
if ($key -eq $null) 
{ 
    throw 'Could not create key $KeyName.' 
} 







        
    
TechNet gallery is retiring! This script was migrated from TechNet script center to GitHub by Microsoft Azure Automation product group. All the Script Center fields like Rating, RatingCount and DownloadCount have been carried over to Github as-is for the migrated scripts only. Note : The Script Center fields will not be applicable for the new repositories created in Github & hence those fields will not show up for new Github repositories.
