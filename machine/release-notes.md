---
title: Docker Machine リリースノート
description: Release notes for Docker Machine
keywords: release notes, machine
toc_max: 2
redirect_from:
  - /release-notes/docker-machine/
---

## 0.16.2 (2019-09-02)

{% comment %}
### General
{% endcomment %}
{: #general }
### 全般

{% comment %}
{% endcomment %}
* Golang 1.12.9 によりコンパイルしました。
* VTX 検出を修正しました。

{% comment %}
### Drivers
{% endcomment %}
{: #drivers }
### ドライバー

{% comment %}
`amazonec2`
* Add ssh-port flag
* Add eu-north-1 zone
{% endcomment %}
`amazonec2`
* ssh-port フラグを追加しました。
* ゾーン eu-north-1 を追加しました。

{% comment %}
`vmwarefusion`
* Add nonempty flag to fix share-folder bug
{% endcomment %}
`vmwarefusion`
* 共有フォルダーのバグ修正のために、空でないフラグを追加しました。

{% comment %}
`hyperv`
* Allow localized names for Virtual Switch
{% endcomment %}
`hyperv`
* Virtual Switch 向けにローカル名を可能にしました。

{% comment %}
`google`
* Add support for non-default service account
{% endcomment %}
`google`
* デフォルトではないサービスアカウントに対応しました。

{% comment %}
`openstack`
* Add flag for metadata
{% endcomment %}
`openstack`
* メタデータ用のフラグを追加しました。

{% comment %}
`softlayer`
* Don't set the request method again
{% endcomment %}
`softlayer`
* リクエストメソッドを再設定しないようにしました。

## 0.16.0 (2018-11-08)

{% comment %}
### General
{% endcomment %}
{: #general-1 }
### 全般

{% comment %}
* Updated the default storage driver to `overlay2` for several systems.
* Improved error reporting for the `ssh` subcommand when using the `--native-ssh` flag.
{% endcomment %}
* Updated the default storage driver to `overlay2` for several systems.
* Improved error reporting for the `ssh` subcommand when using the `--native-ssh` flag.

{% comment %}
### Drivers
{% endcomment %}
{: #drivers-1 }
### ドライバー

{% comment %}
`amazonec2`
* Improved handling of VPC errors.
{% endcomment %}
`amazonec2`
* VPC エラーの扱いを改善しました。

{% comment %}
`openstack`
* Machine removal no longer fails upon attempting to delete a nonexistent keypair.
{% endcomment %}
`openstack`
* 存在しないキーペアの削除があったとしても、マシン削除が失敗しないようにしました。

## 0.15.0 (2018-06-12)

{% comment %}
### General
{% endcomment %}
{: #general-2 }
### 全般

{% comment %}
* `docker-machine` can now be installed using `go install`.
* Docker Machine is now built with Go `1.10`.
* SSH connections now include a keep alive option. (docker/machine #4450)
{% endcomment %}
* `docker-machine`を`go install`を使ってインストールできるようにしました。
* Docker Machine を Go `1.10` を使ってビルドしました。
* SSH 接続においてキープアライブオプションを含めました。(docker/machine #4450)

{% comment %}
### Drivers
{% endcomment %}
{: #drivers-2 }
### ドライバー

{% comment %}
{% endcomment %}
`amazonec2`
* Updated default AMIs to mitigate Meltdown and Spectre.
* Added `--amazonec2-security-group-readonly` flag to prevent mutating security groups.

{% comment %}
{% endcomment %}
`exoscale`
* Updated driver to `v0.9.23`.

{% comment %}
{% endcomment %}
`hyperv`
* Fixed Hyper-V precreate issues. (docker/machine #4426)
* Added the ability to disable Hyper-V dynamic memory management during VM creation with `--hyperv-disable-dynamic-memory`.

{% comment %}
{% endcomment %}
`vmwarefusion`
* Improved shell checks (docker/machine #4491).

## 0.14.0 (2018-03-06)

{% comment %}
### General
{% endcomment %}
{: #general-3 }
### 全般

{% comment %}
{% endcomment %}
* Added `--client-certs` flag to the `docker-machine regenerate-certs` command.
* Improved OpenBSD support.
* Fixed a bug with `scp` commands issued from a Windows host.
* Enabled progress output by default for `scp` commands using `rsync`.
* Added `--quiet` flag to `scp` to suppress progress output.
* Machine now uses the `ss` command to detect connectivity when `netstat` is unavailable.
* Added bash completion for `docker-machine mount`.
* Improved provisioning resilience on Debian-based hosts.

{% comment %}
### Drivers
{% endcomment %}
{: #drivers-3 }
### ドライバー

{% comment %}
{% endcomment %}
`amazonec2`
* Added support for `eu-west-3` region
* Upon failure, the `create` command now ensures dangling resources are cleaned up before exiting
* Machine creation no longer fails when waiting on spot instance readiness

{% comment %}
{% endcomment %}
`digitalocean`
* Added `--digitalocean-monitoring` flag
* Increased the default droplet size

{% comment %}
{% endcomment %}
`exoscale`
* Updated driver library
* Several improvements and fixes to the default machine template
* Added support for user-provided SSH key (`--exoscale-ssh-key`)
* Added support for arbitrary disk size

{% comment %}
{% endcomment %}
`google`
* Enabled disk auto-deletion on newly created machines.
* Fixed a bug preventing the removal of a machine if it had already been removed remotely.
* Added support for fully qualified network and subnetwork names.

{% comment %}
{% endcomment %}
`hyperv`
* Fixed potential cmdlet collision with VMWare powercli.
* Fixed a bug with virtual switch selection.
* Machine now correctly detects if the user is a Hyper-V administrator when using a localized version of Windows.

{% comment %}
{% endcomment %}
`openstack`
* Added `--openstack-config-drive` flag
* Fixed an issue causing some user-uploaded keypairs to be removed when removing the associated machine.
* Fixed a bug preventing the removal of a machine if it had already been removed remotely.

{% comment %}
{% endcomment %}
`virtualbox`
* Added OpenBSD support

{% comment %}
{% endcomment %}
`vmwarefusion`
* Improved error detection and reporting when creating a new instance.

{% comment %}
{% endcomment %}
`vmwarevsphere`
* Added  `--vmwarevsphere-folder` flag.

## 0.13.0 (2017-10-12)

{% comment %}
### General
{% endcomment %}
{: #general-4 }
### 全般

{% comment %}
{% endcomment %}
- Added new `docker-machine mount` command for mounting machine directories over SSHFS.
- Improved some logging messages.
- Fixed a bug with the `scp` command when using an identity file.
- Fixed a parsing error that caused the boot2docker ISO cache to malfunction, forcing a new download everytime.

{% comment %}
### Drivers
{% endcomment %}
{: #drivers-4 }
### ドライバー

{% comment %}
{% endcomment %}
- `azure`
  - `docker-machine rm` now also cleans up the associated storage account if it has no remaining storage containers.
  - The creation process will no longer recreate the associated subnet if it already it exists.
- `exoscale`
  - Updated driver.
  - Removed default `docker-machine` affinity group if no other affinity group was specified.
- `virtualbox`
  - Fixed a bug where the machine would sometimes be assigned an invalid IP address at creation time.
- `vmwaresphere`
  - Added support for multiple networks.

## 0.12.2 (2017-7-12)

{% comment %}
### General
{% endcomment %}
{: #general-5 }
### 全般

{% comment %}
{% endcomment %}
* The `scp` sub-command now allows to provide an optional `user@` to the address.
* Fixed bash completion on OS X.

{% comment %}
### Drivers
{% endcomment %}
{: #drivers-5 }
### ドライバー

{% comment %}
{% endcomment %}
* `amazonec2`
  * Updated default AMIs to the latest version of Ubuntu 16.04 LTS.
  * Fixed a bug preventing proper machine removal.
* `vmwarevsphere`
  * Creating VMs on a DRS-enabled cluster should now work properly.
  * Fixed a bug that prevented provisioning.
* `vmwarefusion`
  * Fixed a bug that prevented provisioning.
* `exoscale`
  * Updated library.

## 0.12.1 (2017-6-30)

{% comment %}
### General
{% endcomment %}
{: #general-6 }
### 全般

{% comment %}
{% endcomment %}
- Fixed an issue with the version comparison function that prevented machines created with Engine 17.06.0 from starting properly.

## 0.12.0 (2017-6-5)

{% comment %}
### General
{% endcomment %}
{: #general-7 }
### 全般

{% comment %}
{% endcomment %}
- Various bash completion improvements.
- Bump Go to version 1.8.3.

{% comment %}
### Drivers
{% endcomment %}
{: #drivers-6 }
### ドライバー

{% comment %}
{% endcomment %}
- `openstack`
    - Enable `HTTP_PROXY`
- `digitalocean`
    - Add support for tagging.
- `virtualbox`
    - Scope DHCP address range based on CIDR
- `generic`
    - Increase default timeout
- `google`
    - Add subnetwork support

{% comment %}
{% endcomment %}
### Provisioners

{% comment %}
{% endcomment %}
- Remove restriction on `--engine-install-url` in default-to-boot2docker drivers (`virtualbox`, `vmwarefusion`, etc.)
- Reduce provisioning time of SUSE/openSUSE systems

## 0.11.0 (2017-4-25)

{% comment %}
### General
{% endcomment %}
{: #general-8 }
### 全般

{% comment %}
{% endcomment %}
- Various bugfixes and updated library dependencies
- new `docker-machine scp --delta` to invoke `rsync` behind the scenes for more efficient transfer

{% comment %}
### Drivers
{% endcomment %}
{: #drivers-7 }
### ドライバー

{% comment %}
{% endcomment %}
- `digitalocean`
  - Add support for tagging DigitalOcean instances.
- `google`
  - Add support for subnetworks


## 0.10.0 (2017-2-27)

{% comment %}
### General
{% endcomment %}
{: #general-9 }
### 全般

{% comment %}
{% endcomment %}
- Various improvements to shell tab completion
- Add support for compiling on ARM64 architecture

{% comment %}
### Drivers
{% endcomment %}
{: #drivers-8 }
### ドライバー

{% comment %}
{% endcomment %}
- Make `virtualbox` default driver
- `amazonec2`
    - Update AMIs to latest version of Ubuntu 16.04 LTS
- `virtualbox`
    - Fix parsing of `--virtualbox-share-folder` on Windows
- `google`
    - Add `--google-open-port` flag to specify additional ports to open

{% comment %}
{% endcomment %}
### Provisioners

{% comment %}
{% endcomment %}
- Machine now uses systemd drop-in files instead of over-writing the system units
- Add support for upgrade to new Docker versioning scheme
- Use `dockerd` only in Docker versions where it is available
- Support multiple architectures in SUSE provisioner

## 0.9.0 (2017-1-17)

{% comment %}
### General
{% endcomment %}
{: #general-10 }
### 全般
{% comment %}
{% endcomment %}
- On Windows, the `COMPOSE_CONVERT_WINDOWS_PATHS` environment variable is now set by `docker-machine env` to improve Compose usability.
- Docker Machine can now be built on FreeBSD
- `docker-machine scp` non-22 port support
- `scp` supports SSH agent
- Bump Go version to 1.7.4

{% comment %}
### Drivers
{% endcomment %}
{: #drivers-9 }
### ドライバー
{% comment %}
{% endcomment %}
- `amazonec2`
  - Credentials can now be loaded from IAM instance profiles
  - Add `--amazonec2-userdata` flag
  - Add `--amazonec2-block-duration-minutes` flag
  - Add support for `us-east-2` (Ohio)
  - Update base images to Ubuntu 16.04
{% comment %}
{% endcomment %}
- `azure`
  - Add `--azure-dns` flag for specifying DNS names
  - Add `--azure-storage-type` flag
  - Allow using vnets from another resource group
  - Add AzureGermanCloud support
  - Add support for custom data
  - Support Service Principal authentication
  - Update base images to Ubuntu 16.04
{% comment %}
{% endcomment %}
- `digitalocean`
  - Add ability to specify the private SSH key path
{% comment %}
{% endcomment %}
- `gce`
  - Update base images to Ubuntu 16.04
{% comment %}
{% endcomment %}
- `virtualbox`
  - Shared folder location can be specified instead of "hardcoded" to `C:\Users` or `/Users`
{% comment %}
{% endcomment %}
- `openstack`
  - Add support for `OS_CACERT`

{% comment %}
{% endcomment %}
### Provisioners
{% comment %}
{% endcomment %}
- OpenSUSE provisioner refactored to use properly supported 3rd party code


## # 0.8.2 (2016-8-26)

{% comment %}
{% endcomment %}
- Update Go version to 1.7.1

## 0.8.1 (2016-8-20)

{% comment %}
{% endcomment %}
### Provisioners
{% comment %}
{% endcomment %}
- Fix issue with generated systemd service file on RedHat family distros

{% comment %}
### Drivers
{% endcomment %}
{: #drivers-10 }
### ドライバー
{% comment %}
{% endcomment %}
- `azure`
    - Bump Ubuntu image to 16.04
    - Update docs with updated default parameters
    - Change logging slightly

## 0.8.0 (2016-6-14)

{% comment %}
### General
{% endcomment %}
{: #general-11 }
### 全般
{% comment %}
{% endcomment %}
- Fix issue with plugin heartbeat log repeating on disconnect
- Add `tcsh` support to `env --shell`
- Add `zsh` completion scripts
- Bump Go version to 1.6.2

{% comment %}
### Drivers
{% endcomment %}
{: #drivers-11 }
### ドライバー
{% comment %}
{% endcomment %}
- `amazonec2`
    - Workaround to prevent orphaned SSH keys
- `virtualbox`
    - Add option for VM UI type (`--virtualbox-ui-type`)
- `vmwarefusion`
    - Fix CPU option inconsistency
- `openstack`
    - Expose user data parameter (`--openstack-user-data-file`)
- `generic`
    - Copy public key to created Machine directory

{% comment %}
{% endcomment %}
### Provisioners
{% comment %}
{% endcomment %}
- Add Oracle Enterprise Linux support
- Fix port binding of Swarm master
- Add ability to create a manager instance which does not get scheduled on
- Introduce `--swarm-join-opt` to pass options to agent nodes
- Various SSH-related fixes
- Fix state for upgrade path

## 0.7.0 (2016-4-13)

{% comment %}
### General
{% endcomment %}
{: #general-12 }
### 全般
{% comment %}
{% endcomment %}
- `DRIVER` environment variable now supported to supply value for `create --driver` flag
- Update to Go 1.6.1
- SSH client has been refactored
- RC versions of Machine will now create and upgrade to boot2docker RCs instead
  of stable versions if available

{% comment %}
### Drivers
{% endcomment %}
{: #drivers-12 }
### ドライバー
{% comment %}
{% endcomment %}
- `azure`
    - Driver has been completely re-written to use resource templates and a significantly easier-to-use authentication model
- `digitalocean`
    - New `--digitalocean-ssh-key-fingerprint` for using existing SSH keys instead of creating new ones
- `virtualbox`
    - Fix issue with `bootlocal.sh`
    - New `--virtualbox-nictype` flag to set driver for NAT network
    - More robust host-only interface collision detection
    - Add support for running VirtualBox on a Windows 32 bit host
    - Change default DNS passthrough handling
- `amazonec2`
    - Specifying multiple security groups to use is now supported
- `exoscale`
    - Add support for user-data
- `hyperv`
    - Machines can now be created by a non-administrator
- `rackspace`
    - New `--rackspace-active-timeout` parameter
- `vmwarefusion`
    - Bind mount shared folder directory by default
- `google`
    - New `--google-use-internal-ip-only` parameter

{% comment %}
{% endcomment %}
### Provisioners
{% comment %}
{% endcomment %}
- General
    - Support for specifying Docker engine port in some cases
- CentOS
    - Now defaults to using upstream `get.docker.com` script instead of custom RPMs.
- boot2docker
    - More robust eth* interface detection
- Swarm
    - Add `--swarm-experimental` parameter to enable experimental Swarm features


## 0.6.0 (2016-02-04)

{% comment %}
{% endcomment %}
+ Fix SSH wait before provisioning issue

## 0.6.0-rc4 (2016-02-03)

{% comment %}
### General
{% endcomment %}
{: #general-13 }
### 全般

{% comment %}
{% endcomment %}
+ `env`
    + Fix shell auto detection

{% comment %}
### Drivers
{% endcomment %}
{: #drivers-13 }
### ドライバー

{% comment %}
{% endcomment %}
+ `exoscale`
    + Fix configuration of exoscale endpoint

## 0.6.0-rc3 (2016-02-01)

{% comment %}
{% endcomment %}
- Exit with code 3 if error is during pre-create check

## 0.6.0-rc2 (2016-01-28)

{% comment %}
{% endcomment %}
- Fix issue creating Swarms
- Fix `ls` header issue
- Add code to wait for Docker daemon before returning from `start` / `restart`
- Start porting integration tests to Go from BATS
- Add Appveyor for Windows tests
- Update CoreOS provisioner to use `docker daemon`
- Various documentation and error message fixes
- Add ability to create GCE machine using existing VM

## 0.6.0-rc1 (2016-01-18)

{% comment %}
### General
{% endcomment %}
{: #general-14 }
### 全般

{% comment %}
{% endcomment %}
- Update to Go 1.5.3
- Short form of command invocations is now supported
    - `docker-machine start`, `docker-machine stop` and others will now use
      `default` as the machine name argument if one is not specified
- Fix issue with panics in drivers
- Machine now returns exit code 3 if the pre-create check fails.
    - This is potentially useful for scripting `docker-machine`.
- `docker-machine provision` command added to allow re-running of provisioning
  on instances.
    - This allows users to re-run provisioning if it fails during `create`
      instead of needing to completely start over.

{% comment %}
{% endcomment %}
### Provisioning

{% comment %}
{% endcomment %}
- Most provisioners now use `docker daemon` instead of `docker -d`
- Swarm masters now run with replication enabled
- If `/var/lib` is a BTRFS partition, `btrfs` will now be used as the storage
  driver for the instance

{% comment %}
### Drivers
{% endcomment %}
{: #drivers-14 }
### ドライバー

{% comment %}
{% endcomment %}
- Amazon EC2
    - Default VPC will be used automatically if none is specified
    - Credentials are now be read from the conventional `~/.aws/credentials`
      file automatically
    - Fix a few issues such as nil pointer dereferences
- VMware Fusion
    - Try to get IP from multiple DHCP lease files
- OpenStack
    - Only derive tenant ID if tenant name is supplied

## 0.5.6 (2016-01-11)

{% comment %}
### General
{% endcomment %}
{: #general-15 }
### 全般

{% comment %}
{% endcomment %}
- `create`
  - Set swarm master to advertise on port 3376
  - Fix swarm restart policy
  - Stop asking for ssh key passwords interactively
- `env`
  - Improve documentation
  - Fix bash on windows
  - Automatic shell detection on Windows
- `help`
  - Don't show the full path to `docker-machine.exe` on windows
- `ls`
  - Allow custom format
  - Improve documentation
- `restart`
  - Improve documentation
- `rm`
  - Improve documentation
  - Better user experience when removing multiple hosts
- `version`
  - Don't show the full path to `docker-machine.exe` on windows
- `start`, `stop`, `restart`, `kill`
  - Better logs and homogeneous behaviour across all drivers

{% comment %}
{% endcomment %}
### Build

{% comment %}
{% endcomment %}
- Introduce CI tests for external binary compatibility
- Add amazon EC2 integration test

{% comment %}
{% endcomment %}
### Misc

{% comment %}
{% endcomment %}
- Improve BugSnags reports: better shell detection, better windows version detection
- Update DockerClient dependency
- Improve bash-completion script
- Improve documentation for bash-completion

{% comment %}
### Drivers
{% endcomment %}
{: #drivers-15 }
### ドライバー

{% comment %}
{% endcomment %}
- Amazon EC2
  - Improve documentation
  - Support optional tags
  - Option to create EbsOptimized instances
- Google
  - Fix remove when instance is stopped
- Openstack
  - Flags to import and reuse existing nova keypairs
- VirtualBox
  - Fix multiple bugs related to host-only adapters
  - Retry commands when `VBoxManage` is not ready
  - Reject VirtualBox versions older that 4.3
  - Fail with a clear message when Hyper-v installation prevents VirtualBox from working
  - Print a warning for Boot2Docker v1.9.1, which is known to have an issue with AUFS
- Vmware Fusion
  - Support soft links in VM paths

{% comment %}
{% endcomment %}
### Libmachine

{% comment %}
{% endcomment %}
- Fix code sample that uses libmachine
- libmachine can be used in external applications


## 0.5.5 (2015-12-28)

{% comment %}
### General
{% endcomment %}
{: #general-16 }
### 全般

{% comment %}
{% endcomment %}
- `env`
  - Better error message if swarm is down
  - Add quotes to command if there are spaces in the path
  - Fix Powershell env hints
  - Default to cmd shell on windows
  - Detect fish shell
- `scp`
  - Ignore empty ssh key
- `stop`, `start`, `kill`
  - Add feedback to the user
- `rm`
  - Now works when `config.json` is not found
- `ssh`
  - Disable ControlPath
  - Log which SSH client is used
- `ls`
  - Listing is now faster by reducing calls to the driver
  - Shows if the active machine is a swarm cluster

{% comment %}
{% endcomment %}
### Build

{% comment %}
{% endcomment %}
- Automate 90% of the release process
- Upgrade to Go 1.5.2
- Don't build 32bits binaries for Linux and OSX
- Prevent makefile from defaulting to using containers

{% comment %}
{% endcomment %}
### Misc

{% comment %}
{% endcomment %}
- Update docker-machine version
- Updated the bash completion with new options added
- Bugsnag: Retrieve windows version on non-English OS

{% comment %}
### Drivers
{% endcomment %}
{: #drivers-16 }
### ドライバー

{% comment %}
{% endcomment %}
- Amazon EC2
  - Convert API calls to official SDK
  - Make DeviceName configurable
- DigitalOcean
  - Custom SSH port support
- Generic
  - Don't support `kill` since `stop` is not supported
- Google
  - Coreos provisionning
- Hyper-V
  - Lot's of code simplifications
  - Pre-Check that the user is an Administrator
  - Pre-Check that the virtual switch exists
  - Add Environment variables for each flag
  - Fix how Powershell is detected
  - VSwitch name should be saved to config.json
  - Add a flag to set the CPU count
  - Close handle after copying boot2docker.iso into vm folder - will otherwise keep hyper-v from starting vm
  - Update Boot2Docker cache in PreCreateCheck phase
- OpenStack
 - Filter floating IPs by tenant ID
- VirtualBox
  - Reject duplicate hostonlyifs Name/IP with clear message
  - Detect when hostonlyif can't be created. Point to known working version of VirtualBox
  - Don't create the VM if no hardware virtualization is available and add a flag to force create
  - Add `VBox.log` to bugsnag crashreport
  - Update Boot2Docker cache in PreCreateCheck phase
  - Detect Incompatibility with Hyper-v
- VSphere
 - Rewrite driver to work with govmomi instead of wrapping govc
- All
  - Change host restart to use the driver implementation
  - Fix truncated logs
  - Increase heartbeat interval and timeout

{% comment %}
{% endcomment %}
### Provisioners

{% comment %}
{% endcomment %}
- Download latest Boot2Docker if it is out-of-date
- Add swarm config to coreos
- All provisioners now honor `engine-install-url`

## 0.5.4 (2015-12-28)

{% comment %}
{% endcomment %}
This is a patch release to fix a regression with STDOUT/STDERR behavior (#2587).

## 0.5.3 (2015-12-14)

{% comment %}
{% endcomment %}
With this release Machine reverts to distribution in a single binary, which is more efficient on bandwidth and hard disk space. All the core driver plugins are now included in the main binary. Delete the old driver binaries that you might have in your path.

```console
$ rm /usr/local/bin/docker-machine-driver-{amazonec2,azure,digitalocean,exoscale,generic,google,hyperv,none,openstack,rackspace,softlayer,virtualbox,vmwarefusion,vmwarevcloudair,vmwarevsphere}
```

{% comment %}
{% endcomment %}
Non-core driver plugins should still work as intended (in externally distributed binaries of the form `docker-machine-driver-name`.  Report any issues you encounter them with externally loaded plugins.

{% comment %}
### General
{% endcomment %}
{: #general-17 }
### 全般

{% comment %}
{% endcomment %}
- Optionally report crashes to Bugsnag to help us improve docker-machine
- Fix multiple nil dereferences in `docker-machine ls` command
- Improve the build and CI
- `docker-machine env` now supports emacs
- Run Swarm containers in provisioning step using Docker API instead of SSH/shell
- Show docker daemon version in `docker-machine ls`
- `docker-machine ls` can filter by engine label
- `docker-machine ls` filters are case insensitive
- `--timeout` flag for `docker-machine ls`
- Logs use `logrus` library
- Swarm container network is now `host`
- Added advertise flag to Swarm manager template
- Fix `help` flag for `docker-machine ssh`
- Add confirmation `-y` flag to `docker-machine rm`
- Fix `docker-machine config` for fish
- Embed all core drivers in `docker-machine` binary to reduce the bundle from 120M to 15M

{% comment %}
### Drivers
{% endcomment %}
{: #drivers-17 }
### ドライバー

{% comment %}
{% endcomment %}
- Generic
	- Support password protected ssh keys though ssh-agent
	- Support DNS names
- VirtualBox
	- Show a warning if virtualbox is too old
	- Recognize yet another Hardware Virtualization issue pattern
	- Fix Hardware Virtualization on Linux/AMD
	- Add the `--virtualbox-host-dns-resolver` flag
	- Allow virtualbox DNSProxy override
- Google
	- Open firewall port for Swarm when needed
- VMware Fusion
	- Explicitly set umask before invoking vmrun in vmwarefusion
	- Activate the plugin only on OSX
	- Add id/gid option to mount when using vmhgfs
	- Fix for vSphere driver boot2docker ISO issues
- DigitalOcean
	- Support for creating Droplets with Cloud-init User Data
- Openstack
	- Sanitize keynames by replacing dots with underscores
- All
	- Most base images are now set to `Ubuntu 15.10`
	- Fix compatibility with drivers developed with docker-machine 0.5.0
	- Better error report for broken/incompatible drivers
	- Don't break `config.json` configuration when the disk is full

{% comment %}
{% endcomment %}
### Provisioners

{% comment %}
{% endcomment %}
- Increase timeout for installing boot2docker
- Support `Ubuntu 15.10`

{% comment %}
{% endcomment %}
### Misc

{% comment %}
{% endcomment %}
- Improve the documentation
- Update known drivers list

## 0.5.2 (2015-11-30)

{% comment %}
### General
{% endcomment %}
{: #general-18 }
### 全般

{% comment %}
{% endcomment %}
-   Bash autocompletion and helpers fixed
-   Remove `RawDriver` from `config.json` - Driver parameters can now be edited
    directly again in this file.
-   Change fish `env` variable setting to be global
-   Add `docker-machine version` command
-   Move back to normal `codegangsta/cli` upstream
-   `--tls-san` flag for extra SANs

{% comment %}
### Drivers
{% endcomment %}
{: #drivers-18 }
### ドライバー

{% comment %}
{% endcomment %}
-   Fix `GetURL` IPv6 compatibility
-   Add documentation page for available 3rd party drivers
-   VirtualBox
    -   Support for shared folders and virtualization detection on Linux hosts
    -   Improved detection of invalid host-only interface settings
-   Google
    -   Update default images
-   VMware Fusion
    -   Add option to disable shared folder
-   Generic
    -   New environment variables for flags

{% comment %}
{% endcomment %}
### Provisioners

{% comment %}
{% endcomment %}
-   Support for Ubuntu >=15.04.  This means Ubuntu machines can be created which
    work with `overlay` driver of lib network.
-   Fix issue with current netstat / daemon availability checking

## 0.5.1 (2015-11-16)

{% comment %}
{% endcomment %}
-   Fixed boot2docker VM import regression
-   Fix regression breaking `docker-machine env -u` to unset environment variables
-   Enhanced virtualization capability detection and `VBoxManage` path detection
-   Properly lock VirtualBox access when running several commands concurrently
-   Allow plugins to write to STDOUT without `--debug` enabled
-   Fix Rackspace driver regression
-   Support colons in `docker-machine scp` filepaths
-   Pass environment variables for provisioned Engines to Swarm as well
-   Various enhancements around boot2docker ISO upgrade (progress bar, increased timeout)

## 0.5.0 (2015-11-1)

{% comment %}
### General
{% endcomment %}
{: #general-19 }
### 全般
    {% comment %}
    {% endcomment %}
    -   Add pluggable driver model
    -   Clean up code to be more modular and reusable in `libmachine`
    -   Add `--github-api-token` for situations where users are getting rate limited
        by GitHub attempting to get the current `boot2docker.iso` version
    -   Various enhancements around the Makefile and build toolchain (still an active WIP)
    -   Disable SSH multiplex explicitly in commands run with the "External" client
    -   Show "-" for "inactive" machines instead of nothing
    -   Make daemon status detection more robust
{% comment %}
{% endcomment %}
### Provisioners
    {% comment %}
    {% endcomment %}
    -   New CoreOS, SUSE, and Arch Linux provisioners
    -   Fixes around package installation / upgrade code on Debian and Ubuntu
{% comment %}
{% endcomment %}
### CLI
    {% comment %}
    {% endcomment %}
    -   Support for regular expression pattern matching and matching by names in `ls --filter`
    -   `--no-proxy` flag for `env` (sets `NO_PROXY` in addition to other environment variables)
{% comment %}
### Drivers
{% endcomment %}
{: #drivers-19 }
### ドライバー
    {% comment %}
    {% endcomment %}
    -   `openstack`
        -   `--openstack-ip-version` parameter
        -   `--openstack-active-timeout` parameter
    -   `google`
        -   fix destructive behavior of `start` / `stop`
    -   `hyperv`
        -   fix issues with PowerShell
    -   `vmwarefusion`
        -   some issues with shared folders fixed
        -   `--vmwarefusion-configdrive-url` option for configuration via `cloud-init`
    -   `amazonec2`
        -   `--amazonec2-use-private-address` option to use private networking
    -   `virtualbox`
        -   Enhancements around robustness of the created host-only network
        -   Fix IPv6 network mask prefix parsing
        -   `--virtualbox-no-share` option to disable the automatic home directory mount
        -   `--virtualbox-hostonly-nictype` and `--virtualbox-hostonly-nicpromisc` for controlling settings around the created hostonly NIC

## 0.4.1 (2015-08)

{% comment %}
{% endcomment %}
-   Fixes `upgrade` functionality on Debian based systems
-   Fixes `upgrade` functionality on Ubuntu based systems

## 0.4.0 (2015-08-11)

{% comment %}
{% endcomment %}
### Updates

{% comment %}
{% endcomment %}
-   HTTP Proxy support for Docker Engine
-   RedHat distros now use Docker Yum repositories
-   Ability to set environment variables in the Docker Engine
-   Internal libmachine updates for stability

{% comment %}
### Drivers
{% endcomment %}
{: #drivers-20 }
### ドライバー

{% comment %}
{% endcomment %}
-   Google:
    -   Preemptible instances
    -   Static IP support

{% comment %}
{% endcomment %}
### Fixes

{% comment %}
{% endcomment %}
-   Swarm Discovery Flag is verified
-   Timeout added to `ls` command to prevent hangups
-   SSH command failure now reports information about error
-   Configuration migration updates

## 0.3.0 (2015-06-18)

{% comment %}
{% endcomment %}
### Features

{% comment %}
{% endcomment %}
-   Engine option configuration (ability to configure all engine options)
-   Swarm option configuration (ability to configure all swarm options)
-   New Provisioning system to allow for greater flexibility and stability for installing and configuring Docker
-   New Provisioners
    -   Rancher OS
    -   RedHat Enterprise Linux 7.0+ (experimental)
    -   Fedora 21+ (experimental)
    -   Debian 8+ (experimental)
-   PowerShell support (configure Windows Docker CLI)
-   Command Prompt (cmd.exe) support (configure Windows Docker CLI)
-   Filter command help by driver
-   Ability to import Boot2Docker instances
-   Boot2Docker CLI migration guide (experimental)
-   Format option for `inspect` command
-   New logging output format to improve readability and display across platforms
-   Updated "active" machine concept - now is implicit according to `DOCKER_HOST` environment variable.  Note: this removes the implicit "active" machine and can no longer be specified with the `active` command.  You change the "active" host by using the `env` command instead.
-   Specify Swarm version (`--swarm-image` flag)

{% comment %}
### Drivers
{% endcomment %}
{: #drivers-21 }
### ドライバー

{% comment %}
{% endcomment %}
-   New: Exoscale Driver
-   New: Generic Driver (provision any host with supported base OS and SSH)
-   Amazon EC2
    -   SSH user is configurable
    -   Support for Spot instances
    -   Add option to use private address only
    -   Base AMI updated to 20150417
-   Google
    -   Support custom disk types
    -   Updated base image to v20150316
-   Openstack
    -   Support for Keystone v3 domains
-   Rackspace
    -   Misc fixes including environment variable for Flavor Id and stability
-   Softlayer
    -   Enable local disk as provisioning option
    -   Fixes for SSH access errors
    -   Fixed bug where public IP would always be returned when requesting private
    -   Add support for specifying public and private VLAN IDs
-   VirtualBox
    -   Use Intel network interface driver (adds great stability)
    -   Stability fixes for NAT access
    -   Use DNS pass through
    -   Default CPU to single core for improved performance
    -   Enable shared folder support for Windows hosts
-   VMware Fusion
    -   Boot2Docker ISO updated
    -   Shared folder support

{% comment %}
{% endcomment %}
### Fixes

{% comment %}
{% endcomment %}
-   Provisioning improvements to ensure Docker is available
-   SSH improvements for provisioning stability
-   Fixed SSH key generation bug on Windows
-   Help formatting for improved readability

{% comment %}
{% endcomment %}
### Breaking Changes

{% comment %}
{% endcomment %}
-   "Short-Form" name reference no longer supported Instead of "docker-machine " implying the active host you must now use docker-machine
-   VMware shared folders require Boot2Docker 1.7

{% comment %}
{% endcomment %}
### Special Thanks

{% comment %}
{% endcomment %}
We would like to thank all contributors.  Machine would not be where it is
without you.  We would also like to give special thanks to the following
contributors for outstanding contributions to the project:

{% comment %}
{% endcomment %}
-   @frapposelli for VMware updates and fixes
-   @hairyhenderson for several improvements to Softlayer driver, inspect formatting and lots of fixes
-   @ibuildthecloud for rancher os provisioning
-   @sthulb for portable SSH library
-   @vincentbernat for exoscale
-   @zchee for Amazon updates and great doc updates

## 0.2.0 (2015-04-16)

{% comment %}
{% endcomment %}
Core Stability and Driver Updates

{% comment %}
{% endcomment %}
### Core

{% comment %}
{% endcomment %}
-   Support for system proxy environment
-   New command to regenerate TLS certificates
    -   Note: this will restart the Docker engine to apply
-   Updates to driver operations (create, start, stop, etc) for better reliability
-   New internal `libmachine` package for internal api (not ready for public usage)
-   Updated Driver Interface
    -   [Driver Spec](https://github.com/docker/machine/blob/master/docs/DRIVER_SPEC.md)
    -   Removed host provisioning from Drivers to enable a more consistent install
    -   Removed SSH commands from each Driver for more consistent operations
-   Swarm: machine now uses Swarm default binpacking strategy

{% comment %}
{% endcomment %}
### Driver Updates

{% comment %}
{% endcomment %}
-   All drivers updated to new Driver interface
-   Amazon EC2
    -   Better checking for subnets on creation
    -   Support for using Private IPs in VPC
    -   Fixed bug with duplicate security group authorization with Swarm
    -   Support for IAM instance profile
    -   Fixed bug where IP was not properly detected upon stop
-   DigitalOcean
    -   IPv6 support
    -   Backup option
    -   Private Networking
-   Openstack / Rackspace
    -   Gophercloud updated to latest version
    -   New insecure flag to disable TLS (use with caution)
-   Google
    -   Google source image updated
    -   Ability to specify auth token via file
-   VMware Fusion
    -   Paravirtualized driver for disk (pvscsi)
    -   Enhanced paravirtualized NIC (vmxnet3)
    -   Power option updates
    -   SSH keys persistent across reboots
    -   Stop now gracefully stops VM
    -   vCPUs now match host CPUs
-   SoftLayer
    -   Fixed provision bug where `curl` was not present
-   VirtualBox
    -   Correct power operations with Saved VM state
    -   Fixed bug where image option was ignored

{% comment %}
{% endcomment %}
### CLI

{% comment %}
{% endcomment %}
-   Auto-regeneration of TLS certificates when TLS error is detected
    -   Note: this will restart the Docker engine to apply
-   Minor UI updates including improved sorting and updated command docs
-   Bug with `config` and `env` with spaces fixed
    -   Note: you now must use `eval $(docker-machine env machine)` to load environment settings
-   Updates to better support `fish` shell
-   Use `--tlsverify` for both `config` and `env` commands
-   Commands now use eval for better interoperability with shell

{% comment %}
{% endcomment %}
### Testing

{% comment %}
{% endcomment %}
-   New integration test framework (bats)

## 0.1.0 (2015-02-26)

{% comment %}
{% endcomment %}
Initial beta release.

{% comment %}
{% endcomment %}
-   Provision Docker Engines using multiple drivers
-   Provide light management for the machines
    -   Create, Start, Stop, Restart, Kill, Remove, SSH
-   Configure the Docker Engine for secure communication (TLS)
-   Easily switch target machine for fast configuration of Docker Engine client
-   Provision Swarm clusters (experimental)

{% comment %}
{% endcomment %}
### Included drivers

-   Amazon EC2
-   DigitalOcean
-   Google
-   Microsoft Azure
-   Microsoft Hyper-V
-   Openstack
-   Rackspace
-   VirtualBox
-   VMware Fusion
-   VMware vCloud Air
-   VMware vSphere
