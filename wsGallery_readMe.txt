6 June 2022

wsGallery:  ver 1.3  -- quickly  list and view  images, and other files,  in a directory tree

  CAUTION: wsGallery can be setup to allow access to all the directories of an account.
           That is: any file on any directory accessible by php.
           Thus: if you do not control access to the directory and drive wsGallery.php is installed on
                       -- you should NOT install wsGallery on your web server!

Contents

I.   Features
I.a     Other means of accesing wsGallery data
I.b     Setup and intallation
II.   Specifying wsGallery galleries
II.a    Specifying trees in a `gallery`
II.b    Specifying trees: details
II.c.   Initializing galleries and trees
II.d.   Directory initialization
III.  wsGallery parameters
IV.   Reinstallling wsGallery
IV.   Contact

   ----------------------

I.  Features
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

  wsGallery can also be run with request line attributes. These can be used to specify what is displayed by default --
  such as a set of files in directory, using a chosen screen layout. For details, see wsGallery_request.txt

I.a  Other means of accesing wsGallery data

  A simpler interface to wsGallery -- that does NOT do any intialization -- is wsGallery_simple.php.
  wsGallery_simple.php works with "cache" and other information created by wsGallery.php.
  It has fewer features, and is more compact.

  You can also "access" wsGallery's data  -- the files it can access, including thumbnails and snapshots -- using an
  ajax-like (javascript / php) techniques. This provides a means of creating a custom user-interface to a set of image and other
  files.  wsGallery_get.js and wsGallery_get.php provide a means to do this!
  Note that wsGallery_simple.php demonstrates the uses of wsGallery_get.js and wsGallery_get.php.

  See wsGallery_get.txt, and wsGallery_simple.php, for the details.

I.b Setup and intallation

  wsGallery is part of the wsurvey. package, and uses several javascript and php libraries packaged with wsurvey.
  However, wsGalleryxxx.zip is shipped with the necessary libraries and can be used in a standalone mode.

  The basic installation steps:
      * unzip wsGallery to its own directory. It will unzip to a wsGallery_v12 directroy, which will contain a number of subdirectories.
      * modify  wsGallery_treeList.php, and wsurvey.adminLogon_params.php
      * run a "tree" initialization step 
      * ready to go (just add image and other files)!

 1)  Unzip wsGalleryxxx (xxx a version code) into its own subdirectory in your web tree.
    * wsGallery is typically unzipped to a wsGallery_v12 directory. wsGallery_v12 will contain a several sub directories.
     For example: if you saved wsGalleryxx.zip to /wsGallery, /wsGallery/wsGallery_v12 will be created.
      You can use wsGallery_v12 in url request.s Or rename wsGallery_v12 to wsGallery. Or copy its contents to an emtpy wsGallery directory. 
    * wsGallery.php is the front end to wsGallery.

 2) Set some "tree specifications" in wsGallery/galleries/main/wsGallery_treeList.php.

 3) Edit wsurvey.adminLogon_params.php,
       Example: /wsGallery/wsurvey.adminLogon_params.php
    and change the $passwordActual variable to a password of your choice.
    You can also change the $pwdDuration line.

    Note that logon is only required for administrators. Controlling access to clients can be accomplished
    through the usual server tools.

 4) Assuming wsGallery.php ended up in /www/wsGallery (and that /www is the root web directory),
   access wsGallery with something like http://mysite.org/wsGallery/wsGallery.php

 5) On first load of wsGallery.php, you will see an Administrator Notes screen.  
   It contains descriptions of the current parameter files.
   If you scroll to the Status area, you will probably see an error message (that no treelist.json is available)

 5a) You will have to restart wsGallery several times. Each time, you will be asked to logon (using the password created in step 3).
    a) Initialize the wsGallery paramerers. If there are errors: error messages are shown. You will have to fix them, and then restart wsGallery again
    b) Initialize the "spinners" (animated gifs used to denote server processing is occurring)
    c) Initialize the gallery. If there are errors: error messages are shown. You will have to fix them, and then restart wsGallery again
       See section II.c for further details
  
    Step a will also occur whenever you update (even inconsequentially) the data/wsGallery_params.php   
    Step b rarely occurs -- only if you add animated icons to wsGallery/icons/spinners
    Step c will also occur whenever you update the wsGallery_treeList.php file of a selected gallery.

 5b) Restart wsGallery again! You should see the wsGallery screen, listing the trees in the current gallery (by default, the _default tree in the  main gallery).

 5c) Using the admin tools in wsGallery, initialize the trees in the current gallery.

 6) For optimal performance, such as creating a cache of thumbnails from images, you should initialize each of the directories
    in these trees. See the help notes on the pros and cons of doing this (much faster performance versus a need for disk space).

 7) You (and your public)  will then be able to view files -- images and others -- in the directories in the "trees" you specified.
    And if you are ambitious, you can add descriptions to these files!

 8) Ambitious administrators, who want to control wsGallery performance, can edit wsGallery_params.php.
    See the descriptions in wsGallery/data/wsGallery_params_original for the details.
    In general, we don't recommend changing wsGallery_params.php.

    There is one exception: if your version of php does NOT support all the image types, you must edit  wsGallery_params.php --
    removing the unsupported image types from the 'imgExts' parameter specification.

        Note: the original version of wsGallery_params.php, and wsGallery_treeList.php, are
              stored in the wsGallery/data/ directory.   Use them if you mangle the working versions!

                 ---------------

