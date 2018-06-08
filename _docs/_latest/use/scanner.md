---
title: Scanner
order: 004
---

# {{ page.title }}

Forseti Scanner scans the inventory data according to the rules (policies) you defined.

You can learn about how to [define custom rules]({% link _docs/latest/configure/scanner/rules.md %}).

## Running Forseti Scanner

Forseti Scanner works on a data model, so before you start using Scanner, you'll select a model to use. 

Instructions on how to [create a model]({% link _docs/latest/use/model.md %}).

To configure which scanners to run, see 
[Configuring Forseti: Configuring Scanner]({% link _docs/latest/configure/scanner/index.md %}).


### Selecting a data model

```bash
$ forseti model use <YOUR_MODEL_NAME>
```

### Running the scanner

```bash
$ forseti scanner run
```

When Scanner finds a rule violation, it outputs the data to a Cloud SQL database.

Scanner can save violations as a CSV and send an email notification or upload it
automatically to a Cloud Storage bucket.

## Sample scanner violation

### Firewall rule violation

{: .table .table-striped}
| resource_id | resource_type | full_name | rule_index | rule_name | violation_type | violation_data |
|---|---|---|---|---|---|---|
| my-project-1 | firewall_rule | organization/1234567890/project/my-project-1/firewall/494704901029517064/ | 1 | disallow_all_ports | FIREWALL_BLACKLIST_VIOLATION | {u'policy_names': [u'gke-staging-9696e33a-all'], u'recommended_actions': {u'DELETE_FIREWALL_RULES': [u'gke-staging-9696e33a-all']}} |
| my-project-2 | firewall_rule | organization/1234567890/project/my-project-2/firewall/494704901029517064/ | 1 | disallow_all_ports | FIREWALL_BLACKLIST_VIOLATION | {u'policy_names': [u'gke-canary-west-69bb2963-all'], u'recommended_actions': {u'DELETE_FIREWALL_RULES': [u'gke-canary-west-69bb2963-all']}} |

### Bigquery violation

{: .table .table-striped}
| resource_id | resource_type | full_name | rule_index | rule_name | violation_type | violation_data |
|--------------|---|---|---|---|---|---|
| my-project-1:testdataset1 | bigquery_dataset | organization/1234567890/project/my-project-1/dataset/my-project-1:testdataset1/dataset_policy/dataset/my-project-1:testdataset1/ | 0 | Search for public datasets | BIGQUERY_VIOLATION | {u'access_user_by_email': u'', u'access_special_group': u'allAuthenticatedUsers', u'access_domain': u'', u'access_group_by_email': u'', u'role': u'READER', u'full_name': u'organization/1234567890/project/my-project-1/dataset/my-project-1:testdataset1/dataset_policy/dataset/my-project-1:testdataset1/', u'dataset_id': u'my-project-1:testdataset1', u'view': {}} |
| my-project-1:testdataset1 | bigquery_dataset | organization/1234567890/project/my-project-1/dataset/my-project-1:testdataset1/dataset_policy/dataset/my-project-1:testdataset1/ | 0 | Search for datasets accessible by users with gmail.com addresses | BIGQUERY_VIOLATION | {u'access_user_by_email': u'my_test_acc_1@gmail.com', u'access_special_group': u'allAuthenticatedUsers', u'access_domain': u'', u'access_group_by_email': u'', u'role': u'READER', u'full_name': u'organization/1234567890/project/my-project-1/dataset/my-project-1:testdataset1/dataset_policy/dataset/my-project-1:testdataset1/', u'dataset_id': u'my-project-1:testdataset1', u'view': {}} |

### Cloudsql violation

{: .table .table-striped}
| resource_id | resource_type | full_name | rule_index | rule_name | violation_type | violation_data |
|---|---|---|---|---|---|---|
| my-project-1 | cloudsql | organization/1234567890/project/my-project-1/cloudsqlinstance/readme1/  | 0  |  publicly exposed instances | CLOUD_SQL_VIOLATION | {u'instance_name': u'readme1', u'require_ssl': False, u'project_id': u'readme1', u'authorized_networks': [u'0.0.0.0/0'], u'full_name': u'organization/1234567890/project/my-project-1/cloudsqlinstance/readme1/'} |

To receive notifications for violations, run the 
[Forseti Notifier]({% link _docs/latest/use/notifier.md %}).
