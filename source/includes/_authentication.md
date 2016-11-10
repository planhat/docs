# Authentication
```shell
# For and-point that require authentication, simply pass the correct header with each request
curl https://api.planhat.com/myprofile
  -H "Authorization: Bearer [PERSONAL_ACCESS_TOKEN]"
```

> Replace `[PERSONAL_ACCESS_TOKEN]` with your API key.

Most end-points require authentication by a Personal Access Token, managed from the Settings section in Planhat. Personal Access Tokens are static tokens created by you from within the App. Once a token is created, it will last forever. To disable access to a token, simply remove it.

Access tokens are personal, meaning that whatever automatic operation you perform with this token, will appear as if the user in question did it. Consider creating a separate API user and generate tokens for that user to keep things separated.

The Token should be placed in the Authorization header in ONE of the following ways:

`Authorization: Bearer [PERSONAL_ACCESS_TOKEN]`

OR

`Authorization: Basic [base64encode(PERSONAL_ACCESS_TOKEN:)]`

<aside class="notice">
Note the colon after the token if you go with the Basic approach. It’s required even though there is no password.
</aside>
