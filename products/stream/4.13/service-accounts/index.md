# Service Accounts


Cribl [internal logs](internal-logs) identify `admin` and `system` users, which perform certain administrative and automated tasks while maintaining security and efficiency.

Both users are critical for auditing and troubleshooting. Always correlate `admin` and `system` user actions in logs with expected administrative and automation events. In Distributed deployments, these users may appear in logs across multiple nodes.

## `admin` User

The `admin` user is a privileged account with full access to Cribl. The built-in `admin` user performs automated operations and orchestration tasks on your behalf. Logs and audit trails record the `admin` user's actions as `"user":"admin"` as shown in this example from `access.log`:
 
```json
{"time":"2025-07-08T15:25:19.561Z","src":"127.0.0.1","user":"admin","method":"GET","url":"/api/v1/system/teams/virtualization/users","status":200,"message":"GET /api/v1/system/teams/virtualization/users","response_time":1,"requestId":"1a2b3c4d-5e6f-7g8h-9i0j-k1l2m3n4o5p6","http_user_agent":"cribl"}
{"time":"2025-07-08T16:10:27.208Z","src":"127.0.0.1","user":"admin","method":"PATCH","url":"/api/v1/system/users/samlp%7C-silence-zebras-1234abcd%7Cnico.jones%40example.com","status":200,"message":"PATCH /api/v1/system/users/samlp%7C-silence-zebras-1234abcd%7Cnico.jones%40example.com","response_time":1785,"requestId":"a1b2c3d4-e5f6-7g8h-9i0j-k1l2m3n4o5p6","http_user_agent":"cribl"}
{"time":"2025-07-08T16:10:27.280Z","src":"127.0.0.1","user":"admin","method":"DELETE","url":"/api/v1/products/edge/users/__cache__","status":200,"message":"DELETE /api/v1/products/edge/users/__cache__","response_time":1,"requestId":"123e4567-e89b-12d3-a456-426614174000","http_user_agent":"cribl"}
{"time":"2025-07-08T16:10:27.596Z","src":"127.0.0.1","user":"admin","method":"DELETE","url":"/api/v1/auth/users/samlp%7C-silence-zebras-1234abcd%7Cnico.jones%40example.com/token","status":200,"message":"DELETE /api/v1/auth/users/samlp%7C-silence-zebras-1234abcd%7Cnico.jones%40example.com/token","response_time":1,"requestId":"0a1b2c3d-4e5f-6g7h-8i9j-k0l1m2n3o4p5","http_user_agent":"cribl"}
```

In on-prem Cribl deployments, the `admin` account is the default [local user](local-users). Limit interactive login as the `admin` user to initial setup and emergency access. Create individual named accounts for users who need administrative access and use role-based access control to assign the **Admin** [Permission](permissions).

## `system` User

The `system` user is an internal service account that Cribl uses exclusively for automated, background, and system-level operations in which there is no human user context. Such operations include invalidating cache, executing jobs, collecting results, and sending internal notifications. The `system` user is not available for interactive login.

Logs and audit trails record the `system` user's actions as `"user":"system"` as shown in this example from `access.log`:

```json
{"time":"2025-07-05T20:45:03.569Z","src":"127.0.0.1","user":"system","method":"POST","url":"/api/v1/m/default_search/search/jobs/Cribl_Leader_audit_logs_for_all_actions.1234567891011.AbCdEF/dispatch-executors","status":200,"message":"POST /api/v1/m/default_search/search/jobs/Cribl_Leader_audit_logs_for_all_actions.1234567891011.AbCdEF/dispatch-executors","response_time":184,"requestId":"0a1b2c3d-4e5f-6g7h-8i9j-k0l1m2n3o4p5"}
{"time":"2025-07-05T20:45:03.578Z","src":"127.0.0.1","user":"system","method":"POST","url":"/api/v1/m/default_search/search/jobs/all_access.1234567891011.1aB2DE/dispatch-executors","status":200,"message":"POST /api/v1/m/default_search/search/jobs/all_access.1234567891011.1aB2DE/dispatch-executors","response_time":111,"requestId":"123e4567-e89b-12d3-a456-426614174000"}
```
