# mui-app-weather

## 项目描述

使用MUI布局平板HD的天气信息界面

* 基础布局Flex
* mui.min.js
* vue.js视图渲染

## 天气信息界面

![image.png](https://i.loli.net/2018/12/29/5c27193a241e9.png) 

**天气接口**

中国天气网基础API

```
http://www.weather.com.cn/data/sk/101010100.html
```

```json
{
    "weatherinfo": {
        "city": "北京",
        "cityid": "101010100",
        "temp": "27.9",
        "WD": "南风",
        "WS": "小于3级",
        "SD": "28%",
        "AP": "1002hPa",
        "njd": "暂无实况",
        "WSE": "<3",
        "time": "17:55",
        "sm": "2.1",
        "isRadar": "1",
        "Radar": "JC_RADAR_AZ9010_JB"
    }
}
```


中国天气网实时天气API

```
http://www.weather.com.cn/data/cityinfo/101010100.html
```

```json
{
    "weatherinfo": {
        "city": "北京",
        "cityid": "101010100",
        "temp1": "18℃",
        "temp2": "31℃",
        "weather": "多云转阴",
        "img1": "n1.gif",
        "img2": "d2.gif",
        "ptime": "18:00"
    }
}
```

通过城市名字获得天气数据，json数据

```
http://wthrcdn.etouch.cn/weather_mini?city=广州
```

```json
{
    "data": {
        "yesterday": {
            "date": "28日星期五",
            "high": "高温 18℃",
            "fx": "北风",
            "low": "低温 7℃",
            "fl": "<![CDATA[3-4级]]>",
            "type": "多云"
        },
        "city": "广州",
        "aqi": "49",
        "forecast": [
            {
                "date": "29日星期六",
                "high": "高温 12℃",
                "fengli": "<![CDATA[4-5级]]>",
                "low": "低温 5℃",
                "fengxiang": "北风",
                "type": "阴"
            },
            {
                "date": "30日星期天",
                "high": "高温 11℃",
                "fengli": "<![CDATA[4-5级]]>",
                "low": "低温 5℃",
                "fengxiang": "北风",
                "type": "阴"
            },
            {
                "date": "31日星期一",
                "high": "高温 11℃",
                "fengli": "<![CDATA[4-5级]]>",
                "low": "低温 6℃",
                "fengxiang": "北风",
                "type": "小雨"
            },
            {
                "date": "1日星期二",
                "high": "高温 11℃",
                "fengli": "<![CDATA[4-5级]]>",
                "low": "低温 6℃",
                "fengxiang": "北风",
                "type": "小雨"
            },
            {
                "date": "2日星期三",
                "high": "高温 10℃",
                "fengli": "<![CDATA[4-5级]]>",
                "low": "低温 7℃",
                "fengxiang": "北风",
                "type": "小雨"
            }
        ],
        "ganmao": "将有一次强降温过程，天气寒冷，且风力较强，极易发生感冒，请特别注意增加衣服保暖防寒。",
        "wendu": "10"
    },
    "status": 1000,
    "desc": "OK"
}
```

通过城市id获得天气数据，json数据

```
http://wthrcdn.etouch.cn/weather_mini?citykey=101010100
```

```json
{
    "data": {
        "yesterday": {
            "date": "28日星期五",
            "high": "高温 -4℃",
            "fx": "西北风",
            "low": "低温 -11℃",
            "fl": "<![CDATA[3-4级]]>",
            "type": "晴"
        },
        "city": "北京",
        "aqi": "109",
        "forecast": [
            {
                "date": "29日星期六",
                "high": "高温 -3℃",
                "fengli": "<![CDATA[<3级]]>",
                "low": "低温 -12℃",
                "fengxiang": "北风",
                "type": "晴"
            },
            {
                "date": "30日星期天",
                "high": "高温 -2℃",
                "fengli": "<![CDATA[<3级]]>",
                "low": "低温 -11℃",
                "fengxiang": "北风",
                "type": "晴"
            },
            {
                "date": "31日星期一",
                "high": "高温 -2℃",
                "fengli": "<![CDATA[<3级]]>",
                "low": "低温 -9℃",
                "fengxiang": "南风",
                "type": "多云"
            },
            {
                "date": "1日星期二",
                "high": "高温 1℃",
                "fengli": "<![CDATA[<3级]]>",
                "low": "低温 -9℃",
                "fengxiang": "北风",
                "type": "晴"
            },
            {
                "date": "2日星期三",
                "high": "高温 2℃",
                "fengli": "<![CDATA[<3级]]>",
                "low": "低温 -9℃",
                "fengxiang": "南风",
                "type": "晴"
            }
        ],
        "ganmao": "昼夜温差很大，易发生感冒，请注意适当增减衣服，加强自我防护避免感冒。",
        "wendu": "-2"
    },
    "status": 1000,
    "desc": "OK"
}
```

通过城市id获得天气数据，xml文件数据

```
http://wthrcdn.etouch.cn/WeatherApi?citykey=101010100
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<resp>
    <city>北京</city>
    <updatetime>09:49</updatetime>
    <wendu>-5</wendu>
    <fengli>
        <![CDATA[2级]]>
    </fengli>
    <shidu>12%</shidu>
    <fengxiang>北风</fengxiang>
    <sunrise_1>07:35</sunrise_1>
    <sunset_1>16:58</sunset_1>
    <sunrise_2></sunrise_2>
    <sunset_2></sunset_2>
    <environment>
        <aqi>109</aqi>
        <pm25>81</pm25>
        <suggest>儿童、老年人及心脏、呼吸系统疾病患者人群应减少长时间或高强度户外锻炼</suggest>
        <quality>轻度污染</quality>
        <MajorPollutants>颗粒物(PM2.5)</MajorPollutants>
        <o3>12</o3>
        <co>1</co>
        <pm10>133</pm10>
        <so2>11</so2>
        <no2>79</no2>
        <time>19:00:00</time>
    </environment>
    <yesterday>
        <date_1>28日星期五</date_1>
        <high_1>高温 -4℃</high_1>
        <low_1>低温 -11℃</low_1>
        <day_1>
            <type_1>晴</type_1>
            <fx_1>西北风</fx_1>
            <fl_1>
                <![CDATA[3-4级]]>
            </fl_1>
        </day_1>
        <night_1>
            <type_1>晴</type_1>
            <fx_1>北风</fx_1>
            <fl_1>
                <![CDATA[<3级]]>
            </fl_1>
        </night_1>
    </yesterday>
    <forecast>
        <weather>
            <date>29日星期六</date>
            <high>高温 -3℃</high>
            <low>低温 -12℃</low>
            <day>
                <type>晴</type>
                <fengxiang>北风</fengxiang>
                <fengli>
                    <![CDATA[<3级]]>
                </fengli>
            </day>
            <night>
                <type>晴</type>
                <fengxiang>东北风</fengxiang>
                <fengli>
                    <![CDATA[<3级]]>
                </fengli>
            </night>
        </weather>
        <weather>
            <date>30日星期天</date>
            <high>高温 -2℃</high>
            <low>低温 -11℃</low>
            <day>
                <type>晴</type>
                <fengxiang>北风</fengxiang>
                <fengli>
                    <![CDATA[<3级]]>
                </fengli>
            </day>
            <night>
                <type>多云</type>
                <fengxiang>东南风</fengxiang>
                <fengli>
                    <![CDATA[<3级]]>
                </fengli>
            </night>
        </weather>
        <weather>
            <date>31日星期一</date>
            <high>高温 -2℃</high>
            <low>低温 -9℃</low>
            <day>
                <type>多云</type>
                <fengxiang>南风</fengxiang>
                <fengli>
                    <![CDATA[<3级]]>
                </fengli>
            </day>
            <night>
                <type>多云</type>
                <fengxiang>西北风</fengxiang>
                <fengli>
                    <![CDATA[<3级]]>
                </fengli>
            </night>
        </weather>
        <weather>
            <date>1日星期二</date>
            <high>高温 1℃</high>
            <low>低温 -8℃</low>
            <day>
                <type>晴</type>
                <fengxiang>北风</fengxiang>
                <fengli>
                    <![CDATA[<3级]]>
                </fengli>
            </day>
            <night>
                <type>晴</type>
                <fengxiang>北风</fengxiang>
                <fengli>
                    <![CDATA[<3级]]>
                </fengli>
            </night>
        </weather>
        <weather>
            <date>2日星期三</date>
            <high>高温 2℃</high>
            <low>低温 -9℃</low>
            <day>
                <type>晴</type>
                <fengxiang>西南风</fengxiang>
                <fengli>
                    <![CDATA[<3级]]>
                </fengli>
            </day>
            <night>
                <type>晴</type>
                <fengxiang>北风</fengxiang>
                <fengli>
                    <![CDATA[<3级]]>
                </fengli>
            </night>
        </weather>
    </forecast>
    <zhishus>
        <zhishu>
            <name>晨练指数</name>
            <value>不宜</value>
            <detail>早晨天气寒冷，风力稍大，请尽量避免户外晨练，若坚持晨练请注意保暖避风防冻，建议年老体弱人群适当减少晨练时间。</detail>
        </zhishu>
        <zhishu>
            <name>舒适度</name>
            <value>较不舒适</value>
            <detail>白天天气晴好，但仍会使您感觉偏冷，不很舒适，请注意适时添加衣物，以防感冒。</detail>
        </zhishu>
        <zhishu>
            <name>穿衣指数</name>
            <value>寒冷</value>
            <detail>天气寒冷，建议着厚羽绒服、毛皮大衣加厚毛衣等隆冬服装。年老体弱者尤其要注意保暖防冻。</detail>
        </zhishu>
        <zhishu>
            <name>感冒指数</name>
            <value>易发</value>
            <detail>昼夜温差很大，易发生感冒，请注意适当增减衣服，加强自我防护避免感冒。</detail>
        </zhishu>
        <zhishu>
            <name>晾晒指数</name>
            <value>基本适宜</value>
            <detail>天气不错，午后温暖的阳光仍能满足你驱潮消霉杀菌的晾晒需求。</detail>
        </zhishu>
        <zhishu>
            <name>旅游指数</name>
            <value>较适宜</value>
            <detail>天气较好，同时又有微风伴您一路同行。稍冷，较适宜旅游，您仍可陶醉于大自然的美丽风光中。</detail>
        </zhishu>
        <zhishu>
            <name>紫外线强度</name>
            <value>弱</value>
            <detail>紫外线强度较弱，建议出门前涂擦SPF在12-15之间、PA+的防晒护肤品。</detail>
        </zhishu>
        <zhishu>
            <name>洗车指数</name>
            <value>较适宜</value>
            <detail>较适宜洗车，未来一天无雨，风力较小，擦洗一新的汽车至少能保持一天。</detail>
        </zhishu>
        <zhishu>
            <name>运动指数</name>
            <value>较不宜</value>
            <detail>天气较好，但考虑天气寒冷，推荐您进行室内运动，户外运动时请注意保暖并做好准备活动。</detail>
        </zhishu>
        <zhishu>
            <name>约会指数</name>
            <value>较不适宜</value>
            <detail>天气较冷，且室外有风，外出约会可能会让恋人受些苦，最好在温暖的室内促膝谈心。</detail>
        </zhishu>
        <zhishu>
            <name>雨伞指数</name>
            <value>不带伞</value>
            <detail>天气较好，您在出门的时候无须带雨伞。</detail>
        </zhishu>
    </zhishus>
</resp>
<!-- 10.42.121.88(10.42.121.88):43763 ; 10.42.161.121:8080 -->
```

通过城市名字获得天气数据，xml文件数据

```
http://wthrcdn.etouch.cn/WeatherApi?city=北京
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<resp>
    <city>北京</city>
    <updatetime>14:28</updatetime>
    <wendu>-2</wendu>
    <fengli>
        <![CDATA[2级]]>
    </fengli>
    <shidu>13%</shidu>
    <fengxiang>西北风</fengxiang>
    <sunrise_1>07:35</sunrise_1>
    <sunset_1>16:58</sunset_1>
    <sunrise_2></sunrise_2>
    <sunset_2></sunset_2>
    <environment>
        <aqi>109</aqi>
        <pm25>81</pm25>
        <suggest>儿童、老年人及心脏、呼吸系统疾病患者人群应减少长时间或高强度户外锻炼</suggest>
        <quality>轻度污染</quality>
        <MajorPollutants>颗粒物(PM2.5)</MajorPollutants>
        <o3>12</o3>
        <co>1</co>
        <pm10>133</pm10>
        <so2>11</so2>
        <no2>79</no2>
        <time>19:00:00</time>
    </environment>
    <yesterday>
        <date_1>28日星期五</date_1>
        <high_1>高温 -4℃</high_1>
        <low_1>低温 -11℃</low_1>
        <day_1>
            <type_1>晴</type_1>
            <fx_1>西北风</fx_1>
            <fl_1>
                <![CDATA[3-4级]]>
            </fl_1>
        </day_1>
        <night_1>
            <type_1>晴</type_1>
            <fx_1>北风</fx_1>
            <fl_1>
                <![CDATA[<3级]]>
            </fl_1>
        </night_1>
    </yesterday>
    <forecast>
        <weather>
            <date>29日星期六</date>
            <high>高温 -3℃</high>
            <low>低温 -12℃</low>
            <day>
                <type>晴</type>
                <fengxiang>北风</fengxiang>
                <fengli>
                    <![CDATA[<3级]]>
                </fengli>
            </day>
            <night>
                <type>晴</type>
                <fengxiang>东北风</fengxiang>
                <fengli>
                    <![CDATA[<3级]]>
                </fengli>
            </night>
        </weather>
        <weather>
            <date>30日星期天</date>
            <high>高温 -2℃</high>
            <low>低温 -11℃</low>
            <day>
                <type>晴</type>
                <fengxiang>北风</fengxiang>
                <fengli>
                    <![CDATA[<3级]]>
                </fengli>
            </day>
            <night>
                <type>多云</type>
                <fengxiang>东南风</fengxiang>
                <fengli>
                    <![CDATA[<3级]]>
                </fengli>
            </night>
        </weather>
        <weather>
            <date>31日星期一</date>
            <high>高温 -2℃</high>
            <low>低温 -9℃</low>
            <day>
                <type>多云</type>
                <fengxiang>南风</fengxiang>
                <fengli>
                    <![CDATA[<3级]]>
                </fengli>
            </day>
            <night>
                <type>多云</type>
                <fengxiang>西北风</fengxiang>
                <fengli>
                    <![CDATA[<3级]]>
                </fengli>
            </night>
        </weather>
        <weather>
            <date>1日星期二</date>
            <high>高温 1℃</high>
            <low>低温 -9℃</low>
            <day>
                <type>晴</type>
                <fengxiang>北风</fengxiang>
                <fengli>
                    <![CDATA[<3级]]>
                </fengli>
            </day>
            <night>
                <type>晴</type>
                <fengxiang>北风</fengxiang>
                <fengli>
                    <![CDATA[<3级]]>
                </fengli>
            </night>
        </weather>
        <weather>
            <date>2日星期三</date>
            <high>高温 2℃</high>
            <low>低温 -9℃</low>
            <day>
                <type>晴</type>
                <fengxiang>南风</fengxiang>
                <fengli>
                    <![CDATA[<3级]]>
                </fengli>
            </day>
            <night>
                <type>晴</type>
                <fengxiang>北风</fengxiang>
                <fengli>
                    <![CDATA[<3级]]>
                </fengli>
            </night>
        </weather>
    </forecast>
    <zhishus>
        <zhishu>
            <name>晨练指数</name>
            <value>不宜</value>
            <detail>早晨天气寒冷，请尽量避免户外晨练，若坚持室外晨练请注意保暖防冻，建议年老体弱人群适当减少晨练时间。</detail>
        </zhishu>
        <zhishu>
            <name>舒适度</name>
            <value>较不舒适</value>
            <detail>白天天气晴好，但仍会使您感觉偏冷，不很舒适，请注意适时添加衣物，以防感冒。</detail>
        </zhishu>
        <zhishu>
            <name>穿衣指数</name>
            <value>寒冷</value>
            <detail>天气寒冷，建议着厚羽绒服、毛皮大衣加厚毛衣等隆冬服装。年老体弱者尤其要注意保暖防冻。</detail>
        </zhishu>
        <zhishu>
            <name>感冒指数</name>
            <value>易发</value>
            <detail>昼夜温差很大，易发生感冒，请注意适当增减衣服，加强自我防护避免感冒。</detail>
        </zhishu>
        <zhishu>
            <name>晾晒指数</name>
            <value>基本适宜</value>
            <detail>天气不错，午后温暖的阳光仍能满足你驱潮消霉杀菌的晾晒需求。</detail>
        </zhishu>
        <zhishu>
            <name>旅游指数</name>
            <value>较适宜</value>
            <detail>天气较好，同时又有微风伴您一路同行。稍冷，较适宜旅游，您仍可陶醉于大自然的美丽风光中。</detail>
        </zhishu>
        <zhishu>
            <name>紫外线强度</name>
            <value>弱</value>
            <detail>紫外线强度较弱，建议出门前涂擦SPF在12-15之间、PA+的防晒护肤品。</detail>
        </zhishu>
        <zhishu>
            <name>洗车指数</name>
            <value>较适宜</value>
            <detail>较适宜洗车，未来一天无雨，风力较小，擦洗一新的汽车至少能保持一天。</detail>
        </zhishu>
        <zhishu>
            <name>运动指数</name>
            <value>较不宜</value>
            <detail>天气较好，但考虑天气寒冷，推荐您进行室内运动，户外运动时请注意保暖并做好准备活动。</detail>
        </zhishu>
        <zhishu>
            <name>约会指数</name>
            <value>较不适宜</value>
            <detail>天气较冷，且室外有风，外出约会可能会让恋人受些苦，最好在温暖的室内促膝谈心。</detail>
        </zhishu>
        <zhishu>
            <name>雨伞指数</name>
            <value>不带伞</value>
            <detail>天气较好，您在出门的时候无须带雨伞。</detail>
        </zhishu>
    </zhishus>
</resp>
<!-- 10.42.121.88(10.42.121.88):44529 ; 10.42.121.88:8080 -->
```