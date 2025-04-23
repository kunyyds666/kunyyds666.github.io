# EasyJava实现

# 1.创建PropertiesUtils类

创建utils包中的PropertiesUtils类用于读取application.properties文件

```java
package com.ks.utils;

import java.io.IOException;
import java.io.InputStream;
import java.util.Map;
import java.util.Properties;
import java.util.concurrent.ConcurrentHashMap;

public class PropertiesUtils {

    private static final Properties props = new Properties();

    private static final Map<String,String> PROPER_MAP = new ConcurrentHashMap<>();

    static {
        InputStream is = null;

        try{
            is = PropertiesUtils.class.getClassLoader().getResourceAsStream("application.properties");
            props.load(is);

            for (Object o : props.keySet()) {
                String key = (String) o;
                PROPER_MAP.put(key, props.getProperty(key));

            }


        }catch (Exception e){
            e.printStackTrace();

        }finally {
            if(is != null){
                try {
                    is.close();
                } catch (IOException e) {
                    throw new RuntimeException(e);
                }
            }

        }
    }
    public static String getString(String key){
        return PROPER_MAP.get(key);
    }

    public static void main(String[] args) {
        System.out.println(getString("db.driver.name"));

    }

}

```

# 2.创建BuildTable类

创建builder包中的BuildTable类用于读取数据库表







