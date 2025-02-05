Changelog for Framework version 1.3:

General
=======
* Removed support for Visual Studio / Visual C++ 6.  If you use these versions, you will need to update your project files manually.
	* Note: you can download Visual Studio 2005 Express edition for free from Microsoft (http://msdn.microsoft.com/vstudio/express/)
* PAKfile support - See docs/Pak Resource File Support.doc
* Added support for wide displays and for windowed emulation of wide displays.
* some wide string/localization related changes to strings/chars: Use the WideString.vcproj projects for widestring support
* Added support to draw anti-aliased lines/polygons
* Cached WAV files are "encrypted" (weakly) to prevent the easy copying of sound resources.
* All demos were made to be widestring compatible and we added corresponding widestring projects. std::string has been replaced with SexyString, char with SexyChar, and a few std::string to SexyString conversion functions are now scattered about the demos, where needed.
* Irrelevant build configurations were removed from the projects
* Fixed problem with project files where the framework wouldn't be built unless you manually built it. This caused the error with not finding sexyappframework.lib and preventing the demos from linking if you just did a rebuild all.
* Made all projects default to not use edit/continue in debug mode

Vista Support
=============
* Added GetAppDataFolder() to get the top-level directory where you can save files to. On Vista, this returns the appropriate storage location. On non-Vista, this returns the empty string.
	* mFullCompanyName member of SexyAppBase is used to create this directory in the SexyAppBase::Init function.
	* This directory looks like this: &lt;Windows return location to ProgramData [defaults to c:\ProgramData]&gt;\mFullCompanyName\mProdName\
* Added CheckForVista() function that returns true if running on Vista.
* Added CheckFor98Mill() function that returns true if running on Win95, Win98, or WinME
* Changed registry functions to use HKEY_CURRENT_USER on all systems
* Cached sound files save to GetAppDataFolder() location now (this is the same location on Pre-Vista machines; see above)
* Added AllowAllAccess() function to Common to allow developers to set the permissions to directories/files to Everyone -> Full Control.

ImageLib
=============
* JPEG2000 support added using j2k-codec (see http://www.j2k-codec.com for licensing information)
* Fix to GetImage function not casting std::string::size_type to an int before passing it to min/max
* Added gAutoLoadAlpha global variable.  When true, GetImage will look for alpha masks (same filename with _ at the end or beginning of file).  When false, alpha masks will not be automatically loaded.
* Added gIgnoreJPEG2000Alpha global variable.  When true, the alpha channel in a jp2 file will be set to entirely opaque.  When false, the alpha channel will be preserved.


SWTri_Pixel888.cpp
===================
* fix for alpha values that might end up getting messed up when writing to memory images.

DDI_FastStretch_Additive.inc
============================
* fix for drawing stretched images in software being off a pixel

HTTPTransfer
====================
* Added member variable /mContentLength/ to hold the "Content-Length: " parameter

MI_NormalBlt/BltRotatedHelper.inc
=================
* 16-bit rotation optimizations

BASS related:
=============
* Upgraded BASS files to work with 2.3 (2.2.0.1 is still shipped by default w/ framework though)
* Exposed a lot more bass.dll methods
* added variable, BassMusicInterface::mMusicLoadFlags, so that you can override the BASS music loading flags with whatever flags you want.


MTRand
======
* Added methods for retrieving random integers and floats within a specified range.

SoundManager/DSoundManager
==========================
* Added GetFreeSoundId to SoundManager to return the first available sound id
* Has resource manager call soundmanager->releasesound when a sound is unloaded
* sound ids are based on GetFreeSoundID instead of mCurSoundId, and mCurSoundId no longer exists.

Slider
=======
* Added the ability to have vertical sliders instead of only horizontal: Just specify mHorizontal = false. Defaults to true for backwards compatibility.
* fixed bug in SetValue():
Setting a value less than 0 or greater than 1 could cause problems/crashes. Now clamps value to 0->1 range.
* Added functionality so that if you click on the scroll bar (but not on the thumb) then the thumb and value get set to the position you clicked on. Previously, the widget didn't respond if you clicked anywhere outside of the thumb.

Font/ImageFont
===============
* Fixed small inefficiency in CharWidth.
* Allow colors specified in the decriptor to be simple integers such as 0xffffff instead of (1,1,1,1).
* Fix crash with legacy font loading.
* Make SysFonts return the correct Descent so that WriteWordWrapped works correctly.
* Support drawing on MemoryImage objects with SysFont objects.

SexyAppBase
===========
* Added mPhysMinimized and defer alt-enter processing to ProcessDeferredMessages.
* Added mEnableMaximizeButton option to enable/disable the maximize button.
* Added KillDialog method which lets you specify whether you want to remove and delete the dialog or not (useful for scrolling the dialog off the screen after it's ended).
* Added tablet support.
* Broke up the GameAlreadyRunning/NotifyGame message logic into virtual functions that can be overridden.  This is so apps can do other things if the game is already running.
* Add mFullCompanyName -- used for setting AppDataFolder
* mChangeDirTo for -changedir command line parameter.
* Add WM_QUERYOPEN hook which calls SexyAppBase::AppCanRestore.  If this method returns true, Windows will be allowed to restore the window. Otherwise, it will not.
* Add mIsDisabled to track disabled state of main window.
* Change RegistryRead to use helper function RegistryReadKey while passing in HKEY_CURRENT_USER as the key to read.

FlashWidget
===========
* SysColorChanged notification for Widgets. Now FlashWidget can detect when it needs to MarkDirty entirely.
* fixed a crash caused by referencing invalid memory
* Windows SDK v6.0 does not include comsuppw.lib by default with comdef.h.  Manually added it.

SexyMatrix
==========
* Allow DrawImageTransform to have scalex = -1 for mirroring an image.

ModVal
======
* Made it so you can have multiple M()'s on a line instead of having to have unique M1(), M2(), etc

D3DInterface
============
* Added mPhysMinimized and defer alt-enter processing to ProcessDeferredMessages.
* Fixed multithreaded issue with mImageSet.
* Fixed problem in BltTransformed when center parameter was true.
* Copy right and bottom pixels to fill the borders of textures which don't go to the border.  This emulates the texture having clamping turned on and going right up to the edge.
* Recover bits shouldn't recover old bits (make sure bitschangedcount is up to date).

Graphics and related .inc files
================================
* Added the ability to return a wordwrapped string to writing its original color by using the ^oldclr^ tag
* Added SetScale:
	Scales all calls that route through DrawImage(Image* theImage, int theX, int theY, const Rect& theSrcRect) by a value. This is part of the graphics state, so you can use Push/PopState with it.
* Added Is3D to see if this graphics is drawing using the 3d card.
* New draw string wordwrapped
* Added DrawStringColor.
* support for antialiased line/polygon drawing/filling
* made WriteWordWrapped be able to continue from an indented point
* Changed DrawStringColor to take an old color.
* Fixed problem with DrawLineClipHelper needing to tell calling function that line is completely clipped.
* Commented back in theOldColor in WriteWordWrappedHelper.
* Fixed for loop scoped variable problem.
* Optimizations to common drawing paths.  Huge increases in FPS in some situations. If things look weird, #undef OPTIMIZE_SOFTWARE_DRAWING in MemoryImage.h
* fixes bug with drawing stretched images to another image and having the resulting image appear partially transparent
* Divide by zero fix when DestRect width/height = 1 and also stretching out (Bilinear blend version of stretching)
* Just return from DrawImageBox if srcWidth or srcHeight <= 0.
* Fixed write string problem where ^^ wouldn't work if it was near the end of the string.
* Added theMaxChars parameter to WriteWordWrapped so you can gradually reveal all of the characters in a bunch of lines of text (just using substr doesn't work because the word wrapping changes as characters appear).
* Made DrawImageRotatedF use the exact subpixel center of the image as the pivot point (as opposed to the integer center).


WidgetContainer
===============
* Added WidgetContainer::WidgetRemovedHelper for calling RemovedFromManager on all child widgets and for calling DisableWidget on all child widgets.
* fixed: If you have a widget that is at the end of mWidgets, and then that widget adds another widget and calls UpdateApp (like the blocking dialog box wait function), if you do not remove the added widget (i.e. set autokill for a blocking dialog wait function), and if the addING widget is not at the END of the mWidgets list, then the app will crash with an invalid iterator, as there is only 1 mUpdateIterator and it is being overwritten while UpdateApp is called. Also, if mUpdateThisRemoved is ever set while calling UpdateApp, its value will overwrite any previous value before the call to UpdateApp.
* Added WidgetContainer::mZOrder which is used to keep a widget at a certain relative position in the widget list
* Fixes a re-entrency bug that can cause the app to crash if mUpdateIterator is already at the end of the widget list inside of UpdateAll and mUpdateThisRemoved is false: previously the iterator got incremented again, causing it to be invalid.
* Fix to GetWidgetAtHelper related to Base Modal dialogs.
* Made all functions virtual


Widget related
===============
* Fixed default values for fast stretch and linear blend in FlushDeferredOverlayWidgets.
* added ability to have draw overlay pass in the priority at which it's calling the function
* made Widget::ShowFinger check if mWidgetManager == NULL: previously it did this:

if (on)
     mWidgetManager->mApp->SetCursor(CURSOR_HAND);
else
     mWidgetManager->mApp->SetCursor(CURSOR_POINTER);

which will crash if the widget manager is NULL.

ResourceManager
===============
* Added LoadFont and DeleteFont.
* Added support to use sys fonts in resources.xml, via path="!sys:font_face" size="point_size"   Bold, underline, italics, and shadow can all be specified as attributes in the tag itself.

Checkbox
========
* added default draw method to checkbox for if no checked/unchecked image is specified

XMLParser
=========
* fixed a potential bug with reusing the same XMLParser object on multiple files.
* Added more supported encoding types: UTF-8, UTF-16, UTF-16LE, UTF-16BE.
* Encoding types can be automatically detected by byte-order-markers, or they can be explicitly set with SetEncodingType method.

D3DTester
=========
* Always retest 3d if it fails...  This way, the user isn't permanently locked out of 3d for any reason.

Common.h/.cpp
=============
* Fixed MkDir so it can handle backwards and forward slashes.
* added functions: CheckForVista, GetAppDataFolder, SetAppDataFolder, AllowAllAccess. See "Vista" section for more Vista details.
* Many helper functions introduced to support wide string, and the new framework string type: SexyString.
* Added macro _S() which can be used to encapsulate string or character constants.  This will ensure that strings are wide string if _USE_WIDE_STRING is defined, or narrow strings if it is undefined.
