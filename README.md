# wsGallery
An image viewer: with zoom, several scrolling methods, thumbnails, and more

```
 CAUTION:
  wsGallery can be setup to allow access to all the directories of an account.
  That is: any file on any directory accessible by php.
  Thus: if you do not control access to the directory and drive wsGallery.php is installed on
         -- you should NOT install wsGallery on your webserver!
```

**Features**
- wsGallery is designed to display image files: 'png','gif','jpg','jpeg','bmp','xbm'.
   It can also display other files, including .mov, .mp4, .avi, .txt, .html, .pdf, .xls, and .ppt.

  The display of these other files may be browser dependent (some files, such as .avi, are 
  not supported by most browsers and depend on what viewers a client has installed).

- Image file display is well supported! You can easily pan within, and zoom into, an image file.
  And you can open a small version of the image to quickly navigate to a location in a larger
  version of the same image.

 - On line help is available by clicking on the ? icons. 
  Use the "lantern" icon (at the top of each help topic window) to see an index of help topics (including 
  admin only ones).
  
 - wsGallery works best on desktops. However, a "compact" version (wsGallery_small.php) provides a reasonable
   interface for mobile devices.

**Installation and setup**

- Unzip the wsGalleryxx.zip file to a dedicated directory. 
 
   Typically a subdirectory with  name like wsGallery_v13 is created.
   It is best to rename this to something more generic (such as wGallery)l
   Several subdirectories will be created under this directory.  

-  Several parameters files (in these subdirectorie) must be modified using your favorite text editor.

-  When ready, point your browser at `wsGallery.php`
   On first load, wsGallery will list the status of the installation, noting what files need to be modified (and where these files are).

See wsGallery_readme.txt for detailed installation and setup instructions.


