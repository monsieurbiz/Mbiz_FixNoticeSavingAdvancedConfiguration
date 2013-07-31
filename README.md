# Mbiz_FixNoticeSavingAdvancedConfiguration

This is a simple override of `Mage_Adminhtml_Model_Config_Data` class.

## Why ?

Because :

    An error occurred while saving this configuration: Notice: Trying to get property of non-object in /var/www/magento/app/code/core/Mage/Adminhtml/Model/Config/Data.php on line 135

This error disallow to save the **Advanced** section on the configuration.

## Solution

I replaced the line bellow: (line 135)

    $backendClass = $fieldConfig->backend_model;

By this one:

    $backendClass = $fieldConfig ? $fieldConfig->backend_model : null;

## Diff

```diff
diff --git a/app/code/community/Mage/Adminhtml/Model/Config/Data.php b/app/code/community/Mage/Adminhtml/Model/Config/Data.php
index 749ee52..62fa9df 100644
--- a/app/code/community/Mage/Adminhtml/Model/Config/Data.php
+++ b/app/code/community/Mage/Adminhtml/Model/Config/Data.php
@@ -132,7 +132,17 @@ class Mage_Adminhtml_Model_Config_Data extends Varien_Object
                 /**
                  * Get field backend model
                  */
-                $backendClass = $fieldConfig->backend_model;
+
+                // FIX START =============================================================== 2013-07-31
+
+                // =============== ORIGINAL
+                //$backendClass = $fieldConfig->backend_model;
+
+                // ======== BY MONSIEUR BIZ
+                $backendClass = $fieldConfig ? $fieldConfig->backend_model : null;
+
+                // FIX END ============================================================================
+
                 if (!$backendClass) {
                     $backendClass = 'core/config_data';
                 }
```
