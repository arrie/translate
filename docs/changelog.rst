
.. _changelog:
.. _important_changes:

Important Changes
*****************

The Translate Toolkit might have changed how it functions in certain cases.
This page lists what has changed, how it might affect you and how to work
around the change either to bring your files in line or to use the old
behaviour if required.

.. _changelog#1.10:

1.10
====

- The matching criterion when merging units can now be specified with the
  ``X-Merge-On`` header. Available values for this header are `location` and
  `id`. By default merges will be done by matching IDs. This supersedes the
  effects of the ``X-Accelerator`` header when merging and establishes an
  explicit way to set the desired matching criterion.


.. _changelog#mozilla_dtd_files_change:

Mozilla DTD files change
------------------------

We now preserve spaces in DTD files i.e.::

  <!ENTITY          some.label          "definition">

Will preserve the spaces around the entity name ``some.lable``

You probably want to run po2moz once to isolate the space changes from real
translations.

.. _changelog#1.6.0:

1.6.0
=====

.. _changelog#po_files_now_always_have_headers:

PO files now always have headers
--------------------------------
Generated PO files now always contain headers. This will mainly affect the
output of pofilter and pogrep. This should allow better interoperability with
gettext tools, and allowed for some improvement in the code.  You should still
be able to use headerless files in msgmerge, although it is recommended that PO
files are consistently handled with headers wherever possible.

.. _changelog#1.4.1:

1.4.1
=====

.. _changelog#csv_column_header_names:

CSV column header names
-----------------------

The names given to CSV column headers have been changed. Early releases of
:doc:`/commands/csv2po` would name the columns "comment,original,translation".
This was done mostly to make it easy for non-technical translators.  However,
comments in the command line help used terms like source and target.  This
release changes the column header names to "location,source,target", this
aligns with terms used throughout the toolkit.

If you have CSV file generated by older versions of the toolkit then a header
entry of "comment,original,translation" will be turned into a unit instead of
being ignored.  You can either change your CSV file to use the headers
"location,source,target" or delete the header row completely.  Once this is
done the files will work as expected.

.. _changelog#1.4.0:

1.4.0
=====

.. _changelog#java_and_mozilla_.properties:

Java and Mozilla .properties
----------------------------
Unusual keys, separators and spacing should all be handled correctly now. Some
Mozilla .properties files might now have changed. Regenerate your Mozilla l10n
files from fresh POT files without any changes to your PO files to ensure that
you can see and review these changes.

.. _changelog#hashing_in_podebug:

Hashing in podebug
------------------
The :opt:`--hash` option in :doc:`/commands/podebug` has been replaced by a
format specifier %h to be able to better control the positioning of the hash
value.

.. _changelog#1.3.0:

1.3.0
=====
Several duplicate styles were removed as has been warned about long before.
Please check the recommendations posted at the time that msgctxt was added on
how to migrate.

.. _changelog#1.2:

1.2
===

.. _changelog#new_formats:

New formats
-----------

The toolkit now supports:

* :doc:`/formats/qt_phrase_book`
* :doc:`/formats/ts` v1.1

This allows reading, counting and working on these formats.  The
:doc:`/commands/ts2po` converter has not been changed so you will not be able
to benefit from the new .ts support. However, you can use the format for
translation memory, etc as its is now fully base class compliant.

.. _changelog#stats_database_change:

