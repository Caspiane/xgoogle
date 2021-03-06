xgoogle
=======

Python wrapper to Google Search service.

This is an command line search tool, which designed to fetch search results from search engine Google.

Provide a wrapper for the following services:
* Google Search
* Google Translate

Forked from [xgoogle](https://github.com/pkrumins/xgoogle)
It was written by Peteris Krumins <peter@catonmat.net>.
His blog is at http://www.catonmat.net  --  good coders code, great reuse.

Install
=======

1. Install requirements by: `pip install -r requirements.txt`.

   <sup>Note: Use `pip3` if required.</sup>

2. Run build by: `python setup.py build`.
3. Install by: `python setup.py install`.

   <sup>Note: Prefix by `sudo` if required.</sup>

Features
========

At the moment it contains:

 * Google Search module (xgoogle/search.py).

   http://www.catonmat.net/blog/python-library-for-google-search/

 * Google Sponsored Links Search module (xgoogle/sponsoredlinks.py)

   http://www.catonmat.net/blog/python-library-for-google-sponsored-links-search/

 * (deprecated) Google Sets module (xgoogle/googlesets.py)

   http://www.catonmat.net/blog/python-library-for-google-sets/
   
   Please note that Google Sets has been shut down since Sep 5, 2011

 * Google Translate module (xgoogle/translate.py)

   http://www.catonmat.net/blog/python-library-for-google-translate/

 * Google Real-Time Search module (realtime.py)
 * Google Image Search (check search.py)
 * Google Video Search (check search.py)

Disclaimer
==========

Before using, please read Google [Terms of Service](https://www.google.com/intl/en/policies/terms/)

> Don't misuse our Services.
> For example, don't interfere with our Services or
> try to access them using a method
> other than the interface and the instructions that we provide.

It is provided for personal study and research.

Usage
=====

Google Search
-------------


Here is an example usage of Google Search module:

    >>> from xgoogle.search import GoogleSearch
    >>> gs = GoogleSearch("catonmat")
    >>> gs.results_per_page = 25
    >>> results = gs.get_results()
    >>> for res in results:
    ...   print res.title.encode('utf8')
    ...

    output:

    good coders code, great reuse
    MIT's Introduction to Algorithms, Lectures 1 and 2: Analysis of ...
    catonmat - Google Code
    ...

The GoogleSearch object has several public methods and properties:

    method get_results() - gets a page of results, returning a list of SearchResult objects.
    property num_results - returns number of search results found.
    property results_per_page - sets/gets the number of results to get per page.
    property page - sets/gets the search page.

A SearchResult object has three attributes -- "title", "desc", and "url".
They are Unicode strings, so do a proper encoding before outputting them.

Google Sponsored Links Search module
------------------------------------

Note: Sponsored Links Search has been changed significantly, so the following example could not work anymore.

Here is an example usage of Google Sponsored Links Search module:

    >>> from xgoogle.sponsoredlinks import SponsoredLinks, SLError
    >>> sl = SponsoredLinks("video software")
    >>> sl.results_per_page = 100
    >>> results = sl.get_results()
    >>> for result in results:
    ...   print result.title.encode('utf8')
    ...

    output:

    Photoshop Video Software
    Video Poker Software
    DVD/Video Rental Software
    ...

The SponsoredLinks object has several public methods and properties:

    method get_results() - gets a page of results, returning a list of SearchResult objects.
    property num_results - returns number of search results found.
    property results_per_page - sets/gets the number of results to get per page.

A SponsoredLink object has four attributes -- "title", "desc", "url", and "display_url".
They are Unicode strings, don't forget to use a proper encoding before outputting them.

Google Sets module
------------------

Here is an example usage of Google Sets module:

    >>> from xgoogle.googlesets import GoogleSets
    >>> gs = GoogleSets(['red', 'yellow'])
    >>> results = gs.get_results()
    >>> print len(results)
    >>> for r in results:
    ...   print r.encode('utf8')
    ...

    output:

    red
    yellow
    blue
    white
    ...

The GoogleSets object has only get_results(set_type) public method. The default value
for set_type is SMALL_SET, which makes it return 15 related items or fewer.
Use LARGE_SET to get more than 15 items. This get_results() method returns a list of
related items that are represented as unicode strings.
Don't forget to do the proper encoding when outputting these strings!

Here is an example showing differences between SMALL_SET and LARGE_SET:

    >>> from xgoogle.googlesets import GoogleSets, LARGE_SET, SMALL_SET
    >>> gs = GoogleSets(['python', 'perl'])
    >>> results_small = gs.get_results() # SMALL_SET by default
    >>> len(results_small)
    11
    >>> results_small
    [u'python', u'perl', u'php', u'ruby', u'java', u'javascript', u'c++', u'c',
     u'cgi', u'tcl', u'c#']
    >>>
    >>> results_large = gs.get_results(LARGE_SET)
    >>> len(results_large)
    46
    >>> results_large
    [u'perl', u'python', u'java', u'c++', u'php', u'c', u'c#', u'javascript',
     u'howto', u'wiki', u'raid', u'dd', u'linux', u'ruby', u'language', u'xml',
     u'sgml', u'svn', u'kernel', ...]

Google Translate
----------------
Here is an example usage of Google Translate module:

    >>> from xgoogle.translate import Translator
    >>>
    >>> translate = Translator().translate
    >>> print translate("Mani sauc Pēteris", lang_to="ru").encode('utf-8')
    Меня зовут Петр
    >>> print translate("Mani sauc Pēteris", lang_to="en")
    My name is Peter
    >>> print translate("Меня зовут Петр")
    My name is Peter

The "translate" function takes three arguments - "message", "lang_from" and "lang_to".
If "lang_from" is not given, Google's translation service auto-detects it.
If "lang_to" is not given, it defaults to "en" (English).

In case of an error the "translate" function throws "TranslationError" exception.
Make sure to wrap your code in try/except block to catch it:

    >>> from xgoogle.translate import Translator, TranslationError
    >>>
    >>> try:
    >>>   translate = Translator().translate
    >>>   print translate("")
    >>> except TranslationError, e:
    >>>   print e

    Failed translating: invalid text


The Google Translate module also provides "LanguageDetector" class that can be used
to detect the language of the text.

Here is an example usage of LanguageDetector:

    >>> from xgoogle.translate import LanguageDetector, DetectionError
    >>>
    >>> detect = LanguageDetector().detect
    >>> english = detect("This is a wonderful library.")
    >>> english.lang_code
    'en'
    >>> english.lang
    'English'
    >>> english.confidence
    0.28078437000000001
    >>> english.is_reliable
    True

The "DetectionError" may get raised if the detection failed.

Google Image Search
-------------------

Please check example: examples/ImageExample.py

Google Video Search
-------------------

Please check example: examples/exampleVideoSearch.py

Requirements
============
Requires NLTK for Google video search, for install instruction see: http://www.nltk.org/install.html

Contributors:
=============
* kenorb (Python 3.x version, maintainance and bug fixes)
* Holger Berndt ('lang', 'tld' args, 'filetype' search, 'last_search_url' property, 'date indexed' search)
* Juanjo Conti (Google Blog Search class)
* Steve Steiner (setup.py)
* azappella (bug fixes)
* Nikola Milosevic (Google Face Image search)
* Ramon Xuriguera (Google Real-Time search)

License
=======
Licensed under MIT license.
