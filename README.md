### Generate a zip file to migrate product or products

![](https://img.shields.io/github/stars/pandao/editor.md.svg) ![](https://img.shields.io/github/forks/pandao/editor.md.svg) ![](https://img.shields.io/github/tag/pandao/editor.md.svg) ![](https://img.shields.io/github/release/pandao/editor.md.svg) ![](https://img.shields.io/github/issues/pandao/editor.md.svg) ![](https://img.shields.io/bower/v/editor.md.svg)

This repository uses a pipeline that works with the ExportCatalog pipelet and a script that uses salesforce commerce cloud classes, which through a job executes and exports a product or several products, depending on the parameters passed to it, in .xml format. This file is generated in the IMPEX directory, with an special structure, and also generate the inventory list and pricebooks with the same structure in the same folder. This folder is copy into a special folder in the IMPEX too, like this: src/instance and is compress in a .zip folder. In this way, you can reach it from other instance from:

Administration > Site Development > Site Import & Export --> Remote 

![](https://i.ibb.co/K60vKLQ/migration-File.png)

![](https://i.ibb.co/yhRwqJB/Create-the-structure-for-copy-into-instance-folder-in-a-zip-file.png)

# Cartridge Path Considerations
bc_importexport cartridge requires the app_storefront_base cartridge or SFRA Architecture.

Log in to Business Manager.
Go to Administration > Sites > Manage Sites.
Select the Business Manager Site to use bc_importexport.
![](https://i.ibb.co/W3jzqPp/Business-Manager-Site.png)

In the Cartridges field, add the new cartridge path: bc_importexport. It must be added before the path for app_storefront_base. Example path: bc_importexport:app_storefront_base

# Get Started
### Requirements
#### Install and Upload the bc_importexport Cartridge

Clone this repository. The name of the top-level directory bc_importexport and also you need the bc_job_components cartridge to allow to zip the file.

From the bc_importexport directory, run npm install. This command installs all of the package dependencies.

Create a dw.json file in *bc_importexport** directory. Replace the PLACEHOLDER_ strings with actual values.

{
    "hostname": "PLACEHOLDER_HOSTNAME.demandware.net",
    "username": "PLACEHOLDER_USERNAME",
    "password": "PLACEHOLDER_PASSWORD",
    "code-version": "PLACEHOLDER_VERSION"
}
From the bc_importexport directory, npm run uploadCartridge.

For more information on uploading the cartridge, see the following topic on the B2C Commerce Infocenter: [Upload Code for SFRA](https://documentation.b2c.commercecloud.salesforce.com/DOC2/index.jsp?topic=%2Fcom.demandware.dochelp%2Fcontent%2Fb2c_commerce%2Ftopics%2Fsfra%2Fb2c_uploading_code.html "Upload Code for SFRA").

# Create Job
Log in to Business Manager.
Go to Administration > Operations > Jobs.
![](https://i.ibb.co/0r3W0cG/Create-New-Job.png)

- And then call the pipeline that we create in this repository in tag Job Steps,  like this:
![](https://i.ibb.co/ctR9DNG/Configuration-Steps.png)

- To Execute our pipeline we need to pass the pipeline's name follow by a hippen and how start our pipeline. In our case is: **ExportProductsByIdOrByCategory-ExportProduct**. It is like our controller name follow by our url in the SFRA.

- If you pass the pid parameter you can search for a particular product, instead if you pass categoryId parameter, you can search for all the products in that specific category. You must to pass one or other, no both. (pid, categoryId). The other parameter is catalogId that is necessary to use this ExportCatalog Pipelet and for categoryId search you need to use the boilerplate-master-catalog because the class ProductSearchModel just work whit this catalog.

- You need to do another 3 steps to export the inventory, and pricebook data with his pipelets ExportInventoryLists and ExportPriceBook, and the last step is to remove that files created and move it in a zip file (with the structure that needs the arquitecture) to a shared folder in the WebDav call instance, and we create the zip folder call migration.zip. With this zip folder in this share folder, you can import remotely in the sandbox instance you want, but just in the SIG.  

The job steps should look like this:

![](https://i.ibb.co/hRh18GX/job-Migration.png)

- Then run the job and see the IMPEX/src/instance directory in the webdav to see our file migration.zip, and use this product information in the instance you need it.

# If you want to use Eclipse IDE
- Your need to instal this version [Neon 4.6](https://www.eclipse.org/downloads/packages/release/neon/3 "Neon 4.6")
 You can see how to configure in this link: [Configure Eclipse IDE](https://confluence.ontrq.com/pages/viewpage.action?spaceKey=ACDC&title=Lab%3A+Eclipse+IDE+Setup "Configure Eclipse IDE")
 
 And also your going to need this aditional configuration: ![](https://i.ibb.co/1zxD5cg/Preference-Network-Connections-Direct.png)
- Install JDK 8 because is need it, for use a plug-in (Create a jira ticket for this).
- Install  UX Studio plug-in [Install UX Studio](https://documentation.b2c.commercecloud.salesforce.com/DOC1/index.jsp?topic=%2Fcom.demandware.dochelp%2FLegacyDevDoc%2FInstallUXStudio.html&resultof=%22%45%63%6c%69%70%73%65%22%20%22%65%63%6c%69%70%73%22%20 "Install UX Studio")

- After complete the Server Connection and import our cartridge, you can see our pipeline in grafic mode, and also the script that is part of this flow.
- This is how it see our pipeline **ExportProductsByIdOrByCategory.xml**

![](https://i.ibb.co/1qWkRMh/pipeline-Export-Products-By-Id-Or-By-Category-xml.png)
