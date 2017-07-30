### Note

This is just a simple script porting to Slackware (and/or maybe other UNIX-like OS).

### Install it on Slackware

just run:
	$ ./macchiato.slackbuild

### Run it manually

##### Usage 1: Apply configuration to all network interfaces:

        $ macchiato [<confdir>]

For each file inside confdir (or inside `$scriptDir/conf` if not provided), it will apply that configuration to 
the interface it is meant for. If `confdir` contains a file named `_default.sh`, this configuration will be appl
ied to all network interfaces which don't have a interface-specific configuration file. If there is no such file
, then no configuration will be applied to network interfaces which don't have a interface-specific configuratio
n file.
       
##### Usage 2: Apply configuration to selected network interfaces:
                                                                  
        $ macchiato [<confdir>] <interface1> [<interface2> [...]] 
                                                                 
`macchiato` will check for interface-specific configuration for each of the provided interfaces inside `confdir`
, or inside `$scriptDir/conf` if `confdir` is not provided. It will not affect any other interface.
                                                                                                   
##### Usage 3: Config-less usage                                                                   
                                
        $ macchiato --manual <interface>
                             [-o <class1> [-o <class2> [...]]]
                             [-b <blacklisted1> [-b <blacklisted2> [...]]]
                             [-e <ending>] [-r] [-d]
       
Manual mode allows you to run `macchiato` without having a config file. You must specify `--manual` as the first
 argument in order to use this. The next argument (`<interface>`) should be the name of the network interface to
 apply the rules to. Then, you can use the following:

* `-o <class>` or `--oui-class <class>`: Specifies a class of OUI prefixes to use for this interface. For exampl
e, if you specify `--oui-class wired_console`, then the OUIs defined in `$scriptDir/oui/wired_console.sh` will b
e added to the list of OUIs to consider. You can specify this multiple times to add other possible OUI classes. 
If no class is specified, the interface's OUI prefix will be preserved (but the ending will be changed).

* `-b <blaclistedOUI>` or `--blacklist <blaclistedOUI>`: Specifies single OUI that should never be used. You can
 specify this multiple times to blacklist multiple OUIs.
* `-e <ending>` or `--ending <ending>`: Specifies the last 3 bytes to use for the generated MAC address (example
: `dd:ee:ff`). If unspecified, these 3 bytes will be chosen randomly.
* `-r` or `--random`: If specified, macchiato will use `/dev/random` instead of `/dev/urandom` as a source of ra
ndomness. On Linux systems, this may block for some time until enough entropy is available, but provides higher-
quality randomness used when generating a MAC address. This option requires having `xxd` installed.
* `-d` or `--no-down`: If specified, macchiato will not bring the interface down before changing its MAC address
, and will not bring it back up again afterwards; it will just leave the interface in the state it already is.

### Environment variables

The following environment variables are reconized:

* `MACCHIATO_DEBUG`: If set, turns on Bash debugging.
* `MACCHIATO_IFCONFIG_COMMAND`: If set, `macchiato` will assume that `ifconfig` is installed and can be run by r
unning this command. This allows `macchiato` to be used on Android devices, where `ifconfig` may be provided by 
busybox and one needs to run `busybox ifconfig <arguments>`.

