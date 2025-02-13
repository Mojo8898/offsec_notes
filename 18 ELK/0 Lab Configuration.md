# Lab Configuration

## Docker Setup

I've elected to use [this](https://github.com/peasead/elastic-container) repo to automate the setup and configuration of elastic. Following the guide is smooth from within kali, I just had to change the line:

```bash
$(. /etc/os-release && echo "$VERSION_CODENAME")
```

To:

```bash
bookworm stable
```

I've also changed the default passwords in the `.env` file as specified in the repo along with setting the default OS detections to Windows.

### Container Management

```bash
# Start elastic containers
./elastic-container.sh start

# Check status of running containers
./elastic-container.sh status

# Stop containers without deleting
./elastic-container.sh stop

# Restart containers
./elastic-container.sh restart

# Download all container images but don't start them
./elastic-container.sh stage

# Reset and delete containers and volumes
./elastic-container.sh destroy
```

### Target Enrollment

I've elected to focus testing on a Windows Server 2019 DC.

According to the repo, we first need to grab an enrollment token via the following command.

```bash
curl -k --request GET --url 'https://localhost:5601/api/fleet/enrollment_api_keys' -u elastic:'Password123!' --header 'Content-Type: application/json' --header 'kbn-xsrf: xx'
```

With this token, we can enroll our Windows Server 2019 DC target with the following commands.

```powershell
$ProgressPreference = 'SilentlyContinue'
Invoke-WebRequest -Uri https://artifacts.elastic.co/downloads/beats/elastic-agent/elastic-agent-8.12.2-windows-x86_64.zip -OutFile elastic-agent-8.12.2-windows-x86_64.zip
Expand-Archive .\elastic-agent-8.12.2-windows-x86_64.zip -DestinationPath .
cd elastic-agent-8.12.2-windows-x86_64
.\elastic-agent.exe install --url=https://192.168.1.18:8220 --insecure -f --enrollment-token=<api_key>
```
