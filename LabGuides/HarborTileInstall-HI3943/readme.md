# Harbor Tile Installation

- [Step 1: Install Harbor]()
- [Step 2: Enable Harbor Client Secure Connections]()

## Install Harbor

1.1 On a new browser tab, open a connection to the Ops Manager UI, click on `Import a Product` select the Harbor file and click `Open`. It can take a few minutes to import the Harbor file

<details><summary>Screenshot 1.1.1 </summary>
<img src="Images/2018-10-22-21-23-55.png">
</details>

<details><summary>Screenshot 1.1.2 </summary>
<img src="Images/2018-10-22-01-27-45.png">
</details>
<br/>

1.2 In the left hand column of the Ops Manager homepage under `VMware Harbor Registry`, click on the `+` icon to add the Harbor tile to the `Installation Dashboard`

<details><summary>Screenshot 1.2 </summary>
<img src="Images/2018-10-22-21-45-54.png">
</details>
<br/>

1.3 Click on the `VMware Harbor Registry` tile to open its configuration settings

<details><summary>Screenshot 1.3 </summary>
<img src="Images/2018-10-22-21-47-27.png">
</details>
<br/>

1.4 Select the `Assign AZs and Networks` tab and enter the following values:

- Singleton Availability Zone: PKS-MGMT-1
- Balance other jobs in: PKS-MGMT-1
- Network: PKS-MGMT
- Click `Save`

<details><summary>Screenshot 1.4</summary>
<img src="Images/2018-10-22-21-53-32.png">
</details>
<br/>

1.5 Select the `General` tab and set the `Hostname` to `harbor.corp.local` , Click `Save`

<details><summary>Screenshot 1.5</summary>
<img src="Images/2018-10-22-21-57-03.png">
</details>
<br/>

1.6 Select the `Certificate` tab, select `Generate RSA Certificate`, enter `harbor.corp.local` and click `Generate`

<details><summary>Screenshot 1.6</summary>
<img src="Images/2018-10-31-15-24-23.png">
</details>
<br/>

1.7 On the `Certificate` tab, click `Save`

<details><summary>Screenshot 1.7</summary>
<img src="Images/2018-10-22-22-11-03.png">
</details>
<br/>

1.8 On the `Credentials` tab, set the `Admin Password` to `VMware1!` and click `Save`

<details><summary>Screenshot 1.8</summary>
<img src="Images/2018-10-22-22-13-53.png">
</details>
<br/>

1.9 On the `Resource Config` tab, set the `Persistent Disk Type` to `20 GB`

<details><summary>Screenshot 1.9</summary>
<img src="Images/2018-10-22-22-18-57.png">
</details>
<br/>

**STOP**: Before proceeding, ensure that the PKS tile deployment has completed.  There will be a blue bar across the top that will show `Applying Changes` and a button for `Show Progress` as it continues to apply

1.10 In the Ops Manager UI on the top menu bar click `Installation Dashboard`, next select `Review Pending Changes`. Uncheck the checkbox by `Pivotal Container Service` and click `Apply Changes`. Monitor the `Applying Changes` screen until the deployment is complete

<details><summary>Screenshot 1.10</summary>
<img src="Images/2019-01-12-02-25-47.png">
</details>

#### You have now completed the Harbor installation

## Enable Harbor Client Secure Connections

Harbor's integration with PKS natively enables the PKS Control Plane and Kubernetes nodes for certificate based communications with Harbor, but most environments have additional external hosts that need to negotiate communication with harbor other than just K8s nodes. For example, developer workstations, pipeline tools,etc

To ensure developer and automated workflows can have secure interaction with Harbor, a certificate should be installed on the client machine

In the following exercise, you  Harbor self-signed certificate on the `cli-vm` host which you will use in later steps to interact with Harbor services

2.1 From the Ops Manager homepage, click on the `VMware Harbor Registry` tile, go to the `Certificate` tab and copy the SSL certificate text from the top textbox

<details><summary>Screenshot 2.1.1</summary>
<img src="Images/2018-10-24-01-50-50.png">
</details>

<details><summary>Screenshot 2.1.2</summary>
<img src="Images/2018-10-24-01-48-15.png">
</details>
<br/>

2.2 From the ControlCenter Desktop, open putty and under `Saved Sessions` connect to `cli-vm`.

<details><summary>Screenshot 2.2 </summary>
<img src="Images/2018-10-23-03-04-55.png">
</details>
<br/>

2.3 Install the cert as a trusted source on the cli-vm by navigating to the `/etc/docker/certs.d/harbor.corp.local` directory (create this dierectory if i doesn't already exist) and creating a `ca.crt` file with the certificate text you copied in the previous step using the following commands:

```bash
mkdir /etc/docker/certs.d/harbor.corp.local
cd /etc/docker/certs.d/harbor.corp.local
nano ca.crt
# Paste the certificate text into nano, save and close the file
systemctl daemon-reload
systemctl restart docker
```

<details><summary>Screenshot 2.3.1</summary>
<img src="Images/2018-10-24-02-12-17.png">
</details>

<details><summary>Screenshot 2.3.2</summary>
<img src="Images/2018-10-24-02-15-15.png">
</details>

#### You have now prepared `cli-vm' for secure communication with Harbor

***End of lab***