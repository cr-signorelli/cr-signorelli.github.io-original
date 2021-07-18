---
title: "Solaris 10 - Procedimento básico de instalação"
classes: wide
permalink: /solaris-10-procedimento-basico-de-instalacao/
header:
  overlay_filter: 0.5
  overlay_image: https://source.unsplash.com/tL_IIeMc5Xo/1920x300
  og_image: https://source.unsplash.com/tL_IIeMc5Xo/120x120
  teaser: https://source.unsplash.com/tL_IIeMc5Xo/200x100
  image_description: "Fundo azulado abstrato."
  caption: "Imagem: [**Daniele Levis Pelusi**](https://unsplash.com/photos/tL_IIeMc5Xo)"
  #actions:
  #  - label: "Leia mais"
  #    url: "https://cr-signorelli.github.io/"
excerpt: Veja do as telas do processo de instalação em modo texto do Solaris 10 em um servidor Oracle da família T-Series.
categories:
  - solaris-10
tags:
  - solaris-10
  - sparc
  - sysadmin
  - instalacao
show_date: false
date: 2021-05-02T01:30:00-03:00
last_modified_at: 2021-05-02T01:30:00-03:00
---

Quando se trata de servidores é comum que a instalação dos sistemas operacionais sejam feitas em modo texto, remota ou através de ferramentas de automação. Por isso acredito que seja importante poder conhecer as telas de todo processo sempre que possível.
{: style="text-align: justify;"}

Especificações que vamos usar para o exemplo:
{: style="text-align: justify;"}

> vnet0 = network interface  
> 192.0.2.10/24 = ip address  
> 192.0.2.1 = default gateway  
> server01 = hostname

```console
{0} ok boot cdrom
Boot device: /virtual-devices@100/channel-devices@200/disk@1  File and args:
SunOS Release 5.10 Version Generic_147440-01 64-bit
Copyright (c) 1983, 2011, Oracle and/or its affiliates. All rights reserved.
Configuring devices.
Using RPC Bootparams for network configuration information.
Attempting to configure interface vnet0...
Skipped interface vnet0
Setting up Java. Please wait...
Serial console, reverting to text install
Beginning system identification...
Searching for configuration file(s)...
Search complete.
Discovering additional network configuration...
```

```console
Select a Language

   0. English
   1. Brazilian Portuguese
   2. French
   3. German
   4. Italian
   5. Japanese
   6. Korean
   7. Simplified Chinese
   8. Spanish
   9. Swedish
  10. Traditional Chinese

Please make a choice (0 - 10), or press h or ? for help: 0
```

```console
What type of terminal are you using?
 1) ANSI Standard CRT
 2) DEC VT52
 3) DEC VT100
 4) Heathkit 19
 5) Lear Siegler ADM31
 6) PC Console
 7) Sun Command Tool
 8) Sun Workstation
 9) Televideo 910
 10) Televideo 925
 11) Wyse Model 50
 12) X Terminal Emulator (xterms)
 13) CDE Terminal Emulator (dtterm)
 14) Other
Type the number of your choice and press Return: 3
Completing system identification...
in.rdisc: No interfaces up
```

Dependendo do teclado o F2 não funciona, não se preocupe utilize a sequência **ESC + 2**.

```console
# The Solaris Installation Program #############################################

  The Solaris installation program is divided into a series of short sections
  where you'll be prompted to provide information for the installation. At
  the end of each section, you'll be able to change the selections you've
  made before continuing.

  About navigation...
        - The mouse cannot be used
        - If your keyboard does not have function keys, or they do not
          respond, press ESC; the legend at the bottom of the screen
          will change to show the ESC keys to use for navigation.

################################################################################
    Esc-2_Continue    Esc-6_Help
```

