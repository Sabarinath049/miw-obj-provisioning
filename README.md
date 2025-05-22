**Minerva MIW Snowflake Object Provisioning Guidelines**

Welcome to the Automated Snowflake Object Provisioning Framework managed by MIW Team for Cargill Minerva Program.

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
        
        - **Generic Checks**
          
          - Public roles must never be granted access to Snowflake objects (Database, Schema, Warehouse).
          - Validate whether the object already exists in Snowflake or if it is a new request.
          - Ensure that roles listed in the access_roles JSON key are already SCIM-provisioned. Provisioning fails if roles are missing.

        - **Pull Request Specific Checks**
   
          - It is adviced to only request changes in 5 of less objects as part of a single PR. This can be ensured by not adding/modifying more than 5 files as part of a single PR.
          - At this point of time **we don't allow any file deletions** if it has been already merged into the base branches like dev, stg and prod. A separate object deletion pipeline will be established in upcoming PIs.
          - If you are modifying a file, be cautious of the fact that you restrain from changing below parameters for given objects since it can trigger a destroy and recreate on Snowflake environment which is a risk to stability of use cases and platform.
            
          | Method      | Description                          |
| ----------- | ------------------------------------ |
| `GET`       | :material-check:     Fetch resource  |
| `PUT`       | :material-check-all: Update resource |
| `DELETE`    | :material-close:     Delete resource |  

        Warehouse-Specific Checks
✅ Only the following warehouse sizes are supported:

XSMALL, SMALL, MEDIUM, LARGE, XLARGE

📊 Credit limit per warehouse: 500 credits/month
Requests above this threshold require justification and may be flagged for MIW approval.

⏱️ Auto-suspend timeout must be ≤ 60 seconds.
Justification is required for higher values.

🔮 Coming Soon: Validation support for fields like:

max_cluster_count, scaling_policy, query_timeout_in_seconds,
statement_queued_timeout_in_seconds, max_concurrency_level

Schema-Specific Checks
🕒 Data retention settings > 1 day must be approved by your Data Product Manager.

Storage Integration Checks
👨‍🔧 Only the Data Engineer persona can access storage integrations.

🔐 Role ARN must comply with AWS standards.

🧪 Convention Validation
Our naming convention checks ensure objects follow strict patterns to support platform governance, cost control, and scalability.

Separator Check:
Object names must have the correct number of underscores (_) as per convention.

Prefix Check:
Some object types are required to start with a specific prefix.

Suffix Check:
All objects (except process users) must end with the appropriate object-type suffix.
