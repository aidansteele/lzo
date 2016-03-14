# LZO Ruby gem

Wikipedia:

> Lempel–Ziv–Oberhumer (LZO) is a lossless data compression algorithm that is focused on decompression speed.

It's an alternative to Gzip, essentially. This gem exists because the previous 
Ruby LZO gem ([`lzoruby`][lzoruby]) hasn't been updated for 5 years and doesn't 
support LZOP (as generated by the `lzop` utility) or JRuby. We use the 
[`ffi`][ffi] gem to provide VM-agnostic bindings to the LZO library.

[lzoruby]: https://rubygems.org/gems/lzoruby
[ffi]: https://rubygems.org/gems/ffi

## Installation

The LZO library is **not** bundled with this gem. It must be installed beforehand.
Here's how you can do that on various popular OSes:

* OS X: `brew install lzo`
* Ubuntu: `apt-get install liblzo2-2` (`liblzo2-dev` works fine too)
* Fedora: `yum install lzo` (or `lzo-devel`)

Then the bog-standard `gem install lzo` - you know the drill.

## Usage

```ruby
LZO.decompress(string_or_io) # returns String
LZO.compress(string_or_io) # returns String

file = File.open('/path/to/file.lzo', 'rb')
reader = LZO::LzopDecompressor.new file
reader.name # => "file.txt"
reader.mode # => 0100644
reader.mtime # => 2016-02-03 14:29:36 +1100
reader.method # => :M_LZO1X_1
reader.level # => 5
reader.read(10) # => "The quick "

output = File.open('/path/to/output.lzo', 'wb')
writer = LZO::LzopCompressor.new output, name: 'output.txt', mode: 0100644, mtime: Time.now
writer.write "first chunk of data"
writer.write "second chunk of data"
writer.close
```