```console
# Identify This System #########################################################

  On the next screens, you must identify this system as networked or
  non-networked, and set the default time zone and date/time.

  If this system is networked, the software will try to find the information
  it needs to identify your system; you will be prompted to supply any
  information it cannot find.

  > To begin identifying this system, press F2.

################################################################################
    Esc-2_Continue    Esc-6_Help
```

```console
# Network Connectivity #########################################################

  Specify Yes if the system is connected to the network by one of the Solaris
  or vendor network/communication Ethernet cards that are supported on the
  Solaris CD. See your hardware documentation for the current list of
  supported cards.
  Specify No if the system is connected to a network/communication card that
  is not supported on the Solaris CD, and follow the instructions listed under
  Help.


      Networked
      #########
      [X] Yes
      [ ] No

################################################################################
    Esc-2_Continue    Esc-6_Help
```

```console
# DHCP for vnet0 ###############################################################

  Specify whether or not this network interface should use DHCP to configure
  itself.  Choose Yes if DHCP is to be used, or No if the network interface is
  to be configured manually.

  NOTE: DHCP support will not be enabled, if selected, until after the system
  reboots.

      Use DHCP for vnet0
      ##################
      [ ] Yes
      [X] No

################################################################################
    Esc-2_Continue    Esc-6_Help
```

```console
# Host Name for vnet0 ##########################################################

  Enter the host name which identifies this system on the network.  The name
  must be unique within your domain; creating a duplicate host name will cause
  problems on the network after you install Solaris.

  A host name must have at least one character; it can contain letters,
  digits, and minus signs (-).


                 Host name for vnet0 server01

################################################################################
    Esc-2_Continue    Esc-6_Help
```

```console
# IP Address for vnet0 #########################################################

  Enter the Internet Protocol (IP) address for this network interface.  It
  must be unique and follow your site's address conventions, or a
  system/network failure could result.

  IP addresses contain four sets of numbers separated by periods (for example
  129.200.9.1).


                IP address for vnet0 192.0.2.10

################################################################################
    Esc-2_Continue    Esc-6_Help
```

```console
# Subnet for vnet0 #############################################################

  On this screen you must specify whether this system is part of a subnet.  If
  you specify incorrectly, the system will have problems communicating on the
  network after you reboot.

  > To make a selection, use the arrow keys to highlight the option and
    press Return to mark it [X].


      System part of a subnet
      #######################
      [X] Yes
      [ ] No

################################################################################
    Esc-2_Continue    Esc-6_Help
```

```console
# Netmask for vnet0 ############################################################

  On this screen you must specify the netmask of your subnet.  A default
  netmask is shown; do not accept the default unless you are sure it is
  correct for your subnet.  A netmask must contain four sets of numbers
  separated by periods (for example 255.255.255.0).


                   Netmask for vnet0 255.255.255.0

################################################################################
    Esc-2_Continue    Esc-6_Help
```

```console
# IPv6 for vnet0 ###############################################################

  Specify whether or not you want to enable IPv6, the next generation Internet
  Protocol, on this network interface.  Enabling IPv6 will have no effect if
  this machine is not on a network that provides IPv6 service.  IPv4 service
  will not be affected if IPv6 is enabled.

  > To make a selection, use the arrow keys to highlight the option and
    press Return to mark it [X].


      Enable IPv6 for vnet0
      #####################
      [ ] Yes
      [X] No


  Please wait...
################################################################################
    Esc-2_Continue    Esc-6_Help
```

```console
# Set the Default Route for vnet0 ##############################################

  To specify the default route, you can let the software try to detect one
  upon reboot, you can specify the IP address of the router, or you can choose
  None.  Choose None if you do not have a router on your subnet.

  > To make a selection, use the arrow keys to select your choice and press
  Return to mark it [X].


      Default Route for vnet0
      ##########################
      [ ] Detect one upon reboot
      [X] Specify one
      [ ] None

################################################################################
    Esc-2_Continue    Esc-6_Help
```

