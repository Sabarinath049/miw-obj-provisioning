
# Minerva Lakehouse – PR Guidance for Users

> **Purpose:** This README helps you raise high‑quality PRs for Minerva Lakehouse provisioning—covering scope, naming, approvals, and object‑specific rules.  
> **Audience:** Data engineers, analysts, and project owners who submit metadata templates for Lakehouse and Compute provisioning.  
> **Tone:** Guidance for users (what to do, why, and how).

---

## 1. PR Scope & Hygiene

- **Always sync your forks**
- **Keep changes small:** Limit each PR to **5 or fewer files** to simplify review and reduce risk.
- **Separate SageMaker Unified Studio PR**: Don't merge requests for smus_roles and smus_projects with iam, s3, glue_db and resource_policy. Please have separate PR for SMUS entities.   
- **Separate PR for Data Federation:**  
  - First provision the **databases** in original AWS account.  
  - Then, raise a **separate PR** to request federation into cross‑account in form of a resource links.
  - Once the Federation PR is merged, then only a PR with cross account Role or SMUS project can be raised to gain access to that federated Database.
- **No object renames:** Once provisioned, names like **S3 buckets** or **Glue DBs** **cannot** be changed.  
  - Only new object can be requested or you need to explicitely request MIW to drop original object first.  
