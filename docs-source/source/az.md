# Azure CLI - az command

The code is designed for using AzProvider.py to access and manage resource in Azure instance and run scripts in it.

In this code, we provide these command for achieving the goal of connecting to Azure services, creating virtual machines on it and manage those created vm.

## Code Description

In the azure folder, there are several basic python files:

### Login and connect to azure

By calling this part of code:

```
login(self):
	r = Shell.execute("az login", shell=True)
```

Then CLI can open your default browser, it will do so and load a sign-in page.


### Create Resource Group

```
def create_resource_group(self, name=None, location=None):
    self.az(f"az group create --name {name} --location {location}")
```

Every virutal machine must run under a specific resource group. After creating, you can list and check all existing resource group:

```
def list_resource_group(self):
    return self.az("az group list")
```

### Create Virtual Machine

```
def create(self, name=None, image=None, size=None, timeout=360, **kwargs):
        username = kwargs["username"]
	 command = \
            "az vm create" \
                f" --resource-group {self.resource_group}" \
                f" --name {name}" \
                f" --image {image}" \
                f" --admin-username {username}" \
                f" --generate-ssh-keys"
return self.az(command)
```

This will create a virual machine with name and it wil return all information related to that virtual machine, like its size, operating system, ip address and so on.

### Start Virtual Machine

```
def start(self, name=None):
        command = \
            f"az vm start" \
                f" --resource-group {self.resource_group}" \
                f" --name {name}"
return self.az_2(command)
```

By calling this method, it will start a virutal machine with name and reutrn a status 1.

### Stop Virtual Machine

```
def stop(self, name=None):
        command = \
            f"az vm stop" \
                f" --resource-group {self.resource_group}" \
                f" --name {name}"
	self.az_2(command)
```

This command will stop a running virutal machine called "name" under specific resource group.

### Delete Virtual Machine

```
def destroy(self, name=None):
        r = self.stop(name=name)
        command = \
            "az vm delete --yes" \
                f" --resource-group {self.resource_group}" \
                f" --name {name}"
	return self.az_2(command)
```

This command will first check if the virtual machine is running or not. If it is running, it will stop that virtual machine first and then delete it.