# Sample Logs for Login Scenarios


This page lists sample event series that Cribl Stream records in common login scenarios. Cribl Stream captures event series for both successful and failed login attempts. Note that these samples omit the following repeating fields that are captured in each log:

- `cid`:`api`
- `channel`:`cribl`
- `level`:`info`

## Successful Authentication Using the `memberOf` Attribute

`{"time":"2022-03-30T03:09:55.569Z","message":"Running LDAP search","searchBase":"dc=mydomain,dc=com","filter":"(sAMAccountName=admin)","options":{"scope":"sub","filter":"(sAMAccountName=admin)"}}`

`{"time":"2022-03-30T03:09:55.575Z","message":"LDAP user search results","count":1,"username":"REDACTED","user":{"dn":"CN=admin,CN=Users,DC=mydomain,DC=com","memberOf":["CN=admingrp,CN=Users,DC=mydomain,DC=com"]}}`

`{"time":"2022-03-30T03:09:55.582Z","channel":"LDAPMapper","level":"debug","message":"External groups found","groups":["admingrp","Users"]}`

`{"time":"2022-03-30T03:09:55.583Z","message":"Successful login","user":"admin","provider":"ldap:win2008ad1.mydomain.com:389"}`

## Successful Authentication With Fallback When a Bad Login Is Triggered

`{"time":"2022-03-30T03:10:48.389Z","message":"Running LDAP search","searchBase":"dc=mydomain,dc=com","filter":"(sAMAccountName=admin)","options":{"scope":"sub","filter":"(sAMAccountName=admin)"}}`

`{"time":"2022-03-30T03:10:48.393Z","message":"LDAP user search results","count":1,"username":"REDACTED","user":{"dn":"CN=admin,CN=Users,DC=mydomain,DC=com","memberOf":["CN=admingrp,CN=Users,DC=mydomain,DC=com"]}}`

`{"time":"2022-03-30T03:10:48.399Z","message":"Attempting fallback to local authentication","reason":"failed login","provider":"ldap:win2008ad1.mydomain.com:389","user":"admin"}`

`{"time":"2022-03-30T03:10:48.400Z","message":"Successful login","user":"admin","provider":"local"}`

## Failed Authentication with Fallback When a Bad Login Is Disabled

`{"time":"2022-03-30T03:11:54.175Z","message":"Running LDAP search","searchBase":"dc=mydomain,dc=com","filter":"(sAMAccountName=admin)","options":{"scope":"sub","filter":"(sAMAccountName=admin)"}}`

`{"time":"2022-03-30T03:11:54.178Z","message":"LDAP user search results","count":1,"username":"REDACTED","user":{"dn":"CN=admin,CN=Users,DC=mydomain,DC=com","memberOf":["CN=admingrp,CN=Users,DC=mydomain,DC=com"]}}`

`{"time":"2022-03-30T03:11:54.184Z","level":"warn","message":"Failed login","user":"admin","provider":"ldap:win2008ad1.mydomain.com:389","details":{"message":"80090308: LdapErr: DSID-0C0903A9, comment: AcceptSecurityContext error, data 52e, v1db1\u0000 Code: 0x31","stack":"Error: 80090308: LdapErr: DSID-0C0903A9, comment: AcceptSecurityContext error, data 52e, v1db1\u0000 Code: 0x31`

## Incorrect Proxy Bind Password With Fallback When a Fatal Error Is Triggered

`{"time":"2022-03-30T03:04:16.400Z","level":"warn","message":"Authentication Provider Error","provider":"ldap:win2008ad1.mydomain.com:389","fatal":{"message":"80090308: LdapErr: DSID-0C0903A9, comment: AcceptSecurityContext error, data 52e, v1db1\u0000 Code: 0x31","stack":"Error: 80090308: LdapErr: DSID-0C0903A9, comment: AcceptSecurityContext error, data 52e, v1db1\u0000 Code: 0x31\n	at Function.parse [snip]`

`{"time":"2022-03-30T03:04:16.401Z","message":"Attempting fallback to local authentication","reason":"provider error","provider":"ldap:win2008ad1.mydomain.com:389","user":"admin","error":{"message":"80090308: LdapErr: DSID-0C0903A9, comment: AcceptSecurityContext error, data 52e, v1db1\u0000 Code: 0x31","stack":"Error: 80090308: LdapErr: DSID-0C0903A9, comment: AcceptSecurityContext error, data 52e, v1db1\u0000 Code: 0x31`

## Successful Authentication Without `memberOf` to Trigger Group Search but Without Group Memberships

`{"time":"2022-03-30T03:15:21.410Z","message":"Running LDAP search","searchBase":"dc=mydomain,dc=com","filter":"(sAMAccountName=admin)","options":{"scope":"sub","filter":"(sAMAccountName=admin)"}}`

`{"time":"2022-03-30T03:15:21.416Z","message":"LDAP user search results","count":1,"username":"REDACTED","user":{"dn":"CN=admin,CN=Users,DC=mydomain,DC=com"}}`

`{"time":"2022-03-30T03:15:21.422Z","message":"Running LDAP search","searchBase":"dc=mydomain,dc=com","filter":"(member=CN=admin,CN=Users,DC=mydomain,DC=com)","options":{"scope":"sub","filter":"(member=CN=admin,CN=Users,DC=mydomain,DC=com)"}}`

`{"time":"2022-03-30T03:15:21.424Z","message":"LDAP groups search results","count":0,"opts":{"groupSearchBase":"dc=mydomain,dc=com","memberSearch":{"memberField":"member","memberDN":"CN=admin,CN=Users,DC=mydomain,DC=com"}},"groups":[]}`

`{"time":"2022-03-30T03:15:21.425Z","channel":"LDAPMapper","level":"debug","message":"External groups found","groups":[]}`

`{"time":"2022-03-30T03:15:21.427Z","message":"Successful login","user":"admin","provider":"ldap:win2008ad1.mydomain.com:389"}`
