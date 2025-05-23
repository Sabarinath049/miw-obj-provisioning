**Minerva MIW Snowflake Object Provisioning Guidelines**

Welcome to the Automated Snowflake Object Provisioning Framework managed by MIW Team for Cargill Minerva Program.
To gain understanding of the overall process of raising a PR, please refer : [How to Raise Object Provisioning Request](https://minerva-docs.cglcloud.com/userguide/miw/MIW_vNext_Guide/Object_Creation_Requests/Raising_Object_Provisioning_Request.html)

While we appreciate you reading the document, we wish to bring to your notice key validations that your PR will undergo. Having solid understanding of validation rules will enable you to reduce turn around time of your object provisoning process.
This document outlines the guardrails, standards, and validations enforced during the provisioning of Snowflake objects, ensuring compliance with Minerva and MIW governance.

**Minerva Standards & Conventions** : It is critical that all user object provisioning requests follow the conventions and standards established by MIW. Failure to comply may result in provisioning delays or rejection.

**Key Standards**

- **Naming Conventions**

  - All Snowflake object names must follow approved naming standards.
  - Refer to: [Minerva Conventions](https://minerva-docs.cglcloud.com/userguide/miw/MIW_vNext_Guide/Minerva_Standards/Minerva_ConventionStandards_for_Snowflake_Objects.html)

- **Input JSON Templates**

  - Provisioning requests must use the standard JSON metadata templates.
  - You can refer to the Production Templates from : [Production Templates](https://git.cglcloud.com/Minerva/miw-snowflake-object-provisioning/tree/main/templates)

- **MIW Rule Book Compliance**

  - Our automated provisioning framework enforces rules as per the MIW rule book, with multiple layers of validation.

  - **Automated Validations**
    - Currently, our framework supports naming convention checks and technical validations. Please review the rules below to avoid failed provisioning attempts.

      - **Technical Validation**
        
        - **Pull Request Specific Checks**
 
          - **Pull requests must be limited to a single Product Group.** All files included in a pull request should belong exclusively to the same Product Group. 
          - It is adviced to only **request changes 5 of less object changes as part of a single PR**. This can be ensured by not adding/modifying more than 5 files as part of a single PR. 
          - At this point of time **we don't allow any file deletions** if it has been already merged into the base branches like dev, stg and prod. A separate object deletion pipeline will be established in upcoming PIs. 
          - If you are modifying a file, be cautious of the fact that you restrain from changing parameters called out at the bottom of this file since it can trigger destroy and recreate on Snowflake environment which is a risk to stability of use cases and platform.

        - **Generic Checks**
          
          - Public roles must never be granted access to Snowflake objects (Database, Schema, Warehouse).
          - Validate whether the object already exists in Snowflake or if it is a new request.
          - Ensure that roles listed in the access_roles JSON key are already SCIM-provisioned. Provisioning fails if roles are missing.

        - **Warehouse-Specific Checks**

          - Only the following warehouse sizes are supported: XSMALL, SMALL, MEDIUM, LARGE, XLARGE 
          - Credit limit per warehouse: **500 credits/month**. Requests above this threshold require justification and may be flagged for MIW approval. 
          - **Auto-suspend timeout must be ≤ 60 seconds**. Justification is required for higher values.
          - Only **STANDARD** type of warehouses are supported by MIW at this point.
          - Maximum **Cluster count of 10** is supported by MIW.
          - Only **ECONOMY** scaling policy is supported by MIW warehouses for now. If you need changes, MIW may need additional justifications.
          - Maximum allowed query timeout is 1800 seconds for now.
          - **Alert subscriber list needs to be mandatorily populated** in the Warehouse JSON. Please ensure all users are verified in snowflake and **Email-IDs are mentioned in ALL CAPITALS**.

        - **Schema-Specific Check**
        
          - Data retention settings > 1 day must be approved by your Data Product Manager.
          - The Data Classifications allowed at this point of time are : "General", "Public", "Limited", "Restricted", "Highly Restricted" only.

        - **Storage Integration Checks** 
        
          - Only the Data Engineer persona can access storage integrations. 
          - Role ARN must comply with AWS standards.

      - **Convention Validation**
      
      - Our naming convention checks ensure objects follow strict patterns to support platform governance, cost control, and scalability.

        - **Separator Check**
        
        - Object names must have the correct number of underscores (_) as per convention.

        - **Prefix Check**

          - Some object types are required to start with a specific prefix.

        - **Suffix Check**

        - All objects (except process users) must end with the appropriate object-type suffix.
        
**Snowflake Object Attributes Likely to trigger Destroy and Recreate**

| Snowflake Objects   | Attributes Likely to trigger Destroy and Recreate                                              |
|---------------------|------------------------------------------------------------------------------------------------|
| Warehouse           | "WAREHOUSE_NAME", "WAREHOUSE_SIZE", "MIN_CLUSTER_COUNT", "MAX_CLUSTER_COUNT", "WAREHOUSE_TYPE" |
| Database            | "DATABASE_NAME"                                                                                |
| Schema              | "SCHEMA_NAME"                                                                                  |
| Storage Integration | "STORAGE_INTEGRATION_NAME", "STORAGE_PROVIDER", "STORAGE_AWS_ROLE_ARN"                         |
| Persona             | "PERSONA_NAME"/ "ROLE_NAME"                                                                    |
| OAUTH               | "SUB-UUID"                                                                                     |
