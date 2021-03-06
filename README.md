# SimpleVC (Simple Video Converter)

## What is it? 

SimpleVC is a PHP audio and video converter

## Why?

The idea here is to provide a simple way to deal with multiple 
browser support for different audio and video formats. Unfortunately 
different web browsers, for different reasons, tend to support different 
audio and video formats. This means in order to ensure that your audio/video 
is accessible to everyone, you have to provide fall-back options, and 
then different browsers can display it in whatever format is friendly to 
them. It would be nice if you could just supply audio/video files in 
whatever format you have and not worry about whether it will play on 
Firefox or Chrome or whatever. That's what SimpleVC is all about. 
Just supply audio/video in whatever format you have and let SimpleVC 
create and render the fall-back options for you. 

## How? 

SimpleVC does it all through the awesome [FFmpeg] (https://ffmpeg.org/ffmpeg.html) library. 

## Requirements 

* PHP 5.4+ 
* FFmpeg (Version: N-71455-gfbdaebb)

## Quick guide 

The first thing you need to do is add SimpleVC as a dependency 
for your project. 

* Install with [Composer] (https://getcomposer.org): `composer require apefood/simplevc` 

Now you can use SimpleVC's methods and classes like: 

## Example 1: video conversion 

```PHP 

// file to convert
* $filename = /path/to/video/file/; 

// instantiate video object  
* $video = new Video($filename); 

// instantiate  video converter object 
* $video_converter = new VideoConverter($video); 

// get the ffmpeg conversion command 
* $command = $video_converter->convert(); 

```

The convert() method returns the ffmpeg command as a string so that means ultimately 
it's up to you what you do with it. The conversion process is slow, so instead 
of running exec() on the command immediately, perhaps you might want to schedule 
that command as a job/task. 

```PHP 

// get html5 video's <source> tags for the video file & it's associated fall-back options. 
$html = new HtmlFactory($video); 
$source = $html->render(); 

```

Now since the exec() command will be deferred and not run immediately, that means the 
$source variable points to at least 2 non-existent files at the moment. So perhaps you can 
also defer publishing the video on a web page until the conversion task has completed. 

# Extracting video frames 

This command will help you extract video frames and save them as images. You can then use those 
images as preview content for your videos. 

```PHP 

// Time on the video where you want to start extracting the frames. 
// Defaults to null, in which case FFmpeg will start the extraction from beginning of the video.  
$start = "01:00"; // from minute 1 [hh]:mm:ss 

// Stop the extraction after t = $end. Note that if you don't provide the $start argument, it's pointless 
// to specify an $end parameter since the VideoConverter class will just override it to null. 
$end = "03:00"; // stop after 3 minutes 

// How many frames per minute would you like to extract? 
$frames = 2; 

// Your preferred image format. Defaults to JPEG. 
$format = "jpeg"; 

$video_frames = $video_converter->extractFrames($start, $end, $frames, $format); 

``` 

Again, this only returns the FFmpeg command [string] needed to complete this task. It's up to you to actually 
execute this command on the shell (i.e. ``` $ php exec($video_frames); ``` ), when and how you please. 

## Example 2: audio conversion 

There is no example 2 because it's pretty much the same as example 1, but with an audio file. 
Basically swap out every instance of 'video' with 'audio' and you are good to go. 

##WARNING! 

# NO 3GPs ALLOWED! 

When I tried converting a 3GP file to MP4 it failed, on multiple attempts. So do not use this 
package with 3GP files. In fact, I advise you to stick to popular media formats (i.e. MP3/4, MKV, WAV etc.) 
just to be on the safe side. It's 2016, why would you have 3GP files anyway? 

## Closing. 







 

