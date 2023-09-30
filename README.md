# ffmpeg
Get the hls format

```
ffmpeg -i hey.mp4 \
-filter_complex \
"[0:v]split=5[v1][v2][v3][v4][v5]; \
[v1]copy[v1out]; \
[v2]scale=w=1280:h=720[v2out]; \
[v3]scale=w=854:h=480[v3out]; \
[v4]scale=w=640:h=360[v4out]; \
[v5]scale=w=426:h=240[v5out]" \
-map "[v1out]" -c:v:0 libx264 -x264-params "nal-hrd=cbr:force-cfr=1" -b:v:0 5M -maxrate:v:0 5M -minrate:v:0 5M -bufsize:v:0 10M -preset slow -g 48 -sc_threshold 0 -keyint_min 48 \
-map "[v2out]" -c:v:1 libx264 -x264-params "nal-hrd=cbr:force-cfr=1" -b:v:1 3M -maxrate:v:1 3M -minrate:v:1 3M -bufsize:v:1 3M -preset slow -g 48 -sc_threshold 0 -keyint_min 48 \
-map "[v3out]" -c:v:2 libx264 -x264-params "nal-hrd=cbr:force-cfr=1" -b:v:2 1M -maxrate:v:2 1M -minrate:v:2 1M -bufsize:v:2 1M -preset slow -g 48 -sc_threshold 0 -keyint_min 48 \
-map "[v4out]" -c:v:3 libx264 -x264-params "nal-hrd=cbr:force-cfr=1" -b:v:3 500k -maxrate:v:3 500k -minrate:v:3 500k -bufsize:v:3 500k -preset slow -g 48 -sc_threshold 0 -keyint_min 48 \
-map "[v5out]" -c:v:4 libx264 -x264-params "nal-hrd=cbr:force-cfr=1" -b:v:4 300k -maxrate:v:4 300k -minrate:v:4 300k -bufsize:v:4 300k -preset slow -g 48 -sc_threshold 0 -keyint_min 48 \
-map a:0 -c:a:0 aac -b:a:0 96k -ac 2 \
-map a:0 -c:a:1 aac -b:a:1 96k -ac 2 \
-map a:0 -c:a:2 aac -b:a:2 48k -ac 2 \
-map a:0 -c:a:3 aac -b:a:3 24k -ac 1 \
-map a:0 -c:a:4 aac -b:a:4 24k -ac 1 \
-f hls \
-hls_time 2 \
-hls_playlist_type vod \
-hls_flags independent_segments \
-hls_segment_type mpegts \
-hls_segment_filename stream_%v/data%02d.ts \
-master_pl_name master.m3u8 \
-var_stream_map "v:0,a:0 v:1,a:1 v:2,a:2 v:3,a:3 v:4,a:4" stream_%v.m3u8
```