- **Delete/Destroy not supported initially:**  
  - A standard deletion/destroy path is under design (to consider DR, backups, etc.).  
  - For exceptions, raise an explicit request via **Minerva Central issues:**  
    [Minerva Central Issues](https://git.cglcloud.com/Minerva/minerva-central/issues/)
  - MIW will assess case‑by‑case and perform only after required approvals.  
- **Access management is supported:**  
  - Grant/revoke access through **IAM** and **Lake Formation** (preferred over object re‑creation) is supported by Automation.  
- **No file deletions in PRs:** Only add or modify files—**do not delete** files in the repo.

---

## 2. Immutable Attributes (cannot be changed later)

Changing the following after creation will force object re‑creation—**not allowed** in subsequent requests.

| Object Type       | Immutable fields                                                                 |
|-------------------|-----------------------------------------------------------------------------------|
| Glue Database     | `database_name`, `database_s3_location`, `aws_account_id`, `region`               |
| IAM Role          | `role_name`, `aws_account_id`                                                     |
| Resource Policy   | `aws_account_id`, `cross_account_aws_id`, `principal_role_arn`                    |
| S3 Bucket         | `bucket_name`, `aws_account_id`, `aws_region`                                     |
| Data Federation   | `aws_account_id`                                                                  |

---

## 3. Naming Conventions & Abbreviations

- Use **approved naming conventions** (old or new) from MIW SharePoint docs:  
  - [Old Naming Convention – AWS_LakeHouse_Nomenclature.xlsx](https://cargillonline.sharepoint.com/:x:/r/sites/Minerva-FullTeam/Shared%20Documents/Information%20Warehouse/LakeHouse%20MVP/AWS_LakeHouse_Nomenclature.xlsx?d=w5507f97edd484c80b31702ab646d8f0f&csf=1&web=1&e=gClh8a&nav=MTVfe0RGMDc2NzdELTA3QzctNDQzNC1CQzY4LTYxOEQzRjZCNTE5Mn0)  
  - [New Naming Convention – LakeHouse_Brainstorming_Sessions.xlsx](https://cargillonline.sharepoint.com/:x:/r/sites/Minerva-FullTeam/Shared%20Documents/Information%20Warehouse/LakeHouse%20MVP/LakeHouse_Brainstorming_Sessions.xlsx?d=w7cae0903859e42b78119ee79dd579a4d&csf=1&web=1&e=HrLwYQ&nav=MTVfezFDQjBFQkQ4LUVCOEQtNDZEQy1BMDQzLTY4MkZGNjA1MTU1Nn0)
- **Always** use **governance‑approved abbreviations** in fields like `enterprise_or_func_name` and `enterprise_or_func_subgrp_name`:  
  [Governance-approved abbreviations](https://git.cglcloud.com/Minerva/minerva-tags/blob/main/meta/value_refs/abbreviations.yaml)  
  - **PRs will be rejected** if non‑approved abbreviations are used.
- The conventions in SharePoint will be upgraded once documentation is published—keep checkin.
  [MIW Documentation](https://cargillonline.sharepoint.com/sites/minervacentral/SitePages/MIW.aspx?csf=1&web=1&e=2eYujQ&OR=Teams-HL&CT=1748258064817&CID=9ee7a1a1-303c-0000-b0d1-cb9f262be36f&cidOR=SPO)

---

## 4. Account & Repo Reference (use in templates)

| Git repo folder | AWS account alias             | AWS account ID | Enterprise/Function |
|-----------------|-------------------------------|----------------|---------------------|
| lakehouse-001   | minerva-prod-lakehouse-001     | 578647603827   |                     |
| compute-001     | minerva-prod-compute-001      | 367241115350   |    Ag & Trading     |
| compute-002     | minerva-prod-compute-002       | 884308299029   |       Food          |
| compute-003     | minerva-prod-compute-003       | 011379513867   |Specialized Portfolio|
| compute-004     | minerva-prod-compute-004       | 632247962242   |     Corporate       |

> **Tip:** Place your template files under the **correct git repo folder**, and make sure `aws_account_id` and **alias** match the target account.

---

## 5. Region, Ownership & Intake

- **Region restriction:** Request **only in `us-east-1`**. Other regions are not supported at this stage.  
- **Mandatory DB ownership fields:**  
  - `data_owner_email`  
  - `data_owner_github_uname`  
  - `data_leader`  
  > `data_owner_github_uname` is used for **explicit approvals** so that DB access is only granted to roles approved by the data owner. The DB owner(s) must be part of **Cargill GitHub Enterprise** to approve PRs.  
- **Verify intake authenticity:** Confirm **Intake ID**, **Status**, **Data Leader**, **Role Owner**, **Data Owner**, etc., before submission.

---

## 6. Object‑Specific Guidance (templates & validations)

### 6.1 Glue Databases (Lakehouse entities)
- `database_s3_location` is **mandatory**.  
- `aws_account_id` for Prod Lakehouse **must be `578647603827`**; ensure correct for your environment.  
- `region` must be **`us-east-1`**.  
- `data_construct` ∈ **`DataProduct` | `Source`**.  
- `data_env` ∈ **`snd` | `dev` | `stg` | `prd`**.  
- `data_layer` ∈ **`raw` | `cur` | `srv` | `int`**.  
- `source_name` must be **Governance‑approved** (`tags` repo):  
  [Governance Approved Tags](https://git.cglcloud.com/Minerva/minerva-tags/tree/main/tags)
- `data_owner_email` and `data_owner_github_uname` **must not be empty** and must match Intake/Data Contract.  
- Every `github_uname` in the template **must be a valid GitHub profile**.  
- `data_classification` and `data_privacy` **must** use values from the **tags repo**.  
- `enterprise_or_func_name` and `enterprise_or_func_subgrp_name` **must** use valid abbreviations from the **tags repo**.

---

### 6.2 IAM Roles

#### 6.2.1 Ingestion Engineer
- Request **no** access beyond the **TableFlow‑created DB** (e.g., `lkc-vzdk2j`).  
- Grant access **only** to the **equivalent enterprise/function source bucket** (specific prefixes) — **no bucket‑wide (`s3://bucket/*`)** grants.  
- Role exists **only** in **lakehouse‑001** (Dev/Prod), **not** in Compute.  
- **Grain:** Enterprise/Function (CORP may follow its bucket grain).  
- `usage_type` = **`IngestionEngineer`**.  
- `principal_role_arn` must be a **valid cross‑account role ARN** (source). Please raise this request **only when your role in source MIF account already exists**, otherwise the provisioning on Lakehouse will fail.
- **No read access** to any database.  
- **Support channel** if unsure about paths:  
  [Teams MIW Support Channel](https://teams.microsoft.com/l/channel/19%3A5a7600cbb5364d879827e49df32d45c5%40thread.tacv2/MIW%20-%20Object%20Provisioning%20Support?groupId=d6e0463c-4725-481f-94ac-5836bfad0e30&tenantId=57368c21-b8cf-42cf-bd0b-43ecd4bc62ae)

#### 6.2.2 Integrated Ingestion Engineer
- Role exists **only** in **lakehouse‑001** (Dev/Prod).  
- Glue DB access: **only `raw` DBs (R/W)**; **no** access to curated, serving, or internal DBs.  
- Access **only** to the **equivalent enterprise/function source bucket**.  
- **Grain:** Subgroup.  
- `usage_type` = **`IntegratedIngestionEngineer`**.  
- `principal_role_arn` must be **valid**. Please raise this request **only when your role in source MIF account already exists**, otherwise the provisioning on Lakehouse will fail.
- **No read access** to any database.

#### 6.2.3 Proc Data Engineer
- **Grain (Lakehouse):** Subgroup (not Enterprise/Function).  
- `compute_size` ∈ **`XSML` | `SML` | `MED` | `LRG` | `XLRG`**; anything **above `MED`** needs justification.  
- **R/W** access **only** to **non‑CDP raw DBs** owned by the same enterprise/function/subgroup; **no R/W** on CDP DBs.  
- Access **only** to **`merged`** paths in source buckets of the same enterprise/function/subgroup (for historical + current stitching).  
- Access subgroup **eng‑assets** and **scripts** buckets only, e.g.:  
  - `s3://minerva-prd-eng-assets-corp-fin/prd/fin/glue/`  
  - `s3://minerva-prd-scripts-corp-fin/prd/fin/glue/`  
- Trigger **only** Glue crawlers belonging to the **same** Ent/Func/Subgroup.  
- Glue jobs must be limited by **name prefix** (`job_control_configs`).  
- **Athena workgroup naming:** `[ENT/FUNC]_[SUBGRP]_[DATA_ENV]_[WG_PURPOSE]_[T-SIZE]` (e.g., `agtr_apac_prd_eng_xsml`).  
- **Dynamic PR approval** will ensure DB owner approval for DB access.  
- `usage_type` = **`DataEngineer`**.  
- `max_session_duration` **> 2h** must be **justified**.  
- `execution_asset_prefixes` must be limited to **same** ent/function/subgroup paths.  
- Flags `enable_glue_interactive_session`, `enable_athena_access`, `enable_glue_jobs`, `snowflake_iceberg_support` are **boolean** only.  
- `principal_role_arn` must be a **valid ARN** generated from **Snowflake catalog integration**.

---

### 6.3 S3 Buckets

#### 6.3.1 Ent/Func Source System Buckets
- **Grain:** Enterprise/Function (except CORP specifics).  
- **Region:** `us-east-1` only (**non‑us‑east‑1** fails review).  
- `usage_type` = **`Source`**.  
- `enterprise_or_func_name` **mandatory**; for **CORP**, `enterprise_or_func_subgrp_name` also **mandatory**.  
- `public_access_blocked` must **never** be `false`.  
- `encryption_key_arn` is **not supported** (raise explicit request if needed).

#### 6.3.2 Source System Buckets (Deprecated)
- Requests for **source‑only** buckets should **fail** review (deprecated pattern).  
- If exception is discussed with MIW:  
  - `source_system` must be **populated**.  
  - **Region:** `us-east-1` (others → **soft failure**).  
  - `usage_type` = **`Source`**.  
  - `enterprise_or_func_name` and `enterprise_or_func_subgrp_name` should be **empty**.  
  - `public_access_blocked` must **never** be `false`.  
  - `encryption_key_arn` **not supported**.

#### 6.3.3 Ent/Func Scripts Buckets
- **Grain:** Enterprise/Function (except CORP specifics).  
- **Region:** `us-east-1`.  
- `usage_type` = **`Scripts`**.  
- `enterprise_or_func_name` **mandatory**; for **CORP**, `enterprise_or_func_subgrp_name` also **mandatory**.  
- `public_access_blocked` must **never** be `false`.  
- `encryption_key_arn` **not supported**.

#### 6.3.4 Ent/Func Engineering Assets Buckets
- **Grain:** Enterprise/Function (except CORP specifics).  
- **Region:** `us-east-1` (others → **soft failure**).  
- `usage_type` = **`EngAssets`**.  
- `enterprise_or_func_name` **mandatory**; for **CORP**, `enterprise_or_func_subgrp_name` also **mandatory**.  
- `public_access_blocked` must **never** be `false`.  
- `encryption_key_arn` **not supported**.

#### 6.3.5 Ent/Func Specific Data Product Buckets
- **Grain:** Enterprise/Function only.  
- **Region:** `us-east-1` (others → **soft failure**).  
- `usage_type` = **`DataProduct`**.  
- `enterprise_or_func_name` **mandatory**; for **CORP**, `enterprise_or_func_subgrp_name` also **mandatory**.  
- `public_access_blocked` must **never** be `false`.  
- Any value of `encryption_key_arn` should **fail** the pipeline (MIW will review).

---

### 6.4 Resource Policy
- **File name pattern:** `[MINERVA]_[ENT/FUNC]_[SRC_AWS_ACCNT]_[ENT/FUNC]_[SUBGRP]_[DATA_ENV]_rp`  
  - Example: `minerva_prd_cmt_agtr_apac_prd_rp`  
- `Intake_ID` must be **valid** and must belong to the relevant **Ent/Func** and **Subgroup**.  
- `aws_account_id` and `cross_account_aws_id` must be **valid AWS Account IDs**.  
- `principal_role_arn` must be **valid** for CMT‑based ingestion. Please raise this request **only when your role in source MIF account already exists**, otherwise the provisioning on Lakehouse will fail.
- Grant access **only** to **CDP‑specific** DBs and S3 paths.  
- Restrict to DBs and S3 paths **within the same** Ent/Func/Subgroup—**nothing beyond**.

---

### 6.5 Glue Crawler
- **One crawler → one database:** The template must specify a **single** DB aligned to the **same** Ent/Func/Subgroup.  
- **Grain:**  
  - **Data Product** for **curated/serving** DBs.  
  - **Source System** (e.g., Concur, Archer, SAP_TC1) for **raw**.  
- **Naming:** Must include `data_env` and `data_layer`; for **serving**, also include `usage_type`.  
- **S3 paths:**  
  - **Raw** for Source crawlers.  
  - **Curated/Serving** for Data Product crawlers.  
  - Bucket must match the **same** Ent/Func/Subgroup.  
- **Storage type:** `iceberg`—otherwise call out soft warning/failure in pipeline summary and reach MIW.  
- **Governance:** `data_classification` and (if Source) `source_name` must match **pre‑approved** tags.  
- **Account & region:** `aws_account_id` must be a **Lakehouse ecosystem** account; `region` = **`us-east-1`**.  
- Ensure the crawler writes **only** within its **own** subgroup’s DB.  
- For **serving** layer crawlers, `usage_type` is **mandatory** and part of the crawler name.

---

### 6.6 Data Federation
- Can be at **Source System** or **Data Product** grain.  
- **Dynamic PR approval** applies (DBs shared across accounts require owner approvals).  
- `cross_account_aws_ids` must be from the **approved** Lakehouse list; otherwise **soft failure**.  
- `source_name` must be **governance‑valid**.  
- `data_env` must appear in DB names—**environments must match**.  
- All DBs listed must belong to the **same** Ent/Func/Subgroup as the template.

---

### 6.7 SMUS Roles
- `aad_group_name` must be a **valid SailPoint entitlement**.  
- **Role owner** details must match **Intake**.  
- **Data Leader** and **Governance Lead** must match **Intake**.  
- Enterprise/Function/Subgroup values must use **approved abbreviations**.  
- If the role is **Data Product level**, the Intake must belong to the **same Data Product**.

---

### 6.8 SMUS Project
- `parent_domain_unit` must be from the **maintained list** in automation backend.  
- `project_owner` must match **Intake**.  
- **Project membership** must align with `smus_project_name` (avoid analyst entitlement in an engineer’s project).  
- `compute_size` must be from valid **T‑shirt sizes**.  
- The project‑specific role should **tie to a Glue Usage Profile** (to govern compute).  
- The associated **Athena workgroup** should be controlled by **T‑shirt size**.  
- `usage_type` must be **valid** (tags repo).  
- Populating `data_product_team` triggers a **soft failure** → MIW manual scrutiny (is this truly a Data Product Team or subgroup‑level?).  
- If `cross_account_configs` = enabled: **raw** should be accessed via **Data Federation**, map the **resource link** (not the original DB).  
- **Dynamic PR approval** applies (cross‑account sharing).  
- **Database permissions:** **read‑only**.  