Stats database change
---------------------
There were some changes in the database used by pocount for storing statistics.
The location of the database might also have changed, depending on what the
last version is that you used. Remove the file stats.db from any of
~/.translate_toolkit, ~/.wordforge (or the corresponding directories on your
Windows installation.

.. _changelog#valid_accelerators:

Valid accelerators
------------------

The :doc:`/commands/pofilter` accelerator test is now able to make use of a
list of valid accelerators.  This allows translators to control the behaviour
of the test for their language and add or remove characters that can be used as
accelerators.  Please define :doc:`l10n/valid accelerators` for your language
and these will then be included in future releases of the toolkit.  By default
the old process if followed so if you take no action then this check will
continue to work as expected.

.. _changelog#branches:

branches
========

These are branches that contain quite invasive changes that will most likely be
merged into the main development and be released sometime in the future.

.. _changelog#toolkit-c-po:

toolkit-C-po
------------

Converting the current Python based PO parser to the Gettext C based parser for
PO.  This offers quite a dramatic speed improvement and conformance to the
output found in Gettext itself.  For most users there will be a number of
changes in layout of the files as they will now conform fully to Gettext
layout.  The 'keep' option in :opt:`--duplicatestyle` will no longer be
supported as this is not valid Gettext output.

.. _changelog#1.1.1:

1.1.1
=====

.. _changelog#premature_termination_of_dtd_entities:

Premature termination of DTD entities
-------------------------------------

Although this does not occur frequently a case emerged where some DTD entities
where not fully extracted from the DTD source.  This was fixed in :bug:`331`.

We expect this change to create a few new fuzzy entries.  There is no action
required from the user as the next update of your PO files will bring the
correct text into your translations, if you are using a translation memory your
translation might be recovered from obsolete translations.

.. _changelog#1.1:

1.1
===

.. _changelog#oo2po_help_helpcontent2_escaping_fixed:

oo2po Help (helpcontent2) escaping fixed
----------------------------------------

OpenOffice.org Help (helpcontent2) has notoriously contained some unreadable
esacping, e.g. ``\\\\<tag attr=\\"value\\"\\\\>``.  The escaping has been fixed
and oo2po now understands helpcontent2 escaping while leaving the current GUI
escape handling unaltered.

If you have not translated helpcontent2 then you are unaffected by this change.
If you have translated this content then you will need to follow these
instructions when upgrading.

If you follow normal procedures of creating POT files and upgrading your PO
files using pot2po then your strings will not match and you will obtain files
with many fuzzies.  To avoid this do the following:

#. Make sure your PO files contain no fuzzy entries
#. Use po2oo from the previous release to create and SDF file
#. Upgrade to the latest Translate Toolkit with new po2oo
#. Use ``po2oo -l xx-YY your.sdf po`` to create a new set of PO files with
   correct escaping

You can choose to do this with only your helpcontent2 PO files if needed, this
will allow you to leave your GUI work in its current state.  Simply do the
above procedure and discard all PO files except helpcontent2, then move these
new helpcontent2 files into your current work.

.. _changelog#prop2po_uses_developer_comments:

prop2po uses developer comments
-------------------------------

prop2po used to place comments found in the source .properties file in
traditional translator comments, they should of course go into developer
comments.    The reason for this change is twofold, it allows these comments to
be correctly managed and it is part of the process of cleaning up these formats
so that they are closer to the base class and can thus work with XLIFF.

For the user there will be fairly large changes as one comment format moves to
the next.  It is best to :doc:`cleanup translator comments
</guides/cleanup_translator_comments>` and get your translations into a fit
state, i.e. no fuzzies, and then proceed with any migrations.

.. _changelog#moz2po_no_longer_uses_kde_comments:

moz2po no longer uses KDE comments
----------------------------------

moz2po has traditionally used KDE style comments for storing comments aimed at
translators.  Many translators confuse these and try to translate them.  Thus
these have been moved into automatic or developer comments.  The result for
many people migrating Mozilla PO files will be that many strings will become
fuzzy, you can avoid much of this by using pot2po which should intelligently be
able to match without considering the KDE comments.

The best strategy is to get your translations into a relatively good shape
before migration.  You can then migrate them first to a new set of POT files
generated from the same source files that the translation is based on.
Eliminate all fuzzies as these should only relate to the changes in layout.
Then proceed to migrate to a new set of POT files.  If you cannot work against
the original source files then the best would be to also first eliminate fuzzy
matches before proceeding to translation.  Your fuzzies will include changes in
layout and changes in content so proceed carefully.

At the end of this you should have PO files that conform to the Gettext
standard without KDE comments.

.. _changelog#read_and_write_mo_files:

Read and Write MO files
-----------------------

You can read and write Gettext MO files (compiled PO files).  Thus pocount can
now count files on your filesystem and you can also compile MO files using
pocompile.  MO files can be compiled from either PO or XLIFF sources.

MO will now also produce correct output for msgctxt and plural forms found in
PO files.

.. _changelog#read_qt_.qm_files:

Read Qt .qm files
-----------------

We can now read Qt .qm files, thus pocount can count the contents of compiled
files.  We cannot however write .qm files at this time.

.. _changelog#1.0.1:

1.0.1
=====

.. _changelog#pot2po_will_create_new_empty_po_files_if_needed:

pot2po will create new empty PO files if needed
-----------------------------------------------

From version 1.0.1, pot2po will create empty PO files corresponding to new POT
files that might have been introduced. If some new POT files are present in the
input to pot2po, you will see a new PO file appear in your output directory
that was not in your old PO files.  You will not lose any data but in the worst
case you will see new files on projects that you thought were fully translated.

.. _changelog#1.0:

1.0
===

.. _changelog#improved_xliff_support:

Improved XLIFF support
----------------------
Many toolkit tools that only worked with PO files before, can now also work
with XLIFF files. pogrep, pocount, pomerge, and pofilter all work with XLIFF,
for example.

.. _changelog#pretty_xml_output:

Pretty XML output
-----------------
All XML formats should now be more human readable, and the converters to Qt .ts
files should work correctly again.

.. _changelog#fuzzy_matching_in_pot2po_is_optional:

Fuzzy matching in pot2po is optional
------------------------------------
Fuzzy matching can now be entirely disabled in :doc:`/commands/pot2po` with the
:opt:`--nofuzzymatching` parameter. This should make it much faster, although
pot2po is **substantially** faster than earlier versions, especially if
:doc:`python-Levenshtein </commands/levenshtein_distance>` is installed.

.. _changelog#old_match/levenshtein.py*_can_cause_name_clash:

Old match/Levenshtein.py* can cause name clash
----------------------------------------------
The file previously called match/Levenshtein.py was renamed to lshtein.py in
order to use the python-Levenshtein package mentioned above. If you follow the
basic installation instructions, the old file will not be overwritten, and can
cause problems. Ensure that you remove all files starting with Levenshtein.py
in the installation path of the translate toolkit, usually something like
/usr/lib/python2.4/site-packages/translate/search/. It could be up to three
files.

.. _changelog#po_file_layout_now_follows_gettext_more_closely:

PO file layout now follows Gettext more closely
-----------------------------------------------

The toolkits output PO format should now resemble Gettext PO files more
closely.  Long lines are wrapped correctly, messages with long initial lines
will start with a 'msgid ""' entry.  The reason for this change is to ensure
that differences in files relate to content change not format change, no matter
what tool you use.

To understand the problem more clearly.  If a user creates POT files with e.g.
:doc:`/commands/oo2po`.  She then edits them in a PO editor or manipulate them
with the Gettext tools.  The layout of the file after manipulation was often
different from the original produced by the Toolkit.  Thus making it hard to
tell what where content changes as opposed to layout changes.

The changes will affect you as follows:

#. They will only impact you when using the Toolkit tools.
#. You manipulate your files with a tool that follows Gettext PO layout

   * your experience should now improve as the new PO files will align with
     your existing files
   * updates should now only include real content changes not layout changes

#. You manipulate your files using Toolkit related tools or manual editing

   * your files will go through a re-layout the first time you use any of the
     tools
   * subsequent usage should continue as normal
   * any manipulation using Gettext tools will leave your files correctly layed
     out.

Our suggestion is that if you are about to suffer a major reflow that your
initial merge contain only reflow and update changes.  Do content changes in
subsequent steps.  Once you have gone through the reflow you should see no
layout changes and only content changes.

.. _changelog#language_awareness:

Language awareness
------------------
The toolkit is gradually becoming more aware of the differences between
languages. Currently this mostly affects pofilter checks (and therefore also
Pootle) where tests involving punctuation and capitalisation will be more aware
of the differences between English and some other languages. Provisional
customisation for the following languages are in place and we will welcome more
work on the language module: Amharic, Arabic, Greek, Persian, French, Armenian,
Japanese,  Khmer, Vietnamese, all types of Chinese.

.. _changelog#new_pofilter_tests:_newlines_and_tabs:

New pofilter tests: newlines and tabs
-------------------------------------

The escapes test has been refined with two new tests, ``newlines`` and
``tabs``.  This makes identifying the errors easier and makes it easier to
control the results of the tests.  You shouldn't have to change your testing
behaviour in any way.

.. _changelog#merging_can_change_fuzzy_status:

Merging can change fuzzy status
-------------------------------

pomerge now handles fuzzy states::

  pomerge -t old -i merge -o new

Messages that are fuzzy in *merge* will now also be fuzzy in *new*.  Similarly
if a fuzzy state is present in *old* but removed in *merge* then the message in
*new* will not be fuzzy.

Previously no fuzzy states were changed during a merge.

.. _changelog#pofilter_will_make_mozilla_accelerators_a_serious_failure:

pofilter will make Mozilla accelerators a serious failure
---------------------------------------------------------

If you use :doc:`/commands/pofilter` with the :opt:`--mozilla` option then
accelerator failures will produce a serious filter error, i.e. the message will
be marked as ``fuzzy``.  This has been done because accelerator problems in
your translations have the potential to break Mozilla applications.

.. _changelog#po2prop_can_output_mozilla_or_java_style_properties:

po2prop can output Mozilla or Java style properties
---------------------------------------------------

We have added the :opt:`--personality` option to allow a user to select output
in either :opt:`java`, or :opt:`mozilla` style (Java property files use escaped
Unicode, while Mozilla uses actual Unicode characters).  This functionality was
always available but was not exposed to the user and we always defaulted to the
Mozilla style.

When using :doc:`po2moz </commands/moz2po>` the behaviour is not changed for
the user as the programs will ensure that the properties convertor uses Mozilla
style.

However, when using :doc:`po2prop </commands/prop2po>` the default style is now
``java``, thus if you are converting a single ``.properties`` file as part of a
Mozilla conversion you will need to add :opt:`--personality=mozilla` to your
conversion.  Thus::

  po2prop -t moz.properties moz.properties.po my-moz.properties

Would become::

  po2prop --personality=mozilla -t moz.properties moz.properties.po my-moz.properties

.. note:: Output in java style escaped Unicode will still be usable by Mozilla
   but will be harder to read.

.. _changelog#support_for_compressed_files:

Support for compressed files
----------------------------
There is some initial support for reading from and writing to compressed files.
Single files compressed with gzip or bzip2 compression is supported, but not
tarballs.  Most tools don't support it, but pocount and the :opt:`--tm`
parameter to pot2po will work with it, for example. Naturally it is slower than
working with uncompressed files. Hopefully more tools can support it in future.

.. _changelog#0.11:

0.11
====

.. _changelog#po2oo_defaults_to_not_check_for_errors:

po2oo defaults to not check for errors
--------------------------------------

In po2oo we made the default :opt:`--filteraction=none` i.e. do nothing and
don't warn.  Until we have a way of clearly marking false positives we'll have
to disable this functionality as there is no way to quiet the output or mark
non errors.  Also renamed exclude to exclude-all so that it is clearer what it
does i.e. it excludes 'all' vs excludes 'serious'.

.. _changelog#pofilter_xmltags_produces_less_false_positives:

pofilter xmltags produces less false positives
----------------------------------------------

In the xmltags check we handle the case where we had some false positives. E.g.
"<Error>" which looks like XML/HTML but should actually be translated. These
are handled by

#. identifying them as being the same length as the source text,
#. not containing any '=' sign.  Thus the following would not be detected by
   this hack. "An <Error> occurred" -> "<Error name="bob">", but these ones need
   human eyes anyway.

.. _changelog#0.10:

0.10
====

.. _changelog#po_to_xliff_conversion:

PO to XLIFF conversion
----------------------

Conversion from PO to XLIFF is greatly improved in 0.10 and this was done
according to the specification at
http://xliff-tools.freedesktop.org/wiki/Projects/XliffPoGuide -- please let us
know if there are features lacking.

.. _changelog#pot2po_can_replace_msgmerge:

pot2po can replace msgmerge
---------------------------

:doc:`/commands/pot2po` has undergone major changes which means that it now
respects your header entries, can resurrect obsolete messages, does fuzzy
matching using :doc:`Levenshtein distance </commands/levenshtein_distance>`
algorithm, will correctly match messages with KDE style comments and can use an
external Translation Memory.  You can now use pot2po instead of Gettext's
msgmerge and it can also replace :doc:`/commands/pomigrate2`.  You may still
want to use pomigrate2 if there where file movements between versions as pot2po
can still not do intelligent matching of PO and POT files, pomigrate2 has also
been adapted so that it can use pot2po as it background merging tool. ::

  pomigrate2 --use-compendium --pot2po <old> <pot> <new>

This will migrate file with a compendium built from PO files in *<old>* and
will use pot2po as its conversion engine.

.. _changelog#.properties_pretty_formatting:

.properties pretty formatting
-----------------------------

When using templates for generating translated .properties files we will now
preserve the formatting around the equal sign.

.. code-block:: properties

  # Previously if the template had
  property     =      value

.. code-block:: properties

  # We output
  property=translation

.. code-block:: properties

  # We will now output
  property     =      translation

This change ensures that there is less noise when checking differences against
the template file.  However, there will be quite a bit of noise when you make
your first .properties commits with the new pretty layout.  Our suggestion is
that you make a single commit of .properties files without changes of
translations to gt the formatting correct.

.. _changelog#0.9:

0.9
===

.. _changelog#escaping_-_dtd_files_are_no_longer_escaped:

Escaping -- DTD files are no longer escaped
-------------------------------------------

Previously each converter handled escaping, which made it a nightmare every
time we identified an escaping related error or added a new format.  Escaping
has now been moved into the format classes as much as possible, the result
being that formats exchange Python strings and manage their own escaping.

I doing this migration we revisited some of the format migration.  We found
that we were escaping elements in our output DTD files.  DTD's should have no
escaping i.e. ``\n`` is a literal ``\`` followed by an ``n`` not a newline.

A result of this change is that older PO files will have different escaping to
what po2moz will now expect. Probably resulting in bad output .dtd files.

We did not make this backward compatible as the fix is relatively simple and is
one you would have done for any migration of your PO files.

1. Create a new set of POT files ::

     moz2po -P mozilla pot

2. Migrate your old PO files ::

     pomigrate2 old new pot

3. Fix all the fuzzy translations by editing your PO files
4. Use pofilter to check for escaping problems and fix them ::

      pofilter -t escapes new new-check

5. Edit file in new-check in your PO editor ::

      pomerge -t new -i new-check -o new-check

.. _changelog#migration_to_base_class:

Migration to base class
-----------------------

All filters are/have been migrate to a base class.  This move is so that it is
easier to add new format, interchange formats and to create converters.  Thus
xx2po and xx2xlf become easier to create.  Also adding a new format should be
as simple as working towards the API exposed in the base class. An unexpected
side effect will be the Pootle should be able to work directly with any base
class file (although that will not be the normal Pootle operation)

We have checks in place to ensure the the current operation remains correct.
However, nothing is perfect and unfortunately the only way to really expose all
bugs is to release this software.

If you discover a bug please report it on Bugzilla or on the Pootle mailing
list.  If you have the skills please check on HEAD to see if it is not already
fixed and if you regard it as critical discuss on the mailing list backporting
the fix (note some fixes will not be backported because they may be too
invasive for the stable branch).  If you are a developer please write a test to
expose the bug and a fix if possible.

.. _changelog#duplicate_merging_in_po_files_-_merge_now_the_default:

Duplicate Merging in PO files -- merge now the default
------------------------------------------------------

We added the :opt:`--duplicatestyle` option to allow duplicate messages to be
merged, commented or simply appear in the PO unmerged.  Initially we used the
msgid_comments options as the default.  This adds a KDE style comment to all
affected messages which created a good balance allowing users to see duplicates
in the PO file but still create a valid PO file.

'msgid_comments' was the default for 0.8 (FIXME check), however it seemed to
create more confusion then it solved.  Thus we have reverted to using 'merge'
as the default (this then completely mimics Gettext behaviour).

As Gettext will soon introduce the msgctxt attribute we may revert to using
that to manage disambiguation messages instead of KDE comments.  This we feel
will put us back at a good balance of usefulness and usability.  We will only
release this when msgctxt version of the Gettext tools are released.

.. _changelog#.properties_files_no_longer_use_escaped_unicode:

.properties files no longer use escaped Unicode
-----------------------------------------------

The main use of the .properties converter class is to translate Mozilla files,
although .properties files are actually a Java standard.  The old Mozilla way,
and still the Java way, of working with .properties files is to escape any
Unicode characters using the ``\uNNNN`` convention.  Mozilla now allows you to
use Unicode in UTF-8 encoding for these files.  Thus in 0.9 of the Toolkit we
now output UTF-8 encoded properties files. :bug:`Bug 114 <114>` tracks the
status of this and we hope to add a feature to prop2po to restore the correct
Java convention as an option.

.. _changelog#0.8:

0.8
===
