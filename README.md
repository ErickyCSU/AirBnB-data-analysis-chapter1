Airbnb room data analysises
=====================

Analyze the open data from https://www.kaggle.com/airbnb/boston with python3 to answer there questions:

1. How is the variation of prices and occupancy rate of Airbnb houses in one year？
2. What kind of houses will attract more customers to order?
3. What kind of houses will get a higher score?

See the project for more details.

# 怎样让你的房间更受欢迎？
> Airbnb数据分析
## 背景介绍
![](<images/airbnb_logo.jpg>)
Airbnb是著名的网上旅行房屋租赁社区，房主可以通过Airbnb在网络上发布空闲房间的租赁信息，并可以通过文字、图片以及视频的形式对房屋信息进行介绍以吸引顾客。同时顾客通过Airbnb获取房子信息之后可以预定适合自己的房间入住，并向房主缴纳一定的租金。为了增加自己的收益，每个房主都希望自己的房子能够吸引更多的顾客，那么如何才能让你的房子更受欢迎呢？下面我们会通过数据来回答这个问题。

## 数据介绍
数据由3部分组成，分别是：
1.	房屋属性数据：3585条分布在全球各地的Airbnb入驻房屋信息，开始营业时间从2008年11月11日到2016年9月6日不等，数据内容包括房屋所在地、价格、营业时间和浏览量等
2.	房屋入住记录：2016年9月6日到2017年9月5日的入住情况，t代表当天有人入住，f代表空闲
3.	房屋评价：入住过的用户对房屋的评价文本

*我们使用第1部分和第2部分数据对3个问题进行回答。*


## 问题回答

### 问题1：店铺入住情况和价格在一年中是怎样变化的？
![](<images/occupied_countandaverage_pricevariation.png>)

房屋的月均入住天数在不同月份的变化情况如图所示，可以看出9、10月份是一年中最适合出游的时段，也是房屋入住率最高的月份，平均每个房间每个月有18天入住，受供需关系调节，这段时间也是一年中房屋租金最高的时段。随后到来的11、12和次年的1、2月份是出游的淡季，房屋的入住天数会大幅下降至平均每月只有12~14天有人入住，相应的租金也会调整到旺季的70%左右，平均价格在$190每天。随着天气回暖，3月份入住情况有明显回升而房屋的租金还会维持在一个较低水平，所以对于时间允许的游客来说，**在3月出游会是一个性价比较高的选择**。
### 问题2：什么样的房屋入住率高？
通过建模地方法对用户最关切地房屋属性进行发掘,对房屋入住率影响最大的特征前5位依次是：
1.	首次浏览时间
2.	最后浏览时间
3.	营业时长
4.	单日价格
5.	订单接受率

*房屋入住率的计算方法是统计2016年9月6日到2017年9月5日的入住天数除以365天。*

### 问题3：什么样的房屋最令顾客满意（评分高）？
对租客评分级别影响最大的特征前5位依次是：
1.	最后浏览时间
2.	首次浏览时间
3.	单日价格
4.	营业时长
5.	订单接受率

*由于对入住率和满意度影响较大的特征重合度较高，所以我们将这两个问题放在一起一并分析。*

#### 营业时间相关变量对于入住率和满意度的影响
![](<images/occupied_ratioandreview_scores_valuevariation.png>)

因为首次评论时间和末次评论时间属同一时间轴上的两个点，为综合查看其影响，将房屋的首次评论时间和末次评论时间做差，得到变量：*评论时间差(review_delta)*，并根据其值大小将房屋样本分为数目相差不大的几组并计算每组的月均入住天数（代表房屋吸引力）和得分的平均数（代表顾客对房屋的满意度），得到如图所示曲线。

可以发现房屋入住率与首末次评论时间差呈反比，可以得出营业时间越长的房屋在2016.09.06~2017.09.05这一年中的入住率越低的结论。究其原因，因为房屋数据的统计时间区间是2008年11月11日到2016年9月6日，而入住率统计时间是2016年9月6日到2017年9月5日，所以如果截止到2016年9月房屋已经营业很长时间，可能由于宣传力度没有新开业的房屋大，导致其对于租客没有足够的曝光度，同时由于经营时间较长，必定覆盖了旅游淡季，对整体入住率也会有影响。

结合客户满意度评分去看，经营时间较短和较长的房屋评分较低，经营时间在80天以上300天以下的评分最高。可得出结论：新平台尽管能够通过较大的宣传力度吸引用户入住，但因为缺少经营经验，导致客户满意度不高，同时老平台由于经营时间较长，房屋可能出现老旧现象，若房主对房屋缺少打理就会导致用户体验下降。

#### 房屋价格对于入住率和满意度的影响
![](<images/The influence of price.png>)

从入住率曲线（蓝色）可以看出，客户对于房屋价格较为敏感，价格低确实可以成为吸引顾客的一个因素，高价格的房屋入住率会大幅下滑，但是超低的价格（<$65）必然会导致居住条件不尽如人意，所以低价区间的满意度也是最低的。相比而言中档价位（$80~$295）的房屋从入住率上表现很好，其中中低价位（$80~$99）的房屋的入住满意度最高，可见价格也是影响用户体验的重要因素。

#### 各个离散变量对评分的影响
*1、房屋类型对满意度评分的影响*

![](<images/room_type.jpg>)

*2、床垫类型对满意度评分的影响*

![](<images/bed_type.jpg>)

*3、房主是否实名认证对满意度评分的影响*

![](<images/host_id_varify.jpg>)

*4、房主是否在网上提供房间的准确地址对满意度评分的影响*

![](<images/accu_add.jpg>)

*5、订单取消政策对满意度评分的影响*

![](<images/cancle_policy.jpg>)

## 建议
**对于租客**：在选择住房的时候尽量选择营业时间在一个月到一年之间的房屋能够带来更好的体验。预算不高的游客可以选择低价位中价格在$100左右的房屋，预算充足的游客可以选择$200美元左右的房屋，因为这两档是在相近价位的房屋中能给客户带来最高满意度的房屋。

**对于房主**：为了提高顾客的满意度，要定期修缮房屋，与时俱进地追踪当前客户审美，同时可以考虑将房间规划为共享房间，尽量选择Futon床以提升顾客的入住体验。验证自己的真实身份以提高自己的可靠度与安全性，在可接受范围内尽量选择宽松的订单取消策略，同时数据显示租客对房主是否在Airbnb上提供房屋的准确地址并不敏感，所以为了最大限度地保障自己的信息安全尽量不暴露自己的具体房屋地址信息。
