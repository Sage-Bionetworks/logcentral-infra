# Overview
Install, configure and manage the
[AWS Sage central logging account](https://sagebionetworks.jira.com/wiki/spaces/IT/pages/577044503/Sage+AWS+Logging).
It is used as a secure account to store logs accross multiple AWS accounts.
The use of a secure logging account is an [established best practice](https://aws.amazon.com/blogs/architecture/central-logging-in-multi-account-environments).
It helps security teams detect malicious activities both in real-time and during incident response.
It provides protection to log data in case it is accidentally or intentionally deleted.  It also
helps application teams correlate and analyze log data across multiple application tiers.

## Cloudtrail
Cloudtrail logs from all Sage accounts are sent to an S3 bucket on Logcentral

## AWS Config
AWS Config data from all Sage accounts are sent to an S3 bucket on Logcentral 

## Guard Duty
Guard duty findings from selected Sage accounts are sent to Guard duty on Logcentral.

## Instructions to create or update CF stacks

```
# unlock repo
git-crypt unlock
# set env vars
source env_vars && source env_vars.secret
# Run sceptre to create or update CF stacks
```

The above should setup resources for the account.  Once the infrastructure for the account has been setup
you can access and view the account using the [AWS console](https://AWS-account-ID-or-alias.signin.aws.amazon.com/console).

*Note - This project depends on CF templates from [aws-infra repo](https://github.com/Sage-Bionetworks/aws-infra).*

## Setup Cloudtrail and Config Aggregation
Steps to setup aggregation from source account to Logcentral
 1. Create a [PR to add policy/permissions](https://github.com/Sage-Bionetworks/logcentral-infra/pull/14)
    to allow another account to send data to logcentral.
 2. Setup cloudtrail and config in source account with [essentails.yaml in aws-infra](https://github.com/Sage-Bionetworks/aws-infra/blob/master/templates/essentials.yaml)
    which will send the logs to a cloudtrail and config bucket in Logcentral.


## Setup Guard Duty Aggregation
Steps to guard duty aggregation from source account to Logcentral
 1. Create a PR to add a member account resource, similar to
    [GDSynapseProdMember resource](https://github.com/Sage-Bionetworks/logcentral-infra/templates/GuardDutyMaster.yaml)
 2. Create a PR to add a new [sceptre stack config](https://github.com/Sage-Bionetworks/synapseprod-infra/pull/9)
    to setup a guard duty member on the source account. 
    

## Continuous Integration
We have configured Travis to deploy CF template updates.  Travis deploys using
[sceptre](https://sceptre.cloudreach.com/latest/about.html)

# Contributions

## Issues
* https://sagebionetworks.jira.com/projects/IT

## Builds
* https://travis-ci.org/Sage-Bionetworks/logcentral-infra

## Secrets
* We use [git-crypt](https://github.com/AGWA/git-crypt) to hide secrets.
Access to secrets is tightly controlled.  You will be required to
have your own [GPG key](https://help.github.com/articles/generating-a-new-gpg-key)
and you must request access by a maintainer of this project.
