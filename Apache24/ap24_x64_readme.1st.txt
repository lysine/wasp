
	February 6, 2017
								Apache Haus Distribution 
						
	Application:		Mod subversion 1.9.5
	Distribution File: mod_svn-1.9.5-ap24-x64.zip
	sqlite-amalgamation 3081101
	OpenSSL 1.0.1
	zlib-1.2.11
	Serf 1.3.9

	Original Home:      http://subversion.apache.org/

	Win64 binary by: Mario
	Mail:            info@apachehaus.com 
	Home:            http://www.apachehaus.com 

	** This build for Apache 2.4.x only! **

	Requires at least 2.4.13

	Supported Windows Versions:
	Windows 7 64bit
	Windows Server 2008 64bit
	Windows Server 2008 R2
	Windows Server 2012
	Windows Server 2016 (no tested but assume to work)

	NOTES:

	The module is built with Visual Studio® 2012, be sure to install the 
	new Visual C++ 2012 Redistributable Package, download from;

	http://www.microsoft.com/downloads/en/details.aspx?FamilyID=BA9257CA-337F-4B40-8C14-157CFDFFEE4E
  


	INTRODUCTION 

	mod_subversion (mod_svn) supports a subversion repositority over apache with or without SSL. 
	Often useful for users behind a firewall with limited web access. 
	Also the apache users can be used for auth. 

	UPGRADING
	
	If you upgrade from a lower version like 1.7.x or 1.6.x it is highly recommended to dump your repos and set it up in the new format.
	a) More speed b) it might happen if you stay in the old format that the repo breakes.
	
	INSTALL

	Copy the folder bin and modules to your Apache 2.4.x folder
	.../apache24/

	Configuration and documentation:
	Can be found at http://svnbook.red-bean.com/

	STANDALONE INSTALLATION
	
	If you want to use only the svn command line tools without using apache you have to download aoache and copy
	libapr-1.dll, libapriconv-1.dll, libaprutil-1.dll, libeay32.dll, ssleay32.dll into the folder where svn bin folder.
 
	EXAMPLE CONFIG:

LoadModule dav_module modules/mod_dav.so
LoadModule dav_fs_module modules/mod_dav_fs.so

LoadModule auth_digest_module modules/mod_auth_digest.so
LoadModule dav_svn_module modules/mod_dav_svn.so
LoadModule authz_svn_module modules/mod_authz_svn.so

<Location /svn/>
  DAV svn

  SVNListParentPath on
  SVNParentPath /Repositories/
  SVNIndexXSLT "/svnindex.xsl"
  SVNPathAuthz on
  AuthzSVNAccessFile "C:/Repositories/authz"

  AuthName "Subversion Repositories"
  AuthType Basic
  AuthUserFile "C:/Repositories/htpasswd"

  require valid-user
</Location>


	mod_dontdothat is an Apache module that allows you to block specific types
	of Subversion requests.  Specifically, it's designed to keep users from doing
	things that are particularly hard on the server, like checking out the root
	of the tree, or the tags or branches directories.  It works by sticking an
	input filter in front of all REPORT requests and looking for dangerous types
	of requests.  If it finds any, it returns a 403 Forbidden error.

	It is enabled via single httpd.conf directive, DontDoThatConfigFile:

<Location /svn>
	DAV svn
	SVNParentPath /path/to/repositories
	DontDoThatConfigFile /path/to/config.file
	DontDoThatDisallowReplay off
</Location>

	The file you give to DontDoThatConfigFile is a Subversion configuration file
	that contains the following sections.

[recursive-actions]
/*/trunk = allow
/ = deny
/* = deny
/*/tags = deny
/*/branches = deny
/*/* = deny
/*/*/tags = deny
/*/*/branches = deny

	As you might guess, this defines a set of patterns that control what the
	user is not allowed to do.  Anything with a 'deny' after it is denied, and
	as a fallback mechanism anything with an 'allow' after it is special cased
	to be allowed, even if it matches something that is denied.

	Note that the wildcard portions of a rule only swallow a single directory,
	so /* will match /foo, but not /foo/bar.  They also must be at the end of
	a directory segment, so /foo* or /* are valid, but /*foo is not.

	These rules are applied to any recursive action, which basically means any
	Subversion command that goes through the update-report, like update, diff,
	checkout, merge, etc.

	The DontDoThatDisallowReplay option makes mod_dontdothat disallow
	replay requests, which is on by default.