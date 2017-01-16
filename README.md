#thold

The Cacti thold plugin is designed to be a fault management system driven by Cacti's graph information.  It provides the facility to inspect data in a Cacti graph and the underlying RRDfile, and generate alerts for management and operations personnel.  It provides  Email, Syslog, and SNMP Trap or Inform escalations.  In addition, it also can notify personnel of host status status changes through Email, Syslog, and both SNMP Traps and Informs.

##Installation
	
To install the plugin, simply copy the plugin_thold directory to Cacti's plugins directory and rename it to simply 'thold'.  Once this is complete, goto Cacti's Plugin Management section, and Install and Enable the plugin.  Once this is complete, you can grant users permission to view and create thresholds.

Once you have installed thold, you should verify that Email support if function in Cacti by going to Console -> Configuration -> Settings -> Mail/Reporting/DNS and test your mail settings to validate that users will receive notifications via email.

After you have completed that, you should goto the 'Thresholds' Settings tab, and become familiar with the settings.  From there, you can provide overall control of thold, and set defaults for things like Email bodies, weekend exemptions, alert log retention, logging, etc.

As with much of Cacti, settings should be documented in line with the actual setting.  If you find that any of these settings are ambiguous, please create a pull request with your change proposal.

##Usage

The Cacti 1.0 version of thold is designed to work with Device Templates.  Therefore, when you configure a Device Template, you can add default Thresold Templates to that Device Template and when a Device in Cacti is created with that Device Template, all the required Thresolds will be created automatically.  Of course, creating stand alone Threshold is still supported.

Also new in thold version 1.0 is the ability to create multiple Threshold per Data Source.  So, you can have a Baseline Threshold say measuring the rate of change of a file system, while at the same having a Hi/Low and Time Based thresholds to notify you of free space low type of events.

Most standalone Thresholds are created from the Graph Management interface in Cacti.  This process starts with first creating a Threshold Template for the specific Graph Template in question, and then from Graph Management selecting the Graphs that you wish to apply this Template to.  Then, from the Cacti Actions drop down, select 'Create Threshold from Template' and simply select your desiredd Threshold Template.  Though this method continues to work today, we believe that with the support of associating Threshold Templates with Device Templates, that this method will become less popular over time.

When creating your first Threshold using thold, you need to be first understand the Threshold Type.  They include: High / Low, Time Based, and Baseline Deviation.  The High / Low are the easiest to understand.  If the measured value falls either above or below the High / Low values, for the Min Trigger Duration specified in the High / Low section, it will trigger an alert.  In the Time Based Threshold type, the measured value must go above or below the High / Low values so many times in the measurement window, or the 'Time Period Length'.  Lastly, the Baseline Deviation provides a floating window in the 'Time reference in the past' to measure change.  If the change in the measured value either goes up or down by a certain value in that time period, an alert will be triggered.

The Re-Alert Cycle is how often you wish to re-inform either via Email, Syslog, or SNMP Trap or Inform if the Threshold has not resolved itself before then.

Thold has multiple Data Manipulation types, including: Exact Value, CDEF, Percentage, and RPN Expression.  The simplest form is the Exact Value data manipulation where thold simply takes the raw value collected from Cacti's Data Collector, and applies rules to it.  In the case of COUNTER type data, thold will convert that to a relative value automatically.  The CDEF data manipulation allows you to use some, but not all Cacti CDEF's and apply them to Graph Data.  The CDEF's that work, have to leverage one or more of the special types included in Cacti's CDEF implementation like 'CURRENT_DATASOURCE' to be relative.  The Percentage Data Manipulation requires you to select the 'primary' Data Source as the Numerator, and then when selecting 'Percentage', you will be able to select the Denominator of the percentage calculation.  The most involved Data Manipulation is the RPN Expression type.  This Data Manipulation type allows you to use RPN Expressions to determine the value to be evaluated.  It can include other Data Sources in the Cacti graph in addition to the selected Data Source.  It follows closely RRDtools RPN logic, and most RRDtool RPN functions are supported. 

