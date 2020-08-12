# Performance Tier of Managed Disks in preview

Azure Disks Storage currently offers built-in bursting capabilities to achieve higher performance for handling short-term unexpected traffic. For an event like Black Friday, performance testing, running a training environment, you need to achieve consistently higher performance for a few days or hours and then return to the normal performance levels. On Premium SSDs, you now have the flexibility to increase the disk performance without increasing the disk size and come back to baseline performance, allowing you to match workload performance needs and reduce costs.

A baseline performance tier is set based on your provisioned disk size. You can set a higher performance tier when your application requires this to meet higher demand and return to the initial baseline performance tier once this period is complete. For example, if you provision a P10 disk (128 GiB), your baseline performance tier is set as P10 (500 IOPS and 100 MB/s). You can update the tier to match the performance of P50 (7500 IOPS and 250 MB/s) without increasing the disk size and return to P10 when higher performance is no longer needed. 

| Disk Size | Baseline Performance Tier | Can be upgraded to |
|----------------|-----|-------------------------------------|
| 4 GiB | P1 | P1, P2, P3, P4, P6, P10, P15, P20, P30, P40, P50 |
| 8 GiB | P2 | P2, P3, P4, P6, P10, P15, P20, P30, P40, P50 |
| 16 GiB | P3 | P3, P4, P6, P10, P15, P20, P30, P40, P50 | 
| 32 GiB | P4 | P4, P6, P10, P15, P20, P30, P40, P50 |
| 64 GiB | P6 | P6, P10, P15, P20, P30, P40, P50 |
| 128 GiB | P10 | P10, P15, P20, P30, P40, P50 |
| 256 GiB | P15 | P15, P20, P30, P40, P50 |
| 512 GiB | P20 | P20, P30, P40, P50 |
| 1 TiB | P30 | P30, P40, P50 |
| 2 TiB | P40 | P40, P50 |
| 4 TiB | P50 | P50 |
| 8 TiB | P60 | P60, P70, P80 |
| 16 TiB | P70 | P70, P80 |
| 32 TiB | P80 | P80 |

## Prerequisite

You must get the feature enabled for your subscriptions before you can use performance tier. Please [sign up](https://aka.ms/perftiersignup) for access to our private preview.

## Create/update a data disk with a tier higher than the baseline tier

1. Create an empty data disk with a tier higher than the baseline tier or update the tier of a disk higher than the baseline tier using the sample template [CreateUpdateDataDiskWithTier.json](https://github.com/Azure/azure-managed-disks-performance-tiers/blob/main/CreateUpdateDataDiskWithTier.json)

 ```cli
 subscriptionId=dd80b94e-0463-4a65-8d04-c94f403879dc
 resourceGroupName=perftiertesting
 diskName=myDataDiskwithperftier1
 diskSize=128
 performanceTier=P50
 region=EastUS2EUAP

 az login

 az account set --subscription $subscriptionId

 az group deployment create -g $resourceGroupName \
 --template-uri "https://raw.githubusercontent.com/Azure/azure-managed-disks-performance-tiers/main/CreateUpdateDataDiskWithTier.json" \
 --parameters "region=$region" "diskName=$diskName" "performanceTier=$performanceTier" "dataDiskSizeInGb=$diskSize"
 ```

2. Confirm the tier of the disk
```cli
az resource show -n $diskName -g $resourceGroupName --namespace Microsoft.Compute --resource-type disks --api-version 2020-06-30 --query [properties.tier] -o tsv
 ```
## Create/update a OS disk with a tier higher than the baseline tier

1. Create an OS disk from a marketplace image or update the tier of a OS disk higher than the baseline tier using the sample template [CreateUpdateOSDiskWithTier.json](https://github.com/Azure/azure-managed-disks-performance-tiers/blob/main/CreateUpdateOSDiskWithTier.json)

 ```cli
 resourceGroupName=perftiertesting
 diskName=myOSdiskwithperftier1
 performanceTier=P30
 region=EastUS2EUAP

 az group deployment create -g $resourceGroupName \
 --template-uri "https://raw.githubusercontent.com/Azure/azure-managed-disks-performance-tiers/main/CreateUpdateOSDiskWithTier.json" \
 --parameters "region=$region" "diskName=$diskName" "performanceTier=$performanceTier"
 ```
 
 2. Confirm the tier of the disk
 ```cli
 az resource show -n $diskName -g $resourceGroupName --namespace Microsoft.Compute --resource-type disks --api-version 2020-06-30 --query [properties.tier] -o tsv
 ```
 
## Contributing

This project welcomes contributions and suggestions.  Most contributions require you to agree to a
Contributor License Agreement (CLA) declaring that you have the right to, and actually do, grant us
the rights to use your contribution. For details, visit https://cla.opensource.microsoft.com.

When you submit a pull request, a CLA bot will automatically determine whether you need to provide
a CLA and decorate the PR appropriately (e.g., status check, comment). Simply follow the instructions
provided by the bot. You will only need to do this once across all repos using our CLA.

This project has adopted the [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/).
For more information see the [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) or
contact [opencode@microsoft.com](mailto:opencode@microsoft.com) with any additional questions or comments.

## Legal Notices

Microsoft and any contributors grant you a license to the Microsoft documentation and other content
in this repository under the [Creative Commons Attribution 4.0 International Public License](https://creativecommons.org/licenses/by/4.0/legalcode),
see the [LICENSE](LICENSE) file, and grant you a license to any code in the repository under the [MIT License](https://opensource.org/licenses/MIT), see the
[LICENSE-CODE](LICENSE-CODE) file.

Microsoft, Windows, Microsoft Azure and/or other Microsoft products and services referenced in the documentation
may be either trademarks or registered trademarks of Microsoft in the United States and/or other countries.
The licenses for this project do not grant you rights to use any Microsoft names, logos, or trademarks.
Microsoft's general trademark guidelines can be found at http://go.microsoft.com/fwlink/?LinkID=254653.

Privacy information can be found at https://privacy.microsoft.com/en-us/

Microsoft and any contributors reserve all other rights, whether under their respective copyrights, patents,
or trademarks, whether by implication, estoppel or otherwise.
