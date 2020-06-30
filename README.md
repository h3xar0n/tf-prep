- [1 Understand Infrastructure as Code (IaC) concepts](#1-understand-infrastructure-as-code-iac-concepts)
  - [1a Explain what IaC is](#1a-explain-what-iac-is)
    - [Resource Graph](#resource-graph)
    - [Change Automation](#change-automation)
  - [1b Describe advantages of IaC patterns](#1b-describe-advantages-of-iac-patterns)
- [2 Understand Terraform's purpose (vs other IaC)](#2-understand-terraforms-purpose-vs-other-iac)
  - [2a Explain multi-cloud and provider-agnostic benefits](#2a-explain-multi-cloud-and-provider-agnostic-benefits)
  - [2b Explain the benefits of state](#2b-explain-the-benefits-of-state)
    - [Mapping to the Real World](#mapping-to-the-real-world)
    - [Metadata](#metadata)
    - [Performance](#performance)
    - [Syncing](#syncing)
- [3 Understand Terraform basics](#3-understand-terraform-basics)
  - [3a Handle Terraform and provider installation and versioning](#3a-handle-terraform-and-provider-installation-and-versioning)
    - [Providers](#providers)
    - [Provider Configuration](#provider-configuration)
    - [Initialization](#initialization)
    - [Provider Versions](#provider-versions)
    - [Writing Custom Providers](#writing-custom-providers)
    - [Terraform Settings](#terraform-settings)
      - [Configuring a Terraform Backend](#configuring-a-terraform-backend)
      - [Specifying a Required Terraform Version](#specifying-a-required-terraform-version)
      - [Experimental Language Features](#experimental-language-features)
  - [3b Describe plug-in based architecture](#3b-describe-plug-in-based-architecture)
  - [3c Demonstrate using multiple providers](#3c-demonstrate-using-multiple-providers)
  - [3d Describe how Terraform finds and fetches providers](#3d-describe-how-terraform-finds-and-fetches-providers)
    - [`alias`: Multiple Provider Instances](#alias-multiple-provider-instances)
    - [Third-party plugins](#third-party-plugins)
    - [Provider Plugin Cache](#provider-plugin-cache)
  - [3e Explain when to use and not use provisioners and when to use local-exec or remote-exec](#3e-explain-when-to-use-and-not-use-provisioners-and-when-to-use-local-exec-or-remote-exec)
    - [Passing data into virtual machines and other compute resources](#passing-data-into-virtual-machines-and-other-compute-resources)
    - [Running configuration management software](#running-configuration-management-software)
    - [First-class Terraform provider functionality may be available](#first-class-terraform-provider-functionality-may-be-available)
    - [`local-exec` Provisioner](#local-exec-provisioner)
      - [`local-exec` Arguments Supported](#local-exec-arguments-supported)
    - [`remote-exec` Provisioner](#remote-exec-provisioner)
      - [`remote-exec` Arguments Supported](#remote-exec-arguments-supported)
      - [Passing Script Arguments](#passing-script-arguments)
- [4 Use the Terraform CLI (outside of core workflow)](#4-use-the-terraform-cli-outside-of-core-workflow)
  - [4a Given a scenario: choose when to use terraform fmt to format code](#4a-given-a-scenario-choose-when-to-use-terraform-fmt-to-format-code)
  - [4b Given a scenario: choose when to use terraform taint to taint Terraform resources](#4b-given-a-scenario-choose-when-to-use-terraform-taint-to-taint-terraform-resources)
    - [Tainting a Single Resource](#tainting-a-single-resource)
    - [Tainting a single resource created with for_each](#tainting-a-single-resource-created-with-for_each)
    - [Tainting a Resource within a Module](#tainting-a-resource-within-a-module)
  - [4c Given a scenario: choose when to use terraform import to import existing infrastructure into your Terraform state](#4c-given-a-scenario-choose-when-to-use-terraform-import-to-import-existing-infrastructure-into-your-terraform-state)
  - [4d Given a scenario: choose when to use terraform workspace to create workspaces](#4d-given-a-scenario-choose-when-to-use-terraform-workspace-to-create-workspaces)
  - [4e Given a scenario: choose when to use terraform state to view Terraform state](#4e-given-a-scenario-choose-when-to-use-terraform-state-to-view-terraform-state)
  - [4f Given a scenario: choose when to enable verbose logging and what the outcome/value is](#4f-given-a-scenario-choose-when-to-enable-verbose-logging-and-what-the-outcomevalue-is)
- [5 Interact with Terraform modules](#5-interact-with-terraform-modules)
  - [5a Contrast module source options](#5a-contrast-module-source-options)
  - [5b Interact with module inputs and outputs](#5b-interact-with-module-inputs-and-outputs)
  - [5c Describe variable scope within modules/child modules](#5c-describe-variable-scope-within-moduleschild-modules)
  - [5d Discover modules from the public Terraform Module Registry](#5d-discover-modules-from-the-public-terraform-module-registry)
  - [5e Defining module version](#5e-defining-module-version)
- [6  Navigate Terraform workflow](#6-navigate-terraform-workflow)
  - [6a Describe Terraform workflow ( Write -> Plan -> Create )](#6a-describe-terraform-workflow--write---plan---create-)
  - [6b Initialize a Terraform working directory (terraform init)](#6b-initialize-a-terraform-working-directory-terraform-init)
  - [6c Validate a Terraform configuration (terraform validate)](#6c-validate-a-terraform-configuration-terraform-validate)
  - [6d Generate and review an execution plan for Terraform (terraform plan)](#6d-generate-and-review-an-execution-plan-for-terraform-terraform-plan)
  - [6e Execute changes to infrastructure with Terraform (terraform apply)](#6e-execute-changes-to-infrastructure-with-terraform-terraform-apply)
  - [6f Destroy Terraform managed infrastructure (terraform destroy)](#6f-destroy-terraform-managed-infrastructure-terraform-destroy)
- [7  Implement and maintain state](#7-implement-and-maintain-state)
  - [7a Describe default local backend](#7a-describe-default-local-backend)
  - [7b Outline state locking](#7b-outline-state-locking)
    - [Force Unlock](#force-unlock)
  - [7c Handle backend authentication methods](#7c-handle-backend-authentication-methods)
  - [7d Describe remote state storage mechanisms and supported standard backends](#7d-describe-remote-state-storage-mechanisms-and-supported-standard-backends)
  - [7e Describe effect of Terraform refresh on state](#7e-describe-effect-of-terraform-refresh-on-state)
  - [7f Describe backend block in configuration and best practices for partial configurations](#7f-describe-backend-block-in-configuration-and-best-practices-for-partial-configurations)
    - [Backend Configuration](#backend-configuration)
    - [First Time Configuration](#first-time-configuration)
    - [Partial Configuration](#partial-configuration)
  - [7g Understand secret management in state files](#7g-understand-secret-management-in-state-files)
    - [Sensitive Data in State](#sensitive-data-in-state)
    - [Recommendations](#recommendations)
- [8  Read, generate, and modify configuration](#8-read-generate-and-modify-configuration)
  - [8a Demonstrate use of variables and outputs](#8a-demonstrate-use-of-variables-and-outputs)
  - [8b Describe secure secret injection best practice](#8b-describe-secure-secret-injection-best-practice)
  - [8c Understand the use of collection and structural types](#8c-understand-the-use-of-collection-and-structural-types)
  - [8d Create and differentiate resource and data configuration](#8d-create-and-differentiate-resource-and-data-configuration)
  - [8e Use resource addressing and resource parameters to connect resources together](#8e-use-resource-addressing-and-resource-parameters-to-connect-resources-together)
  - [8f Use Terraform built-in functions to write configuration](#8f-use-terraform-built-in-functions-to-write-configuration)
  - [8g Configure resource using a dynamic block](#8g-configure-resource-using-a-dynamic-block)
  - [8h Describe built-in dependency management (order of execution based)](#8h-describe-built-in-dependency-management-order-of-execution-based)
- [9  Understand Terraform Cloud and Enterprise capabilities](#9-understand-terraform-cloud-and-enterprise-capabilities)
  - [9a Describe the benefits of Sentinel, registry, and workspaces](#9a-describe-the-benefits-of-sentinel-registry-and-workspaces)
    - [Sentinel](#sentinel)
    - [Registry](#registry)
    - [Workspaces](#workspaces)
      - [Workspaces are Collections of Infrastructure](#workspaces-are-collections-of-infrastructure)
      - [Listing and Filtering Workspaces](#listing-and-filtering-workspaces)
      - [Planning and Organizing Workspaces](#planning-and-organizing-workspaces)
  - [9b Differentiate OSS and Terraform Cloud workspaces](#9b-differentiate-oss-and-terraform-cloud-workspaces)
    - [OSS Workspaces](#oss-workspaces)
  - [9c Summarize features of Terraform Cloud](#9c-summarize-features-of-terraform-cloud)

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
> **Simplify**
- Terraform stores a cache of the attribute values for all resources in the state.
- This is the most optional feature of Terraform state and is done only as a performance improvement.
- When running a `terraform plan`, Terraform must know the current state of resources in order to effectively determine the changes that it needs to make to reach your desired configuration.
- For *small infrastructures,* the **default behavior of Terraform** is:
  > For every plan and apply, Terraform queries your providers and syncs the latest attributes from all resources in your state.
- For *larger infrastructures*, the default of querying every resource is too slow.
  - Many cloud providers do not provide APIs to query multiple resources at once
  - The round trip time for each resource is 100s of milliseconds
  - On top of this, cloud providers almost always have API rate limiting so Terraform can only request a certain number of resources in a period of time.
- Larger users of Terraform make heavy use of the `-refresh=false` flag as well as the `-target` flag in order to work around this, e.g.
  - [`terraform apply -refresh=false`](https://www.terraform.io/docs/commands/apply.html#refresh-true) does not update the state for each resource prior to applying. (This has no effect if a plan file is given directly to apply.)
  - [`terraform plan -target=resource`](https://www.terraform.io/docs/commands/plan.html#target-resource) only focuses Terraform's attention on the specified resource. Since it can be used multiple times, you can create a single plan file for multiple targets.
  - [`terraform apply -target=resource`](https://www.terraform.io/docs/commands/apply.html#target-resource) only applies specified resources from Terraform's state.
- In these scenarios, the cached state is treated as the record of truth.

> **Investigate**
- Resource Targeting https://www.terraform.io/docs/commands/plan.html#resource-targeting

#### Syncing
* For solo use, Terraform creates a state file in the local directory.
* This local file can lead to conflicts when a team is working with the same state.
* To avoid conflicts, using remote state enables teams to benefit from **state locking**.

## 3 Understand Terraform basics

### 3a Handle Terraform and provider installation and versioning

#### Providers
**providers** → define resource *types* → entail *behaviors* of resources → **resources** (primary construct in TF)
- Each provider offers a set of named resource types defines for each resource type:
  - which arguments it accepts
  - which attributes it exports
  - how changes to resources of that type are actually applied to remote APIs
- Most of the available providers correspond to one cloud or on-premises infrastructure platform, and offer resource types that correspond to each of the features of that platform.

> **Investigate**
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

> **Investigate**
- Since provider configurations must be evaluated in order to perform any resource type action, provider configurations may refer only to values that are known before the configuration is applied.
- In particular, avoid referring to attributes exported by other resources unless their values are specified directly in the configuration.

#### Initialization
> **Simplify**
- Provider initialization is one of the actions of `terraform init`.
 * Running this command will download and initialize any providers that are not already initialized.
- Providers downloaded by `terraform init` are only installed for the *current working directory*; other working directories can have their own installed provider versions.
- Note that `terraform init` *cannot automatically download providers that are not distributed by HashiCorp*.

#### Provider Versions
- Providers are plugins released on a separate rhythm from Terraform itself, and so they have their own version numbers.
- For production use, you should constrain the acceptable provider versions via configuration, to ensure that new versions with breaking changes will not be automatically installed by terraform init in future.

> **Investigate**
- When `terraform init` is run *without provider version constraints*, it prints a suggested version constraint string for each provider:

  ```
  The following providers do not have any version constraints in configuration,
  so the latest version was installed.

  To prevent automatic upgrades to new major versions that may contain breaking
  changes, it is recommended to add version = "..." constraints to the
  corresponding provider blocks in configuration, with the constraint strings
  suggested below.

  - provider.aws: version = "~> 1.0"
  ```

- To constrain the provider version as suggested, add a required_providers block inside a terraform block:

  ```hcl
  terraform {
    required_providers {
      aws = "~> 1.0"
    }
  }
  ```

> **Investigate**
- When `terraform init` is re-run with providers already installed, it will use an already-installed provider that meets the constraints in preference to downloading a new version.
- To upgrade to the latest acceptable version of each provider, run `terraform init -upgrade`.
  - This command also upgrades to the latest versions of all Terraform modules.

#### Writing Custom Providers

> **Investigate**
- Writing Custom Providers https://www.terraform.io/docs/extend/writing-custom-providers.html

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
> **Simplify**
- The `required_version` setting can be used to constrain which versions of the Terraform CLI can be used with your configuration.
- If the running version of Terraform doesn't match the constraints specified, Terraform will produce an error and exit without taking any further actions.
- When you use child modules, each module can specify its own version requirements.
  - The requirements of all modules in the tree must be satisfied.
- The `required_version` setting applies only to the version of Terraform CLI.
- Various behaviors of Terraform are actually implemented by Terraform Providers, which are [released on a cycle independent of Terraform CLI and of each other](#provider-versions).
- Re-usable modules should constrain only the minimum allowed version, such as `>= 0.12.0`.
  - This specifies the earliest version that the module is compatible with while leaving the user of the module flexibility to upgrade to newer versions of Terraform without altering the module.

##### Experimental Language Features
> **Simplify**
- In releases where experimental features are available, you can enable them on a per-module basis by setting the experiments argument inside a terraform block:
  ```hcl
  terraform {
    experiments = [example]
  }
  ```
- The above would opt in to an experiment named example, assuming such an experiment were available in the current Terraform version.
  - Experiments are subject to arbitrary changes in later releases and, depending on the outcome of the experiment, may change drastically before final release or may not be released in stable form at all.
  - Such breaking changes may appear even in minor and patch releases.
  - We do not recommend using experimental features in Terraform modules intended for production use.
- In order to make that explicit and to avoid module callers inadvertently depending on an experimental feature, any module with experiments enabled will generate a warning on every `terraform plan` or `terraform apply`.
- If you want to try experimental features in a shared module, we recommend enabling the experiment only in alpha or beta releases of the module.
- The introduction and completion of experiments is reported in Terraform's changelog, so you can watch the release notes there to discover which experiment keywords, if any, are available in a particular Terraform release.

### 3b Describe plug-in based architecture

> **Investigate**
- https://learn.hashicorp.com/terraform/getting-started/build#initialization

### 3c Demonstrate using multiple providers

> **Investigate**
- https://learn.hashicorp.com/terraform/getting-started/build#providers

### 3d Describe how Terraform finds and fetches providers

#### `alias`: Multiple Provider Instances

> **Investigate**
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
  - For providers that have no required configuration arguments, the implied empty configuration is considered to be the default provider configuration.
- When *alias* is set, it creates an additional provider configuration.
- When Terraform needs the name of a provider configuration, it always expects a reference of the form `<PROVIDER NAME>.<ALIAS>`.
  - In the example above, `aws.west` would refer to the provider with the `us-west-2` region.
  - These special expressions are only valid in specific meta-arguments of `resource`, `data`, and `module` blocks, and can't be used in arbitrary expressions, e.g.:
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
> **Investigate**
- Install third-party providers by placing their plugin executables in the user plugins directory.
- The user plugins directory is in one of the following locations, depending on the host operating system:
  | Operating system  | User plugins directory          |
  |-------------------|---------------------------------|
  | Windows           | `%APPDATA%\terraform.d\plugins` |
  | All other systems | `~/.terraform.d/plugins`        |
- Once a plugin is installed, `terraform init` can initialize it normally.
  - You must run this command from the *directory where the configuration files are located*.
- Providers distributed by HashiCorp can also go in the user plugins directory.
- If a manually installed version meets the configuration's version constraints, Terraform will use it instead of downloading that provider.
  - This is useful in airgapped environments and when testing pre-release provider builds.
- The naming scheme for provider plugins is `terraform-provider-<NAME>_vX.Y.Z`, and Terraform uses the name to understand the name and version of a particular provider binary.
- If multiple versions of a plugin are installed, Terraform will use the *newest version that meets the configuration's version constraints*.
- Third-party plugins are *often* distributed with an appropriate filename already set in the distribution archive, so that they can be extracted directly into the user plugins directory.
- Terraform plugins are compiled for a specific operating system and architecture, and any plugins in the root of the user plugins directory must be compiled for the current system.
- If you use the same plugins directory on multiple systems, you can install plugins into subdirectories with a naming scheme of `<OS>_<ARCH>` (for example, `darwin_amd64`).
- Terraform uses plugins from the root of the plugins directory and from the subdirectory that corresponds to the current system, ignoring other subdirectories.

#### Provider Plugin Cache
> **Investigate**
- By default, `terraform init` downloads plugins into a subdirectory of the working directory so that each working directory is self-contained.
  - As a consequence, if you have multiple configurations that use the same provider then a separate copy of its plugin will be downloaded for each configuration.
- Given that provider plugins can be quite large (on the order of 100s of megabytes), this default behavior can be inconvenient for those with slow or metered Internet connections.
  - Therefore Terraform optionally allows the use of a local directory as a shared plugin cache, which then allows each distinct plugin binary to be downloaded only once.
- To enable the plugin cache, use the `plugin_cache_dir` setting in the CLI configuration file (`.terraformrc` on all OS except Windows, `terraform.rc`). e.g.:

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
  - If so, the already-downloaded plugin binary will be used.
  - If the selected plugin is not already in the cache, it will be downloaded into the cache first and then copied from there into the correct location under your current working directory.
- When possible, Terraform will use hardlinks or symlinks to avoid storing a separate copy of a cached plugin in multiple directories. At present, this is not supported on Windows and instead a copy is always created.
- The plugin cache directory must not be the third-party plugin directory or any other directory Terraform searches for pre-installed plugins, since the cache management logic conflicts with the normal plugin discovery logic when operating on the same directory.
- Please note that Terraform will never itself delete a plugin from the plugin cache once it's been placed there. Over time, as plugins are upgraded, the cache directory may grow to contain several unused versions which must be manually deleted.

### 3e Explain when to use and not use provisioners and when to use local-exec or remote-exec
> **Simplify**
- Provisioners add a considerable amount of complexity and uncertainty to Terraform usage.
  - Terraform cannot model the actions of provisioners as part of a plan because they can in principle take any action.
  - Successful use of provisioners requires coordinating many more details than Terraform usage usually requires:
    - direct network access to your servers,
    - issuing Terraform credentials to log in,
    - making sure that all of the necessary external software is installed, etc.

#### Passing data into virtual machines and other compute resources
> **Simplify**
- When deploying virtual machines or other similar compute resources, we often need to pass in data about other related infrastructure that the software on that server will need to do its job.
- The various provisioners that interact with remote servers over SSH or WinRM can potentially be used to pass such data by logging in to the server and providing it directly, but most cloud computing platforms provide mechanisms to pass data to instances at the time of their creation such that the data is immediately available on system boot. For example:
  - Alibaba Cloud: `user_data` on `alicloud_instance` or `alicloud_launch_template`.
  - Amazon EC2: `user_data` or `user_data_base64` on `aws_instance`, `aws_launch_template`, and `aws_launch_configuration`.
  - Amazon Lightsail: `user_data` on `aws_lightsail_instance`.
  - Microsoft Azure: `custom_data` on `azurerm_virtual_machine` or `azurerm_virtual_machine_scale_set`.
  - Google Cloud Platform: `metadata` on `google_compute_instance` or `google_compute_instance_group`.
  - Oracle Cloud Infrastructure: `metadata` or `extended_metadata` on `oci_core_instance` or `oci_core_instance_configuration`.
  - VMware vSphere: Attach a virtual CDROM to `vsphere_virtual_machine` using the `cdrom` block, containing a file called `user-data.txt`.
- Many official Linux distribution disk images include software called `cloud-init` that can automatically process in various ways data passed via the means described above, allowing you to run arbitrary scripts and do basic system configuration immediately during the boot process and without the need to access the machine over SSH.
- If you are building custom machine images, you can make use of the "user data" or "metadata" passed by the above means in whatever way makes sense to your application, by referring to your vendor's documentation on how to access the data at runtime.
- This approach is required if you intend to use any mechanism in your cloud provider for automatically launching and destroying servers in a group, because in that case individual servers will launch unattended while Terraform is not around to provision them.
- Even if you're deploying individual servers directly with Terraform, passing data this way will allow faster boot times and simplify deployment by avoiding the need for direct network access from Terraform to the new server and for remote access credentials to be provided.

#### Running configuration management software
> **Simplify**
- As a convenience to users who are forced to use generic operating system distribution images, Terraform includes a number of specialized provisioners for launching specific configuration management products.
  - We strongly recommend not using these, and instead running system configuration steps during a custom image build process.
  - For example, HashiCorp Packer offers a similar complement of configuration management provisioners and can run their installation steps during a separate build process, before creating a system disk image that you can deploy many times.
- If you are using configuration management software that has a centralized server component, you will need to delay the registration step until the final system is booted from your custom image.
  - To achieve that, use one of the mechanisms described above to pass the necessary information into each instance so that it can register itself with the configuration management server immediately on boot, without the need to accept commands from Terraform over SSH or WinRM.

#### First-class Terraform provider functionality may be available
> **Investigate**
- It is technically possible to use the local-exec provisioner to run the CLI for your target system in order to create, update, or otherwise interact with remote objects in that system.
  - If you are trying to use a new feature of the remote system that isn't yet supported in its Terraform provider, that might be the only option.
  - However, if there is provider support for the feature you intend to use, prefer to use that provider functionality rather than a provisioner so that Terraform can be fully aware of the object and properly manage ongoing changes to it.
- Even if the functionality you need is not available in a provider today, we suggest to consider `local-exec` usage a temporary workaround and to also open an issue in the relevant provider's repository to discuss adding first-class provider support.
  - Provider development teams often prioritize features based on interest, so opening an issue is a way to record your interest in the feature.
- Provisioners are used to execute scripts on a local or remote machine as part of resource creation or destruction. - - Provisioners can be used to bootstrap a resource, cleanup before destroy, run configuration management, etc.

#### `local-exec` Provisioner
> **Simplify**
- The `local-exec` provisioner invokes a local executable after a resource is created.
  -  Note that even though the resource will be fully created when the provisioner is run, there is no guarantee that it will be in an operable state - for example system services such as `sshd` may not be started yet on compute resources.

  ```hcl
  resource "aws_instance" "web" {
    # ...

    provisioner "local-exec" {
      command = "echo ${aws_instance.web.private_ip} >> private_ips.txt"
    }
  }
  ```

##### `local-exec` Arguments Supported
> **Investigate**
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
> **Investigate**
- `inline` - This is a list of command strings. They are executed in the order they are provided. This cannot be provided with `script` or `scripts`.
- `script` - This is a path (relative or absolute) to a local script that will be copied to the remote resource and then executed. This cannot be provided with `inline` or `scripts`.
- `scripts` - This is a list of paths (relative or absolute) to local scripts that will be copied to the remote resource and then executed. They are executed in the order they are provided. This cannot be provided with `inline` or `script`.

##### Passing Script Arguments
> **Investigate**
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
> **Simplify**
- Other Terraform commands that generate Terraform configuration will produce configuration files that conform to the style imposed by `terraform fmt`, so using this style in your own files will ensure consistency.
- The canonical format may change in minor ways between Terraform versions, so after upgrading Terraform they recommend to proactively run `terraform fmt` on your modules along with any other changes you are making to adopt the new version.

Usage: `terraform fmt [options] [DIR]`

By default, `fmt` scans the current directory for configuration files. If the `dir` argument is provided then it will scan that given directory instead. If `dir` is a single dash (`-`) then `fmt` will read from standard input (STDIN).

The command-line flags are all optional. The list of available flags are:

> **Investigate**
* `-list=false` - Don't list the files containing formatting inconsistencies.
* `-write=false` - Don't overwrite the input files. (This is implied by -check or when the input is STDIN.)
* `-diff` - Display diffs of formatting changes
* `-check` - Check if the input is formatted. Exit status will be 0 if all input is properly formatted and non-zero otherwise.
* `-recursive` - Also process files in subdirectories. By default, only the given directory (or current directory) is processed.

### 4b Given a scenario: choose when to use terraform taint to taint Terraform resources
> **Investigate**
- This command will not modify infrastructure, but does modify the state file in order to mark a resource as tainted.
- Once a resource is marked as tainted, the next plan will show that the resource will be destroyed and recreated and the next apply will implement this change.
- Forcing the recreation of a resource is useful when you want a certain side effect of recreation that is not visible in the attributes of a resource.
  - For example: re-running provisioners will cause the node to be different or rebooting the machine from a base image will cause new startup scripts to run.
- Note that tainting a resource for recreation may affect resources that depend on the newly tainted resource.
  - For example, a DNS resource that uses the IP address of a server may need to be modified to reflect the potentially new IP address of a tainted server.
  - The `plan` command will show this if this is the case.
- Usage: `terraform taint [options] address`
  - The address argument is the address of the resource to mark as tainted. The address is in the resource address syntax syntax, as shown in the output from other commands, such as:
    - `aws_instance.foo`
    - `aws_instance.bar[1]`
    - `aws_instance.baz[\"key\"]`
      - (quotes in resource addresses must be escaped on the command line, so that they are not interpreted by your shell)
    - `module.foo.module.bar.aws_instance.qux`
  - The command-line flags are all optional. The list of available flags are:
    - `-allow-missing` - If specified, the command will succeed (exit code 0) even if the resource is missing. The command can still error, but only in critically erroneous cases.
    - `-backup=path` - Path to the backup file. Defaults to -state-out with the ".backup" extension. Disabled by setting to "-".
    - `-lock=true` - Lock the state file when locking is supported.
    - `-lock-timeout=0s` - Duration to retry a state lock.
    - `-state=path` - Path to read and write the state file to. Defaults to "terraform.tfstate". Ignored when remote state is used.
    - `-state-out=path` - Path to write updated state file. By default, the -state path will be used. Ignored when remote state is used.

> **Investigate**
- https://learn.hashicorp.com/terraform/getting-started/provision#failed-provisioners-and-tainted-resources

#### Tainting a Single Resource
```bash
$ terraform taint aws_security_group.allow_all
The resource aws_security_group.allow_all in the module root has been marked as tainted.
```

#### Tainting a single resource created with for_each
```bash
$ terraform taint 'module.route_tables.azurerm_route_table.rt[\"DefaultSubnet\"]'
The resource module.route_tables.azurerm_route_table.rt["DefaultSubnet"] in the module root has been marked as tainted.
```

#### Tainting a Resource within a Module
```bash
$ terraform taint "module.couchbase.aws_instance.cb_node[9]"
Resource instance module.couchbase.aws_instance.cb_node[9] has been marked as tainted.
```

### 4c Given a scenario: choose when to use terraform import to import existing infrastructure into your Terraform state

> **Investigate**
- https://www.terraform.io/docs/commands/import.html

### 4d Given a scenario: choose when to use terraform workspace to create workspaces

> **Investigate**
- https://www.terraform.io/docs/state/workspaces.html

### 4e Given a scenario: choose when to use terraform state to view Terraform state

> **Investigate**
- https://www.terraform.io/docs/commands/state/index.html

### 4f Given a scenario: choose when to enable verbose logging and what the outcome/value is

> **Investigate**
- https://www.terraform.io/docs/internals/debugging.html

## 5 Interact with Terraform modules

### 5a Contrast module source options

> **Investigate**
- https://www.terraform.io/docs/registry/modules/use.html#finding-modules

### 5b Interact with module inputs and outputs

> **Investigate**
- https://learn.hashicorp.com/terraform/modules/modules-overview

### 5c Describe variable scope within modules/child modules

> **Investigate**
- https://www.terraform.io/docs/configuration/variables.html

> **Investigate**
- https://www.terraform.io/docs/configuration/modules.html#calling-a-child-module

### 5d Discover modules from the public Terraform Module Registry

> **Investigate**
- https://www.terraform.io/docs/registry/modules/use.html#using-modules

### 5e Defining module version

> **Investigate**
- https://www.terraform.io/docs/configuration/modules.html#module-versions

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
- By default, Terraform uses the "local" backend, which is the normal behavior of Terraform you're used to.
- The local backend stores state on the local filesystem, locks that state using system APIs, and performs operations locally.
> **Investigate**
- By default, the path to the state file is "terraform.tfstate" relative to the root module.
  - Modify using the `path` variable:
  ```hcl
  terraform {
    backend "local" {
      path = "relative/path/to/terraform.tfstate"
    }
  }
  ```
- **workspace_dir** - optionally provides the path to non-default workspaces.

### 7b Outline state locking
> **Simplify**
- Some backends do not support state locking or have it optionally:
  - [artifactory](https://www.terraform.io/docs/backends/types/artifactory.html)
  - [etcd](https://www.terraform.io/docs/backends/types/etcd.html)
  - [http](https://www.terraform.io/docs/backends/types/http.html) (Optional)
- If supported by your backend, state locking happens automatically on all operations that could write state.
- You won't see any message that it is happening. If state locking fails, Terraform will not continue. You can disable state locking for most commands with the `-lock` flag but it is not recommended.
- If acquiring the lock is taking longer than expected, Terraform will output a status message.
- If Terraform doesn't output a message, state locking is still occurring if your backend supports it.

#### Force Unlock
> **Investigate**
- `terraform force-unlock LOCK_ID [DIR]`
  - **`-force`** - Don't ask for input for unlock confirmation.
- Terraform has a `force-unlock` command to manually unlock the state if unlocking failed.
- **Be very careful with this command.**
  - If you unlock the state when someone else is holding the lock it could cause multiple writers.
  - Force unlock should only be used to unlock your own lock in the situation where automatic unlocking failed.
- To protect you, the `force-unlock` command requires a unique lock ID. Terraform will output this lock ID if unlocking fails. This lock ID acts as a nonce, ensuring that locks and unlocks target the correct lock.

### 7c Handle backend authentication methods
> **Investigate**
- **`token`** - (Optional, not recommended) The token used to authenticate with the remote backend.
```hcl
terraform {
  backend "remote" {
    hostname = "app.terraform.io"
    token = "xxxxxxxxxxxxxxxxxxx"
    organization = "company"

    workspaces {
      name = "my-app-prod"
    }
  }
}
```
- They recommend omitting the token from the configuration as shown above, and instead using `terraform login` or manually configuring credentials in the CLI config file (`.terraformrc` on all OS except Windows, `terraform.rc`).
  - `terraform login [hostname]` - command to automatically obtain and save an API token for TF Cloud, Enterprise, or other host. If no explicit hostname, defaults to Terraform Cloud [app.terraform.io](https://app.terraform.io/).
    - By default, Terraform will obtain an API token and save it in plain text in a local CLI configuration file called `credentials.tfrc.json`.
    - When you run `terraform login`, it will explain specifically where it intends to save the API token and give you a chance to cancel if the current configuration is not as desired.
  - In the CLI config file (`.terraformrc` on all OS except Windows, `terraform.rc`), use a `credentials` block to set the token:
  ```bash
  credentials "app.terraform.io" {
    token = "xxxxxx.atlasv1.zzzzzzzzzzzzz"
  }

  plugin_cache_dir = "$HOME/.terraform.d/plugin-cache"
  ```

### 7d Describe remote state storage mechanisms and supported standard backends
> **Simplify**
- Most backends run all operations on the local system — although Terraform stores its state remotely with these backends, it still executes its logic locally and makes API requests directly from the system where it was invoked.
- This is simple to understand and work with, but when many people are collaborating on the same Terraform configurations, it requires everyone's execution environment to be similar.
  - This includes sharing access to infrastructure provider credentials, keeping Terraform versions in sync, keeping Terraform variables in sync, and installing any extra software required by Terraform providers.
  - This becomes more burdensome as teams get larger.
- Some backends can run operations (plan, apply, etc.) on a remote machine, while appearing to execute locally. This enables a more consistent execution environment and more powerful access controls, without disrupting workflows for users who are already comfortable with running Terraform.
- Currently, the **remote backend** is the only backend to support remote operations, and **Terraform Cloud** is the only remote execution environment that supports it.
- Standard backends:
  - artifactory
  - azurerm
  - consul
  - cos
  - etcd
  - etcdv3
  - gcs
  - http
  - manta
  - oss
  - pg
  - s3
  - swift
  - terraform enterprise

### 7e Describe effect of Terraform refresh on state
> **Investigate**
- `terraform state` detects drift from the last known state and updates the state file.
- It does not modify **infrastructure** but it does modify the **state file**.
- Usage: terraform refresh [options] [dir]
  - By default, refresh requires no flags and looks in the current directory for the configuration and state file to refresh.
  - `-backup=path` - Path to the backup file. Defaults to -state-out with the ".backup" extension. Disabled by setting to "-".
  - `-compact-warnings` - If Terraform produces any warnings that are not accompanied by errors, show them in a more compact form that includes only the summary messages.
  - `-input=true` - Ask for input for variables if not directly set.
  - `-lock=true` - Lock the state file when locking is supported.
  - `-lock-timeout=0s` - Duration to retry a state lock.
  - `-no-color` - If specified, output won't contain any color.
  - `-parallelism=n` - Limit the number of concurrent operation as Terraform walks the graph. Defaults to 10.
  - `-state=path` - Path to read and write the state file to. Defaults to "terraform.tfstate". Ignored when remote state is used.
  - `-state-out=path` - Path to write updated state file. By default, the -state path will be used. Ignored when remote state is used.
  - `-target=resource` - A Resource Address to target. Operation will be limited to this resource and its dependencies. This flag can be used multiple times.
  - `-var 'foo=bar'` - Set a variable in the Terraform configuration. This flag can be set multiple times. Variable values are interpreted as HCL, so list and map values can be specified via this flag.
  - `-var-file=foo` - Set variables in the Terraform configuration from a variable file. If a terraform.tfvars or any .auto.tfvars files are present in the current directory, they will be automatically loaded. terraform.tfvars is loaded first and the .auto.tfvars files after in alphabetical order. Any files specified by -var-file override any values set automatically from files in the working directory. This flag can be used multiple times.

### 7f Describe backend block in configuration and best practices for partial configurations

#### Backend Configuration
> **Simplify**
- Backends are configured directly in Terraform files in the terraform section. After configuring a backend, it has to be **initialized**.
- Example configuring the "consul" backend:
  ```hcl
  terraform {
    backend "consul" {
      address = "demo.consul.io"
      scheme  = "https"
      path    = "example_app/terraform_state"
    }
  }
  ```
- You specify the backend type as a key to the backend stanza.
  - Within the stanza are backend-specific configuration keys.
- Only one backend may be specified and the configuration may not contain interpolations.
  - Terraform will validate this.

#### First Time Configuration
> **Simplify**
- When configuring a backend for the first time (moving from no defined backend to explicitly configuring one), Terraform will give you the option to migrate your state to the new backend.
  - This lets you adopt backends without losing any existing state.
- To be extra careful, we always recommend manually backing up your state as well.
  - You can do this by simply copying your terraform.tfstate file to another location.
  - The initialization process should create a backup as well, but it never hurts to be safe!
- Configuring a backend for the first time is no different than changing a configuration in the future: create the new configuration and run `terraform init`.
  - Terraform will guide you the rest of the way.

#### Partial Configuration
> **Investigate**
- You do not need to specify every required argument in the backend configuration.
  - Omitting certain arguments may be desirable to avoid storing secrets, such as access keys, within the main configuration.
  - When some or all of the arguments are omitted, we call this a **partial configuration**.
- With a partial configuration, the remaining configuration arguments must be provided as part of the initialization process. There are several ways to supply the remaining arguments:
  - **Interactively**: Terraform will interactively ask you for the required values, unless interactive input is disabled. Terraform will not prompt for optional values.
  - **File**: A configuration file may be specified via the init command line. To specify a file, use the -backend-config=PATH option when running terraform init. If the file contains secrets it may be kept in a secure data store, such as Vault, in which case it must be downloaded to the local disk before running Terraform.
  - **Command-line key/value pairs**: Key/value pairs can be specified via the init command line. Note that many shells retain command-line flags in a history file, so this isn't recommended for secrets. To specify a single key/value pair, use the -backend-config="KEY=VALUE" option when running terraform init.
- If backend settings are provided in multiple locations, the **top-level settings** are merged such that any c**ommand-line options override** the settings in the main configuration and then the **command-line options are processed in order**, with **later options overriding values set by earlier options**.
- The final, merged configuration is stored on disk in the `.terraform` directory, which should be **ignored from version control**. This means that **sensitive information can be omitted from version control**, but it will be present in plain text on local disk when running Terraform.
- When using partial configuration, Terraform requires at a minimum that an empty backend configuration is specified in one of the root Terraform configuration files, to specify the backend type. For example:
```hcl
terraform {
  backend "consul" {}
}
```
- A backend configuration file has the contents of the backend block as top-level attributes, without the need to wrap it in another terraform or backend block:
```hcl
address = "demo.consul.io"
path    = "example_app/terraform_state"
scheme  = "https"
```
- The same settings can alternatively be specified on the command line as follows:
```bash
$ terraform init \
    -backend-config="address=demo.consul.io" \
    -backend-config="path=example_app/terraform_state" \
    -backend-config="scheme=https"
```

### 7g Understand secret management in state files

#### Sensitive Data in State
> **Simplify**
- Terraform state can contain sensitive data, depending on the resources in use and your definition of "sensitive."
  - The state contains resource IDs and all resource attributes.
  - For resources such as databases, this may contain initial passwords.
- When using local state, state is stored in plain-text JSON files.
- When using remote state, state is only ever held in memory when used by Terraform.
  - It may be encrypted at rest, but this depends on the specific remote state backend.

#### Recommendations
> **Simplify**
- If you manage any sensitive data with Terraform (like database passwords, user passwords, or private keys), treat the state itself as sensitive data.
- Storing state remotely can provide better security.
  - As of Terraform 0.9, Terraform does not persist state to the local disk when remote state is in use, and some backends can be configured to encrypt the state data at rest.
- For example:
  - **Terraform Cloud** always encrypts state at rest and protects it with TLS in transit.
    - Terraform Cloud also knows the identity of the user requesting state and maintains a history of state changes.
    - This can be used to control access and track activity.
    - Terraform Enterprise also supports detailed audit logging.
  - **The S3 backend** supports encryption at rest when the encrypt option is enabled.
    - IAM policies and logging can be used to identify any invalid access.
    - Requests for the state go over a TLS connection.

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

#### Sentinel
- **Define the policies in TF Cloud** - Policies are defined using the policy language with imports for parsing the Terraform plan, state and configuration, e.g.,
  ```hcl
  import "time"

  # Validate time is between 8 AM and 4 PM
  valid_time = rule { time.now.hour >= 8 and time.now.hour < 16 }

  # Validate day is M - Th
  valid_day = rule {
      time.now.weekday_name in ["Monday", "Tuesday", "Wednesday", "Thursday"]
  }

  main = rule { valid_time and valid_day }
  ```

- **Managing policies for organizations** - Users with permission to manage policies can add policies to their organization by configuring VCS integration or uploading policy sets through the API. They also define which workspaces the policy sets are checked against during runs.
- **Enforcing policy checks on runs** - Policies are checked when a run is performed, after the `terraform plan` but before it can be confirmed or the `terraform apply` is executed.
- **Mocking Sentinel Terraform data** - Terraform Cloud provides the ability to generate mock data for any run within a workspace. This data can be used with the Sentinel CLI to test policies before deployment.


#### Registry
- Terraform Cloud's private module registry helps you share Terraform modules across your organization.
  - It includes support for module versioning, a searchable and filterable list of available modules, and a configuration designer to help you build new workspaces faster.
- By design, the private module registry works much like the public Terraform Registry.
  - If you're already used the public registry, Terraform Cloud's registry will feel familiar.
- Note: Currently, the private module registry works with all supported VCS providers; however, the private module registry does not support GitLab subgroups.

#### Workspaces

##### Workspaces are Collections of Infrastructure
- Working with Terraform involves managing collections of infrastructure resources, and most organizations manage many different collections.
- When run locally, Terraform manages each collection of infrastructure with a persistent working directory, which contains a configuration, state data, and variables.
  - Since Terraform CLI uses content from the directory it runs in, you can organize infrastructure resources into meaningful groups by keeping their configurations in separate directories.
- Terraform Cloud manages infrastructure collections with workspaces instead of directories. A workspace contains everything Terraform needs to manage a given collection of infrastructure, and separate workspaces function like completely separate working directories.
- Note: Terraform Cloud and Terraform CLI both have features called "workspaces," but they're slightly different. CLI workspaces are alternate state files in the same working directory; they're a convenience feature for using one configuration to manage multiple similar groups of resources.

**Workspace Contents**

| Component               | Local Terraform                                             | Terraform Cloud                                                            |
|-------------------------|-------------------------------------------------------------|----------------------------------------------------------------------------|
| Terraform configuration | On disk                                                     | In linked version control repository, or periodically uploaded via API/CLI |
| Variable values         | As .tfvars files, as CLI arguments, or in shell environment | In workspace                                                               |
| State                   | On disk or in remote backend                                | In workspace                                                               |
| Credentials and secrets | In shell environment or entered at prompts                  | In workspace, stored as sensitive variables                                |

- **State versions**: Each workspace retains backups of its previous state files. Although only the current state is necessary for managing resources, the state history can be useful for tracking changes over time or recovering from problems. (See also: State.)
- **Run history**: When Terraform Cloud manages a workspace's Terraform runs, it retains a record of all run activity, including summaries, logs, a reference to the changes that caused the run, and user comments. (See also: Viewing and Managing Runs.)

##### Listing and Filtering Workspaces
- This list only includes workspaces where the current user account has permission to read runs.
- The following filters are available:
  - **Status filters**: These filters sort workspaces by the status of their current run. There are four quick filter buttons that collect the most commonly used groups of statuses (success, error, needs attention, and running), and a custom filter button (with a funnel icon) where you can select any number of statuses from a menu.
  When you choose a status filter, the list will only include workspaces whose current runs match the selected statuses. You can remove the status filter by clicking the "All" button, or by unchecking everything in the custom filter menu.
  - **List order**: The list order button is marked with two arrows, pointing up and down. You can choose to order the list by time or by name, in forward or reverse order.
  - **Name filter**: The search field at the far right of the filter bar lets you filter workspaces by name. If you enter a string in this field and press enter, only workspaces whose names contain that string will be shown.
  The name filter can combine with a status filter, to narrow the list down further.

##### Planning and Organizing Workspaces
- We recommend that organizations break down large monolithic Terraform configurations into smaller ones, then assign each one to its own workspace and delegate permissions and responsibilities for them. Terraform Cloud can manage monolithic configurations just fine, but managing smaller infrastructure components like this is the best way to take full advantage of Terraform Cloud's governance and delegation features.
- For example, the code that manages your production environment's infrastructure could be split into a networking configuration, the main application's configuration, and a monitoring configuration. After splitting the code, you would create "networking-prod", "app1-prod", "monitoring-prod" workspaces, and assign separate teams to manage them.
- Much like splitting monolithic applications into smaller microservices, this enables teams to make changes in parallel. In addition, it makes it easier to re-use configurations to manage other environments of infrastructure ("app1-dev," etc.).

### 9b Differentiate OSS and Terraform Cloud workspaces

#### OSS Workspaces

Creating a new workspace:

```
$ terraform workspace new bar
Created and switched to workspace "bar"!

You're now on a new, empty workspace. Workspaces isolate their state,
so if you run "terraform plan" Terraform will not see any existing state for this configuration.
```

As the command says, if you run `terraform plan`, Terraform will not see any existing resources that existed on the default (or any other) workspace. These resources still physically exist, but are managed in another Terraform workspace.

### 9c Summarize features of Terraform Cloud

| Cloud Free              | Cloud Team              | Cloud Team & Governance | Enterprise Self-Hosted       |
|-------------------------|-------------------------|-------------------------|------------------------------|
| VCS Integration         | VCS Integration         | VCS Integration         | VCS Integration              |
| Workspace Management    | Workspace Management    | Workspace Management    | Workspace Management         |
| Secure Variable Storage | Secure Variable Storage | Secure Variable Storage | Secure Variable Storage      |
| Remote Runs & Applies   | Remote Runs & Applies   | Remote Runs & Applies   | Remote Runs & Applies        |
| Full API Coverage       | Full API Coverage       | Full API Coverage       | Full API Coverage            |
| Private Module Registry | Private Module Registry | Private Module Registry | Private Module Registry      |
|                         | Roles / Team Management | Roles / Team Management | Roles / Team Management      |
|                         |                         | Sentinel                | Sentinel                     |
|                         |                         | Cost Estimation         | Cost Estimation              |
|                         |                         |                         | SAML / SSO                   |
|                         |                         |                         | Private DC Installation      |
|                         |                         |                         | Private Network Connectivity |
|                         |                         |                         | Self-Hosted                  |
|                         |                         |                         | Audit Logs                   |