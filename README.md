
# yanyun-shefu-helper<!DOCTYPE html>
<html lang="zh-TW">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
  <title>燕雲十六聲 - 射覆解謎助手</title>
  
  <!-- Tailwind CSS -->
  <script src="https://cdn.tailwindcss.com"></script>
  <!-- FontAwesome Icons -->
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
  <!-- Alpine.js -->
  <script defer src="https://unpkg.com/alpinejs@3.x.x/dist/cdn.min.js"></script>
  
  <!-- Google Fonts: Noto Serif TC (明朝體) -->
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Noto+Serif+TC:wght@400;500;700;900&display=swap" rel="stylesheet">

  <!-- PWA Setup: Manifest injected as Data URL -->
  <script>
    // PWA Manifest Configuration
    const manifest = {
      "name": "燕雲十六聲 - 射覆助手",
      "short_name": "射覆助手",
      "description": "燕雲十六聲水墨風射覆解謎輔助工具",
      "start_url": "./",
      "display": "standalone",
      "background_color": "#FDFCF6",
      "theme_color": "#2A5D54",
      "icons": [
        {
          "src": "data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCA1MTIgNTEyIiBmaWxsPSIjMkE1RDU0Ij48cGF0aCBkPSJNMjU2IDUxMkEyNTYgMjU2IDAgMSAwIDI1NiAwYTI1NiAyNTYgMCAxIDAgMCA1MTJ6TTM2OSAxNzVjMTEuNSAxMS41IDExLjUgMzAuMSAwIDQxLjZMMjY2LjkgMzE4LjlDMjU5LjcgMzI2LjEgMjUwLjEgMzMwLjEgMjQwIDMzMC4xczE5LjctNCAxMi41LTExLjJMMTA5LjkgMjE2LjdjLTExLjUtMTEuNS0xMS41LTMwLjEgMC00MS42czMwLjEtMTEuNSA0MS42IDBMMjQwIDI0MC42bDEwNC40LTEwNi4xYzExLjUtMTEuNSAzMC4xLTExLjUgNDEuNiAweiIvPjwvc3ZnPg==",
          "sizes": "192x192",
          "type": "image/svg+xml",
          "purpose": "any maskable"
        },
        {
          "src": "data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCA1MTIgNTEyIiBmaWxsPSIjMkE1RDU0Ij48cGF0aCBkPSJNMjU2IDUxMkEyNTYgMjU2IDAgMSAwIDI1NiAwYTI1NiAyNTYgMCAxIDAgMCA1MTJ6TTM2OSAxNzVjMTEuNSAxMS41IDExLjUgMzAuMSAwIDQxLjZMMjY2LjkgMzE4LjlDMjU5LjcgMzI2LjEgMjUwLjEgMzMwLjEgMjQwIDMzMC4xczE5LjctNCAxMi41LTExLjJMMTA5LjkgMjE2LjdjLTExLjUtMTEuNS0xMS41LTMwLjEgMC00MS42czMwLjEtMTEuNSA0MS42IDBMMjQwIDI0MC42bDEwNC40LTEwNi4xYzExLjUtMTEuNSAzMC4xLTExLjUgNDEuNiAweiIvPjwvc3ZnPg==",
          "sizes": "512x512",
          "type": "image/svg+xml",
          "purpose": "any maskable"
        }
      ]
    };
    
    // Inject Manifest Link
    const stringManifest = JSON.stringify(manifest);
    const blob = new Blob([stringManifest], {type: 'application/json'});
    const manifestURL = URL.createObjectURL(blob);
    const link = document.createElement('link');
    link.rel = 'manifest';
    link.href = manifestURL;
    document.head.appendChild(link);

    // Meta tags for PWA / iOS support
    document.write('<meta name="theme-color" content="#2A5D54">');
    document.write('<meta name="apple-mobile-web-app-capable" content="yes">');
    document.write('<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">');
    document.write('<meta name="apple-mobile-web-app-title" content="射覆助手">');
  </script>

  <!-- Service Worker Registration -->
  <script>
    if ('serviceWorker' in navigator) {
      window.addEventListener('load', () => {
        // Create a basic service worker script as a blob
        const swScript = `
          const CACHE_NAME = 'yanyun-shefu-v1';
          const urlsToCache = [
            './',
            'https://cdn.tailwindcss.com',
            'https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css',
            'https://unpkg.com/alpinejs@3.x.x/dist/cdn.min.js',
            'https://fonts.googleapis.com/css2?family=Noto+Serif+TC:wght@400;500;700;900&display=swap'
          ];

          self.addEventListener('install', event => {
            event.waitUntil(
              caches.open(CACHE_NAME)
                .then(cache => cache.addAll(urlsToCache))
            );
          });

          self.addEventListener('fetch', event => {
            event.respondWith(
              caches.match(event.request)
                .then(response => {
                  // Return cached response if found
                  if (response) {
                    return response;
                  }
                  // Otherwise fetch from network
                  return fetch(event.request);
                })
            );
          });
        `;
        const blob = new Blob([swScript], { type: 'application/javascript' });
        const swUrl = URL.createObjectURL(blob);

        navigator.serviceWorker.register(swUrl)
          .then(registration => {
            console.log('ServiceWorker registration successful with scope: ', registration.scope);
          })
          .catch(err => {
            console.log('ServiceWorker registration failed: ', err);
          });
      });
    }
  </script>

  <script>
    tailwind.config = {
      theme: {
        extend: {
          colors: {
            // 水墨武俠淺色主題調色盤
            ink: {
              bg: '#FDFCF6',       // 杏白 - 整體底色
              card: '#F6F5ED',     // 稍深的杏白 - 卡片底色
              border: '#E8E4D9',   // 邊框色
              green: '#2A5D54',    // 竹青 - 主要強調色/標題
              greenLight: '#3F7A70',// 竹青 (淺)
              brown: '#6B4A3A',    // 棕褐 - 次要強調色/答案
              brownDark: '#4A3226', // 棕褐 (深)
              textMain: '#333333', // 主要文字色
              textMuted: '#666666' // 次要文字色
            }
          },
          fontFamily: {
            // 設定為明朝體
            serif: ['"Noto Serif TC"', 'serif'],
          },
          fontSize: {
            // 自訂字體大小
            'content': '12pt',
            'title': '14pt',
          },
          boxShadow: {
            'ink-soft': '0 4px 12px rgba(107, 74, 58, 0.08)',
            'ink-strong': '0 4px 16px rgba(42, 93, 84, 0.15)',
          }
        }
      }
    }
  </script>
  <style>
    /* Mobile tap highlight reset */
    * { -webkit-tap-highlight-color: transparent; }
    /* Custom scrollbar for horizontal category filter */
    .no-scrollbar::-webkit-scrollbar { display: none; }
    .no-scrollbar { -ms-overflow-style: none; scrollbar-width: none; }
    
    body {
      /* 強制全局使用明朝體 */
      font-family: 'Noto Serif TC', serif;
    }

    /* 水墨風格裝飾線 */
    .ink-divider {
      height: 1px;
      background: linear-gradient(90deg, transparent 0%, rgba(107,74,58,0.2) 50%, transparent 100%);
      margin: 1rem 0;
    }
  </style>
</head>

