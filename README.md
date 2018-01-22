## ParallelGibbsLda  
LDA（Latent Dirichlet Allocation）由Blei于2003年提出，是机器学习中一种重要的隐变量挖掘模型（topic model），目前被广泛应用于文本挖掘、社交分析、计算广告等领域。LDA的数学推导十分复杂，但基于Gibbs Sampling的工程实现却相对简单，作者在实习期间首次接触topic model，深感其中数学与工程之美。

***
训练语料来源于某站点新闻资讯频道，由于未获授权，本项目仅抽取少量的2000篇新闻文本做测试；  
resource目录下的语料已预先进行分词、去停用词；另外，将低频噪声词过滤掉会显著提升模型perplexity；  
因此，resource目录下同时包含一份训练语料中的高频词；
***

项目由两部分组成，第一部分实现传统单线程Gibbs采样，第二部分实现AD-LDA并行Gibbs采样
 
### 单线程

    Hyperparameters： K=30，alpha=2.0，beta=0.5，iter=200； 

    training process cost 39s
	after training, the corpus perplexity is 703.0678
	topic-word martrix：  
	topic 0 >>> 旅游:0.0294 中国:0.0197 酒店:0.0170 文化:0.0165 城市:0.0163 景区:0.0149 公园:0.0138 活动:0.0125 大理:0.0112 体验:0.0108  
	topic 1 >>> 风格:0.0005 机动车:0.0004 前台:0.0003 一群:0.0003 女孩:0.0003 条件:0.0003 能力:0.0003 开放:0.0003 有史以来:0.0002 贵人:0.0002  
	topic 2 >>> 魅力:0.0085 不错:0.0038 特别:0.0006 结婚:0.0005 第一:0.0005 实验:0.0004 印度:0.0004 一件:0.0004 宝宝:0.0004 网友:0.0004  
	topic 3 >>> 工作:0.0151 公司:0.0125 两个:0.0104 人员:0.0104 发展:0.0102 站点:0.0095 交易:0.0090 办理:0.0084 服务:0.0084 尝试:0.0079  
	topic 4 >>> 美国:0.0672 展开:0.0006 诊断:0.0005 一杯:0.0005 视频:0.0005 保鲜:0.0004 天津市:0.0004 公共:0.0004 国内:0.0004 城市:0.0004  
	topic 5 >>> 建议:0.0280 女性:0.0077 公交:0.0011 体质:0.0005 回家:0.0005 一道:0.0005 衣物:0.0004 城乡:0.0004 判断:0.0003 话题:0.0003  
	topic 6 >>> 武汉:0.0255 研发:0.0223 呼吸:0.0217 电话:0.0196 地址:0.0131 最为:0.0056 调整:0.0033 高跟鞋:0.0032 烤肉:0.0020 统筹:0.0019  
	topic 7 >>> 简介:0.0142 免费:0.0006 第一步:0.0005 瞬间:0.0005 微信:0.0005 不知:0.0004 蜂蜜:0.0004 即可:0.0004 翻滚:0.0003 第一天:0.0003  
	topic 8 >>> 做法:0.0187 适量:0.0163 分钟:0.0152 鸡蛋:0.0144 即可:0.0120 食物:0.0115 营养:0.0111 倒入:0.0100 食用:0.0100 习惯:0.0095  
	topic 9 >>> 恐怖:0.0108 适合:0.0052 也许:0.0033 美女:0.0015 增加:0.0006 零食:0.0005 超级:0.0005 娱乐:0.0005 一份:0.0005 昨日:0.0004  
	topic 10 >>> 东方:0.0105 客厅:0.0079 影视:0.0078 过瘾:0.0066 没什么:0.0061 村民:0.0061 发酵:0.0057 两旁:0.0005 香蕉:0.0005 周岁:0.0004  
	topic 11 >>> 心灵:0.0007 满满:0.0007 一点:0.0006 欧式:0.0004 增加:0.0004 武汉:0.0004 钥匙:0.0003 特价:0.0003 不知:0.0003 房子:0.0003  
	topic 12 >>> 健康:0.0009 主流:0.0004 服装:0.0004 规模:0.0004 香甜:0.0003 气温:0.0003 负责:0.0003 房子:0.0003 控制:0.0003 赶紧:0.0003  
	topic 13 >>> 网友:0.0006 贵人:0.0005 资格:0.0005 音乐:0.0005 告诉:0.0005 辜负:0.0004 机票:0.0004 茄子:0.0004 大理:0.0004 姐妹:0.0003  
	topic 14 >>> 肯定:0.0004 一份:0.0004 公交:0.0004 无处:0.0003 时髦:0.0003 杀手:0.0003 蜂蜜:0.0003 训练:0.0003 工程:0.0003 各家:0.0002  
	topic 15 >>> 海鲜:0.0153 学会:0.0152 贷款:0.0117 饭店:0.0112 客人:0.0094 家常菜:0.0092 料理:0.0033 野外:0.0005 质量:0.0005 统筹:0.0004  
	topic 16 >>> 刻画:0.0006 欧式:0.0003 米其林:0.0003 零食:0.0003 近日:0.0003 玉米:0.0003 汽车:0.0003 一份:0.0003 下午:0.0003 钥匙:0.0002  
	topic 17 >>> 米其林:0.0006 明天:0.0005 顾客:0.0005 厨房:0.0004 网友:0.0004 丛生:0.0003 印记:0.0003 开心:0.0003 美女:0.0003 公布:0.0003  
	topic 18 >>> 利润:0.0006 厨房:0.0006 超级:0.0005 石头:0.0004 校服:0.0004 形式:0.0004 朋友:0.0004 鸡皮:0.0003 杀手:0.0003 不知:0.0003  
	topic 19 >>> 电影:0.0801 国内:0.0266 作品:0.0195 故事:0.0178 导演:0.0167 演员:0.0160 拍摄:0.0148 角色:0.0144 观众:0.0140 厦门:0.0139  
	topic 20 >>> null:0.0846 蚕丝:0.0113 有史以来:0.0110 贵人:0.0107 结膜:0.0103 吃透:0.0102 牵挂:0.0098 坠入:0.0094 丛生:0.0092 饭庄:0.0089  
	topic 21 >>> 美食:0.0260 地址:0.0257 味道:0.0217 推荐:0.0194 好吃:0.0165 机动车:0.0153 餐厅:0.0143 电话:0.0134 人均:0.0129 牛肉:0.0110  
	topic 22 >>> 几个:0.0006 辜负:0.0004 上午:0.0004 女孩:0.0004 完美:0.0004 关注:0.0004 姐妹:0.0003 车祸:0.0003 发挥:0.0003 来到:0.0003  
	topic 23 >>> 统筹:0.0058 出发:0.0058 感受:0.0054 携手:0.0049 站点:0.0046 时光:0.0042 爱好者:0.0005 国际:0.0005 坠入:0.0004 邀请:0.0004  
	topic 24 >>> 餐桌:0.0260 近日:0.0256 节奏:0.0205 地方:0.0161 实验:0.0085 湿地:0.0081 手法:0.0077 领域:0.0077 喧嚣:0.0071 携手:0.0070  
	topic 25 >>> 冬瓜:0.0239 吃法:0.0009 日本:0.0006 绿色:0.0004 演员:0.0004 丝绸:0.0003 印记:0.0003 携手:0.0003 判断:0.0003 搜索:0.0003  
	topic 26 >>> 孩子:0.0399 医院:0.0268 道路:0.0252 记者:0.0236 民警:0.0164 男子:0.0149 市民:0.0146 学校:0.0136 校服:0.0132 学生:0.0129  
	topic 27 >>> 停电:0.0612 街道:0.0364 时间:0.0314 社区:0.0172 小区:0.0171 果子:0.0094 楼市:0.0070 天下第一:0.0069 名气:0.0067 人口:0.0050  
	topic 28 >>> 变化:0.0300 健康:0.0231 慕思:0.0181 运动:0.0126 患者:0.0112 人体:0.0079 症状:0.0064 导致:0.0056 地铁:0.0048 值得:0.0043  
	topic 29 >>> 衣物:0.0005 散发:0.0005 带来:0.0005 吃透:0.0004 琵琶:0.0004 食疗:0.0004 判断:0.0004 饺子:0.0003 影视:0.0003 上午:0.0003  
	
