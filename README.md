# Local-File-Inclusion: Wrappers
### 1. Recon

![image](https://user-images.githubusercontent.com/106916011/179256914-37030c33-4e06-4db5-8c48-f33b7af78912.png)

First, I try a well-known Wrappers "php://filter" to see if I can read some sensitive files content of this server. Thus, I construct a payload

```powershell
redis_cmd = """
INFO
quit
"""
I also get a list of Linux filenames, After a few minutes with intruder the result 