<body class="bg-ink-bg text-ink-textMain min-h-screen pb-20 font-serif antialiased selection:bg-ink-green selection:text-white" x-data="appData()">

  <script>
    const DEFAULT_DATABASE = [
      // 一、 歷史人物、文學與文化典故
      { category: "歷史人物與典故", clue: "七劍", ans: "左冷禪" },
      { category: "歷史人物與典故", clue: "七縱", ans: "孟獲" },
      { category: "歷史人物與典故", clue: "七夕", ans: "鵲橋" },
      { category: "歷史人物與典故", clue: "九陰", ans: "梅超風" },
      { category: "歷史人物與典故", clue: "九黎", ans: "蚩尤" },
      { category: "歷史人物與典故", clue: "入川", ans: "徐庶" },
      { category: "歷史人物與典故", clue: "八卦", ans: "伏羲" },
      { category: "歷史人物與典故", clue: "八門", ans: "張遼" },
      { category: "歷史人物與典故", clue: "十三", ans: "孫子" },
      { category: "歷史人物與典故", clue: "丈八", ans: "張飛" },
      { category: "歷史人物與典故", clue: "三五", ans: "賈詡" },
      { category: "歷史人物與典故", clue: "不老", ans: "天山童姥" },
      { category: "歷史人物與典故", clue: "口舌", ans: "諸葛瑾" },
      { category: "歷史人物與典故", clue: "大理", ans: "段正淳" },
      { category: "歷史人物與典故", clue: "大禹", ans: "治水" },
      { category: "歷史人物與典故", clue: "小石", ans: "柳宗元" },
      { category: "歷史人物與典故", clue: "小廝", ans: "賈璉" },
      { category: "歷史人物與典故", clue: "中興", ans: "劉秀/宋高宗" },
      { category: "歷史人物與典故", clue: "五官", ans: "曹丕" },
      { category: "歷史人物與典故", clue: "仁君", ans: "堯" },
      { category: "歷史人物與典故", clue: "仁義", ans: "孟子" },
      { category: "歷史人物與典故", clue: "內史", ans: "蒙毅" },
      { category: "歷史人物與典故", clue: "內助", ans: "獨孤伽羅" },
      { category: "歷史人物與典故", clue: "六一", ans: "歐陽修" },
      { category: "歷史人物與典故", clue: "太史", ans: "司馬遷" },
      { category: "歷史人物與典故", clue: "太宗", ans: "趙光義" },
      { category: "歷史人物與典故", clue: "文王", ans: "姬昌" },
      { category: "歷史人物與典故", clue: "文和", ans: "賈詡" },
      { category: "歷史人物與典故", clue: "文獻", ans: "獨孤伽羅" },
      { category: "歷史人物與典故", clue: "方士", ans: "徐福" },
      { category: "歷史人物與典故", clue: "北海", ans: "蘇武/太史慈" },
      { category: "歷史人物與典故", clue: "半山", ans: "王安石" },
      { category: "歷史人物與典故", clue: "古之", ans: "洪七公" },
      { category: "歷史人物與典故", clue: "司徒", ans: "王允" },
      { category: "歷史人物與典故", clue: "四世", ans: "袁紹" },
      { category: "歷史人物與典故", clue: "四鎮", ans: "朱溫" },
      { category: "歷史人物與典故", clue: "失衡", ans: "馬謖" },
      { category: "歷史人物與典故", clue: "玄妙", ans: "老子" },
      { category: "歷史人物與典故", clue: "玄鳥", ans: "商湯" },
      { category: "歷史人物與典故", clue: "瓦崗", ans: "程咬金" },
      { category: "歷史人物與典故", clue: "生死", ans: "天山童姥" },
      { category: "歷史人物與典故", clue: "仲父", ans: "呂不韋" },
      { category: "歷史人物與典故", clue: "丞相", ans: "李斯" },
      { category: "歷史人物與典故", clue: "地府", ans: "判官" },
      { category: "歷史人物與典故", clue: "奸臣", ans: "高俅" },
      { category: "歷史人物與典故", clue: "好漢", ans: "柴進" },
      { category: "歷史人物與典故", clue: "成語", ans: "沉魚落雁/狐假虎威/畫蛇添足" },
      { category: "歷史人物與典故", clue: "曳落", ans: "安祿山" },
      { category: "歷史人物與典故", clue: "江湖", ans: "司空摘星" },
      { category: "歷史人物與典故", clue: "西域", ans: "張騫/班超" },
      { category: "歷史人物與典故", clue: "西涼", ans: "馬超" },
      { category: "歷史人物與典故", clue: "西湖", ans: "白素貞" },
      { category: "歷史人物與典故", clue: "西醉", ans: "黃鶴樓" },
      { category: "歷史人物與典故", clue: "兵法", ans: "孫臏" },
      { category: "歷史人物與典故", clue: "兵敗", ans: "項梁" },
      { category: "歷史人物與典故", clue: "呆霸王", ans: "薛蟠" },
      { category: "歷史人物與典故", clue: "孝聖", ans: "舜" },
      { category: "歷史人物與典故", clue: "孝賢", ans: "伯邑考" },
      { category: "歷史人物與典故", clue: "序文", ans: "王羲之" },
      { category: "歷史人物與典故", clue: "弄權", ans: "王熙鳳" },
      { category: "歷史人物與典故", clue: "快意", ans: "蕭十一郎" },
      { category: "歷史人物與典故", clue: "抄檢", ans: "王夫人" },
      { category: "歷史人物與典故", clue: "汨羅", ans: "屈原" },
      { category: "歷史人物與典故", clue: "沉魚", ans: "西施" },
      { category: "歷史人物與典故", clue: "狂客", ans: "賀知章" },
      { category: "歷史人物與典故", clue: "角色", ans: "判官、月老" },
      { category: "歷史人物與典故", clue: "赤帝", ans: "祝融" },
      { category: "歷史人物與典故", clue: "赤壁", ans: "蘇軾" },
      { category: "歷史人物與典故", clue: "亞子", ans: "李存勗" },
      { category: "歷史人物與典故", clue: "京口", ans: "劉裕" },
      { category: "歷史人物與典故", clue: "使楚", ans: "晏子" },
      { category: "歷史人物與典故", clue: "侍寢", ans: "襲人" },
      { category: "歷史人物與典故", clue: "兩界", ans: "劉伯欽" },
      { category: "歷史人物與典故", clue: "典故", ans: "三顧茅廬/田忌賽馬/大禹治水/四面楚歌" },
      { category: "歷史人物與典故", clue: "刺客", ans: "荊軻" },
      { category: "歷史人物與典故", clue: "呼延", ans: "秦明" },
      { category: "歷史人物與典故", clue: "奇門", ans: "黃藥師" },
      { category: "歷史人物與典故", clue: "姑蘇", ans: "夫差" },
      { category: "歷史人物與典故", clue: "孟德", ans: "曹操" },
      { category: "歷史人物與典故", clue: "定軍", ans: "夏侯淵" },
      { category: "歷史人物與典故", clue: "岳陽", ans: "范仲淹" },
      { category: "歷史人物與典故", clue: "弦歌", ans: "太史慈" },
      { category: "歷史人物與典故", clue: "征途", ans: "楊素" },
      { category: "歷史人物與典故", clue: "忠奸", ans: "伍子胥" },
      { category: "歷史人物與典故", clue: "忠義", ans: "宋江" },
      { category: "歷史人物與典故", clue: "忠誠", ans: "阮明正" },
      { category: "歷史人物與典故", clue: "忠魂", ans: "屈原" },
      { category: "歷史人物與典故", clue: "昆陽", ans: "劉秀" },
      { category: "歷史人物與典故", clue: "武安", ans: "白起" },
      { category: "歷史人物與典故", clue: "武成", ans: "王翦" },
      { category: "歷史人物與典故", clue: "武當", ans: "張三豐" },
      { category: "歷史人物與典故", clue: "河北", ans: "顏良" },
      { category: "歷史人物與典故", clue: "法家", ans: "李斯" },
      { category: "歷史人物與典故", clue: "狐媚", ans: "蘇妲己" },
      { category: "歷史人物與典故", clue: "宣和", ans: "宋徽宗" },
      { category: "歷史人物與典故", clue: "宦官", ans: "趙高" },
      { category: "歷史人物與典故", clue: "後德", ans: "衛子夫" },
      { category: "歷史人物與典故", clue: "怒目", ans: "典韋" },
      { category: "歷史人物與典故", clue: "故事", ans: "西天取經" },
      { category: "歷史人物與典故", clue: "洛神", ans: "曹植" },
      { category: "歷史人物與典故", clue: "洛陽", ans: "孫堅" },
      { category: "歷史人物與典故", clue: "相秦", ans: "范雎" },
      { category: "歷史人物與典故", clue: "科聖", ans: "張衡" },
      { category: "歷史人物與典故", clue: "英魂", ans: "孫策" },
      { category: "歷史人物與典故", clue: "陌巷", ans: "顏回" },
      { category: "歷史人物與典故", clue: "降洞", ans: "賈寶玉" },
      { category: "歷史人物與典故", clue: "修史", ans: "班固" },
      { category: "歷史人物與典故", clue: "傲世", ans: "阮籍" },
      { category: "歷史人物與典故", clue: "詠懷", ans: "阮籍" },
      { category: "歷史人物與典故", clue: "貴妃", ans: "楊玉環" },
      { category: "歷史人物與典故", clue: "進學", ans: "韓愈" },
      { category: "歷史人物與典故", clue: "開元", ans: "李隆基" },
      { category: "歷史人物與典故", clue: "開封", ans: "包拯" },
      { category: "歷史人物與典故", clue: "雄才", ans: "劉徹" },
      { category: "歷史人物與典故", clue: "集序", ans: "上官婉兒" },
      { category: "歷史人物與典故", clue: "菩薩", ans: "夏啟" },
      { category: "歷史人物與典故", clue: "詞客", ans: "晏幾道" },
      { category: "歷史人物與典故", clue: "傳道", ans: "孔子" },
      { category: "歷史人物與典故", clue: "愛蓮", ans: "周敦頤" },
      { category: "歷史人物與典故", clue: "敬德", ans: "秦瓊" },
      { category: "歷史人物與典故", clue: "新朝", ans: "王莽" },
      { category: "歷史人物與典故", clue: "暗算", ans: "龐涓" },
      { category: "歷史人物與典故", clue: "楚柱", ans: "項梁" },
      { category: "歷史人物與典故", clue: "熙寧", ans: "王安石" },
      { category: "歷史人物與典故", clue: "義釋", ans: "關勝" },
      { category: "歷史人物與典故", clue: "虞兮", ans: "虞姬" },
      { category: "歷史人物與典故", clue: "虞美", ans: "李煜" },
      { category: "歷史人物與典故", clue: "詩詞", ans: "唐婉" },
      { category: "歷史人物與典故", clue: "聚義", ans: "晁蓋" },
      { category: "歷史人物與典故", clue: "腐刑", ans: "司馬遷" },
      { category: "歷史人物與典故", clue: "輕敵", ans: "李信" },
      { category: "歷史人物與典故", clue: "運河", ans: "楊廣" },
      { category: "歷史人物與典故", clue: "漢化", ans: "拓跋宏" },
      { category: "歷史人物與典故", clue: "漢光", ans: "劉秀" },
      { category: "歷史人物與典故", clue: "漢將", ans: "鍾離眜" },
      { category: "歷史人物與典故", clue: "漢舞", ans: "趙飛燕" },
      { category: "歷史人物與典故", clue: "獄中", ans: "華佗" },
      { category: "歷史人物與典故", clue: "獄詠", ans: "駱賓王" },
      { category: "歷史人物與典故", clue: "精忠", ans: "岳飛" },
      { category: "歷史人物與典故", clue: "慕容", ans: "鳳凰" },
      { category: "歷史人物與典故", clue: "暴虐", ans: "商紂王" },
      { category: "歷史人物與典故", clue: "賢回", ans: "顏回" },
      { category: "歷史人物與典故", clue: "賦文", ans: "向秀" },
      { category: "歷史人物與典故", clue: "醉鄉", ans: "劉伶" },
      { category: "歷史人物與典故", clue: "遺計", ans: "郭嘉" },
      { category: "歷史人物與典故", clue: "霓裳", ans: "李隆基" },
      { category: "歷史人物與典故", clue: "龍城", ans: "衛青" },
      { category: "歷史人物與典故", clue: "龍裔", ans: "黃帝" },
      { category: "歷史人物與典故", clue: "龍圖", ans: "包拯" },
      { category: "歷史人物與典故", clue: "濡須", ans: "諸葛瑾" },
      { category: "歷史人物與典故", clue: "簡貴", ans: "王戎" },
      { category: "歷史人物與典故", clue: "獵戶", ans: "劉伯欽" },
      { category: "歷史人物與典故", clue: "典當", ans: "邢岫煙" },
      { category: "歷史人物與典故", clue: "鎖壓", ans: "法海" },
      { category: "歷史人物與典故", clue: "巧解", ans: "遊坦之" },
      { category: "歷史人物與典故", clue: "額雀", ans: "王之渙" },

      // 二、 動物、昆蟲與水族生物
      { category: "動物昆蟲與水族", clue: "力大", ans: "熊" },
      { category: "動物昆蟲與水族", clue: "人猿", ans: "猩猩" },
      { category: "動物昆蟲與水族", clue: "大口", ans: "牛蛙" },
      { category: "動物昆蟲與水族", clue: "大麥克", ans: "河馬" },
      { category: "動物昆蟲與水族", clue: "大眼", ans: "鹿" },
      { category: "動物昆蟲與水族", clue: "山林", ans: "狐狸/竹鼠/熊" },
      { category: "動物昆蟲與水族", clue: "小丑魚", ans: "共生/海中" },
      { category: "動物昆蟲與水族", clue: "小馬", ans: "薩摩耶" },
      { category: "動物昆蟲與水族", clue: "不詳", ans: "烏鴉" },
      { category: "動物昆蟲與水族", clue: "中型犬種", ans: "薩摩耶" },
      { category: "動物昆蟲與水族", clue: "五角", ans: "海星" },
      { category: "動物昆蟲與水族", clue: "水底", ans: "清道夫魚" },
      { category: "動物昆蟲與水族", clue: "水域", ans: "水豚" },
      { category: "動物昆蟲與水族", clue: "水稻", ans: "水牛" },
      { category: "動物昆蟲與水族", clue: "水邊", ans: "白鷺" },
      { category: "動物昆蟲與水族", clue: "冬眠", ans: "土撥鼠/熊" },
      { category: "動物昆蟲與水族", clue: "北極", ans: "海象/海獅" },
      { category: "動物昆蟲與水族", clue: "叮咬", ans: "蚊子" },
      { category: "動物昆蟲與水族", clue: "平天", ans: "牛魔王" },
      { category: "動物昆蟲與水族", clue: "打雷", ans: "鰻魚" },
      { category: "動物昆蟲與水族", clue: "打鳴", ans: "公雞" },
      { category: "動物昆蟲與水族", clue: "用翅膀游泳", ans: "企鵝" },
      { category: "動物昆蟲與水族", clue: "田野", ans: "兔子" },
      { category: "動物昆蟲與水族", clue: "白毛", ans: "薩摩耶" },
      { category: "動物昆蟲與水族", clue: "白羽", ans: "白鷺" },
      { category: "動物昆蟲與水族", clue: "白肉", ans: "鱈魚" },
      { category: "動物昆蟲與水族", clue: "白色鳥類", ans: "白鷺" },
      { category: "動物昆蟲與水族", clue: "再生", ans: "海星" },
      { category: "動物昆蟲與水族", clue: "冰面", ans: "海豹" },
      { category: "動物昆蟲與水族", clue: "地上", ans: "螞蟻" },
      { category: "動物昆蟲與水族", clue: "地下", ans: "鼴鼠/蚯蚓" },
      { category: "動物昆蟲與水族", clue: "多足", ans: "蜈蚣" },
      { category: "動物昆蟲與水族", clue: "多臂", ans: "海星" },
      { category: "動物昆蟲與水族", clue: "尖刺", ans: "刺蝟" },
      { category: "動物昆蟲與水族", clue: "吞食", ans: "鯰魚" },
      { category: "動物昆蟲與水族", clue: "吸附", ans: "壁虎" },
      { category: "動物昆蟲與水族", clue: "池塘", ans: "蝌蚪/牛蛙/鴨子" },
      { category: "動物昆蟲與水族", clue: "池塘（動物）", ans: "蝌蚪" },
      { category: "動物昆蟲與水族", clue: "灰色犬種", ans: "雪納瑞" },
      { category: "動物昆蟲與水族", clue: "灰色鳥類", ans: "杜鵑" },
      { category: "動物昆蟲與水族", clue: "羊毛", ans: "綿羊" },
      { category: "動物昆蟲與水族", clue: "自衛", ans: "臭鼬" },
      { category: "動物昆蟲與水族", clue: "西海", ans: "小白龍" },
      { category: "動物昆蟲與水族", clue: "冷水", ans: "鱈魚" },
      { category: "動物昆蟲與水族", clue: "冷血", ans: "蛇" },
      { category: "動物昆蟲與水族", clue: "冷淡注視", ans: "蛇" },
      { category: "動物昆蟲與水族", clue: "利爪", ans: "猞猁" },
      { category: "動物昆蟲與水族", clue: "尾鉤", ans: "蠍子" },
      { category: "動物昆蟲與水族", clue: "巡弋", ans: "鯊魚" },
      { category: "動物昆蟲與水族", clue: "快跑", ans: "鴕鳥" },
      { category: "動物昆蟲與水族", clue: "育兒", ans: "海馬" },
      { category: "動物昆蟲與水族", clue: "貝殼", ans: "珍珠" },
      { category: "動物昆蟲與水族", clue: "夜行", ans: "貓頭鷹" },
      { category: "動物昆蟲與水族", clue: "夜晚", ans: "老鼠/蚊子" },
      { category: "動物昆蟲與水族", clue: "奔跑", ans: "鴕鳥/斑馬" },
      { category: "動物昆蟲與水族", clue: "指鹿", ans: "趙高" },
      { category: "動物昆蟲與水族", clue: "挖洞藏頭", ans: "鴕鳥" },
      { category: "動物昆蟲與水族", clue: "洄游", ans: "大馬哈魚" },
      { category: "動物昆蟲與水族", clue: "狩獵", ans: "猞猁" },
      { category: "動物昆蟲與水族", clue: "看家", ans: "狗" },
      { category: "動物昆蟲與水族", clue: "紅冠家禽", ans: "公雞" },
      { category: "動物昆蟲與水族", clue: "紅眼睛", ans: "兔兒" },
      { category: "動物昆蟲與水族", clue: "負重", ans: "烏龜" },
      { category: "動物昆蟲與水族", clue: "面具", ans: "浣熊" },
      { category: "動物昆蟲與水族", clue: "飛渡", ans: "羚羊" },
      { category: "動物昆蟲與水族", clue: "食根", ans: "豪豬" },
      { category: "動物昆蟲與水族", clue: "家禽", ans: "公雞" },
      { category: "動物昆蟲與水族", clue: "哨壁", ans: "金絲猴" },
      { category: "動物昆蟲與水族", clue: "庭院", ans: "雞" },
      { category: "動物昆蟲與水族", clue: "振翅", ans: "蟋蟀" },
      { category: "動物昆蟲與水族", clue: "捕魚", ans: "鸕鶿/水獺" },
      { category: "動物昆蟲與水族", clue: "捕鼠", ans: "貓貓" },
      { category: "動物昆蟲與水族", clue: "海中", ans: "小丑魚/河豚" },
      { category: "動物昆蟲與水族", clue: "海岸", ans: "海獅" },
      { category: "動物昆蟲與水族", clue: "海底", ans: "比目魚" },
      { category: "動物昆蟲與水族", clue: "海洋", ans: "海牛/海龜/海豚/鯨" },
      { category: "動物昆蟲與水族", clue: "海草", ans: "海馬" },
      { category: "動物昆蟲與水族", clue: "海陸", ans: "烏龜" },
      { category: "動物昆蟲與水族", clue: "海邊", ans: "鸕鶿/海星" },
      { category: "動物昆蟲與水族", clue: "鳥女", ans: "精衛" },
      { category: "動物昆蟲與水族", clue: "善心", ans: "辛十四娘" },
      { category: "動物昆蟲與水族", clue: "喵喵", ans: "貓" },
      { category: "動物昆蟲與水族", clue: "智遊", ans: "海豚" },
      { category: "動物昆蟲與水族", clue: "游泳", ans: "青蛙/海豚" },
      { category: "動物昆蟲與水族", clue: "硬刺", ans: "豪豬" },
      { category: "動物昆蟲與水族", clue: "築壩", ans: "河狸" },
      { category: "動物昆蟲與水族", clue: "腕足", ans: "魷魚" },
      { category: "動物昆蟲與水族", clue: "開屏", ans: "孔雀" },
      { category: "動物昆蟲與水族", clue: "黑白皮膚", ans: "企鵝" },
      { category: "動物昆蟲與水族", clue: "黑羽", ans: "烏鴉" },
      { category: "動物昆蟲與水族", clue: "黑斑", ans: "花豹" },
      { category: "動物昆蟲與水族", clue: "勤勞", ans: "螞蟻" },
      { category: "動物昆蟲與水族", clue: "嗡嗡", ans: "蒼蠅" },
      { category: "動物昆蟲與水族", clue: "搖擺", ans: "鴨子" },
      { category: "動物昆蟲與水族", clue: "搬運", ans: "螞蟻" },
      { category: "動物昆蟲與水族", clue: "溪流", ans: "水獺" },
      { category: "動物昆蟲與水族", clue: "溫順", ans: "梅花鹿、海牛、綿羊" },
      { category: "動物昆蟲與水族", clue: "滑水", ans: "海獅" },
      { category: "動物昆蟲與水族", clue: "滑溜", ans: "黃鱔" },
      { category: "動物昆蟲與水族", clue: "滑稽", ans: "小魚兒" },
      { category: "動物昆蟲與水族", clue: "群居", ans: "鯊魚/水豚/火焰鳥" },
      { category: "動物昆蟲與水族", clue: "聖誕節", ans: "馴鹿" },
      { category: "動物昆蟲與水族", clue: "蛻皮", ans: "蛇" },
      { category: "動物昆蟲與水族", clue: "跳遠", ans: "蛤蟆" },
      { category: "動物昆蟲與水族", clue: "跳躍", ans: "金絲猴/牛蛙/兔子" },
      { category: "動物昆蟲與水族", clue: "馱物/馱運", ans: "毛驢" },
      { category: "動物昆蟲與水族", clue: "鼓氣", ans: "河豚" },
      { category: "動物昆蟲與水族", clue: "壽司", ans: "鮪魚" },
      { category: "動物昆蟲與水族", clue: "長角", ans: "天牛" },
      { category: "動物昆蟲與水族", clue: "長壽", ans: "烏龜" },
      { category: "動物昆蟲與水族", clue: "長腿的鳥", ans: "火烈鳥" },
      { category: "動物昆蟲與水族", clue: "長頸", ans: "長頸鹿" },
      { category: "動物昆蟲與水族", clue: "非洲", ans: "長頸鹿" },
      { category: "動物昆蟲與水族", clue: "扁嘴", ans: "鴨嘴獸" },
      { category: "動物昆蟲與水族", clue: "星宿", ans: "阿紫" },
      { category: "動物昆蟲與水族", clue: "炫耀", ans: "孔雀" },
      { category: "動物昆蟲與水族", clue: "猛獸", ans: "雪豹" },
      { category: "動物昆蟲與水族", clue: "產卵", ans: "大馬哈魚" },
      { category: "動物昆蟲與水族", clue: "細長", ans: "鸕鶿/蚊子" },
      { category: "動物昆蟲與水族", clue: "細長魚類", ans: "秋刀魚" },
      { category: "動物昆蟲與水族", clue: "脫胎", ans: "高力士" },
      { category: "動物昆蟲與水族", clue: "蜣螂", ans: "蜣螂" },
      { category: "動物昆蟲與水族", clue: "袖舞", ans: "鹿茸" },
      { category: "動物昆蟲與水族", clue: "覓食", ans: "雞" },
      { category: "動物昆蟲與水族", clue: "通體赤紅", ans: "火烈鳥" },
      { category: "動物昆蟲與水族", clue: "野豬", ans: "林沖" },
      { category: "動物昆蟲與水族", clue: "陸海", ans: "烏龜" },
      { category: "動物昆蟲與水族", clue: "雪山", ans: "雪豹" },
      { category: "動物昆蟲與水族", clue: "雪地", ans: "猞猁" },
      { category: "動物昆蟲與水族", clue: "頂球雜耍", ans: "海獅" },
      { category: "動物昆蟲與水族", clue: "嚙齒", ans: "鼠" },
      { category: "動物昆蟲與水族", clue: "牆壁", ans: "壁虎" },
      { category: "動物昆蟲與水族", clue: "蠕動", ans: "蚯蚓" },
      { category: "動物昆蟲與水族", clue: "高原獵手", ans: "雪豹" },
      { category: "動物昆蟲與水族", clue: "高覽", ans: "長頸鹿" },
      { category: "動物昆蟲與水族", clue: "變色/偽裝", ans: "變色龍/比目魚" },
      { category: "動物昆蟲與水族", clue: "蹼足", ans: "鴨嘴獸" },
      { category: "動物昆蟲與水族", clue: "鬍鬚", ans: "雪納瑞" },
      { category: "動物昆蟲與水族", clue: "龐大", ans: "鯨魚" },
      { category: "動物昆蟲與水族", clue: "靈長", ans: "金絲猴" },
      { category: "動物昆蟲與水族", clue: "鹽水", ans: "秋刀魚" },
      { category: "動物昆蟲與水族", clue: "鹽湖", ans: "火烈鳥" },
      { category: "動物昆蟲與水族", clue: "觀音", ans: "木吒" },

      // 三、 植物、蔬果、花卉與藥材
      { category: "植物蔬果與藥材", clue: "土中", ans: "紅薯" },
      { category: "植物蔬果與藥材", clue: "土壤", ans: "紅蘿蔔" },
      { category: "植物蔬果與藥材", clue: "大顆粒", ans: "水稻" },
      { category: "植物蔬果與藥材", clue: "小粒", ans: "綠豆" },
      { category: "植物蔬果與藥材", clue: "孔洞/荷花", ans: "蓮藕" },
      { category: "植物蔬果與藥材", clue: "水生", ans: "空心菜、馬蹄蓮、芋頭、蓮藕" },
      { category: "植物蔬果與藥材", clue: "水果", ans: "櫻桃" },
      { category: "植物蔬果與藥材", clue: "田中、日間、黃粒", ans: "玉米" },
      { category: "植物蔬果與藥材", clue: "白花", ans: "菜花" },
      { category: "植物蔬果與藥材", clue: "白扁", ans: "扁豆" },
      { category: "植物蔬果與藥材", clue: "白筍", ans: "蘆筍" },
      { category: "植物蔬果與藥材", clue: "白皙青葉", ans: "小白菜" },
      { category: "植物蔬果與藥材", clue: "多汁", ans: "梨樹、番茄、桃樹" },
      { category: "植物蔬果與藥材", clue: "豆科", ans: "蠶豆" },
      { category: "植物蔬果與藥材", clue: "豆莢", ans: "蠶豆" },
      { category: "植物蔬果與藥材", clue: "辛苦、球莖、紫皮", ans: "洋蔥" },
      { category: "植物蔬果與藥材", clue: "空心、空洞、菜地", ans: "芹菜" },
      { category: "植物蔬果與藥材", clue: "金黃圓潤、酸甜", ans: "柑橘、柑橘樹" },
      { category: "植物蔬果與藥材", clue: "長條、棒狀", ans: "黃瓜" },
      { category: "植物蔬果與藥材", clue: "紅色", ans: "番茄" },
      { category: "植物蔬果與藥材", clue: "紅心", ans: "紅薯" },
      { category: "植物蔬果與藥材", clue: "脆甜", ans: "胡蘿蔔、紅蘿蔔、萵筍" },
      { category: "植物蔬果與藥材", clue: "菜園、蒜葉、綠色長莖", ans: "蒜苗" },
      { category: "植物蔬果與藥材", clue: "菜蔬", ans: "扁豆" },
      { category: "植物蔬果與藥材", clue: "球形、球型", ans: "花椰菜" },
      { category: "植物蔬果與藥材", clue: "甜味水果、解暑、種子", ans: "西瓜、西瓜籽" },
      { category: "植物蔬果與藥材", clue: "軟糯", ans: "茄子" },
      { category: "植物蔬果與藥材", clue: "萵菜、萬菜", ans: "萵苣" },
      { category: "植物蔬果與藥材", clue: "綠色", ans: "蓮子" },
      { category: "植物蔬果與藥材", clue: "綠色蔬菜", ans: "小白菜" },
      { category: "植物蔬果與藥材", clue: "蒸食", ans: "芋頭" },
      { category: "植物蔬果與藥材", clue: "大水果、熱帶、濃情", ans: "鳳梨蜜、波羅蜜" },
      { category: "植物蔬果與藥材", clue: "五月花神、美麗、觀賞", ans: "芍藥花" },
      { category: "植物蔬果與藥材", clue: "五月開花、錦簇", ans: "牡丹、牡丹花" },
      { category: "植物蔬果與藥材", clue: "水養殖、素顏、淡波", ans: "水仙花" },
      { category: "植物蔬果與藥材", clue: "四季", ans: "長春花、常春藤" },
      { category: "植物蔬果與藥材", clue: "多色", ans: "鳳仙花、薔薇" },
      { category: "植物蔬果與藥材", clue: "多彩、短暫", ans: "繡球花、百日草" },
      { category: "植物蔬果與藥材", clue: "有毒、紅白、有毒", ans: "夾竹桃" },
      { category: "植物蔬果與藥材", clue: "花蕾、芳香", ans: "丁香" },
      { category: "植物蔬果與藥材", clue: "花邊", ans: "康乃馨" },
      { category: "植物蔬果與藥材", clue: "垂掛、藤蔓、藍紫", ans: "紫藤" },
      { category: "植物蔬果與藥材", clue: "垂絲、粉紅、觀賞", ans: "海棠、海棠樹" },
      { category: "植物蔬果與藥材", clue: "春信、藍紫", ans: "風信子" },
      { category: "植物蔬果與藥材", clue: "春意、紅艷", ans: "朱頂紅" },
      { category: "植物蔬果與藥材", clue: "染甲", ans: "鳳仙花" },
      { category: "植物蔬果與藥材", clue: "紅顏、映山、鳴叫", ans: "杜鵑花、杜鵑" },
      { category: "植物蔬果與藥材", clue: "茶香、潔白", ans: "茉莉" },
      { category: "植物蔬果與藥材", clue: "粉紅、粉嫩、甜蜜", ans: "桃花、桃樹" },
      { category: "植物蔬果與藥材", clue: "粉紅白、核仁、黃果", ans: "杏花、杏樹" },
      { category: "植物蔬果與藥材", clue: "絲狀花、菊花、霜枝", ans: "菊花" },
      { category: "植物蔬果與藥材", clue: "喇叭", ans: "牽牛花" },
      { category: "植物蔬果與藥材", clue: "愛意/濃情", ans: "玫瑰" },
      { category: "植物蔬果與藥材", clue: "寒香、先花後葉", ans: "梅花" },
      { category: "植物蔬果與藥材", clue: "落英", ans: "櫻花" },
      { category: "植物蔬果與藥材", clue: "攀爬", ans: "常春藤" },
      { category: "植物蔬果與藥材", clue: "攀緣", ans: "凌霄/薔薇" },
      { category: "植物蔬果與藥材", clue: "藍黑", ans: "藍莓" },
      { category: "植物蔬果與藥材", clue: "芳香", ans: "艾葉" },
      { category: "植物蔬果與藥材", clue: "大且繁華、掌形、鳳凰", ans: "梧桐、梧桐樹" },
      { category: "植物蔬果與藥材", clue: "四季、蒼翠、長青", ans: "柏樹/松樹" },
      { category: "植物蔬果與藥材", clue: "古老", ans: "銀杏" },
      { category: "植物蔬果與藥材", clue: "白皮", ans: "楊樹" },
      { category: "植物蔬果與藥材", clue: "柔條", ans: "柳樹" },
      { category: "植物蔬果與藥材", clue: "秋色、紅葉、掌狀", ans: "楓樹" },
      { category: "植物蔬果與藥材", clue: "挺拔", ans: "松樹" },
      { category: "植物蔬果與藥材", clue: "綠冠", ans: "樟樹" },
      { category: "植物蔬果與藥材", clue: "綠葉", ans: "芭蕉、黃楊、爬山虎" },
      { category: "植物蔬果與藥材", clue: "綠蔭", ans: "榕樹" },
      { category: "植物蔬果與藥材", clue: "綠陰", ans: "龍眼樹" },
      { category: "植物蔬果與藥材", clue: "橘紅、橙紅", ans: "凌霄" },
      { category: "植物蔬果與藥材", clue: "橢圓葉片", ans: "榆樹" },
      { category: "植物蔬果與藥材", clue: "獨木", ans: "榕樹" },
      { category: "植物蔬果與藥材", clue: "樹樹脂", ans: "沉香" },
      { category: "植物蔬果與藥材", clue: "中藥、甜汁", ans: "甘草" },
      { category: "植物蔬果與藥材", clue: "止咳、柔軟、黃亮", ans: "枇杷" },
      { category: "植物蔬果與藥材", clue: "珍貴、傘狀、藥草", ans: "靈芝" },
      { category: "植物蔬果與藥材", clue: "根莖、藥用", ans: "何首烏" },
      { category: "植物蔬果與藥材", clue: "藥用、苦、根莖", ans: "黃連" },
      { category: "植物蔬果與藥材", clue: "藥用、根莖", ans: "西洋參" },
      { category: "植物蔬果與藥材", clue: "補血、草本", ans: "當歸" },
      { category: "植物蔬果與藥材", clue: "道旁、藥材、輪生", ans: "車前草" },
      { category: "植物蔬果與藥材", clue: "香料、紫色、解暑", ans: "藿香" },
      { category: "植物蔬果與藥材", clue: "滋補、藥效", ans: "人參" },
      { category: "植物蔬果與藥材", clue: "滋補、藥食", ans: "枸杞" },
      { category: "植物蔬果與藥材", clue: "傘形、繖形", ans: "香菇" },
      { category: "植物蔬果與藥材", clue: "提示、提神、清涼", ans: "薄荷腦" },
      { category: "植物蔬果與藥材", clue: "藥祖", ans: "神農氏" },
      { category: "植物蔬果與藥材", clue: "藥材", ans: "山藥" },
      { category: "植物蔬果與藥材", clue: "藥用", ans: "板藍根" },
      { category: "植物蔬果與藥材", clue: "菌類", ans: "冬蟲夏草" },
      { category: "植物蔬果與藥材", clue: "油料、果實", ans: "橄欖、橄欖油" },

      // 四、 生活器物、日常用品與建築
      { category: "生活器物與建築", clue: "三足", ans: "鼎" },
      { category: "生活器物與建築", clue: "刀刃", ans: "剪刀、菜刀" },
      { category: "生活器物與建築", clue: "工具", ans: "鞴子、輪子、鉤子" },
      { category: "生活器物與建築", clue: "中國建築、古蹟", ans: "長城" },
      { category: "生活器物與建築", clue: "中繼站、驛站", ans: "驛站" },
      { category: "生活器物與建築", clue: "切菜、板實、板實", ans: "案板" },
      { category: "生活器物與建築", clue: "木制", ans: "床、筆架、畫筒、佛珠、梳子、箱子、桌案" },
      { category: "生活器物與建築", clue: "用具、帶鏡子的家具、床邊家具", ans: "梳妝台" },
      { category: "生活器物與建築", clue: "收納", ans: "箱子" },
      { category: "生活器物與建築", clue: "米", ans: "米缸" },
      { category: "生活器物與建築", clue: "防雨", ans: "蓑衣" },
      { category: "生活器物與建築", clue: "防護/戰袍", ans: "鎧甲" },
      { category: "生活器物與建築", clue: "兒童玩具", ans: "鞦韆、鞴子、不倒翁" },
      { category: "生活器物與建築", clue: "固定、頭端", ans: "釘子" },
      { category: "生活器物與建築", clue: "明亮、照明", ans: "蠟燭" },
      { category: "生活器物與建築", clue: "油料", ans: "橄欖油" },
      { category: "生活器物與建築", clue: "裝飾、陶瓷、插花", ans: "花瓶" },
      { category: "生活器物與建築", clue: "運載", ans: "騾子" },
      { category: "生活器物與建築", clue: "釣子、金屬", ans: "魚鉤" },
      { category: "生活器物與建築", clue: "器具", ans: "青花瓷" },
      { category: "生活器物與建築", clue: "橡皮筋", ans: "彈弓" },
      { category: "生活器物與建築", clue: "隨身、繡袋", ans: "香囊" },
      { category: "生活器物與建築", clue: "鎖閉/鐵製", ans: "鎖具" },
      { category: "生活器物與建築", clue: "鐵製", ans: "熨斗" },
      { category: "生活器物與建築", clue: "鐵項", ans: "頸圈" },
      { category: "生活器物與建築", clue: "鐵腕", ans: "鐵手" },
      { category: "生活器物與建築", clue: "繩網、捕鱗、補鱗", ans: "漁網" },
      { category: "生活器物與建築", clue: "罐滿、甜味容器", ans: "糖罐" },
      { category: "生活器物與建築", clue: "節日食物", ans: "月餅" },
      { category: "生活器物與建築", clue: "頭髮", ans: "梳子" },

      // 五、 武學兵器、運動與競技
      { category: "武學兵器與競技", clue: "上肢運動、命中、箭矢", ans: "射箭" },
      { category: "武學兵器與競技", clue: "短劍、劍舞、短兵", ans: "短劍" },
      { category: "武學兵器與競技", clue: "長槍、長桿武器、戰矛", ans: "長槍、長矛" },
      { category: "武學兵器與競技", clue: "箭簇、投射", ans: "箭簇" },
      { category: "武學兵器與競技", clue: "武術、傳統武術", ans: "太極、功夫" },
      { category: "武學兵器與競技", clue: "比武", ans: "穆念慈" },
      { category: "武學兵器與競技", clue: "多人遊戲", ans: "捉迷藏、足球" },
      { category: "武學兵器與競技", clue: "投擲", ans: "投壺、飛鏢" },
      { category: "武學兵器與競技", clue: "流星錘、練舞、鏈舞", ans: "流星錘" },
      { category: "武學兵器與競技", clue: "競技運動、摔跤", ans: "相撲" },
      { category: "武學兵器與競技", clue: "劍鞘", ans: "寶劍" },
      { category: "武學兵器與競技", clue: "箭術", ans: "扳指、花榮" },
      { category: "武學兵器與競技", clue: "射戟", ans: "呂布" },
      { category: "武學兵器與競技", clue: "射戰", ans: "文醜" },
      { category: "武學兵器與競技", clue: "射擊遊戲", ans: "彈弓" },

      // 六、 地理、自然現象與天文氣象
      { category: "地理自然與天象", clue: "大水", ans: "瀑布" },
      { category: "地理自然與天象", clue: "方向、方位、方位", ans: "指南針" },
      { category: "地理自然與天象", clue: "火助", ans: "風箱" },
      { category: "地理自然與天象", clue: "加熱", ans: "飯菜" },
      { category: "地理自然與天象", clue: "自然現象", ans: "雨" },
      { category: "地理自然與天象", clue: "風沙", ans: "胡楊" },
      { category: "地理自然與天象", clue: "沙漠", ans: "胡楊/蠍子" },
      { category: "地理自然與天象", clue: "雨霖", ans: "柳永" },
      { category: "地理自然與天象", clue: "夜空、點燃、燦爛", ans: "放煙火" },
      { category: "地理自然與天象", clue: "深海", ans: "鱷魚、鱈魚、電鰻、鯊魚、魷魚" },
      { category: "地理自然與天象", clue: "淺海", ans: "海象、海牛" },
      { category: "地理自然與天象", clue: "清涼", ans: "水缸" },
      { category: "地理自然與天象", clue: "潮濕、陰濕、覆蓋", ans: "苔蘚" },
      { category: "地理自然與天象", clue: "陰陽、太極", ans: "陰陽" },
      { category: "地理自然與天象", clue: "瀑布", ans: "瀑布" },

      // 七、 色彩、抽象概念與綜合類
      { category: "色彩抽象與綜合", clue: "黑白", ans: "陰陽、太極、企鵝" },
      { category: "色彩抽象與綜合", clue: "硬殼", ans: "海龜" },
      { category: "色彩抽象與綜合", clue: "冷熱", ans: "熨斗" },
      { category: "色彩抽象與綜合", clue: "多角", ans: "菱角" },
      { category: "色彩抽象與綜合", clue: "柔勁、蛇行、鞭影", ans: "長鞭" },
      { category: "色彩抽象與綜合", clue: "命中", ans: "射箭" },
      { category: "色彩抽象與綜合", clue: "圓周", ans: "祖沖之" },
      { category: "色彩抽象與綜合", clue: "醫治", ans: "郎中" },
      { category: "色彩抽象與綜合", clue: "感謝", ans: "康乃馨" },
      { category: "色彩抽象與綜合", clue: "滑溜", ans: "黃鱔" },
      { category: "色彩抽象與綜合", clue: "雜交", ans: "騾子" },
      { category: "色彩抽象與綜合", clue: "鬆土", ans: "耙子/蚯蚓" },
      { category: "色彩抽象與綜合", clue: "精油", ans: "檸檬" },
      { category: "色彩抽象與綜合", clue: "醬香、調味品", ans: "醬油" },
      { category: "色彩抽象與綜合", clue: "色彩", ans: "丹青" },
      { category: "色彩抽象與綜合", clue: "小粒", ans: "綠豆" },
      { category: "色彩抽象與綜合", clue: "大顆粒", ans: "水稻" },
      { category: "色彩抽象與綜合", clue: "清脆", ans: "蘋果" }
    ];
  </script>

  <script>
    function appData() {
      return {
        // App Navigation & Tabs
        activeTab: 'search', // 'search' | 'manage'
        searchQuery: '',
        selectedCategory: '全部',
        categories: [
          '全部',
          '歷史人物與典故',
          '動物昆蟲與水族',
          '植物蔬果與藥材',
          '生活器物與建築',
          '武學兵器與競技',
          '地理自然與天象',
          '色彩抽象與綜合'
        ],

        // Main Dataset
        items: [],

        // Modal States
        showEditModal: false,
        showConfirmModal: false,
        showImportModal: false,
        
        // Toast notification
        toastMessage: '',
        showToast: false,

        // Form Item for Add/Edit
        formItem: { id: null, category: '歷史人物與典故', clue: '', ans: '' },
        isEditing: false,

        // Confirmation Modal Settings
        confirmTitle: '',
        confirmMsg: '',
        onConfirm: null,

        // Import/Export string data
        importExportText: '',

        init() {
          const savedData = localStorage.getItem('yanyun_shefu_db');
          if (savedData) {
            try {
              this.items = JSON.parse(savedData);
            } catch(e) {
              this.loadDefaults();
            }
          } else {
            this.loadDefaults();
          }
        },

        loadDefaults() {
          // Assign auto IDs
          this.items = DEFAULT_DATABASE.map((item, idx) => ({
            id: Date.now() + idx,
            category: item.category,
            clue: item.clue,
            ans: item.ans
          }));
          this.saveToStorage();
        },

        saveToStorage() {
          localStorage.setItem('yanyun_shefu_db', JSON.stringify(this.items));
        },

        // Search logic
        get filteredItems() {
          let list = this.items;

          // Category filter
          if (this.selectedCategory !== '全部') {
            list = list.filter(i => i.category === this.selectedCategory);
          }

          // Search query filter (clue or answer match)
          if (this.searchQuery.trim() !== '') {
            const q = this.searchQuery.trim().toLowerCase();
            list = list.filter(i => 
              i.clue.toLowerCase().includes(q) || 
              i.ans.toLowerCase().includes(q)
            );
          }

          return list;
        },

        // Helper: Category count badge
        getCategoryCount(cat) {
          if (cat === '全部') return this.items.length;
          return this.items.filter(i => i.category === cat).length;
        },

        // Add/Edit operations
        openAddModal() {
          this.isEditing = false;
          this.formItem = { 
            id: null, 
            category: this.selectedCategory !== '全部' ? this.selectedCategory : '歷史人物與典故', 
            clue: '', 
            ans: '' 
          };
          this.showEditModal = true;
        },

        openEditModal(item) {
          this.isEditing = true;
          this.formItem = { ...item };
          this.showEditModal = true;
        },

        saveItem() {
          if (!this.formItem.clue.trim() || !this.formItem.ans.trim()) {
            this.notify('請完整填寫題目提示與答案！');
            return;
          }

          if (this.isEditing) {
            const idx = this.items.findIndex(i => i.id === this.formItem.id);
            if (idx !== -1) {
              this.items[idx] = { ...this.formItem };
            }
            this.notify('題目修改成功！');
          } else {
            const newItem = {
              ...this.formItem,
              id: Date.now()
            };
            this.items.unshift(newItem);
            this.notify('新增題目成功！');
          }

          this.saveToStorage();
          this.showEditModal = false;
        },

        // Delete item
        confirmDelete(item) {
          this.confirmTitle = '確認刪除';
          this.confirmMsg = `確定要刪除「${item.clue} → ${item.ans}」這筆資料嗎？`;
          this.onConfirm = () => {
            this.items = this.items.filter(i => i.id !== item.id);
            this.saveToStorage();
            this.notify('已刪除題目！');
          };
          this.showConfirmModal = true;
        },

        // Reset Database
        confirmReset() {
          this.confirmTitle = '重置為預設題庫';
          this.confirmMsg = '警告：這將會清除您新增或修改過的所有數據，回復至初始的射覆題庫！確定要重置嗎？';
          this.onConfirm = () => {
            this.loadDefaults();
            this.notify('題庫已恢復為預設狀態！');
          };
          this.showConfirmModal = true;
        },

        // Export/Import Modal
        openImportExport() {
          this.importExportText = JSON.stringify(this.items, null, 2);
          this.showImportModal = true;
        },

        copyExportText() {
          this.copyToClipboard(this.importExportText);
          this.notify('備份資料已複製至剪貼簿！');
        },

        importData() {
          try {
            const parsed = JSON.parse(this.importExportText);
            if (Array.isArray(parsed)) {
              // Ensure fields exist
              const validated = parsed.map((item, idx) => ({
                id: item.id || Date.now() + idx,
                category: item.category || '色彩抽象與綜合',
                clue: item.clue || '',
                ans: item.ans || ''
              })).filter(i => i.clue && i.ans);

              if (validated.length === 0) {
                this.notify('匯入資料格式不正確或無有效內容');
                return;
              }

              this.items = validated;
              this.saveToStorage();
              this.showImportModal = false;
              this.notify(`成功匯入 ${validated.length} 筆題庫！`);
            } else {
              this.notify('資料格式必須為 JSON 陣列！');
            }
          } catch(e) {
            this.notify('JSON 解析失敗，請檢查格式是否正確。');
          }
        },

        // Clipboard Helper
        copyToClipboard(text) {
          if (navigator.clipboard && window.isSecureContext) {
            navigator.clipboard.writeText(text);
          } else {
            const textArea = document.createElement("textarea");
            textArea.value = text;
            textArea.style.position = "fixed";
            textArea.style.left = "-999999px";
            document.body.appendChild(textArea);
            textArea.focus();
            textArea.select();
            try {
              document.execCommand('copy');
            } catch (error) {
              console.error('複製失敗', error);
            }
            document.body.removeChild(textArea);
          }
        },

        copyAnswer(ans) {
          this.copyToClipboard(ans);
          this.notify(`已複製答案：「${ans}」`);
        },

        // Toast notification logic
        notify(msg) {
          this.toastMessage = msg;
          this.showToast = true;
          setTimeout(() => {
            this.showToast = false;
          }, 2500);
        }
      }
    }
  </script>

  <!-- HEADER -->
  <header class="sticky top-0 z-30 bg-ink-bg/95 backdrop-blur shadow-sm border-b border-ink-border px-4 py-3">
    <div class="max-w-3xl mx-auto flex items-center justify-between">
      <div class="flex items-center space-x-3">
        <!-- 水墨風格 Icon 容器 -->
        <div class="w-10 h-10 rounded bg-ink-card border border-ink-border flex items-center justify-center text-ink-green shadow-sm relative overflow-hidden">
          <i class="fa-solid fa-fan text-lg z-10 opacity-90"></i>
          <!-- 簡單的水墨底紋點綴 -->
          <div class="absolute -bottom-2 -right-2 w-6 h-6 bg-ink-green opacity-10 rounded-full blur-md"></div>
        </div>
        <div>
          <h1 class="font-bold text-title leading-tight text-ink-green tracking-widest">燕雲十六聲</h1>
          <p class="text-content text-ink-brown opacity-80 mt-0.5">射覆解謎助手</p>
        </div>
      </div>

      <!-- Mode Toggle Switch -->
      <div class="flex bg-ink-card p-1 rounded-md border border-ink-border shadow-inner">
        <button 
          @click="activeTab = 'search'"
          :class="activeTab === 'search' ? 'bg-ink-bg text-ink-green font-bold shadow-sm border border-ink-border' : 'text-ink-textMuted hover:text-ink-green'"
          class="px-3 py-1.5 rounded text-content transition-all flex items-center space-x-1.5">
          <i class="fa-solid fa-magnifying-glass text-[10pt]"></i>
          <span>查詢</span>
        </button>
        <button 
          @click="activeTab = 'manage'"
          :class="activeTab === 'manage' ? 'bg-ink-bg text-ink-green font-bold shadow-sm border border-ink-border' : 'text-ink-textMuted hover:text-ink-green'"
          class="px-3 py-1.5 rounded text-content transition-all flex items-center space-x-1.5">
          <i class="fa-solid fa-book-open text-[10pt]"></i>
          <span>書庫</span>
        </button>
      </div>
    </div>
  </header>

  <!-- MAIN CONTAINER -->
  <main class="max-w-3xl mx-auto px-4 pt-6">

    <!-- SEARCH PAGE -->
    <div x-show="activeTab === 'search'" x-transition:enter="transition ease-out duration-300" x-transition:enter-start="opacity-0 translate-y-2" x-transition:enter-end="opacity-100 translate-y-0">
      
      <!-- Big Search Input -->
      <div class="relative mb-6">
        <div class="absolute inset-y-0 left-0 pl-4 flex items-center pointer-events-none text-ink-green opacity-70">
          <i class="fa-solid fa-brush text-lg"></i>
        </div>
        <input 
          type="text" 
          x-model="searchQuery" 
          placeholder="提筆搜尋題面或謎底（如：七劍、左冷禪）..." 
          class="w-full pl-12 pr-12 py-4 bg-ink-card rounded border border-ink-border focus:border-ink-green focus:ring-1 focus:ring-ink-green text-ink-textMain placeholder-ink-textMuted text-title shadow-inner outline-none transition-all"
        />
        <!-- Clear input button -->
        <button 
          x-show="searchQuery.length > 0" 
          @click="searchQuery = ''" 
          class="absolute inset-y-0 right-0 pr-4 flex items-center text-ink-textMuted hover:text-ink-brown">
          <i class="fa-solid fa-xmark text-lg"></i>
        </button>
      </div>

      <!-- Category Chips Filter -->
      <div class="mb-6">
        <div class="flex items-center justify-between mb-3">
          <span class="text-content font-bold text-ink-brown tracking-widest flex items-center">
            <i class="fa-solid fa-bookmark mr-2 text-[10pt] opacity-70"></i>卷宗分類
          </span>
          <span class="text-content text-ink-green" x-text="`尋得 ${filteredItems.length} 篇`"></span>
        </div>
        <div class="flex overflow-x-auto space-x-2 pb-2 no-scrollbar -mx-4 px-4">
          <template x-for="cat in categories" :key="cat">
            <button 
              @click="selectedCategory = cat"
              :class="selectedCategory === cat ? 'bg-ink-green text-white border-ink-greenLight shadow-ink-strong' : 'bg-ink-card text-ink-textMain border-ink-border hover:bg-ink-bg'"
              class="px-4 py-2 rounded text-content font-medium border whitespace-nowrap transition-all flex items-center space-x-2 flex-shrink-0">
              <span x-text="cat" class="tracking-wide"></span>
              <span 
                :class="selectedCategory === cat ? 'text-ink-bg opacity-90' : 'text-ink-textMuted'"
                class="text-[10pt]"
                x-text="`(${getCategoryCount(cat)})`"></span>
            </button>
          </template>
        </div>
        <div class="ink-divider"></div>
      </div>

      <!-- Results List -->
      <div class="space-y-4">
        <!-- Empty State -->
        <div x-show="filteredItems.length === 0" class="text-center py-16 px-4 bg-ink-card rounded border border-dashed border-ink-border">
          <i class="fa-solid fa-wind text-4xl text-ink-textMuted mb-4 opacity-50"></i>
          <h3 class="text-title font-bold text-ink-brown">渺無音訊</h3>
          <p class="text-content text-ink-textMuted mt-2">未尋得相關卷宗，請嘗試其他線索。</p>
          <button 
            @click="searchQuery = ''; selectedCategory = '全部'"
            class="mt-6 px-6 py-2 bg-ink-bg hover:bg-ink-card text-ink-green border border-ink-green rounded text-content font-medium transition-all shadow-sm">
            重整思緒
          </button>
        </div>

        <!-- Result Card -->
        <template x-for="item in filteredItems" :key="item.id">
          <div class="bg-ink-card border border-ink-border hover:border-ink-greenLight rounded p-5 shadow-ink-soft transition-all flex flex-col sm:flex-row sm:items-center justify-between gap-4 relative overflow-hidden group">
            
            <!-- 水墨裝飾角 -->
            <div class="absolute top-0 right-0 w-16 h-16 bg-gradient-to-bl from-ink-green/5 to-transparent pointer-events-none transition-all group-hover:from-ink-green/10"></div>

            <div class="flex-1 min-w-0 z-10">
              <div class="flex items-center space-x-2 mb-2">
                <span class="px-2 py-0.5 rounded text-[10pt] bg-ink-bg text-ink-brown border border-ink-border tracking-wider" x-text="item.category"></span>
              </div>
              
              <!-- Clue & Answer Layout -->
              <div class="flex flex-col sm:flex-row sm:items-baseline gap-1 sm:gap-4 mt-1">
                <div class="flex items-center text-ink-textMain">
                  <span class="text-[10pt] text-ink-textMuted mr-2 select-none">題：</span>
                  <span class="text-title font-bold tracking-widest" x-text="item.clue"></span>
                </div>
                
                <i class="fa-solid fa-arrow-right-long text-ink-green/40 hidden sm:block"></i>
                
                <div class="flex items-center text-ink-brown mt-1 sm:mt-0">
                  <span class="text-[10pt] text-ink-textMuted mr-2 select-none">解：</span>
                  <span class="text-title font-black tracking-widest" x-text="item.ans"></span>
                </div>
              </div>
            </div>

            <!-- Quick Action: Copy Answer -->
            <div class="flex items-center space-x-3 self-end sm:self-center z-10 mt-2 sm:mt-0">
              <button 
                @click="openEditModal(item)"
                title="批註修改"
                class="p-2.5 text-ink-textMuted hover:text-ink-green bg-ink-bg hover:bg-white rounded border border-transparent hover:border-ink-border transition-all shadow-sm">
                <i class="fa-solid fa-pen-nib text-content"></i>
              </button>
              <button 
                @click="copyAnswer(item.ans)"
                class="px-5 py-2.5 bg-ink-green hover:bg-ink-greenLight active:scale-95 text-white font-medium text-content rounded shadow-ink-strong transition-all flex items-center space-x-2">
                <i class="fa-regular fa-copy"></i>
                <span class="tracking-widest">拓印解答</span>
              </button>
            </div>
          </div>
        </template>
      </div>

    </div>

    <!-- DATABASE MANAGEMENT PAGE -->
    <div x-show="activeTab === 'manage'" x-transition:enter="transition ease-out duration-300" x-transition:enter-start="opacity-0 translate-y-2" x-transition:enter-end="opacity-100 translate-y-0" class="space-y-6">
      
      <!-- Management Action Bar -->
      <div class="bg-ink-card rounded border border-ink-border p-5 shadow-ink-soft flex flex-col md:flex-row items-center justify-between gap-4 relative overflow-hidden">
        <div class="absolute -left-4 -top-4 w-20 h-20 bg-ink-brown opacity-5 rounded-full blur-xl pointer-events-none"></div>
        
        <div class="z-10 text-center md:text-left">
          <h2 class="text-title font-bold text-ink-brown tracking-widest">藏經閣</h2>
          <p class="text-content text-ink-textMuted mt-1">編纂修訂您的專屬射覆題庫</p>
        </div>

        <div class="flex flex-wrap justify-center md:justify-end items-center gap-2 z-10 w-full md:w-auto">
          <button 
            @click="openAddModal()"
            class="flex-1 md:flex-initial px-5 py-2.5 bg-ink-brown hover:bg-ink-brownDark text-white text-content font-bold rounded shadow-ink-soft transition-all flex items-center justify-center space-x-2">
            <i class="fa-solid fa-plus"></i>
            <span class="tracking-widest">收錄新題</span>
          </button>
          <button 
            @click="openImportExport()"
            class="px-4 py-2.5 bg-ink-bg hover:bg-white text-ink-textMain border border-ink-border text-content font-medium rounded transition-all flex items-center justify-center space-x-2 shadow-sm">
            <i class="fa-solid fa-scroll"></i>
            <span class="tracking-widest">謄寫/匯入</span>
          </button>
          <button 
            @click="confirmReset()"
            title="歸復初始"
            class="p-2.5 bg-red-50 hover:bg-red-100 text-red-800 border border-red-200 text-content rounded transition-all shadow-sm">
            <i class="fa-solid fa-rotate-left"></i>
          </button>
        </div>
      </div>

      <!-- Quick Filter for Management -->
      <div class="flex flex-col sm:flex-row items-center gap-3">
        <div class="relative w-full sm:flex-1">
          <input 
            type="text" 
            x-model="searchQuery" 
            placeholder="於書海中尋覓..." 
            class="w-full pl-10 pr-4 py-2.5 bg-ink-card rounded border border-ink-border text-content text-ink-textMain placeholder-ink-textMuted outline-none focus:border-ink-green focus:ring-1 focus:ring-ink-green shadow-inner"
          />
          <i class="fa-solid fa-filter absolute left-3.5 top-3.5 text-ink-textMuted text-[10pt]"></i>
        </div>
        <select 
          x-model="selectedCategory"
          class="w-full sm:w-auto bg-ink-card border border-ink-border text-ink-textMain text-content rounded px-4 py-2.5 outline-none focus:border-ink-green shadow-sm">
          <template x-for="cat in categories" :key="cat">
            <option :value="cat" x-text="cat"></option>
          </template>
        </select>
      </div>

      <!-- Full Table List -->
      <div class="bg-ink-card rounded border border-ink-border overflow-hidden shadow-ink-soft">
        <div class="px-5 py-3 bg-ink-bg border-b border-ink-border flex justify-between items-center">
          <span class="text-content font-bold text-ink-brown" x-text="`現存 ${filteredItems.length} 卷`"></span>
          <span class="text-[10pt] text-ink-textMuted">紀錄存於當下客棧(本機)</span>
        </div>

        <div class="divide-y divide-ink-border/60 max-h-[60vh] overflow-y-auto">
          <template x-for="item in filteredItems" :key="item.id">
            <div class="p-4 hover:bg-ink-bg transition-all flex items-center justify-between gap-3">
              <div class="min-w-0 flex-1">
                <div <!doctype html>
