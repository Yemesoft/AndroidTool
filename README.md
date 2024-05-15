

由于360多渠道打包工具开始收费了，所以做了个小工具免费给大家使用。
比360打包更快，零代码侵入。

欢迎加入QQ交流群：601971480

渠道配置文件和360渠道配置文件一样。

渠道名获取方法：
```

public class ChannelUtil {
    private static final String CHANNEL_FILE_PREFIX = "_app_channel_";

    public static String getChannelName(Context context) {
        String channelName = null;
        try {
            String zipPath = context.getPackageResourcePath();
            ZipFile zipFile = new ZipFile(zipPath);
            Enumeration<? extends ZipEntry> entries = zipFile.entries();
            while (entries.hasMoreElements()) {
                ZipEntry entry = entries.nextElement();
                if (entry.getName().startsWith(CHANNEL_FILE_PREFIX)) {
                    channelName = entry.getName().substring(CHANNEL_FILE_PREFIX.length());
                    break;
                }
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
        if (null == channelName) {
            try {
                //兼容友盟
                ApplicationInfo applicationInfo = context.getPackageManager().getApplicationInfo(context.getPackageName(), PackageManager.GET_META_DATA);
                channelName = applicationInfo.metaData.getString("UMENG_CHANNEL");
            } catch (PackageManager.NameNotFoundException e) {
                e.printStackTrace();
            }
        }
        System.err.println("channelName=" + channelName);
        if (null == channelName) {
            channelName = "normal";
        }
        return channelName;
    }

}



```
