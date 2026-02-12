---
name: my-life-feed
description: Manage My Life Feed things and groups via the MyFeed REST API.
metadata: {"clawdbot":{"emoji":"📋","requires":{"bins":["jq"],"env":["Myfeed_API_KEY"]}}}
---

# My Life Feed Skill

Add things for friends and groups, list my groups

## Setup

1. Get your API key: ask the owner to get it from My Life Feed app
2. Set environment variables:   
   ```bash
   export MyFeed_API_KEY="your-api-key"
   ```

## Usage

All commands use curl to hit the My Life Feed REST API.

### Create thing and invite a friend

```bash
curl -X POST https://skill.myfeed.life/api -H "Authorization: ApiKey $MyFeed_API_KEY" -H "Content-Type: application/json" 
-d '{"request":"create_thing",
 "params":{
   "description":"Thing description", 
   "start_time": Thing starttime in epoch,
   "alarms":[
     {
        "type": "minutes / hours / days / weeks / months",
        "value": how many units
     }
    ],
   "invites": [
      {"phone_number":"Friend phone number"}
    ]
   
 }
}'
```

### List groups and receive group id
```bash
curl -X POST https://skill.myfeed.life/api -H "Authorization: ApiKey $Myfeed_API_KEY" -H "Content-Type: application/json" -d '
{
 "request":"get_groups",
 "params":{
   "starting_from": 1739383324000
   }
}'| jq '.groups[] | {group_id,url_group,is_admin}'
```

### Create thing and invite a group
```bash
curl -X POST https://skill.myfeed.life/api -H "Authorization: ApiKey $Myfeed_API_KEY" -H "Content-Type: application/json" 
-d '{"request":"create_thing",
 "params":{
   "description":"Thing description", 
   "start_time": Thing starttime in epoch,
   "alarms":[
     {
        "type": "minutes / hours / days / weeks / months",
        "value": how many units
     }
    ],
   "invites": [
      {"group_id":group_id }
    ]
   
 }
}'
```
## Notes

- Group Id can be found by listing the groups with a certain name
- The API key and token provide full access to your My Life Feed / MyFeed account - keep them secret!
- Rate limits: 3 requests per 10 seconds per API key; 

## Examples

```bash
#Get the group id by group name
curl -X POST https://skill.myfeed.life/api -H "Authorization: ApiKey $Myfeed_API_KEY" -H "Content-Type: application/json" -d '
{
 "request":"get_groups",
 "params":{
   "starting_from": 1739383324000
   }
}'| jq '.groups[] | select(.group|contains ("group name"))'
#
```
