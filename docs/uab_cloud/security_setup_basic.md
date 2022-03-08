# Basic Security Setup

Using SSH with your instances requires following the steps on this page. At a minimum, an SSH security group and at least one keypair must be created. Other security groups can and should be added as needed.

## Creating a Security Group

These instructions show you how to prepare to use SSH with your instances. Security Groups are used to set rules for how external devices can connect to our instances. Here we will create an SSH Security Group using a method that can be applied to other types of connections. The method used can be applied to other types of Security Groups as well.

1. Click "Networks" in the left-hand navigation pane to open the fold-out menu.

    ![!OpenStack Overview page. Security Groups is selected in the Network Topology fold-out menu in the left-hand navigation pane. ><](./images/networks_000.png)

2. Click "Security Groups" in the fold out menu.

    ![!OpenStack Security Groups page. The Security Groups table has one entry, the default, persistent entry labeled default. ><](./images/security_groups_001.png)

3. Click "+ Create Security Group" to open a dialog box.

4. Fill out the dialog box.

    1. Under "Name" enter `ssh`.
    2. Leave "Description" empty.

    ![!Create Security Group dialog. The dialog has been filled out with the name set as ssh. ><](./images/security_groups_002.png)

5. Click "Create Security Group".

    1. Redirects to the "Manage Security Group Rules: ssh" page.
    2. There should be an entry for "Egress IPv4" and "Egress IPv6". Leave these alone.

    ![!Manage Security Group Rules for ssh. The Table has two entries, both Egress direction. One is for IPv4 and the other for IPv6. Both have no IP restrictions. ><](./images/security_groups_003.png)

6. Click "+ Add Rule" to open a dialog box.

    1. Select "SSH" in the "Rule" drop down box. This will change the remaining fields.
    2. Leave "Description" empty.
    3. Select "CIDR" in the "Remote" drop down box.
    4. Type `0.0.0.0/0` in the "CIDR" box. **WARNING!** This is **NOT** good practice! For your research instances, you'll want to constrain the CIDR value further to a narrower range of IP addresses. The rule we have shown here leaves the SSH port open to all IP addresses world-wide.

    ![!Add Rule dialog box. The dialog box is filled out. The rule is set to SSH. ><](./images/security_groups_004.png)

7. Click "Add".

    1. Redirects to the "Manage Security Group Rules: ssh" page.
    2. There should be a new entry in the table.

    ![!Manage Security Group Rules for ssh. The Table has three entries. The new entry is Ingress direction with IPv4. It is restricted to TCP port 22 on all IPs. ><](./images/security_groups_005.png)

## Creating a Key Pair

A Key Pair is required for SSH access to OpenStack instances for security reasons.

Using a password protected Key Pair is highly recommended for additional security, as it buys time to revoke a key if it is compromised by an attacker. Currently, this is only possible by uploading a custom public key generated on your local machine.

Good practice is to only use one key pair per person and per local machine. So if you have two computers, each one will need its own key pair. If you have two users, each will need their own key pair. Private keys are secrets and should not be passed around. Copying the key increases the risk of the system being compromised by an attacker.

1. Click "Compute" in the left-hand navigation pane to open the fold-out menu.

    ![!OpenStack Overview page. Key Pairs is selected in the Compute fold-out menu in the left-hand navigation pane. ><](./images/key_pairs_000.png)

2. Click "Key Pairs".

    ![!Key Pairs page. The Key Pairs table is empty. ><](./images/key_pairs_001.png)

3. Click "+ Create Key Pair" to open a dialog box.

4. Fill out the dialog box.

    1. Enter a "Key Pair Name".
    2. Select "SSH Key" in the "Key Type" drop down box.

        ![!Create Key pair dialog. The dialog form is filled out. The Key Pair Name is set to my_key_pair. ><](./images/key_pairs_002.png)

5. Click "+ Create Key Pair"

    1. Opens a download file dialog box in your browser to download a `pem` file containing the secret private key.
    2. Download the `pem` file. For security reasons this will be your only chance to ever obtain the private key from OpenStack.
    3. Failing to download the `pem` file now means a new key pair will need to be created.

        ![!Download File dialog on Firefox for Windows. The file being downloaded is my_key_pair.pem. ><](./images/key_pairs_003.png)

    4. Redirects to the "Key Pairs" page.
    5. There should be a new entry in the table.

        ![!Key Pairs page. The Key Pairs table has one entry labeled my_key_pair. ><](./images/key_pairs_004.png)

6. To use the private key on your local machine.

    1. `mv` the `pem` file to the `.ssh` directory under your home directory. If you are on a Windows machine, you'll need to install ssh by one of various means.
    2. `cd` to the `.ssh` directory under your home directory.
    3. `ssh-add <pem_file>` to add the private key to the ssh keyring for use by ssh.
    4. `ssh-add -d <pem_file>` to remove the key.

        ![!MINGW64 terminal on Windows. Commands have been used to move the private key file into the ssh folder and add it to the ssh agent. ><](./images/key_pairs_005.png)

<!-- markdownlint-disable MD046 -->
!!! note

    It is alternately possible to use a custom key pair created on your local machine. We assume you know how to create a key pair on your local machine and have already done so. To upload a key pair, replace steps 3 and 4 above with the following, perform step 5 from above, and skip step 6.

    3\. Click "Import Public Key" to open a dialog box.

    4\. Fill out the dialog box.

    - Enter a "Key Pair Name".
    - Select "SSH Key" in the "Key Type" drop-down box.
    - Click "Browse..." to upload a public key file from your custom key pair **OR** copy-paste the content of that key file into the "Public Key" box.

    ![!Import Public Key dialog. The dialog form is empty. ><](./images/key_pairs_alt_002.png)
<!-- markdownlint-disable MD046 -->