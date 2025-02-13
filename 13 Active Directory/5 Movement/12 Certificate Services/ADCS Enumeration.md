# ADCS Enumeration

## UNIX-like

```powershell
# Using nxc to enumerate existence
nxc ldap $IP -u $USER -p '$PASSWD' -M adcs

# Using certipy to enumerate details
certipy find -u $USER@$DOMAIN -p '$PASSWD' -dc-ip $IP -stdout
KRB5CCNAME=./$TICKET.ccache certipy find -u $USER@$DOMAIN -k -no-pass -target $FQDN -stdout
```

### Understanding Output

Baseline attributes required to request tickets:

- `Enrollment Rights`: Contains a group we have access to in order for us to enroll.
- `Requires Manager Approval`: `False`
- `Authorized Signature Required`: `0`
- `Client Authentication`: `True` or `Extended Key Usage`: `Client Authentication`/any OID that enables authentication

|Description|OID|
|---|---|
|Client Authentication|1.3.6.1.5.5.7.3.2|
|PKINIT Client Authentication*|1.3.6.1.5.2.3.4|
|Smart Card Logon|1.3.6.1.4.1.311.20.2.2|
|Any Purpose|2.5.29.37.0|
|SubCA|(no EKUs)|

**Note:** The `1.3.6.1.5.2.3.4 OID` requires manual addition in AD CS deployments but it can be used for client authentication.

**Escalation specific indicators:**

- **Certificate Templates:**
	- ESC1: `Enrollee Supplies Subject`: `True` - Allows requestors to specify a `subjectAltName (SAN)` in the `CSR`.
	- ESC2: `Extended Key Usage`: `Any Purpose`/undefined - Allows requestors to use the certificate for any purpose.
	- ESC3: `Extended Key Usage`: `Certificate Request Agent` - Allows requestors to request certificates on behalf of any other user.
	- **Certificate Mapping:**
		- ESC9: `Enrollment Flag`: `NoSecurityExtension` - Negates the embedding of the `szOID_NTDS_CA_SECURITY_EXT` security extension, allowing a user to set their `User Principal Name (UPN)` to that of another user and request a certificate with their credentials.
		- ESC10: `StrongCertificateBindingEnforcement` registry key is set to `0` (Kerberos auth), or `CertificateMappingMethods` registry key is set to `0x4` (Schannel auth), each indicating no strong mapping for their associated auth type (not viewable as a low-privileged user - must be tested for).
- **CA Configuration:**
	- ESC6: `User Specified SAN`: `Enabled` - Allows requestors to specify a `subjectAltName (SAN)` in the `CSR` for any certificate template.
- **Access Control:**
	- ESC4: `Object Control Permissions`: `<USER_WE_CONTROL>` - Allows us to modify the template and enable other escalation methods such as ESC1.
	- ESC7: `ManageCa`: `<USER_WE_CONTROL>` or `ManageCertificates`: `<USER_WE_CONTROL>`, allowing us to either enable SAN specification in any template via the `EDITF_ATTRIBUTESUBJECTALTNAME2` bit (ESC6) or bypass the protection of `CA certificate manager approval` respectively.
	- ESC5: Not shown by Certipy - Leverages privileges directly over the ADCS machine itself.
- **NTLM Relay:**
	- ESC8: `Web Enrollment`: `Enabled` - Allows relaying NTLM authentication to a HTTP web enrollment endpoint.
	- ESC11: `Enforce Encryption for Requests`: `Disabled` & `Request Disposition`: `Issue` - Allows relaying NTLM authentication to a ICPR enrollment endpoint.

## Windows

```powershell
# Check for existence with built-in "Cert Publishers" group, any ADCS servers will be a member of this group
net localgroup "Cert Publishers"

# Find vulnerable template information
.\Certify.exe find /vulnerable

# Find information on registered CAs
.\Certify.exe cas
```

REDACTED