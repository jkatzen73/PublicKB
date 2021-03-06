{{{
  "title": "Using a Software-Defined Virtual NAS/SAN on CenturyLink Cloud",
  "date": "11-23-2014",
  "author": "Chris Little",
  "attachments": [],
  "contentIsHTML": false
}}}

Using a Software-Defined Virtual NAS/SAN on CenturyLink Cloud
<p>Customers looking to operate CIFS, NFS or iSCSI services within the cloud platform can import software-defined virtual appliances via a <a href="http://www.centurylinkcloud.com/products/support/service-tasks">service task</a>. By using a <strong>qualified</strong> unified storage virtual appliance customers can deliver NAS based storage services to virtual instances, pool disks to exceed 1 TB disk limits imposed by the cloud platform or deliver legacy shared-storage iSCSI volumes for database clusters. In this knowledge base we will be using a <a href="http://www.softnas.com/">SoftNAS</a>&trade;&nbsp; Virtual Appliance. <a href="http://www.softnas.com/">SoftNAS</a>&trade; is a software-defined unified NAS/SAN storage solution for businesses that need powerful, frictionless and agile storage.</p>
<h3>Supporting Information</h3>
<p>CenturyLink Cloud provides no support for the SoftNAS&trade; virtual appliance. The goal of this KB is to provide a sample use case to deliver unified storage services within the parameters of the cloud platform. Customers are responsible for configuration and sizing of the virtual appliance resources according to the vendors best practices. There are a large number of configuration options/customization's not covered in this KB and customers should read the installation guides provided by SoftNAS&trade;. </p>
<h3>Security Notes</h3>
<p>In this sample deployment no security services were put in place around access to either iSCSI block or filesystem (CIFS, NFS) services. SoftNAS&trade; and the CenturyLink Cloud provide a wide range of authentication and security components customers can and should implement that are outside the scope of a basic functionality overview. </p>

<h3>Prerequisites</h3>

* A CenturyLink Cloud Account
*<a href="http://www.centurylinkcloud.com/products/support/service-tasks">Service Task for OVF Import</a>
* <a href="https://www.softnas.com/wp/purchase/">A licensed copy of SoftNAS&trade;</a>

<h3>Basic Configuration of the Virtual Appliance</h3>

1. Once your SoftNAS virtual appliance is provisioned to the appropriate account via Service Task, login to Control, navigate to the Virtual Appliance, choose edit storage. Provision the appropriate number of RAW disks you wish to pool together into a larger storage pool. In this sample we've chosen to add 4 x 1 TB RAW Disks. NOTE: &nbsp;Understanding the <a href="https://t3n.zendesk.com/entries/21819996-Cloud-Server-Instance-Size-and-Performance">platform maximums</a> by VM type is important. We recommend you import your Virtual Appliance as a Standard VM type if you wish to use up to the 4 TB Maximum.
<img src="https://t3n.zendesk.com/attachments/token/0ZsPgqKIA4veBnI7cAzmrFaLF/?name=01.png" alt="01.png" />
2. Once the RAW disks are added, login to the SoftNAS StorageCenter, select disk devices. The RAW disks added via Control should be present. Quick Tip: &nbsp;use the refresh button if disks don't show up immediately.</p>
<img src="https://t3n.zendesk.com/attachments/token/d4pSont7RJBqUiCxQbOCDBGdK/?name=02.png" alt="02.png" />
3. Select the Partition All button and confirm you wish to create partitions on all the devices.
<img src="https://t3n.zendesk.com/attachments/token/oI56Y7R1zEh96nYcQnATzhnwm/?name=03.png" alt="03.png" />
4. In the SoftNAS StorageCenter navigate to Storage Pools and select the create icon. Provide a friendly Pool name and choose an appropriate level of RAID you wish to use on this group of RAW disks. In this example, we leveraged RAID0, striping as all RAW storage disks provisioned in CenturyLink Cloud already have data protection capabilities in the infrastructure. Select create.
<img src="https://t3n.zendesk.com/attachments/token/IgA5AaqpTL3WWJ3C08ZAkcNIz/?name=04.png" alt="04.png" />
<img src="https://t3n.zendesk.com/attachments/token/IeVWtKfwwueGpZpNw2HF99yGr/?name=05.png" alt="05.png" />
5. If you plan to deliver iSCSI volumes you need to establish an create an iSCSI Target on the SoftNAS. Navigate to iSCSI Lun Targets in the SoftNAS StorageCenter and create a new target. the Default option 'Don't Create new LUN' is adequate as these will be created later.
<img src="https://t3n.zendesk.com/attachments/token/0MXxRdYOWjhr2063hWF2iOrAr/?name=07.5.png" alt="07.5.png" />
6. In the SoftNAS StorageCenter navigate to Volumes and LUNs and select the create icon. The Create Volume dialog box allows you to choose the type of Volume/LUN you wish to deploy. Choices include CIFS, NFS &amp; iSCSI.
<strong>NAS (CIFS or NFS)</strong>
  * Provide a volume name
  * Choose the storage pool previously provisioned
  * Select Filesystem (NFS, CIFS)
  * You can choose to export this volume as NFS, CIFS or Both
  * Choose the storage provisioning options: Thin or Thick
  * Choose the storage optimization options: Compression and/or Deduplication
  * Choose if you wish to leverage snapshots and their schedule.
