The framework ships with a demo version of the j2k-codec. The demo has the following restrictions:
	* beeps at the end of each decoding
 	* open the ordering page once in about 30 decoded images
	* on the rare occasions do several other little nags

If you want to use the j2k codec in your application, you will need to purchase a key from
http://j2k-codec.com. The price is between $49 and $399. Once you have purchased a key,
you can use it in the framework by calling:

ImageLib::SetJ2KCodecKey(&lt;your key&gt;)

When you load a JPEG2000 image, your key will be automatically passed to the j2k-codec library.
