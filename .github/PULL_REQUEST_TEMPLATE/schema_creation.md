# Checklist

<!-- Please verify the following checks -->

- Read the instruction for How to request a new schema mentioned in
  [readme](https://minerva-docs.cglcloud.com/userguide/miw/MIW_vNext_Guide/Object_Creation_Requests/Raising_Object_Provisioning_Request.html#raising-a-request)?
  - [ ] No
  - [ ] Yes
- Created the .json file in the schemas/ subdirectory.?
  - [ ] No
  - [ ] Yes
  <!-- If no, explain why tests have not/cannot be written -->
- Have you followed the Naming convention for Schema mentioned
  [here](https://minerva-docs.cglcloud.com/userguide/miw/MIW_vNext_Guide/Minerva_Standards/Minerva_ConventionStandards_for_Snowflake_Objects.html#data-product-object-naming-conventions)?
  - [ ] No
  - [ ] Yes
  <!-- If no, explain why not -->
- Is parent DB mentioned for this Schema is already present in Snowflake?
  - [ ] No
  - [ ] Yes
  <!-- If no, raise the request for your DB by using the steps mentioned in https://minerva-docs.cglcloud.com/userguide/miw/MIW_vNext_Guide/Object_Creation_Requests/Raising_Object_Provisioning_Request.html#raising-a-request -->
- Is access_roles mentioned schema request JSON is already present in access.cargill.com and SCIM into snowflake?
  - [ ] No
  - [ ] Yes
  <!-- If no, raise the request for your AAD group in access.cargill.com using document - https://minerva-docs.cglcloud.com/userguide/miw/MIW_vNext_Guide/Security_and_compliance/Role_Creation/Create_ADGroup.html and SCIM into snowflake using https://minerva-docs.cglcloud.com/userguide/miw/MIW_vNext_Guide/Object_Creation_Requests/Raising_Object_Provisioning_Request.html#raising-a-request-->
- Is name of your schema in the JSON key "schema_name" same as file name of your JSON?
  - [ ] No
  - [ ] Yes
  <!-- If no, explain why not -->
- Is your data product manager aligned on the schema creation request into Snowflake environment?
  - [ ] No
  - [ ] Yes
  <!-- If no, explain why not -->
- Have you referred the template "templates/schemas/FDP_PRODUCTX_GBL_TRF_SC.json" to create your role SCIM request?
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
- Have you correctly put "data_first_classification" details in alignment with Cargill's Data Classification Policies?
  - [ ] No
  - [ ] Yes
  <!-- If no, explain why not -->
