## Text View内のurl等をリンクにする

```
import android.text.util.Linkify;

testView.setAutoLinkMask(Linkify.ALL);
```


## android studio gradle ファイルをDLする
app/build.gradle

```
ext.download_files = [
        'https://example.com' : 'app/libs/Example_SDK_1.0.0.aar',
]

task downloadMultipleFiles {
    doLast {
        for (s in download_files) {
            def f = new File(rootDir, s.value)
            if (!f.exists()) {
                new URL(s.key).withInputStream{ i -> f.withOutputStream{ it << i }}
            }
        }
    }
}
preBuild.dependsOn(downloadMultipleFiles)
```
