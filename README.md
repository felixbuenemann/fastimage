FastImage
=========

### FastImage finds the size or type of an image given its uri by fetching as little as needed

The problem
-----------

Your app needs to find the size or type of an image.  This could be for adding width and height attributes to an image tag, for adjusting layouts or overlays to fit an image or any other of dozens of reasons.

But the image is not locally stored - it's on another asset server, or in the cloud - at Amazon S3 for example.

You don't want to download the entire image to your app server - it could be many tens of kilobytes, or even megabytes just to get this information.  For most image types, the size of the image is simply stored at the start of the file.  For JPEG files it's a little bit more complex, but even so you do not need to fetch much of the image to find the size.

FastImage does this minimal fetch for image types GIF, JPEG, PNG and BMP.  And it doesn't rely on installing external libraries such as RMagick (which relies on ImageMagick or GraphicsMagick) or ImageScience (which relies on FreeImage).

You only need supply the uri, and FastImage will do the rest.

Features
--------

Fastimage can also read local (and other) files, and uses the open-uri library to do so.

FastImage will automatically read from any object that responds to :read - for
instance an IO object if that is passed instead of a URI.

FastImage will follow up to 4 HTTP redirects to get the image.

FastImage will obey the http_proxy setting in your environment to route requests via a proxy.

You can add a timeout to the request which will limit the request time by passing :timeout => number_of_seconds.

FastImage normally replies will nil if it encounters an error, but you can pass :raise_on_failure => true to get an exception.

Examples
--------

```ruby
require 'fastimage'

FastImage.size("http://stephensykes.com/images/ss.com_x.gif")
=> [266, 56]  # width, height
FastImage.type("http://stephensykes.com/images/pngimage")
=> :png
FastImage.type("/some/local/file.gif")
=> :gif
FastImage.size("http://upload.wikimedia.org/wikipedia/commons/b/b4/Mardin_1350660_1350692_33_images.jpg", :raise_on_failure=>true, :timeout=>0.1)
=> FastImage::ImageFetchFailure: FastImage::ImageFetchFailure
FastImage.size("http://upload.wikimedia.org/wikipedia/commons/b/b4/Mardin_1350660_1350692_33_images.jpg", :raise_on_failure=>true, :timeout=>2.0)
=> [9545, 6623]
```

Installation
------------

### Gem

```
gem install fastimage
```

### Rails

Install the gem as above, and add it to your Gemfile if you are using bundler, or configure it in your environment.rb file as below:
```ruby
Rails::Initializer.run do |config|
  config.gem "fastimage"
end
```
Then you're off - just use `FastImage.size()` and `FastImage.type()` in your code as in the examples.

Documentation
-------------

http://rdoc.info/projects/sdsykes/fastimage

Tests
-----

You'll need to 'sudo gem install fakeweb' to be able to run the tests.

```
ruby ./test/test.rb
Loaded suite test/test
Started
...............
Finished in 0.059392 seconds.

15 tests, 27 assertions, 0 failures, 0 errors
```


References
----------

* [Pennysmalls - Find jpeg dimensions fast in pure Ruby, no image library needed](http://pennysmalls.com/find-jpeg-dimensions-fast-in-pure-ruby-no-ima)
* [DZone - Determine Image Size](http://dzone.com/snippets/determine-image-size)
* [Antti Kupila - Getting JPG dimensions with AS3 without loading the entire file](http://www.anttikupila.com/flash/getting-jpg-dimensions-with-as3-without-loading-the-entire-file/)
* [imagesize gem](http://imagesize.rubyforge.org/)

Licence
-------

MIT, see file [MIT-LICENSE](MIT-LICENSE)
