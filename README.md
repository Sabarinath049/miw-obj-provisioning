**Minerva MIW Object Provisioning Guidelines**

Welcome to the Automated Object Provisioning Framework managed by MIW Team for Cargill Minerva Program.
To gain understanding of the overall process of raising a PR, please refer : [How to Raise Object Provisioning Request](https://cargillonline.sharepoint.com/sites/minervacentral/SitePages/MIW.aspx?csf=1&web=1&e=2eYujQ&OR=Teams-HL&CT=1748258064817&CID=9ee7a1a1-303c-0000-b0d1-cb9f262be36f&cidOR=SPO)

While we appreciate you reading the document, we wish to bring to your notice key validations that your PR will undergo. Having solid understanding of validation rules will enable you to reduce turn around time of your object provisoning process.
This document outlines the guardrails, standards, and validations enforced during the provisioning of objects, ensuring compliance with Minerva and MIW governance.

**Minerva Standards & Conventions** : It is critical that all user object provisioning requests follow the conventions and standards established by MIW. Failure to comply may result in provisioning delays or rejection.

**Key Standards**

- **Naming Conventions**

  - All object names must follow approved naming standards.
  - Refer to Snowflake Conventions: [Minerva Snowflake Conventions](https://cargillonline.sharepoint.com/sites/minervacentral/SitePages/MIW.aspx?csf=1&web=1&e=2eYujQ&OR=Teams-HL&CT=1748258064817&CID=9ee7a1a1-303c-0000-b0d1-cb9f262be36f&cidOR=SPO&startedResponseCatch=true#:~:text=Conventions%20and%20Templates)
  - Refer to LakeHouse Conventions: [Minerva Lakehouse Conventions](https://cargillonline.sharepoint.com/:x:/r/sites/Minerva-FullTeam/Shared%20Documents/Information%20Warehouse/LakeHouse%20MVP/AWS_LakeHouse_Nomenclature.xlsx?d=w5507f97edd484c80b31702ab646d8f0f&csf=1&web=1&e=Lq72dE)

- **Input JSON Templates**

  - Provisioning requests must use the standard JSON (for Snowflake) and YAML(for Lakehouse) metadata templates.
  - You can refer to the Snowflake Production Templates from : [Snowflake Production Templates](https://git.cglcloud.com/Minerva/miw-object-provisioning/tree/main/templates/snowflake)
  - You can refer to the Lakehouse Production Templates from : [Lakehouse Production Templates](https://git.cglcloud.com/Minerva/miw-object-provisioning/tree/main/templates/aws_lakehouse)

- **MIW Rule Book Compliance (Only enabled for Snowflake Provisionings for now)**

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

