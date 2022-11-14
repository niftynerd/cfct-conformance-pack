# cfct-conformance-pack
Example of how to add config conformance packs using CfCT - https://medium.com/@geoff1337/deploying-org-wide-aws-conformance-packs-through-cfct-d999b0acce75

Prerequisites — You will need a landing zone for your organization set up through control tower, AWS CLI installed

1. Create an S3 bucket in an account you would like to delegate administrator access for AWS Config to and take note of it’s name e.g. awsconfigconforms-cfct-[0–9*] and upload a Config Conformance Pack Template you’d like to apply to the root of the S3 bucket. Refer to the list of sample templates and download them from github
2. Whilst authenticated as an administrator from your master account in the AWS CLI, run the following command to delegate administrator access for AWS Config where {Admin-Account-ID} is the account number of the delegated account:
aws organizations register-delegated-administrator — service-principal=config-multiaccountsetup.amazonaws.com — account-id=”{Admin-Account-ID}”
Confirm that the delegated admin registered successfully by running the following command from the master account:
aws organizations list-delegated-administrators --service-principal=config-multiaccountsetup.amazonaws.com
3. Follow the guide here to launch the CfCT stack through cloud formation in your master account
4. Checkout the code from the CodeCommit repo that this creates
5. Refer to the manifest.yaml in the github repo for this solution which contains a reference to a resource file and parameter file. Add something similar to the manifest.yaml you just checked out and put the delegated administrator account number in the part that says <accountNo> . Also make sure you update the region: and regions: to the region that your control tower is in
6. Copy over the parameters file organization-conformance-packs.json and resource file organization-conformance-packs.yaml to your checked out code from step 4 and make sure the references to them are correct in your manifest.yaml
7. Update organization-conformance-packs.json from the previous step to have the parameter value of the name of your S3 bucket you created in the first step
8. Update organization-conformance-packs.yaml from step 6 to reference the file name of the template you uploaded in step 1.
9. If you want to apply more conformance packs, simply add more resources for this to your organization-conformance-packs.yaml file and upload the template to your s3 bucket to be referenced
10. Stage and commit your changes and push your code to code commit and the pipeline should run in your master account to deploy the conformance packs in your organization!