<img src="https://t3n.zendesk.com/attachments/token/oVVG69HLoCUca5kn0Ad9G4dmX/?name=06.png" alt="06.png" />
<img src="https://t3n.zendesk.com/attachments/token/CQdtxA17BiBtBWDPGrL529GTE/?name=07.png" alt="07.png" />
<strong>iSCSI Block Storage</strong>
* Provide a volume name
* Choose the storage pool previously provisioned
* Select Block Device (iSCSI LUN) &amp; Share as iSCSI LUN
* Choose the storage provisioning options: Thin or Thick
* Choose the storage optimization options: Compression and/or Deduplication
* Choose if you wish to leverage snapshots and their schedule.
* Select the iSCSI LUN Target created in Step #5
<img src="https://t3n.zendesk.com/attachments/token/WMcQ523BCP8rmfJOSH2HAF5m6/?name=08.png" alt="08.png" />
<img src="https://t3n.zendesk.com/attachments/token/uhDPrDHGodnohoDn8djHGxTJo/?name=09.png" alt="09.png" />
7. Once completed the SoftNAS StorageCenter should show the volumes/LUNs you've created.
<img src="https://t3n.zendesk.com/attachments/token/R4OkVbxVxrZAIImKMIYnrdSUe/?name=11.png" alt="11.png" />
###Connecting to NAS or Block Storage on Windows
The examples below are just examples of using a Windows 2012 R2 Virtual Instance to mount a CIFS and iSCSI Volume. Customers should follow standard NFS mount procedures for Linux Instances.
  * If you are leveraging a centralized Active Directory DNS we recommend you add an (A) record so the SoftNAS can be resolved via name or IP.
  * To map a CIFS share use the command line to execute (or Map Network Drive in the Desktop) net use * \\&lt;nasip or name&gt;\&lt;share name&gt;. In this example the command was net use * \\softnas\cifs_share.
<img src="https://t3n.zendesk.com/attachments/token/rsxU1iVZFiVq1Zm9qdWHskBwy/?name=13.png" alt="13.png" />
  * To mount an iSCSI block volume locate the iSCSI initiator (built into Windows 2012, download for 2008 R2) and input the IP address of the SoftNAS virtual appliance, select quickconnect. Select the volumes and devices tab and choose auto configure. Use the disk management tools in Windows to bring online, initialize and format the newly presented iSCSI LUN.
<img src="https://t3n.zendesk.com/attachments/token/Ej2UatZKKxHwufg6a0xCjknlH/?name=14.png" alt="14.png" />
<img src="https://t3n.zendesk.com/attachments/token/VS3faxlm2uGEToqVUTBLJ9N4t/?name=15.png" alt="15.png" />