```console
# Default Route IP Address for vnet0 ###########################################

  Enter the IP address of the default route. This entry will be placed in the
  /etc/defaultrouter file and will be the default route after you reboot
  (example 129.146.89.225).



         Router IP Address for vnet0 192.0.2.1

################################################################################
    Esc-2_Continue    Esc-6_Help
```

```console
# Confirm Information for vnet0 ################################################

  > Confirm the following information.  If it is correct, press F2;
    to change any information, press F4.


                          Networked: Yes
                           Use DHCP: No
                          Host name: server01
                         IP address: 192.0.2.10
            System part of a subnet: Yes
                            Netmask: 255.255.255.0
                        Enable IPv6: No
                      Default Route: Specify one
                  Router IP Address: 192.0.2.1

################################################################################
    Esc-2_Continue    Esc-4_Change    Esc-6_Help
```

```console
# Configure Security Policy: ###################################################

  Specify Yes if the system will use the Kerberos security mechanism.

  Specify No if this system will use standard UNIX security.

      Configure Kerberos Security
      ###########################
      [ ] Yes
      [X] No

################################################################################
    Esc-2_Continue    Esc-6_Help
```

```console
# Confirm Information ##########################################################

  > Confirm the following information.  If it is correct, press F2;
    to change any information, press F4.


        Configure Kerberos Security: No

################################################################################
    Esc-2_Continue    Esc-4_Change    Esc-6_Help
```

```console
# Name Service #################################################################

  On this screen you must provide name service information.  Select the name
  service that will be used by this system, or None if your system will either
  not use a name service at all, or if it will use a name service not listed
  here.

  > To make a selection, use the arrow keys to highlight the option
    and press Return to mark it [X].


      Name service
      ############
      [ ] NIS+
      [ ] NIS
      [ ] DNS
      [ ] LDAP
      [X] None

################################################################################
    Esc-2_Continue    Esc-6_Help
```

```console
# Confirm Information ##########################################################

  > Confirm the following information.  If it is correct, press F2;
    to change any information, press F4.


                       Name service: None

################################################################################
    Esc-2_Continue    Esc-4_Change    Esc-6_Help
```

```console
# NFSv4 Domain Name ############################################################

  NFS version 4 uses a domain name that is automatically derived from the
  system's naming services. The derived domain name is sufficient for most
  configurations. In a few cases, mounts that cross domain boundaries might
  cause files to appear to be owned by "nobody" due to the lack of a common
  domain name.

  The current NFSv4 default domain is: ""


      NFSv4 Domain Configuration
      ##############################################
      [X] Use the NFSv4 domain derived by the system
      [ ] Specify a different NFSv4 domain

################################################################################
    Esc-2_Continue    Esc-6_Help
```

```console
# Confirm Information for NFSv4 Domain #########################################

  > Confirm the following information.  If it is correct, press F2;
    to change any information, press F4.


                 NFSv4 Domain Name:  << Value to be derived dynamically >>

################################################################################
    Esc-2_Continue    Esc-4_Change    Esc-6_Help
```

```console
# Time Zone ####################################################################

  On this screen you must specify your default time zone.  You can specify a
  time zone in three ways:  select one of the continents or oceans from the
  list, select other - offset from GMT, or other - specify time zone file.

  > To make a selection, use the arrow keys to highlight the option and
    press Return to mark it [X].

      Continents and Oceans
      ##################################
  -   [ ] Africa
  x   [X] Americas
  x   [ ] Antarctica
  x   [ ] Arctic Ocean
  x   [ ] Asia
  x   [ ] Atlantic Ocean
  x   [ ] Australia
  x   [ ] Europe
  v   [ ] Indian Ocean

################################################################################
    Esc-2_Continue    Esc-6_Help
```

