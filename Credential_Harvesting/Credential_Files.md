# Credential Files
A collection of file paths where you might happen onto credentials, auth tokens, certificates, and keypairs useful for lateral movement.

## Dev & DevOps

### Github OAUTH Token
*Default path(s)*: `$HOME/.config/hub`, `$XDG_CONFIG_HOME/hub`  

*Description*: Contains Github OAUTH credentials and created by the `hub` tool. Bypasses 2FA/SSO if enabled.

*Using it*: The contents of the file can be copied to your personal machine and used as is with `git` or `hub` commands.

If you want to test the validity of the token run `curl -s -i -H 'Authorization: token <OAUTH_TOKEN>' https://api.github.com/`. A `200 OK` status indicates a valid token while a `401 Unauthorized`  means the token has gone stale and is unusable until the next successful authorization with the `hub` command 

Sample file

```
github.com:
- user: n0ncetonic
  oauth_token: 3a222191f33b2b3f83f8cd2f93c493e2cbdfecec
  protocol: https
```
