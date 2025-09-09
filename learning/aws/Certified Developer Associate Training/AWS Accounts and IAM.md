![[Pasted image 20250205181733.png]]
### IAM Principals
- Must be authenticated to send requests (with a few exceptions).
- A principal is a person or application that can make a request for an action or operation on an AWS resource.
- AWS determines whether to authorize the request (allow/deny)
- Everything in AWS is an API call: RunInstances, GetBucket, CreateUser
- ![[Pasted image 20250205182146.png]]

### IAM User, Groups, Roles and Policies
- User: 
	- Gains the permissions applied to the group through the policy
	- Account root user: Full permissions.
	- Individual user: Up to 5000 accounts. No permissions by default
- Group: 
	- Collection of users. We can create different groups for different users. E.g. admin group, development group, operations group
- Role:
	- It's and IAM identity that has specific permissions.
- Policy: 
	- Are documents that define permissions and are written in JSON

### Create IAM users and groups