Facebook粉絲專業團分析(分析專業名稱蔡英文)
================

說明:分析蔡英文總統粉絲專業，資料分析區間為2016/01/01到2016/04/10。

讀取Cherng粉絲團資料
--------------------

    if (!require('Rfacebook')){
        install.packages("Rfacebook")
        library(Rfacebook)
    }


    totalPage<-NULL
    lastDate<-Sys.Date()
    numberOfPost<-30
    DateVector<-seq(as.Date("2016-01-01"),lastDate,by="5 days")
    DateVectorStr<-as.character(DateVector)


    token<-'EAACEdEose0cBAG7SOZC0PArPZCclSe0q2UUZCIYlHucetIZBfot7e6WpT6KXeA5KYS3TR3C5bCXgFgww1eTMOibRB3GY7WPgCpa4CYGwZAySlJwZCmMLtcQ5tyZA2UNqSkxXyLROskZBMAXAdElkwlLsn120ji4OlSWgzBixwAcm9QZDZD'
    for(i in 1:(length(DateVectorStr)-1)){
        tempPage<-getPage("tsaiingwen", token,
                          since = DateVectorStr[i],until = DateVectorStr[i+1])
        totalPage<-rbind(totalPage,tempPage)
    }

    ## 18 posts 26 posts 32 posts 11 posts 14 posts 12 posts 9 posts 6 posts 6 posts 8 posts 9 posts 10 posts 6 posts 7 posts 7 posts 6 posts 6 posts 8 posts 5 posts 6 posts 

    nrow(totalPage)

    ##212

討論:2016/01/01至2016/04/10蔡英文粉絲團一共有212篇文章。

每日發文數分析
--------------

說明:分析蔡英文粉絲團每天的發文數，由於日期格式跟台灣不一樣，先將其轉換為台灣時區。

    totalPage$datetime <- as.POSIXct(totalPage$created_time, 
                                     format = "%Y-%m-%dT%H:%M:%S+0000", 
                                     tz = "GMT") #2016-01-16T15:05:36+0000
    totalPage$dateTPE <- format(totalPage$datetime, "%Y-%m-%d", 
                                tz = "Asia/Taipei") #2016-01-16
    totalPage$weekdays <-weekdays(as.Date(totalPage$dateTPE))
    PostCount<-aggregate(id~dateTPE,totalPage,length)
    library(knitr)
    kable(head(PostCount[order(PostCount$id,decreasing = T),]))

|     | dateTPE    |   id|
|:----|:-----------|----:|
| 15  | 2016-01-15 |    8|
| 11  | 2016-01-11 |    7|
| 14  | 2016-01-14 |    7|
| 8   | 2016-01-08 |    6|
| 10  | 2016-01-10 |    6|
| 13  | 2016-01-13 |    6|

討論:2016/01/15的發文數最多，一共有八篇，2016/01/11和2016/01/14居次，再來是2016/01/08和2016/01/10和2016/01/13，這是因為2016/1/16是總統大選，選前需要拉高知名度，努力得到選民的支持。

每日Likes數分析
---------------

說明:分析蔡英文粉絲團每天的Likes數，由於日期格式跟台灣不一樣，先將其轉換為台灣時區。

    totalPage$datetime <- as.POSIXct(totalPage$created_time, 
                                     format = "%Y-%m-%dT%H:%M:%S+0000", 
                                     tz = "GMT") #2016-01-16T15:05:36+0000
    totalPage$dateTPE <- format(totalPage$datetime, "%Y-%m-%d", 
                                tz = "Asia/Taipei") #2016-01-16
    totalPage$weekdays <-weekdays(as.Date(totalPage$dateTPE))
    likesCount<-aggregate(likes_count~dateTPE,totalPage,mean)
    library(knitr)
    kable(head(likesCount[order(likesCount$likes_count,decreasing = T),]))

|     | dateTPE    |  likes\_count|
|:----|:-----------|-------------:|
| 17  | 2016-01-17 |      260415.0|
| 16  | 2016-01-16 |      241246.8|
| 89  | 2016-03-29 |      189566.0|
| 20  | 2016-01-20 |      121719.5|
| 42  | 2016-02-11 |      113708.0|
| 39  | 2016-02-08 |      101000.0|

討論:2016/01/17的likes數最多，一共有26萬個讚，2016/01/16居次，再來是2016/03/29，2016/01/16和2016/01/17是因為剛剛總統大選完，所以關注度較高，2016/03/29是因為小燈泡事件，全台灣民眾都憤怒的事件，所以關注度高。

每日Comments數分析
------------------

說明:分析蔡英文粉絲團每天的Comments數，由於日期格式跟台灣不一樣，先將其轉換為台灣時區。

    totalPage$datetime <- as.POSIXct(totalPage$created_time, 
                                     format = "%Y-%m-%dT%H:%M:%S+0000", 
                                     tz = "GMT") #2016-01-16T15:05:36+0000
    totalPage$dateTPE <- format(totalPage$datetime, "%Y-%m-%d", 
                                tz = "Asia/Taipei") #2016-01-16
    totalPage$weekdays <-weekdays(as.Date(totalPage$dateTPE))
    commentsCount<-aggregate(comments_count~dateTPE,totalPage,mean)
    library(knitr)
    kable(head(commentsCount[order(commentsCount$comments_count,decreasing = T),]))

|     | dateTPE    |  comments\_count|
|:----|:-----------|----------------:|
| 20  | 2016-01-20 |        27994.500|
| 21  | 2016-01-21 |        16111.667|
| 17  | 2016-01-17 |        10525.000|
| 19  | 2016-01-19 |         9388.000|
| 18  | 2016-01-18 |         9133.000|
| 16  | 2016-01-16 |         8233.833|

討論:2016/01/20的comments數最多，2016/01/21居次，2016/01/20是蔡英文總統選後第一次民進黨中常會，台灣政治的重大改變，所以接連兩天，全臺人民熱烈反應。

每日Shares數分析
----------------

說明:分析蔡英文粉絲團每天的Shares數，由於日期格式跟台灣不一樣，先將其轉換為台灣時區。

    totalPage$datetime <- as.POSIXct(totalPage$created_time, 
                                     format = "%Y-%m-%dT%H:%M:%S+0000", 
                                     tz = "GMT") #2016-01-16T15:05:36+0000
    totalPage$dateTPE <- format(totalPage$datetime, "%Y-%m-%d", 
                                tz = "Asia/Taipei") #2016-01-16
    totalPage$weekdays <-weekdays(as.Date(totalPage$dateTPE))
    sharesCount<-aggregate(shares_count~dateTPE,totalPage,mean)
    library(knitr)
    kable(head(sharesCount[order(sharesCount$shares_count,decreasing = T),]))

|     | dateTPE    |  shares\_count|
|:----|:-----------|--------------:|
| 89  | 2016-03-29 |         6245.5|
| 16  | 2016-01-16 |         4195.0|
| 17  | 2016-01-17 |         3811.0|
| 38  | 2016-02-07 |         3119.0|
| 18  | 2016-01-18 |         3008.0|
| 42  | 2016-02-11 |         2570.0|

討論:2016/3/29分享數最多，2016/01/16和2016/01/17居次，2016/03/29是小燈泡事件，蔡英文總統把對王女生的發言的想法表達在文章裡，這篇文章受到大家的迴響，所以shares數很高，2016/01/16和2016/01/17是因為總統大選後，所以分享度高。