Lastly, please note that their all several forks of the thold plugin available from different sources.  These forks of thold are not necessarily compatible with the current version of Cacti's thold plugin.  Please be aware of this when installing thold for the first time.

##Authors

The thold plugin has been developed for over a decade, and there are several contributors.  Chief amount them are Jimmy Conner, Larry Adams, and Andreas Braun.  We hope that version 1.0 and beyond are the most stable and robust versions of thold ever published.  We are always looking for new ideas.  So, this won't be the last release of thold, you can rest assured of that.

##ChangeLog

	--- 1.0 ---
		feature: Initial Support for Cacti 1.0
    feature: Multiple tholds per data source

	--- 0.6 ---
		feature: Reduce influence upon Cacti's poller runtime to a minimum by introducing a Thold daemon (also allows distribution)
		feature: Support of SNMP traps and informs ( requires the CACTI SNMPAgent plugin v0.2 or above )
		feature: CACTI-THOLD-MIB added. Yep, Thold got its own MIB. ;)
		Bug#0002371: Thold plugin - Undefined index: template in /var/www/html/plugins/thold/thold.php on line 273

	--- 0.5 ---
		feature: Allow threshold log retention to be configurable
		bug: Thold Suggested Names not applied in all cases
		bug: Use of Aggregate breaks Thold edit UI and new Tholds
		bug: #0002247 - SQL Injection in thold.php (must be authenticated first) Thanks Primož!!

	--- 0.4.9 ---
		feature: Allow HRULES based upon HI/LOW Value portions courtesy of Barnaby Puttick
		bug: Restoral Emails not working in all cases
		bug: When polling returns non-numeric data,  don't return false LOW Alerts
		bug: Fix time based Warnings
		bug: More issues with Realert for Time Based and Hi/Low
		bug: Be more specific about what 'Hosts:' means when generating stats

	--- 0.4.8 ---
		feature: Support for Ugroup Plugin
		bug: Speed of |query_*| replacements in RPN Expressions
		bug: Correct name space collision with Weathermap
		bug: <THRESHOLDNAME> replacement using Data Source name and not Threshold Name
		bug: Notification List Disassociate Global List not functional

	--- 0.4.7 ---
		feature: Add index to optimize page loads
		feature: Allow more hosts to be reported as down

	--- 0.4.6 ---
		Add Warning Support Curtesy (Thomas Urban)
		Improve display of Baseline Alerts
		Add Log Tab and new Events for Readablilty
		Allow a variable replacements in Threshold names
		Fix several GUI and polling issues
		Don't alert on blank data output
		Remove old ununsed variables
		Fix thold messages on restorals
		Reapply Suggested Name for Tholds
		Add Email Priority per 'dragossto'
		Add Email Notification Lists for Thresholds, Templates and Dead Hosts
		Add Ability to Disable Legacy Alerting
		Add Ability to use |ds:dsname| and |query_ifSpeed| in RPN Expression
		Add RPN Expressions 'AND' and 'OR'
		Add Support for Boost to Baseline Tholds
		Add Template Export/Import Functionality
		Add DSStats functionality to RPN Expressions save on Disk I/O

	--- 0.4.4 ---
		Fix emailing of alerts when PHP-GD is not available
		Add Debug logging
		Sort Threshold drop down list by Description
		Add missing column to upgrade script
		Update baseline description
		Multiple fixes posted by our hard working forum users!!!

	--- 0.4.3 ---
		Add support for maint plugin
		Fix to allow Add Wizard to show all datasources belonging to a graph (even when in separate data templates)
		Re-apply SQL speed up when polling
		Several fixes to Baselining
		Several fixes to CDEFs
		Add customizable subjects and message body for down host alerts

	--- 0.4.2 ---
		Fixed Cacti 0.8.7g compatibility
		Bug#0001753: Lotus Notes are unable to render inline png pictures
		Bug#0001810: Thold: RRDTool 1.4.x error while determining last RRD value
		Bug: Fix for compatibility with other plugins using datasource action hook
		Bug: Re-add syslog messages for down hosts
		Bug: Fixed a few minor issues
		Bug: Allow the use of query_XYZ in CDEFs
		Bug: Fix host status page to only allow users to see hosts they have access to
		Bug: Fix ru_nswap errors on Windows

	--- 0.4.1 ---
		Feature: Add thold statistics to settings table to allow graphing the results
		Bug:	 Return False from host status check function if it is disabled
		Bug:	 Speed up Datasource Query on Told Tab
		Bug:	 Fix CDEF usage
		Bug:	 Fix Thold Add Wizard Bug with IE6/7
		Bug:	 Fix HTTP_REFERER error
		Bug:	 Fix duplicate function names (when improperly installing plugin)

	--- 0.4.0 ---
		Bug:     Fix for multiple poller intervals, use RRD Step of Data Source instead of Polling Interval
		Bug:     Fix for down host alerting on disabled hosts
		Feature: Add filtering to listthold.php
		Feature: Use time periods instead of number of pollings when specifying Repeat Alerts and Fail Triggers
		Feature: Time Based Threshold Checking
		Feature: Percentage Calculation
		Feature: Add Threshold Sub Tabs with Both Threshold and Host Status
		Feature: Allow Naming of Threshold Templates and Thresholds
		Feature: Allow Thresholds to be added in mass via a Data Sources dropdown
		Feature: Allow Thresholds to be added in mass via a Graph Management dropdown
		Feature: Added Background Color Legend for Multiple Interfaces
		Feature: Add Threshold creation Wizard
		Feature: Make Wizard Design Consistent
		Feature: Add Filtering to User Threshold View and Host Status View
		Feature: Allow Disable/Enable/Edit/View Graph Actions from Main Page
		Feature: Allow Edit Host from Host Status
		Feature: Enable Toggle VRULE On/Off to show breached Thresholds on the graph images
		Feature: Allow Adding Thresholds from Graphs Page
		Feature: Use Cacti User Permissions when viewing and editing Thresholds
		Feature: Allow Weekend Exemptions per Threshold
		Feature: Allow the disabling of the Restoration Email per Threshold
		Feature: Allow logging of all Threshold Breaches to Cacti Log File
		Feature: Allow logging of all Threshold creations / changing / deletions to Cacti Log File
		Feature: Allow global disabling of all Thresholds
		Feature: Allow setting of Syslog Facility for Syslog Logging

	--- 0.3.9 ---
		Major poller speed increase when using large numbers of thresholds

	--- 0.3.8 ---
		Fix undefined variable error on thold.php

	--- 0.3.7 ---
		Fix issue with thold.php not correctly saving the host id
		Fix issue with Setting plugin having to be before thold in the plugins array

	--- 0.3.6 ---
		Compatible with Cacti v0.8.7 (not backwards compatible with previous versions)
		Fixed issue with saving user email addresses
		Fixed issue with tab images

	--- 0.3.5.2 ---
		Fix issues for users not using latest SVN of the Plugin Architecture

	--- 0.3.5.1 ---
		Fix for latest Cacti v0.8.6k SVN (requires latest SVN of Plugin Architecture)

	--- 0.3.5 ---
		Update plugin to use the Settings plugin for mail functionality
		Fix for thold values being off when using different polling intervals
		Use new "api_user_realm_auth" from Plugin Architecture
		Fix for creating multiple thresholds via templates from the same DataSource
		Fix for threshold template data propagating to an incorrect threshold
		Added Email Address field to User's Profiles
		Added ability to select a user to alert for a threshold instead of having to type in their email address
		Change to using the Settings plugin for mail functionality

	--- 0.3.4 ---
		Allow text only threshold alerts (aka no graph!)
		Add some text to the alerts, including the hostname
		Change the email to be sent as "Cacti" instead of PHPMailer
		Fix issue with host alerts still being sent as multipart messages
		Add the ability to completely customize the threshold alert (allow descriptors)
		Re-arrange the Settings page to group like options
		Fix an issue when applying thresholds to a device with no datasources / dataqueries
		Add the ability for template changes to propagate back to the thresholds (with the ability to disable per threshold)

	--- 0.3.3 ---
		#0000076 - Fix to speed up processing of thresholds (thanks mikv!)
		#0000079 - Bug causing thold to not respect the others plugins device page actions
		Fix an issue with re-alert set to 0 still alerting
		Fix the host down messages, this will work with cactid also
		Host Down messages are now sent as text only emails

	--- 0.3.2 ---
		Fix an index error message displayed when clicking the auto-creation link
		Fix an issue with thresholds not switching into "is still down" mode when alerting
		Fix a rare error where under certain conditions no data is passed back to threshold during polling

	--- 0.3.1 ---
		Patch from William Riley to allow the threshold management page to be split into separate pages
		Fix a php short tag issue on graph_thold.php
		Major rewrite of thold processing, now we pull from the poller output table instead of directly from the rrd files
		Major code cleanup in a few files
		Remove the tholdset table
		Remove the thold table
		Add an option for the priority level used when syslogging
		Add the option allow applying thresholds to multiple hosts at once through the Devices page
		#0000035 - Does not handle INDEXED data sources correctly
		#0000038 - Thresholding non-integer does not seem to work
		#0000041 - Subject of mail message now reflects the data source item (also #0000066)
		#0000059 - Thold always displays and assigns only one associated graph with the lowest graph_id
		#0000060 - Issue with "nan" values in the RRD File
		#0000062 - Step value of the rra is not considered for fetching rrd values
		#0000063 - CDEF function error (100 -DS)

	--- 0.3.0 ---
		#0000040 - Fix issue with invalid link in Navigation panel under certain circumstances
		#0000048 - Fix improper notification of global address when Threshold set to "Force: Off"
		#0000042 - Add ability to apply a CDEF to the threshold before using the data
		#0000054 - Fix issue with CDEFs on manual threshold creating page

	--- 0.2.9 ---
		#0000021 - Fix for rare SQL errors when auto-creating Thresholds when no Graph is associated with a Datasource
		#0000024 - Thold Templates not allowing for NULL Upper or Lower Baselines
		#0000031 - When creating Thresholds and Templates, default values were not provided
		#0000032 - Validation Error on listthold.php when selecting "Show All"
		Added some more POST validation to Threshold Templates
		Fix for Undefined offset in thold.php
		Changed the font size for the Auto-Create Thold Messages

	--- 0.2.8 ---
		#0000013 - Fix issues with database names with uncommon characters by enclosing in back-ticks.
		#0000030 - Allow use of decimal values in thresholds up to 4 decimal places
		#0000005 - Fix for threshold values not matching the graph values
		Change "Thresholds" to "Threshold Templates"

	--- 0.2.7 ---
		Fixes for "are you sure you meant month 899" errors
		Fixes for table tholdset being empty causes poller to not function
		Resolved issue with Base URL auto generation pointing to the plugin directory
		Code Cleanup of Threshold Management Page
		"Instructions" rewording on Threshold Management Page
		Can now select multiple Thresholds to delete
		Orphan thresholds are now cleaned up automatically
		Fixed Guest account access to View Thresholds

	--- 0.2.6 ---
		Fixes for HI and Low thresholds limiting the max characters
		Fixed wrong data reported to thold.log
		Fix for the error: "sh: line 1: -e: command not found" during thold checks
		Added command line switch /show for check-thold.php, which will show the output of all thresholds
		Added command line switch /debug to allow it to log to file (to make it permanent, just set debug=1 in the file)
		Fixed the Test Email link for IE

	--- 0.2.5 ---
		Test Link Created to help debug mail sending issues
		Several fixes to the Threshold Mailing (SMTP especially was broken)
		Several fixes to the Down Host Notification

	--- 0.2.4 ---
		Added Threshold Templates
		A few other minor interface fixes

	--- 0.2.3 ---
		 Emails now use embedded PNG images (instead of links)
		 Option to send mail via PHP Mail function, Sendmail, or SMTP (even authenicated)
		 Set the from email address and name
		 Fixed the Host Down Notification

	--- 0.2.0 ---
		 Auto-create the database if it doesn't exist
		 Better sorting on threshold tables
		 Does not require its own cron job anymore
		 Lots of bug fixes for issues in the original threshold module
