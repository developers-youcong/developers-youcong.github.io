---
title: Java之数据字典实现
date: 2020-11-30 20:15:00
tags: "Java"
---

**数据字典核心代码实现:**
<!--more-->
```
@Component
public class DictMap {
    @Autowired
    private SysDictDataMapper dictDataMapper;

    private static HashMap<String, String> hashMap = new HashMap<>();

    public static DictMap dictMap;

    /**
     * 从数据库中取值放入到HashMap中(存储字典)
     */
    @PostConstruct
    public void queryDic() {

        dictMap = this;
        dictMap.dictDataMapper = this.dictDataMapper;

        System.out.println("初始化");

        List<SysDictData> dics = dictMap.dictDataMapper.selectDictDataAll();

        for (int i = 0; i < dics.size(); i++) {
            SysDictData dic = dics.get(i);

            String fieldName = dic.getDictType();
            String fieldValue = dic.getDictValue();
            String key = fieldName + "_" + fieldValue;
            String value = dic.getDictLabel();
            System.out.println(key + "=" + value);
            hashMap.put(key, value);
        }
    }

    /**
     * 获取字典
     *
     * @param fieldName
     * @param fieldValue
     * @return
     */
    public static String getFieldDetail(String fieldName, String fieldValue) {
        StringBuilder sb = new StringBuilder();
        StringBuilder keySb = sb.append(fieldName).append("_").append(fieldValue);
        String key = keySb.toString();
        String value = hashMap.get(key);
        return value;
    }
}



```

**代码引用:**
```
@Data
public class UserVo implements Serializable {


    private Long userId;

    private String sex;

    public String getSex() {
        return sex = DictMap.getFieldDetail("sex_type", sex);
    }
}


```