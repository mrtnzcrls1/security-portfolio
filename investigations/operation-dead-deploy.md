# Operation Dead Deploy

## Scenario

I investigated an unauthorized test deployment in a live multi-user Azure training tenant. My goal was to identify the suspicious resource, trace how it was deployed, and determine why Azure governance allowed the deployment to succeed.

## Environment

- Microsoft Azure
- Live multi-user Azure training tenant
- Azure Resource Manager
- Azure Policy
- Reader access

## Investigation

1. Reviewed the Azure resource groups and identified one that did not follow the expected naming convention.
2. Inspected the resource and its tags to understand its ownership and purpose.
3. Checked the resource group's deployment history to trace how and when the resource was created.
4. Reviewed the Azure Policy assignment and found that the naming policy was auditing violations rather than blocking them.

## What broke / what surprised me

I was surprised that an Azure Policy could detect a naming violation but still allow the resource to be created. I learned that an Audit policy records the violation, while a Deny policy would actually block the non-compliant deployment.

## Findings and recommendations

The investigation found that the resource violated the expected naming convention, but the Azure Policy was configured to audit violations rather than block them.

Recommendations:
- Change the naming policy from Audit to Deny if non-compliant resources should be prevented.
- Review temporary Contributor access and remove it when it is no longer required.
- Require appropriate tags during deployment to improve ownership and accountability.

## What I learned

- I learned how Azure resources are organized through subscriptions, resource groups, and individual resources.
- I learned how tags can provide useful information about ownership and the purpose of a resource.
- I learned how deployment history can be used to investigate when and how a resource was created.
- I learned the difference between an Audit policy, which records violations, and a Deny policy, which prevents non-compliant deployments.
- If I did this investigation again, I would check the policy configuration earlier, after identifying a non-compliant resource.
