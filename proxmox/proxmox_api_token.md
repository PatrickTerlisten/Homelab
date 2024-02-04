# API

## Intro

Proxmox VE uses a REST like API, and the whole API is formally defined using JSON Schema. The API uses HTTPS and listens on the same port as the web interface (8006/tcp). The base URL is always `https://your.server:8006/api2/json`.

Parameters can be passed using standard techniques using 

- the URL, and
- using x-www-form-urlencoded content-type for PUT and POST request

Proxmox VE uses a ticket or token based authentication. All request to the API need to include a ticket inside a Cookie (header) or sending an API token through the Authorization header. THe API token allows stateless access to most parts of the REST API. An API token is necessary when using Terraform to deploy VMs.

## Create API Token

To create an API token for the root@pam account:

```bash
root@proxmox:~# pveum user token add root@pam terraform-api-token
┌──────────────┬──────────────────────────────────────┐
│ key          │ value                                │
╞══════════════╪══════════════════════════════════════╡
│ full-tokenid │ root@pam!terraform-api-token         │
├──────────────┼──────────────────────────────────────┤
│ info         │ {"privsep":1}                        │
├──────────────┼──────────────────────────────────────┤
│ value        │ 930ed580-xxxx-xxxx-xxxx-fca6b2689aec │
└──────────────┴──────────────────────────────────────┘
```

## Test API Token

A PowerShell module für Proxmox VE is available from the PowerShell Gallery. You can test your API token using the PowerShell Module.

```powershell
 p.terlisten@x1c-g10 ~ Install-Module -Name Corsinvest.ProxmoxVE.Api -Scope CurrentUser

Untrusted repository
You are installing the modules from an untrusted repository. If you trust this repository, change its InstallationPolicy value by running the Set-PSRepository cmdlet.
Are you sure you want to install the modules from 'PSGallery'?
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "N"): Y
 p.terlisten@x1c-g10 ~ Connect-PveCluster -HostsAndPorts proxmox.fritz.box:8006 -SkipCertificateCheck -ApiToken root@pam!terraform-api-token=930ed580-xxxx-xxxx-xxxx-fca6b2689aec

HostName             : proxmox.fritz.box
Port                 : 8006
SkipCertificateCheck : True
Ticket               :
CSRFPreventionToken  :
ApiToken             : root@pam!terraform-api-token=930ed580-xxxx-xxxx-xxxx-fca6b2689aec

p.terlisten@x1c-g10 ~ Get-PveNode

status      : online
node        : proxmox
level       :
type        : node
id          : node/proxmox
cgroup-mode : 2
```

As you can see, the API token was used to connect to the Proxmox VE node. I will use the same approach when it comes to deploy infrastructure using Terraform.