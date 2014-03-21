#Asterisk Opus/VP8 patch


Since Opus and VP8 cannot, as of now, be integrated in the Asterisk repositories (learn why in this [thread](http://lists.digium.com/pipermail/asterisk-dev/2013-May/060356.html)), we prepared a patch that adds support for both codecs (Opus transcoding, VP8 passthrough).

## Credit
Initial 11.1.x work done by [meetecho](http://github.com/meetecho).
Additional 11.4.x & 11.5.x patches produced by [netaskd](http://github.com/netaskd).

##Installing the patch
To support Opus, you'll need to install [libopus](http://www.opus-codec.org/downloads/) first. No library is needed for VP8, as its support is passthrough only.

Choose the patch for the version of Asterisk you're using:

	patch -p1 -u < asterisk-11.5.x.diff

Run the bootstrap script to regenerate the configure:

	./bootstrap.sh

Configure the patched Asterisk.

	./configure --prefix=/usr

Make sure that codec\_opus and format\_vp8 are enabled in menuselect before going on. For better playback quality, install the slin16 versions of the Asterisk sounds, which are not enabled by default.

	make menuselect

Compile and install.

	make
	make install

##Testing
You can test Opus using the free softphone [PhonerLite](http://phonerlite.de/download_en.htm). Make sure you choose the beta version, as the stable one does not comply with [draft-ietf-payload-rtp-opus](http://tools.ietf.org/html/draft-ietf-payload-rtp-opus-00) (RTP timestamp increment). The codec\_opus module also has a CLI command to enable debugging: type _opus set debug_ for information about it.

	Usage: opus set debug {status|none|normal|huge}
		Enable/Disable Opus debugging: normal only debugs setup and errors, huge debugs every single packet

For VP8 you can make use of the open source softphone [Linphone](http://www.linphone.org/eng/linphone/news/linphone-3.5.0-released-for-desktop.html), which added support for VP8 in version 3.5.0.

##What is missing
SDP fmtp parameters related to Opus and defined in [draft-ietf-payload-rtp-opus](http://tools.ietf.org/html/draft-ietf-payload-rtp-opus-00) are parsed but currently ignored: this means that there's no interaction between chan\_sip and codec\_opus in that sense. Besides, there still is no ad-hoc Opus configuration file for codec defaults. VP8, as anticipated, is passthrough only: besides, there's currently no way to read VP8 files for Playback.


