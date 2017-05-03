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
This section specifies the new concepts introduced in this specification, note that all settings below will NOT be available if it's not applicable

The "source" file extension is rooted from GNU Bash, which means read the file and assign the variable defined in it.

It is recommended to fill all the variables applicable for your application, however missing variable assignment should be accepted as well with fallback results

### META_APPLICATION_IDENTIFIER
Application's identifier, application's name with limitation posed by the underlying platform

### META_APPLICATION_INSTALL_STYLE
This variable controls the above mentioned paradigms used in the application:

* STANDALONE for standalone paradigm
* SHC for S.H.C.
* FHS for F.H.S.

### Software Directories Configuration(S.D.C.)
This section defines and determines the directories referred by the software

#### SDC_EXECUTABLES_DIR
Directory to find executables

#### SDC_LIBRARIES_DIR
Directory to find libraries

#### SDC_SHARED_RES_DIR
Directory contain shared resources

#### SDC_I18N_DATA_DIR
Directory contain internationalization(I18N) data

#### SDC_SETTINGS_DIR
Direcotory contain software settings

#### SDC_TEMP_DIR
Directory contain temporory files created by program

### Runtime-determined Settings
The following variables defines the environment aspects that can only be detected in runtime, we use RUNTIME_ namespace for these variables.

These variables will not be set if technically not available(e.g. the program is provided to intepreter/etc. via stdin), or just not implemented yet

#### RUNTIME_EXECUTABLE_FILENAME
The running executable's filename(without the underlying path)

e.g. hello.executable

#### RUNTIME_EXECUTABLE_NAME
The running executable's filename(like `RUNTIME_EXECUTABLE_FILENAME`, but without the filename extension

e.g. hello

This variable is made so that developers will make sensible executable names, which can be reused in program if needed

#### RUNTIME_EXECUTABLE_DIRECTORY
The path of the directory that the executable reside in

e.g.

* /usr/bin
* /home/&lt;username&gt;/software/&lt;application name&gt;/bin
* or simply any path where the executable is located

#### RUNTIME_EXECUTABLE_PATH_ABSOLUTE
The executable's full path(absolute path)

Note that this variable will NOT follow symbolic links, which you have to use other method to know the application/package's directory, like assuming `/usr/share/&lt;package name>` if `RUNTIME_EXECUTABLE_PATH_ABSOLUTE` starts with `/usr` and vice-versa for `/usr/local`, or just hardcode the F.H.S. prefix during build time

#### RUNTIME_EXECUTABLE_PATH_RELATIVE
The executable's full path(but relative to the ${PWD})

#### RUNTIME_PATH_DIRECTORIES
Runtime environment's executable search path priority array

May not be implementable if the programming langauge doesn't support arrays

#### RUNTIME_COMMAND_BASE
The (probably guessed) base command that executes the program(not including commandline arguments)

This is handy when showing help, where the proper base command can be displayed

Example algorithm:

* If ${RUNTIME_EXECUTABLE_DIRECTORY} is in ${RUNTIME_PATH_DIRECTORIES}, this would be
	* ${RUNTIME_EXECUTABLE_FILENAME}
* if not this would be
	* ./${RUNTIME_EXECUTABLE_PATH_RELATIVE}

### PATH_TO_SOFTWARE_INSTALLATION_PREFIX_DIRECTORY.source(S.H.C. only)
This is the setting file that configures PATH_TO_SOFTWARE_INSTALLATION_PREFIX_DIRECTORY, the relative path fraction to the software's installation prefix directory, it should be placed with the same directory with the executable files, to determine the installation prefix from that location.

### SOFTWARE_INSTALLATION_PREFIX_DIRECTORY(S.H.C. only)
This variable specifies the installation prefix directory for S.H.C. paradigm, which may be one of the following:

* ${USERDIR}/Software/&lt;Application Name&gt;
	* USERDIR variable is a pseodo name for the user's home directory in different's OS's preference
* /opt/&lt;Application Name&gt;
* &lt;some random directory&gt;/&lt;Application Name&gt;

### FHS_PREFIX_DIR(F.H.S. only)
This variable specifies the installation prefix directory for F.H.S. paradigm, usually the following:

* /usr
* /usr/local

### SOFTWARE_DIRECTORY_CONFIGURATION.source(S.H.C. only)
This is the setting file that configures S.D.C., it should be placed to ${SOFTWARE_INSTALLATION_PREFIX_DIRECTORY}

### SOFTWARE_APPLICATION_METADATA.source
This is the configuration file that configures the metadata of the application

This file should be at the `${SOFTWARE_INSTALLATION_PREFIX_DIRECTORY}` for S.H.C paradigm and `${SDC_SHARED_RES_DIR}` for F.H.S. paradigm

This file may contains following values:

#### META_APPLICATION_NAME
The human-readable name of application

#### META_APPLICATION_DEVELOPER_NAME
Developers' name of application

#### META_APPLICATION_SITE_URL
Application's official site URL

#### META_APPLICATION_ISSUE_TRACKER_URL
Application's issue/bug tracker URL

#### META_APPLICATION_SEEKING_HELP_OPTION
An action to let user get help from developer or other sources when error occurred

Example:

* "contact developer"
* "go to &lt;support-url&gt;"
