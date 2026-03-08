import React, { useState, useEffect, useRef } from 'react';
import { Play, Pause, SkipForward, Music, Trophy, Star, Heart, Cake, Gift, HelpCircle } from 'lucide-react';

// K-pop 專屬歌曲資料庫 (已大幅擴充，加入非最熱門但粉絲必知的金曲)
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

const TOTAL_ROUNDS = 15; // 增加到 15 題

export default function App() {
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

  const audioRef = useRef(null);
  const bgMusicRef = useRef(null);

  // 洗牌函數
  const shuffleArray = (array) => {
    const shuffled = [...array];
    for (let i = shuffled.length - 1; i > 0; i--) {
      const j = Math.floor(Math.random() * (i + 1));
      [shuffled[i], shuffled[j]] = [shuffled[j], shuffled[i]];
    }
    return shuffled;
  };

  // 產生 4 個選項
  const generateOptions = (correctItem, type) => {
    let newOptions = [correctItem];
    
    if (type === 'year') {
      // Level 1: 估年份 (選項之間至少相差 2 年)
      let validWrongYears = [];
      for (let y = 2005; y <= 2025; y++) {
         if (Math.abs(y - correctItem) >= 2) {
            validWrongYears.push(y);
         }
      }
      // 隨機抽取 3 個錯誤年份
      validWrongYears = shuffleArray(validWrongYears).slice(0, 3);
      newOptions.push(...validWrongYears);
    } else {
      // Level 2: 估歌手
      const allArtists = [...new Set(KPOP_DATABASE.map(song => song.artist))];
      const otherArtists = shuffleArray(allArtists.filter(a => a !== correctItem));
      newOptions = [correctItem, ...otherArtists.slice(0, 3)];
    }

    return shuffleArray(newOptions); 
  };

  // 處理拆禮物並播放生日歌
  const handleOpenGift = async () => {
    setGameState('menu');
    try {
      const response = await fetch(`https://itunes.apple.com/search?term=Happy+Birthday+To+You+English&limit=1&entity=song`);
      const data = await response.json();
      if (data.results && data.results.length > 0) {
        if (!bgMusicRef.current) {
          bgMusicRef.current = new Audio(data.results[0].previewUrl);
        }
        bgMusicRef.current.volume = 0.6;
        bgMusicRef.current.play().catch(e => console.log("Autoplay blocked:", e));
      }
    } catch (err) {
      console.error("無法載入生日歌", err);
    }
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
    loadNextSong(0, shuffledSongs, selectedLevel);
  };

  const loadNextSong = async (roundIndex, currentPlaylist, currentLevel) => {
    if (roundIndex >= TOTAL_ROUNDS) {
      setGameState('end');
      return;
    }

    setGameState('loading');
    setErrorMsg('');
    setCurrentTrackData(null);
    setIsPlaying(false);
    setSelectedAnswer(null);
    
    const songMeta = currentPlaylist[roundIndex];

    try {
      const response = await fetch(`https://itunes.apple.com/search?term=${encodeURIComponent(songMeta.term)}&limit=1&entity=song`);
      const data = await response.json();

      if (data.results && data.results.length > 0) {
        const track = data.results[0];
        const trackData = {
          ...songMeta,
          previewUrl: track.previewUrl,
          artworkUrl: track.artworkUrl100.replace('100x100bb', '300x300bb')
        };
        
        setCurrentTrackData(trackData);
        
        const targetAnswer = currentLevel === 1 ? trackData.year : trackData.artist;
        setOptions(generateOptions(targetAnswer, currentLevel === 1 ? 'year' : 'artist'));
        
        setGameState('playing');
        setCurrentRound(roundIndex + 1);
      } else {
        loadNextSong(roundIndex + 1, currentPlaylist, currentLevel); 
      }
    } catch (err) {
      setErrorMsg("網絡連線發生錯誤，正在嘗試下一首...");
      setTimeout(() => loadNextSong(roundIndex + 1, currentPlaylist, currentLevel), 2000);
    }
  };

  const togglePlay = () => {
    if (audioRef.current) {
      if (isPlaying) {
        audioRef.current.pause();
      } else {
        audioRef.current.play().catch(() => console.log("等待使用者互動"));
      }
      setIsPlaying(!isPlaying);
    }
  };

  useEffect(() => {
    if (gameState === 'playing' && audioRef.current && currentTrackData) {
      audioRef.current.play().then(() => {
        setIsPlaying(true);
      }).catch((e) => {
        setIsPlaying(false);
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
    
    if (answer === correctAnswer) {
      setScore(prev => prev + 10);
    }
    
    setGameState('result');
  };

  const handleNextRound = () => {
    loadNextSong(currentRound, playlist, level);
  };

  return (
    <div className="min-h-screen bg-slate-950 text-slate-100 font-sans selection:bg-pink-500 selection:text-white flex flex-col items-center overflow-x-hidden">
      
      {/* 頂部導航列 */}
      <header className="w-full p-4 flex justify-center items-center border-b border-pink-900/50 bg-slate-900/80 backdrop-blur-md shadow-lg sticky top-0 z-50">
        <div className="flex items-center gap-3 text-pink-400">
          <Cake className="w-6 h-6 animate-bounce" />
          <h1 className="text-xl md:text-2xl font-black tracking-widest bg-gradient-to-r from-pink-400 to-purple-500 bg-clip-text text-transparent drop-shadow-sm">
            歌迷序會：Elaine生日特別版
          </h1>
          <Heart className="w-5 h-5 fill-pink-500 animate-pulse" />
        </div>
      </header>

      <main className="flex-1 w-full max-w-2xl p-6 flex flex-col justify-center items-center relative z-10">
        
        {/* === 開場拆禮物畫面 === */}
        {gameState === 'welcome' && (
          <div className="text-center space-y-10 animate-in fade-in zoom-in duration-1000 flex flex-col items-center justify-center h-full">
            <h2 className="text-3xl md:text-4xl font-black text-white tracking-widest mb-4">
              有一份專屬禮物等緊妳 💌
            </h2>
            
            <button 
              onClick={handleOpenGift}
              className="group relative w-48 h-48 flex items-center justify-center transition-transform hover:scale-110 active:scale-95"
            >
              <div className="absolute inset-0 bg-pink-500 rounded-full blur-[50px] opacity-40 group-hover:opacity-70 group-hover:animate-pulse transition-opacity"></div>
              <div className="relative z-10 bg-slate-800 border-4 border-pink-500 rounded-3xl p-8 shadow-[0_0_40px_rgba(236,72,153,0.5)] transform transition-transform group-hover:rotate-12">
                <Gift className="w-24 h-24 text-pink-400" />
              </div>
            </button>
            
            <p className="text-pink-400 font-bold text-xl tracking-widest animate-pulse">
              點擊打開禮物！
            </p>
          </div>
        )}

        {/* === 主選單 === */}
        {gameState === 'menu' && (
          <div className="text-center space-y-8 animate-in fade-in zoom-in duration-700">
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

            <button 
              onClick={handleStartLevelSelect}
              className="mt-8 bg-gradient-to-r from-pink-500 to-purple-600 hover:from-pink-400 hover:to-purple-500 text-white font-bold text-xl py-4 px-14 rounded-full transition-all hover:scale-110 hover:shadow-[0_0_30px_rgba(236,72,153,0.5)] active:scale-95"
            >
              進入遊戲
            </button>
          </div>
        )}

        {/* === 選擇難度 (Level Select) === */}
        {gameState === 'level-select' && (
          <div className="w-full text-center space-y-8 animate-in slide-in-from-right duration-500 max-w-lg">
            <h2 className="text-3xl font-black text-white mb-8">選擇挑戰模式</h2>
            
            <div className="grid gap-6">
              {/* Level 1 Button */}
              <button 
                onClick={() => startGame(1)}
                className="group relative flex flex-col items-center p-8 bg-slate-800/50 hover:bg-slate-800 border-2 border-slate-700 hover:border-pink-500 rounded-3xl transition-all hover:-translate-y-2 hover:shadow-[0_10px_30px_rgba(236,72,153,0.2)] overflow-hidden"
              >
                <div className="absolute inset-0 bg-gradient-to-br from-pink-500/10 to-transparent opacity-0 group-hover:opacity-100 transition-opacity"></div>
                <div className="bg-slate-900 text-pink-400 font-black px-4 py-1 rounded-full text-sm tracking-widest mb-4 border border-pink-900/50 shadow-inner">LEVEL 1</div>
                <h3 className="text-2xl font-bold text-white mb-2">時光機挑戰 (估年份)</h3>
                <p className="text-slate-400 text-sm">選項相差至少 2 年，考驗你對 K-pop 世代的直覺！</p>
              </button>

              {/* Level 2 Button */}
              <button 
                onClick={() => startGame(2)}
                className="group relative flex flex-col items-center p-8 bg-slate-800/50 hover:bg-slate-800 border-2 border-slate-700 hover:border-purple-500 rounded-3xl transition-all hover:-translate-y-2 hover:shadow-[0_10px_30px_rgba(168,85,247,0.2)] overflow-hidden"
              >
                <div className="absolute inset-0 bg-gradient-to-br from-purple-500/10 to-transparent opacity-0 group-hover:opacity-100 transition-opacity"></div>
                <div className="bg-slate-900 text-purple-400 font-black px-4 py-1 rounded-full text-sm tracking-widest mb-4 border border-purple-900/50 shadow-inner">LEVEL 2 (盲猜模式)</div>
                <h3 className="text-2xl font-bold text-white mb-2">粉絲雷達 (估歌手)</h3>
                <p className="text-slate-400 text-sm">播放時將完全隱藏專輯封面，純憑實力聽聲辨人！</p>
              </button>
            </div>
            
            <button onClick={() => setGameState('menu')} className="mt-8 text-slate-500 hover:text-white transition-colors underline underline-offset-4">
              返回主選單
            </button>
          </div>
        )}

        {/* === 載入中 === */}
        {gameState === 'loading' && (
          <div className="flex flex-col items-center justify-center space-y-6">
            <div className="relative w-16 h-16">
               <div className="absolute inset-0 border-4 border-slate-800 rounded-full"></div>
               <div className="absolute inset-0 border-4 border-pink-500 rounded-full border-t-transparent animate-spin"></div>
            </div>
            <p className="text-slate-400 text-lg animate-pulse tracking-widest">載入專屬金曲中...</p>
            {errorMsg && <p className="text-red-400 text-sm mt-2">{errorMsg}</p>}
          </div>
        )}

        {/* === 遊戲進行中 & 結果畫面 === */}
        {(gameState === 'playing' || gameState === 'result') && currentTrackData && (
          <div className="w-full flex flex-col items-center max-w-md">
            
            {/* 頂部資訊列 */}
            <div className="w-full flex justify-between items-center mb-8 text-sm font-bold text-slate-300">
              <span className="bg-slate-800/80 border border-slate-700 px-5 py-2 rounded-full shadow-inner tracking-widest">
                Round <span className="text-pink-400">{currentRound}</span> / {TOTAL_ROUNDS}
              </span>
              <span className="bg-slate-800/80 border border-slate-700 px-5 py-2 rounded-full flex items-center gap-2 shadow-inner tracking-widest">
                <Star className="w-4 h-4 text-yellow-400 fill-yellow-400" />
                Score: <span className="text-yellow-400">{score}</span>
              </span>
            </div>

            {/* 黑膠唱片/專輯封面區 */}
            <div className="relative w-72 h-72 mb-10">
              {isPlaying && <div className="absolute inset-0 bg-pink-500 rounded-full blur-3xl opacity-20 animate-pulse"></div>}
              
              <div className="relative w-full h-full rounded-full border-[8px] border-slate-800 shadow-2xl overflow-hidden bg-slate-900 flex items-center justify-center group cursor-pointer transition-transform hover:scale-105" onClick={togglePlay}>
                {gameState === 'playing' ? (
                  <div className={`w-full h-full rounded-full border-[20px] border-black bg-slate-900 flex items-center justify-center ${isPlaying ? 'animate-[spin_4s_linear_infinite]' : ''} relative`}>
                    <div className="absolute inset-0 rounded-full border border-white/5 m-2"></div>
                    <div className="absolute inset-0 rounded-full border border-white/5 m-6"></div>
                    <div className="absolute inset-0 rounded-full border border-white/5 m-10"></div>
                    
                    {/* Level 1 顯示模糊封面，Level 2 完全隱藏封面以防劇透 */}
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
                  <img src={currentTrackData.artworkUrl} alt="Album Art" className="w-full h-full object-cover animate-in zoom-in duration-500" />
                )}

                {gameState === 'playing' && (
                  <div className="absolute inset-0 flex items-center justify-center bg-black/50 opacity-0 group-hover:opacity-100 transition-opacity backdrop-blur-sm z-30 rounded-full">
                    {isPlaying ? <Pause className="w-16 h-16 text-white" /> : <Play className="w-16 h-16 text-white ml-2" />}
                  </div>
                )}
              </div>
            </div>

            <audio 
              ref={audioRef} 
              src={currentTrackData.previewUrl} 
              onEnded={() => setIsPlaying(false)}
            />

            {/* 遊戲操作區 */}
            {gameState === 'playing' ? (
              <div className="w-full animate-in slide-in-from-bottom-6 duration-500">
                <h3 className="text-2xl font-black text-center mb-6 text-white tracking-wide">
                  {level === 1 ? "這首歌大約是哪一年發行的？" : "這首歌的歌手是誰？"}
                </h3>
                <div className="grid grid-cols-2 gap-4 w-full">
                  {options.map((option, index) => (
                    <button
                      key={index}
                      onClick={() => handleGuess(option)}
                      className="py-5 px-4 rounded-2xl font-bold text-xl bg-slate-800/80 hover:bg-gradient-to-r hover:from-pink-500 hover:to-purple-600 text-slate-200 hover:text-white border border-slate-700 hover:border-transparent transition-all active:scale-95 shadow-lg flex items-center justify-center text-center break-words leading-tight min-h-[5rem]"
                    >
                      {option}
                    </button>
                  ))}
                </div>
              </div>
            ) : (
              /* 結果區塊 */
              <div className="w-full text-center p-8 bg-slate-800/80 border border-slate-700 rounded-3xl animate-in slide-in-from-bottom-8 duration-500 backdrop-blur-md shadow-2xl relative overflow-hidden">
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

                <div className="my-8">
                  <p className="text-2xl font-black text-pink-400 mb-1">{currentTrackData.title}</p>
                  <p className="text-slate-300 font-medium tracking-wide">
                    {level === 1 ? currentTrackData.artist : currentTrackData.year}
                  </p>
                </div>

                <button 
                  onClick={handleNextRound}
                  className="w-full bg-white hover:bg-slate-200 text-slate-900 font-black py-4 rounded-xl flex items-center justify-center gap-2 transition-all active:scale-95 shadow-lg text-lg tracking-widest uppercase"
                >
                  <SkipForward className="w-6 h-6" />
                  {currentRound < TOTAL_ROUNDS ? "下一首 (Next)" : "查看總結 (Result)"}
                </button>
              </div>
            )}
          </div>
        )}

        {/* === 遊戲結束 === */}
        {gameState === 'end' && (
          <div className="text-center space-y-8 animate-in zoom-in duration-700 w-full max-w-md">
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
                <p className="text-xl font-black text-white">
                  {score >= 120 ? "💖 完美！真正的 K-pop 達人！" : 
                   score >= 80 ? "✨ 很不錯！韓流骨灰級粉絲！" : 
                   "😅 當作溫習！再挑戰一次吧！"}
                </p>
              </div>
            </div>

            <div className="flex gap-4">
               <button 
                onClick={() => startGame(level)}
                className="flex-1 bg-slate-800 hover:bg-slate-700 border border-slate-600 text-white font-bold text-lg py-4 rounded-2xl transition-all active:scale-95"
              >
                重玩此難度
              </button>
              <button 
                onClick={() => setGameState('level-select')}
                className="flex-1 bg-gradient-to-r from-pink-500 to-purple-600 hover:from-pink-400 hover:to-purple-500 text-white font-bold text-lg py-4 rounded-2xl transition-all active:scale-95 shadow-[0_0_20px_rgba(236,72,153,0.3)]"
              >
                切換 Level
              </button>
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