```console
# Country or Region ############################################################

  > To make a selection, use the arrow keys to highlight the option and
    press Return to mark it [X].

      Countries and Regions
      ###########################
  ^   [ ] Antigua & Barbuda
  x   [ ] Argentina
  x   [ ] Aruba
  x   [ ] Bahamas
  x   [ ] Barbados
  x   [ ] Belize
  x   [ ] Bolivia
  x   [X] Brazil
  x   [ ] Canada
  x   [ ] Cayman Islands
  x   [ ] Chile
  x   [ ] Colombia
  v   [ ] Costa Rica

################################################################################
    Esc-2_Continue    Esc-6_Help
```

```console
# Time Zone ####################################################################

  > To make a selection, use the arrow keys to highlight the option and
    press Return to mark it [X].

      Time zones
      ######################################################
  ^   [ ] Pernambuco
  x   [ ] Tocantins
  x   [ ] Alagoas, Sergipe
  x   [ ] Bahia
  x   [X] S & SE Brazil (GO, DF, MG, ES, RJ, SP, PR, SC, RS)
  x   [ ] Mato Grosso do Sul
  x   [ ] Mato Grosso
  x   [ ] W Para
  x   [ ] Rondonia
  x   [ ] Roraima
  x   [ ] E Amazonas
  x   [ ] W Amazonas
  -   [ ] Acre

################################################################################
    Esc-2_Continue    Esc-6_Help
```

```console
# Date and Time ################################################################

  > Accept the default date and time or enter
    new values.

  Date and time: 2012-04-16 15:31

                 Year   (4 digits) : 2012
                 Month  (1-12)     : 04
                 Day    (1-31)     : 16
                 Hour   (0-23)     : 15
                 Minute (0-59)     : 31

################################################################################
    Esc-2_Continue    Esc-6_Help
```

```console
# Confirm Information ##########################################################

  > Confirm the following information.  If it is correct, press F2;
    to change any information, press F4.


                          Time zone: S & SE Brazil (GO, DF, MG, ES, RJ, SP, P>
                                     (Brazil/East)
                      Date and time: 2012-04-16 15:31:00

################################################################################
    Esc-2_Continue    Esc-4_Change    Esc-6_Help
```

```console
# Root Password ################################################################

  Please enter the root password for this system.

  The root password may contain alphanumeric and special characters.  For
  security, the password will not be displayed on the screen as you type it.

  > If you do not want a root password, leave both entries blank.


                     Root password:  *********
                     Root password:  *********

################################################################################
    Esc-2_Continue    Esc-6_Help
```

```console
# Enabling remote services #####################################################

  Would you like to enable network services for use by remote clients?

  Selecting "No" provides a more secure configuration in
  which Secure Shell is the only network service provided to
  remote clients.  Selecting "Yes" enables a larger set of
  services as in previous Solaris releases. If in doubt, it is
  safe to select "No" as any services can be individually enabled
  after installation.

  Note: This choice only affects initial installs. It doesn't affect upgrades.


      Remote services enabled
      #######################
      [ ] Yes
      [X] No


################################################################################
    Esc-2_Continue    Esc-6_Help
```

```console
# Provide Solaris Auto Registration Info: ######################################

  To improve products and services, Oracle Solaris communicates configuration
  data to Oracle after rebooting.

  You can register your version of Oracle Solaris to capture this data for
  your use, or the data is sent anonymously.

  For information about what configuration data is communicated and how to
  control this facility, see the Release Notes or
  www.oracle.com/goto/solarisautoreg.

  > Use the arrow keys to select the option and press Return to
     mark it [X].

      #################################################################
      [ ] I would like to register using My Oracle Support information.

################################################################################
    Esc-2_Continue    Esc-6_Help
```

```console
# Provide Solaris Auto Registration Info: ######################################

  To send the configuration data anonymously, complete the following fields.
  If using a proxy server, provide the proxy settings.

  For information about what configuration data is communicated and how to
  control this facility, see the Release Notes or
  www.oracle.com/goto/solarisautoreg.

             Proxy Server Host Name:
           Proxy Server Port Number:
               HTTP Proxy User Name:
                HTTP Proxy Password:

################################################################################
    Esc-2_Continue    Esc-6_Help
```

