## 1 Understand Infrastructure as Code (IaC) concepts

### 1a Explain what IaC is

#### Resource Graph
- Terraform builds a graph of all your resources. 
- Terraform parallelizes the creation and modification of any non-dependent resources. 
- Because of this, Terraform builds infrastructure as efficiently as possible, and operators get insight into dependencies in their infrastructure.
- Default: 10 nodes at a time. Limit the number of concurrent nodes walked when applying with [`terraform apply -parallelism-n`](https://www.terraform.io/docs/commands/apply.html#parallelism-n)

#### Change Automation
- With the *execution plan* and *resource graph*, you know exactly what Terraform will change and in what order, avoiding many possible human errors.

### 1b Describe advantages of IaC patterns

/ https://www.hashicorp.com/blog/infrastructure-as-code-in-a-private-or-public-cloud/#iac-makes-infrastructure-more-reliable

## 2 Understand Terraform's purpose (vs other IaC)

### 2a Explain multi-cloud and provider-agnostic benefits
- By using only a single region or cloud provider, fault tolerance is limited by the availability of that provider.
- Having a multi-cloud deployment allows for more graceful recovery of the loss of a region or entire provider.
- Realizing multi-cloud deployments can be very challenging as many existing tools for infrastructure management are cloud-specific. 
- Terraform is cloud-agnostic and allows a single configuration to be used to manage multiple providers, and to even handle cross-cloud dependencies.

### 2b Explain the benefits of state

#### Mapping to the Real World
- For some providers like AWS, Terraform could theoretically use something like AWS tags. 
- Early prototypes of Terraform actually had no state files and used this method. 
- However, they quickly ran into problems. The first major issue was a simple one: not all resources support tags, and not all cloud providers support tags.

#### Metadata
- Alongside the mappings between resources and remote objects, Terraform must also track metadata such as [resource dependencies](#resource-graph).
- To ensure correct operation, Terraform retains a copy of the most recent set of dependencies within the state. 
- Now Terraform can still determine the correct order for destruction from the state when you delete one or more items from the configuration.
- Terraform also stores other metadata for similar reasons, such as a pointer to the provider configuration that was most recently used with the resource in situations where multiple aliased providers are present.

#### Performance
- Terraform stores a cache of the attribute values for all resources in the state. 
- This is the most optional feature of Terraform state and is done only as a performance improvement.
- When running a `terraform plan`, Terraform must know the current state of resources in order to effectively determine the changes that it needs to make to reach your desired configuration.
- For *small infrastructures,* the **default behavior of Terraform** is: 
  > For every plan and apply, Terraform queries your providers and syncs the latest attributes from all resources in your state.
- For *larger infrastructures*, the default of querying every resource is too slow. 
  * Many cloud providers do not provide APIs to query multiple resources at once
  * The round trip time for each resource is 100s of milliseconds
  * On top of this, cloud providers almost always have API rate limiting so Terraform can only request a certain number of resources in a period of time. 
- Larger users of Terraform make heavy use of the `-refresh=false` flag as well as the `-target` flag in order to work around this, e.g.
  * [`terraform apply -refresh=false`](https://www.terraform.io/docs/commands/apply.html#refresh-true) does not update the state for each resource prior to applying. (This has no effect if a plan file is given directly to apply.)
  * [`terraform plan -target=resource`](https://www.terraform.io/docs/commands/plan.html#target-resource) only focuses Terraform's attention on the specified resource. Since it can be used multiple times, you can create a single plan file for multiple targets.
  * [`terraform apply -target=resource`](https://www.terraform.io/docs/commands/apply.html#target-resource) only applies specified resources from Terraform's state.
- In these scenarios, the cached state is treated as the record of truth.

/ Resource Targeting https://www.terraform.io/docs/commands/plan.html#resource-targeting


#### Syncing
* For solo use, Terraform creates a state file in the local directory.
* This local file can lead to conflicts when a team is working with the same state.
* To avoid conflicts, using remote state enables teams to benefit from state locking.

## 3 Understand Terraform basics

### 3a Handle Terraform and provider installation and versioning

#### Providers
**providers** → define resource *types* → entail *behaviors* of resources → **resources** (primary construct in TF)
- Each provider offers a set of named resource types defines for each resource type:
  * which arguments it accepts
  * which attributes it exports
  * how changes to resources of that type are actually applied to remote APIs
- Most of the available providers correspond to one cloud or on-premises infrastructure platform, and offer resource types that correspond to each of the features of that platform.
- Providers usually require some configuration of their own to specify endpoint URLs, regions, authentication settings, and so on. 
- All resource types belonging to the same provider will share the same configuration, avoiding the need to repeat this common information across every resource declaration.

#### Provider Configuration
- A provider configuration is created using a provider block:
  ```hcl
  provider "google" {
    project = "acme-app"
    region  = "us-central1"
  }
  ```
- Terraform associates each resource type with a provider by taking the *first word of the resource type name*, e.g. 
  - the "google" provider is assumed to be the provider for the resource type name `google_compute_instance`.
  - the "aws" provider is assumed for `aws_instance`.
- Since provider configurations must be evaluated in order to perform any resource type action, provider configurations may refer only to values that are known before the configuration is applied. 
- In particular, avoid referring to attributes exported by other resources unless their values are specified directly in the configuration.

#### Initialization
- Provider initialization is one of the actions of `terraform init`. Running this command will download and initialize any providers that are not already initialized.
- Providers downloaded by `terraform init` are only installed for the *current working directory*; other working directories can have their own installed provider versions.
- Note that `terraform init` *cannot automatically download providers that are not distributed by HashiCorp*.

#### Provider Versions
- Providers are plugins released on a separate rhythm from Terraform itself, and so they have their own version numbers. 
- For production use, you should constrain the acceptable provider versions via configuration, to ensure that new versions with breaking changes will not be automatically installed by terraform init in future.
- When `terraform init` is run *without provider version constraints*, it prints a suggested version constraint string for each provider:
  ```
  The following providers do not have any version constraints in configuration,
  so the latest version was installed.

  To prevent automatic upgrades to new major versions that may contain breaking
  changes, it is recommended to add version = "..." constraints to the
  corresponding provider blocks in configuration, with the constraint strings
  suggested below.
  
  * provider.aws: version = "~> 1.0"
  ```
- To constrain the provider version as suggested, add a required_providers block inside a terraform block:
  ```hcl
  terraform {
    required_providers {
      aws = "~> 1.0"
    }
  }
  ```
- When `terraform init` is re-run with providers already installed, it will use an already-installed provider that meets the constraints in preference to downloading a new version. 
- To upgrade to the latest acceptable version of each provider, run `terraform init -upgrade`. This command also upgrades to the latest versions of all Terraform modules.

#### Writing Custom Providers

/ Writing Custom Providers https://www.terraform.io/docs/extend/writing-custom-providers.html

#### Terraform Settings

##### Configuring a Terraform Backend
- Most non-trivial Terraform configurations will have a backend configuration that configures a remote backend to allow collaboration within a team.
- A backend configuration is given in a nested backend block within a terraform block:
  ```hcl
  terraform {
    backend "s3" {
      # (backend-specific settings...)
    }
  }
  ```
  
##### Specifying a Required Terraform Version
- The `required_version` setting can be used to constrain which versions of the Terraform CLI can be used with your configuration. 
- If the running version of Terraform doesn't match the constraints specified, Terraform will produce an error and exit without taking any further actions.
- When you use child modules, each module can specify its own version requirements. 
  * The requirements of all modules in the tree must be satisfied.
- The `required_version` setting applies only to the version of Terraform CLI. 
- Various behaviors of Terraform are actually implemented by Terraform Providers, which are [released on a cycle independent of Terraform CLI and of each other](#provider-versions). 
- Re-usable modules should constrain only the minimum allowed version, such as `>= 0.12.0`. 
  * This specifies the earliest version that the module is compatible with while leaving the user of the module flexibility to upgrade to newer versions of Terraform without altering the module.

##### Experimental Language Features
- From time to time the Terraform team will introduce new language features initially via an opt-in experiment, so that the community can try the new feature and give feedback on it prior to it becoming a backward-compatibility constraint.
- In releases where experimental features are available, you can enable them on a per-module basis by setting the experiments argument inside a terraform block:
  ```hcl
  terraform {
    experiments = [example]
  }
  ```
- The above would opt in to an experiment named example, assuming such an experiment were available in the current Terraform version.
  * Experiments are subject to arbitrary changes in later releases and, depending on the outcome of the experiment, may change drastically before final release or may not be released in stable form at all. 
  * Such breaking changes may appear even in minor and patch releases. 
  * We do not recommend using experimental features in Terraform modules intended for production use.
- In order to make that explicit and to avoid module callers inadvertently depending on an experimental feature, any module with experiments enabled will generate a warning on every `terraform plan` or `terraform apply`. 
- If you want to try experimental features in a shared module, we recommend enabling the experiment only in alpha or beta releases of the module.
- The introduction and completion of experiments is reported in Terraform's changelog, so you can watch the release notes there to discover which experiment keywords, if any, are available in a particular Terraform release.

### 3b Describe plug-in based architecture

/ https://learn.hashicorp.com/terraform/getting-started/build#initialization

### 3c Demonstrate using multiple providers

/ https://learn.hashicorp.com/terraform/getting-started/build#providers

### 3d Describe how Terraform finds and fetches providers


#### `alias`: Multiple Provider Instances

- To include multiple configurations for a given provider, include multiple provider blocks with the same provider name, but set the alias meta-argument to an alias name to use for each additional configuration. e.g.:
  ```hcl
  # The default provider configuration
  provider "aws" {
    region = "us-east-1"
  }

  # Additional provider configuration for west coast region
  provider "aws" {
    alias  = "west"
    region = "us-west-2"
  }
  ```
- The provider block without alias set is known as the *default* provider configuration. 
  * For providers that have no required configuration arguments, the implied empty configuration is considered to be the default provider configuration.
- When *alias* is set, it creates an additional provider configuration.
- When Terraform needs the name of a provider configuration, it always expects a reference of the form `<PROVIDER NAME>.<ALIAS>`. 
  * In the example above, `aws.west` would refer to the provider with the `us-west-2` region.
  * These special expressions are only valid in specific meta-arguments of `resource`, `data`, and `module` blocks, and can't be used in arbitrary expressions, e.g.:
    - `resource`:
      ```hcl
      resource "aws_instance" "foo" {
        provider = aws.west

        # ...
      }
      ```
    - `module`:
      ```hcl
      module "aws_vpc" {
        source = "./aws_vpc"
        providers = {
          aws = aws.west
        }
      }
      ```
#### Third-party plugins
- Install third-party providers by placing their plugin executables in the user plugins directory. 
- The user plugins directory is in one of the following locations, depending on the host operating system:
  | Operating system  | User plugins directory          |
  |-------------------|---------------------------------|
  | Windows           | `%APPDATA%\terraform.d\plugins` |
  | All other systems | `~/.terraform.d/plugins`        |
- Once a plugin is installed, `terraform init` can initialize it normally. 
  * You must run this command from the *directory where the configuration files are located*.
- Providers distributed by HashiCorp can also go in the user plugins directory. 
- If a manually installed version meets the configuration's version constraints, Terraform will use it instead of downloading that provider. 
  * This is useful in airgapped environments and when testing pre-release provider builds.
- The naming scheme for provider plugins is `terraform-provider-<NAME>_vX.Y.Z`, and Terraform uses the name to understand the name and version of a particular provider binary.
- If multiple versions of a plugin are installed, Terraform will use the *newest version that meets the configuration's version constraints*.
- Third-party plugins are *often* distributed with an appropriate filename already set in the distribution archive, so that they can be extracted directly into the user plugins directory.
- Terraform plugins are compiled for a specific operating system and architecture, and any plugins in the root of the user plugins directory must be compiled for the current system.
- If you use the same plugins directory on multiple systems, you can install plugins into subdirectories with a naming scheme of `<OS>_<ARCH>` (for example, `darwin_amd64`). 
- Terraform uses plugins from the root of the plugins directory and from the subdirectory that corresponds to the current system, ignoring other subdirectories.

#### Provider Plugin Cache
- By default, `terraform init` downloads plugins into a subdirectory of the working directory so that each working directory is self-contained. 
  * As a consequence, if you have multiple configurations that use the same provider then a separate copy of its plugin will be downloaded for each configuration.
- Given that provider plugins can be quite large (on the order of 100s of megabytes), this default behavior can be inconvenient for those with slow or metered Internet connections. 
  * Therefore Terraform optionally allows the use of a local directory as a shared plugin cache, which then allows each distinct plugin binary to be downloaded only once.
- To enable the plugin cache, use the `plugin_cache_dir` setting in the CLI configuration file. e.g.:

  ```bash
  # (Note that the CLI configuration file is _not_ the same as the .tf files
  #  used to configure infrastructure.)

  plugin_cache_dir = "$HOME/.terraform.d/plugin-cache"
  ```

- This directory must already exist before Terraform will cache plugins; Terraform will not create the directory itself.
- Please note that on Windows it is necessary to use forward slash separators (`/`) rather than the conventional backslash (`\`) since the configuration file parser considers a backslash to begin an escape sequence.
- Setting this in the configuration file is the recommended approach for a persistent setting. 
- Alternatively, the `TF_PLUGIN_CACHE_DIR` environment variable can be used to enable caching or to override an existing cache directory within a particular shell session:
  ```bash
  $ export TF_PLUGIN_CACHE_DIR="$HOME/.terraform.d/plugin-cache"
  ```
- When a plugin cache directory is enabled, the `terraform init` command will still access the plugin distribution server to obtain metadata about which plugins are available, but once a suitable version has been selected it will first check to see if the selected plugin is already available in the cache directory. 
  * If so, the already-downloaded plugin binary will be used.
  * If the selected plugin is not already in the cache, it will be downloaded into the cache first and then copied from there into the correct location under your current working directory.
- When possible, Terraform will use hardlinks or symlinks to avoid storing a separate copy of a cached plugin in multiple directories. At present, this is not supported on Windows and instead a copy is always created.
- The plugin cache directory must not be the third-party plugin directory or any other directory Terraform searches for pre-installed plugins, since the cache management logic conflicts with the normal plugin discovery logic when operating on the same directory.
- Please note that Terraform will never itself delete a plugin from the plugin cache once it's been placed there. Over time, as plugins are upgraded, the cache directory may grow to contain several unused versions which must be manually deleted.

### 3e Explain when to use and not use provisioners and when to use local-exec or remote-exec
- Provisioners add a considerable amount of complexity and uncertainty to Terraform usage. 
  * Terraform cannot model the actions of provisioners as part of a plan because they can in principle take any action.   
  * Successful use of provisioners requires coordinating many more details than Terraform usage usually requires: 
    - direct network access to your servers, 
    - issuing Terraform credentials to log in, 
    - making sure that all of the necessary external software is installed, etc.

#### Passing data into virtual machines and other compute resources
- When deploying virtual machines or other similar compute resources, we often need to pass in data about other related infrastructure that the software on that server will need to do its job.
- The various provisioners that interact with remote servers over SSH or WinRM can potentially be used to pass such data by logging in to the server and providing it directly, but most cloud computing platforms provide mechanisms to pass data to instances at the time of their creation such that the data is immediately available on system boot. For example:
  * Alibaba Cloud: `user_data` on `alicloud_instance` or `alicloud_launch_template`.
  * Amazon EC2: `user_data` or `user_data_base64` on `aws_instance`, `aws_launch_template`, and `aws_launch_configuration`.
  * Amazon Lightsail: `user_data` on `aws_lightsail_instance`.
  * Microsoft Azure: `custom_data` on `azurerm_virtual_machine` or `azurerm_virtual_machine_scale_set`.
  * Google Cloud Platform: `metadata` on `google_compute_instance` or `google_compute_instance_group`.
  * Oracle Cloud Infrastructure: `metadata` or `extended_metadata` on `oci_core_instance` or `oci_core_instance_configuration`.
  * VMware vSphere: Attach a virtual CDROM to `vsphere_virtual_machine` using the `cdrom` block, containing a file called `user-data.txt`.
- Many official Linux distribution disk images include software called `cloud-init` that can automatically process in various ways data passed via the means described above, allowing you to run arbitrary scripts and do basic system configuration immediately during the boot process and without the need to access the machine over SSH.
- If you are building custom machine images, you can make use of the "user data" or "metadata" passed by the above means in whatever way makes sense to your application, by referring to your vendor's documentation on how to access the data at runtime.
- This approach is required if you intend to use any mechanism in your cloud provider for automatically launching and destroying servers in a group, because in that case individual servers will launch unattended while Terraform is not around to provision them.
- Even if you're deploying individual servers directly with Terraform, passing data this way will allow faster boot times and simplify deployment by avoiding the need for direct network access from Terraform to the new server and for remote access credentials to be provided.

#### Running configuration management software
- As a convenience to users who are forced to use generic operating system distribution images, Terraform includes a number of specialized provisioners for launching specific configuration management products.
  * We strongly recommend not using these, and instead running system configuration steps during a custom image build process. 
  * For example, HashiCorp Packer offers a similar complement of configuration management provisioners and can run their installation steps during a separate build process, before creating a system disk image that you can deploy many times.
- If you are using configuration management software that has a centralized server component, you will need to delay the registration step until the final system is booted from your custom image. 
  * To achieve that, use one of the mechanisms described above to pass the necessary information into each instance so that it can register itself with the configuration management server immediately on boot, without the need to accept commands from Terraform over SSH or WinRM.

#### First-class Terraform provider functionality may be available
- It is technically possible to use the local-exec provisioner to run the CLI for your target system in order to create, update, or otherwise interact with remote objects in that system.
  * If you are trying to use a new feature of the remote system that isn't yet supported in its Terraform provider, that might be the only option. 
  * However, if there is provider support for the feature you intend to use, prefer to use that provider functionality rather than a provisioner so that Terraform can be fully aware of the object and properly manage ongoing changes to it.
- Even if the functionality you need is not available in a provider today, we suggest to consider `local-exec` usage a temporary workaround and to also open an issue in the relevant provider's repository to discuss adding first-class provider support. 
  * Provider development teams often prioritize features based on interest, so opening an issue is a way to record your interest in the feature.
- Provisioners are used to execute scripts on a local or remote machine as part of resource creation or destruction. - - Provisioners can be used to bootstrap a resource, cleanup before destroy, run configuration management, etc.

#### `local-exec` Provisioner
- The `local-exec` provisioner invokes a local executable after a resource is created. 
  *  Note that even though the resource will be fully created when the provisioner is run, there is no guarantee that it will be in an operable state - for example system services such as `sshd` may not be started yet on compute resources.
 
  ```hcl
  resource "aws_instance" "web" {
    # ...

    provisioner "local-exec" {
      command = "echo ${aws_instance.web.private_ip} >> private_ips.txt"
    }
  }
  ```

##### `local-exec` Arguments Supported
- `command` - (Required) This is the command to execute. It can be provided as a relative path to the current working directory or as an absolute path. It is evaluated in a shell, and can use environment variables or Terraform variables.
- `working_dir` - (Optional) If provided, specifies the working directory where `command` will be executed. It can be provided as as a relative path to the current working directory or as an absolute path. The directory must exist.
- `interpreter` - (Optional) If provided, this is a list of interpreter arguments used to execute the command. The first argument is the interpreter itself. It can be provided as a relative path to the current working directory or as an absolute path. The remaining arguments are appended prior to the command. This allows building command lines of the form "/bin/bash", "-c", "echo foo". If `interpreter` is unspecified, sensible defaults will be chosen based on the system OS.
- `environment` - (Optional) block of key value pairs representing the environment of the executed command. inherits the current process environment.
- Examples:

  ```hcl
  resource "null_resource" "example1" {
    provisioner "local-exec" {
      command = "open WFH, '>completed.txt' and print WFH scalar localtime"
      interpreter = ["perl", "-e"]
    }
  }
  ```

  ```hcl
  resource "null_resource" "example2" {
    provisioner "local-exec" {
      command = "Get-Date > completed.txt"
      interpreter = ["PowerShell", "-Command"]
    }
  }
  ```

  ```hcl
  resource "aws_instance" "web" {
    # ...

    provisioner "local-exec" {
      command = "echo $FOO $BAR $BAZ >> env_vars.txt"

      environment = {
        FOO = "bar"
        BAR = 1
        BAZ = "true"
      }
    }
  }
  ```

#### `remote-exec` Provisioner
- The `remote-exec` provisioner invokes a script on a remote resource after it is created. 
- This can be used to run a configuration management tool, bootstrap into a cluster, etc.
- The `remote-exec` provisioner supports both `ssh` and `winrm` type connections.

  ```hcl
  resource "aws_instance" "web" {
    # ...

    provisioner "remote-exec" {
      inline = [
        "puppet apply",
        "consul join ${aws_instance.web.private_ip}",
      ]
    }
  }
  ```

##### `remote-exec` Arguments Supported
- `inline` - This is a list of command strings. They are executed in the order they are provided. This cannot be provided with `script` or `scripts`.
- `script` - This is a path (relative or absolute) to a local script that will be copied to the remote resource and then executed. This cannot be provided with `inline` or `scripts`.
- `scripts` - This is a list of paths (relative or absolute) to local scripts that will be copied to the remote resource and then executed. They are executed in the order they are provided. This cannot be provided with `inline` or `script`.

##### Passing Script Arguments
- You cannot pass any arguments to scripts using the script or scripts arguments to this provisioner. If you want to specify arguments, upload the script with the file provisioner and then use inline to call it, e.g.

```hcl
resource "aws_instance" "web" {
  # ...

  provisioner "file" {
    source      = "script.sh"
    destination = "/tmp/script.sh"
  }

  provisioner "remote-exec" {
    inline = [
      "chmod +x /tmp/script.sh",
      "/tmp/script.sh args",
    ]
  }
}
```

## 4 Use the Terraform CLI (outside of core workflow)

### 4a Given a scenario: choose when to use terraform fmt to format code

- Other Terraform commands that generate Terraform configuration will produce configuration files that conform to the style imposed by `terraform fmt`, so using this style in your own files will ensure consistency.
- The canonical format may change in minor ways between Terraform versions, so after upgrading Terraform they recommend to proactively run `terraform fmt` on your modules along with any other changes you are making to adopt the new version.

Usage: `terraform fmt [options] [DIR]`

By default, `fmt` scans the current directory for configuration files. If the `dir` argument is provided then it will scan that given directory instead. If `dir` is a single dash (`-`) then `fmt` will read from standard input (STDIN).

The command-line flags are all optional. The list of available flags are:

* `-list=false` - Don't list the files containing formatting inconsistencies.
* `-write=false` - Don't overwrite the input files. (This is implied by -check or when the input is STDIN.)
* `-diff` - Display diffs of formatting changes
* `-check` - Check if the input is formatted. Exit status will be 0 if all input is properly formatted and non-zero otherwise.
* `-recursive` - Also process files in subdirectories. By default, only the given directory (or current directory) is processed.

### 4b Given a scenario: choose when to use terraform taint to taint Terraform resources
- This command will not modify infrastructure, but does modify the state file in order to mark a resource as tainted. 
- Once a resource is marked as tainted, the next plan will show that the resource will be destroyed and recreated and the next apply will implement this change.
- Forcing the recreation of a resource is useful when you want a certain side effect of recreation that is not visible in the attributes of a resource. 
  * For example: re-running provisioners will cause the node to be different or rebooting the machine from a base image will cause new startup scripts to run.
- Note that tainting a resource for recreation may affect resources that depend on the newly tainted resource. 
  * For example, a DNS resource that uses the IP address of a server may need to be modified to reflect the potentially new IP address of a tainted server. 
  * The `plan` command will show this if this is the case.
- Usage: `terraform taint [options] address`
  * The address argument is the address of the resource to mark as tainted. The address is in the resource address syntax syntax, as shown in the output from other commands, such as:
    - `aws_instance.foo`
    - `aws_instance.bar[1]`
    - `aws_instance.baz[\"key\"]` 
      * (quotes in resource addresses must be escaped on the command line, so that they are not interpreted by your shell)
    - `module.foo.module.bar.aws_instance.qux`
  * The command-line flags are all optional. The list of available flags are:
    - `-allow-missing` - If specified, the command will succeed (exit code 0) even if the resource is missing. The command can still error, but only in critically erroneous cases.
    - `-backup=path` - Path to the backup file. Defaults to -state-out with the ".backup" extension. Disabled by setting to "-".
    - `-lock=true` - Lock the state file when locking is supported.
    - `-lock-timeout=0s` - Duration to retry a state lock.
    - `-state=path` - Path to read and write the state file to. Defaults to "terraform.tfstate". Ignored when remote state is used.
    - `-state-out=path` - Path to write updated state file. By default, the -state path will be used. Ignored when remote state is used.

/ https://learn.hashicorp.com/terraform/getting-started/provision#failed-provisioners-and-tainted-resources

### 4c Given a scenario: choose when to use terraform import to import existing infrastructure into your Terraform state

/ https://www.terraform.io/docs/commands/import.html

### 4d Given a scenario: choose when to use terraform workspace to create workspaces		

/ https://www.terraform.io/docs/state/workspaces.html

### 4e Given a scenario: choose when to use terraform state to view Terraform state

/ https://www.terraform.io/docs/commands/state/index.html

### 4f Given a scenario: choose when to enable verbose logging and what the outcome/value is

/ https://www.terraform.io/docs/internals/debugging.html

## 5 Interact with Terraform modules

### 5a Contrast module source options

/ https://www.terraform.io/docs/registry/modules/use.html#finding-modules

### 5b Interact with module inputs and outputs

/ https://learn.hashicorp.com/terraform/modules/modules-overview

### 5c Describe variable scope within modules/child modules

/ https://www.terraform.io/docs/configuration/variables.html

/ https://www.terraform.io/docs/configuration/modules.html#calling-a-child-module

### 5d Discover modules from the public Terraform Module Registry 

/ https://www.terraform.io/docs/registry/modules/use.html#using-modules

### 5e Defining module version   

/ https://www.terraform.io/docs/configuration/modules.html#module-versions

## 6  Navigate Terraform workflow     

### 6a Describe Terraform workflow ( Write -> Plan -> Create ) 

/ https://www.terraform.io/guides/core-workflow.html

### 6b Initialize a Terraform working directory (terraform init)                             

/ https://www.terraform.io/docs/commands/init.html

### 6c Validate a Terraform configuration (terraform validate)      

/ https://www.terraform.io/docs/commands/validate.html

### 6d Generate and review an execution plan for Terraform (terraform plan) 

/ https://www.terraform.io/docs/commands/plan.html

### 6e Execute changes to infrastructure with Terraform (terraform apply)                    

/ https://www.terraform.io/docs/commands/apply.html

### 6f Destroy Terraform managed infrastructure (terraform destroy)

/ https://www.terraform.io/docs/commands/destroy.html

## 7  Implement and maintain state                                                          

### 7a Describe default local backend 



### 7b Outline state locking                                                                 
### 7c Handle backend authentication methods                                                 
### 7d Describe remote state storage mechanisms and supported standard backends     

/ Remote State https://www.terraform.io/docs/state/remote.html

### 7e Describe effect of Terraform refresh on state                                         
### 7f Describe backend block in configuration and best practices for partial configurations 
### 7g Understand secret management in state files                                           
## 8  Read, generate, and modify configuration                                              
### 8a Demonstrate use of variables and outputs

/ Expressions https://www.terraform.io/docs/configuration/expressions.html

### 8b Describe secure secret injection best practice                                        
### 8c Understand the use of collection and structural types                                 
### 8d Create and differentiate resource and data configuration                              
### 8e Use resource addressing and resource parameters to connect resources together         
### 8f Use Terraform built-in functions to write configuration                               
### 8g Configure resource using a dynamic block                                              
### 8h Describe built-in dependency management (order of execution based)                    
## 9  Understand Terraform Cloud and Enterprise capabilities                                
### 9a Describe the benefits of Sentinel, registry, and workspaces                           
### 9b Differentiate OSS and Terraform Cloud workspaces                                      
### 9c Summarize features of Terraform Cloud                                                 #                               
