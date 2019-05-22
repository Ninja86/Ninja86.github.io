---
title: "[Android] Exoplayer에서 Youtube 같은 ripple effect animation 구현하기"
date: 2019-05-06
comments: true
---

유투브에서 전, 후 영상 재생 탐색 할 때, 반타원형태의 ripple effect효과가 나오는 걸 볼 수 있습니다.
보통 우리가 ripple effect를 UI에 넣고자 할때
**android:background="?attr/selectableItemBackground"**
이런식으로 많이 구현하게 됩니다.

~~~
<androidx.constraintlayout.widget.ConstraintLayout
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="?attr/selectableItemBackground"
    android:clickable="true">
</androidx.constraintlayout.widget.ConstraintLayout>
~~~

하지만 이렇게 되면 layout 형태와 같은 사각형일 수 밖에 없기 때문에, 
위와같은 방식으로는 우리가 하고자 하는 반타원 형태에서 ripple effect를 표현 할 수 없습니다.

사각형 이외의 형태에서 구현하려면 xml에 drawable resource를 생성해서 구현하면 됩니다.

~~~
<?xml version="1.0" encoding="utf-8"?>
    <ripple xmlns:android="http://schemas.android.com/apk/res/android"
    android:color="?android:attr/colorControlHighlight">
        <item
            android:id="@android:id/mask"
            android:left="-100dp"
            android:top="-100dp"
            android:bottom="-100dp">

            <shape android:shape="oval">
                <solid android:color="#000000" />
            </shape>
        </item>
</ripple>
~~~

ripple tag로 선언해 주신 다음에 item tag를 **@android:id/mask** 를 선언하면,
item 내의 child tag들만 ripple effect가 적용되게 됩니다.
shape tag로 원하는 형태의 shape을 선언 해줍니다. 여기서는 oval로 하였습니다.
그리고 ripple tag에서 **android:color="<color>"** 를 지정하면 
ripple effect 발생할 때의 색을 지정할 수 있습니다.
여기서는 android:attr/colorControlHighlight로 지정했는데 
안드로이드에서 기본적으로 제공해주는 material design용 highlight color 입니다.

item에 **left, right, top, bottom** 에 값을 주면 item의 anchor point를 이동시킬 수가 있습니다.
위와 같이 left, top, bottom에 마이너스 값을 주면 item의 크기가 부모 layout보다 커지게 되어 그려지게 됩니다.

![layout-example](https://raw.githubusercontent.com/Ninja86/Ninja86.github.io/master/assets/article_images/2019-05-07-Android-Youtube-Player-Style-prev-rewind-%E1%84%80%E1%85%AE%E1%84%92%E1%85%A7%E1%86%AB%E1%84%92%E1%85%A1%E1%84%80%E1%85%B5/github-blog.png)

이런식으로 반타원 형태의 ripple effect가 발생하는 drawable resource를 만들 수 있습니다.