```console
System identification is completed.

System identification complete.
Starting Solaris installation program...
Executing JumpStart preinstall phase...
Searching for SolStart directory...
Checking rules.ok file...

Using begin script: install_begin
Using finish script: patch_finish
Executing SolStart preinstall phase...
Executing begin script "install_begin"...
Begin script install_begin execution completed.
```

```console
# Solaris Interactive Installation #############################################

  On the following screens, you can accept the defaults or you can customize
  how Solaris software will be installed by:

        - Selecting the type of Solaris software to install
        - Selecting disks to hold software you've selected
        - Selecting unbundled products to be installed with Solaris
        - Specifying how file systems are laid out on the disks

  After completing these tasks, a summary of your selections (called a
  profile) will be displayed.

  There are two ways to install your Solaris software:

   - "Standard" installs your system from a standard Solaris Distribution.
      Selecting "Standard" allows you to choose between initial install
      and upgrade, if your system is upgradable.
   - "Flash" installs your system from one or more Flash Archives.

################################################################################
     F2_Standard    F4_Flash    F5_Exit    F6_Help
```

```console
# Eject a CD/DVD Automatically? ################################################

  During the installation of Solaris software, you may be using one or more
  CDs/DVDs. You can choose to have the system eject each CD/DVD automatically
  after it is installed or you can choose to manually eject each CD/DVD.


            [ ] Automatically eject CD/DVD
            [X] Manually eject CD/DVD

################################################################################
     F2_Continue    F3_Go Back    F5_Exit
```

```console
# Reboot After Installation? ###################################################

  After Solaris software is installed, the system must be rebooted. You can
  choose to have the system automatically reboot, or you can choose to
  manually reboot the system if you want to run scripts or do other
  customizations before the reboot.  You can manually reboot a system by using
  the reboot(1M) command.


            [X] Auto Reboot
            [ ] Manual Reboot

################################################################################
     F2_Continue    F3_Go Back    F5_Exit
```

```console
# Initializing #################################################################

  The system is being initialized.

  Loading install media, please wait...
```

```console
# License ######################################################################

        You acknowledge that your use of this software is subject to (i) the
        license terms that you accepted when you obtained a right to use this
        software; or (ii) the license terms that you signed when you placed
        your software order with us; or (iii) the license terms included in
        hard copy form with the hardware that you acquired from us; or, if (i),
        (ii) or (iii) are not applicable, then, (iv) the Oracle Electronic
        Delivery Trial License Agreement (which you acknowledge that you have
        read and understand), available at edelivery.oracle.com. Note: Software
        downloaded for trial use or downloaded as replacement media may not be
        used to update any unsupported software.

################################################################################
     Esc-2_Accept License    F5_Exit
```

```console
# Select Geographic Regions ####################################################

  Select the geographic regions for which support should be installed.

  > [ ] Southern Africa
  > [ ] Australasia
  > [ ] Northern Africa
  > [ ] Middle East
  > [ ] Eastern Europe
  > [ ] Southern Europe
  V [/] South America
    [ ]     Argentina (ISO8859-1)
    [ ]     Bolivia (ISO8859-1)
    [X]     Brazil (ISO8859-1)
    [ ]     Chile (ISO8859-1)
    [ ]     Colombia (ISO8859-1)
    [ ]     Ecuador (ISO8859-1)
    [ ]     Paraguay (ISO8859-1)
    [ ]     Peru (ISO8859-1)
    [ ]     Spanish

   Locale is selected.  Press Return to deselect
################################################################################
     Esc-2_Continue    F3_Go Back    F5_Exit    F6_Help
```

