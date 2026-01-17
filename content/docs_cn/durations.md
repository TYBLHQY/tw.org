---
title: "Taskwarrior - 时间段"
---

# 时间段

Taskwarrior 支持时间段值。
在核心中，时间段直接用于一个地方 - `recur` 属性，用于循环任务。
例如，这是一个每周一循环的任务：

    $ task add Take out the trash due:monday recur:weekly

值 `monday` 被解释为"下一个星期一"，值 `weekly` 被解释为 7 天的时间段。

直接支持时间段的另一个地方是使用 [UDA 属性](../udas/) 的 `duration` 类型。

## 相对日期

在任何期望日期值的地方，都支持间接的时间段支持。
例如，这是一个在两天内到期的任务：

    $ task add Birthday party due:2days

每当 Taskwarrior 期望一个日期（在本例中是到期日期）但找到时间段时，它被解释为相对时间段加到当前日期/时间，在本例中为 `now + 2days`。

## 时间段格式

* `5 seconds`                            五秒。
* `5 second`
* `5 secs`
* `5 sec`
* `5 s`
* `5seconds`
* `5second`
* `5secs`
* `5sec`
* `5s`
* `second`                               一秒。缺少序数意味着一秒。
* `sec`
* `5 minutes`                            五分钟。
* `5 minute`
* `5 mins`
* `5 min`
* `5minutes`
* `5minute`
* `5mins`
* `5min`
* `minute`                               一分钟。缺少序数意味着一分钟。
* `min`
* `3 hours`                              三小时。
* `3 hour`
* `3 hrs`
* `3 hr`
* `3 h`
* `3hours`
* `3hour`
* `3hrs`
* `3hr`
* `3h`
* `hour`                                 一小时。缺少序数意味着一小时。
* `hr`
* `2 days`                               两天。
* `2 day`
* `2 d`
* `2days`
* `2day`
* `2d`
* `daily`                                一天。缺少序数意味着一天。
* `day`
* `3 weeks`                              三周。
* `3 week`
* `3 wks`
* `3 wk`
* `3 w`
* `3weeks`
* `3week`
* `3wks`
* `3wk`
* `3w`
* `weekly`                               一周。缺少序数意味着一周。
* `week`
* `wk`
* `weekdays`                             每个工作日，周一至周五。
* `2 fortnight`                          二十八天。
* `2 sennight`
* `2fortnight`
* `2sennight`
* `biweekly`                             十四天。缺少序数意味着十四天。
* `fortnight`
* `sennight`
* `5 months`                             五个月。
* `5 month`
* `5 mnths`
* `5 mths`
* `5 mth`
* `5 mo`
* `5 m`
* `5months`
* `5month`
* `5mnths`
* `5mths`
* `5mth`
* `5mo`
* `5m`
* `monthly`                              三十天。缺少序数意味着一
* `month`                                个月。这是一个不精确的值。
* `mth`
* `mo`
* `bimonthly`                            六十一天。缺少序数意味着两个月。这是一个不精确的值。
* `1 quarterly`                          九十一天。
* `1 quarters`
* `1 quarter`
* `1 qrtrs`
* `1 qrtr`
* `1 qtr`
* `1 q`
* `1quarterly`
* `1quarters`
* `1quarter`
* `1qrtrs`
* `1qrtr`
* `1qtr`
* `1q`
* `quarterly`                            九十一天。缺少序数意味着三个月。这是一个不精确的值。
* `quarter`
* `qrtr`
* `qtr`
* `semiannual`                           一百八十三天。缺少序数意味着六个月。这是一个不精确的值。
* `1 years`                              一年。
* `1 year`
* `1 yrs`
* `1 yr`
* `1 y`
* `1years`
* `1year`
* `1yrs`
* `1yr`
* `1y`
* `annual`                               三百六十五天。缺少序数意味着一年。这是一个不精确的值。
* `yearly`
* `year`
* `yr`
* `biannual`                             七百三十天。缺少序数意味着两年。这是一个不精确的值。
* `biyearly`

## ISO-8601 格式

ISO-8601 定义了 Taskwarrior 支持的时间段格式。
格式如下所示：

    P[nY][nM][nD][T[nH][nM][nS]]

格式始终以"P"（周期）开头。
日期元素（`[nY][nM][nD]`）是可选的，时间元素（`[nH][nM][nS]`）也是可选的，但必须指定一个元素。

虽然月份和分钟值使用字符"M"，但由于字符"T"的放置（它将日期和时间元素分开），所以没有歧义。

这是一个示例列表：

* `P3D`               - 三天。
* `P1000D`            - 一千天。
* `P2M`               - 两个月。这是一个不精确的值。
* `P2M3D`             - 两个月零三天。这是一个不精确的值。
* `P1Y`               - 一年。这是一个不精确的值。
* `P1Y3D`             - 一年零三天。这是一个不精确的值。
* `P1Y2M`             - 一年零两个月。这是一个不精确的值。
* `P1Y2M3D`           - 一年两个月零三天。这是一个不精确的值。
* `PT50S`             - 五十秒。
* `PT40M`             - 四十分钟。
* `PT40M50S`          - 四十分钟五十秒。
* `PT12H`             - 十二小时。
* `PT12H50S`          - 十二小时五十秒。
* `PT12H40M`          - 十二小时四十分钟。
* `PT12H40M50S`       - 十二小时四十分钟五十秒。
* `P1Y2M3DT12H40M50S` - 一年两个月三天十二小时四十分钟五十秒。这是一个不精确的值。

精确值包括天、小时、分钟、秒。
年和月不精确，因为它们不同，只有在将时间段与已知日期相关联时才能变得精确。
