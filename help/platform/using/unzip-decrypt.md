---
solution: Campaign Classic
product: campaign
title: Unzipping or decrypting a file
description: Learn how to unzip or decrypt a file in Campaign Classic before processing.
audience: platform
content-type: reference
topic-tags: importing-and-exporting-data
---

# Unzipping or decrypting a file {#unzipping-or-decrypting-a-file-before-processing}

Adobe Campaign lets you import zipped or encrypted files. Before they can be read in a [Data loading (file)](../../workflow/using/data-loading--file-.md) activity, you can define a pre-processing to unzip or to decrypt the file.

To be able to do so:

1. Use the [Control Panel](https://docs.adobe.com/content/help/en/control-panel/using/instances-settings/gpg-keys-management.html#decrypting-data) to generate a public/private key pair.

    >[!NOTE]
    >
    >Control Panel is available to all customers hosted on AWS (excepted for customers who host their marketing instances on premise).

1. If your installation of Adobe Campaign is hosted by Adobe, contact [Adobe Customer Care](https://helpx.adobe.com/enterprise/admin-guide.html/enterprise/using/support-for-experience-cloud.ug.html) to have the necessary utilities installed on the server.
1. If your installation of Adobe Campaign is on premise, install the utility you want to use (for example: GPG, GZIP) as well as the necessary keys (encryption key) on the application server.

You can then use the desired pre-processing commands into your workflows:

1. Add and configure a **[!UICONTROL File transfer]** activity in your workflow.
1. Add a **[!UICONTROL Data loading (file)]** activity and define the file format.
1. Check the **[!UICONTROL Pre-process the file]** option.
1. Specify the pre-processing command you want to apply.
1. Add other activities to manage data coming from the file.
1. Save and execute your workflow.

An example is presented in the use case below.

**Related topics:**

* [Data loading (file) activity](../../workflow/using/data-loading--file-.md).
* [Zipping or encrypting a file](../../workflow/using/how-to-use-workflow-data.md#zipping-or-encrypting-a-file).

## Use case: Importing data encrypted using a key generated by Control Panel {#use-case-gpg-decrypt}

In this use case, we will build a workflow in order to import data that has been encrypted in an external system, using a key generated in the Control Panel.

![](assets/do-not-localize/how-to-video.png) [Discover this feature in video](#video)

The steps to perform this use case are as follows:

1. Use the Control Panel to generate a key pair (public/private). Detailed steps are available in [Control Panel documentation](https://docs.adobe.com/content/help/en/control-panel/using/instances-settings/gpg-keys-management.html#decrypting-data).

    * The public key will be shared with the external system, which will use it to  encrypt the data to send to Campaign.
    * The private key will be used by Campaign Classic to decrypt the incoming encrypted data.

    ![](assets/gpg_generate.png)

1. In the external system, use the public key downloaded from the Control Panel to encrypt the data to import into Campaign Classic.

1. In Campaign Classic, build a workflow to import the encrypted data and decrypt it using the private key that has been installed via the Control Panel. To do this, we will build a workflow as follows:

     ![](assets/gpg_import_workflow.png)

    * **[!UICONTROL File transfer]** activity: Transfers the file from an external source to Campaign Classic. In this example, we want transfer the file from an SFTP server.
    * **[!UICONTROL Data loading (file)]** activity: Loads the data from the file into the database and decrypt it using the private key generated in the Control Panel.

1. Open the **[!UICONTROL File transfer]** activity then specify the external account from which you want to import the encrypted .gpg file.

     ![](assets/gpg_key_transfer.png)

     Global concepts on how to configure the activity are available in [this section](../../workflow/using/file-transfer.md).

1. Open the **[!UICONTROL Data loading (file)]** activity, then configure it according to your needs. Global concepts on how to configure the activity are available in [this section](../../workflow/using/data-loading--file-.md).

    Add a pre-processing stage to the activity, in order to decrypt the incoming data. To do this, select the **[!UICONTROL Pre-process the file]** option, then copy-paste this decryption command in the **[!UICONTROL Command]** field :

    `gpg --batch --passphrase passphrase --decrypt <%=vars.filename%>`

     ![](assets/gpg_load.png)

    >[!CAUTION]
    >
    >In this example, we are using the passphrase used by default by Control Panel, which is "passphrase".
    >
    >If you have already had GPG keys installed on your instance through a Customer Care request in the past, the passphrase may have been changed and be different form the one by default.

1. Click **[!UICONTROL OK]** to confirm the activity configuration.

1. You can now run the workflow. Once it is executed, you can check in the workflow logs that the decryption has been executed, and that data from the file have been imported.

    ![](assets/gpg_run.png)

## Tutorial video {#video}

This video shows how to use a GPG key to decrypt data.

>[!VIDEO](https://video.tv.adobe.com/v/36482?quality=12)

Additional Campaign Classic how-to videos are available [here](https://experienceleague.adobe.com/docs/campaign-classic-learn/tutorials/overview.html).