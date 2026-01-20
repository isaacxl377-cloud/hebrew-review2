<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>希伯来历史复习 v5.4 (键盘适配版)</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script crossorigin src="https://unpkg.com/react@18/umd/react.development.js"></script>
    <script crossorigin src="https://unpkg.com/react-dom@18/umd/react-dom.development.js"></script>
    <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
    <style>
        body, html { 
            height: 100%;
            overflow: hidden; /* 防止iOS橡皮筋效果 */
            -webkit-tap-highlight-color: transparent; 
            background-color: #f8fafc; 
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, "Noto Sans", sans-serif;
        }
        #root {
            height: 100%;
        }
        .input-highlight:focus { 
            transform: scale(1.02); 
            background-color: #ffffff; 
            border-color: #4338ca; 
            box-shadow: 0 4px 12px rgba(99, 102, 241, 0.15);
        }
        @keyframes slideUp { from { opacity: 0; transform: translateY(20px); } to { opacity: 1; transform: translateY(0); } }
        .animate-slide-up { animation: slideUp 0.3s ease-out forwards; }
        .glass-header { background: rgba(255, 255, 255, 0.95); backdrop-filter: blur(8px); }
        
        /* 针对不同屏幕高度的微调 */
        .safe-area-bottom {
            padding-bottom: env(safe-area-inset-bottom);
        }
    </style>
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: { primary: '#4338ca', secondary: '#be185d' }
                }
            }
        }
    </script>
