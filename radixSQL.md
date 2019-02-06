
# RADIX SETUP FILES & SQL

(c) Craig Duncan 2019

# Radix DB Intro

Radix works with the permissions for SQL server authority

The installation process will then run a number of scripts to create the database used by Radix DM.  Enter the details required as per the following screen below.  Note that if the "Windows Authentication" option is selected, then the current Windows user must have permissions in Microsoft SQL Server to create the Radix DM database.  If the "SQL Authentication" option is selected, then the user whose name was entered must have permissions in Microsoft SQL Server to create the Radix DM database.  If errors are generated during this process they will be displayed in dialogs that appear on screen with information about the error.

# Radix DM Program Folders

The Radix DM primary installation creates a number of folders at the network share location: 

`\Config`: 

This folder stores information about the Radix DM installation, including a connection string to the database that was set up as part of the installation procedure. 

`\DocumentQueues`: 

This is a folder used internally by Radix DM.

`\Documents`: 

This is a default folder set up as a base path to store all the documents saved into Radix DM.  This location can be changed.

`\Index`: 

Searches for text within documents require the creation and maintenance of text indices by the Radix DM Indexer service.  This folder is created as the default location for these indices.  This location can be changed.

`\Installations`: 

Other executable programs that can be installed as part of a complete product installation can be found here in their own subfolders.  These include the Radix DM Client and Radix DM Indexer programs.

`\Quarantine`: 

The Radix DM Crawler program can identify files located in the document store that should not be there.  By default, the Radix DM Crawler is configured to move these files to this location.  This location can be changed.  

# Radix Installation

`http://reference.radix.com.au/display/RDMDOC/Installing+the+Radix+DM+Server`

“The installation process will then run a number of scripts to create the database used by Radix DM.“

## Radix DB Storage location

Where is the radix database going to be stored?

	1.	Radix DM requires the Microsoft .Net Framework 4 or higher to run.  All machines that use Radix DM software must have this version or later installed, please check to see if it is installed.  The full package for this program is at: http://msdn.microsoft.com/en-us/library/5a4x27ek.  Other Windows Installer dependencies may also need to be installed before this process can be completed.
	
	2.	Determine which machine will host the Microsoft SQL Server database that will be used to store Radix DM data.  This database must be Microsoft SQL Server 2005 or a later version.  The current free Express version of Microsoft SQL Server is available from: http://www.microsoft.com/express/Database/
	
	3.	Ensure that the Microsoft SQL Server user login for the Windows user who is installing Radix DM has permission to create a new database OR that a new dedicated user has been created for use with Radix DM that has this permission.
	
	4.	Determine the location of the shared folder where the Radix DM primary installation will be set up.  This network share is where configuration and installation files will be stored.  It is also the default location of all document files stored in Radix DM (although this location can be changed in Radix DM Administration).  For the purposes of an example, we will refer to the following location: \\SharedNetworkLocation\Programs

## Database connectivity

Have a look in the Config folder listed above (in the main Radix directory)

### It should be clear to the organisation who the Windows user is.  This may or may not be the Radix Administrator.

### Tools

What tools are available for direct queries of the SQL server database used for Radix DM?

Nb SQL server may be installed as part of the Microsoft SQL server installation.  

Check:
`https://www.tutorialspoint.com/ms_sql_server/ms_sql_server_management_studio.htm`

Powershell, SQL server are prerequisites.

`https://en.wikipedia.org/wiki/SQL_Server_Management_Studio`
`https://en.wikipedia.org/wiki/`

Comparison_of_database_tools
`https://en.wikipedia.org/wiki/MySQL_Workbench`

## Windows File Manager

How is the folder system saved in Windows, independently of Radix DM profile DB?

{Each Library will have a folder in the location for the document database configured for the Radix DM system.  e.g. If the Library is ‘Invoices’ then its base path may be R:\RadixDocuments\Invoices}

“it is considered best practice for each library group's base path to be different.  In general, the recommended practice is to have one shared network folder that acts as a base directory, with each library group having an appropriately named subfolder beneath it.  In our tutorial, assuming that we are using a base shared directory called R:\RadixDocuments then we could expect to see the base path for the 'Business Plans' library group to be R:\RadixDocuments\BusinessPlans and the 'Correspondence' library group to have the base path R:\Documents\Correspondence.  This style of base path is appropriate for smaller library groups but it is easy to see that in time, such folders will become overly crowded with documents.  

It is possible to further distinguish groups of documents based on shared field values.  In our example, invoice documents could be collated based on their supplier. The base path for 'Invoices' would be expanded from R:\RadixDocuments\Invoices to include a supplier by incorporating the use of a field value in the folder path.  Only one field can be used in a base path; that field is selected in the same manner as fields are chosen for use in the library group, but by ticking the box in the 'Base Path' column.  Tick this column for the field 'Supplier', also include the fields 'Invoice Date', 'Amount', 'Responsible For' and 'Customer Name', then click Save.”

`http://reference.radix.com.au/display/RDMDOC/Step+11%3A+Administration+-+Base+Paths`

### Setup

Selecting the field to be used for categorising the Windows subfolder system:

e.g. If you have a reference Library, you may wish to subdivide by topic (rather than type of reference).