```console
# Select System Locale #########################################################

  Select the initial locale to be used after the system has been installed.

    [X]     POSIX C ( C )
        South America
    [ ]     Brazil (UTF-8) ( pt_BR.UTF-8 )
    [ ]     Brazil (ISO8859-1) ( pt_BR.ISO8859-1 )

################################################################################
     Esc-2_Continue    F3_Go Back    F5_Exit    F6_Help
```

```console
# Additional Products ##########################################################

  To scan for additional products, select the location you wish to scan.
  Products found at the selected location that are in a Web Start Ready
  install form will be added to the Products list.

  Web Start Ready product scan location:

      [X]  None
      [ ]  CD/DVD
      [ ]  Network File System


   Please wait ...
################################################################################
     Esc-2_Continue    F3_Go Back    F5_Exit
```

```console
# Choose Filesystem Type #######################################################

  Select the filesystem to use for your Solaris installation


            [X] UFS
            [ ] ZFS

################################################################################
     Esc-2_Continue    F3_Go Back    F5_Exit    F6_Help
```

```console
# Select Software ##############################################################

  Select the Solaris software to install on the system.

  NOTE: After selecting a software group, you can add or remove software by
  customizing it. However, this requires understanding of software
  dependencies and how Solaris software is packaged.

      [ ]  Entire Distribution plus OEM support ....... 6880.00 MB
      [X]  Entire Distribution ........................ 6838.00 MB
      [ ]  Developer System Support ................... 6647.00 MB
      [ ]  End User System Support .................... 5392.00 MB
      [ ]  Core System Support ........................ 1545.00 MB
      [ ]  Reduced Networking Core System Support ..... 1487.00 MB

   Please wait ...
################################################################################
     Esc-2_Continue    F3_Go Back    F4_Customize    F5_Exit    F6_Help
```

```console
# Select Disks #################################################################

  On this screen you must select the disks for installing Solaris software.
  Start by looking at the Suggested Minimum field; this value is the
  approximate space needed to install the software you've selected. Keep
  selecting disks until the Total Selected value exceeds the Suggested Minimum
  value.
  NOTE: ** denotes current boot disk

  Disk Device                                              Available Space
  =============================================================================
  [X]    c0d0                                             127908 MB  (F4 to edit
)
                                     Total Selected: 127908 MB
                                  Suggested Minimum:   5377 MB

################################################################################
     Esc-2_Continue    F3_Go Back    F4_Edit    F5_Exit    F6_Help
```

```console
# Preserve Data? ###############################################################

  Do you want to preserve existing data? At least one of the disks you've
  selected for installing Solaris software has file systems or unnamed slices
  that you may want to save.

################################################################################
     Esc-2_Continue    F3_Go Back    F4_Preserve    F5_Exit    F6_Help
```

```console
# Automatically Layout File Systems? ###########################################

  Do you want to use auto-layout to automatically layout file systems?
  Manually laying out file systems requires advanced system administration
  skills.

################################################################################
     F2_Auto Layout    F3_Go Back    F4_Manual Layout    F5_Exit    F6_Help
```

```console
# File System and Disk Layout ##################################################

  The summary below is your current file system and disk layout, based on the
  information you've supplied.

  NOTE: If you choose to customize, you should understand file systems, their
  intended purpose on the disk, and how changing them may affect the operation
  of the system.

      File sys/Mnt point Disk/Slice                                       Size
      ========================================================================
      overlap            c0d0s2                                     127908  MB

################################################################################
     Esc-2_Continue    F3_Go Back    F4_Customize    F5_Exit    F6_Help
```

Para o layout do disco é possivel usar o padrão do sistema ou usar um customizado.

```console
# Customize Disk: c0d0 #########################################################

  Entry:                            Recommended:      MB     Minimum:      MB
================================================================================
  Slice  Mount Point                 Size (MB)
     0   /                                7020
     1   swap                            10008
     2   overlap                        127908
     3   /tmp                             2016
     4   /var                             9000
     5   /usr                             9000
     6   /opt                             5004
     7   /                               85860
================================================================================
                         Capacity:      127908 MB
                        Allocated:       44028 MB
                             Free:           0 MB

################################################################################
     F2_OK    Esc-4_Options    F5_Cancel    F6_Help
```

