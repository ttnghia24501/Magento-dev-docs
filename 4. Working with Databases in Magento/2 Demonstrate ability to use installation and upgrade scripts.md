# Demonstrate ability to use installation and upgrade scripts

## Describe the install/upgrade workflow. Where are setup scripts located, and how are they executed?
##### Running setup scripts


Use CLI command to run migration scripts: `$ php bin/magento setup:upgrade`

##### [Configure declarative schema](https://developer.adobe.com/commerce/php/development/components/declarative-schema/configuration/)

- To declare tables and foreign keys, you need to create a file `Vendor/Module/etc/db_schema.xml`.
- When adding a new column to the table, remember [generate](https://developer.adobe.com/commerce/php/development/components/declarative-schema/migration-scripts/#create-a-schema-whitelist) file `db_schema_whitelist.json`.

##### [Develop data and schema patches](https://developer.adobe.com/commerce/php/development/components/declarative-schema/patches/)
A data patch is a class that contains data modification instructions. It is defined in a
`<Vendor>/<Module_Name>/Setup/Patch/Data/<Patch_Name>.php` file and implements `\Magento\Framework\Setup\Patch\DataPatchInterface.`

Unlike the declarative schema approach, patches will only be applied once. A list of applied patches is stored in the patch_list database table.
An unapplied patch will be applied when running the setup:upgrade from the Magento CLI.

##### Will old scripts work in newer versions?
Old scripts will work with new versions of Magento. However, if you want to convert your old scripts to the new format, 
implement \Magento\Framework\Setup\Patch\PatchVersionInterface. This interface allows you to specify the setup version 
of the module in your database. If the version of the module is higher than or equal to the version specified in your patch,
then the patch is skipped. If the version in the database is lower, then the patch installs.