”…BUT BY TICKING THE BOX IN THE BASE PATH COLUMN”:
1. As an Administrator, go to the main Fields Menu or the main Library menu?
 2. To identify one of the fields in the library to be used to further classify documents in the library within folders in the Windows system (usually hidden from the user), :
Select one (not more?) of the category fields to be used as the basis for subfolders.
 The effect of this will be to include that field as the basis for distinguishing subfolders for the base path of the Library?  .  

In the Windows system, the folder names are based on Radix’s numbers given to those fields, rather than the field names that may exist in the main Radix database.

Although an Administrator may choose a particular field to determine base path folders, it seems Radix uses a number system to name the resulting subfolders.  The possible design is as follows :
1)  Radix assigns a number to each list item in the ‘list of values’ for the base path category field; and
2)  The list value numbers are used as the names of the sub-folders numbers in the main Document database location used for Radix DM.

## SQL fields & modification

What limitations exist in modifying SQL database fields?

SQL databases have limitations on the extent to which fields can be modified.

`https://msdn.microsoft.com/en-us/library/ms188617.aspx`

`https://msdn.microsoft.com/en-us/library/ms190259.aspx`

As Microsoft says:
“Modifying the data type of a column that already contains data can result in the permanent loss of data when the existing data is converted to the new type. In addition, code and applications that depend on the modified column may fail. These include queries, views, stored procedures, user-defined functions, and client applications. Note that these failures will cascade. For example, a stored procedure that calls a user-defined function that depends on the modified column may fail. Carefully consider any changes you want to make to a column before making it.”

## SQL Server Management Studio

You can modify the data type of a column in SQL Server 2016 by using SQL Server Management Studio or Transact-SQL.

`https://docs.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms`


## Modifying fields in Radix

To what extent can the Radix DM profiling and fields be configured without changing the database filenames and or/SQL database field set?

All Libraries

`\Documents`: 

This is a default folder set up as a base path to store all the documents saved into Radix DM.  This location can be changed.
Within this folder, the first level of subfolders will correspond to the names of the Libraries chosen by the user (e.g. ‘Precedents’, ‘Matters’).

However it is possible to create a new Library and set a base path to any folder, not merely the root level of \Documents.  This could still be a subfolder somewhere inside the \Documents subfolder system on Windows.  Radix does not seem to have a rule that Libraries must be at the first folder level in the \Documents folder, if that is the base path root.

## Library subfolders: 

### List of values for a ‘base path’ category field

Subfolders in the document folder specified for the Radix DM documents can correspond to possible values for a single category field in the Radix DM database.  I say correspond because they may not be named with the values of the chosen field.

For example, if a category field ‘CAT1’ is chosen as the base path field inside the Library, then its list of values becomes the basis for the subfolders in the Windows file system.   However, Radix may actually use numbers to identify the subfolder names.   It does not use the text descriptions used in the ‘list of values’.

Only one level of subfolders is permitted?

The use of numbers to identify values in a ‘list of values’ may be a general feature, or it may be that Radix DM only does this when those values are selected as a base path field.    This raises the question whether the descriptions for lists of values can be changed without requiring any change to the subfolder names in the Radix document location in the Windows file system.  It is also not clear what would happen if a value in a list of value was deleted - it would seem to affect the way in which subfolders were created in the document library.

### Fields/field types

Changes to the ‘data type’ of a field in Radix seem to be unavailable, in the same way that changes to the data type in an SQL database would be restricted to preserve data integrity.

Even if the field names in the SQL database itself cannot be altered(?), the field names displayed to the user can be modified by the Radix DM administrator, e.g. for display for the ‘search’ and ‘save functions’.  This allows some flexibility in how fields appear to the user, or for changes in field names to be made without altering the underlying database set up.

What about the names of fields whose ‘list of values’ are used for base paths in the database? (e.g. the “CAT1” field list of values).   Can this field name be renamed, or just changed in the way it displays to the user? Try altering the descriptions of fields in the “Fields” part of the Library function in Radix Administrator app.

### DOCID, filenames

The DOCID is associated with a document by Radix and is not generally user-customisable.   Document numbers in Radix DM are used for the filenames for Library documents, when saved in the Windows file system.    Any other filename would be removed/discarded when a document is brought into the Radix system: only document numbers would be expected to exist within the Radix DM database folders in the Windows file system.

If a number filename is given to a file within the Radix folder in the Windows file system, RADIX DM will assume that it is a document in the system (at least, it notifies of a conflict).   This allows a new file with the same number to reference existing profile information (which may be desired or undesired).  Note that Radix also has a quarantine function and folder, where files in the system that do not seem to belong are moved.

Even though there is a DOCID, it may be that Radix DM has an even more fundamental primary key reference for the SQL database which is independent of the filename/DocID.

### Document ‘titles’ in the profiles

The “title” of the document, for user purposes, is stored solely with the document profile and can be changed without altering the underlying filename itself.   This allows user flexibility and preserves the numbers as one of the the primary references in the Radix DM system.    

The DOCID numbers are usually less significant for the user.  If users start to rely on the Radix DM DOCID numbers, it limits some of the advantage of the system.

## Bulk rename or replacement of field contents

Is it possible to do a replace of all values within a particular field?  e.g. If a list of values item is going to be replaced?
Cf SQL functions that would normally allow this.
