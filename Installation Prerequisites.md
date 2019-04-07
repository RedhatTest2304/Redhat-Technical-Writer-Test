## Prerequisites for Installing an ownCloud Server  

The following requirements must be met to install and configure an ownCloud server:

Type | Specifications
----- | ---------------
Operating System	| Ubuntu 16.04 and 18.04 <br> Debian 7, 8, and 9 <br> SUSE Linux Enterprise Server 12 with SP1, SP2, and SP3 <br> Red Hat Enterprise Linux/Centos 6.9, 7.3, 7.4, and 7.5 <br> Fedora 27, 28, and 29 <br> openSUSE Leap 42.3 and 15
Database | MySQL or MariaDB 5.5+ <br> Oracle 11g <br> PostgreSQL 9 (versions 10 and above are not yet supported) <br> SQLite
Web server | Apache 2.4 with `prefork` and `mod_php`
PHP Runtime | 5.6, 7.0, 7.1, and 7.2
Server | Debian 7 and 8 <br> SUSE Linux Enterprise Server 12 and 12 SP1 <br> Red Hat Enterprise Linux/Centos 6.5 and 7 (7 is 64-bit only) <br> Ubuntu 14.04 LTS <br> NGINX with PHP-FPM (alternative but unsupported option)
Hypervisors | Hyper-V <br> VMware ESX <br>  Xen <br> KVM
Desktop | Windows 7+ <br> Mac OS X 10.7+ (64-bit only) <br> CentOS 6 and 7 (64-bit only) <br> Ubuntu 16.04, 18.04, and 18.10 <br> Debian 8.0 and 9.0 <br> Fedora 27, 28, and 29 <br> openSUSE Leap 42.3, 15.0, and 15.1
Mobile | iOS 9.0+ <br> Android 4.0+
Web Browser| Edge (current version on Windows 10) <br> IE11+ (except Compatibility Mode) <br> Firefox 57+ or 52 ESR <br> Chrome 66+ <br> Safari 10+
Client Versions | Desktop Client 2.3.3 <br> Android App <br> iOS App
Memory | Minimum of 512 MB  

>**Note**: If you use Ubuntu 16.04 and want to use PHP 7.x:  
>* PHP 7.1 and 7.2 are only available via ppa. To add a [PPA (Personal Package Archive)](https://itsfoss.com/ppa-guide/) to your system, use this command:`sudo add-aptrepository ppa:user/ppa-name`.    
>* PHP 7.2 standard installable, but you have to install some mandatory modules yourself, such as intl.  

>**Note**:
It is recommended to use **PHP {recommended-php-version}** as older versions have reached {php-supported-versions-url}[EOL] and will be
deprecated for use with ownCloud Server in a future release.    

>**Note**: Supported editions 
>* Red Hat Enterprise Linux & Centos 7 are 64-bit only.  
>* Oracle 11g is only supported for the Enterprise edition.  
>* SQLite is not encouraged for production use  

### Database Requirements  
The following are required for running ownCloud together with a MySQL or MariaDB database:  
* Disabled or `BINLOG_FORMAT = MIXED or BINLOG_FORMAT = ROW` configured Binary Logging (See: db-binlog-label)
* InnoDB storage engine (The MyISAM storage engine is not supported, see: dbstorage-engine-label)
* `READ COMMITED` transaction isolation level (See: db-transaction-label)