<html lang="zh-Hant"><head><meta charset="utf-8"><meta name="viewport" content="width=device-width,initial-scale=1,viewport-fit=cover"><meta name="theme-color" content="#f5f2e9"><title>燕雲射覆小箋</title><style>
:root{--bg:#f5f2e9;--paper:#fffdf7;--ink:#263e37;--muted:#6c756b;--line:#dedfd3;--green:#235c4d;--gold:#a66c2c}*{box-sizing:border-box}body{margin:0;background:var(--bg);color:var(--ink);font:16px/1.6 -apple-system,BlinkMacSystemFont,'Segoe UI',sans-serif}button,input,select{font:inherit}button,select{min-height:46px;cursor:pointer}button{border:1px solid var(--line);border-radius:12px;background:var(--paper);color:var(--ink);padding:8px 14px}button:focus-visible,input:focus-visible,select:focus-visible{outline:3px solid #ba8e4e;outline-offset:2px}button.active{background:var(--green);color:white;border-color:var(--green)}header,main,footer{max-width:940px;margin:auto;padding:0 20px}header{padding-top:32px;padding-bottom:22px}.eyebrow{font-size:11px;letter-spacing:3px;color:var(--gold);font-weight:700}h1{font-family:serif;font-size:34px;letter-spacing:3px;margin:8px 0 4px}h1 span{font-size:14px;letter-spacing:0;border:1px solid #b3bdad;border-radius:50%;padding:5px;vertical-align:middle;margin-left:10px}p{margin:8px 0}.sub{color:var(--muted);font-size:14px}.stats{display:flex;gap:22px;margin-top:20px}.stats strong{font-size:22px;font-weight:600}.stats small{color:var(--muted);margin-left:5px}.controls{position:sticky;top:0;background:var(--bg);padding:12px 0 16px;z-index:2;border-bottom:1px solid var(--line)}.search{display:flex;gap:8px}input[type=search]{min-width:0;width:100%;min-height:52px;border:1px solid #b1c2b5;border-radius:14px;padding:12px;background:var(--paper);color:var(--ink)}.search button{background:var(--green);color:white;flex-shrink:0}.recent{display:flex;gap:7px;overflow:auto;margin-top:10px}.recent:empty{display:none}.recent button{white-space:nowrap;font-size:13px;min-height:44px;padding:6px 12px}.filterrow{display:flex;gap:8px;margin-top:12px}.filterrow select{width:100%;min-width:0;border:1px solid var(--line);border-radius:12px;background:var(--paper);padding:8px;color:var(--ink)}.filterrow button{white-space:nowrap}.tabs{display:flex;gap:6px;margin-top:12px}.tabs button{flex:1;font-size:14px;padding:8px}.summary{display:flex;justify-content:space-between;align-items:center;gap:12px;margin:16px 0;font-size:13px;color:var(--muted)}.summary button{font-size:13px}.cards{display:grid;grid-template-columns:repeat(2,minmax(0,1fr));gap:12px}.card{background:var(--paper);border:1px solid var(--line);border-radius:16px;padding:18px;min-width:0}.cardtop{display:flex;align-items:flex-start;justify-content:space-between;gap:12px}.clue{font-family:serif;font-size:21px;font-weight:600;margin:0;overflow-wrap:anywhere}.label{font-size:11px;color:var(--muted);letter-spacing:1px}.star{font-size:21px;min-width:46px;padding:3px;flex-shrink:0}.star[aria-pressed=true]{color:var(--gold);background:#fbefd8}.answerlist{display:flex;flex-wrap:wrap;gap:8px;margin:12px 0}.answer{border:0;border-radius:9px;background:#e8efe7;padding:6px 10px;font-size:18px;min-height:44px}.answer small{display:block;font-size:10px;color:var(--muted)}.cardfoot{color:var(--muted);font-size:12px;display:flex;gap:8px;align-items:center;justify-content:space-between}.copy{font-size:12px;min-height:44px;padding:6px 12px}.links{display:flex;flex-wrap:wrap;gap:7px;margin-top:12px}.links button{font-size:14px}.empty{grid-column:1/-1;padding:40px 12px;text-align:center;color:var(--muted)}#more{display:block;margin:22px auto;width:100%;max-width:360px}footer{padding-top:20px;padding-bottom:calc(26px + env(safe-area-inset-bottom));color:var(--muted);font-size:12px}details{border-top:1px solid var(--line);padding-top:14px}summary{cursor:pointer;min-height:44px;font-size:14px}.tools{display:flex;flex-wrap:wrap;gap:8px;margin:12px 0}.tools button{font-size:13px}#storage{font-size:12px;margin-bottom:14px}.warning{color:#904725}#toast{position:fixed;bottom:calc(22px + env(safe-area-inset-bottom));left:50%;transform:translateX(-50%);background:var(--ink);color:white;padding:12px 18px;border-radius:12px;z-index:10;max-width:90%;width:max-content;font-size:14px}#toast:empty{display:none}[hidden]{display:none!important}mark{background:#f0dba9;color:inherit;border-radius:3px}@media(max-width:600px){header,main,footer{padding-left:16px;padding-right:16px}.cards{grid-template-columns:1fr}h1{font-size:30px}.stats{gap:16px}.card{padding:16px}.controls{padding-top:8px}}@media(prefers-reduced-motion:no-preference){button{transition:background .15s}}
dialog{width:calc(100% - 24px);max-width:560px;max-height:90dvh;overflow:auto;border:1px solid var(--line);border-radius:18px;background:var(--paper);color:var(--ink);padding:20px}dialog::backdrop{background:#1b302b99}dialog h2{margin:0;font-family:serif}dialog label{display:block;font-size:13px;color:var(--muted);margin:10px 0 5px}dialog input{display:block;width:100%;min-width:0;min-height:48px;border:1px solid #b1c2b5;border-radius:10px;background:white;color:var(--ink);padding:10px;font-size:16px}.answerrow{border:1px solid var(--line);border-radius:12px;padding:12px;margin:12px 0}.answerrow button{margin-top:10px;font-size:13px}.editorhead,.managebar{display:flex;align-items:center;justify-content:space-between;gap:10px}.managebar{margin-top:12px;font-size:12px;color:var(--muted)}.managebar button{flex-shrink:0;background:#e8efe7}.editoractions{display:flex;gap:8px;margin-top:18px}.editoractions button{flex:1}.primary{background:var(--green);color:white}.cardfoot{flex-wrap:wrap}#formerror{color:#904725;font-size:14px}button[data-remove-answer]{color:#904725}</style></head><body><header><div class="eyebrow">燕雲十六聲 · NPC 解謎隨身簿</div><h1>射覆小箋<span>解</span></h1><p class="sub">NPC 出題，你翻小箋。江湖再大，答案也有跡可循。</p><div class="stats"><div><strong id="total"></strong><small>條線索</small></div><div><strong id="atotal"></strong><small>個答案</small></div><div><strong id="fcount">0</strong><small>個收藏</small></div></div></header>
<main><section class="controls" aria-label="搜尋與篩選"><form class="search" id="searchform"><input id="q" type="search" placeholder="輸入線索或答案，如：七縱、孟獲" aria-label="搜尋線索或答案" autocomplete="off"><button type="submit">查詢</button></form><div id="recent" class="recent" aria-label="最近搜尋"></div><div class="filterrow"><select id="category" aria-label="依答案內容分類"><option value="">全部答案分類</option></select><button id="reset" type="button">清除篩選</button></div><div class="managebar"><span>把新遇見的謎題，收入自己的題庫。</span><button id="newquestion" type="button">＋ 新增題目</button></div><nav class="tabs" aria-label="瀏覽方式"><button type="button" data-mode="clues">線索題庫</button><button type="button" data-mode="answers">依答案瀏覽</button><button type="button" data-mode="favorites">我的收藏</button></nav></section><div class="summary"><span id="result" role="status" aria-live="polite"></span><span>點答案可反查</span></div><div id="cards" class="cards"></div><button id="more" hidden>載入更多</button></main>
<footer><div id="storage" role="status"></div><details><summary>使用說明與資料備份</summary><p>支援線索與答案搜尋；以空格分隔關鍵字可縮小範圍。分類按答案內容整理，同一線索可能包含不同類別的答案。</p><p>題庫完整內建於這個 HTML，無須登入或連線。新增及修改的題庫、收藏、最近搜尋與篩選設定會保存在目前瀏覽器。請使用同一瀏覽器與同一檔案位置；私密模式、清除網站資料或移動檔案，可能使紀錄無法保留。</p><p>題庫來自你提供的「文字.txt」，未另行核實遊戲答案。多答案保留所有候選，請配合 NPC 後續提示判斷。</p><div class="tools"><button id="export">匯出完整題庫備份</button><button id="import">匯入題庫備份</button></div><input type="file" id="importfile" accept="application/json,.json" hidden><p>備份包含完整題庫、收藏及瀏覽設定。匯入新版備份會取代目前題庫及個人紀錄；舊版備份只還原個人紀錄。更換檔案或手機前，請先匯出備份。手機若只顯示 HTML 原始碼或靜態預覽，請改以能執行 JavaScript 的瀏覽器方式開啟。</p></details><p>一箋在手，少東家且慢慢猜。</p></footer><dialog id="editor" aria-labelledby="editortitle"><div class="editorhead"><h2 id="editortitle">新增題目</h2><button id="closeeditor" type="button" aria-label="關閉編輯">✕</button></div><form id="editorform"><label for="editclue">題目／NPC 線索</label><input id="editclue" required maxlength="300" placeholder="例如：七縱"><p class="sub">每個答案可有自己的分類；分類可選取，也可直接輸入新名稱。</p><datalist id="categorylist"></datalist><div id="answerrows"></div><button id="addanswer" type="button">＋ 增加一個答案</button><p id="formerror" role="alert"></p><div class="editoractions"><button id="canceleditor" type="button">取消</button><button type="submit" class="primary">儲存題目</button></div></form></dialog><div id="toast" role="status" aria-live="polite"></div>
<script>
'use strict';
const BASE=[{"id": 1, "clue": "七劍", "answers": [{"text": "左冷禪", "cat": "人物與角色"}]}, {"id": 2, "clue": "七縱", "answers": [{"text": "孟獲", "cat": "人物與角色"}]}, {"id": 3, "clue": "七夕", "answers": [{"text": "鵲橋", "cat": "典故與文化"}]}, {"id": 4, "clue": "九陰", "answers": [{"text": "梅超風", "cat": "人物與角色"}]}, {"id": 5, "clue": "九黎", "answers": [{"text": "蚩尤", "cat": "人物與角色"}]}, {"id": 6, "clue": "入川", "answers": [{"text": "徐庶", "cat": "人物與角色"}]}, {"id": 7, "clue": "八卦", "answers": [{"text": "伏羲", "cat": "人物與角色"}]}, {"id": 8, "clue": "八門", "answers": [{"text": "張遼", "cat": "人物與角色"}]}, {"id": 9, "clue": "十三", "answers": [{"text": "孫子", "cat": "人物與角色"}]}, {"id": 10, "clue": "丈八", "answers": [{"text": "張飛", "cat": "人物與角色"}]}, {"id": 11, "clue": "三五", "answers": [{"text": "賈詡", "cat": "人物與角色"}]}, {"id": 12, "clue": "不老", "answers": [{"text": "天山童姥", "cat": "人物與角色"}]}, {"id": 13, "clue": "口舌", "answers": [{"text": "諸葛瑾", "cat": "人物與角色"}]}, {"id": 14, "clue": "大理", "answers": [{"text": "段正淳", "cat": "人物與角色"}]}, {"id": 15, "clue": "大禹", "answers": [{"text": "治水", "cat": "典故與文化"}]}, {"id": 16, "clue": "小石", "answers": [{"text": "柳宗元", "cat": "人物與角色"}]}, {"id": 17, "clue": "小廝", "answers": [{"text": "賈璉", "cat": "人物與角色"}]}, {"id": 18, "clue": "中興", "answers": [{"text": "劉秀", "cat": "人物與角色"}, {"text": "宋高宗", "cat": "人物與角色"}]}, {"id": 19, "clue": "五官", "answers": [{"text": "曹丕", "cat": "人物與角色"}]}, {"id": 20, "clue": "仁君", "answers": [{"text": "堯", "cat": "人物與角色"}]}, {"id": 21, "clue": "仁義", "answers": [{"text": "孟子", "cat": "人物與角色"}]}, {"id": 22, "clue": "內史", "answers": [{"text": "蒙毅", "cat": "人物與角色"}]}, {"id": 23, "clue": "內助", "answers": [{"text": "獨孤伽羅", "cat": "人物與角色"}]}, {"id": 24, "clue": "六一", "answers": [{"text": "歐陽修", "cat": "人物與角色"}]}, {"id": 25, "clue": "太史", "answers": [{"text": "司馬遷", "cat": "人物與角色"}]}, {"id": 26, "clue": "太宗", "answers": [{"text": "趙光義", "cat": "人物與角色"}]}, {"id": 27, "clue": "文王", "answers": [{"text": "姬昌", "cat": "人物與角色"}]}, {"id": 28, "clue": "文和", "answers": [{"text": "賈詡", "cat": "人物與角色"}]}, {"id": 29, "clue": "文獻", "answers": [{"text": "獨孤伽羅", "cat": "人物與角色"}]}, {"id": 30, "clue": "方士", "answers": [{"text": "徐福", "cat": "人物與角色"}]}, {"id": 31, "clue": "北海", "answers": [{"text": "蘇武", "cat": "人物與角色"}, {"text": "太史慈", "cat": "人物與角色"}]}, {"id": 32, "clue": "半山", "answers": [{"text": "王安石", "cat": "人物與角色"}]}, {"id": 33, "clue": "古之", "answers": [{"text": "洪七公", "cat": "人物與角色"}]}, {"id": 34, "clue": "司徒", "answers": [{"text": "王允", "cat": "人物與角色"}]}, {"id": 35, "clue": "四世", "answers": [{"text": "袁紹", "cat": "人物與角色"}]}, {"id": 36, "clue": "四鎮", "answers": [{"text": "朱溫", "cat": "人物與角色"}]}, {"id": 37, "clue": "失衡", "answers": [{"text": "馬謖", "cat": "人物與角色"}]}, {"id": 38, "clue": "玄妙", "answers": [{"text": "老子", "cat": "人物與角色"}]}, {"id": 39, "clue": "玄鳥", "answers": [{"text": "商湯", "cat": "人物與角色"}]}, {"id": 40, "clue": "瓦崗", "answers": [{"text": "程咬金", "cat": "人物與角色"}]}, {"id": 41, "clue": "生死", "answers": [{"text": "天山童姥", "cat": "人物與角色"}]}, {"id": 42, "clue": "仲父", "answers": [{"text": "呂不韋", "cat": "人物與角色"}]}, {"id": 43, "clue": "丞相", "answers": [{"text": "李斯", "cat": "人物與角色"}]}, {"id": 44, "clue": "地府", "answers": [{"text": "判官", "cat": "人物與角色"}]}, {"id": 45, "clue": "奸臣", "answers": [{"text": "高俅", "cat": "人物與角色"}]}, {"id": 46, "clue": "好漢", "answers": [{"text": "柴進", "cat": "人物與角色"}]}, {"id": 47, "clue": "成語", "answers": [{"text": "沉魚落雁", "cat": "典故與文化"}, {"text": "狐假虎威", "cat": "典故與文化"}, {"text": "畫蛇添足", "cat": "典故與文化"}]}, {"id": 48, "clue": "曳落", "answers": [{"text": "安祿山", "cat": "人物與角色"}]}, {"id": 49, "clue": "江湖", "answers": [{"text": "司空摘星", "cat": "人物與角色"}]}, {"id": 50, "clue": "西域", "answers": [{"text": "張騫", "cat": "人物與角色"}, {"text": "班超", "cat": "人物與角色"}]}, {"id": 51, "clue": "西涼", "answers": [{"text": "馬超", "cat": "人物與角色"}]}, {"id": 52, "clue": "西湖", "answers": [{"text": "白素貞", "cat": "人物與角色"}]}, {"id": 53, "clue": "西醉", "answers": [{"text": "黃鶴樓", "cat": "典故與文化"}]}, {"id": 54, "clue": "兵法", "answers": [{"text": "孫臏", "cat": "人物與角色"}]}, {"id": 55, "clue": "兵敗", "answers": [{"text": "項梁", "cat": "人物與角色"}]}, {"id": 56, "clue": "呆霸王", "answers": [{"text": "薛蟠", "cat": "人物與角色"}]}, {"id": 57, "clue": "孝聖", "answers": [{"text": "舜", "cat": "人物與角色"}]}, {"id": 58, "clue": "孝賢", "answers": [{"text": "伯邑考", "cat": "人物與角色"}]}, {"id": 59, "clue": "序文", "answers": [{"text": "王羲之", "cat": "人物與角色"}]}, {"id": 60, "clue": "弄權", "answers": [{"text": "王熙鳳", "cat": "人物與角色"}]}, {"id": 61, "clue": "快意", "answers": [{"text": "蕭十一郎", "cat": "人物與角色"}]}, {"id": 62, "clue": "抄檢", "answers": [{"text": "王夫人", "cat": "人物與角色"}]}, {"id": 63, "clue": "汨羅", "answers": [{"text": "屈原", "cat": "人物與角色"}]}, {"id": 64, "clue": "沉魚", "answers": [{"text": "西施", "cat": "人物與角色"}]}, {"id": 65, "clue": "狂客", "answers": [{"text": "賀知章", "cat": "人物與角色"}]}, {"id": 66, "clue": "角色", "answers": [{"text": "判官", "cat": "人物與角色"}, {"text": "月老", "cat": "人物與角色"}]}, {"id": 67, "clue": "赤帝", "answers": [{"text": "祝融", "cat": "人物與角色"}]}, {"id": 68, "clue": "赤壁", "answers": [{"text": "蘇軾", "cat": "人物與角色"}]}, {"id": 69, "clue": "亞子", "answers": [{"text": "李存勗", "cat": "人物與角色"}]}, {"id": 70, "clue": "京口", "answers": [{"text": "劉裕", "cat": "人物與角色"}]}, {"id": 71, "clue": "使楚", "answers": [{"text": "晏子", "cat": "人物與角色"}]}, {"id": 72, "clue": "侍寢", "answers": [{"text": "襲人", "cat": "人物與角色"}]}, {"id": 73, "clue": "兩界", "answers": [{"text": "劉伯欽", "cat": "人物與角色"}]}, {"id": 74, "clue": "典故", "answers": [{"text": "三顧茅廬", "cat": "典故與文化"}, {"text": "田忌賽馬", "cat": "典故與文化"}, {"text": "大禹治水", "cat": "典故與文化"}, {"text": "四面楚歌", "cat": "典故與文化"}]}, {"id": 75, "clue": "刺客", "answers": [{"text": "荊軻", "cat": "人物與角色"}]}, {"id": 76, "clue": "呼延", "answers": [{"text": "秦明", "cat": "人物與角色"}]}, {"id": 77, "clue": "奇門", "answers": [{"text": "黃藥師", "cat": "人物與角色"}]}, {"id": 78, "clue": "姑蘇", "answers": [{"text": "夫差", "cat": "人物與角色"}]}, {"id": 79, "clue": "孟德", "answers": [{"text": "曹操", "cat": "人物與角色"}]}, {"id": 80, "clue": "定軍", "answers": [{"text": "夏侯淵", "cat": "人物與角色"}]}, {"id": 81, "clue": "岳陽", "answers": [{"text": "范仲淹", "cat": "人物與角色"}]}, {"id": 82, "clue": "弦歌", "answers": [{"text": "太史慈", "cat": "人物與角色"}]}, {"id": 83, "clue": "征途", "answers": [{"text": "楊素", "cat": "人物與角色"}]}, {"id": 84, "clue": "忠奸", "answers": [{"text": "伍子胥", "cat": "人物與角色"}]}, {"id": 85, "clue": "忠義", "answers": [{"text": "宋江", "cat": "人物與角色"}]}, {"id": 86, "clue": "忠誠", "answers": [{"text": "阮明正", "cat": "人物與角色"}]}, {"id": 87, "clue": "忠魂", "answers": [{"text": "屈原", "cat": "人物與角色"}]}, {"id": 88, "clue": "昆陽", "answers": [{"text": "劉秀", "cat": "人物與角色"}]}, {"id": 89, "clue": "武安", "answers": [{"text": "白起", "cat": "人物與角色"}]}, {"id": 90, "clue": "武成", "answers": [{"text": "王翦", "cat": "人物與角色"}]}, {"id": 91, "clue": "武當", "answers": [{"text": "張三豐", "cat": "人物與角色"}]}, {"id": 92, "clue": "河北", "answers": [{"text": "顏良", "cat": "人物與角色"}]}, {"id": 93, "clue": "法家", "answers": [{"text": "李斯", "cat": "人物與角色"}]}, {"id": 94, "clue": "狐媚", "answers": [{"text": "蘇妲己", "cat": "人物與角色"}]}, {"id": 95, "clue": "宣和", "answers": [{"text": "宋徽宗", "cat": "人物與角色"}]}, {"id": 96, "clue": "宦官", "answers": [{"text": "趙高", "cat": "人物與角色"}]}, {"id": 97, "clue": "後德", "answers": [{"text": "衛子夫", "cat": "人物與角色"}]}, {"id": 98, "clue": "怒目", "answers": [{"text": "典韋", "cat": "人物與角色"}]}, {"id": 99, "clue": "故事", "answers": [{"text": "西天取經", "cat": "典故與文化"}]}, {"id": 100, "clue": "洛神", "answers": [{"text": "曹植", "cat": "人物與角色"}]}, {"id": 101, "clue": "洛陽", "answers": [{"text": "孫堅", "cat": "人物與角色"}]}, {"id": 102, "clue": "相秦", "answers": [{"text": "范雎", "cat": "人物與角色"}]}, {"id": 103, "clue": "科聖", "answers": [{"text": "張衡", "cat": "人物與角色"}]}, {"id": 104, "clue": "英魂", "answers": [{"text": "孫策", "cat": "人物與角色"}]}, {"id": 105, "clue": "陌巷", "answers": [{"text": "顏回", "cat": "人物與角色"}]}, {"id": 106, "clue": "降洞", "answers": [{"text": "賈寶玉", "cat": "人物與角色"}]}, {"id": 107, "clue": "修史", "answers": [{"text": "班固", "cat": "人物與角色"}]}, {"id": 108, "clue": "傲世", "answers": [{"text": "阮籍", "cat": "人物與角色"}]}, {"id": 109, "clue": "詠懷", "answers": [{"text": "阮籍", "cat": "人物與角色"}]}, {"id": 110, "clue": "貴妃", "answers": [{"text": "楊玉環", "cat": "人物與角色"}]}, {"id": 111, "clue": "進學", "answers": [{"text": "韓愈", "cat": "人物與角色"}]}, {"id": 112, "clue": "開元", "answers": [{"text": "李隆基", "cat": "人物與角色"}]}, {"id": 113, "clue": "開封", "answers": [{"text": "包拯", "cat": "人物與角色"}]}, {"id": 114, "clue": "雄才", "answers": [{"text": "劉徹", "cat": "人物與角色"}]}, {"id": 115, "clue": "集序", "answers": [{"text": "上官婉兒", "cat": "人物與角色"}]}, {"id": 116, "clue": "菩薩", "answers": [{"text": "夏啟", "cat": "人物與角色"}]}, {"id": 117, "clue": "詞客", "answers": [{"text": "晏幾道", "cat": "人物與角色"}]}, {"id": 118, "clue": "傳道", "answers": [{"text": "孔子", "cat": "人物與角色"}]}, {"id": 119, "clue": "愛蓮", "answers": [{"text": "周敦頤", "cat": "人物與角色"}]}, {"id": 120, "clue": "敬德", "answers": [{"text": "秦瓊", "cat": "人物與角色"}]}, {"id": 121, "clue": "新朝", "answers": [{"text": "王莽", "cat": "人物與角色"}]}, {"id": 122, "clue": "暗算", "answers": [{"text": "龐涓", "cat": "人物與角色"}]}, {"id": 123, "clue": "楚柱", "answers": [{"text": "項梁", "cat": "人物與角色"}]}, {"id": 124, "clue": "熙寧", "answers": [{"text": "王安石", "cat": "人物與角色"}]}, {"id": 125, "clue": "義釋", "answers": [{"text": "關勝", "cat": "人物與角色"}]}, {"id": 126, "clue": "虞兮", "answers": [{"text": "虞姬", "cat": "人物與角色"}]}, {"id": 127, "clue": "虞美", "answers": [{"text": "李煜", "cat": "人物與角色"}]}, {"id": 128, "clue": "詩詞", "answers": [{"text": "唐婉", "cat": "人物與角色"}]}, {"id": 129, "clue": "聚義", "answers": [{"text": "晁蓋", "cat": "人物與角色"}]}, {"id": 130, "clue": "腐刑", "answers": [{"text": "司馬遷", "cat": "人物與角色"}]}, {"id": 131, "clue": "輕敵", "answers": [{"text": "李信", "cat": "人物與角色"}]}, {"id": 132, "clue": "運河", "answers": [{"text": "楊廣", "cat": "人物與角色"}]}, {"id": 133, "clue": "漢化", "answers": [{"text": "拓跋宏", "cat": "人物與角色"}]}, {"id": 134, "clue": "漢光", "answers": [{"text": "劉秀", "cat": "人物與角色"}]}, {"id": 135, "clue": "漢將", "answers": [{"text": "鍾離眜", "cat": "人物與角色"}]}, {"id": 136, "clue": "漢舞", "answers": [{"text": "趙飛燕", "cat": "人物與角色"}]}, {"id": 137, "clue": "獄中", "answers": [{"text": "華佗", "cat": "人物與角色"}]}, {"id": 138, "clue": "獄詠", "answers": [{"text": "駱賓王", "cat": "人物與角色"}]}, {"id": 139, "clue": "精忠", "answers": [{"text": "岳飛", "cat": "人物與角色"}]}, {"id": 140, "clue": "慕容", "answers": [{"text": "鳳凰", "cat": "人物與角色"}]}, {"id": 141, "clue": "暴虐", "answers": [{"text": "商紂王", "cat": "人物與角色"}]}, {"id": 142, "clue": "賢回", "answers": [{"text": "顏回", "cat": "人物與角色"}]}, {"id": 143, "clue": "賦文", "answers": [{"text": "向秀", "cat": "人物與角色"}]}, {"id": 144, "clue": "醉鄉", "answers": [{"text": "劉伶", "cat": "人物與角色"}]}, {"id": 145, "clue": "遺計", "answers": [{"text": "郭嘉", "cat": "人物與角色"}]}, {"id": 146, "clue": "霓裳", "answers": [{"text": "李隆基", "cat": "人物與角色"}]}, {"id": 147, "clue": "龍城", "answers": [{"text": "衛青", "cat": "人物與角色"}]}, {"id": 148, "clue": "龍裔", "answers": [{"text": "黃帝", "cat": "人物與角色"}]}, {"id": 149, "clue": "龍圖", "answers": [{"text": "包拯", "cat": "人物與角色"}]}, {"id": 150, "clue": "濡須", "answers": [{"text": "諸葛瑾", "cat": "人物與角色"}]}, {"id": 151, "clue": "簡貴", "answers": [{"text": "王戎", "cat": "人物與角色"}]}, {"id": 152, "clue": "獵戶", "answers": [{"text": "劉伯欽", "cat": "人物與角色"}]}, {"id": 153, "clue": "典當", "answers": [{"text": "邢岫煙", "cat": "人物與角色"}]}, {"id": 154, "clue": "鎖壓", "answers": [{"text": "法海", "cat": "人物與角色"}]}, {"id": 155, "clue": "巧解", "answers": [{"text": "遊坦之", "cat": "人物與角色"}]}, {"id": 156, "clue": "額雀", "answers": [{"text": "王之渙", "cat": "人物與角色"}]}, {"id": 157, "clue": "力大", "answers": [{"text": "熊", "cat": "動物與昆蟲"}]}, {"id": 158, "clue": "人猿", "answers": [{"text": "猩猩", "cat": "動物與昆蟲"}]}, {"id": 159, "clue": "大口", "answers": [{"text": "牛蛙", "cat": "動物與昆蟲"}]}, {"id": 160, "clue": "大麥克", "answers": [{"text": "河馬", "cat": "動物與昆蟲"}]}, {"id": 161, "clue": "大眼", "answers": [{"text": "鹿", "cat": "動物與昆蟲"}]}, {"id": 162, "clue": "山林", "answers": [{"text": "狐狸", "cat": "動物與昆蟲"}, {"text": "竹鼠", "cat": "動物與昆蟲"}, {"text": "熊", "cat": "動物與昆蟲"}]}, {"id": 163, "clue": "小丑魚", "answers": [{"text": "共生", "cat": "自然與其他"}, {"text": "海中", "cat": "自然與其他"}]}, {"id": 164, "clue": "小馬", "answers": [{"text": "薩摩耶", "cat": "動物與昆蟲"}]}, {"id": 165, "clue": "不詳", "answers": [{"text": "烏鴉", "cat": "動物與昆蟲"}]}, {"id": 166, "clue": "中型犬種", "answers": [{"text": "薩摩耶", "cat": "動物與昆蟲"}]}, {"id": 167, "clue": "五角", "answers": [{"text": "海星", "cat": "動物與昆蟲"}]}, {"id": 168, "clue": "水底", "answers": [{"text": "清道夫魚", "cat": "動物與昆蟲"}]}, {"id": 169, "clue": "水域", "answers": [{"text": "水豚", "cat": "動物與昆蟲"}]}, {"id": 170, "clue": "水稻", "answers": [{"text": "水牛", "cat": "動物與昆蟲"}]}, {"id": 171, "clue": "水邊", "answers": [{"text": "白鷺", "cat": "動物與昆蟲"}]}, {"id": 172, "clue": "冬眠", "answers": [{"text": "土撥鼠", "cat": "動物與昆蟲"}, {"text": "熊", "cat": "動物與昆蟲"}]}, {"id": 173, "clue": "北極", "answers": [{"text": "海象", "cat": "動物與昆蟲"}, {"text": "海獅", "cat": "動物與昆蟲"}]}, {"id": 174, "clue": "叮咬", "answers": [{"text": "蚊子", "cat": "動物與昆蟲"}]}, {"id": 175, "clue": "平天", "answers": [{"text": "牛魔王", "cat": "人物與角色"}]}, {"id": 176, "clue": "打雷", "answers": [{"text": "鰻魚", "cat": "動物與昆蟲"}]}, {"id": 177, "clue": "打鳴", "answers": [{"text": "公雞", "cat": "動物與昆蟲"}]}, {"id": 178, "clue": "用翅膀游泳", "answers": [{"text": "企鵝", "cat": "動物與昆蟲"}]}, {"id": 179, "clue": "田野", "answers": [{"text": "兔子", "cat": "動物與昆蟲"}]}, {"id": 180, "clue": "白毛", "answers": [{"text": "薩摩耶", "cat": "動物與昆蟲"}]}, {"id": 181, "clue": "白羽", "answers": [{"text": "白鷺", "cat": "動物與昆蟲"}]}, {"id": 182, "clue": "白肉", "answers": [{"text": "鱈魚", "cat": "動物與昆蟲"}]}, {"id": 183, "clue": "白色鳥類", "answers": [{"text": "白鷺", "cat": "動物與昆蟲"}]}, {"id": 184, "clue": "再生", "answers": [{"text": "海星", "cat": "動物與昆蟲"}]}, {"id": 185, "clue": "冰面", "answers": [{"text": "海豹", "cat": "動物與昆蟲"}]}, {"id": 186, "clue": "地上", "answers": [{"text": "螞蟻", "cat": "動物與昆蟲"}]}, {"id": 187, "clue": "地下", "answers": [{"text": "鼴鼠", "cat": "動物與昆蟲"}, {"text": "蚯蚓", "cat": "動物與昆蟲"}]}, {"id": 188, "clue": "多足", "answers": [{"text": "蜈蚣", "cat": "動物與昆蟲"}]}, {"id": 189, "clue": "多臂", "answers": [{"text": "海星", "cat": "動物與昆蟲"}]}, {"id": 190, "clue": "尖刺", "answers": [{"text": "刺蝟", "cat": "動物與昆蟲"}]}, {"id": 191, "clue": "吞食", "answers": [{"text": "鯰魚", "cat": "動物與昆蟲"}]}, {"id": 192, "clue": "吸附", "answers": [{"text": "壁虎", "cat": "動物與昆蟲"}]}, {"id": 193, "clue": "池塘", "answers": [{"text": "蝌蚪", "cat": "動物與昆蟲"}, {"text": "牛蛙", "cat": "動物與昆蟲"}, {"text": "鴨子", "cat": "動物與昆蟲"}]}, {"id": 194, "clue": "池塘（動物）", "answers": [{"text": "蝌蚪", "cat": "動物與昆蟲"}]}, {"id": 195, "clue": "灰色犬種", "answers": [{"text": "雪納瑞", "cat": "動物與昆蟲"}]}, {"id": 196, "clue": "灰色鳥類", "answers": [{"text": "杜鵑", "cat": "動物與昆蟲"}]}, {"id": 197, "clue": "羊毛", "answers": [{"text": "綿羊", "cat": "動物與昆蟲"}]}, {"id": 198, "clue": "自衛", "answers": [{"text": "臭鼬", "cat": "動物與昆蟲"}]}, {"id": 199, "clue": "西海", "answers": [{"text": "小白龍", "cat": "人物與角色"}]}, {"id": 200, "clue": "冷水", "answers": [{"text": "鱈魚", "cat": "動物與昆蟲"}]}, {"id": 201, "clue": "冷血", "answers": [{"text": "蛇", "cat": "動物與昆蟲"}]}, {"id": 202, "clue": "冷淡注視", "answers": [{"text": "蛇", "cat": "動物與昆蟲"}]}, {"id": 203, "clue": "利爪", "answers": [{"text": "猞猁", "cat": "動物與昆蟲"}]}, {"id": 204, "clue": "尾鉤", "answers": [{"text": "蠍子", "cat": "動物與昆蟲"}]}, {"id": 205, "clue": "巡弋", "answers": [{"text": "鯊魚", "cat": "動物與昆蟲"}]}, {"id": 206, "clue": "快跑", "answers": [{"text": "鴕鳥", "cat": "動物與昆蟲"}]}, {"id": 207, "clue": "育兒", "answers": [{"text": "海馬", "cat": "動物與昆蟲"}]}, {"id": 208, "clue": "貝殼", "answers": [{"text": "珍珠", "cat": "器物與建築"}]}, {"id": 209, "clue": "夜行", "answers": [{"text": "貓頭鷹", "cat": "動物與昆蟲"}]}, {"id": 210, "clue": "夜晚", "answers": [{"text": "老鼠", "cat": "動物與昆蟲"}, {"text": "蚊子", "cat": "動物與昆蟲"}]}, {"id": 211, "clue": "奔跑", "answers": [{"text": "鴕鳥", "cat": "動物與昆蟲"}, {"text": "斑馬", "cat": "動物與昆蟲"}]}, {"id": 212, "clue": "指鹿", "answers": [{"text": "趙高", "cat": "人物與角色"}]}, {"id": 213, "clue": "挖洞藏頭", "answers": [{"text": "鴕鳥", "cat": "動物與昆蟲"}]}, {"id": 214, "clue": "洄游", "answers": [{"text": "大馬哈魚", "cat": "動物與昆蟲"}]}, {"id": 215, "clue": "狩獵", "answers": [{"text": "猞猁", "cat": "動物與昆蟲"}]}, {"id": 216, "clue": "看家", "answers": [{"text": "狗", "cat": "動物與昆蟲"}]}, {"id": 217, "clue": "紅冠家禽", "answers": [{"text": "公雞", "cat": "動物與昆蟲"}]}, {"id": 218, "clue": "紅眼睛", "answers": [{"text": "兔兒", "cat": "動物與昆蟲"}]}, {"id": 219, "clue": "負重", "answers": [{"text": "烏龜", "cat": "動物與昆蟲"}]}, {"id": 220, "clue": "面具", "answers": [{"text": "浣熊", "cat": "動物與昆蟲"}]}, {"id": 221, "clue": "飛渡", "answers": [{"text": "羚羊", "cat": "動物與昆蟲"}]}, {"id": 222, "clue": "食根", "answers": [{"text": "豪豬", "cat": "動物與昆蟲"}]}, {"id": 223, "clue": "家禽", "answers": [{"text": "公雞", "cat": "動物與昆蟲"}]}, {"id": 224, "clue": "哨壁", "answers": [{"text": "金絲猴", "cat": "動物與昆蟲"}]}, {"id": 225, "clue": "庭院", "answers": [{"text": "雞", "cat": "動物與昆蟲"}]}, {"id": 226, "clue": "振翅", "answers": [{"text": "蟋蟀", "cat": "動物與昆蟲"}]}, {"id": 227, "clue": "捕魚", "answers": [{"text": "鸕鶿", "cat": "動物與昆蟲"}, {"text": "水獺", "cat": "動物與昆蟲"}]}, {"id": 228, "clue": "捕鼠", "answers": [{"text": "貓貓", "cat": "動物與昆蟲"}]}, {"id": 229, "clue": "海中", "answers": [{"text": "小丑魚", "cat": "動物與昆蟲"}, {"text": "河豚", "cat": "動物與昆蟲"}]}, {"id": 230, "clue": "海岸", "answers": [{"text": "海獅", "cat": "動物與昆蟲"}]}, {"id": 231, "clue": "海底", "answers": [{"text": "比目魚", "cat": "動物與昆蟲"}]}, {"id": 232, "clue": "海洋", "answers": [{"text": "海牛", "cat": "動物與昆蟲"}, {"text": "海龜", "cat": "動物與昆蟲"}, {"text": "海豚", "cat": "動物與昆蟲"}, {"text": "鯨", "cat": "動物與昆蟲"}]}, {"id": 233, "clue": "海草", "answers": [{"text": "海馬", "cat": "動物與昆蟲"}]}, {"id": 234, "clue": "海陸", "answers": [{"text": "烏龜", "cat": "動物與昆蟲"}]}, {"id": 235, "clue": "海邊", "answers": [{"text": "鸕鶿", "cat": "動物與昆蟲"}, {"text": "海星", "cat": "動物與昆蟲"}]}, {"id": 236, "clue": "鳥女", "answers": [{"text": "精衛", "cat": "人物與角色"}]}, {"id": 237, "clue": "善心", "answers": [{"text": "辛十四娘", "cat": "人物與角色"}]}, {"id": 238, "clue": "喵喵", "answers": [{"text": "貓", "cat": "動物與昆蟲"}]}, {"id": 239, "clue": "智遊", "answers": [{"text": "海豚", "cat": "動物與昆蟲"}]}, {"id": 240, "clue": "游泳", "answers": [{"text": "青蛙", "cat": "動物與昆蟲"}, {"text": "海豚", "cat": "動物與昆蟲"}]}, {"id": 241, "clue": "硬刺", "answers": [{"text": "豪豬", "cat": "動物與昆蟲"}]}, {"id": 242, "clue": "築壩", "answers": [{"text": "河狸", "cat": "動物與昆蟲"}]}, {"id": 243, "clue": "腕足", "answers": [{"text": "魷魚", "cat": "動物與昆蟲"}]}, {"id": 244, "clue": "開屏", "answers": [{"text": "孔雀", "cat": "動物與昆蟲"}]}, {"id": 245, "clue": "黑白皮膚", "answers": [{"text": "企鵝", "cat": "動物與昆蟲"}]}, {"id": 246, "clue": "黑羽", "answers": [{"text": "烏鴉", "cat": "動物與昆蟲"}]}, {"id": 247, "clue": "黑斑", "answers": [{"text": "花豹", "cat": "動物與昆蟲"}]}, {"id": 248, "clue": "勤勞", "answers": [{"text": "螞蟻", "cat": "動物與昆蟲"}]}, {"id": 249, "clue": "嗡嗡", "answers": [{"text": "蒼蠅", "cat": "動物與昆蟲"}]}, {"id": 250, "clue": "搖擺", "answers": [{"text": "鴨子", "cat": "動物與昆蟲"}]}, {"id": 251, "clue": "搬運", "answers": [{"text": "螞蟻", "cat": "動物與昆蟲"}]}, {"id": 252, "clue": "溪流", "answers": [{"text": "水獺", "cat": "動物與昆蟲"}]}, {"id": 253, "clue": "溫順", "answers": [{"text": "梅花鹿", "cat": "動物與昆蟲"}, {"text": "海牛", "cat": "動物與昆蟲"}, {"text": "綿羊", "cat": "動物與昆蟲"}]}, {"id": 254, "clue": "滑水", "answers": [{"text": "海獅", "cat": "動物與昆蟲"}]}, {"id": 255, "clue": "滑溜", "answers": [{"text": "黃鱔", "cat": "動物與昆蟲"}]}, {"id": 256, "clue": "滑稽", "answers": [{"text": "小魚兒", "cat": "人物與角色"}]}, {"id": 257, "clue": "群居", "answers": [{"text": "鯊魚", "cat": "動物與昆蟲"}, {"text": "水豚", "cat": "動物與昆蟲"}, {"text": "火焰鳥", "cat": "動物與昆蟲"}]}, {"id": 258, "clue": "聖誕節", "answers": [{"text": "馴鹿", "cat": "動物與昆蟲"}]}, {"id": 259, "clue": "蛻皮", "answers": [{"text": "蛇", "cat": "動物與昆蟲"}]}, {"id": 260, "clue": "跳遠", "answers": [{"text": "蛤蟆", "cat": "動物與昆蟲"}]}, {"id": 261, "clue": "跳躍", "answers": [{"text": "金絲猴", "cat": "動物與昆蟲"}, {"text": "牛蛙", "cat": "動物與昆蟲"}, {"text": "兔子", "cat": "動物與昆蟲"}]}, {"id": 262, "clue": "馱物/馱運", "answers": [{"text": "毛驢", "cat": "動物與昆蟲"}]}, {"id": 263, "clue": "鼓氣", "answers": [{"text": "河豚", "cat": "動物與昆蟲"}]}, {"id": 264, "clue": "壽司", "answers": [{"text": "鮪魚", "cat": "動物與昆蟲"}]}, {"id": 265, "clue": "長角", "answers": [{"text": "天牛", "cat": "動物與昆蟲"}]}, {"id": 266, "clue": "長壽", "answers": [{"text": "烏龜", "cat": "動物與昆蟲"}]}, {"id": 267, "clue": "長腿的鳥", "answers": [{"text": "火烈鳥", "cat": "動物與昆蟲"}]}, {"id": 268, "clue": "長頸", "answers": [{"text": "長頸鹿", "cat": "動物與昆蟲"}]}, {"id": 269, "clue": "非洲", "answers": [{"text": "長頸鹿", "cat": "動物與昆蟲"}]}, {"id": 270, "clue": "扁嘴", "answers": [{"text": "鴨嘴獸", "cat": "動物與昆蟲"}]}, {"id": 271, "clue": "星宿", "answers": [{"text": "阿紫", "cat": "人物與角色"}]}, {"id": 272, "clue": "炫耀", "answers": [{"text": "孔雀", "cat": "動物與昆蟲"}]}, {"id": 273, "clue": "猛獸", "answers": [{"text": "雪豹", "cat": "動物與昆蟲"}]}, {"id": 274, "clue": "產卵", "answers": [{"text": "大馬哈魚", "cat": "動物與昆蟲"}]}, {"id": 275, "clue": "細長", "answers": [{"text": "鸕鶿", "cat": "動物與昆蟲"}, {"text": "蚊子", "cat": "動物與昆蟲"}]}, {"id": 276, "clue": "細長魚類", "answers": [{"text": "秋刀魚", "cat": "動物與昆蟲"}]}, {"id": 277, "clue": "脫胎", "answers": [{"text": "高力士", "cat": "人物與角色"}]}, {"id": 278, "clue": "蜣螂", "answers": [{"text": "蜣螂", "cat": "動物與昆蟲"}]}, {"id": 279, "clue": "袖舞", "answers": [{"text": "鹿茸", "cat": "器物與建築"}]}, {"id": 280, "clue": "覓食", "answers": [{"text": "雞", "cat": "動物與昆蟲"}]}, {"id": 281, "clue": "通體赤紅", "answers": [{"text": "火烈鳥", "cat": "動物與昆蟲"}]}, {"id": 282, "clue": "野豬", "answers": [{"text": "林沖", "cat": "人物與角色"}]}, {"id": 283, "clue": "陸海", "answers": [{"text": "烏龜", "cat": "動物與昆蟲"}]}, {"id": 284, "clue": "雪山", "answers": [{"text": "雪豹", "cat": "動物與昆蟲"}]}, {"id": 285, "clue": "雪地", "answers": [{"text": "猞猁", "cat": "動物與昆蟲"}]}, {"id": 286, "clue": "頂球雜耍", "answers": [{"text": "海獅", "cat": "動物與昆蟲"}]}, {"id": 287, "clue": "嚙齒", "answers": [{"text": "鼠", "cat": "動物與昆蟲"}]}, {"id": 288, "clue": "牆壁", "answers": [{"text": "壁虎", "cat": "動物與昆蟲"}]}, {"id": 289, "clue": "蠕動", "answers": [{"text": "蚯蚓", "cat": "動物與昆蟲"}]}, {"id": 290, "clue": "高原獵手", "answers": [{"text": "雪豹", "cat": "動物與昆蟲"}]}, {"id": 291, "clue": "高覽", "answers": [{"text": "長頸鹿", "cat": "動物與昆蟲"}]}, {"id": 292, "clue": "變色/偽裝", "answers": [{"text": "變色龍", "cat": "動物與昆蟲"}, {"text": "比目魚", "cat": "動物與昆蟲"}]}, {"id": 293, "clue": "蹼足", "answers": [{"text": "鴨嘴獸", "cat": "動物與昆蟲"}]}, {"id": 294, "clue": "鬍鬚", "answers": [{"text": "雪納瑞", "cat": "動物與昆蟲"}]}, {"id": 295, "clue": "龐大", "answers": [{"text": "鯨魚", "cat": "動物與昆蟲"}]}, {"id": 296, "clue": "靈長", "answers": [{"text": "金絲猴", "cat": "動物與昆蟲"}]}, {"id": 297, "clue": "鹽水", "answers": [{"text": "秋刀魚", "cat": "動物與昆蟲"}]}, {"id": 298, "clue": "鹽湖", "answers": [{"text": "火烈鳥", "cat": "動物與昆蟲"}]}, {"id": 299, "clue": "觀音", "answers": [{"text": "木吒", "cat": "人物與角色"}]}, {"id": 300, "clue": "土中", "answers": [{"text": "紅薯", "cat": "蔬果與作物"}]}, {"id": 301, "clue": "土壤", "answers": [{"text": "紅蘿蔔", "cat": "蔬果與作物"}]}, {"id": 302, "clue": "大顆粒", "answers": [{"text": "水稻", "cat": "蔬果與作物"}]}, {"id": 303, "clue": "小粒", "answers": [{"text": "綠豆", "cat": "蔬果與作物"}]}, {"id": 304, "clue": "孔洞/荷花", "answers": [{"text": "蓮藕", "cat": "蔬果與作物"}]}, {"id": 305, "clue": "水生", "answers": [{"text": "空心菜", "cat": "蔬果與作物"}, {"text": "馬蹄蓮", "cat": "蔬果與作物"}, {"text": "芋頭", "cat": "蔬果與作物"}, {"text": "蓮藕", "cat": "蔬果與作物"}]}, {"id": 306, "clue": "水果", "answers": [{"text": "櫻桃", "cat": "蔬果與作物"}]}, {"id": 307, "clue": "田中、日間、黃粒", "answers": [{"text": "玉米", "cat": "蔬果與作物"}]}, {"id": 308, "clue": "白花", "answers": [{"text": "菜花", "cat": "蔬果與作物"}]}, {"id": 309, "clue": "白扁", "answers": [{"text": "扁豆", "cat": "蔬果與作物"}]}, {"id": 310, "clue": "白筍", "answers": [{"text": "蘆筍", "cat": "蔬果與作物"}]}, {"id": 311, "clue": "白皙青葉", "answers": [{"text": "小白菜", "cat": "蔬果與作物"}]}, {"id": 312, "clue": "多汁", "answers": [{"text": "梨樹", "cat": "蔬果與作物"}, {"text": "番茄", "cat": "蔬果與作物"}, {"text": "桃樹", "cat": "蔬果與作物"}]}, {"id": 313, "clue": "豆科", "answers": [{"text": "蠶豆", "cat": "蔬果與作物"}]}, {"id": 314, "clue": "豆莢", "answers": [{"text": "蠶豆", "cat": "蔬果與作物"}]}, {"id": 315, "clue": "辛苦、球莖、紫皮", "answers": [{"text": "洋蔥", "cat": "蔬果與作物"}]}, {"id": 316, "clue": "空心、空洞、菜地", "answers": [{"text": "芹菜", "cat": "蔬果與作物"}]}, {"id": 317, "clue": "金黃圓潤、酸甜", "answers": [{"text": "柑橘", "cat": "蔬果與作物"}, {"text": "柑橘樹", "cat": "蔬果與作物"}]}, {"id": 318, "clue": "長條、棒狀", "answers": [{"text": "黃瓜", "cat": "蔬果與作物"}]}, {"id": 319, "clue": "紅色", "answers": [{"text": "番茄", "cat": "蔬果與作物"}]}, {"id": 320, "clue": "紅心", "answers": [{"text": "紅薯", "cat": "蔬果與作物"}]}, {"id": 321, "clue": "脆甜", "answers": [{"text": "胡蘿蔔", "cat": "蔬果與作物"}, {"text": "紅蘿蔔", "cat": "蔬果與作物"}, {"text": "萵筍", "cat": "蔬果與作物"}]}, {"id": 322, "clue": "菜園、蒜葉、綠色長莖", "answers": [{"text": "蒜苗", "cat": "蔬果與作物"}]}, {"id": 323, "clue": "菜蔬", "answers": [{"text": "扁豆", "cat": "蔬果與作物"}]}, {"id": 324, "clue": "球形、球型", "answers": [{"text": "花椰菜", "cat": "蔬果與作物"}]}, {"id": 325, "clue": "甜味水果、解暑、種子", "answers": [{"text": "西瓜", "cat": "蔬果與作物"}, {"text": "西瓜籽", "cat": "蔬果與作物"}]}, {"id": 326, "clue": "軟糯", "answers": [{"text": "茄子", "cat": "蔬果與作物"}]}, {"id": 327, "clue": "萵菜、萬菜", "answers": [{"text": "萵苣", "cat": "蔬果與作物"}]}, {"id": 328, "clue": "綠色", "answers": [{"text": "蓮子", "cat": "蔬果與作物"}]}, {"id": 329, "clue": "綠色蔬菜", "answers": [{"text": "小白菜", "cat": "蔬果與作物"}]}, {"id": 330, "clue": "蒸食", "answers": [{"text": "芋頭", "cat": "蔬果與作物"}]}, {"id": 331, "clue": "大水果、熱帶、濃情", "answers": [{"text": "鳳梨蜜", "cat": "蔬果與作物"}, {"text": "波羅蜜", "cat": "蔬果與作物"}]}, {"id": 332, "clue": "五月花神、美麗、觀賞", "answers": [{"text": "芍藥花", "cat": "花卉與景觀"}]}, {"id": 333, "clue": "五月開花、錦簇", "answers": [{"text": "牡丹", "cat": "花卉與景觀"}, {"text": "牡丹花", "cat": "花卉與景觀"}]}, {"id": 334, "clue": "水養殖、素顏、淡波", "answers": [{"text": "水仙花", "cat": "花卉與景觀"}]}, {"id": 335, "clue": "四季", "answers": [{"text": "長春花", "cat": "花卉與景觀"}, {"text": "常春藤", "cat": "花卉與景觀"}]}, {"id": 336, "clue": "多色", "answers": [{"text": "鳳仙花", "cat": "花卉與景觀"}, {"text": "薔薇", "cat": "花卉與景觀"}]}, {"id": 337, "clue": "多彩、短暫", "answers": [{"text": "繡球花", "cat": "花卉與景觀"}, {"text": "百日草", "cat": "花卉與景觀"}]}, {"id": 338, "clue": "有毒、紅白、有毒", "answers": [{"text": "夾竹桃", "cat": "花卉與景觀"}]}, {"id": 339, "clue": "花蕾、芳香", "answers": [{"text": "丁香", "cat": "花卉與景觀"}]}, {"id": 340, "clue": "花邊", "answers": [{"text": "康乃馨", "cat": "花卉與景觀"}]}, {"id": 341, "clue": "垂掛、藤蔓、藍紫", "answers": [{"text": "紫藤", "cat": "花卉與景觀"}]}, {"id": 342, "clue": "垂絲、粉紅、觀賞", "answers": [{"text": "海棠", "cat": "花卉與景觀"}, {"text": "海棠樹", "cat": "花卉與景觀"}]}, {"id": 343, "clue": "春信、藍紫", "answers": [{"text": "風信子", "cat": "花卉與景觀"}]}, {"id": 344, "clue": "春意、紅艷", "answers": [{"text": "朱頂紅", "cat": "花卉與景觀"}]}, {"id": 345, "clue": "染甲", "answers": [{"text": "鳳仙花", "cat": "花卉與景觀"}]}, {"id": 346, "clue": "紅顏、映山、鳴叫", "answers": [{"text": "杜鵑花", "cat": "花卉與景觀"}, {"text": "杜鵑", "cat": "花卉與景觀"}]}, {"id": 347, "clue": "茶香、潔白", "answers": [{"text": "茉莉", "cat": "花卉與景觀"}]}, {"id": 348, "clue": "粉紅、粉嫩、甜蜜", "answers": [{"text": "桃花", "cat": "花卉與景觀"}, {"text": "桃樹", "cat": "花卉與景觀"}]}, {"id": 349, "clue": "粉紅白、核仁、黃果", "answers": [{"text": "杏花", "cat": "花卉與景觀"}, {"text": "杏樹", "cat": "花卉與景觀"}]}, {"id": 350, "clue": "絲狀花、菊花、霜枝", "answers": [{"text": "菊花", "cat": "花卉與景觀"}]}, {"id": 351, "clue": "喇叭", "answers": [{"text": "牽牛花", "cat": "花卉與景觀"}]}, {"id": 352, "clue": "愛意/濃情", "answers": [{"text": "玫瑰", "cat": "花卉與景觀"}]}, {"id": 353, "clue": "寒香、先花後葉", "answers": [{"text": "梅花", "cat": "花卉與景觀"}]}, {"id": 354, "clue": "落英", "answers": [{"text": "櫻花", "cat": "花卉與景觀"}]}, {"id": 355, "clue": "攀爬", "answers": [{"text": "常春藤", "cat": "花卉與景觀"}]}, {"id": 356, "clue": "攀緣", "answers": [{"text": "凌霄", "cat": "花卉與景觀"}, {"text": "薔薇", "cat": "花卉與景觀"}]}, {"id": 357, "clue": "藍黑", "answers": [{"text": "藍莓", "cat": "花卉與景觀"}]}, {"id": 358, "clue": "芳香", "answers": [{"text": "艾葉", "cat": "花卉與景觀"}]}, {"id": 359, "clue": "大且繁華、掌形、鳳凰", "answers": [{"text": "梧桐", "cat": "樹木"}, {"text": "梧桐樹", "cat": "樹木"}]}, {"id": 360, "clue": "四季、蒼翠、長青", "answers": [{"text": "柏樹", "cat": "樹木"}, {"text": "松樹", "cat": "樹木"}]}, {"id": 361, "clue": "古老", "answers": [{"text": "銀杏", "cat": "樹木"}]}, {"id": 362, "clue": "白皮", "answers": [{"text": "楊樹", "cat": "樹木"}]}, {"id": 363, "clue": "柔條", "answers": [{"text": "柳樹", "cat": "樹木"}]}, {"id": 364, "clue": "秋色、紅葉、掌狀", "answers": [{"text": "楓樹", "cat": "樹木"}]}, {"id": 365, "clue": "挺拔", "answers": [{"text": "松樹", "cat": "樹木"}]}, {"id": 366, "clue": "綠冠", "answers": [{"text": "樟樹", "cat": "樹木"}]}, {"id": 367, "clue": "綠葉", "answers": [{"text": "芭蕉", "cat": "樹木"}, {"text": "黃楊", "cat": "樹木"}, {"text": "爬山虎", "cat": "樹木"}]}, {"id": 368, "clue": "綠蔭", "answers": [{"text": "榕樹", "cat": "樹木"}]}, {"id": 369, "clue": "綠陰", "answers": [{"text": "龍眼樹", "cat": "樹木"}]}, {"id": 370, "clue": "橘紅、橙紅", "answers": [{"text": "凌霄", "cat": "樹木"}]}, {"id": 371, "clue": "橢圓葉片", "answers": [{"text": "榆樹", "cat": "樹木"}]}, {"id": 372, "clue": "獨木", "answers": [{"text": "榕樹", "cat": "樹木"}]}, {"id": 373, "clue": "樹樹脂", "answers": [{"text": "沉香", "cat": "樹木"}]}, {"id": 374, "clue": "中藥、甜汁", "answers": [{"text": "甘草", "cat": "藥材與菌類"}]}, {"id": 375, "clue": "止咳、柔軟、黃亮", "answers": [{"text": "枇杷", "cat": "藥材與菌類"}]}, {"id": 376, "clue": "珍貴、傘狀、藥草", "answers": [{"text": "靈芝", "cat": "藥材與菌類"}]}, {"id": 377, "clue": "根莖、藥用", "answers": [{"text": "何首烏", "cat": "藥材與菌類"}]}, {"id": 378, "clue": "藥用、苦、根莖", "answers": [{"text": "黃連", "cat": "藥材與菌類"}]}, {"id": 379, "clue": "藥用、根莖", "answers": [{"text": "西洋參", "cat": "藥材與菌類"}]}, {"id": 380, "clue": "補血、草本", "answers": [{"text": "當歸", "cat": "藥材與菌類"}]}, {"id": 381, "clue": "道旁、藥材、輪生", "answers": [{"text": "車前草", "cat": "藥材與菌類"}]}, {"id": 382, "clue": "香料、紫色、解暑", "answers": [{"text": "藿香", "cat": "藥材與菌類"}]}, {"id": 383, "clue": "滋補、藥效", "answers": [{"text": "人參", "cat": "藥材與菌類"}]}, {"id": 384, "clue": "滋補、藥食", "answers": [{"text": "枸杞", "cat": "藥材與菌類"}]}, {"id": 385, "clue": "傘形、繖形", "answers": [{"text": "香菇", "cat": "藥材與菌類"}]}, {"id": 386, "clue": "提示、提神、清涼", "answers": [{"text": "薄荷腦", "cat": "藥材與菌類"}]}, {"id": 387, "clue": "藥祖", "answers": [{"text": "神農氏", "cat": "人物與角色"}]}, {"id": 388, "clue": "藥材", "answers": [{"text": "山藥", "cat": "藥材與菌類"}]}, {"id": 389, "clue": "藥用", "answers": [{"text": "板藍根", "cat": "藥材與菌類"}]}, {"id": 390, "clue": "菌類", "answers": [{"text": "冬蟲夏草", "cat": "藥材與菌類"}]}, {"id": 391, "clue": "油料、果實", "answers": [{"text": "橄欖", "cat": "藥材與菌類"}, {"text": "橄欖油", "cat": "器物與建築"}]}, {"id": 392, "clue": "三足", "answers": [{"text": "鼎", "cat": "器物與建築"}]}, {"id": 393, "clue": "刀刃", "answers": [{"text": "剪刀", "cat": "器物與建築"}, {"text": "菜刀", "cat": "器物與建築"}]}, {"id": 394, "clue": "工具", "answers": [{"text": "鞴子", "cat": "器物與建築"}, {"text": "輪子", "cat": "器物與建築"}, {"text": "鉤子", "cat": "器物與建築"}]}, {"id": 395, "clue": "中國建築、古蹟", "answers": [{"text": "長城", "cat": "器物與建築"}]}, {"id": 396, "clue": "中繼站、驛站", "answers": [{"text": "驛站", "cat": "器物與建築"}]}, {"id": 397, "clue": "切菜、板實、板實", "answers": [{"text": "案板", "cat": "器物與建築"}]}, {"id": 398, "clue": "木制", "answers": [{"text": "床", "cat": "器物與建築"}, {"text": "筆架", "cat": "器物與建築"}, {"text": "畫筒", "cat": "器物與建築"}, {"text": "佛珠", "cat": "器物與建築"}, {"text": "梳子", "cat": "器物與建築"}, {"text": "箱子", "cat": "器物與建築"}, {"text": "桌案", "cat": "器物與建築"}]}, {"id": 399, "clue": "用具、帶鏡子的家具、床邊家具", "answers": [{"text": "梳妝台", "cat": "器物與建築"}]}, {"id": 400, "clue": "收納", "answers": [{"text": "箱子", "cat": "器物與建築"}]}, {"id": 401, "clue": "米", "answers": [{"text": "米缸", "cat": "器物與建築"}]}, {"id": 402, "clue": "防雨", "answers": [{"text": "蓑衣", "cat": "器物與建築"}]}, {"id": 403, "clue": "防護/戰袍", "answers": [{"text": "鎧甲", "cat": "器物與建築"}]}, {"id": 404, "clue": "兒童玩具", "answers": [{"text": "鞦韆", "cat": "器物與建築"}, {"text": "鞴子", "cat": "器物與建築"}, {"text": "不倒翁", "cat": "器物與建築"}]}, {"id": 405, "clue": "固定、頭端", "answers": [{"text": "釘子", "cat": "器物與建築"}]}, {"id": 406, "clue": "明亮、照明", "answers": [{"text": "蠟燭", "cat": "器物與建築"}]}, {"id": 407, "clue": "油料", "answers": [{"text": "橄欖油", "cat": "器物與建築"}]}, {"id": 408, "clue": "裝飾、陶瓷、插花", "answers": [{"text": "花瓶", "cat": "器物與建築"}]}, {"id": 409, "clue": "運載", "answers": [{"text": "騾子", "cat": "動物與昆蟲"}]}, {"id": 410, "clue": "釣子、金屬", "answers": [{"text": "魚鉤", "cat": "器物與建築"}]}, {"id": 411, "clue": "器具", "answers": [{"text": "青花瓷", "cat": "器物與建築"}]}, {"id": 412, "clue": "橡皮筋", "answers": [{"text": "彈弓", "cat": "器物與建築"}]}, {"id": 413, "clue": "隨身、繡袋", "answers": [{"text": "香囊", "cat": "器物與建築"}]}, {"id": 414, "clue": "鎖閉/鐵製", "answers": [{"text": "鎖具", "cat": "器物與建築"}]}, {"id": 415, "clue": "鐵製", "answers": [{"text": "熨斗", "cat": "器物與建築"}]}, {"id": 416, "clue": "鐵項", "answers": [{"text": "頸圈", "cat": "器物與建築"}]}, {"id": 417, "clue": "鐵腕", "answers": [{"text": "鐵手", "cat": "人物與角色"}]}, {"id": 418, "clue": "繩網、捕鱗、補鱗", "answers": [{"text": "漁網", "cat": "器物與建築"}]}, {"id": 419, "clue": "罐滿、甜味容器", "answers": [{"text": "糖罐", "cat": "器物與建築"}]}, {"id": 420, "clue": "節日食物", "answers": [{"text": "月餅", "cat": "器物與建築"}]}, {"id": 421, "clue": "頭髮", "answers": [{"text": "梳子", "cat": "器物與建築"}]}, {"id": 422, "clue": "上肢運動、命中、箭矢", "answers": [{"text": "射箭", "cat": "武學與競技"}]}, {"id": 423, "clue": "短劍、劍舞、短兵", "answers": [{"text": "短劍", "cat": "武學與競技"}]}, {"id": 424, "clue": "長槍、長桿武器、戰矛", "answers": [{"text": "長槍", "cat": "武學與競技"}, {"text": "長矛", "cat": "武學與競技"}]}, {"id": 425, "clue": "箭簇、投射", "answers": [{"text": "箭簇", "cat": "武學與競技"}]}, {"id": 426, "clue": "武術、傳統武術", "answers": [{"text": "太極", "cat": "武學與競技"}, {"text": "功夫", "cat": "武學與競技"}]}, {"id": 427, "clue": "比武", "answers": [{"text": "穆念慈", "cat": "人物與角色"}]}, {"id": 428, "clue": "多人遊戲", "answers": [{"text": "捉迷藏", "cat": "武學與競技"}, {"text": "足球", "cat": "武學與競技"}]}, {"id": 429, "clue": "投擲", "answers": [{"text": "投壺", "cat": "武學與競技"}, {"text": "飛鏢", "cat": "武學與競技"}]}, {"id": 430, "clue": "流星錘、練舞、鏈舞", "answers": [{"text": "流星錘", "cat": "武學與競技"}]}, {"id": 431, "clue": "競技運動、摔跤", "answers": [{"text": "相撲", "cat": "武學與競技"}]}, {"id": 432, "clue": "劍鞘", "answers": [{"text": "寶劍", "cat": "武學與競技"}]}, {"id": 433, "clue": "箭術", "answers": [{"text": "扳指", "cat": "武學與競技"}, {"text": "花榮", "cat": "人物與角色"}]}, {"id": 434, "clue": "射戟", "answers": [{"text": "呂布", "cat": "人物與角色"}]}, {"id": 435, "clue": "射戰", "answers": [{"text": "文醜", "cat": "人物與角色"}]}, {"id": 436, "clue": "射擊遊戲", "answers": [{"text": "彈弓", "cat": "器物與建築"}]}, {"id": 437, "clue": "大水", "answers": [{"text": "瀑布", "cat": "自然與其他"}]}, {"id": 438, "clue": "方向、方位、方位", "answers": [{"text": "指南針", "cat": "器物與建築"}]}, {"id": 439, "clue": "火助", "answers": [{"text": "風箱", "cat": "器物與建築"}]}, {"id": 440, "clue": "加熱", "answers": [{"text": "飯菜", "cat": "自然與其他"}]}, {"id": 441, "clue": "自然現象", "answers": [{"text": "雨", "cat": "自然與其他"}]}, {"id": 442, "clue": "風沙", "answers": [{"text": "胡楊", "cat": "樹木"}]}, {"id": 443, "clue": "沙漠", "answers": [{"text": "胡楊", "cat": "樹木"}, {"text": "蠍子", "cat": "動物與昆蟲"}]}, {"id": 444, "clue": "雨霖", "answers": [{"text": "柳永", "cat": "人物與角色"}]}, {"id": 445, "clue": "夜空、點燃、燦爛", "answers": [{"text": "放煙火", "cat": "自然與其他"}]}, {"id": 446, "clue": "深海", "answers": [{"text": "鱷魚", "cat": "動物與昆蟲"}, {"text": "鱈魚", "cat": "動物與昆蟲"}, {"text": "電鰻", "cat": "動物與昆蟲"}, {"text": "鯊魚", "cat": "動物與昆蟲"}, {"text": "魷魚", "cat": "動物與昆蟲"}]}, {"id": 447, "clue": "淺海", "answers": [{"text": "海象", "cat": "動物與昆蟲"}, {"text": "海牛", "cat": "動物與昆蟲"}]}, {"id": 448, "clue": "清涼", "answers": [{"text": "水缸", "cat": "器物與建築"}]}, {"id": 449, "clue": "潮濕、陰濕、覆蓋", "answers": [{"text": "苔蘚", "cat": "花卉與景觀"}]}, {"id": 450, "clue": "陰陽、太極", "answers": [{"text": "陰陽", "cat": "自然與其他"}]}, {"id": 451, "clue": "瀑布", "answers": [{"text": "瀑布", "cat": "自然與其他"}]}, {"id": 452, "clue": "黑白", "answers": [{"text": "陰陽", "cat": "自然與其他"}, {"text": "太極", "cat": "武學與競技"}, {"text": "企鵝", "cat": "動物與昆蟲"}]}, {"id": 453, "clue": "硬殼", "answers": [{"text": "海龜", "cat": "動物與昆蟲"}]}, {"id": 454, "clue": "冷熱", "answers": [{"text": "熨斗", "cat": "器物與建築"}]}, {"id": 455, "clue": "多角", "answers": [{"text": "菱角", "cat": "蔬果與作物"}]}, {"id": 456, "clue": "柔勁、蛇行、鞭影", "answers": [{"text": "長鞭", "cat": "武學與競技"}]}, {"id": 457, "clue": "命中", "answers": [{"text": "射箭", "cat": "武學與競技"}]}, {"id": 458, "clue": "圓周", "answers": [{"text": "祖沖之", "cat": "人物與角色"}]}, {"id": 459, "clue": "醫治", "answers": [{"text": "郎中", "cat": "人物與角色"}]}, {"id": 460, "clue": "感謝", "answers": [{"text": "康乃馨", "cat": "花卉與景觀"}]}, {"id": 461, "clue": "雜交", "answers": [{"text": "騾子", "cat": "動物與昆蟲"}]}, {"id": 462, "clue": "鬆土", "answers": [{"text": "耙子", "cat": "器物與建築"}, {"text": "蚯蚓", "cat": "動物與昆蟲"}]}, {"id": 463, "clue": "精油", "answers": [{"text": "檸檬", "cat": "蔬果與作物"}]}, {"id": 464, "clue": "醬香、調味品", "answers": [{"text": "醬油", "cat": "器物與建築"}]}, {"id": 465, "clue": "色彩", "answers": [{"text": "丹青", "cat": "自然與其他"}]}, {"id": 466, "clue": "清脆", "answers": [{"text": "蘋果", "cat": "蔬果與作物"}]}],CATS=["人物與角色", "典故與文化", "動物與昆蟲", "蔬果與作物", "花卉與景觀", "樹木", "藥材與菌類", "器物與建築", "武學與競技", "自然與其他"],KEY='yanyun-shefu-notebook-v1';
const $=id=>document.getElementById(id);let DATA=JSON.parse(JSON.stringify(BASE));
let state={favorites:[],recent:[],q:'',category:'',mode:'clues'},limit=40,storageOK=true;
function validateDB(rows){
 if(!Array.isArray(rows)||!rows.length||rows.length>20000)throw Error('題庫格式不正確');
 const seen=new Set();return rows.map(r=>{
 if(!r||!Number.isSafeInteger(r.id)||r.id<1||seen.has(r.id)||typeof r.clue!=='string'||!r.clue.trim()||r.clue.length>300||!Array.isArray(r.answers)||!r.answers.length||r.answers.length>50)throw Error('題目格式不正確');seen.add(r.id);
 return {id:r.id,clue:r.clue.trim(),answers:r.answers.map(a=>{if(!a||typeof a.text!=='string'||!a.text.trim()||a.text.length>150||typeof a.cat!=='string'||!a.cat.trim()||a.cat.length>50)throw Error('答案或分類格式不正確');return {text:a.text.trim(),cat:a.cat.trim()};})};});
}
function categories(rows=DATA){return [...new Set([...CATS,...rows.flatMap(r=>r.answers.map(a=>a.cat))])];}
function valid(s,rows=DATA){if(!s||typeof s!=='object'||Array.isArray(s))throw Error('格式不正確');const allowed=new Set(rows.map(r=>r.id));return {favorites:Array.isArray(s.favorites)?[...new Set(s.favorites.filter(x=>allowed.has(x)))]:[],recent:Array.isArray(s.recent)?s.recent.filter(x=>typeof x==='string'&&x.length<=150).slice(0,6):[],q:typeof s.q==='string'?s.q.slice(0,150):'',category:categories(rows).includes(s.category)?s.category:'',mode:['clues','answers','favorites'].includes(s.mode)?s.mode:'clues'};}
try{const saved=localStorage.getItem(KEY);if(saved){const d=JSON.parse(saved);const rows=d.database?validateDB(d.database):DATA;const next=valid(d,rows);DATA=rows;state=next;}localStorage.setItem(KEY,JSON.stringify({...state,database:DATA}));}catch(e){storageOK=false;}
function storageStatus(){ $('storage').textContent=storageOK?'● 題庫與個人紀錄已啟用自動儲存':'△ 瀏覽器未允許儲存；本次修改尚未永久保存，請立即匯出完整備份。';$('storage').className=storageOK?'':'warning';}
function save(){try{localStorage.setItem(KEY,JSON.stringify({...state,database:DATA}));storageOK=true;}catch(e){storageOK=false;}storageStatus();}
const esc=s=>String(s).replace(/[&<>"']/g,c=>({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;'}[c]));
const norm=s=>s.normalize('NFKC').toLocaleLowerCase().replace(/\s+/g,' ').trim();
function marked(s){const tokens=state.q.trim().split(/\s+/).filter(Boolean);if(!tokens.length)return esc(s);const pattern=tokens.map(t=>t.replace(/[.*+?^${}()|[\]\\]/g,'\\$&')).join('|');return s.split(new RegExp('('+pattern+')','ig')).map((x,i)=>i%2?'<mark>'+esc(x)+'</mark>':esc(x)).join('');}
function matches(text){return norm(state.q).split(' ').filter(Boolean).every(t=>norm(text).includes(t));}
function toast(msg){$('toast').textContent=msg;clearTimeout(toast.timer);toast.timer=setTimeout(()=>$('toast').textContent='',2800);}
function recents(){ $('recent').innerHTML=state.recent.map(s=>'<button type="button" data-recent="'+esc(s)+'">'+esc(s)+'</button>').join('');}
function render(){
 $('total').textContent=DATA.length;$('atotal').textContent=new Set(DATA.flatMap(r=>r.answers.map(a=>a.text))).size;
 $('category').innerHTML='<option value="">全部答案分類</option>'+categories().map(c=>'<option value="'+esc(c)+'">'+esc(c)+'</option>').join('');
 $('q').value=state.q;$('category').value=state.category;document.querySelectorAll('[data-mode]').forEach(b=>{const on=b.dataset.mode===state.mode;b.classList.toggle('active',on);b.setAttribute('aria-pressed',on);});$('fcount').textContent=state.favorites.length;recents();
 let items;
 if(state.mode==='answers'){
 const groups=new Map();DATA.forEach(row=>row.answers.forEach(a=>{if(state.category&&a.cat!==state.category)return;let g=groups.get(a.text);if(!g){g={text:a.text,cat:a.cat,rows:[]};groups.set(a.text,g);}g.rows.push(row);}));items=[...groups.values()].filter(g=>matches(g.text+' '+g.rows.map(r=>r.clue).join(' '))).sort((a,b)=>a.text.localeCompare(b.text,'zh-Hant'));
 $('result').textContent='找到 '+items.length+' 個答案 · 已顯示 '+Math.min(limit,items.length)+' 個';
 $('cards').innerHTML=items.slice(0,limit).map(g=>'<article class="card"><span class="label">'+esc(g.cat)+'</span><h2 class="clue">'+marked(g.text)+'</h2><div class="links">'+g.rows.map(r=>'<button data-clue="'+r.id+'">'+marked(r.clue)+'</button>').join('')+'</div><div class="cardfoot"><span>'+g.rows.length+' 條對應線索</span><button class="copy" data-copy="'+esc(g.text)+'">複製答案</button></div></article>').join('');
 }else{
 items=DATA.filter(r=>(state.mode!=='favorites'||state.favorites.includes(r.id))&&(!state.category||r.answers.some(a=>a.cat===state.category))&&matches(r.clue+' '+r.answers.map(a=>a.text).join(' ')));
 items.sort((a,b)=>{const score=r=>norm(r.clue)===norm(state.q)?0:r.answers.some(a=>norm(a.text)===norm(state.q))?1:2;return score(a)-score(b)||a.id-b.id;});
 $('result').textContent='找到 '+items.length+' 條線索 · 已顯示 '+Math.min(limit,items.length)+' 條';
 $('cards').innerHTML=items.slice(0,limit).map(r=>{const on=state.favorites.includes(r.id);return '<article class="card"><div class="cardtop"><div><span class="label">NPC 線索</span><h2 class="clue">'+marked(r.clue)+'</h2></div><button class="star" data-star="'+r.id+'" aria-pressed="'+on+'" aria-label="'+(on?'取消收藏':'收藏')+'：'+esc(r.clue)+'">'+(on?'★':'☆')+'</button></div><div class="answerlist">'+r.answers.map(a=>'<button class="answer" data-answer="'+esc(a.text)+'">'+marked(a.text)+'<small>'+esc(a.cat)+'</small></button>').join('')+'</div><div class="cardfoot"><span>'+(r.answers.length>1?'多個候選 · 請核對後續提示':'對應答案')+'</span><button class="copy" data-edit="'+r.id+'">編輯題目</button><button class="copy" data-copy="'+esc(r.answers.map(a=>a.text).join('／'))+'">複製答案</button></div></article>';}).join('');
 }
 if(!items.length)$('cards').innerHTML='<div class="empty"><p>這一頁暫時沒有答案。</p><p>'+(state.mode==='favorites'?'點線索旁的 ☆，把常用題目收入小箋。':'試試更短的關鍵字，或清除分類篩選。')+'</p></div>';
 $('more').hidden=items.length<=limit;storageStatus();
}
function change(){limit=40;save();render();}
function query(q){state.q=q;state.category='';state.mode='clues';change();window.scrollTo({top:0,behavior:'auto'});}
$('q').addEventListener('input',e=>{state.q=e.target.value;limit=40;save();render();});
$('searchform').addEventListener('submit',e=>{e.preventDefault();const q=state.q.trim();if(q)state.recent=[q,...state.recent.filter(x=>x!==q)].slice(0,6);$('q').blur();change();});
$('category').addEventListener('change',e=>{state.category=e.target.value;change();});
$('reset').onclick=()=>{state.q='';state.category='';change();};
$('more').onclick=()=>{limit+=40;render();};
document.addEventListener('click',async e=>{const b=e.target.closest('button');if(!b)return;if(b.dataset.edit){openEditor(Number(b.dataset.edit));}if(b.dataset.removeAnswer){const row=b.closest('.answerrow');if($('answerrows').children.length>1)row.remove();else toast('至少保留一個答案');}if(b.dataset.mode){state.mode=b.dataset.mode;change();}if(b.dataset.star){const id=Number(b.dataset.star);state.favorites=state.favorites.includes(id)?state.favorites.filter(x=>x!==id):[...state.favorites,id];save();render();}if(b.dataset.answer)query(b.dataset.answer);if(b.dataset.clue)query(DATA.find(r=>r.id===Number(b.dataset.clue)).clue);if(b.dataset.recent)query(b.dataset.recent);if(b.dataset.copy){try{await navigator.clipboard.writeText(b.dataset.copy);toast('答案已複製');}catch(err){const t=document.createElement('textarea');t.value=b.dataset.copy;t.style.position='fixed';t.style.top='0';document.body.append(t);t.focus();t.select();t.setSelectionRange(0,t.value.length);let ok=false;try{ok=document.execCommand('copy');}catch(e){}t.remove();toast(ok?'答案已複製':'無法自動複製，請長按答案文字。');}}});
let editingId=null;
function answerRow(a={text:'',cat:'自然與其他'}){
 const row=document.createElement('div');row.className='answerrow';
 row.innerHTML='<label>答案<input class="editanswer" required maxlength="150" placeholder="例如：孟獲" value="'+esc(a.text)+'"></label><label>分類<input class="editcat" required maxlength="50" list="categorylist" placeholder="選擇或輸入分類" value="'+esc(a.cat)+'"></label><button type="button" data-remove-answer="1">移除此答案</button>';
 $('answerrows').append(row);
}
function openEditor(id=null){
 editingId=id;const r=id===null?null:DATA.find(r=>r.id===id);if(id!==null&&!r)return;
 $('editortitle').textContent=r?'編輯題目':'新增題目';$('editclue').value=r?r.clue:'';$('answerrows').innerHTML='';$('formerror').textContent='';
 $('categorylist').innerHTML=categories().map(c=>'<option value="'+esc(c)+'"></option>').join('');
 (r?r.answers:[{text:'',cat:state.category||'自然與其他'}]).forEach(answerRow);
 $('editor').showModal();$('editclue').focus();
}
function storeQuestion(id,clue,answers){
 if(id!==null&&!DATA.some(r=>r.id===id))throw Error('找不到原題目');
 const row=validateDB([{id:id===null?Math.max(0,...DATA.map(r=>r.id))+1:id,clue,answers}])[0];
 if(DATA.some(r=>r.id!==row.id&&norm(r.clue)===norm(row.clue)&&JSON.stringify(r.answers)===JSON.stringify(row.answers)))throw Error('這組題目與答案已存在');
 DATA=id===null?[...DATA,row]:DATA.map(r=>r.id===id?row:r);state.q=row.clue;state.category='';state.mode='clues';change();return row;
}
$('newquestion').onclick=()=>openEditor();$('closeeditor').onclick=$('canceleditor').onclick=()=>$('editor').close();
$('addanswer').onclick=()=>{if($('answerrows').children.length>=50){toast('每題最多 50 個答案');return;}answerRow({text:'',cat:state.category||'自然與其他'});};
$('editorform').onsubmit=e=>{e.preventDefault();try{
 const answers=[...$('answerrows').querySelectorAll('.answerrow')].map(row=>({text:row.querySelector('.editanswer').value,cat:row.querySelector('.editcat').value}));
 storeQuestion(editingId,$('editclue').value,answers);$('editor').close();toast(storageOK?'題目已儲存':'已更新本次題庫，請匯出備份以保留修改');
 }catch(err){$('formerror').textContent=err.message;}};
function backup(){return {app:KEY,version:2,state,database:DATA};}
function restoreBackup(d){
 if(!d||d.app!==KEY||![1,2].includes(d.version))throw Error('備份格式不正確');
 const rows=d.version===2?validateDB(d.database):DATA;const next=valid(d.state,rows);
 return {rows,next};
}
$('export').onclick=()=>{const blob=new Blob([JSON.stringify(backup(),null,2)],{type:'application/json'});const url=URL.createObjectURL(blob),a=document.createElement('a');a.href=url;a.download='射覆小箋_完整題庫備份.json';document.body.append(a);a.click();a.remove();setTimeout(()=>URL.revokeObjectURL(url),30000);toast('已產生完整題庫備份');};
$('import').onclick=()=>$('importfile').click();
$('importfile').onchange=async e=>{const f=e.target.files[0];if(!f)return;try{
 if(f.size>20000000)throw Error();const d=JSON.parse(await f.text());const {rows,next}=restoreBackup(d);
 if(confirm(d.version===2?'匯入將取代目前全部題庫、收藏與瀏覽設定，確定匯入？':'這是舊版個人紀錄備份，將取代收藏與瀏覽設定；目前題庫會保留。確定匯入？')){DATA=rows;state=next;change();toast(storageOK?'題庫備份已匯入':'已匯入本次題庫，瀏覽器未允許永久儲存');}
 }catch(err){toast('匯入失敗：請選擇此工具匯出的有效 JSON 備份。');}e.target.value='';};
render();
</script></body></html>
class="flex items-center space-x-3">
                  <span class="text-[10pt] px-2 py-0.5 rounded bg-white text-ink-textMuted border border-ink-border" x-text="item.category"></span>
                  <span class="text-title font-bold text-ink-textMain tracking-wide truncate" x-text="item.clue"></span>
                </div>
                <div class="text-content text-ink-brown mt-1.5 flex items-center">
                  <i class="fa-solid fa-caret-right text-[10pt] mr-2 opacity-50"></i>
                  <span class="font-bold tracking-widest truncate" x-text="item.ans"></span>
                </div>
              </div>

              <div class="flex items-center space-x-1 flex-shrink-0">
                <button 
                  @click="openEditModal(item)" 
                  class="p-2.5 text-ink-textMuted hover:text-ink-green hover:bg-white rounded border border-transparent hover:border-ink-border transition-all">
                  <i class="fa-solid fa-pen-nib text-content"></i>
                </button>
                <button 
                  @click="confirmDelete(item)" 
                  class="p-2.5 text-ink-textMuted hover:text-red-700 hover:bg-red-50 rounded border border-transparent hover:border-red-200 transition-all">
                  <i class="fa-solid fa-trash-can text-content"></i>
                </button>
              </div>
            </div>
          </template>

          <div x-show="filteredItems.length === 0" class="p-10 text-center text-content text-ink-textMuted">
            書架空空如也
          </div>
        </div>
      </div>

    </div>

  </main>

  <!-- MODAL: ADD / EDIT ITEM -->
  <div 

    x-show="showEditModal" 
    x-transition:enter="transition ease-out duration-300"
    x-transition:enter-start="opacity-0"
    x-transition:enter-end="opacity-100"
    x-transition:leave="transition ease-in duration-200"
    x-transition:leave-start="opacity-100"
    x-transition:leave-end="opacity-0"
    class="fixed inset-0 z-50 flex items-center justify-center p-4 bg-ink-textMain/40 backdrop-blur-sm"
    style="display: none;">
    
    <div 
      @click.outside="showEditModal = false"
      class="bg-ink-bg border-2 border-ink-brown rounded w-full max-w-md shadow-2xl relative">
      
      <!-- 卷軸裝飾邊 -->
      <div class="absolute left-0 top-0 bottom-0 w-2 bg-ink-brown/10 border-r border-ink-border"></div>

      <div class="p-6">
        <div class="flex justify-between items-center mb-4">
          <h3 class="font-bold text-title text-ink-brown flex items-center space-x-2 tracking-widest pl-2 border-l-4 border-ink-green">
            <span x-text="isEditing ? '修訂卷宗' : '收錄新題'"></span>
          </h3>
          <button @click="showEditModal = false" class="text-ink-textMuted hover:text-ink-brown">
            <i class="fa-solid fa-xmark text-lg"></i>
          </button>
        </div>

        <div class="ink-divider mt-0 mb-5"></div>

        <div class="space-y-4 text-content pl-2">
          <div>
            <label class="block text-ink-textMain font-bold mb-1.5">歸屬門派 (分類)</label>
            <select 
              x-model="formItem.category"
              class="w-full bg-ink-card border border-ink-border rounded px-3 py-2.5 text-ink-textMain outline-none focus:border-ink-green focus:ring-1 focus:ring-ink-green shadow-inner">
              <template x-for="cat in categories.filter(c => c !== '全部')" :key="cat">
                <option :value="cat" x-text="cat"></option>
              </template>
            </select>
          </div>

          <div>
            <label class="block text-ink-textMain font-bold mb-1.5">題面 (射覆提示)</label>
            <input 
              type="text" 
              x-model="formItem.clue"
              placeholder="請輸入提示字眼..." 
              class="w-full bg-ink-card border border-ink-border rounded px-3 py-2.5 text-ink-textMain outline-none focus:border-ink-green focus:ring-1 focus:ring-ink-green shadow-inner"
            />
          </div>

          <div>
            <label class="block text-ink-textMain font-bold mb-1.5">謎底 (對應答案)</label>
            <input 
              type="text" 
              x-model="formItem.ans"
              placeholder="請輸入正確解答..." 
              class="w-full bg-ink-card border border-ink-border rounded px-3 py-2.5 text-ink-textMain outline-none focus:border-ink-green focus:ring-1 focus:ring-ink-green shadow-inner"
            />
          </div>
        </div>

        <div class="flex items-center space-x-3 pt-6 mt-6 border-t border-ink-border pl-2">
          <button 
            @click="showEditModal = false"
            class="flex-1 py-2.5 bg-ink-card hover:bg-white text-ink-textMain border border-ink-border rounded text-content font-medium transition-all shadow-sm">
            暫且擱置
          </button>
          <button 
            @click="saveItem()"
            class="flex-1 py-2.5 bg-ink-green hover:bg-ink-greenLight text-white rounded text-content font-bold shadow-ink-strong transition-all tracking-widest">
            落筆封存
          </button>
        </div>
      </div>
    </div>
  </div>

  <!-- MODAL: CONFIRMATION -->
  <div 
    x-show="showConfirmModal" 
    x-transition
    class="fixed inset-0 z-50 flex items-center justify-center p-4 bg-ink-textMain/40 backdrop-blur-sm"
    style="display: none;">
    <div 
      @click.outside="showConfirmModal = false"
      class="bg-ink-bg border-2 border-ink-brown rounded w-full max-w-sm p-6 shadow-2xl text-center">
      
      <div class="w-14 h-14 bg-red-50 border border-red-200 text-red-700 rounded-full flex items-center justify-center mx-auto text-2xl mb-4 shadow-sm">
        <i class="fa-solid fa-bell"></i>
      </div>

      <div>
        <h3 class="font-bold text-title text-ink-brown tracking-widest" x-text="confirmTitle"></h3>
        <p class="text-content text-ink-textMain mt-3 leading-loose" x-text="confirmMsg"></p>
      </div>

      <div class="flex items-center space-x-3 pt-6 mt-6 border-t border-ink-border">
        <button 
          @click="showConfirmModal = false"
          class="flex-1 py-2.5 bg-ink-card hover:bg-white text-ink-textMain border border-ink-border rounded text-content font-medium transition-all shadow-sm">
          收回成命
        </button>
        <button 
          @click="onConfirm(); showConfirmModal = false;"
          class="flex-1 py-2.5 bg-red-700 hover:bg-red-600 text-white rounded text-content font-bold shadow-md transition-all tracking-widest">
          執意如此
        </button>
      </div>

    </div>
  </div>

  <!-- MODAL: IMPORT / EXPORT -->
  <div 
    x-show="showImportModal" 
    x-transition
    class="fixed inset-0 z-50 flex items-center justify-center p-4 bg-ink-textMain/40 backdrop-blur-sm"
    style="display: none;">
    <div 
      @click.outside="showImportModal = false"
      class="bg-ink-bg border-2 border-ink-brown rounded w-full max-w-lg p-6 shadow-2xl space-y-4">
      
      <div class="flex justify-between items-center border-b border-ink-border pb-3">
        <h3 class="font-bold text-title text-ink-brown flex items-center space-x-2 tracking-widest">
          <i class="fa-solid fa-scroll text-ink-green"></i>
          <span>拓本與謄寫 (JSON)</span>
        </h3>
        <button @click="showImportModal = false" class="text-ink-textMuted hover:text-ink-brown">
          <i class="fa-solid fa-xmark text-lg"></i>
        </button>
      </div>

      <p class="text-content text-ink-textMain leading-relaxed">
        可複製下方密文以妥善保管您的題庫，亦可貼上他人給予的密文，以全新內容覆蓋現有書庫。
      </p>

      <textarea 
        x-model="importExportText"
        rows="8"
        class="w-full bg-ink-card border border-ink-border rounded p-3 text-[10pt] text-ink-brown font-mono outline-none focus:border-ink-green focus:ring-1 focus:ring-ink-green shadow-inner"
        placeholder="請於此處貼上 JSON 格式之密文..."></textarea>

      <div class="flex flex-col sm:flex-row items-center justify-between gap-3 pt-4 border-t border-ink-border">
        <button 
          @click="copyExportText()"
          class="w-full sm:w-auto px-5 py-2.5 bg-ink-card hover:bg-white text-ink-textMain border border-ink-border text-content font-bold rounded transition-all flex items-center justify-center space-x-2 shadow-sm">
          <i class="fa-regular fa-copy"></i>
          <span class="tracking-widest">拓印全文</span>
        </button>

        <div class="flex w-full sm:w-auto space-x-2">
          <button 
            @click="showImportModal = false"
            class="flex-1 sm:flex-none px-4 py-2.5 bg-transparent hover:bg-ink-card text-ink-textMuted rounded text-content font-medium transition-all">
            取消
          </button>
          <button 
            @click="importData()"
            class="flex-1 sm:flex-none px-5 py-2.5 bg-ink-brown hover:bg-ink-brownDark text-white rounded text-content font-bold shadow-ink-soft transition-all flex items-center justify-center space-x-2">
            <i class="fa-solid fa-stamp"></i>
            <span class="tracking-widest">覆寫入庫</span>
          </button>
        </div>
      </div>

    </div>
  </div>

  <!-- TOAST NOTIFICATION -->
  <div 
    x-show="showToast" 
    x-transition:enter="transition ease-out duration-300 transform"
    x-transition:enter-start="opacity-0 translate-y-4 scale-95"
    x-transition:enter-end="opacity-100 translate-y-0 scale-100"
    x-transition:leave="transition ease-in duration-200 transform"
    x-transition:leave-start="opacity-100 translate-y-0 scale-100"
    x-transition:leave-end="opacity-0 translate-y-4 scale-95"
    class="fixed bottom-8 left-1/2 -translate-x-1/2 z-50 bg-ink-brown text-ink-bg px-6 py-3 rounded shadow-2xl text-content font-bold flex items-center space-x-3 pointer-events-none border border-ink-brownDark tracking-wider"
    style="display: none;">
    <i class="fa-solid fa-circle-check text-ink-bg opacity-80 text-title"></i>
    <span x-text="toastMessage"></span>
  </div>

</body>
</html>