II. Specifying wsGallery galleries

  wsGallery can support completely seperate sets of images & files.
  Each of these sets is called a `gallery`.  Only one `gallery` is viewable at a time.

    * Within a `gallery` there can be several `trees`. The user can switch between trees easily.
    * Each `tree` can contain numerous subdirectories.   The user can switch subdirectories easily.
    * Each subdirectory can contain numerous files.  The user can easily select a file to view.

  For security reasons: to specify a `gallery`, an administrator must create a subdirectory under
  wsGallery/galleries. We recommend using a lowercase name.

  Notes:
    * wsGallery is distributed with a `main` gallery already specified (wsGallery/galleries/main).

    * if the switchGallery parameter (in wsGallery_params.php) is enabled, a "switch gallery" menu is available  (displayed in the
      select-directory/tree menu).   This can be used to view a different gallery via clicking a buttons within the wsGallery user screens.

    * You can also specify "collections" -- sets of directories that are group together, that may be in completely
      different galleries and trees.

II.a Specifying trees in a `gallery`

   Each gallery can contain several trees, where each tree points to a different 'root directory'.
   And under such a root directory there can be hundreds of subdirectories with hundreds of files -- all of which can be accessed
   by wsGallery.

   Trees are specified in a gallery specific wsGallery_treeList.php file.

   The 'gallery specific' treelist (wsGallery_treeList.php) must be created by the administrator.
   It is used to specify directory "trees" that comprise this gallery.
   In particular, each tree is associated with a root directory.
   This root directory is often a directly under the sites's www home directory.
   But it can be any directory accessible to php.

   Caution: wsGallery can be used to access files that are not in a site's web directory.
              Hence, wsGallery.php should NOT be installed if non-trusted people can edit the wsGallery.php,
              or wsGallery_treeList.php, files

   Technical note:
      wsGallery uses "caching" to expedite performance, and to support "collections".
      This caching is done on a directory-in-a-tree basis.
      The caching uses data stored in tree-specific subdirectories under a gallery's directory.
      For example: for the 'main' gallery: the 'my_photos' tree would use wsGallery/galleries/main/my_photos

   Reminder: each gallery must have its own version of  wsGallery_treeList.php.
             For example, for a gallery named 'yourGalleryName':
                  wsGallery/galleries/yourGalleryName/wsGallery_treeList.php

