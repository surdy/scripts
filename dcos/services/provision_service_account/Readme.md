# Automatically provision a service account for a package

## Intro
This is a script to compute the correct ACLs for package depending upon 
* Package Name (Different packages need different permissions)
* Package Version (Permissions may vary from version to version, SDK 0.57.0 or newer are quota management aware)
* DC/OS version (DC/OS 2.x is quota management aware)
* Service name (If service is installed in quota-limited top-level group)

## Details
* bash script using DC/OS CLI, [jq](https://stedolan.github.io/jq/) and [mustache-cli](https://github.com/quantumew/mustache-cli)
* not battle tested , not for production use. More of a POC or convenience tool for me
* Takes in package name, service name 
* Creates service account, secret and then associates appropriate ACLs with the service account
* `./provision_service_account --help` to get usage
* `--verbose` to show 
* Uses templated ACL values, hardcoded per package version (under packages directory)
* Default service name = package name
* Default service account name = replace `/` with `__` in service name
* Default service account secret name = <service account name/serviceCredential>
* Uses the cluster currently attachced to DC/OS CLI
* Computes DC/OS version
* Computes DC/OS security mode
* Computes if top-level group is `enforceRole=true`
* Assumes`enforceRole=true` for a new top level group unless root `/`
* Package list not exhaustive in terms of packages as well as versions

## Future work 
* This should be part of DC/OS so that user does not have to think
* Perhaps have a productized verison of this script for customers in short term
* Perhaps have a `permissions.json` mustache file in universe packages
* Cosmos component to do the logic this script performs
* Ask superuser for confirmation before performing actions 
* Bonus non-super user can initiate it and the request goesto superuser to approve/deny
