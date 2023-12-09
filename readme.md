# Shaka Player - Offline Player

This app can download a video to local indexedDB from remote mpd link and play from local.



## Read docs to convert mp4 video to mpd format

 - [DYNAMIC ADAPTIVE STREAMING OVER HTTP (MPEG-DASH) FOR EVERYONE](https://rybakov.com/blog/mpeg-dash/)


## Commands to convert video files to mpd

Convert the source file called input.mov into an audio file and two video

```bash
    ffmpeg -y -i "input.mov" -c:a aac -b:a 192k -vn "output_audio.m4a"

    ffmpeg -y -i "input.mov" -preset slow -tune film -vsync passthrough -an -c:v libx264 -x264opts 'keyint=25:min-keyint=25:no-scenecut' -crf 22  -maxrate 5000k -bufsize 10000k -pix_fmt yuv420p -f mp4 "output_5000.mp4"

    ffmpeg -y -i "input.mov" -preset slow -tune film -vsync passthrough -an -c:v libx264 -x264opts 'keyint=25:min-keyint=25:no-scenecut' -crf 23  -maxrate 2000k -bufsize 4000k -pix_fmt yuv420p -f mp4 "output_2000.mp4"
```

Now that the files are created, we can make a manifest file:
```bash
    MP4Box -dash 2000 -rap -frag-rap  -bs-switching no -profile "dashavc264:live" "output_5000.mp4" "output_3000.mp4"  "output_audio.m4a" -out "output/output.mpd"
```

## Run index file via live server


## Essential Packages to convert into mpd

- [gpac](https://gpac.wp.imt.fr/downloads/)
- [ffmpeg](https://ffmpeg.org/download.html)

<!-- Convert -->
```ffmpeg -y -i "input/bigbuckbunnyclip.mp4" -c:a aac -b:a 192k -vn "output_bigbuckbunnyclip_audio.m4a" -preset slow -tune film -vsync passthrough -an -c:v libx264 -x264opts 'keyint=25:min-keyint=25:no-scenecut' -crf 22  -maxrate 5000k -bufsize 10000k -pix_fmt yuv420p -f mp4 "output_bigbuckbunnyclip_5000.mp4" && MP4Box -dash 2000 -rap -frag-rap  -bs-switching no -profile "dashavc264:live" "output_bigbuckbunnyclip_5000.mp4" "output_bigbuckbunnyclip_audio.m4a" -out "output/output_bigbuckbunnyclip/output_bigbuckbunnyclip.mpd" && rm "output_bigbuckbunnyclip_audio.m4a" && rm "output_bigbuckbunnyclip_5000.mp4"```

<!-- aws upload -->

```aws s3 cp "/Users/metadesign/Desktop/projects/learning/shaka-player-offline-download-js/output/output_bigbuckbunnyclip" "s3://syportalclpepisodebucket/output_bigbuckbunnyclip" --acl public-read --recursive```

<!-- Link will be -->

```https://syportalclpepisodebucket.s3.ap-south-1.amazonaws.com/output_bigbuckbunnyclip/output_bigbuckbunnyclip.mpd```


ffmpeg -i input/bigbuckbunnyclip.mp4 -vf "scale=854:480" -c:v libx264 -crf 23 -c:a aac -b:a 128k bigbuckbunnyclip_480p.mp4
