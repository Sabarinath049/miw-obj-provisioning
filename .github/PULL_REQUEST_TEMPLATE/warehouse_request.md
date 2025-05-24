# Checklist

<!-- Please verify the following checks -->

- Read the instruction for How to request a new Warehouse mentioned in
  [readme](https://minerva-docs.cglcloud.com/userguide/miw/MIW_vNext_Guide/Object_Creation_Requests/Raising_Object_Provisioning_Request.html#raising-a-request)?
  - [ ] No
  - [ ] Yes
- Created the .json file in the warehouses/ subdirectory.?
  - [ ] No
  - [ ] Yes
  <!-- If no, explain why tests have not/cannot be written -->
- Have you followed the Naming convention for Warehouse mentioned
  [here](https://minerva-docs.cglcloud.com/userguide/miw/MIW_vNext_Guide/Minerva_Standards/Minerva_ConventionStandards_for_Snowflake_Objects.html#data-product-object-naming-conventions)?
  - [ ] No
  - [ ] Yes
  <!-- If no, explain why not -->
- Is access_roles mentioned in warehouse request JSON is already present in access.cargill.com and SCIM'ed into snowflake?
  - [ ] No
  - [ ] Yes
  <!-- If no, raise the request for your AAD group in access.cargill.com using document - https://minerva-docs.cglcloud.com/userguide/miw/MIW_vNext_Guide/Security_and_compliance/Role_Creation/Create_ADGroup.html and SCIM into snowflake using https://minerva-docs.cglcloud.com/userguide/miw/MIW_vNext_Guide/Object_Creation_Requests/Raising_Object_Provisioning_Request.html#raising-a-request-->
- Is name of your warehouse in the JSON key "warehouse_name" same as file name of your JSON?
  - [ ] No
  - [ ] Yes
  <!-- If no, explain why not -->
- Is your data product manager aligned on the warehouse creation request into Snowflake environment?
  - [ ] No
  - [ ] Yes
  <!-- If no, explain why not -->
- Have you referred the template "templates/warehouses/PRD_DTD_PRDGRP_ADH_SNF_1_WH.json" to create your role SCIM request?
  - [ ] No
  - [ ] Yes
  <!-- If no, explain why not -->
- Have you thoroughly gone through the [Mandatory READ](https://git.cglcloud.com/Minerva/miw-snowflake-object-provisioning/blob/main/README.md) file?
  - [ ] No
  - [ ] Yes
  <!-- If no, explain why not -->
- Have you filled the mandatory "description" field in the Database Metadata JSON template?
  - [ ] No
  - [ ] Yes
  <!-- If no, explain why not -->
- Can you confirm if the changes requested under this Pull request belong to same Product Group?
  - [ ] No
  - [ ] Yes
  <!-- If no, explain why not -->
- Can you confirm the warehouse_counter number your Product Group team is providing follows the incremental approach mentioned in Minerva MIW documentation?
  - [ ] No
  - [ ] Yes
  <!-- If no, explain why not -->
- Can you confirm you have atleast listed 1 and atmost 5 Email ID's in all UPPERCASE as part of attribute "alert_subscribers_list" in your Warehouse JSON?
  - [ ] No
  - [ ] Yes
  <!-- If no, explain why not -->
