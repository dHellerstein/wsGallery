23 Feb 2022

wsGallery:  -- quickly  list and view  images, and other files,  in a directory tree
 
  CAUTION: wsGallery can be setup to allow access to all the directories of an account.
           That is: any file on any directory accessible by php.
           Thus: if you do not control access to the directory and drive wsGallery.php is installed on
                       -- you should NOT install wsGallery on your webserver!

Features
  wsGallery is designed to display image files: 'png','gif','jpg','jpeg','bmp','xbm'.
  It can also display other files, including .mov, .mp4, .avi, .txt, .html, .pdf, .xls, and .ppt.
  The display of these other files may be browser dependent (some files, such as .avi, are 
  not supported by most browsers and depend on what viewers a client has installed).

  Image file display is well supported! You can easily pan within, and zoom into, an image file.
  And you can open a small version of the image to quickly navigate to a location in a larger
  version of the same image.

  On line help is available by clicking on the ? icons. 
  Use the "lantern" icon (at the top of each help topic window) to see an index of help topics (including 
  admin only ones).

Setup:

  wsGallery is part of the wsurvey. package, and uses several javascript and php libraries packaged with wsurvey.
  However, wsGalleryxxx.zip is shipped with the necessary libraries and can be used in a standalone mode.

Installation:

  The key steps: 
      * unzip wsGallery to its own directory. It will create a few directories.
      * modify  wsGallery_treeList.php, and wsurvey.adminLogon_params.php
      * create some directories under data/
      * ready to go (just add image and other files)! 
 	  

 1)  Unzip wsGalleryxxx (xxx a version code) into its own subdirectory in your web tree.
    * Several directories are created.
    * wsGallery.php is the front end to wsGallery. 

 2a) Set some "tree specifications" in wsGallery_treeList.php. 
 2b) Create some subdirectories under data/
    F or the details: see the 'Specifying data/ directories' and 'Specifying directory trees' sections below

 3) Edit wsurvey.adminLogon_params.php, and change the $passwordActual variable to a password of your choice.
    You can also change the $pwdDuration line.

   Note that logon is only required for administrators. Controlling access to clients can be accomplished
   through the usual server tools.

 4) Assuming wsGallery was unzipped to d:/www/gallery (and that d:/www is the root web directory),
   access wsGallery with something like http://mysite.org/gallery/wsGallery.php

 5) You will be asked to initialize a "directory list" for each "tree" you specified

 6) For optimal performance, such as creating a cache of thumbnails from images, you should initialize each of the directories
    in these trees. See the help notes on the pros and cons of doing this (much faster performance versus a need for disk space).

 7) You (and your public)  will then be able to view files -- images and others -- in the directories in the "trees" you specified.
    And if you are ambitious, you can add descriptions to these files!

 8) Ambitious administrators, who want to control wsGallery performance, can edit wsGallery_params.php. 
    See the descriptions in src/wsGallery_params_original for the details.
    In general, we don't recommend changing wsGallery_params.php.
        Note: the original version of wsGallery_params.php, and wsGallery_treeList.php, are
              stored in the src/ directory.   Use them if you mangle the working versions!


Specifying data/ directories

   wsGallery uses "caching" to expedite performance. This caching is done on a directory-in-a-tree basis.
   The caching requires the administrator to create subdirectories under the data/ directory.
   In particular, a directory for each tree specified in the wsGallery_treList.php.
   For security reasons, this is NOT automatically done by wsGallery -- an administrator must do this.

   However, all you have to do is create the directory. In particular: you do NOT have to create subdirectories for each directory in a tree.
   wsGallery will do that as needed; whein creating fileLists,  prebuilt thumbnails, etc. 

Specifying directory trees 

  The admin changeable parameters file, wsGallery_treList.php, must be modified by the administrator. 
   It is used to specify directory "trees" -- where a "tree" is a base directory. 
   This is often a directory under the sites's www home directory. But it can be any directory accessible to php.

     Caution: wsGallery can be used to access files that are not in a site's web directory.
              Hence, wsGallery.php should NOT be installed if non-trusted people can edit the wsGallery.php, or wsGallery_treeList.php, files

  Details

    You can specify several "trees" -- each tree being a directory and its subdirectories.
    Typically, these trees are under your web root. But they do NOT have to be.

    Tree specification is done using a simple syntax, with one row used for each tree.
    Syntax of a row:
        treename:  treeRootDir, selBase, descripton

  In addition to tree specification rows, you can specify a "default tree" parameters (which MUST start with a period).
        .showTree:  aTreename

    where
        treeName: REQUIRED. A  one word  name (a string) used to identify this tree.
                  If must NOT be a number. It must NOT start with a period. It must NOT be more than one word!
        treeRootDir : a fully qualified path to the root of the tree.
                    If not specified, the web site`s root is used (i.e.; d:\www).
        selBase : the "selector" used to point to files, relative to the treeRootDir.
                  If not specified, '/' is used
                  If ~ appears (typically as the first character), the ~ is replaced by the relative path to wsGallery.php  
        desription: optional one line used for this tree. Do NOT use HTML tags (they will be removed)!
                    If not specified, a generic description is used

   and (a line that starts with a period)
        .showTree: wsGallery will show this tree when first invoked.
                    If not specified, _default is used.
                    If specified, it MUST be one of the treenames you specified, or _default


    The _default tree.
          wsGallery is packaged with support for a '_default' tree, specified as:
           _default: , ~/images,   The default tree for wsGallery
          where, as noted above, ~ is the path to wsGallery.

     Example (assuming that d:\www is the root directory for this website, and /gallery/wsGallery.php is the full selector):

           _default: , ~/images, /, The default tree for wsGallery
           newPhotos: , personal/photos, My family photos
           oldPhotos: f:\archive, /pre2000/photos, f:\archive, My old photos
          .showTree:

      * The  _default tree is under d:\www.  Its subdirectories are under d:\www\gallery\images  (since ~ yields /www/gallery)
      * newPhotos is under d:\www. Its subdirectories are under d:\www\personal\photos\
      * oldPhotos is NOT under the standard web directory. Its subdirectories are under f:\archive\pre2000\photos

     Notes:

        * Specifying a treeRootDir that is NOT under the web tree is a security risk. It can be used to access
          any file in the file space (of whatever account is hosting the web domain).

        * The specification of the fully qualified path to your file containing directories is a concatenation
          of the treeRootDir and the selBase.  Thus, you can choose which portion of the fully qualified path
          is in treeRootDir, and which in selBase

        *  HOWEVER: if the path is under the standard web directory, it is best to use the default treeRootDir, a
          and set the selBase accordingly. This recommendation is due to the use of direct links to files
          in the standard web directory tree, rather than php calls.

        * If a treeRootDir, or the selBase under this rootDirTree, directories do NOT exists: an error will be displayed

        * Technical note: the default rootdir is derived using $_SERVER['DOCUMENT_ROOT']

        * / will be added (or removed) from treeRootDir and selBase as needed. You don't have to worry about whether or not
          to include them.

        * If invalid trees are specified, wsGallery will exit with an error message

        * You do NOT have to specify a _default tree. Or, you can specify it to be anywhere (using the standard syntax).
          Thus while the  installed version of _default points to the images/ directory (a directory created on
          installation), it does NOT have to.


  -------------------
Daniel Hellerstein, danielh@crosslink.net

Please see the readme.txt file for additional contact information and the disclaimer.