</head>
<body>
    <div id="root"></div>

    <script type="text/babel">
        // 图标组件
        const Icon = ({ path, className }) => (
            <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2.5" strokeLinecap="round" strokeLinejoin="round" className={className}>{path}</svg>
        );
        const Icons = {
            Home: (p) => <Icon {...p} path={<path d="M3 9l9-7 9 7v11a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2z"/>} />,
            Book: (p) => <Icon {...p} path={<><path d="M4 19.5A2.5 2.5 0 0 1 6.5 17H20"/><path d="M6.5 2H20v20H6.5A2.5 2.5 0 0 1 4 19.5v-15A2.5 2.5 0 0 1 6.5 2z"/></>} />,
            Save: (p) => <Icon {...p} path={<><path d="M19 21H5a2 2 0 0 1-2-2V5a2 2 0 0 1 2-2h11l5 5v11a2 2 0 0 1-2 2z"/><polyline points="17 21 17 13 7 13 7 21"/><polyline points="7 3 7 8 15 8"/></>} />,
            RotateCcw: (p) => <Icon {...p} path={<><path d="M3 12a9 9 0 1 0 9-9 9.75 9.75 0 0 0-6.74 2.74L3 8"/><path d="M3 3v5h5"/></>} />,
            Check: (p) => <Icon {...p} path={<polyline points="20 6 9 17 4 12"/>} />,
            X: (p) => <Icon {...p} path={<><line x1="18" y1="6" x2="6" y2="18"/><line x1="6" y1="6" x2="18" y2="18"/></>} />,
            Eye: (p) => <Icon {...p} path={<><path d="M1 12s4-8 11-8 11 8 11 8-4 8-11 8-11-8-11-8z"/><circle cx="12" cy="12" r="3"/></>} />,
            EyeOff: (p) => <Icon {...p} path={<><path d="M17.94 17.94A10.07 10.07 0 0 1 12 20c-7 0-11-8-11-8a18.45 18.45 0 0 1 5.06-5.94M9.9 4.24A9.12 9.12 0 0 1 12 4c7 0 11 8 11 8a18.5 18.5 0 0 1-2.16 3.19m-6.72-1.07-2.3 2.3"/><line x1="1" y1="1" x2="23" y2="23"/></>} />,
            ChevronRight: (p) => <Icon {...p} path={<polyline points="9 18 15 12 9 6"/>} />,
            AlertCircle: (p) => <Icon {...p} path={<><circle cx="12" cy="12" r="10"/><line x1="12" y1="8" x2="12" y2="12"/><line x1="12" y1="16" x2="12.01" y2="16"/></>} />,
            ArrowLeft: (p) => <Icon {...p} path={<><line x1="19" y1="12" x2="5" y2="12"/><polyline points="12 19 5 12 12 5"/></>} />,
            Award: (p) => <Icon {...p} path={<><circle cx="12" cy="8" r="7"/><polyline points="8.21 13.89 7 23 12 20 17 23 15.79 13.88"/></>} />,
        };

        // -----------------------------------------------------------------------------
        // 核心题库：100% 覆盖 PPT (希伯来历史下半学期)
        // -----------------------------------------------------------------------------
        const rawQuestions = [
            {
                "category": "经文默写",
                "question": "撒上 8:7\n百姓向你说的一切话，你只管{依从}，因为他们不是{厌弃}你，乃是厌弃我..."
            },
            {
                "category": "经文默写",
                "question": "撒上 8:7\n因为他们不是厌弃你，乃是厌弃我，不要我{作他们的王}。"
            },
            {
                "category": "经文默写",
                "question": "撒上 8:9\n只是当{警戒}他们，告诉他们将来那王怎样{管辖}他们。"
            },
            {
                "category": "经文默写",
                "question": "撒上 12:20\n你们虽然行了这恶，却不要{偏离}耶和华，只要{尽心}侍奉他。"
            },
            {
                "category": "经文默写",
                "question": "撒上 12:22\n耶和华既{喜悦}选你们作他的子民，就必因他的大名{不撇弃}你们。"
            },
            {
                "category": "经文默写",
                "question": "撒上 14:6\n或者耶和华为我们{施展能力}，因为耶和华使人得胜，不在乎{人多人少}。"
            },
            {
                "category": "经文默写",
                "question": "撒上 14:6\n因为耶和华使人{得胜}，不在乎{人多人少}。"
            },
            {
                "category": "经文默写",
                "question": "撒上 15:22\n耶和华喜悦{燔祭}和平安祭，岂如喜悦人{听从}他的话呢？"
            },
            {
                "category": "经文默写",
                "question": "撒上 15:22\n听命胜于{献祭}；顺从胜于{公羊的脂油}。"
            },
            {
                "category": "经文默写",
                "question": "撒上 15:23\n{悖逆}的罪与行邪术的罪相等；{顽梗}的罪与拜虚神和偶像的罪相同。"
            },
            {
                "category": "经文默写",
                "question": "撒上 16:7\n不要看他的{外貌}和他{身材高大}，我不拣选他..."
            },
            {
                "category": "经文默写",
                "question": "撒上 16:7\n因为耶和华不像{人看人}，人是看外貌，耶和华是看{内心}。"
            },
            {
                "category": "经文默写",
                "question": "撒上 17:45\n你来攻击我，是靠着{刀枪}和{铜戟}；"
            },
            {
                "category": "经文默写",
                "question": "撒上 17:45\n我来攻击你，是靠着{万军之耶和华}的名。"
            },
            {
                "category": "经文默写",
                "question": "撒上 17:47\n又使这众人知道耶和华使人得胜，不是用{刀用枪}，因为争战的胜败全在乎{耶和华}。"
            },
            {
                "category": "经文默写",
                "question": "撒上 24:10\n我不敢伸手害我的主，因为他是耶和华的{受膏者}。"
            },
            {
                "category": "经文默写",
                "question": "撒下 7:12\n你寿数满足，与你列祖同睡的时候，我必使你的{后裔}接续你的位，我也必坚定他的{国}。"
            },
            {
                "category": "经文默写",
                "question": "撒下 7:14\n我要作他的{父}，他要作我的{子}。"
            },
            {
                "category": "经文默写",
                "question": "撒下 7:16\n你的{家}和你的{国}，必在你面前永远坚立。"
            },
            {
                "category": "经文默写",
                "question": "撒下 7:16\n你的国位也必{坚定}，直到{永远}。"
            },
            {
                "category": "经文默写",
                "question": "撒下 12:13\n大卫对拿单说：“我{得罪}耶和华了！”"
            },
            {
                "category": "经文默写",
                "question": "撒下 24:10\n大卫{数点百姓}以后，就心中自责。"
            },
            {
                "category": "撒上:撒母耳与约柜",
                "question": "哈拿因为不生育被谁欺负？\n{毗尼拿}"
            },
            {
                "category": "撒上:撒母耳与约柜",
                "question": "哈拿在示罗的殿中默祷，被祭司{以利}误以为是喝醉了。"
            },
            {
                "category": "撒上:撒母耳与约柜",
                "question": "哈拿许愿若生儿子，必使他终身归与耶和华，不用{剃头刀}剃他的头。"
            },
            {
                "category": "撒上:撒母耳与约柜",
                "question": "撒母耳的名字的意思是：{从神求来的}。"
            },
            {
                "category": "撒上:撒母耳与约柜",
                "question": "撒母耳童年时，耶和华在夜间呼唤他{4}次。"
            },
            {
                "category": "撒上:撒母耳与约柜",
                "question": "当神呼唤撒母耳时，以利教导他回答：“耶和华啊，请说，{仆人}敬听。”"
            },
            {
                "category": "撒上:撒母耳与约柜",
                "question": "以利的两个儿子是恶人，不认识耶和华，他们的名字是何弗尼和{非尼哈}。"
            },
            {
                "category": "撒上:撒母耳与约柜",
                "question": "以色列人与非利士人打仗战败，便将{约柜}抬到战场，结果被掳。"
            },
            {
                "category": "撒上:撒母耳与约柜",
                "question": "以利听说约柜被掳，就向后跌倒，折断{颈项}而死。"
            },
            {
                "category": "撒上:撒母耳与约柜",
                "question": "非尼哈的妻临产死去，给孩子起名叫{以迦博}，意为“荣耀离开以色列”。"
            },
            {
                "category": "撒上:撒母耳与约柜",
                "question": "非利士人将约柜抬进{大衮}庙，偶像扑倒，头手折断。"
            },
            {
                "category": "撒上:撒母耳与约柜",
                "question": "约柜在非利士地引起了什么灾祸？\n{痔疮}"
            },
            {
                "category": "撒上:撒母耳与约柜",
                "question": "非利士人送回约柜时，使用了两只未曾负轭的{母牛}拉车。"
            },
            {
                "category": "撒上:撒母耳与约柜",
                "question": "约柜被送回到了{伯示麦}（地名），那里的人因擅观约柜被击杀。"
            },
            {
                "category": "撒上:撒母耳与约柜",
                "question": "约柜后来存放在基列耶琳的{亚比拿达}家中二十年。"
            },
            {
                "category": "撒上:撒母耳与约柜",
                "question": "撒母耳带领百姓在{米斯巴}聚集，禁食悔改，打败非利士人。"
            },
            {
                "category": "撒上:撒母耳与约柜",
                "question": "撒母耳立了一块石头，起名叫{以便以谢}，意为“到如今耶和华都帮助我们”。"
            },
            {
                "category": "撒上:撒母耳与约柜",
                "question": "撒母耳平生作以色列的士师，每年巡行伯特利、吉甲、{米斯巴}。"
            },
            {
                "category": "撒上:扫罗王",
                "question": "以色列百姓要求立王，是因为撒母耳年纪老迈，且他的儿子{不行他的道}。"
            },
            {
                "category": "撒上:扫罗王",
                "question": "扫罗属于以色列的{便雅悯}支派。"
            },
            {
                "category": "撒上:扫罗王",
                "question": "扫罗遇见撒母耳是因为他去寻找父亲丢失的{驴}。"
            },
            {
                "category": "撒上:扫罗王",
                "question": "扫罗第一次在哪里违背命令私自献祭？\n{吉甲}"
            },
            {
                "category": "撒上:扫罗王",
                "question": "非利士人控制以色列，当时以色列全地没有一个{铁匠}。"
            },
            {
                "category": "撒上:扫罗王",
                "question": "约拿单和拿兵器的人去攻击非利士人，他说：“耶和华使人得胜，不在乎{人多人少}。”"
            },
            {
                "category": "撒上:扫罗王",
                "question": "扫罗被神厌弃，是因为他在攻打{亚玛力}人的事上没有听从神的命令。"
            },
            {
                "category": "撒上:扫罗王",
                "question": "扫罗怜惜亚玛力王{亚甲}，没有杀他。"
            },
            {
                "category": "撒上:扫罗王",
                "question": "撒母耳责备扫罗说：“{听命}胜于献祭。”"
            },
            {
                "category": "撒上:大卫与扫罗",
                "question": "大卫是耶西的第{8}个儿子。"
            },
            {
                "category": "撒上:大卫与扫罗",
                "question": "神差遣撒母耳到{伯利恒}（地名）去膏大卫。"
            },
            {
                "category": "撒上:大卫与扫罗",
                "question": "耶和华的灵离开扫罗，有{恶魔}从耶和华那里来扰乱他。"
            },
            {
                "category": "撒上:大卫与扫罗",
                "question": "大卫初次进宫是为了给扫罗{弹琴}。"
            },
            {
                "category": "撒上:大卫与扫罗",
                "question": "歌利亚是{非利士}人，向以色列骂阵40天。"
            },
            {
                "category": "撒上:大卫与扫罗",
                "question": "大卫杀歌利亚用的武器是机弦和{石子}。"
            },
            {
                "category": "撒上:大卫与扫罗",
                "question": "扫罗嫉妒大卫，因为妇女们唱道：“扫罗杀死千千，大卫杀死{万万}。”"
            },
            {
                "category": "撒上:大卫与扫罗",
                "question": "大卫逃跑时，祭司亚希米勒给了他{陈设饼}（食物）和歌利亚的刀。"
            },
            {
                "category": "撒上:大卫与扫罗",
                "question": "因为大卫的缘故，扫罗吩咐{多益}（以东人）杀死了挪伯的85个祭司。"
            },
            {
                "category": "撒上:大卫与扫罗",
                "question": "大卫在迦特王面前装{疯}才得以逃脱。"
            },
            {
                "category": "撒上:大卫与扫罗",
                "question": "大卫在{隐基底}洞不杀扫罗，只割下他的衣襟。"
            },
            {
                "category": "撒上:大卫与扫罗",
                "question": "拿八为人愚顽，他的妻子{亚比该}却聪明俊美。"
            },
            {
                "category": "撒上:大卫与扫罗",
                "question": "大卫第二次不杀扫罗，拿走了他头旁的{枪}和水瓶。"
            },
            {
                "category": "撒上:大卫与扫罗",
                "question": "扫罗在绝望中去求问{隐多珥}的女巫。"
            },
            {
                "category": "撒上:大卫与扫罗",
                "question": "扫罗和他的三个儿子最终战死在{基利波}山。"
            },
            {
                "category": "撒下:大卫作王",
                "question": "大卫听到扫罗死讯，作了{弓歌}（歌名）哀悼他们。"
            },
            {
                "category": "撒下:大卫作王",
                "question": "大卫先在{希伯仑}（城市）作犹大王七年零六个月。"
            },
            {
                "category": "撒下:大卫作王",
                "question": "扫罗的元帅{押尼珥}拥立伊施波设作以色列王。"
            },
            {
                "category": "撒下:大卫作王",
                "question": "约押杀死了押尼珥，是为了报他兄弟{亚撒黑}被杀之仇。"
            },
            {
                "category": "撒下:大卫作王",
                "question": "大卫攻取了耶路撒冷，又叫{锡安}。"
            },
            {
                "category": "撒下:大卫作王",
                "question": "大卫运约柜时，{乌撒}因为伸手扶约柜而被击杀。"
            },
            {
                "category": "撒下:大卫作王",
                "question": "约柜运到{俄别以东}家，三个月，神赐福给他。"
            },
            {
                "category": "撒下:大卫作王",
                "question": "大卫迎接约柜进城时极其快乐跳舞，被妻子{米甲}轻视。"
            },
            {
                "category": "撒下:大卫作王",
                "question": "神通过先知{拿单}告诉大卫，不可为神建殿。"
            },
            {
                "category": "撒下:大卫作王",
                "question": "神应许大卫的国位必{直到永远}。"
            },
            {
                "category": "撒下:大卫作王",
                "question": "大卫恩待约拿单的儿子{米非波设}，让他常与王同席吃饭。"
            },
            {
                "category": "撒下:大卫作王",
                "question": "谁告诉大卫关于米非波设的事？（扫罗的仆人）\n{洗巴}"
            },
            {
                "category": "撒下:犯罪与晚年",
                "question": "大卫与拔示巴犯奸淫时，她的丈夫{乌利亚}正在前线打仗。"
            },
            {
                "category": "撒下:犯罪与晚年",
                "question": "大卫借刀杀人，写信给元帅{约押}，让他把乌利亚派到阵势极险之处。"
            },
            {
                "category": "撒下:犯罪与晚年",
                "question": "先知拿单讲了一个富户抢夺穷人{羊羔}的比喻来指责大卫。"
            },
            {
                "category": "撒下:犯罪与晚年",
                "question": "大卫认罪后，神除掉了他的罪，但他与拔示巴生的第一个孩子必{死}。"
            },
            {
                "category": "撒下:犯罪与晚年",
                "question": "后来拔示巴生了{所罗门}，神喜爱他，赐名耶底底亚。"
            },
            {
                "category": "撒下:犯罪与晚年",
                "question": "大卫的长子{暗嫩}玷污了胞妹他玛。"
            },
            {
                "category": "撒下:犯罪与晚年",
                "question": "{押沙龙}为了报仇杀死了暗嫩，之后逃亡。"
            },
            {
                "category": "撒下:犯罪与晚年",
                "question": "押沙龙在城门口暗中得了以色列人的{心}，以此谋反。"
            },
            {
                "category": "撒下:犯罪与晚年",
                "question": "大卫逃离耶路撒冷时，{示每}（人名）沿路咒骂并拿石头砍他。"
            },
            {
                "category": "撒下:犯罪与晚年",
                "question": "大卫的谋士{亚希多弗}背叛归向押沙龙，后来上吊自杀。"
            },
            {
                "category": "撒下:犯罪与晚年",
                "question": "大卫的朋友{户筛}假意归顺押沙龙，破坏了亚希多弗的计谋。"
            },
            {
                "category": "撒下:犯罪与晚年",
                "question": "押沙龙骑骡子经过大橡树，被{头发}绕住挂在树上。"
            },
            {
                "category": "撒下:犯罪与晚年",
                "question": "虽然大卫嘱咐不可害押沙龙，但{约押}还是违命刺透了他的心。"
            },
            {
                "category": "撒下:犯罪与晚年",
                "question": "大卫因{数点百姓}（行为）招致瘟疫，死了七万人。"
            },
            {
                "category": "撒下:犯罪与晚年",
                "question": "神让大卫三选一的灾祸是：七年饥荒、三个月被敌人追赶、或三日{瘟疫}。"
            },
            {
                "category": "撒下:犯罪与晚年",
                "question": "大卫在{亚劳拿}的禾场筑坛献祭，瘟疫就止住了。"
            },
            {
                "category": "历史总结",
                "question": "撒母耳身兼三个职分：士师、祭司和{先知}。"
            },
            {
                "category": "历史总结",
                "question": "大卫一生三次受膏：伯利恒、希伯伦（作犹大王）、希伯伦（作{以色列王}）。"
            },
            {
                "category": "重点知识",
                "question": "扫罗被弃的三个原因：\n1. 违命未灭绝{亚玛力}人。\n2. 听命胜于{献祭}。\n3. {悖逆}与行邪术同罪。"
            },
            {
                "category": "重点知识",
                "question": "亚比该的三个智慧：\n1. 在危机中{迅速}反应。\n2. 用{柔和}的言语止息怒气。\n3. 拦阻大卫自己{伸冤}。"
            },
            {
                "category": "重点知识",
                "question": "大卫所犯的三个罪：\n1. 与拔示巴犯{奸淫}。\n2. 借刀杀人谋害{乌利亚}。\n3. {掩盖}罪恶。"
            },
            {
                "category": "重点知识",
                "question": "拿单对大卫的审判：\n1. {刀剑}必永不离开你的家。\n2. 妃嫔必在日光下被别人{羞辱}。\n3. 所得的孩子必定要{死}。"
            }
        ];

        // -----------------------------------------------------------------------------
        // 逻辑部分 (Shuffle, Parse, State)
        // -----------------------------------------------------------------------------
        const shuffle = (array) => {
            let currentIndex = array.length, randomIndex;
            while (currentIndex !== 0) {
                randomIndex = Math.floor(Math.random() * currentIndex);
                currentIndex--;
                [array[currentIndex], array[randomIndex]] = [array[randomIndex], array[currentIndex]];
            }
            return array;
        };

        const parseQuestion = (qStr, id) => {
            const lines = qStr.question.split('\n');
            const parsedLines = lines.map(line => {
                const parts = [];
                let lastIndex = 0;
                const regex = /\{([^}]+)\}/g;
                let match;
                while ((match = regex.exec(line)) !== null) {
                    if (match.index > lastIndex) {
                        parts.push({ type: 'text', content: line.substring(lastIndex, match.index) });
                    }
                    parts.push({ type: 'input', answer: match[1] });
                    lastIndex = regex.lastIndex;
                }
                if (lastIndex < line.length) {
                    parts.push({ type: 'text', content: line.substring(lastIndex) });
                }
                return parts;
            });
            return {
                id,
                category: qStr.category,
                isList: lines.length > 1 || qStr.type === 'list',
                lines: parsedLines,
            };
        };

        const parsedQuestions = rawQuestions.map((q, i) => parseQuestion(q, i));

        // -----------------------------------------------------------------------------
        // React App 组件
        // -----------------------------------------------------------------------------
        function App() {
            const [view, setView] = React.useState('home');
            const [questions, setQuestions] = React.useState([]);
            const [currentIndex, setCurrentIndex] = React.useState(0);
            const [inputs, setInputs] = React.useState({});
            const [showResult, setShowResult] = React.useState(false);
            const [wrongQuestionIds, setWrongQuestionIds] = React.useState(new Set());
            const [mode, setMode] = React.useState('all'); 
            const [stats, setStats] = React.useState({ correct: 0, total: 0 });
            const [showHint, setShowHint] = React.useState(false);

            React.useEffect(() => {
                const savedWrong = localStorage.getItem('hebrewHistory_wrongIds_final_v54');
                if (savedWrong) setWrongQuestionIds(new Set(JSON.parse(savedWrong)));
            }, []);

            React.useEffect(() => {
                localStorage.setItem('hebrewHistory_wrongIds_final_v54', JSON.stringify([...wrongQuestionIds]));
            }, [wrongQuestionIds]);

            const startExam = (newMode) => {
                setMode(newMode);
                let qPool = newMode === 'review' ? parsedQuestions.filter(q => wrongQuestionIds.has(q.id)) : [...parsedQuestions];
                if (newMode === 'review' && qPool.length === 0) return;
                setQuestions(shuffle(qPool));
                setCurrentIndex(0);
                setInputs({});
                setShowResult(false);
                setShowHint(false);
                setStats({ correct: 0, total: 0 });
                setView('quiz');
            };

            const goHome = () => setView('home');
            const handleInputChange = (key, value) => setInputs(prev => ({ ...prev, [key]: value }));

            // -------------------------------------------------------
            // 核心判题逻辑
            // -------------------------------------------------------
            const isAnswerCorrect = (userVal, correctVal, category) => {
                if (category === '经文默写') {
                    // 经文默写要求严格一点，但允许首尾空格
                    return (userVal || '').trim() === correctVal.trim();
                }

                let u = (userVal || '').trim().replace(/\s+/g, '').toLowerCase();
                let c = correctVal.trim().replace(/\s+/g, '').toLowerCase();
                
                if (!u) return false;
                if (u === c) return true;

                const equivalentGroups = [
                    ['1', '一'], ['2', '二', '两'], ['3', '三'], ['4', '四'], 
                    ['5', '五'], ['6', '六'], ['7', '七'], ['8', '八'], ['9', '九'], 
                    ['10', '十'], ['12', '十二'], ['15', '十五'], 
                    ['31', '三十一'], ['110', '一百一十'],
                    ['耶和华', '神', '上帝'],
                    ['纪念', '记念'],
                    ['起誓', '发誓'],
                    ['顺服', '顺服神', '顺服耶和华'],
                    ['不顺服', '不顺服神', '不顺服耶和华', '背逆'],
                    ['背景', '事件'],
                    ['意义', '含义'],
                    ['时间', '年代'],
                    ['求问耶和华', '求问神', '祷告', '求问'],
                    ['应许', '允许', '承诺'],
                    ['希伯伦', '希伯仑'],
                    ['母牛', '乳牛'],
                    ['石子', '石头', '甩石'],
                    ['剃头刀', '剃刀'],
                    ['隐多珥', '隐多尔']
                ];

                for (let group of equivalentGroups) {
                    if (group.includes(c) && group.includes(u)) return true;
                }

                if (u.includes(c)) return true;
                if (c.includes(u)) {
                    if (u.length >= 2) return true;
                    // 单字白名单
                    const singleCharWhitelist = ['王', '东', '滚', '死', '葬', '闪', '田', '城', '场', '家', '妾', '心', '疯', '枪', '驴', '8', '4'];
                    if (singleCharWhitelist.includes(u)) return true;
                    if (/^[\u4e00-\u9fa5]$/.test(u)) return true; // 允许单个汉字匹配
                }
                
                return false;
            };

            const checkCurrentQuestion = () => {
                const currentQ = questions[currentIndex];
                return currentQ.lines.every((line, lineIdx) => 
                    line.every((part, partIdx) => 
                        part.type !== 'input' || isAnswerCorrect(inputs[`${lineIdx}-${partIdx}`], part.answer, currentQ.category)
                    )
                );
            };

            const handleSubmit = () => {
                const isCorrect = checkCurrentQuestion();
                const currentQ = questions[currentIndex];
                setStats(prev => ({ ...prev, total: prev.total + 1, correct: prev.correct + (isCorrect ? 1 : 0) }));
                if (isCorrect) {
                    if (wrongQuestionIds.has(currentQ.id)) {
                        const newSet = new Set(wrongQuestionIds);
                        newSet.delete(currentQ.id);
                        setWrongQuestionIds(newSet);
                    }
                } else {
                    setWrongQuestionIds(prev => new Set(prev).add(currentQ.id));
                }
                setShowResult(true);
            };

            const nextQuestion = () => {
                if (currentIndex < questions.length - 1) {
                    setCurrentIndex(prev => prev + 1);
                    setInputs({});
                    setShowResult(false);
                    setShowHint(false);
                    setTimeout(() => document.querySelector('input')?.focus(), 100);
                } else {
                    setView('result');
                }
            };

            // --- 渲染部分 ---

            if (view === 'home') {
                return (
                    <div className="h-full flex flex-col items-center justify-center p-6 animate-slide-up">
                        <div className="w-24 h-24 bg-gradient-to-br from-indigo-600 to-purple-700 rounded-3xl shadow-xl flex items-center justify-center mb-6">
                            <Icons.Book className="w-12 h-12 text-white" />
                        </div>
                        <h1 className="text-3xl font-black text-slate-900 mb-2 tracking-tight">希伯来历史</h1>
                        <p className="text-lg text-slate-500 mb-10 text-center font-medium">全考点复习助手<br/>v5.4 键盘适配版 (下半学期)</p>

                        <div className="w-full max-w-sm space-y-4">
                            <button onClick={() => startExam('all')} className="w-full flex items-center p-6 bg-white border-2 border-indigo-50 rounded-2xl shadow-sm active:scale-95 transition-all group">
                                <div className="w-14 h-14 rounded-full bg-indigo-50 flex items-center justify-center text-indigo-700 mr-5 group-hover:bg-indigo-600 group-hover:text-white transition-colors">
                                    <Icons.RotateCcw className="w-8 h-8" />
                                </div>
                                <div className="text-left flex-1">
                                    <div className="font-bold text-slate-900 text-xl">全题库练习</div>
                                    <div className="text-slate-500 text-base">共 {parsedQuestions.length} 题</div>
                                </div>
                                <Icons.ChevronRight className="w-6 h-6 text-slate-300" />
                            </button>

                            <button onClick={() => startExam('review')} disabled={wrongQuestionIds.size === 0} 
                                className={`w-full flex items-center p-6 border-2 rounded-2xl shadow-sm active:scale-95 transition-all group ${wrongQuestionIds.size > 0 ? 'bg-white border-pink-50 cursor-pointer' : 'bg-slate-100 border-transparent opacity-60 cursor-not-allowed'}`}>
                                <div className={`w-14 h-14 rounded-full flex items-center justify-center mr-5 transition-colors ${wrongQuestionIds.size > 0 ? 'bg-pink-50 text-pink-600 group-hover:bg-pink-600 group-hover:text-white' : 'bg-slate-200 text-slate-400'}`}>
                                    <Icons.AlertCircle className="w-8 h-8" />
                                </div>
                                <div className="text-left flex-1">
                                    <div className="font-bold text-slate-900 text-xl">复习错题本</div>
                                    <div className="text-slate-500 text-base">{wrongQuestionIds.size > 0 ? `${wrongQuestionIds.size} 题待复习` : '暂无错题'}</div>
                                </div>
                                {wrongQuestionIds.size > 0 && <Icons.ChevronRight className="w-6 h-6 text-slate-300" />}
                            </button>
                        </div>
                    </div>
                );
            }

            if (view === 'result') {
                return (
                    <div className="h-full flex flex-col items-center justify-center p-6 animate-slide-up">
                        <div className="w-24 h-24 bg-green-100 text-green-600 rounded-full flex items-center justify-center mb-6">
                            <Icons.Award className="w-12 h-12" />
                        </div>
                        <h2 className="text-3xl font-black text-slate-900 mb-2">练习完成!</h2>
                        <p className="text-lg text-slate-500 mb-10 font-bold">正确率 {stats.total === 0 ? 0 : Math.round((stats.correct/stats.total)*100)}%</p>
                        
                        <div className="grid grid-cols-2 gap-4 w-full max-w-sm mb-10">
                            <div className="bg-white border-2 border-indigo-50 p-6 rounded-2xl text-center shadow-sm">
                                <div className="text-sm font-black text-indigo-500 uppercase mb-1">答对</div>
                                <div className="text-4xl font-black text-slate-900">{stats.correct}</div>
                            </div>
                            <div className="bg-white border-2 border-pink-50 p-6 rounded-2xl text-center shadow-sm">
                                <div className="text-sm font-black text-pink-500 uppercase mb-1">错题</div>
                                <div className="text-4xl font-black text-slate-900">{wrongQuestionIds.size}</div>
                            </div>
                        </div>

                        <div className="w-full max-w-sm space-y-4">
                            <button onClick={() => startExam('review')} disabled={wrongQuestionIds.size === 0} className="w-full py-5 bg-pink-700 text-white rounded-2xl text-xl font-bold disabled:opacity-50 shadow-lg shadow-pink-200 active:scale-95 transition-transform flex items-center justify-center gap-2">
                                <Icons.RotateCcw className="w-6 h-6"/> 继续攻克错题
                            </button>
                            <button onClick={goHome} className="w-full py-5 bg-white border-2 border-slate-200 text-slate-700 rounded-2xl text-xl font-bold hover:bg-slate-50 active:scale-95 transition-transform flex items-center justify-center gap-2">
                                <Icons.Home className="w-6 h-6"/> 返回首页
                            </button>
                        </div>
                    </div>
                );
            }

            const currentQ = questions[currentIndex];
            if (!currentQ) return <div className="h-full flex items-center justify-center text-slate-400 text-xl font-bold">正在加载题库...</div>;

            return (
                <div className="flex flex-col h-screen bg-slate-100 font-sans">
                    <header className="flex-none h-14 glass-header border-b border-slate-200/60 shadow-sm z-20">
                        <div className="max-w-2xl mx-auto px-4 h-full flex items-center justify-between">
                            <button onClick={goHome} className="p-2 -ml-2 text-slate-500 hover:text-indigo-600 rounded-xl flex items-center gap-1 active:bg-slate-100">
                                <Icons.ArrowLeft className="w-5 h-5"/> <span className="text-sm font-bold">首页</span>
                            </button>
                            <div className="text-sm font-bold text-slate-800">
                                {currentIndex + 1} <span className="text-slate-300 mx-1">/</span> {questions.length}
                            </div>
                            <span className={`px-2 py-1 rounded-lg text-xs font-black ${mode === 'review' ? 'bg-pink-100 text-pink-700' : 'bg-indigo-100 text-indigo-700'}`}>
                                {mode === 'review' ? '错题' : '全题'}
                            </span>
                        </div>
                        <div className="h-1 bg-slate-200/50 w-full"><div className="h-full bg-indigo-600 transition-all duration-300 rounded-r-full" style={{ width: `${((currentIndex + 1) / questions.length) * 100}%` }}></div></div>
                    </header>

                    <main className="flex-1 overflow-y-auto p-4 pb-20 scroll-smooth">
                         <div className="w-full max-w-2xl mx-auto flex flex-col gap-6">
                            
                            {/* Card */}
                            <div className="bg-white rounded-2xl shadow-md border border-slate-100 overflow-hidden animate-slide-up">
                                <div className="bg-slate-50/80 px-4 py-3 border-b border-slate-100 flex justify-between items-center">
                                    <span className="text-[10px] font-black text-slate-400 uppercase tracking-widest">题目类型</span>
                                    <span className="text-[10px] font-black text-indigo-700 bg-indigo-50 px-2 py-1 rounded-lg border border-indigo-100">{currentQ.category}</span>
                                </div>
                                
                                <div className="p-6 sm:p-10">
                                    <div className="flex flex-col justify-center gap-4 sm:gap-6 min-h-0">
                                        {currentQ.lines.map((line, lIdx) => (
                                            <div key={lIdx} className={`text-lg sm:text-xl leading-loose ${currentQ.isList && lIdx === 0 && !line.some(p => p.type === 'input') ? 'font-bold text-slate-900 border-b-2 border-slate-100 pb-2 mb-2' : 'font-semibold text-slate-700'}`}>
                                                {line.map((part, pIdx) => {
                                                    if (part.type === 'text') return <span key={pIdx}>{part.content}</span>;
                                                    const key = `${lIdx}-${pIdx}`;
                                                    const val = inputs[key] || '';
                                                    const correct = isAnswerCorrect(val, part.answer, currentQ.category);
                                                    if (showResult) return (
                                                        <span key={pIdx} className={`mx-1 px-2 py-0.5 rounded-lg border inline-flex items-center gap-1 align-middle text-base ${correct ? 'bg-green-50 border-green-200 text-green-800' : 'bg-red-50 border-red-200 text-red-800 line-through decoration-2'}`}>
                                                            {!correct && <span className="absolute -mt-8 bg-white text-xs text-red-600 px-2 py-0.5 border border-red-100 rounded shadow whitespace-nowrap z-10 font-bold">你的答案: {val || '空'}</span>}
                                                            {part.answer}
                                                        </span>
                                                    );
                                                    return (
                                                        <input key={pIdx} type="text" value={val} onChange={e => handleInputChange(key, e.target.value)} 
                                                            placeholder={showHint ? part.answer : ''}
                                                            className={`mx-1 min-w-[60px] w-[100px] sm:w-[140px] h-10 sm:h-12 text-center text-lg font-bold border-b-2 sm:border-b-4 bg-indigo-50/40 focus:bg-white focus:outline-none rounded-t-lg px-1 text-indigo-900 input-highlight transition-all placeholder:text-slate-300 align-middle ${val ? 'border-indigo-500 bg-indigo-50/60' : 'border-slate-300'}`} 
                                                            autoComplete="off" 
                                                        />
                                                    );
                                                })}
                                            </div>
                                        ))}
                                    </div>
                                    
                                    {showResult && (
                                        <div className={`mt-6 p-4 rounded-xl flex items-center gap-3 border animate-slide-up flex-none ${checkCurrentQuestion() ? 'bg-green-50 border-green-100 text-green-900' : 'bg-red-50 border-red-100 text-red-900'}`}>
                                            <div className="shrink-0">{checkCurrentQuestion() ? <Icons.Check className="w-6 h-6"/> : <Icons.X className="w-6 h-6"/>}</div>
                                            <div>
                                                <div className="font-bold text-sm">{checkCurrentQuestion() ? '回答正确' : '回答错误'}</div>
                                                <div className="text-xs font-medium opacity-80 mt-1">{checkCurrentQuestion() ? '记忆准确！' : '正确答案已显示，已加入错题本。'}</div>
                                            </div>
                                        </div>
                                    )}
                                </div>
                            </div>

                            {/* Buttons - Lifted into scroll view */}
                            <div className="flex gap-3 pb-8">
                                <button onClick={() => setShowHint(!showHint)} className="w-16 h-16 bg-white rounded-2xl flex items-center justify-center text-slate-500 hover:bg-slate-50 active:scale-95 transition-transform border border-slate-200 shadow-sm" title="偷看答案">
                                    {showHint ? <Icons.EyeOff className="w-8 h-8"/> : <Icons.Eye className="w-8 h-8"/>}
                                </button>
                                {showResult ? (
                                    <button onClick={nextQuestion} className="flex-1 h-16 bg-indigo-600 text-white rounded-2xl font-bold text-xl shadow-lg shadow-indigo-200 active:scale-[0.98] transition-all flex items-center justify-center gap-2">
                                        下一题 <Icons.ChevronRight className="w-6 h-6"/>
                                    </button>
                                ) : (
                                    <button onClick={handleSubmit} className="flex-1 h-16 bg-slate-900 text-white rounded-2xl font-bold text-xl shadow-lg shadow-slate-300 active:scale-[0.98] transition-all flex items-center justify-center gap-2">
                                        <Icons.Save className="w-6 h-6"/> 提交答案
                                    </button>
                                )}
                            </div>

                         </div>
                    </main>
                </div>
            );
        }

        ReactDOM.createRoot(document.getElementById('root')).render(<App />);
    </script>
</body>
</html>