II.b Specifying a treelist: details

    You can specify one or more "trees".

    This requires  modifying the wsGallery_treeList.php file in the root directory of a gallery.
    For example: /wsGallery/galleries/main/wsGallery_treeList.php.

    Typically, these trees are under your web root. But they do NOT have to be.

    Tree specification is done using a simple syntax, with one row used for each tree.

    Syntax of a row in wsGallery_treeList.php:

        treename:  treeRootDir, selBase, descripton

    where:

        treename: REQUIRED. A  one word  name (a string) used to identify this tree.
                  It must NOT be a number. It must NOT contans any periods. It must NOT be more than one word!
                  We strongly recommend using lower case ONLY to specify each treename.

        treeRootDir : a fully qualified path to the root of the tree.
                    If not specified, the web site`s root is used (i.e.; d:\www).
                    For better performance: whenever possible, use treeRootDir='' (or the full path to the www root dir)
		    That means that images (and other files) are best placed in directory that is directly accessible via the web.
		    In other words: assuming no access control, a directory whose file can be retrieved using a standard url with
                    no special server side script needed.

        selBase : the "selector" used to point to files, relative to the treeRootDir.
                  If not specified, '/' is used
                  If ~ appears (typically as the first character), the ~ is replaced by the relative path to wsGallery.php

        desription: optional one line used for this tree. Do NOT use HTML tags (they will be removed)!
                    If not specified, a generic description is used

  In addition to tree specification rows, you can specify a "default tree" parameters (which MUST start with a period).
        .showTree:  aTreename

     wsGallery will show this tree when first invoked.
          If not specified, _default is used.
          If specified, it MUST be one of the treenames you specified, or _default


  The _default tree.
          wsGallery is packaged with support for a '_default' tree, specified as:
               _default: , ~/images,   The default tree for wsGallery
          where, as noted above, ~ is the path to wsGallery.

     Example (assuming that d:\www is the root directory for this website, and /wsGallery/wsGallery.php is the full selector):

           _default: , ~/images, /, The default tree for wsGallery
           newphotos: , personal/photos, My family photos
           oldphotos: f:\archive, /pre2000/photos, f:\archive, My old photos
          .showTree:

      * The  _default tree is under d:\www.  Its subdirectories are under d:\www\wsGallery\images  (since `~` is the same as `/wsGallery`)
      * newphotos is under d:\www. Its subdirectories are under d:\www\personal\photos\
      * oldphotos is NOT under the web root. Its subdirectories are under f:\archive\pre2000\photos

     Notes:

        * Specifying a treeRootDir that is NOT under the web  root is a security risk. It can be used to access
          any file in the file space (of whatever account is hosting the web domain).

        * The specification of the fully qualified path to your file containing directories is a concatenation
          of the treeRootDir and the selBase.  Thus, you can choose which portion of the fully qualified path
          is in treeRootDir, and which in selBase.

        * HOWEVER: if the path is under the web root, you SHOULD use the default treeRootDir (that is, leave it blank).
                  And set the selBase accordingly.
                  This allows wsGallery to use direct URLs to files under the web root web directory
                  (in <img ..> elements), rather than php calls.

        * If a treeRootDir, or the selBase under this treeRootDir, directories does NOT exist: an error will be displayed

        * / will be added (or removed) from treeRootDir and selBase as needed. You don't have to worry about whether or not
          to include them.

        * If invalid trees are specified, wsGallery will exit with an error message

        * You do NOT have to specify a _default tree. Or, you can specify it to be anywhere (using the standard syntax).
          Thus while the installed version of _default points to the images/ directory (a directory created on
          installation), it does NOT have to.

        * Technical note: the default rootdir is derived using $_SERVER['DOCUMENT_ROOT']

        *  wsGallery/data/wsGallery_treeList_original.php contains an annotated example of a treelist. 

II.c. Initializing galleries and trees

  After specifying a wsGallery_treeList.php file: subdirectories, and data files, must be initialized for each tree.

  wsGallery will do this. If gallery initialization is required, wsGallery will display a screen noting a problem (such as no .json file),
  and ask you to logon.  Once you logon, gallery initialization will occur (for all the trees in the currently selected gallery).

  Gallery inititalization includes
    a) creating "tree specific" subdirectories for each tree specified in the gallery's wsGallery_treeList.php file
    b) creating "cached" versions of several files (for internal wsGallery use)
  After initialization, you will be asked to reload wsGallery.

  For those who want extra security, this automatic  gallery initialization can be suppressed. 
  To do this edit the data/wsGallery_params.php file   -- and set 
      requireLockme: 1
   When this happens, before attempting tree initialization wsGallery will look for a wsGallery/data/lockme.txt file.
   If one exists, wsGallery will issue an error messages, and will NOT try to initialize.

   In this case (requireLockme:1), to allow wsGallery to initialize trees:

     a) Rename, or delete, wsGallery/data/lockme.txt
     b) Reload wsGallery.php. You will see the "logon to initialize" menu (as described above). 
     c) Recreate wsGallery/data/lockme.txt. If you don't wsGallery will issue a few nagging reminders everytime it runs.
     d) Rewload wsGallery.php!

  Note that if you have several galleries, switch to these galleries -- wsGallery reloads and will intialize the selected gallery (with logon required each time). 

  After this gallery intitialization you should initialize each of its trees. This is done using wsGallery's admin screen.

   e) Re load wsGallery, and logon as an administrator -- use the logon button in the upper right corner, and the
      password you specified in wsurvey.adminLogon_params.php

     An administrative menu will appear, that lists all of the trees specified for the currently gallery.

  f) A '!Create' button is displayed next to each "uninitialized" tree.
     Clicking such a button will bringup a new menu (for this tree).

     The menu has a few options you can change: such as what subdirectories to ignore.
     You can hit the Preview button to list the  directories (in this tree).

  h) When ready, click the Go! button. After a few seconds (or a few minutes, if there are a lot of directories and files),
     a status screen will appear.

wsGallery is now able to display images (and other files) in this tree!

The status screen contains a "manage the directories ..." button. We recommend using it to initialize
the "directories" of this tree. See section II.d for the reasons why!


Notes:

  * you can skip steps e to h: they can be done at a future date.
    But until they have been done: wsGallery can not access the tree.
    When a user select an "uninitialized" tree, a error messages appears -- that notes administrative action is needed.

  * trees are specific to a "gallery".  You can have different galleries that have trees with the same name.
    And these can point to the same, or completely different, directories.

  * under some error conditions a gallery may not display its trees.
    This can usually be fixed by deleting the treelist.json file in  galleries/yourGalleryName -- and then do the above.

  * gallery, tree, and directory "initialization" are seperate steps.
      Gallery "initialization" is required! If you select an uninitialized gallery, wsGallery will insist you initialize it!
      Tree "initialization" is required, but can be done at a later date.
	   Uninitialized trees will not be accessible, but initialized trees will be.
      Directory "initialization" is recommended!  Directory initialization initialization can be done at later date.
           Uninitialized diretories will be accessible, but display will be slower.


II.d. Directory initialization

  After initializing a tree -- its contents (images and other files) can be viewed with wsGallery.
  Your site is ready to go!

  However -- there are some additional actions that can improve peformance.
      In particular: using wsGallery's admin tools to initialize directories.
      That is: create and save information on the potentially numerous directories that comprise a tree.
  
  What is this information?
      Directory "initialization" will create a "cache".
      The cache consists of data files with information on files inside of a directory.
      And... ready-to-download snapshots, and thumbnails, of these files.

  Why inititialize?
    a) Retrieval of snapshots and thumbnails is faster  -- especially for trees that point to directories
      that are not under the web root.
    b) wsGallery_simple.php can only be used to view directories that have been initialized.
    c) wsGallery "collections" can only work with directories that have been initialized.

  There are a few drawbacks:
    a) Initialization can be time consuming. If you have thousands of images spread across hundreds of directories of a tree, it can take hours.
      But you can start and walk away -- wsGallery will continually communicate with the server, so it won't time out.
    b) It takes hard disk space -- roughly about 5% of the size of the viewable image (and other) files.

   There is a case for not initializing:

    a) If you (the administrator) will never create "collections"
    b) You will never use wsGallery_simple.php
    c) It is okay if the first time a directory is viewed, the user must wait for a basic initialization (creation
       of some data files).
       And perhaps a minute or two wait the first time thumbnails are requested.
       And a short wait the first time a snapshot is viewed.

    Note that if hard disk space is limited, you can suppress all caching. See the description of wsGallery_params.php
    (the noCache parameter).

 To initialize directories:

   a) Logon as an admin
   b) In the admin menu, click on a tree's 'dirs' button (that has an eyeball and a pencil).
   c)  A list of all the directories in the tree is displayed in a popup container.
       Click on a check mark to select which directories to initialize
       To initialize all of them, click the Mark button.

   d) After selecting one, several, or all of the directories: click the All button
   e) A new popup menu will appear. You can remove or overwrite existing "data files" (such as the thumbnail files).
      If this is the first initialization, these are irrelevant (since there are no files to overwrite).
   f) Click the Go button.
   
   Directory initializaation will start. wsGallery shows frequent status messages, for each step of each directory's
   initialization. If there are thousands of files, this can take hours!
   
   When done: you can reload wsGallery -- its really ready to go! 

  Note: the admin menu is where you can create collections. See the online help (the ? buttons) for the details.

  -------------------

III. wsGallery parameters

There are a number of parameters that can be set by the administrator.

These are specified in wsGallery/data/wsGallery_params.php.

Most of the time, the defaults are adequate. 
However, some servers may not support all of the image types that wsGallery can work with.
In that case, you should modify the imgExts parameter.

For an example of a parameters file, with descriptions: see wsGallery/data/wsGallery_params_original.php

  -------------------
IV. Reinstalling wsGallery

If you are reinstalling a newer version of wGallery you can maintain the specifications by: 
1 ) Saving
    a) data\wsGallery_params.php
    b) wsurvey.adminLogon_params.php
  to someplace safe.
2) Rename your old wsGallery directory: say from wsGallery to wsGallery_old 
3) After unzipping wsGalleryxxx.zip,  rename (or whatever) the wsGallery_v12 directory to wsGallery
4) Delete the galleries directory from wsGallery
5) Move (or copy if you are nervous) the galleries directory from wsGallery_old to wsGallery
   Copying could take time and disk space: the galleries directory can contain a lot of "snapshot" and "thumbnail" cache files.
6) Copy the saved version of  data\wsGallery_params.php  to wsGallery/data
   and copy the saved version of wsurvey.adminLogon_params.php to wsGallery
7) Your galleries and collections area ready to use.



  -------------------

V. Contact

Daniel Hellerstein, danielh@crosslink.net

Please see the readme.txt file for additional contact information and the disclaimer.

