Access Token required because of private repo

https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token


token: ${{ secrets.ACCESS_TOKEN }}



grid clusters aws $CLUSTER --cost-savings --role-arn xxx  --external-id xxx --region us-west-2 --instance-types t2.medium,g4dn.xlarge --edit-before-creation