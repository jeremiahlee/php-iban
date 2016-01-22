php-iban
========

`php-iban` is a library for parsing and validating IBAN (and IIBAN) bank account information in PHP.

[![Build Status](https://travis-ci.org/globalcitizen/php-iban.png)](https://travis-ci.org/globalcitizen/php-iban)
[![Latest Stable Version](https://poser.pugx.org/globalcitizen/php-iban/v/stable)](https://packagist.org/packages/globalcitizen/php-iban) 
[![Latest Unstable Version](https://poser.pugx.org/globalcitizen/php-iban/v/unstable)](https://packagist.org/packages/globalcitizen/php-iban) 
[![License](https://poser.pugx.org/globalcitizen/php-iban/license)](https://packagist.org/packages/globalcitizen/php-iban)
![PHP Version](https://img.shields.io/badge/version-PHP%205.3%2B-lightgrey.svg)

All parts of an IBAN an be retrieved, including country code, checksum, BBAN, financial institution or bank code, and where a fixed-length national system is in use, also branch/sort code and account number. IBANs can also be converted between human and machine representation. Finally, intelligent suggestions for originally intended input can be made when an incorrect IBAN is detected and is due to mistranscription error.

Tested on PHP versions: 5.3, 5.4, 5.5, 5.6, 7.0, hhvm and nightly.

The parser was built using regular expressions to adapt the contents of the _official_ IBAN registry available from SWIFT at https://www.swift.com/node/11971 then only manually modified for special cases such as errors and omissions in SWIFT's official specifications: https://php-php-iban.googlecode.com/git/docs/COMEDY-OF-ERRORS 

Various deficiencies in the initial adaptation have since been rectified, and the current version should be a fairly correct and reliable implementation.

Where appropriate, __European Committee for Banking Standards__ (ECBS) recommendations have also been incorporated. (Presently, presentation is affected, however the uppercase only requirement is not enforced. In future, an uppercase-only 'strict ECBS checking' mode may be implemented.)

Please bear in mind that because the specification changes frequently, it may not be 100% up to date if a new version has been recently released - I do my best though.

Licensed under LGPL, it is free to use in commercial settings.


Installation via composer
-------------------------

If you are use [composer](https://getcomposer.org/) you can simply run `composer require globalcitizen/php-iban` to get going. Reportedly 
[![Monthly Downloads](https://poser.pugx.org/globalcitizen/php-iban/d/monthly)](https://packagist.org/packages/phpunit/phpunit) and [![Daily Downloads](https://poser.pugx.org/globalcitizen/php-iban/d/daily)](https://packagist.org/packages/phpunit/phpunit) were done via composer.

(If you don't yet have `composer` and wish to install it in an insecure fashion (not recommended, but convenient) you can run `curl -sS https://getcomposer.org/installer | php` or `wget -O- https://getcomposer.org/installer | php`)

Then just add the following to your `composer.json` file:

```js
// composer.json
{
    "require": {
        "globalcitizen/php-iban": "2.0.1"
    }
}
```

Then, you can install the new dependencies by running `composer`'s update command from the directory where your `composer.json` file is located:

```sh
# install
$ php composer.phar install
# update
$ php composer.phar update globalcitizen/php-iban

# or you can simply execute composer command if you set it to
# your PATH environment variable
$ composer install
$ composer update globalcitizen/php-iban
```

You can see this library on Packagist.

`composer` installs `autoloader` at `./vendor/autoload.php`. If you use globalcitizen/php-iban in your php script, add:

```php
require_once 'vendor/autoload.php';
```

Or you can use the `git clone` command:

```sh
# HTTP
$ git clone https://github.com/globalcitizen/php-iban.git
# SSH
$ git clone git@github.com:globalcitizen/php-iban.git
```

Comparison with other libraries
-------------------------------

The following table compares php-iban to other PHP projects offering IBAN-related functionality, on the basis of general project information and programming paradigms supported.

| Project                   | License       | Procedural? | Object-Oriented? | Began   | Latest  | ★   | 👀   | Forks | Installs   | Home culture    | Composer?  | Dependencies |
| ------------------------- | ------------- | ----------- | ---------------- | ------- | ------- | --- | --- | ----- | ---------- | --------------- | ---------- | ------------ |
| __php-iban__              | LGPL          | ✔           | ✔                | 2009    | 2.0.1   | 21  | 11  | 12    | 10,000+*   | International** | ✔          | *none*       |
| jschaedl/iban             | MIT           | ✘           | ✔                | 2013    | 1.1.6   | 38  | 10  | 14    | 52,521     | German          | ✔          | phpunit/phpunit, phpunit/php-code-coverage, phploc/phploc, sebastian/phpcpd, jschaedl/bav |
| ronanguilloux/IsoCodes    | GPL3          | ✘           | ✔                | 2012    | 2.0.0   | 241 | 14  | 28    | 36,360     | French          | ✔          | squizlabs/php_codesniffer, sebastian/phpcpd, phploc/phploc, phpmd/phpmd, pdepend/pdepend, fabpot/php-cs-fixer, symfony/phpunit-bridge, phpunit/phpunit, satooshi/php-coveralls, ext-bcmath |
| AbcAeffchen/SepaUtilities | GPL3          | ✘           | ✔                | 2014    | 1.1.2   | 4   | 3   | 3     | 1,394      | German          | ✔          | phpunit/phpunit |

* Note that original download records for php-iban releases were hosted on Google Code and now lost.
** Author is an Australian born, Australia/New Zealand/German citizen based long-term in mainland China, who has formerly also worked and banked in the US, UK, and many Asian countries.

Now let's take a look at features.

|                           | Generation | IIBAN | Mistranscription | Human? | Registry                                                               |
| ------------------------- | ---------- | ----- | ---------------- | ------ | ---------------------------------------------------------------------- |
| php-iban                  | ✔          | ✔     | ✔                | ✔      | [full, error-corrected CSV](https://github.com/globalcitizen/php-iban/blob/master/registry.txt) with [open-source toolchain](https://github.com/globalcitizen/php-iban/blob/master/utils/convert-registry.php) and [documentation](https://github.com/globalcitizen/php-iban/blob/master/docs/COMEDY-OF-ERRORS) | 
| jschaedl/iban             | ✔*         | ✘     | ✘                | ✘      | [partial, hardcoded, dubious origin](https://github.com/jschaedl/Iban/blob/master/library/IBAN/Core/Constants.php#L44) |
| ronanguilloux/IsoCodes    | ✘          | ✘     | ✘                | ✘      | [partial, hardcoded, dubious origin](https://github.com/ronanguilloux/IsoCodes/blob/master/src/IsoCodes/Iban.php#L25)  |
| AbcAeffchen/SepaUtilities | ✘          | ✘     | ✘                | ✘      | [partial, hardcoded, dubious origin](https://github.com/AbcAeffchen/SepaUtilities/blob/master/src/SepaUtilities.php#L89) |

* It is the author's opinion that generation features without IIBAN support are of dubious use, except in one-off migrations.

Note:
 * __IIBAN__ describes support for the open [proposal](http://www.ifex-project.org/) for decentralized financial endpoint generation by private parties, such as crypto-currency exchanges, whilst maintaining compatibility with the emerging IBAN system. This system has been adopted by major cryptocurrency exchanges such as [Kraken](https://www.kraken.com/).
 * __Mistranscription__ refers to the capacity to automatically detect what the user probably meant when they make a transcription error on IBANs, such as those manually written or printed in confusing fonts.
 * __Human?__ means support for input and output for the human, space-laden or presentation variant of an IBAN, ie. `IBAN XXXX XXXX XXXX XXXX` instead of `XXXXXXXXXXXXXXXX` - a lot more reasonable.

In short, while composer users have apparently lept on rival libraries due to the time it took us to integrate, those libraries are often burdensome in dependencies, less mature, fail to hat-tip to the free software foundation, do not support the procedural programming paradigm (for when AbstractProductClassMakerFactories just won't cut it), and are short on features.

So ... '''choose php-iban''': the ethical, functional, forward-looking, low-hassle library of choice for IBAN and IIBAN processing. '''Choose to win.'''


News: January 2016
------------------
__Version 2.0.1__ has been released. This is to celebrate real testing, composer support, as well as finally catching up with changes. This version should be up to date with all registry changes to the present, including changes or additions to the countries:
 * IIBAN (AA)
 * Brazil (BR)
 * Costa Rica (CR)
 * Kazakhstan (KZ)
 * Kosovo (XK)
 * Kuwait (KW)
 * Saint Barthelemy (BL)
 * Saint Lucia (LC) 
 * Saint Martin (French Part) (MF)
 * Sao Tome and Principe (ST)
 * Timor Leste (TL)
 * Turkey (TR)

__Version 1.6.0__ has been released. This version features more registry corrections (newly added territories with faulty data, bad checksums in sample IBANs, etc.) as well as enhanced testing routines, extended documentation, and corrected documentation. All users are advised to upgrade. We now have automated test script execution with Travis CI, to provide additional robustness for all committed code. This took longer than expected as unfortunately I picked the exact time Travis broke their build logs - https://www.traviscistatus.com/incidents/fcllblkclgmb - to see what all the fuss was about... proving again that cloud computing is just *great* for breaking things unexpectedly. Because they want to hide things, there was literally no debug output whatsoever, and I was led to believe this was my fault. Fellow programmers, behold: it is the dawning of the age of the mystical fail.

__Version 1.5.0__ has been released. There are no code changes, but we now have http://packagist.org/ integration, hopefully this triggers it to start working. If you use packagist, you can now add the library to your project by just running `composer require globalcitizen/php-iban` (thanks to @acoulton for pointing the way)

__Version 1.4.9__ has been released using the new Github-based release process. Hopefully this provides a solid anchor point for those bundling the library with other software. We also have a contributed composer metadata file to ease integration.
New IBAN registry URLs integrated.
Removed old SVN tag/trunk structure.

News: July 2015
---------------
Corrected SWIFT URL to IBAN page.
Emphasized mistranscription error support.

News: March 2015
----------------
Finally, google has killed `code.google.com` and we have migrated to Github! Once the old `trunk`/`tag` structure (lingering from `svn`) is cleaned up and this document translated from the old wiki format to markdown, a new version will be issued.

News: June 2014
---------------

__Version 1.4.6__ has been released:
 * Fixes for Jordan and Qatar. Turns out both of them have broken TXT registry entries, PDF entries differ and the PDF is the one to go for (familiar story).
 * Some further improvements.

Unfortunately, Google now requires `code.google.com` projects to use Google Drive. I tried to use Google Drive (sign up for a new account, jump through email hoops, get treated as a robot, learn stupid new touchy-feely-friendly interface, get meaningless error messages like 'Your sharing limit has been exceeded' (with 2x290KB files on a new account I was told to create) and lost patience entirely.

So for the moment, you'll just have to download using `git`, instead. I will migrate `php-iban` to Github shortly. Google really is a pain in the ass recently, what with all of this Google+ and Google Drive junk, ruining Picasa, ruining Sketchup by lack of attention, etc. What are they thinking?

News: March 2014
----------------
__Version 1.4.5__ has been released:
 * Addition of Jordan and Qatar
 * Minor changes to documentation and support scripts.

__Version 1.4.4__ has been released:
 * Fix SEPA status of Croatia (HR)
 * Subsqeuent SEPA status audit based upon https://en.wikipedia.org/wiki/Single_Euro_Payments_Area turned up some other status issues (this information is not contained within the official IBAN registry)
    * Faroe Islands, Greenland, San Marino status fixed. Everything else apparently hunky dory.

*The project source code repository has switched from `svn` (ugh) to `git` (yay!)*.
 * This should make future changes less painful.


News: September 2013
--------------------
__Version 1.4.3__ has been released:
 * Add Aland Islands (AX), part of Finland (FI) that is only documented in the SEPA status field of Finland and does not have its own entry or mention elsewhere in the IBAN registry document.
    * Consider but do not add either of the somewhat similar Canary Islands (CI) or Ceuta/Melilla (EA) - both minor territories of Spain (ES) - due to lack of any evidence of usage.
 * Fix SEPA status for Spain (ES), Finland (FI), Porgual (PT) due to registry values being mixed with free text.
    * Document this and further issues with the official IBAN registry document, both as documentation in `docs/COMEDY-OF-ERRORS` and inline within the registry converter.
 * Update human country name of Palestine to better mirror current registry document ("State of" is dropped as is the reigning style, so simply "Palestine" is presented)
 * Updating an outstanding last modified date within the registry from the previous release

News: August 2013
-----------------
__Version 1.4.2__ has been released:
 * Resolve issue #19: incorrect SEPA status of France/French territories due to a parser bug. (Thanks to the reporter)

__Version 1.4.1__ has been released:
 * Requests
    * Attempts to intelligently calculate the 'account' portion of a BBAN based upon the (non-)presence of a branch ID and bank ID portion, by request (for Germany/Austria. Previously this was requested for the Netherlands, however this solution should fix results for everyone!)
    * Add 'IIBAN' prefix removal support to machine format conversion function
    * Add _gmp_ disable flag (`$__disable_iiban_gmp_extension=true;`)
 * Silence warnings on some PHP engine configurations
 * Update Brazil record (minor)
 * No longer redistribute IBAN registry in .txt format
 * Improve inline documentation

News: June 2013
---------------
__Version 1.4.1__ is still being prepared, squashing some bugs and updating the registry ... meanwhile, it has come to my attention that we have been featured in the Code Candy blog!  http://www.codecandies.com/2012/05/30/no-exceptions/ Hooray for the German sense of humour! Hahah.

News: March 2013
----------------
__Version 1.4.0__ has been released:
 * Resolves an issue reported affecting the last few versions when attempting to generate a correct checksum for a checksum-invalid IBAN.
 * Adds `VERSION` file, to include hard version information in source tree, by request.

News: February 2013
-------------------
__Version 1.3.9__ has been released:
 * Resolves issue reported in 1.3.7 re-enables the more efficient PHP _gmp_ library based checksum code (thanks to rpkamp)

__Version 1.3.8__ has been released:
 * An error in checksum processing for some IBANs using the new _gmp_ library based MOD97 routine (_only affects users with php-iban 1.3.7 and the PHP _gmp_ library enabled_) has been reported. As an immediate workaround 1.3.8 is being released with the following changes:
 ** Code from 1.3.6
 ** Registry from 1.3.7

__Version 1.3.7__ has been released:
 * Added Brazil
 * Added two new French overseas territories
 * Reduced 'Moldova' to normalized short-form name
 * Large CPU efficiency improvement in IBAN validation routine (16x if PHP _gmp_ extension is installed, 5x otherwise. Special thanks to algorithmic contributor Chris and to engineers everywhere upholding the Germanic tradition of precision and efficiency! Alas, I am but part-German, for shame...)
 * Minor internal/tool updates
 * Some comedy of errors additions

News: November 2012
-------------------

__Version 1.3.6__ has been released:
 * Update IIBAN format for latest IETF draft.

News: October 2012
------------------

__Version 1.3.5__ has been released:
 * Correct lack of support for lower case alphabetic characters (ie. non ECBS-compliant) in human to machine format conversion function.

__Version 1.3.4__ has been released:
 * Add reference to the latest ECBS recommendations and include them in documentation.

__Version 1.3.3__ has been released:
 * Very minor efficiency improvement.

News: September 2012
--------------------

__Version 1.3.2__ has been released:
 * Registry updates
   * Added Palestinian Territories
   * Moldova fixed its format
   * Finland fixed its bank identifier location
   * Saudi Arabia - remove spurious trailing space in example

News: June 2012
---------------

__Version 1.3.1__ has been released:
 * New countries added
   * Azerbaijan (AZ)
   * Costa Rica (CR)
   * Guatemala (GT)
   * Moldova (MD)
   * Pakistan (PK)
   * British Virgin Islands (VG)
 * Miscellaneous updates
   * Normalize/simplify examples (FI,PT,SA)
   * Normalize/simplify human country name (BH,LI,MK)
   * Documentation updates

News: December 2011
-------------------
__Version 1.3.0__ has been released. This release adds mistranscription error suggestion support.

__Version 1.2.0__ has been released. This release adds Internet International Bank Account Number (IIBAN) support, as per the current IIBAN Internet Draft at http://tools.ietf.org/html/draft-iiban-01

News: September 2011
--------------------
__Version 1.1.2__ has been released. This adds long open tags to the main library file in order to simplify deployment on many default PHP installations.

News: August 2011
-----------------
__Version 1.1.1__ has been released. This fixes a typo in a function call in the new OO wrapper. Non OO users do not need to upgrade.

News: July 2011
---------------
__Version 1.1.0__ has been released. This version adds an object oriented wrapper library and related updates to documentation and test scripts. It is not critical for existing users to upgrade.

__Version 1.0.0__ has been released. This version includes the following changes:
 * *Support for the SEPA flag* ("Is this country a member of the Single Euro Payments Area?"), both in the registry and with a new function `iban_country_is_sepa($iban_country)`
 * *Placeholder support for converting machine format IBAN to human format* (simply adds a space every four characters) with the function `iban_to_human_format($iban)`
 * *Fixed a series of domestic example issues* in the registry file that had been imported from SWIFT's own broken IBAN registry
 * *Normalised example fields* in the registry to better facilitate use in automated contexts (Austria, Germany, etc.)
 * *Updated test code*
 * *Added a significant amount of new documentation*
 * *Reorganised file layout•
 * *Moved to _x.y.z_ format versioning and use of subversion 'tags'* in conjunction with the 1.0.0 release.

---

Earlier in the month... *Small maintenance release*, not critical.
 * The _split()_ function has been replaced with _explode()_ to prevent warnings (or error on _very_ new PHP engines)
 * Resolved an issue on PHP environments configured to display warnings would display a warning when an IBAN input to be validated did not include a prefix that was a valid IBAN country code. (Nobody should be running production PHP environments with such warnings enabled, anyway!) 

News: June 2011
---------------
 * We are now well over 1000 downloads: not bad considering how specific this project is!
 * If you are using `php-iban`, you are encouraged to join the [http://groups.google.com/group/php-iban-users mailing list]. This way you can get news of updates and announcements (very infrequent, but better than not knowing!)
 * A *new version* has been released that fixes many important changes to the official registry, plus adds some new features.
   * *Add New French Territories* (GF,GP,MQ,RE)
     Older versions of the specification did not include the GF,GP,MQ,RE French territories, only the PF,TF,YT,NC,PM,WF French territories. The new territories have now been added to the database.
   * *Add New Countries*
     We welcome Bahrain (BH), Dominican Republic (DO), Khazakstan (KZ), United Arab Emirates (AE) to the database.
   * *Format/example updates*
     There have apparently been some minor format/example changes, these have been rolled in to existing countries.
   * *Inclusion of altered IBAN_Registry.txt*
     Errors and omissions have been found within the official IBAN_Registry.txt file, namely the exclusion of Khazakstan (KZ) and only partial information on Kuwait (KW), and errors in both of these countries' PDF specifications. This is SWIFT's fault: shame on them! I suspect they have changed staff recently. Anyway, a version of IBAN_Registry.txt with these problems solved is now distributed along with php-iban.
   * *Fix for Tunisia*
     Strangely I visited Tunisia during the revolution in January this year. Sorry to the Tunisian people for getting their IBAN format wrong! TN59 + 20 digits is the correct format. This is now included in the new registry file.
   * *Fix for Albania*
     The SWIFT format information was updated for Albania. (Did not affect validation, since this uses regular expressions which were already correct)
   * *Additional and revised documentation*
     Further documentation has been added to the project.
   * *Automated IBAN_Registry.txt fix/conversion tool*
     A new _convert-registry_ tool has been added to the project that attempts to automatically normalise/fix problems with the official SWIFT .txt specification as much as possible. Note that this is not enough to get a good registry.txt file (the internal format used by php-iban) as SWIFT's .txt release excludes entire countries in the PDF specification. In addition, there are some errors in the PDF specification that need to be manually resolved at present. These can be seen resolved in the _IBAN_Registry.txt_ file.

News: December 2009
-------------------

*We now have a http://groups.google.com/group/php-iban-users mailing list.  Feel free to post your feedback, queries or suggestions - we'd love to know how you are using the library.  To date, the project has reached over 400 downloads and still going strong, with more than one new user per day - a pretty good showing for a specialised library!

*__version 12__ has been released.  The registry file has been improved, partly as a result of user reports and partly as a result of issues uncovered while performing automated tests against version 11.

 * *Corrected header row*
   Two columns were not represented in the title (`bban_length` and `iban_length`).  They have now been added.

 * *Fixes to registry entries for French Territories* (PF,TF,YT,NC,PM,WF)
   French territories are not explicitly included in the SWIFT specification textfile. 
   They were duplicated from France according to an unstructured comments against 
   that entry.  Example IBANs were then made for illustrative purposes by simply
   modifying the country prefix without regenerating the checksums.  The IBAN 
   examples included for these territories should now be correct.

 * *Gibraltar and Hungary* (GI,HU)
   Fixed a bug where both territories had a superfluous colon appended to their regular expressions after initial document conversion, which was causing validation failures for all IBANs in those countries.

 * *Mauritius* (MU)
   Corrected IBAN length expectation from 31 to 30.

 * *Sweden* (SE)
   Example IBAN had been manually modified from IBAN specification example early in development and did not pass checksum.  The IBAN official example has been restored.

 * *Tunisia* (TN)
   Corrected improper validation strings caused by a bug in initial document conversion (IBAN format-specifier to regular-expression conversion function).
 

Documentation (Procedural/Recommended)
======================================

```
require_once('php-iban.php');
# ... your code utilising IBAN functions...
```

Validation Functions
--------------------

```
# Verify an IBAN number.  Returns true or false.
#  Input can be either printed form ('IBAN xx xx xx...') or machine form ('xxxxx')
if(!verify_iban($iban)) {
 # ...
}

# Check the checksum of an IBAN - code modified from Validate_Finance PEAR class
if(!iban_verify_checksum($iban)) {
 # ...
}

# Suggest what the user really meant in the case of transcription errors
$suggestions = iban_mistranscription_suggestions($bad_iban);
if(count($suggestions) == 1) {
 print "You really meant " . $suggestions[0] . ", right?\n";
}

# Find the correct checksum for an IBAN
$correct_checksum = iban_find_checksum($iban);

# Set the correct checksum for an IBAN
$fixed_iban = iban_set_checksum($iban);
```

Utility Functions
-----------------

```
# Convert an IBAN to machine format.  To do this, we
# remove IBAN from the start, if present, and remove
# non basic roman letter / digit characters
$machine_iban = iban_to_machine_format($iban);

# Convert an IBAN to human format.  To do this, we
# add a space every four characters.
$human_iban = iban_to_human_format($iban);
```


IBAN Country-Level Functions
----------------------------
```
# Get the name of an IBAN country
$country_name = iban_country_get_country_name($iban_country);

# Get the domestic example for an IBAN country
$country_domestic_example = iban_country_get_domestic_example($iban_country);

# Get the BBAN example for an IBAN country
$country_bban_example = iban_country_get_bban_example($iban_country);

# Get the BBAN format (in SWIFT format) for an IBAN country
$country_bban_format_as_swift = iban_country_get_bban_format_swift($iban_country);

# Get the BBAN format (as a regular expression) for an IBAN country
$country_bban_format_as_regex = iban_country_get_bban_format_regex($iban_country);

# Get the BBAN length for an IBAN country
$country_bban_length = iban_country_get_bban_length($iban_country);

# Get the IBAN example for an IBAN country
$country_iban_example = iban_country_get_iban_example($iban_country);

# Get the IBAN length for an IBAN country
$country_iban_length = iban_country_get_iban_length($iban_country);

# Get the IBAN format (in SWIFT format) for an IBAN country
$country_iban_format_as_swift = iban_country_get_iban_format_swift ($iban_country);

# Get the IBAN format (as a regular expression) for an IBAN country
$country_iban_format_as_regex = iban_country_get_iban_format_regex($iban_country);

# Determine whether an IBAN country is a member of SEPA (Single Euro Payments Area)
if(!iban_country_is_sepa($iban_country)) {
 # ... do something xenophobic ...
}

# Get the bank ID start offset for an IBAN country
$country_bankid_start_offset = iban_country_get_bankid_start_offset($iban_country);

# Get the bank ID stop offset for an IBAN country
$country_bankid_stop_offset = iban_country_get_bankid_stop_offset($iban_country);

# Get the branch ID start offset for an IBAN country
$country_branchid_start_offset = iban_country_get_branchid_start_offset($iban_country);

# Get the branch ID stop offset for an IBAN country
$country_branchid_stop_offset = iban_country_get_branchid_stop_offset($iban_country);

# Get the registry edition for an IBAN country (note: IIBAN country 'AA' returns 'N/A')
$country_registry_edition = iban_country_get_registry_edition($iban_country);
```


Parsing Functions
-----------------
```
# Get an array of all the parts from an IBAN
$iban_parts = iban_get_parts($iban);

# Get the country part from an IBAN
$iban_country = iban_get_country_part($iban);

# Get the BBAN part from an IBAN
$bban = iban_get_bban_part($iban);

# Get the Bank ID (institution code) from an IBAN
$bank = iban_get_bank_part($iban);

# Get the Branch ID (sort code) from an IBAN
#  (NOTE: only available for some countries)
$sortcode = iban_get_branch_part($iban);

# Get the (branch-local) account ID from an IBAN
#  (NOTE: only available for some countries)
$account = iban_get_account_part($iban);

# Get the checksum part from an IBAN
$checksum = iban_get_checksum_part($iban);
```

Internal Functions
------------------

```
# Perform MOD97-10 checksum calculation
$mod97_10 = iban_mod97_10($string);

# Character substitution required for IBAN MOD97-10 checksum validation/generation
$mod97_internal_stuff = iban_checksum_string_replace($s);
```


Documentation (Object Oriented Wrapper/Discouraged)
===================================================

OO use is discouraged as there is a present-day trend to overuse the model.  However, if you prefer OO PHP then by all means use the object oriented wrapper, described below.
```
require_once('oophp-iban.php');
# ... your code utilising object oriented PHP IBAN functions...
```

Validation Functions
--------------------
```
# Example instantiation
$iban = 'AZ12345678901234'
$myIban = new IBAN($iban);

# Verify an IBAN number.  Returns true or false.
#  Input can be either printed form ('IBAN xx xx xx...') or machine form ('xxxxx')
if(!$myIban->Verify()) {
 # ...
}

# Check the checksum of an IBAN - code modified from Validate_Finance PEAR class
if(!$myIban->VerifyChecksum()) {
 # ...
}

# Suggest what the user really meant in the case of mistranscription errors
$suggestions = $badIban->MistranscriptionSuggestions();
if(count($suggestions)==1) {
 print "You really meant " . $suggestions[0] . ", right?\n";
}

# Find the correct checksum for an IBAN
$correct_checksum = $myIban->FindChecksum();

# Set the correct checksum for an IBAN
$fixed_iban = $myIban->SetChecksum()
```

Utility Functions
-----------------

```
# Convert an IBAN to machine format.  To do this, we
# remove IBAN from the start, if present, and remove
# non basic roman letter / digit characters
$machine_iban = $myIban->MachineFormat();

# Convert an IBAN to human format.  To do this, we
# add a space every four characters.
$human_iban = $myIban->HumanFormat();
```

IBAN Country-Level Functions
----------------------------

```
# To list countries, use the IBAN Class...
$myIban->Countries();

# ... everything else is in the IBANCountry class.

# Example instantiation
$countrycode = 'DE';
$myCountry = new IBANCountry($countrycode);

# Get the name of an IBAN country
$country_name = $myCountry->Name();

# Get the domestic example for an IBAN country
$country_domestic_example = $myCountry->DomesticExample();

# Get the BBAN example for an IBAN country
$country_bban_example = $myCountry->BBANExample();

# Get the BBAN format (in SWIFT format) for an IBAN country
$country_bban_format_as_swift = $myCountry->BBANFormatSWIFT();

# Get the BBAN format (as a regular expression) for an IBAN country
$country_bban_format_as_regex = $myCountry->BBANFormatRegex();

# Get the BBAN length for an IBAN country
$country_bban_length = $myCountry->BBANLength();

# Get the IBAN example for an IBAN country
$country_iban_example = $myCountry->IBANExample();

# Get the IBAN length for an IBAN country
$country_iban_length = $myCountry->IBANLength();

# Get the IBAN format (in SWIFT format) for an IBAN country
$country_iban_format_as_swift = $myCountry->IBANFormatSWIFT();

# Get the IBAN format (as a regular expression) for an IBAN country
$country_iban_format_as_regex = $myCountry->IBANFormatRegex();

# Determine whether an IBAN country is a member of SEPA (Single Euro Payments Area)
if(!$myCountry->IsSEPA()) {
 # ... do something xenophobic ...
}

# Get the bank ID start offset for an IBAN country
$country_bankid_start_offset = $myCountry->BankIDStartOffset();

# Get the bank ID stop offset for an IBAN country
$country_bankid_stop_offset = $myCountry->BankIDStopOffset();

# Get the branch ID start offset for an IBAN country
$country_branchid_start_offset = $myCountry->BranchIDStartOffset();

# Get the branch ID stop offset for an IBAN country
$country_branchid_stop_offset = $myCountry->BranchIDStopOffset();

# Get the registry edition for an IBAN country (note: IIBAN country 'AA' returns 'N/A')
$country_registry_edition = $myCountry->RegistryEdition();
```


Parsing Functions
-----------------

```
# Get an array of all the parts from an IBAN
$iban_parts = $myIban->Parts();

# Get the country part from an IBAN
$iban_country = $myIban->Country();

# Get the BBAN part from an IBAN
$bban = $myIban->BBAN();

# Get the Bank ID (institution code) from an IBAN
$bank = $myIban->Bank();

# Get the Branch ID (sort code) from an IBAN
#  (NOTE: only available for some countries)
$sortcode = $myIban->Branch();

# Get the (branch-local) account ID from an IBAN
#  (NOTE: only available for some countries)
$account = $myIban->Account();

# Get the checksum part from an IBAN
$checksum = $myIban->Checksum();

```
