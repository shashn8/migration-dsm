**About**

* This project's purpose is to migrate the DSM data from one sdkms account to another account dsm account in a quick, seamless and secure manner. For convenience, let's refer to the account we are migrating to as the "target account," and the account we are migrating from as the "source account."

**Pre-requisites**

* Fortanix DSM access for both the accounts
* Authentication method: username/password
* An RSA key pair will be used to wrap the security objects on SDKMS	 and unwrap (import) them into the target account. 
* Ensure that "migrate.json" is properly configured.

**config file**

* It has 8 parameters

* "url": The URL in "source" should refer to the source node DSM URL, and in "target," it should refer to the target node DSM URL.
* "username": The username in "source" refers to the source node username, and in "target," it refers to the target node username.
* "account_id": The account_id in "source" refers to the account ID of the source node, and in "target," it refers to the account ID in the target node.
* "wrapping_key_id" & "unwrapping_key_id":
	"wrapping_key_id" in "source" refers to the key ID of the RSA public key in the source DSM node.
	"unwrapping_key_id" in "target" refers to the key ID of the RSA private key in the target DSM node.

Refer to the example JSON data below and input it into "migrate.json":

```
{
  "source": {
    "url": "<source node DSM URL>",
    "username": "<source node username>",
    "account_id": "<source node account ID>",
    "wrapping_key_id": "<RSA public key ID at source>"
  },
  "target": {
    "url": "<target node DSM URL>",
    "username": "<target node username>",
    "account_id": "<target node account ID>",
    "unwrapping_key_id": "<RSA private key ID at target>"
  }
}
```

**How to execute**

* Generate an RSA key in the target node, download, and import the public key into the source node. Enter the UUIDs of these keys in "migrate.json."
* Enter the account IDs, usernames and DSM endpoints in the migrate.json as instructed above
* Create and empty folder named "logs" in the same directoy as the migrate binary file and migrate.json file, if not already present.
* Edit the migrate.json as instructed above.
* Run the following command in the directory with "migrate" binary, "migrate.json" file, and the "logs" folder:
```
./migrate
```
* Enter passwords for source and target nodes when prompted.
* The script will provide console output displaying important migration stats.
* Find a CSV file in the same folder, showing all migrated keys, the groups they belong to, and their IDs at source and target.
* Check the logs of the script in the "logs" folder.


**Algorithm**

* Read "migrate.json"
* Authenticate source and target nodes
* Read admin apps from source and create them in target node
* Delete all custom account roles and custom group roles in the target node.
* Read custom account roles and custom group roles from the source and create them in the target node.
* Read custom account roles and custom group roles from the source and create them in the target node.
* Read the groups from source and create them in the target node
* Read the apps from source and create them in the target node
* Generate an AES key at source node, wrap it with the RSA wrapping key (public key), and unwrap it into the target node using the RSA unwrapping key (private key)
* Use this AES key to wrap all sobjects in the source node, and unwrap them into the target node.
* Delete the AES key generated by the script.
* Read plugins from source and create them in the target node
* Update the groups at target to full working state
* Update the log file in logs folder throughout the runtime
* Build a CSV file with sobject details.