观察发现，近半数topic还是具有可读性的，由于每篇新闻内容相差较大，文档集过为稀疏，导致部分topic的topic-word分布非常平均，接近噪声分布，增大数据集会获得很好提升。

***
### 多线程并行
    Hyperparameters： K=30，alpha=2.0，beta=0.5，iter=200，threads=5； 

	training process cost 41s
	after training, the corpus perplexity is 724.8136
	topic 0 >>> 停电:0.0557 街道:0.0331 时间:0.0263 小区:0.0158 电话:0.0151 社区:0.0148 花园:0.0142 研发:0.0141 武汉:0.0113 地址:0.0112
	topic 1 >>> 影视:0.0112 魅力:0.0088 客厅:0.0085 东方:0.0080 村民:0.0075 发酵:0.0063 过瘾:0.0058 没什么:0.0053 不错:0.0024 记住:0.0005
	topic 2 >>> 电影:0.0634 美国:0.0341 国内:0.0219 作品:0.0173 照片:0.0168 导演:0.0155 拍摄:0.0143 故事:0.0142 演员:0.0129 角色:0.0113
	topic 3 >>> 工作:0.0149 公司:0.0107 记者:0.0106 人员:0.0098 两个:0.0091 道路:0.0085 发展:0.0082 办理:0.0078 孩子:0.0074 邀请:0.0072
	topic 4 >>> 私家:0.0003 手工:0.0003 最终:0.0003 贵人:0.0002 坠落:0.0002 丝绸:0.0002 高跟鞋:0.0002 总体:0.0002 特价:0.0002 车主:0.0002
	topic 5 >>> 特别:0.0005 上午:0.0004 医疗:0.0004 广告:0.0004 软件:0.0003 三亚:0.0003 施工:0.0003 白色:0.0003 提醒:0.0003 原因:0.0003
	topic 6 >>> 网友:0.0005 路过:0.0003 组合:0.0003 茄子:0.0003 手机:0.0003 活动:0.0003 牵挂:0.0002 无处:0.0002 翻滚:0.0002 幽静:0.0002
	topic 7 >>> 市民:0.0004 野外:0.0003 坐标:0.0003 融入:0.0003 剩下:0.0003 基础:0.0003 南京大屠杀:0.0002 对话框:0.0002 刻画:0.0002 白鹭:0.0002
	topic 8 >>> 交易:0.0335 尝试:0.0249 自由:0.0215 数量:0.0210 本次:0.0199 徒步:0.0188 站点:0.0127 散发:0.0070 第一:0.0068 氧化:0.0063
	topic 9 >>> 旅游:0.0254 中国:0.0233 文化:0.0163 酒店:0.0148 城市:0.0146 景区:0.0129 活动:0.0122 公园:0.0117 大理:0.0109 世界:0.0108
	topic 10 >>> 学会:0.0174 海鲜:0.0146 贷款:0.0119 家常菜:0.0103 客人:0.0092 饭店:0.0083 料理:0.0026 最为:0.0004 网上:0.0004 天气:0.0004
	topic 11 >>> 做法:0.0204 适量:0.0177 分钟:0.0164 鸡蛋:0.0155 即可:0.0127 倒入:0.0109 营养:0.0104 食用:0.0102 习惯:0.0092 张家界:0.0085
	topic 12 >>> 富有:0.0004 鸡皮:0.0003 气温:0.0003 芝麻:0.0003 费用:0.0003 选择:0.0003 一种:0.0003 丛生:0.0002 坠落:0.0002 各家:0.0002
	topic 13 >>> 满满:0.0006 田园:0.0003 房子:0.0003 查看:0.0003 牵挂:0.0002 辜负:0.0002 填写:0.0002 天下第一:0.0002 清晰:0.0002 记住:0.0002
	topic 14 >>> 身体:0.0004 节奏:0.0003 讲述:0.0003 夏天:0.0003 现场:0.0003 有史以来:0.0002 贵人:0.0002 无处:0.0002 紊乱:0.0002 填写:0.0002
	topic 15 >>> 携手:0.0007 遍布:0.0003 差异:0.0003 时期:0.0003 女孩:0.0003 事情:0.0003 平台:0.0003 东莞:0.0003 野餐:0.0002 丝绸:0.0002
	topic 16 >>> 餐桌:0.0307 近日:0.0282 节奏:0.0228 地方:0.0179 魅力:0.0009 体质:0.0006 花瓣:0.0004 机场:0.0004 特别:0.0004 盛产:0.0003
	topic 17 >>> 建议:0.0237 女性:0.0089 公交:0.0036 学院:0.0018 投稿:0.0010 大胆:0.0009 上山:0.0007 利润:0.0004 正面:0.0004 当地人:0.0004
	topic 18 >>> 预防:0.0005 欧式:0.0004 网友:0.0004 济南:0.0003 芝麻:0.0003 结婚:0.0003 满满:0.0003 西安:0.0003 外加:0.0002 紊乱:0.0002
	topic 19 >>> 明天:0.0006 美女:0.0004 手机:0.0004 蚕丝:0.0003 白鹭:0.0003 一次性:0.0003 随便:0.0003 茄子:0.0003 年轻人:0.0003 海洋:0.0003
	topic 20 >>> 石头:0.0005 二维:0.0004 学会:0.0003 酸奶:0.0003 植物:0.0003 咨询:0.0003 女儿:0.0003 摄影:0.0003 有人:0.0003 丛生:0.0002
	topic 21 >>> 野外:0.0004 改编:0.0004 推出:0.0004 二维:0.0004 坠入:0.0003 麻辣:0.0003 美女:0.0003 房子:0.0003 导致:0.0003 孩子:0.0003
	topic 22 >>> 民警:0.0271 男子:0.0234 网友:0.0005 发现:0.0005 群众:0.0004 对话框:0.0003 姐妹:0.0003 肝癌:0.0003 兴奋:0.0003 回归:0.0003
	topic 23 >>> 地址:0.0249 美食:0.0243 味道:0.0216 推荐:0.0199 好吃:0.0145 餐厅:0.0142 机动车:0.0132 人均:0.0129 电话:0.0129 牛肉:0.0107
	topic 24 >>> null:0.0871 蚕丝:0.0116 有史以来:0.0113 贵人:0.0110 结膜:0.0106 吃透:0.0105 牵挂:0.0100 坠入:0.0098 丛生:0.0095 饭庄:0.0091
	topic 25 >>> 理解:0.0004 主流:0.0003 统筹:0.0003 芝麻:0.0003 居民:0.0003 商务:0.0003 现场:0.0003 钥匙:0.0002 利润:0.0002 张飞:0.0002
	topic 26 >>> 健康:0.0275 变化:0.0243 食物:0.0215 地铁:0.0204 慕思:0.0194 医院:0.0145 作用:0.0128 人体:0.0124 患者:0.0121 运动:0.0119
	topic 27 >>> 学习:0.0004 组织:0.0004 牵挂:0.0003 前台:0.0003 飙升:0.0002 上山:0.0002 填写:0.0002 月月:0.0002 二号:0.0002 果子:0.0002
	topic 28 >>> 冬瓜:0.0289 吃法:0.0004 体现:0.0004 比赛:0.0004 背部:0.0003 快递:0.0003 近日:0.0003 美女:0.0003 早餐:0.0003 一口:0.0003
	topic 29 >>> 周边:0.0006 北京:0.0004 第一天:0.0003 一群:0.0003 富有:0.0003 各类:0.0003 肯定:0.0003 食用:0.0003 美食:0.0003 有史以来:0.0002

例中启动线程数为5，训练后的perplexity及输出的topic-word矩阵与单线程串行训练结果非常接近。误差部分主要由nw和nwsum两个统计矩阵产生。在每轮gibbs开始前，各线程首先把全局nw和nwsum矩阵copy到本地，随后在gibbs过程中，以本地矩阵进行采样和更新，并在该轮结束后将自身的更新量同步到全局nw和nwsum，因此整个过程是近似的并行LDA（Approximate Distributed LDA）。  
本实现中并行算法的同步由阻塞队列控制，并行训练时间稍逊于单线程是由于数据量太少，体现不出多线程价值，并且每轮都要copy全局矩阵和reduce各线程更新量，因此开销比单线程还要稍大一些。


***
将数据集增大10倍（2w份文本），线程数为5，迭代100轮耗时：  
单线程 - 414s  
多线程 - __84s__
***

### Reference
* David M. Blei, Andrew Y. Ng., Michael I. Jordan. Latent Dirchlet Allocation
* Gregor Heinrich. Parameter Estimation for Text Analysis
* Newman, David, et al. Distributed Inference for Latent Dirichlet Allocation
* Rick Jin. LDA数学八卦