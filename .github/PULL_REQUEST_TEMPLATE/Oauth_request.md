# Checklist

<!-- Please verify the following checks -->

- Read the instruction for How to request a OAuth in
  [readme](https://minerva-docs.cglcloud.com/userguide/miw/MIW_vNext_Guide/Object_Creation_Requests/Raising_Object_Provisioning_Request.html#raising-a-request)?
  - [ ] No
  - [ ] Yes
  <!-- If no, explain why not -->
- PS account has been created and followed the prerequisite required for Oauth?
  - [ ] No
  - [ ] Yes
  <!-- If no, follow the steps https://minerva-docs.cglcloud.com/userguide/miw/MIW_vNext_Guide/Security_and_compliance/Role_Creation/Ps_User_Creation_Oauth_Generation.html>
- Created the .json file in the oauth / subdirectory?
  - [ ] No
  - [ ] Yes
  <!-- If no, explain why tests have not/cannot be written -->
- Have you referred the template "/templates/oauth/ps987654.json" to create your oauth user request?
  - [ ] No
  - [ ] Yes
  <!-- If no, explain why not --> 
- Is your data product manager aligned on the process user creation request you are submitting?
  - [ ] No
  - [ ] Yes
  <!-- If no, explain why not -->
- Have you thoroughly gone through the [Mandatory READ](https://git.cglcloud.com/Minerva/miw-snowflake-object-provisioning/blob/main/README.md) file?
  - [ ] No
  - [ ] Yes
  <!-- If no, explain why not -->
- Roles called out in the key "minerva_aad_group" has already been SCIM'ed on snowflake account?
  - [ ] No
  - [ ] Yes
  <!-- If no, explain why not -->
- Please confirm you haven't changed the attributes "azure_tenant_id", "oauth_resource_uri" & "einsight_uuid" in the input JSON template? **Note:** This shouldn't be changed by users.
  - [ ] No
  - [ ] Yes
  <!-- If no, explain why not -->
- Have you filled the mandatory "purpose" field in the Database Metadata JSON template?
  - [ ] No
  - [ ] Yes
  <!-- If no, explain why not -->
- Can you confirm if the changes requested under this Pull request belong to same Product Group?
  - [ ] No
  - [ ] Yes
  <!-- If no, explain why not -->
