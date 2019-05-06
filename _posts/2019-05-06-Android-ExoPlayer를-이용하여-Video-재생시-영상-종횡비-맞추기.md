---
layout: post
title: "Android ExoPlayer를 이용하여 Video 재생시 영상 종횡비 맞추기"
date: 2019-05-06
comments: true
---

# SimpleExoPlayerView를 활용한 Video의 종횡비 설정

ExoPlayer를 이용하여 Video를 재생시 직접 SurfaceView를 재생할 경우, 
Container Layout의 크기에 따라 SurfaceView크기가 결정되기 때문에 
Video Source의 종횡비가 맞지 않고 Stretch 되어 버립니다.

ExoPlayer를 이용하여 비디오를 재생할 경우에는 SimpleExoPlayerView를 이용하면,
setResizeMode에 AspectRatioFrameLayout.RESIZE_MODE_FIT로 설정하면
종횡비가 맞는 화면을 볼 수 있습니다.

~~~
SimpleExoPlayer exoPlayer = createSimpleExoPlayer(); 
SimpleExoPlayerView simpleExoPlayerView = findViewById(R.id.simple_exo_player_view);
simpleExoPlayerView.setPlayer(exoPlayer);
simpleExoPlayerView.setResizeMode(AspectRatioFrameLayout.RESIZE_MODE_FIT);
~~~

# SimpleExoPlayerView에서 resizeMode 선택시 내부 동작 

종횡비에 대한 설정은 내부적으로 AspectRatioFrameLayout이라는 ExoPlayer의 View클래스에 의해 동작됩니다.
Video Source로부터 bitmap을 얻어내어 해당 source의 종횡비를 얻어낸 후 
AspectRatioFrameLayout 내의 FrameLayout의 Size를 resize하여 그 안에 있는 surfaceView의 종횡비를 조절하게 됩니다.
크기를 변경하기 위해 중간에 FrameLayout을 invalidate하여 갱신하게 됩니다.
그 현상 때문에 일부기기에서는 layout이 invalidate 될 때 특정 조건에 따라 surfaceView에 video가 재생되지 않는 문제가 발생할 때도 있습니다.
그런때에는 surfaceType의 설정을 textureView로 변경해보시기 바랍니다.
Exo Player에서 surfaceType의 권고 사항은 surfaceView를 사용하는 것이지만, 
video view를 가공해야하는 경우에는 texture view로 변경해주어 해결 할 수 있습니다.
textureView를 사용함으로써 생기는 단점에 대해서는 exo player 공식 블로그를 참조하시길 바랍니다.

[Exo Player Q&A](https://exoplayer.dev/faqs.html#should-i-use-surfaceview-or-textureview)

# SimpleExoPlayerView의 기타 기능

뿐만아니라, SimpleExoPlayerView에는 video controller를 기본지원하며, 
thumbnail, surfaceView 설정 (TextureView 혹은 SurfaceView) 등을 지원하기에
ExoPlayer 사용시 SimpleExoPlayerView와 함께 사용하기를 권장합니다.
그리고 SimpleExoPlayerView에 대한 설정은 javaCode 뿐만 아니라, xml에서 attribute로 설정 가능합니다.

~~~
<com.google.android.exoplayer2.ui.SimpleExoPlayerView
    android:id="@+id/vod_player_simple_exo_player_view"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    app:resize_mode="fit"
    app:use_controller="true"
    app:surface_type="texture_view"
    app:default_artwork="@drawable/image_backaground">
</com.google.android.exoplayer2.ui.SimpleExoPlayerView>
~~~



