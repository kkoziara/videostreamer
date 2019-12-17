# videostreamer
videostreamer provides a way to stream video from an input source to HTTP.
It remuxes a video input into an MP4 container which it streams to
connecting clients. This provides the ability to stream an input source
that may have limited connections (it opens at most one connection to the
input), is not accessible via HTTP, or is not easily embeddable in a
website.


## Build requirements
* ffmpeg libraries (libavcodec, libavformat, libavdevice, libavutil,
  libswresample).
  * It should work with versions 3.2.x or later.
  * It does not work with 3.0.x or earlier as it depends on new APIs.
  * I'm not sure whether it works with 3.1.x.
* C compiler. Currently it requires a compiler with C11 support.
* Go. It should work with any Go 1 version.


## Installation
* Install the build dependencies (including ffmpeg libraries and a C
  compiler).
  * On Debian/Ubuntu, these packages should include what you need:
    `git-core pkg-config libavutil-dev libavcodec-dev libavformat-dev
    libavdevice-dev`
* Build the daemon.
  * You need a working Go build environment.
  * Run `go get github.com/horgh/videostreamer`
  * This places the `videostreamer` binary at `$GOPATH/bin/videostreamer`.
* Place index.html somewhere accessible. Update the `<video>` element src
  attribute.
* Run the daemon. Its usage output shows the possible flags. There is no
  configuration file.


## Components
* `videostreamer`: The daemon.
* `index.html`: A small sample website with a `<video>` element which
  demonstrates how to stream from the daemon.
* `videostreamer.h`: A library using the ffmpeg libraries to read a video
  input and remux and write to another format.
* `cmd/remux_example`: A C program demonstrating using `videostreamer.h`.
  It remuxes an RTSP input to an MP4 file.


## This fork

The service listens for user requests on `http://127.0.0.1:8080` by default. User queries the `/streams/<stream_id>` endpoint, which asks the urls service provided at the start via `input` param e.g `./videostreamer -input http://localhost:8000/urls` for the stream url by querying `http://localhost:8000/urls/<stream_id>`.
The fork can handle multiple streams.