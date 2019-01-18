# AWS Developer Associate Notes
# IAM
- Consists of:
  - Users
  - Groups
  - Roles
  - Policy Documents

- Universal (does not apply to regions)
- Root account is the first account created (has complete admin access)
- New Users have NO permissions when first created
- New Users are assigned Access Key ID & Secret Access Keys when first created
- They are NOT the same as the password and cannot be used to login
- You can use them to access AWS via APIs and Command Line
- Can only view them once so save them in a secure location
- Always setup MFA for root account
- Password policies can be created
- Use Access keys to access data

- When working on development, you need to use the AWS Access keys to work with the AWS resources

# EC2
## Instances
- On Demand - Pay fixed rate by the hour (or second) with no commitment
Reserved - Capacity reservation and offers a significant discount on the hourly charge for an instance, 1 or 3 year terms

- Spot - Enables bidding for instance capacity, providing greater savings if your applications have flexible start and end times, if the instance is terminated by Amazon EC2 no charge, however you will be charge for the complete hour if you terminate it yourself

- Dedicated Hosts - Physical EC2 server dedicated for use. Can help reduce costs by allowing existing server-bound software licenses to be used
