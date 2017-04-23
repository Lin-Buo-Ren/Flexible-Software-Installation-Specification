# 彈性化軟體安裝規範<br>Flexible Software Installation Specification
This specification defines the configuration to let application/software's installation be flexible to multiple paradigms  
<https://github.com/Lin-Buo-Ren/Flexible-Software-Installation-Specification>

## 智慧財產授權條款<br>Intellectual Property License
Creative Commons BY-SA 4.0+

## Software Paradigms Introduction
### Standalone
This program doesn't refer any external directory, so no configuration is needed.

Example: A classic "Hello World" program

### Self-contained Hierarchy Configuration(S.H.C.)
This configuration meant that everything is put under software's specific prefix directory(e.g. `/home/user/Software/foo-software`)

### Filesystem Hierarchy Standard(F.H.S.)
Refer the spec. here: <http://refspecs.linuxfoundation.org>

This configuration assumes a *NIX root-directory tree structure exists and software files are split to specific directories(e.g. executable files are installed to `/usr/bin`, while resources are installed to `/usr/share/<package-name>` directory

## Introduced Concepts
This section specifies the new concepts introduced in this specification

The "source" file extension is rooted from GNU Bash, which means read the file and assign the variable defined in it.

### Software Directories Configuration(S.D.C.)
This section defines and determines the directories referred by the software

*These variables will NOT be available in STANDALONE paradigm*

#### SDC_EXECUTABLES_DIR
Directory to find executables(default: autodetermined, unset if not available)

#### SDC_LIBRARIES_DIR
Directory to find libraries(default: autodetermined, unset if not available)

#### SDC_SHARED_RES_DIR
Directory contain shared resources(default: autodetermined, unset if not available)

#### SDC_I18N_DATA_DIR
Directory contain internationalization(I18N) data(default: autodetermined, unset if not available)

#### SDC_SETTINGS_DIR
Direcotory contain software settings(default: autodetermined, unset if not available)

#### SDC_TEMP_DIR
Directory contain temporory files created by program(default: autodetermined, unset if not available)

### SOFTWARE_INSTALLATION_PREFIX_DIR.source
This is the setting file that configures SOFTWARE_INSTALLATION_PREFIX_DIR the relative path fraction to the software's installation prefix directory, it should be placed with executable files, to determine the prefix directory from that location.

*This configuration is currently only used in Self-contained Hierarchy Configuration(S.H.C.).*

### SOFTWARE_DIRECTORY_CONFIGURATION.source
This is the setting file that configures software directories referred in the program, it should be placed to ${SOFTWARE_INSTALLATION_PREFIX_DIR}

*This configuration is currently only used in Self-contained Hierarchy Configuration(S.H.C.).*

### FHS_PREFIX_DIR
This variable specifies the installation prefix directory for F.H.S. paradigm, usually the following:

* /usr
* /usr/local

### META_APPLICATION_IDENTIFIER
Application's identifier, application's name with limitation posed by other software, default(not implemented): unnamed-application
