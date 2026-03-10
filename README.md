[加咗Aikey.html](https://github.com/user-attachments/files/25865691/Aikey.html)
<!DOCTYPE html>
<html lang="zh-HK">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>歌迷序會：Elaine生日特別版</title>
    
    <!-- 引入 Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    
    <!-- 引入 React 和 ReactDOM -->
    <script crossorigin src="https://unpkg.com/react@18/umd/react.production.min.js"></script>
    <script crossorigin src="https://unpkg.com/react-dom@18/umd/react-dom.production.min.js"></script>
    
    <!-- 引入 Babel 以便在瀏覽器中編譯 JSX -->
    <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>

    <style>
        body { background-color: #020617; margin: 0; font-family: sans-serif; }
        ::-webkit-scrollbar { width: 0px; background: transparent; }
    </style>
</head>
<body>
    <div id="root"></div>

    <script type="text/babel">
        const { useState, useEffect, useRef } = React;

        // --- 內建圖示組件 ---
        const Icon = ({ children, className = "w-6 h-6", fill = "none" }) => (
            <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill={fill} stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round" className={className}>
                {children}
            </svg>
        );
        const Cake = ({ className }) => <Icon className={className}><path d="M20 21v-8a2 2 0 0 0-2-2H6a2 2 0 0 0-2 2v8"/><path d="M4 16s.5-1 2-1 2.5 2 4 2 2.5-2 4-2 2.5 2 4 2 2 1 2 1"/><path d="M2 21h20"/><path d="M7 8v2"/><path d="M12 8v2"/><path d="M17 8v2"/><path d="M7 4h.01"/><path d="M12 4h.01"/><path d="M17 4h.01"/></Icon>;
        const Heart = ({ className, fill }) => <Icon className={className} fill={fill}><path d="M19 14c1.49-1.46 3-3.21 3-5.5A5.5 5.5 0 0 0 16.5 3c-1.76 0-3 .5-4.5 2-1.5-1.5-2.74-2-4.5-2A5.5 5.5 0 0 0 2 8.5c0 2.3 1.5 4.05 3 5.5l7 7Z"/></Icon>;
        const Gift = ({ className }) => <Icon className={className}><rect x="3" y="8" width="18" height="4" rx="1"/><path d="M12 8v13"/><path d="M19 12v7a2 2 0 0 1-2 2H7a2 2 0 0 1-2-2v-7"/><path d="M7.5 8a2.5 2.5 0 0 1 0-5A4.8 8 0 0 1 12 8a4.8 8 0 0 1 4.5-5 2.5 2.5 0 0 1 0 5"/></Icon>;
        const Music = ({ className }) => <Icon className={className}><path d="M9 18V5l12-2v13"/><circle cx="6" cy="18" r="3"/><circle cx="18" cy="16" r="3"/></Icon>;
        const Star = ({ className, fill }) => <Icon className={className} fill={fill}><polygon points="12 2 15.09 8.26 22 9.27 17 14.14 18.18 21.02 12 17.77 5.82 21.02 7 14.14 2 9.27 8.91 8.26 12 2"/></Icon>;
        const Play = ({ className }) => <Icon className={className} fill="currentColor"><polygon points="5 3 19 12 5 21 5 3"/></Icon>;
        const Pause = ({ className }) => <Icon className={className} fill="currentColor"><rect x="6" y="4" width="4" height="16"/><rect x="14" y="4" width="4" height="16"/></Icon>;
        const SkipForward = ({ className }) => <Icon className={className} fill="currentColor"><polygon points="5 4 15 12 5 20 5 4"/><line x1="19" y1="5" x2="19" y2="19"/></Icon>;
        const Trophy = ({ className }) => <Icon className={className}><path d="M6 9H4.5a2.5 2.5 0 0 1 0-5H6"/><path d="M18 9h1.5a2.5 2.5 0 0 0 0-5H18"/><path d="M4 22h16"/><path d="M10 14.66V17c0 .55-.47.98-.97 1.21C7.85 18.75 7 20.24 7 22"/><path d="M14 14.66V17c0 .55.47.98.97 1.21C16.15 18.75 17 20.24 17 22"/><path d="M18 2H6v7a6 6 0 0 0 12 0V2Z"/></Icon>;
        const HelpCircle = ({ className }) => <Icon className={className}><circle cx="12" cy="12" r="10"/><path d="M9.09 9a3 3 0 0 1 5.83 1c0 2-3 3-3 3"/><line x1="12" y1="17" x2="12.01" y2="17"/></Icon>;
        const Sparkles = ({ className }) => <Icon className={className}><path d="m12 3-1.912 5.813a2 2 0 0 1-1.275 1.275L3 12l5.813 1.912a2 2 0 0 1 1.275 1.275L12 21l1.912-5.813a2 2 0 0 1 1.275-1.275L21 12l-5.813-1.912a2 2 0 0 1-1.275-1.275L12 3Z"/><path d="M5 3v4"/><path d="M3 5h4"/><path d="M19 17v4"/><path d="M17 19h4"/></Icon>;
        const Loader2 = ({ className }) => <Icon className={className}><path d="M21 12a9 9 0 1 1-6.219-8.56"/></Icon>;

        // --- K-pop 資料庫 ---
        const KPOP_DATABASE = [
            { title: "Tell Me", artist: "Wonder Girls", year: 2007, term: "Wonder Girls Tell Me" },
            { title: "Replay", artist: "SHINee", year: 2008, term: "SHINee Replay" },
            { title: "Haru Haru", artist: "BIGBANG", year: 2008, term: "BIGBANG Haru Haru" },
            { title: "Mirotic", artist: "TVXQ!", year: 2008, term: "TVXQ Mirotic" },
            { title: "Sorry, Sorry", artist: "Super Junior", year: 2009, term: "Super Junior Sorry Sorry" },
            { title: "Bad Girl Good Girl", artist: "Miss A", year: 2010, term: "Miss A Bad Girl Good Girl" },
            { title: "I Am The Best", artist: "2NE1", year: 2011, term: "2NE1 I Am The Best" },
            { title: "Roly-Poly", artist: "T-ARA", year: 2011, term: "T-ARA Roly Poly" },
            { title: "Be Mine", artist: "INFINITE", year: 2011, term: "INFINITE Be Mine" },
            { title: "Fantastic Baby", artist: "BIGBANG", year: 2012, term: "BIGBANG Fantastic Baby" },
            { title: "Electric Shock", artist: "f(x)", year: 2012, term: "fx Electric Shock" },
            { title: "Growl", artist: "EXO", year: 2013, term: "EXO Growl" },
            { title: "NoNoNo", artist: "Apink", year: 2013, term: "Apink NoNoNo" },
            { title: "Touch My Body", artist: "SISTAR", year: 2014, term: "SISTAR Touch My Body" },
            { title: "Up & Down", artist: "EXID", year: 2014, term: "EXID Up Down" },
            { title: "Me Gustas Tu", artist: "GFRIEND", year: 2015, term: "GFRIEND Me Gustas Tu" },
            { title: "My House", artist: "2PM", year: 2015, term: "2PM My House" },
            { title: "Cheer Up", artist: "TWICE", year: 2016, term: "TWICE Cheer Up" },
            { title: "Very Nice", artist: "SEVENTEEN", year: 2016, term: "SEVENTEEN Very Nice" },
            { title: "Blood Sweat & Tears", artist: "BTS", year: 2016, term: "BTS Blood Sweat Tears" },
            { title: "Spring Day", artist: "BTS", year: 2017, term: "BTS Spring Day" },
            { title: "Missing You", artist: "BTOB", year: 2017, term: "BTOB Missing You" },
            { title: "You Were Beautiful", artist: "DAY6", year: 2017, term: "DAY6 You Were Beautiful" },
            { title: "Love Scenario", artist: "iKON", year: 2018, term: "iKON Love Scenario" },
            { title: "DDU-DU DDU-DU", artist: "BLACKPINK", year: 2018, term: "BLACKPINK DDU-DU" },
            { title: "Crown", artist: "TXT", year: 2019, term: "TXT Crown" },
            { title: "Psycho", artist: "Red Velvet", year: 2019, term: "Red Velvet Psycho" },
            { title: "God's Menu", artist: "Stray Kids", year: 2020, term: "Stray Kids Gods Menu" },
            { title: "WANNABE", artist: "ITZY", year: 2020, term: "ITZY WANNABE" },
            { title: "Drunk-Dazed", artist: "ENHYPEN", year: 2021, term: "ENHYPEN Drunk Dazed" },
            { title: "ASAP", artist: "STAYC", year: 2021, term: "STAYC ASAP" },
            { title: "Next Level", artist: "aespa", year: 2021, term: "aespa Next Level" },
            { title: "Love Dive", artist: "IVE", year: 2022, term: "IVE Love Dive" },
            { title: "TOMBOY", artist: "(G)I-DLE", year: 2022, term: "GIDLE TOMBOY" },
            { title: "ANTIFRAGILE", artist: "LE SSERAFIM", year: 2022, term: "LE SSERAFIM ANTIFRAGILE" },
            { title: "Hype Boy", artist: "NewJeans", year: 2022, term: "NewJeans Hype Boy" },
            { title: "Super", artist: "SEVENTEEN", year: 2023, term: "SEVENTEEN Super" },
            { title: "Queencard", artist: "(G)I-DLE", year: 2023, term: "GIDLE Queencard" },
            { title: "Supernova", artist: "aespa", year: 2024, term: "aespa Supernova" },
            { title: "Magnetic", artist: "ILLIT", year: 2024, term: "ILLIT Magnetic" },
            { title: "APT.", artist: "ROSÉ & Bruno Mars", year: 2024, term: "ROSE Bruno Mars APT" }
        ];

        const TOTAL_ROUNDS = 15;
        // --- 新增：歌手性別/類型分類 ---
        const ARTIST_GENDER = {
            "Wonder Girls": "F", "SHINee": "M", "BIGBANG": "M", "TVXQ!": "M",
            "Super Junior": "M", "Miss A": "F", "2NE1": "F", "T-ARA": "F",
            "INFINITE": "M", "f(x)": "F", "EXO": "M", "Apink": "F", "SISTAR": "F",
            "EXID": "F", "GFRIEND": "F", "2PM": "M", "TWICE": "F", "SEVENTEEN": "M",
            "BTS": "M", "BTOB": "M", "DAY6": "M", "iKON": "M", "BLACKPINK": "F",
            "TXT": "M", "Red Velvet": "F", "Stray Kids": "M", "ITZY": "F",
            "ENHYPEN": "M", "STAYC": "F", "aespa": "F", "IVE": "F", "(G)I-DLE": "F",
            "LE SSERAFIM": "F", "NewJeans": "F", "ILLIT": "F", "ROSÉ & Bruno Mars": "F" // 歸類為 F 以配合 Rosé 女聲混淆
        };

        // --- 主程式 App ---
        function App() {
            const [gameState, setGameState] = useState('welcome'); 
            const [level, setLevel] = useState(1);
            
            const [playlist, setPlaylist] = useState([]);
            const [currentRound, setCurrentRound] = useState(0);
            const [score, setScore] = useState(0);
            
            const [currentTrackData, setCurrentTrackData] = useState(null);
            const [isPlaying, setIsPlaying] = useState(false);
            const [options, setOptions] = useState([]); 
            const [selectedAnswer, setSelectedAnswer] = useState(null);
            const [errorMsg, setErrorMsg] = useState('');
            const [needsInteraction, setNeedsInteraction] = useState(false); // 提示使用者點擊播放

            const [trivia, setTrivia] = useState(null);
            const [isLoadingTrivia, setIsLoadingTrivia] = useState(false);
            const [birthdayMessage, setBirthdayMessage] = useState(null);
            const [isLoadingMessage, setIsLoadingMessage] = useState(false);

            const apiKey = "AIzaSyC34syJM7eiFh01vCJNwL82edz87adc1Cw"; // Gemini API 密鑰

            const audioRef = useRef(null);
            const bgMusicRef = useRef(null);

            // 確保所有 URL 都是 HTTPS (避免 GitHub Pages 阻擋 mixed content)
            const ensureHttps = (url) => {
                if (!url) return '';
                return url.replace('http://', 'https://');
            };

            const shuffleArray = (array) => {
                const shuffled = [...array];
                for (let i = shuffled.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [shuffled[i], shuffled[j]] = [shuffled[j], shuffled[i]];
                }
                return shuffled;
            };

         const generateOptions = (correctItem, type) => {
                let newOptions = [correctItem];
                if (type === 'year') {
                    let validWrongYears = [];
                    for (let y = 2005; y <= 2025; y++) {
                        if (Math.abs(y - correctItem) >= 2) validWrongYears.push(y);
                    }
                    validWrongYears = shuffleArray(validWrongYears).slice(0, 3);
                    newOptions.push(...validWrongYears);
                } else {
                    // --- 更新：根據性別過濾選項 ---
                    // 1. 先搵出正確答案係男 (M) 定女 (F)
                    const targetGender = ARTIST_GENDER[correctItem] || "F"; 
                    
                    // 2. 喺字典入面，淨係挑選同性別嘅歌手，並排除正確答案
                    const sameGenderArtists = Object.keys(ARTIST_GENDER).filter(a => ARTIST_GENDER[a] === targetGender && a !== correctItem);
                    
                    // 3. 隨機打亂，然後抽 3 個出嚟做錯誤選項
                    const otherArtists = shuffleArray(sameGenderArtists);
                    newOptions = [correctItem, ...otherArtists.slice(0, 3)];
                }
                return shuffleArray(newOptions); 
            };

            const handleOpenGift = async () => {
                setGameState('menu');
                try {
                    const response = await fetch(`https://itunes.apple.com/search?term=Happy+Birthday+To+You+English&limit=1&entity=song`);
                    const data = await response.json();
                    if (data.results && data.results.length > 0) {
                        const audioUrl = ensureHttps(data.results[0].previewUrl);
                        if (!bgMusicRef.current) bgMusicRef.current = new Audio(audioUrl);
                        bgMusicRef.current.volume = 0.6;
                        // 嘗試播放，如果被阻擋只在 console 提示，不影響遊戲進行
                        bgMusicRef.current.play().catch(e => console.log("Autoplay blocked for birthday song:", e));
                    }
                } catch (err) { console.error("無法載入生日歌", err); }
            };

            const handleStartLevelSelect = () => {
                if (bgMusicRef.current) {
                    bgMusicRef.current.pause();
                    bgMusicRef.current.currentTime = 0;
                }
                setGameState('level-select');
            };

            const startGame = (selectedLevel) => {
                setLevel(selectedLevel);
                const shuffledSongs = shuffleArray(KPOP_DATABASE).slice(0, TOTAL_ROUNDS + 5); 
                setPlaylist(shuffledSongs);
                setCurrentRound(0);
                setScore(0);
                setBirthdayMessage(null);
                loadNextSong(0, shuffledSongs, selectedLevel);
            };

            const loadNextSong = async (roundIndex, currentPlaylist, currentLevel) => {
                if (roundIndex >= TOTAL_ROUNDS) return setGameState('end');
                setGameState('loading');
                setErrorMsg('');
                setCurrentTrackData(null);
                setIsPlaying(false);
                setSelectedAnswer(null);
                setTrivia(null);
                setNeedsInteraction(false);
                
                const songMeta = currentPlaylist[roundIndex];

                try {
                    const response = await fetch(`https://itunes.apple.com/search?term=${encodeURIComponent(songMeta.term)}&limit=1&entity=song`);
                    const data = await response.json();

                    if (data.results && data.results.length > 0 && data.results[0].previewUrl) {
                        const track = data.results[0];
                        const trackData = {
                            ...songMeta,
                            previewUrl: ensureHttps(track.previewUrl),
                            artworkUrl: ensureHttps(track.artworkUrl100).replace('100x100bb', '300x300bb')
                        };
                        setCurrentTrackData(trackData);
                        const targetAnswer = currentLevel === 1 ? trackData.year : trackData.artist;
                        setOptions(generateOptions(targetAnswer, currentLevel === 1 ? 'year' : 'artist'));
                        setGameState('playing');
                        setCurrentRound(roundIndex + 1);
                    } else {
                        // 如果這首歌沒有試聽，自動跳下一首
                        console.warn(`No preview found for: ${songMeta.term}`);
                        loadNextSong(roundIndex + 1, currentPlaylist, currentLevel); 
                    }
                } catch (err) {
                    console.error("iTunes API error:", err);
                    setErrorMsg("載入歌曲時發生錯誤，正在嘗試下一首...");
                    setTimeout(() => loadNextSong(roundIndex + 1, currentPlaylist, currentLevel), 2000);
                }
            };

            const togglePlay = () => {
                if (audioRef.current) {
                    if (isPlaying) {
                        audioRef.current.pause();
                        setIsPlaying(false);
                    } else {
                        audioRef.current.play()
                            .then(() => {
                                setIsPlaying(true);
                                setNeedsInteraction(false);
                            })
                            .catch((e) => {
                                console.log("Play failed:", e);
                                setNeedsInteraction(true);
                            });
                    }
                }
            };

            useEffect(() => {
                if (gameState === 'playing' && audioRef.current && currentTrackData) {
                    audioRef.current.play()
                        .then(() => {
                            setIsPlaying(true);
                            setNeedsInteraction(false);
                        })
                        .catch((e) => {
                            console.log("Autoplay prevented:", e);
                            setIsPlaying(false);
                            setNeedsInteraction(true); // 顯示提示讓使用者點擊
                        });
                }
            }, [gameState, currentTrackData]);

            const handleGuess = (answer) => {
                if (gameState !== 'playing') return;
                setSelectedAnswer(answer);
                if (audioRef.current) {
                    audioRef.current.pause();
                    setIsPlaying(false);
                }
                const correctAnswer = level === 1 ? currentTrackData.year : currentTrackData.artist;
                if (answer === correctAnswer) setScore(prev => prev + 10);
                setGameState('result');
            };

            const handleNextRound = () => {
                loadNextSong(currentRound, playlist, level);
            };

            // 呼叫 Gemini API 獲取冷知識
            const fetchTrivia = async () => {
                if (!currentTrackData) return;
                setIsLoadingTrivia(true);
                setTrivia(null);
                if (!apiKey) {
                    setTimeout(() => {
                        setTrivia("這首歌太經典了！(註：請在本地環境或支援平台設定 Gemini API Key 以獲取 AI 內容)");
                        setIsLoadingTrivia(false);
                    }, 1000);
                    return;
                }
                try {
                    const prompt = `介紹一下K-pop歌曲 ${currentTrackData.title} (由 ${currentTrackData.artist} 演唱)。請提供一個簡短、有趣且令人驚訝的冷知識或幕後故事。字數約50-80字，語氣要活潑幽默，適合生日派對的氣氛。使用繁體中文。`;
                    const response = await fetch(`https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-preview-09-2025:generateContent?key=${apiKey}`, {
                        method: 'POST',
                        headers: { 'Content-Type': 'application/json' },
                        body: JSON.stringify({ contents: [{ parts: [{ text: prompt }] }] })
                    });
                    const data = await response.json();
                    const text = data.candidates?.[0]?.content?.parts?.[0]?.text || "哎呀，AI 突然腦袋空白，想不出這首歌的冷知識！";
                    setTrivia(text);
                } catch (error) {
                    setTrivia("AI 連線失敗，無法取得冷知識 😢");
                } finally {
                    setIsLoadingTrivia(false);
                }
            };

            // 呼叫 Gemini API 獲取專屬生日祝福
            const fetchBirthdayMessage = async () => {
                setIsLoadingMessage(true);
                setBirthdayMessage(null);
                if (!apiKey) {
                    setTimeout(() => {
                        setBirthdayMessage("生日快樂！願你每天都像 K-pop 打歌舞台一樣閃耀！(註：請在本地環境或支援平台設定 Gemini API Key 以獲取 AI 內容)");
                        setIsLoadingMessage(false);
                    }, 1000);
                    return;
                }
                try {
                    const performance = score >= 120 ? "完美！K-pop 大神" : score >= 80 ? "不錯！資深粉絲" : "需要再加油";
                    const prompt = `今天是Elaine的生日！她剛剛玩了一個K-pop猜歌遊戲（總分150分）。她的得分是 ${score} 分，表現評級是「${performance}」。請以K-pop偶像或粉絲後援會會長的角度，為她寫一段專屬的生日祝福。如果有高分，請大力稱讚她的K-pop知識；如果分數不高，請幽默地吐槽並鼓勵她多聽歌。字數約80-100字，語氣要熱情、浮誇、有趣。使用繁體中文。結尾加上「✨」。`;
                    const response = await fetch(`https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-preview-09-2025:generateContent?key=${apiKey}`, {
                        method: 'POST',
                        headers: { 'Content-Type': 'application/json' },
                        body: JSON.stringify({ contents: [{ parts: [{ text: prompt }] }] })
                    });
                    const data = await response.json();
                    const text = data.candidates?.[0]?.content?.parts?.[0]?.text || "生日快樂！願你每天都像 K-pop 打歌舞台一樣閃耀！";
                    setBirthdayMessage(text);
                } catch (error) {
                    setBirthdayMessage("生日快樂！(AI 祝福連線中斷 😢)");
                } finally {
                    setIsLoadingMessage(false);
                }
            };

            return (
                <div className="min-h-screen bg-slate-950 text-slate-100 font-sans selection:bg-pink-500 selection:text-white flex flex-col items-center overflow-x-hidden">
                    <header className="w-full p-4 flex justify-center items-center border-b border-pink-900/50 bg-slate-900/80 backdrop-blur-md shadow-lg sticky top-0 z-50">
                        <div className="flex items-center gap-3 text-pink-400">
                        <Cake className="w-6 h-6 animate-bounce" />
                        <h1 className="text-xl md:text-2xl font-black tracking-widest bg-gradient-to-r from-pink-400 to-purple-500 bg-clip-text text-transparent drop-shadow-sm">
                            歌迷序會：Elaine生日特別版
                        </h1>
                        <Heart className="w-5 h-5 animate-pulse" fill="currentColor" />
                        </div>
                    </header>

                    <main className="flex-1 w-full max-w-2xl p-6 flex flex-col justify-center items-center relative z-10">
                        
                        {gameState === 'welcome' && (
                        <div className="text-center space-y-10 animate-[pulse_2s_ease-in-out_infinite] flex flex-col items-center justify-center h-full mt-20">
                            <h2 className="text-3xl md:text-4xl font-black text-white tracking-widest mb-4">
                            有一份專屬禮物等緊妳 💌
                            </h2>
                            <button onClick={handleOpenGift} className="group relative w-48 h-48 flex items-center justify-center transition-transform hover:scale-110 active:scale-95">
                            <div className="absolute inset-0 bg-pink-500 rounded-full blur-[50px] opacity-40 group-hover:opacity-70 transition-opacity"></div>
                            <div className="relative z-10 bg-slate-800 border-4 border-pink-500 rounded-3xl p-8 shadow-[0_0_40px_rgba(236,72,153,0.5)] transform transition-transform group-hover:rotate-12">
                                <Gift className="w-24 h-24 text-pink-400" />
                            </div>
                            </button>
                            <p className="text-pink-400 font-bold text-xl tracking-widest">點擊打開禮物！</p>
                        </div>
                        )}

                        {gameState === 'menu' && (
                        <div className="text-center space-y-8 mt-10">
                            <div className="relative w-56 h-56 mx-auto mb-8">
                            <div className="absolute inset-0 bg-pink-500 rounded-full blur-[60px] opacity-30 animate-pulse"></div>
                            <div className="w-full h-full rounded-full border-[6px] border-slate-800 bg-slate-900 flex flex-col items-center justify-center shadow-[0_0_50px_rgba(236,72,153,0.2)] relative z-10 overflow-hidden">
                                <Music className="w-24 h-24 text-pink-400 mb-2" />
                                <span className="text-pink-500 font-black tracking-widest uppercase text-sm">K-POP Edition</span>
                            </div>
                            </div>
                            <div className="space-y-4">
                            <h2 className="text-4xl md:text-5xl font-black tracking-tight text-white drop-shadow-[0_0_15px_rgba(236,72,153,0.5)]">
                                Happy Birthday <br/> <span className="text-pink-400">Elaine! 🎉</span>
                            </h2>
                            <p className="text-slate-400 text-lg max-w-md mx-auto leading-relaxed">
                                專為妳準備的 K-pop 音樂挑戰！聆聽 30 秒的韓流金曲片段，挑戰妳的粉絲雷達。
                            </p>
                            </div>
                            <button onClick={handleStartLevelSelect} className="mt-8 bg-gradient-to-r from-pink-500 to-purple-600 hover:from-pink-400 hover:to-purple-500 text-white font-bold text-xl py-4 px-14 rounded-full transition-all hover:scale-110 shadow-[0_0_30px_rgba(236,72,153,0.5)] active:scale-95">
                            進入遊戲
                            </button>
                        </div>
                        )}

                        {gameState === 'level-select' && (
                        <div className="w-full text-center space-y-8 max-w-lg mt-10">
                            <h2 className="text-3xl font-black text-white mb-8">選擇挑戰模式</h2>
                            <div className="grid gap-6">
                            <button onClick={() => startGame(1)} className="group relative flex flex-col items-center p-8 bg-slate-800/50 hover:bg-slate-800 border-2 border-slate-700 hover:border-pink-500 rounded-3xl transition-all hover:-translate-y-2 hover:shadow-[0_10px_30px_rgba(236,72,153,0.2)] overflow-hidden">
                                <div className="bg-slate-900 text-pink-400 font-black px-4 py-1 rounded-full text-sm tracking-widest mb-4 border border-pink-900/50 shadow-inner">LEVEL 1</div>
                                <h3 className="text-2xl font-bold text-white mb-2">時光機挑戰 (估年份)</h3>
                                <p className="text-slate-400 text-sm">選項相差至少 2 年，考驗你對 K-pop 世代的直覺！</p>
                            </button>
                            <button onClick={() => startGame(2)} className="group relative flex flex-col items-center p-8 bg-slate-800/50 hover:bg-slate-800 border-2 border-slate-700 hover:border-purple-500 rounded-3xl transition-all hover:-translate-y-2 hover:shadow-[0_10px_30px_rgba(168,85,247,0.2)] overflow-hidden">
                                <div className="bg-slate-900 text-purple-400 font-black px-4 py-1 rounded-full text-sm tracking-widest mb-4 border border-purple-900/50 shadow-inner">LEVEL 2 (盲猜模式)</div>
                                <h3 className="text-2xl font-bold text-white mb-2">粉絲雷達 (估歌手)</h3>
                                <p className="text-slate-400 text-sm">播放時將完全隱藏專輯封面，純憑實力聽聲辨人！</p>
                            </button>
                            </div>
                            <button onClick={() => setGameState('menu')} className="mt-8 text-slate-500 hover:text-white underline underline-offset-4">返回主選單</button>
                        </div>
                        )}

                        {gameState === 'loading' && (
                        <div className="flex flex-col items-center justify-center space-y-6 mt-20">
                            <div className="relative w-16 h-16">
                            <div className="absolute inset-0 border-4 border-slate-800 rounded-full"></div>
                            <div className="absolute inset-0 border-4 border-pink-500 rounded-full border-t-transparent animate-spin"></div>
                            </div>
                            <p className="text-slate-400 text-lg animate-pulse tracking-widest">載入專屬金曲中...</p>
                            {errorMsg && <p className="text-red-400 text-sm mt-2">{errorMsg}</p>}
                        </div>
                        )}

                        {(gameState === 'playing' || gameState === 'result') && currentTrackData && (
                        <div className="w-full flex flex-col items-center max-w-md">
                            <div className="w-full flex justify-between items-center mb-8 text-sm font-bold text-slate-300">
                            <span className="bg-slate-800/80 border border-slate-700 px-5 py-2 rounded-full shadow-inner tracking-widest">
                                Round <span className="text-pink-400">{currentRound}</span> / {TOTAL_ROUNDS}
                            </span>
                            <span className="bg-slate-800/80 border border-slate-700 px-5 py-2 rounded-full flex items-center gap-2 shadow-inner tracking-widest">
                                <Star className="w-4 h-4 text-yellow-400" fill="currentColor" />
                                Score: <span className="text-yellow-400">{score}</span>
                            </span>
                            </div>

                            <div className="relative w-72 h-72 mb-6">
                            {isPlaying && <div className="absolute inset-0 bg-pink-500 rounded-full blur-3xl opacity-20 animate-pulse"></div>}
                            <div className="relative w-full h-full rounded-full border-[8px] border-slate-800 shadow-2xl overflow-hidden bg-slate-900 flex items-center justify-center group cursor-pointer transition-transform hover:scale-105" onClick={togglePlay}>
                                {gameState === 'playing' ? (
                                <div className={`w-full h-full rounded-full border-[20px] border-black bg-slate-900 flex items-center justify-center ${isPlaying ? 'animate-[spin_4s_linear_infinite]' : ''} relative`}>
                                    <div className="absolute inset-0 rounded-full border border-white/5 m-2"></div>
                                    <div className="absolute inset-0 rounded-full border border-white/5 m-6"></div>
                                    <div className="absolute inset-0 rounded-full border border-white/5 m-10"></div>
                                    <div className="w-24 h-24 rounded-full border-4 border-pink-900 flex items-center justify-center relative z-10 overflow-hidden" 
                                        style={level === 1 
                                        ? { backgroundImage: `url(${currentTrackData.artworkUrl})`, backgroundSize: 'cover', backgroundPosition: 'center', filter: 'blur(4px) brightness(0.6)' }
                                        : { background: 'linear-gradient(135deg, #be185d, #4c1d95)' }
                                        }>
                                    {level === 2 && <HelpCircle className="w-10 h-10 text-white/50 absolute" />}
                                    <div className="w-6 h-6 bg-slate-200 rounded-full shadow-[inset_0_3px_5px_rgba(0,0,0,0.5)] z-20"></div>
                                    </div>
                                </div>
                                ) : (
                                <img src={currentTrackData.artworkUrl} className="w-full h-full object-cover" />
                                )}
                                {gameState === 'playing' && (
                                <div className={`absolute inset-0 flex items-center justify-center bg-black/50 transition-opacity backdrop-blur-sm z-30 rounded-full ${needsInteraction ? 'opacity-100' : 'opacity-0 group-hover:opacity-100'}`}>
                                    {isPlaying ? <Pause className="w-16 h-16 text-white" /> : <Play className="w-16 h-16 text-white ml-2" />}
                                </div>
                                )}
                            </div>
                            </div>
                            
                            {needsInteraction && gameState === 'playing' && (
                                <p className="text-pink-400 font-bold mb-4 animate-pulse">👉 點擊唱片播放音樂 👈</p>
                            )}
                            {!needsInteraction && <div className="h-4 mb-4"></div>}

                            <audio ref={audioRef} src={currentTrackData.previewUrl} onEnded={() => setIsPlaying(false)} />

                            {gameState === 'playing' ? (
                            <div className="w-full">
                                <h3 className="text-2xl font-black text-center mb-6 text-white tracking-wide">
                                {level === 1 ? "這首歌大約是哪一年發行的？" : "這首歌的歌手是誰？"}
                                </h3>
                                <div className="grid grid-cols-2 gap-4 w-full">
                                {options.map((option, index) => (
                                    <button key={index} onClick={() => handleGuess(option)} className="py-5 px-4 rounded-2xl font-bold text-xl bg-slate-800/80 hover:bg-gradient-to-r hover:from-pink-500 hover:to-purple-600 text-slate-200 hover:text-white border border-slate-700 hover:border-transparent transition-all active:scale-95 shadow-lg flex items-center justify-center text-center break-words leading-tight min-h-[5rem]">
                                    {option}
                                    </button>
                                ))}
                                </div>
                            </div>
                            ) : (
                            <div className="w-full text-center p-8 bg-slate-800/80 border border-slate-700 rounded-3xl shadow-2xl relative overflow-hidden">
                                <div className={`absolute -top-20 -right-20 w-40 h-40 rounded-full blur-3xl opacity-20 ${selectedAnswer === (level === 1 ? currentTrackData.year : currentTrackData.artist) ? 'bg-green-500' : 'bg-red-500'}`}></div>
                                {selectedAnswer === (level === 1 ? currentTrackData.year : currentTrackData.artist) ? (
                                <h3 className="text-3xl font-black text-green-400 mb-2 drop-shadow-[0_0_10px_rgba(74,222,128,0.5)]">🎉 答對了！</h3>
                                ) : (
                                <h3 className="text-3xl font-black text-red-400 mb-2 drop-shadow-[0_0_10px_rgba(248,113,113,0.5)]">❌ 差一點點！</h3>
                                )}
                                <div className="my-6 space-y-1">
                                <p className="text-slate-400 text-sm uppercase tracking-widest font-bold">正確答案</p>
                                <p className="text-4xl font-black text-white bg-slate-900/50 inline-block px-6 py-2 rounded-xl border border-slate-700">
                                    {level === 1 ? currentTrackData.year : currentTrackData.artist}
                                </p>
                                </div>
                                <div className="my-6">
                                <p className="text-2xl font-black text-pink-400 mb-1">{currentTrackData.title}</p>
                                <p className="text-slate-300 font-medium tracking-wide">
                                    {level === 1 ? currentTrackData.artist : currentTrackData.year}
                                </p>
                                </div>

                                <div className="mb-8 w-full">
                                {!trivia && !isLoadingTrivia ? (
                                    <button onClick={fetchTrivia} className="w-full flex items-center justify-center gap-2 py-3 px-4 rounded-xl border border-pink-500/50 bg-pink-500/10 text-pink-300 font-bold hover:bg-pink-500/20 transition-all text-sm shadow-md">
                                    <Sparkles className="w-4 h-4" /> ✨ 獲取 AI 幕後冷知識
                                    </button>
                                ) : isLoadingTrivia ? (
                                    <div className="w-full p-4 rounded-xl bg-slate-900/50 border border-slate-700 flex items-center justify-center gap-2 text-pink-400">
                                    <Loader2 className="w-5 h-5 animate-spin" />
                                    <span className="text-sm font-medium animate-pulse">AI 正在回想這首歌的八卦...</span>
                                    </div>
                                ) : (
                                    <div className="w-full p-5 rounded-xl bg-slate-900 border border-pink-500/30 text-left relative animate-in zoom-in duration-300">
                                    <Sparkles className="w-5 h-5 text-pink-400 absolute top-3 right-3 opacity-50" />
                                    <p className="text-xs font-bold text-pink-400 mb-2 uppercase tracking-widest">AI 冷知識</p>
                                    <p className="text-sm text-slate-300 leading-relaxed">{trivia}</p>
                                    </div>
                                )}
                                </div>

                                <button onClick={handleNextRound} className="w-full bg-white hover:bg-slate-200 text-slate-900 font-black py-4 rounded-xl flex items-center justify-center gap-2 transition-all active:scale-95 shadow-lg text-lg tracking-widest uppercase">
                                <SkipForward className="w-6 h-6" />
                                {currentRound < TOTAL_ROUNDS ? "下一首 (Next)" : "查看總結 (Result)"}
                                </button>
                            </div>
                            )}
                        </div>
                        )}

                        {gameState === 'end' && (
                        <div className="text-center space-y-8 w-full max-w-md mt-10">
                            <div className="w-36 h-36 bg-slate-900 rounded-full flex items-center justify-center mx-auto border-4 border-pink-500 shadow-[0_0_50px_rgba(236,72,153,0.4)] relative">
                            <Trophy className="w-16 h-16 text-yellow-400 absolute z-10" />
                            <div className="absolute inset-0 border-4 border-dashed border-pink-500/50 rounded-full animate-[spin_10s_linear_infinite]"></div>
                            </div>
                            <div>
                            <h2 className="text-4xl font-black text-white mb-3 tracking-wider">遊戲完成！</h2>
                            <p className="text-pink-400 font-bold tracking-widest">ELAINE'S K-POP JOURNEY</p>
                            </div>
                            <div className="bg-slate-900 border border-slate-700 rounded-3xl p-8 shadow-2xl relative overflow-hidden">
                            <div className="absolute top-0 left-0 w-full h-1 bg-gradient-to-r from-pink-500 to-purple-500"></div>
                            <p className="text-sm text-slate-400 uppercase tracking-widest mb-4 font-bold">Level {level} 最終得分</p>
                            <div className="flex items-end justify-center gap-2 mb-6">
                                <span className="text-8xl font-black text-transparent bg-clip-text bg-gradient-to-br from-yellow-300 to-yellow-600 drop-shadow-lg">{score}</span>
                                <span className="text-2xl font-bold text-slate-500 mb-3">/ {TOTAL_ROUNDS * 10}</span>
                            </div>
                            <div className="pt-6 border-t border-slate-800">
                                <p className="text-xl font-black text-white mb-6">
                                {score >= 120 ? "💖 完美！真正的 K-pop 達人！" : 
                                score >= 80 ? "✨ 很不錯！韓流骨灰級粉絲！" : 
                                "😅 當作溫習！再挑戰一次吧！"}
                                </p>
                                
                                <div className="w-full">
                                {!birthdayMessage && !isLoadingMessage ? (
                                    <button onClick={fetchBirthdayMessage} className="w-full group relative flex items-center justify-center py-4 rounded-2xl bg-slate-900 border border-purple-500/50 overflow-hidden transition-all hover:scale-105 active:scale-95 shadow-lg">
                                    <div className="absolute inset-0 bg-gradient-to-r from-pink-500/20 to-purple-500/20 group-hover:opacity-100 opacity-50 transition-opacity"></div>
                                    <div className="relative z-10 flex items-center gap-2 text-purple-300 font-bold text-lg">
                                        <Sparkles className="w-5 h-5" /> ✨ 領取 AI 專屬生日評語
                                    </div>
                                    </button>
                                ) : isLoadingMessage ? (
                                    <div className="w-full p-6 rounded-2xl bg-slate-900 border border-purple-500/50 flex flex-col items-center justify-center gap-3 text-purple-400">
                                    <Loader2 className="w-8 h-8 animate-spin" />
                                    <span className="text-sm font-bold tracking-widest animate-pulse">AI 偶像正在手寫卡片...</span>
                                    </div>
                                ) : (
                                    <div className="w-full p-6 rounded-2xl bg-gradient-to-br from-slate-900 to-slate-800 border border-pink-500/50 shadow-[0_0_30px_rgba(236,72,153,0.2)] text-left relative animate-in zoom-in duration-500">
                                    <Sparkles className="w-8 h-8 text-pink-400 absolute top-4 right-4 opacity-20" />
                                    <p className="text-sm font-black text-transparent bg-clip-text bg-gradient-to-r from-pink-400 to-purple-400 mb-3 tracking-widest">💌 來自 AI 的生日祝福</p>
                                    <p className="text-base text-slate-200 leading-relaxed font-medium">{birthdayMessage}</p>
                                    </div>
                                )}
                                </div>
                            </div>
                            </div>
                            <div className="flex gap-4">
                            <button onClick={() => startGame(level)} className="flex-1 bg-slate-800 hover:bg-slate-700 border border-slate-600 text-white font-bold text-lg py-4 rounded-2xl transition-all active:scale-95">重玩此難度</button>
                            <button onClick={() => setGameState('level-select')} className="flex-1 bg-gradient-to-r from-pink-500 to-purple-600 hover:from-pink-400 hover:to-purple-500 text-white font-bold text-lg py-4 rounded-2xl transition-all active:scale-95 shadow-[0_0_20px_rgba(236,72,153,0.3)]">切換 Level</button>
                            </div>
                        </div>
                        )}
                    </main>

                    <footer className="w-full py-6 text-center text-slate-500 text-sm font-medium tracking-widest relative z-10">
                        Created with ❤️ by <span className="text-pink-500 font-bold">Ching</span>
                    </footer>
                </div>
            );
        }

        const root = ReactDOM.createRoot(document.getElementById('root'));
        root.render(<App />);
    </script>
</body>
</html>
