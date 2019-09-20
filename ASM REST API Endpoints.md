### Import NEW Parent ASM Policy via REST API:
file = base64 encoded parent ASM policy. The ASM policies should be in .xml format and need base64 encoded to import properly. 
```
POST https://{{big_ip_mgmt}}/mgmt/tm/asm/tasks/import-policy

{
  "isBase64":"true",
  "file": "PD9<truncated>A8T4="
}
```
### Import NEW Child ASM Policies via REST API:
file = base64 encoded ASM policy. The ASM policies should be in .xml format and need base64 encoded to import properly.   
parentPolicy = path to parent ASM policy (example for a policy called parent)

```
POST https://{{big_ip_mgmt}}/mgmt/tm/asm/tasks/import-policy

{
"parentPolicy": {"fullPath": "/Common/parent"},
"policyType": "security",
"retainInheritanceSettings": "true", 
"isBase64":"true",
"file": "PD9<truncated>A8T4="
}
```

### Re-Import (Overwrite) Parent ASM Policy via REST API:
First, get the unique MD5SUM hash of the ASM policy. Note: this is different hash per F5 device and per policy. Once a policy is created it stays at the same MD5SUM value. 

#### Example:
```
GET https://{{big_ip_mgmt}}/mgmt/tm/asm/policies/


"fullPath": "/Common/parent",
            "policyBuilderParameterReference": {
                "link": "https://localhost/mgmt/tm/asm/policies/LleQwK5hzDY45GXuF8SkYw/policy-builder-parameter?ver=13.1.1"
            },
<truncated>
"id": "LleQwK5hzDY45GXuF8SkYw",
```
Then the MD5SUM ID can be referenced to import a new policy and overwrite an existing one.  
file = base64 encoded parent ASM policy. The ASM policies should be in .xml format and need base64 encoded to import properly. 


```
POST https://{{big_ip_mgmt}}/mgmt/tm/asm/tasks/import-policy

{
  "policyReference": {"link":"https://localhost/mgmt/tm/asm/policies/LleQwK5hzDY45GXuF8SkYw"},
  "isBase64":"true",
  "file": "PD9<truncated>A8T4="
}
```

### Re-Import (Overwrite) Child ASM Policy via REST API:
First, get the unique MD5SUM hash of the ASM policy. Note: this is different hash per F5 device and per policy. Once a policy is created it stays at the same MD5SUM value. 

#### Example:
```
GET https://{{big_ip_mgmt}}/mgmt/tm/asm/policies/


"fullPath": "/Common/child1",
            "policyBuilderParameterReference": {
                "link": "https://localhost/mgmt/tm/asm/policies/7iA7aa_OqF3hql83ug4Gdw/policy-builder-parameter?ver=13.1.1"
            },
<truncated>
"id": "7iA7aa_OqF3hql83ug4Gdw",
```
Then the MD5SUM ID can be referenced to import a new policy and overwrite an existing one.  
file = base64 encoded parent ASM policy. The ASM policies should be in .xml format and need base64 encoded to import properly. 


```
POST https://{{big_ip_mgmt}}/mgmt/tm/asm/tasks/import-policy

{
"parentPolicy": {"fullPath": "/Common/parent"},
"policyReference": {"link":"https://localhost/mgmt/tm/asm/policies/7iA7aa_OqF3hql83ug4Gdw"},
"policyType": "security",
"retainInheritanceSettings": "true", 
"isBase64":"true",
"file": "PD9<truncated>A8T4="
}
```

### Apply a previously imported ASM policy
Using the MD5SUM ID collected before, we can apply the policy. 
```
POST https://{{big_ip_mgmt}}/mgmt/tm/asm/tasks/apply-policy

{
"policyReference": {"link":"https://localhost/mgmt/tm/asm/policies/7iA7aa_OqF3hql83ug4Gdw"}
}
```


