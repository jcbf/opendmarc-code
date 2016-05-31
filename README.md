This is a mirror of the SourceForge git ( https://sourceforge.net/projects/opendmarc/ )
with Andreas Schulze's https://andreasschulze.de/dmarc/opendmarc and integration with  
Travis CI [![Build Status](https://travis-ci.org/jcbf/opendmarc-code.svg?branch=master)](https://travis-ci.org/jcbf/opendmarc-code)

================
OPENDMARC README
================

This directory has the latest open source DMARC software from The Trusted
Domain Project.

There is a web site at http://www.trusteddomain.org/opendmarc that is home for
the latest updates.


+--------------+
| INTRODUCTION |
+--------------+

The OpenDMARC project is a community effort to develop and maintain an open
source package for providing DMARC report generation and policy enforcement
services.  It includes a library for handling DMARC record parsing,
a database schema and tools for aggregating and processing transaction
history to produce DMARC reports, and a filter that ties it all together
with an MTA using the milter protocol.

"milter" is a portmanteau of "mail filter" and refers to a protocol and API
for communicating mail traffic information between MTAs and mail filtering
plug-in applications.  It was originally invented at Sendmail, Inc. but
has also been adapted to other MTAs.

+--------------+
| DEPENDENCIES |
+--------------+

To compile and operate, this package requires the following:

o OpenDBX (http://www.linuxnetworks.de/doc/index.php/OpenDBX) which
  acts as a middleware layer between OpenDMARC and your SQL backend
  of choice

o sendmail v8.13.0 (or later), or Postfix 2.3, (or later) and libmilter.
  (These are only required if you are building the filter.)

o Access to a working nameserver (required only for signature verification).

o If you are interested in tinkering with the build and packaging structure,
  you may need to upgrade to these versions of GNU's "autotools" components:
	autoconf (GNU Autoconf) 2.61
	automake (GNU automake) 1.7 (or 1.9 to avoid warnings)
	ltmain.sh (GNU libtool) 2.2.6 (or 1.5.26 after make maintainer-clean)


+-----------------------+
| RELATED DOCUMENTATION |
+-----------------------+

The man page for opendmarc (the actual filter program) is present in the
opendmarc directory of this source distribution.  There is additional
information in the INSTALL and FEATURES files, and in the README file in the
opendmarc directory.  Changes are documented in the RELEASE_NOTES file.

HTML-style documentation for libopendmarc is available in libopendmarc/docs in
this source distribution.

General information about DMARC can be found at http://www.dmarc.org

Mailing lists discussing and supporting the DMARC software found in this
package are maintained via a list server at trusteddomain.org.  Visit
http://www.trusteddomain.org to subscribe or browse archives.  The available
lists are:

	opendmarc-announce	(moderated) Release announcements.

	opendmarc-users		General OpenDMARC user questions and answers.

	opendmarc-dev		Chatter among OpenDMARC developers.

	opendmarc-code		Automated source code change announcements.

Bug tracking is done via the trackers on SourceForge at
http://sourceforge.net/projects/opendmarc.  You can enter new bug
reports there, but please check first for older bugs already open,
or even already closed, before opening a new issue.


+---------------------+
| DIRECTORY STRUCTURE |
+---------------------+

contrib		A collection of user contributed scripts that may be useful.

db		Database schema and tools for generating DMARC reports based
		upon accumulated data.

docs		A collection of RFCs and drafts related to opendmarc.

libopendmarc	A library that implements the proposed DMARC standard.

libopendmarc/docs
		HTML documentation describing the API provided by libopendmarc.

opendmarc	A milter-based filter application which uses libopendmarc (and
		optionally libar) to provide DMARC service via an MTA using
		the milter protocol.


+----------------+
| RUNTIME ISSUES |
+----------------+

WARNING: symbol 'X' not available

 The filter attempted to get some information from the MTA that the MTA
 did not provide.

 At various points in the interaction between the MTA and the filter, certain
 macros containing information about the job in progress or the connection
 being handled are passed from the MTA to the filter.

 In the case of sendmail, the names of the macros the MTA should pass to the
 filter are defined by the "Milter.macros" settings in sendmail.cf, e.g.
 "Milter.macros.connect", "Milter.macros.envfrom", etc.  This message
 indicates that the filter needed the contents of macro X, but that macro
 was not passed down from the MTA.

 Typically the values needed by this filter are passed from the MTA if the
 sendmail.cf was generated by the usual m4 method.  If you do not have
 those options defined in your sendmail.cf, make sure your M4 configuration
 files are current and rebuild your sendmail.cf to get appropriate lines
 added to your sendmail.cf, and then restart sendmail.

MTA timeouts

 By default, the MTA is configured to wait up to ten seconds for a response
 from a filter before giving up.  When querying remote nameservers
 for key and policy data, the DMARC filter may not get a response from the
 resolver within that time frame, and thus this MTA timeout will occur.
 This can cause messages to be rejected, temp-failed or delivered without
 verification, depending on the failure mode selected for the filter.

 When using the standard resolver library provided with your system, the
 DNS timeout cannot be adjusted.  If you encounter this problem, you must
 increase the time the MTA waits for replies.  See the documentation in
 the sendmail open source distribution (libmilter/README in particular)
 for instructions on changing these timeouts.

 When using the provided asynchronous resolver library, you can use the
 "-T" command line option to change the timeout so that it is shorter than
 the MTA timeout.

Other OpenDMARC issues:

 Report any bugs to the email address opendmarc-users@trusteddomain.org or to
 the SourceForge issue tracker accessible at:
 
 http://sourceforge.net/p/opendmarc/tickets/


--
Copyright (c) 2012, The Trusted Domain Project.  All rights reserved.

$Id: README,v 1.13 2010/10/25 20:41:55 cm-msk Exp $
