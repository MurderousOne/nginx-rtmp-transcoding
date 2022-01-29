# nginx-rtmp-transcoding

<p>WILL TRANSCODE A LIVESTREAM FROM ANY SOURCE TO 720P FOR LOWEND MOBILE PLAYBACK ON TWITCH</p>

<h2>General rtmp settings explained</h2>
<h3>All possible directives explained</h3>

<p>record_path: Set this to the path at which you want to record everything.</p>
<p>recorder: This is a recorder set</p>
<p>record: This is set to all so we get both audio and video into the files.</p>
<p>record_suffix: I set it like that so I can easily identify the files using the hour of the day.</p>
<p>record_interval: This is set to 15 minutes, it will restart recording each times this interval is reached.</p>
<p>You may want to play with the Access settings (Allow/Deny) to only allow your local network to have access to your “In” stream. </p> 
<p>You can read more in the nginx-rtmp wiki <a href="https://github.com/dreamsxin/nginx-rtmp-wiki/blob/master/Directives.md#access">Here.</a> </p>


<h2>FFmpeg settings explained</h2>
<p>-i: This is the input (Your stream, using the right STREAM_KEY)</p>
<p>-vb, -minrate, -maxrate, -bufsize: I found out that my stream was a lot more stable when all of those where the same size. -vb is the video bit rate, -minrate and -maxrate needs to be the same for Twitch (Constant bit rate).</p>
<p>-s: This is the output resolution you want. Here I transcode from 1080p to 720p.</p>
<p>-c:v libx264: The encoder you want to use (I strongly recommend to use libx264)</p>
<p>-preset: You can play around with this setting depending on your server. I use the faster preset and it works fine. You could try fastest (To use less CPU) or fast (To use more CPU).</p>
<p>-r, -g, -keyint_min 60, -x264opts: -r is your FPS. -g needs to be double your FPS (To have 2 seconds key-frame interval as requested by Twitch). -keyint_min is set to the same as my FPS. The content of -x264opts is to do the same thing about the key-frame interval.</p>
<p>-sws_flags: I use this to have a better down-scale quality.</p>
<p>-tune: I played around a few of this settings, and had the best experience with film you might need to play around this setting, but film is a safe choice.</p>
<p>-pix_fmt: This is the picture format, yuv420p seems to be the most supported/popular one.</p>
<p>-c:a: The audio codec is set as copy so it won’t re-encode the stream’s audio.</p>
<p>-threads: The number of CPU cores/threads you want to use. I have 8 on my server.
The output needs to be your liveout application with the right STREAM_KEY.</p>
