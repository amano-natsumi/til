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


## location(LocationManager 使用)
```

        if (ActivityCompat.checkSelfPermission(this, Manifest.permission.ACCESS_FINE_LOCATION) != PackageManager.PERMISSION_GRANTED
                && ActivityCompat.checkSelfPermission(this, Manifest.permission.ACCESS_COARSE_LOCATION) != PackageManager.PERMISSION_GRANTED) {
            Log.d(TAG, "checkSelfPermission ");
            // パーミッションの許可を取得する
            ActivityCompat.requestPermissions(this,
                    new String[]{Manifest.permission.ACCESS_FINE_LOCATION, Manifest.permission.ACCESS_COARSE_LOCATION}, 1000);
        }

        // 使用できるproviderを見る
        LocationManager locationManager = (LocationManager) getSystemService(LOCATION_SERVICE);
        Criteria criteria = new Criteria();
        criteria.setAccuracy(Criteria.ACCURACY_FINE);
        criteria.setCostAllowed(false);

        String providerName = locationManager.getBestProvider(criteria, true);
        Log.d(TAG, "getBestProvider " + providerName);

        List<String> providers = locationManager.getAllProviders();
        for (int i = 0; i < providers.size(); i++) {
            Log.d(TAG, "getAllProviders " + providers.get(i));
        }

        LocationListener listener = new LocationListener() {

            @Override
            public void onLocationChanged(Location location) {

                Log.d(TAG, "onLocationChanged");

            }
            @Override
            public void onStatusChanged(String provider, int status, Bundle extras) {
                Log.d(TAG, "onStatusChanged");
            }
            @Override
            public void onProviderEnabled(String provider) {
                Log.d(TAG, "onProviderEnabled");
            }
            @Override
            public void onProviderDisabled(String provider) {
                Log.d(TAG, "onProviderEnabled");
            }


        };
        
        locationManager.requestLocationUpdates(providerName,
                10000,          // 10-second interval.
                10,             // 10 meters.
                listener);

```