```console
# File System and Disk Layout ##################################################

  The summary below is your current file system and disk layout, based on the
  information you've supplied.

  NOTE: If you choose to customize, you should understand file systems, their
  intended purpose on the disk, and how changing them may affect the operation
  of the system.

      File sys/Mnt point Disk/Slice                                       Size
      ========================================================================
      /                  c0d0s0                                     7020    MB
      swap               c0d0s1                                     10008   MB
      overlap            c0d0s2                                     127908  MB
      /tmp               c0d0s3                                     2016    MB
      /var               c0d0s4                                     9000    MB
      /usr               c0d0s5                                     9000    MB
      /opt               c0d0s6                                     5004    MB
      /dados             c0d0s7                                     85860   MB

################################################################################
     Esc-2_Continue    F3_Go Back    F4_Customize    F5_Exit    F6_Help
```

```console
# Mount Remote File Systems? ###################################################

  Do you want to mount software from a remote file server? This may be
  necessary if you had to remove software because of disk space problems.

################################################################################
     Esc-2_Continue    F3_Go Back    F4_Remote Mounts    F5_Exit    F6_Help
```

```console
# Profile ######################################################################

  The information shown below is your profile for installing Solaris software.
  It reflects the choices you've made on previous screens.

  ============================================================================

 -              Installation Option: Initial
 x                      Boot Device: c0d0
 x            Root File System Type: UFS
 x                  Client Services: None
 x
 x                          Locales: Brazil (ISO8859-1)
 x                    System Locale: C ( C )
 x
 x                         Software: Solaris 10, Entire Distribution
 x
 x      File System and Disk Layout: /                   c0d0s0 7020 MB
 x                                   swap                c0d0s1 10008 MB
 x                                   /tmp                c0d0s3 2016 MB
 v                                   /var                c0d0s4 9000 MB

################################################################################
     Esc-2_Begin Installation    F4_Change    F5_Exit    F6_Help
```

```console
# Warning ######################################################################

        The following disk configuration condition(s) have been
        detected. Errors must be fixed to ensure a successful
        installation. Warnings can be ignored without causing the
        installation to fail.

        WARNING: CHANGING DEFAULT BOOT DEVICE
        You have either explicitly changed the default boot device, or
        accepted the default to "Reconfigure EEPROM". In either case,
        the system's EEPROM will be changed so it will always boot
        Solaris from the device that you've specified. If this is not
        what you had in mind, go back to the disk selection screens and
        change the "Reconfigure EEPROM" setting.

################################################################################
     Esc-2_OK    F5_Cancel
```

```console
Preparing system for Solaris install

Configuring disk (c0d0)
        - Creating Solaris disk label (VTOC)

Creating and checking file systems
        - Creating / (c0d0s0)
        - Creating /tmp (c0d0s3)
        - Creating /var (c0d0s4)
        - Creating /usr (c0d0s5)
        - Creating /opt (c0d0s6)
        - Creating /dados (c0d0s7)

Beginning Solaris software installation
                              00:00:00
```

```console
################################################################################

     Solaris Initial Install


            MBytes Installed:  4121.60
            MBytes Remaining:     0.00

                  Installing:

          |           |           |           |           |           |
          0          20          40          60          80         100

################################################################################
```

```console
Solaris 10 software installation succeeded

Customizing system files
        - Mount points table (/etc/vfstab)
        - Network host addresses (/etc/hosts)
        - Environment variables (/etc/default/init)

Cleaning devices

Customizing system devices
        - Physical devices (/devices)
        - Logical devices (/dev)

Installing boot information
        - Installing boot blocks (c0d0s0)
        - Installing boot blocks (/dev/rdsk/c0d0s0)
        - Updating system firmware for automatic rebooting
```
