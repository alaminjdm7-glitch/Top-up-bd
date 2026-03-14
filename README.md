<!DOCTYPE html>
<html lang="bn">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
    <title>Orange App 🍊</title>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@400;500;600;700&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.2/css/all.min.css" />
    <style>
        /* CSS Styles (Same as original, omitted for brevity but included in logic) */
        :root { --font-family: 'Poppins', sans-serif; --primary-color: #E74C3C; --secondary-color: #F39C12; --background-light: #FDFEFE; --surface-light: #FFFFFF; --text-primary-light: #2C3E50; --text-secondary-light: #808B96; --border-light: #EAEDED; --background-dark: #1B2631; --surface-dark: #283747; --text-primary-dark: #FDFEFE; --text-secondary-dark: #ABB2B9; --border-dark: #566573; --success: #27AE60; --danger: #C0392B; --pending: #F39C12; }
        * { margin: 0; padding: 0; box-sizing: border-box; }
        body { font-family: var(--font-family); transition: background-color 0.3s, color 0.3s; -webkit-font-smoothing: antialiased; }
        body.light-theme { background-color: var(--background-light); color: var(--text-primary-light); }
        body.dark-theme { background-color: var(--background-dark); color: var(--text-primary-dark); }
        #app { max-width: 500px; margin: 0 auto; padding-bottom: 80px; }
        .page { display: none; padding: 15px; }
        .page.active { display: block; animation: fadeIn 0.4s ease-in-out; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(15px); } to { opacity: 1; transform: translateY(0); } }
        
        .profile-header { padding: 25px 15px; border-bottom: 1px solid; display: flex; flex-direction: column; align-items: center; text-align: center; }
        .light-theme .profile-header { background-color: var(--surface-light); border-bottom-color: var(--border-light); }
        .dark-theme .profile-header { background-color: var(--surface-dark); border-bottom-color: var(--border-dark); }
        #user-photo { width: 80px; height: 80px; border-radius: 50%; border: 4px solid var(--primary-color); object-fit: cover; margin-bottom: 10px; }
        #user-name { font-size: 1.4rem; font-weight: 700; margin-bottom: 5px; }
        #user-balance-container { font-size: 1rem; font-weight: 500; }
        #user-balance { font-size: 1.1rem; font-weight: 700; color: var(--primary-color); }

        .stats-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 15px; margin: 20px 0; }
        .stat-card { padding: 15px; border-radius: 12px; text-align: center; border: 1px solid; }
        .light-theme .stat-card { background-color: var(--surface-light); border-color: var(--border-light); }
        .dark-theme .stat-card { background-color: var(--surface-dark); border-color: var(--border-dark); }
        .stat-card h3 { font-size: 0.85rem; font-weight: 500; margin-bottom: 8px; text-transform: uppercase; }
        .light-theme .stat-card h3 { color: var(--text-secondary-light); } .dark-theme .stat-card h3 { color: var(--text-secondary-dark); }
        .stat-card p { font-size: 1.4rem; font-weight: 600; color: var(--primary-color); }
        
        .chart-container, .earning-wallet-card { padding: 20px; border-radius: 12px; margin-top: 20px; border: 1px solid; }
        .light-theme .chart-container, .light-theme .earning-wallet-card { background-color: var(--surface-light); border-color: var(--border-light); }
        .dark-theme .chart-container, .dark-theme .earning-wallet-card { background-color: var(--surface-dark); border-color: var(--border-dark); }
        .chart { display: flex; justify-content: space-around; align-items: flex-end; height: 150px; border-left: 1px solid; border-bottom: 1px solid; padding-top: 10px; }
        .light-theme .chart { border-color: var(--border-light); } .dark-theme .chart { border-color: var(--border-dark); }
        .bar { width: 12%; background: linear-gradient(to top, var(--primary-color), var(--secondary-color)); border-radius: 5px 5px 0 0; position: relative; cursor: pointer; }
        .bar .tooltip { position: absolute; top: -30px; left: 50%; transform: translateX(-50%); background: var(--secondary); color: white; padding: 3px 7px; border-radius: 5px; font-size: 0.8rem; white-space: nowrap; opacity: 0; transition: opacity 0.2s; pointer-events: none; }
        .bar:hover .tooltip { opacity: 1; }
        .chart-labels { display: flex; justify-content: space-around; font-size: 0.75rem; margin-top: 5px; }

        .history-item { display: flex; justify-content: space-between; align-items: center; padding: 15px; border-radius: 10px; margin-bottom: 10px; }
        .light-theme .history-item { background-color: var(--surface-light); border: 1px solid var(--border-light); }
        .dark-theme .history-item { background-color: var(--surface-dark); border: 1px solid var(--border-dark); }
        .history-details p { margin: 0; } .history-details p:first-child { font-weight: 600; }
        .history-status { padding: 5px 10px; border-radius: 20px; font-size: 0.8rem; font-weight: 600; color: white; text-transform: capitalize; }
        .status-completed { background-color: var(--success); } .status-pending { background-color: var(--pending); } .status-rejected { background-color: var(--danger); }

        .progress-container { margin-bottom: 20px; }
        .progress-info { display: flex; justify-content: space-between; margin-bottom: 5px; font-size: 0.9rem; font-weight: 500; }
        .progress-bar-bg { width: 100%; height: 10px; border-radius: 5px; }
        .light-theme .progress-bar-bg { background-color: var(--border-light); }
        .dark-theme .progress-bar-bg { background-color: var(--border-dark); }
        .progress-bar-fg { height: 100%; border-radius: 5px; background: linear-gradient(90deg, var(--primary-color), var(--secondary-color)); transition: width 0.3s ease; }
        
        form input, form select, form textarea { transition: all 0.2s ease-in-out; border: 1px solid; }
        .light-theme form input, .light-theme form select { border-color: var(--border-light); background: var(--surface-light); }
        .dark-theme form input, .dark-theme form select { border-color: var(--border-dark); background: var(--surface-dark); color: var(--text-primary-dark); }
        form input:focus, form select:focus, form textarea:focus { outline: none; border-color: var(--primary-color) !important; box-shadow: 0 0 0 3px rgba(231, 76, 60, 0.2); }

        .popup-overlay { position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.6); display: none; justify-content: center; align-items: center; z-index: 10000; }
        .popup-content { padding: 30px; border-radius: 15px; text-align: center; max-width: 80%; }
        .light-theme .popup-content { background-color: var(--surface-light); } .dark-theme .popup-content { background-color: var(--surface-dark); }
        .popup-content h2 { margin-bottom: 15px; color: var(--primary-color); } .popup-content p { margin-bottom: 20px; }
        .popup-close-btn { padding: 10px 25px; border: none; border-radius: 8px; color: #fff; background-color: var(--primary-color); cursor: pointer; }
        .popup-loader .spinner { width: 40px; height: 40px; border-radius: 50%; border: 4px solid; border-color: var(--primary-color) transparent; animation: spin 1s linear infinite; margin: 0 auto 15px;}
        @keyframes spin { to { transform: rotate(360deg); } }
        
        #app-loader { position: fixed; top: 0; left: 0; width: 100%; height: 100%; z-index: 9999; display: flex; flex-direction: column; justify-content: center; align-items: center; }
        .light-theme #app-loader { background-color: var(--background-light); } .dark-theme #app-loader { background-color: var(--background-dark); }
        #app-loader .spinner { width: 50px; height: 50px; border-radius: 50%; border: 5px solid; border-color: var(--primary-color) transparent; animation: spin 1s linear infinite; }
        #loading-progress { margin-top: 15px; font-size: 1.2rem; font-weight: 600; }
        .light-theme #loading-progress { color: var(--text-primary-light); } .dark-theme #loading-progress { color: var(--text-primary-dark); }
        
        .bottom-nav { position: fixed; bottom: 0; left: 0; right: 0; max-width: 500px; margin: 0 auto; display: flex; justify-content: space-around; padding: 10px 0; border-top: 1px solid; z-index: 1000; }
        .light-theme .bottom-nav { background-color: var(--surface-light); border-top-color: var(--border-light); }
        .dark-theme .bottom-nav { background-color: var(--surface-dark); border-top-color: var(--border-dark); }
        .nav-btn { background: none; border: none; cursor: pointer; display: flex; flex-direction: column; align-items: center; font-family: var(--font-family); font-size: 0.75rem; transition: color 0.2s; flex-grow: 1; }
        .light-theme .nav-btn { color: var(--text-secondary-light); } .dark-theme .nav-btn { color: var(--text-secondary-dark); }
        .nav-btn i { font-size: 1.5rem; margin-bottom: 4px; } .nav-btn.active { color: var(--primary-color); }
        
        .task-container { padding: 20px; border-radius: 12px; margin-bottom: 15px; display: flex; align-items: center; gap: 15px; }
        .light-theme .task-container { background-color: var(--surface-light); border: 1px solid var(--border-light); }
        .dark-theme .task-container { background-color: var(--surface-dark); border: 1px solid var(--border-dark); }
        .task-icon-img { width: 40px; height: 40px; border-radius: 8px; object-fit: cover; }
        .task-info h3 { font-size: 1.1rem; font-weight: 600; } .task-info p { font-size: 0.85rem; }
        .action-btn { padding: 10px 20px; font-size: 0.9rem; font-weight: 600; color: #fff; border: none; border-radius: 8px; cursor: pointer; transition: all 0.2s; background: linear-gradient(45deg, var(--primary-color), var(--secondary-color)); margin-left: auto; white-space: nowrap; }
        .action-btn:disabled { background: #808B96; cursor: not-allowed; }
        
        .leaderboard-toggle { display: flex; justify-content: space-around; margin-bottom: 15px; }
        .toggle-btn { padding: 10px 20px; border: none; background: none; font-size: 1rem; cursor: pointer; font-family: var(--font-family); }
        .light-theme .toggle-btn { color: var(--text-secondary-light); } .dark-theme .toggle-btn { color: var(--text-secondary-dark); }
        .toggle-btn.active { font-weight: 700; color: var(--primary-color); border-bottom: 2px solid var(--primary-color); }
        .leaderboard-list { list-style: none; }
        .leaderboard-item { display: flex; align-items: center; padding: 10px; margin-bottom: 8px; border-radius: 8px; }
        .light-theme .leaderboard-item { background-color: var(--surface-light); } .dark-theme .leaderboard-item { background-color: var(--surface-dark); }
        .rank { font-size: 1.2rem; font-weight: 700; color: var(--primary-color); width: 40px; }
        .leaderboard-item img { width: 40px; height: 40px; border-radius: 50%; margin-right: 10px; object-fit: cover; }
        .leaderboard-name { font-weight: 600; flex-grow: 1; } .leaderboard-score { font-weight: 600; color: var(--primary-color); }
    </style>
</head>
<body>
    <div id="app-loader"><div class="spinner"></div><p id="loading-progress">0%</p></div>
    <div id="popup" class="popup-overlay"><div class="popup-content"><div id="popup-body"></div></div></div>
    <div id="app" style="display: none;">
        <header class="profile-header">
            <img id="user-photo" src="https://via.placeholder.com/80" alt="User">
            <h1 id="user-name">Loading...</h1>
            <div id="user-balance-container">Main Balance: <span id="user-balance">0.00</span> TK</div>
        </header>
        <main id="main-content">
            <div id="home-page" class="page active">
                <div class="stats-grid">
                    <div class="stat-card"><h3>Today's Ads</h3><p id="daily-ads-watched">0 / 0</p></div>
                    <div class="stat-card"><h3>Total Referrals</h3><p id="referral-count">0</p></div>
                    <div class="stat-card"><h3>Total Ads Watched</h3><p id="total-ads-watched">0</p></div>
                    <div class="stat-card"><h3>Total Income</h3><p id="total-earned">0.00</p></div>
                </div>
                <div id="earnings-graph-container" class="chart-container">
                    <h3>Last 7 Days Earnings</h3>
                    <div id="earnings-chart" class="chart"></div><div id="earnings-chart-labels" class="chart-labels"></div>
                </div>
                <div id="dynamic-links-container" style="margin-top: 20px;"></div>
            </div>
            <div id="tasks-page" class="page">
                <div class="earning-wallet-card">
                    <h3>Earning Wallet</h3>
                    <p style="font-size: 1.8rem; font-weight: 700; color: var(--primary-color); margin: 10px 0;"><span id="earning-wallet-balance">0.00</span> TK</p>
                    <small>Minimum 100 TK to move to main balance.</small>
                    <button id="move-to-balance-btn" class="action-btn" style="width:100%; margin-top: 15px;" disabled>Move to Main Balance</button>
                </div>
                <div id="ad-progress-container" class="progress-container" style="margin-top:20px;"></div>
                <div id="ad-task-container"></div>
                <h2>Bonus Tasks</h2><div id="dynamic-tasks-container"></div>
            </div>
            <div id="leaderboard-page" class="page">
                <h2>Leaderboard</h2>
                <div class="leaderboard-toggle">
                    <button class="toggle-btn active" data-board="referral">Top Referrers</button>
                    <button class="toggle-btn" data-board="earning">Top Earners</button>
                </div>
                <div id="referral-board"><ul class="leaderboard-list" id="referral-leaderboard-list"></ul></div>
                <div id="earning-board" style="display: none;"><ul class="leaderboard-list" id="earning-leaderboard-list"></ul></div>
            </div>
            <div id="profile-page" class="page">
                <div id="referral-section"></div><div id="withdraw-section"></div>
                <div id="withdrawal-history-section" style="margin-top: 20px;">
                    <h2>Withdrawal History</h2><div id="withdrawal-history-list"></div>
                </div>
            </div>
        </main>
        <nav class="bottom-nav">
            <button class="nav-btn active" data-page="home-page"><i class="fas fa-home"></i><span>Home</span></button>
            <button class="nav-btn" data-page="tasks-page"><i class="fas fa-tasks"></i><span>Tasks</span></button>
            <button class="nav-btn" data-page="leaderboard-page"><i class="fas fa-trophy"></i><span>Leaderboard</span></button>
            <button class="nav-btn" data-page="profile-page"><i class="fas fa-user-alt"></i><span>Profile</span></button>
        </nav>
    </div>

    <script src="https://www.gstatic.com/firebasejs/8.10.1/firebase-app.js"></script>
    <script src="https://www.gstatic.com/firebasejs/8.10.1/firebase-database.js"></script>
    <script src="https://telegram.org/js/telegram-web-app.js"></script>
    
    <script>
    document.addEventListener('DOMContentLoaded', () => {
        const tg = window.Telegram.WebApp;
        // Firebase Configuration Provided
        const firebaseConfig = {
            apiKey: "AIzaSyBTx3Undaw969dNeewB39EoeC_TCVvlE2w",
            authDomain: "ff-tournament-63164.firebaseapp.com",
            databaseURL: "https://ff-tournament-63164-default-rtdb.firebaseio.com",
            projectId: "ff-tournament-63164",
            storageBucket: "ff-tournament-63164.firebasestorage.app",
            messagingSenderId: "93625118854",
            appId: "1:93625118854:web:35bca241f3bb51d8fb2107",
            measurementId: "G-M9HB8PYNTG"
        };
        
        let appConfig = {}; let userState = {}; let tgUser = {}; let earningWalletBalance = 0;
        let leaderboardData = { referral: [], earning: [] };
        let userTransactions = [];

        const popup = document.getElementById('popup');
        const popupBody = document.getElementById('popup-body');
        const loadingProgress = document.getElementById('loading-progress');

        const showPopup = (content) => { popupBody.innerHTML = content; popup.style.display = 'flex'; };
        const closePopup = () => { popup.style.display = 'none'; };
        const updateProgress = (percentage) => { loadingProgress.textContent = `${percentage}%`; };

        const loadAdSdk = (zoneId) => {
            if (!zoneId) { console.error("Ad Zone ID missing"); return; }
            if (document.querySelector(`script[data-zone='${zoneId}']`)) return;
            const script = document.createElement('script');
            script.src = `//libtl.com/sdk.js`;
            script.setAttribute('data-zone', zoneId);
            script.setAttribute('data-sdk', `show_${zoneId}`);
            script.async = true;
            document.body.appendChild(script);
        };

        const initApp = async () => {
            tg.ready(); tg.expand();
            document.body.className = `${tg.colorScheme}-theme`;
            tgUser = tg.initDataUnsafe.user;
            
            if (!tgUser || !tgUser.id) { 
                // Fallback for testing outside Telegram if needed, but strict mode is better
                tg.showAlert("User data not found. Please open via Telegram.", () => tg.close()); 
                return; 
            }

            try {
                updateProgress(10); firebase.initializeApp(firebaseConfig); const db = firebase.database();
                updateProgress(20);
                
                const configSnapshot = await db.ref('config').once('value');
                appConfig = configSnapshot.val() || {};
                updateProgress(40);

                await loadUserData(db); 
                updateProgress(60);

                const referralBoardPromise = db.ref('users').orderByChild('referrals').limitToLast(10).once('value');
                const earningBoardPromise = db.ref('users').orderByChild('totalEarned').limitToLast(10).once('value');
                const historyPromise = loadTransactionHistory(db);

                const [referralBoardSnap, earningBoardSnap] = await Promise.all([referralBoardPromise, earningBoardPromise, historyPromise]);
                updateProgress(80);

                referralBoardSnap.forEach(child => { leaderboardData.referral.push(child.val()); });
                earningBoardSnap.forEach(child => { leaderboardData.earning.push(child.val()); });
                leaderboardData.referral.reverse(); leaderboardData.earning.reverse();
                updateProgress(90);

                loadAdSdk(appConfig.adZoneId);
                earningWalletBalance = parseFloat(localStorage.getItem(`earningWallet_${tgUser.id}`) || '0');
                
                renderUI(); setupNavigation(); setupEventListeners();
                updateProgress(100);

                setTimeout(() => {
                    document.getElementById('app-loader').style.display = 'none';
                    document.getElementById('app').style.display = 'block';
                    if (appConfig.welcomeMessage && !userState.welcomed) {
                        showPopup(`<h2>Welcome!</h2><p>${appConfig.welcomeMessage}</p><button class="popup-close-btn" onclick="closePopup()">Continue</button>`);
                        db.ref(`users/${tgUser.id}/welcomed`).set(true);
                    }
                }, 300);
            } catch (error) { console.error("Init failed:", error); tg.showAlert("Failed to load app. Check console."); }
        };

        const loadUserData = async (db) => {
            const userRef = db.ref(`users/${tgUser.id}`);
            const snapshot = await userRef.once('value');
            let userData = snapshot.val();
            
            if (!userData) {
                const startParam = tg.initDataUnsafe.start_param;
                const referralId = (startParam && !isNaN(startParam)) ? startParam : null;
                
                userData = { 
                    id: tgUser.id, firstName: tgUser.first_name || '', lastName: tgUser.last_name || '', 
                    username: tgUser.username || '', photoUrl: tgUser.photo_url || '', balance: 0, 
                    referrals: 0, referredBy: referralId, totalEarned: 0, lifetimeAdCount: 0, 
                    lastAdWatchDate: '1970-01-01', dailyAdCount: 0, breakUntil: 0, completedTasks: {}, welcomed: false 
                };
                await userRef.set(userData);
                
                if (referralId && referralId != tgUser.id) {
                    const referrerRef = db.ref(`users/${referralId}`);
                    const bonusAmount = parseFloat(appConfig.referralBonus || 0);
                    
                    if (bonusAmount > 0) {
                        await referrerRef.transaction(currentData => {
                            if (currentData) {
                                currentData.balance = (currentData.balance || 0) + bonusAmount;
                                currentData.referrals = (currentData.referrals || 0) + 1;
                            }
                            return currentData;
                        });
                    } else {
                        await referrerRef.child('referrals').set(firebase.database.ServerValue.increment(1));
                    }
                }
            }
            
            const today = new Date().toISOString().slice(0, 10);
            if (userData.lastAdWatchDate !== today) {
                userData.dailyAdCount = 0; userData.lastAdWatchDate = today;
                await userRef.update({ dailyAdCount: 0, lastAdWatchDate: today });
            }
            userState = userData;
        };
        
        const loadTransactionHistory = async (db) => {
            userTransactions = []; 
            const statuses = ['pending', 'completed', 'rejected'];
            const historyPromises = statuses.map(status => db.ref(`withdrawals/${status}`).orderByChild('userId').equalTo(tgUser.id).once('value'));
            const snapshots = await Promise.all(historyPromises);
            snapshots.forEach(snap => { snap.forEach(childSnap => { userTransactions.push(childSnap.val()); }); });
            userTransactions.sort((a, b) => b.timestamp - a.timestamp);
        };
        
        const renderUI = () => {
            document.getElementById('user-photo').src = userState.photoUrl || 'https://via.placeholder.com/80';
            document.getElementById('user-name').textContent = `${userState.firstName} ${userState.lastName}`;
            document.getElementById('user-balance').textContent = (userState.balance || 0).toFixed(2);
            document.getElementById('daily-ads-watched').textContent = `${userState.dailyAdCount || 0} / ${appConfig.dailyAdLimit || 0}`;
            document.getElementById('referral-count').textContent = userState.referrals || 0;
            document.getElementById('total-ads-watched').textContent = userState.lifetimeAdCount || 0;
            document.getElementById('total-earned').textContent = (userState.totalEarned || 0).toFixed(2);
            document.getElementById('earning-wallet-balance').textContent = earningWalletBalance.toFixed(2);
            document.getElementById('move-to-balance-btn').disabled = earningWalletBalance < 100;
            renderAdTask(); renderAdProgress(); renderDynamicTasks(); renderReferralSection(); renderWithdrawSection(); renderEarningsGraph(); renderDynamicLinks();
        };
        
        const renderAdProgress = () => {
            const container = document.getElementById('ad-progress-container');
            const dailyLimit = appConfig.dailyAdLimit || 1;
            const watchedCount = userState.dailyAdCount || 0;
            const percentage = (watchedCount / dailyLimit) * 100;
            container.innerHTML = `<div class="progress-info"><span>Daily Ad Progress</span><span>${watchedCount} / ${dailyLimit}</span></div><div class="progress-bar-bg"><div class="progress-bar-fg" style="width: ${percentage}%;"></div></div>`;
        };
        
        const renderEarningsGraph = async () => {
             const chartEl = document.getElementById('earnings-chart'); const labelsEl = document.getElementById('earnings-chart-labels');
            chartEl.innerHTML = ''; labelsEl.innerHTML = '';
            const today = new Date(); const dates = [];
            for (let i = 6; i >= 0; i--) { const date = new Date(today); date.setDate(today.getDate() - i); dates.push(date.toISOString().slice(0, 10)); }
            const earningsPromises = dates.map(date => firebase.database().ref(`userEarnings/${tgUser.id}/${date}`).once('value'));
            const snapshots = await Promise.all(earningsPromises);
            const earningsData = snapshots.map(snap => snap.val() || 0);
            const maxEarning = Math.max(...earningsData, 1);
            earningsData.forEach((earning, index) => {
                const height = (earning / maxEarning) * 100;
                const dateLabel = new Date(dates[index]).toLocaleDateString('en-US', { day: 'numeric' });
                chartEl.innerHTML += `<div class="bar" style="height: ${height}%;"><span class="tooltip">${earning.toFixed(2)} TK</span></div>`;
                labelsEl.innerHTML += `<span>${dateLabel}</span>`;
            });
        };
        
        const handleWatchAd = async () => {
            if (userState.dailyAdCount >= appConfig.dailyAdLimit) { tg.showAlert("Daily limit reached."); return; }
            const now = Date.now();
            if (userState.breakUntil && now < userState.breakUntil) {
                const remaining = Math.ceil((userState.breakUntil - now) / 60000);
                tg.showAlert(`Wait ${remaining} more minutes.`); return;
            }
            
            const adFunction = window['show_' + appConfig.adZoneId];
            if (typeof adFunction !== 'function') {
                tg.showAlert("Ad not ready. Wait or check config."); return;
            }

            showPopup(`<div class="popup-loader"><div class="spinner"></div></div><h2>Loading Ad</h2><p>Please wait...</p>`);
            
            try {
                await adFunction();
                closePopup();
                const adValue = appConfig.adValue || 0;
                
                earningWalletBalance += adValue;
                localStorage.setItem(`earningWallet_${tgUser.id}`, earningWalletBalance.toString());

                userState.dailyAdCount++;
                userState.lifetimeAdCount++;
                userState.totalEarned = (userState.totalEarned || 0) + adValue;

                const db = firebase.database(); const userRef = db.ref(`users/${tgUser.id}`);
                const updates = {};
                updates.dailyAdCount = userState.dailyAdCount;
                updates.lifetimeAdCount = userState.lifetimeAdCount;
                updates.totalEarned = userState.totalEarned;
                if ((userState.dailyAdCount) % appConfig.adsPerBreak === 0 && appConfig.adsPerBreak > 0) {
                    updates.breakUntil = Date.now() + (appConfig.breakDuration * 60000);
                    userState.breakUntil = updates.breakUntil;
                }
                await userRef.update(updates);
                
                const today = new Date().toISOString().slice(0, 10);
                await db.ref(`userEarnings/${tgUser.id}/${today}`).set(firebase.database.ServerValue.increment(adValue));

                tg.HapticFeedback.notificationOccurred('success');
                tg.showAlert(`+${adValue.toFixed(2)} TK added to Earning Wallet.`);
                renderUI();

            } catch (error) {
                closePopup();
                tg.showAlert("Ad could not be loaded.");
            }
        };

        const handleMoveToBalance = async () => {
            if(earningWalletBalance < 100) { tg.showAlert("Need at least 100 TK."); return; }
            const btn = document.getElementById('move-to-balance-btn'); btn.disabled = true; btn.textContent = 'Moving...';
            try {
                const db = firebase.database();
                await db.ref(`users/${tgUser.id}/balance`).set(firebase.database.ServerValue.increment(earningWalletBalance));
                userState.balance += earningWalletBalance;
                earningWalletBalance = 0;
                localStorage.setItem(`earningWallet_${tgUser.id}`, '0');
                tg.HapticFeedback.notificationOccurred('success');
                tg.showAlert("Moved to main balance!");
            } catch(error) { tg.showAlert("Failed."); }
            finally { btn.textContent = 'Move to Main Balance'; renderUI(); }
        };

        const renderWithdrawalHistory = () => {
            const listEl = document.getElementById('withdrawal-history-list');
            if (userTransactions.length === 0) { listEl.innerHTML = '<p>No history found.</p>'; return; }
            listEl.innerHTML = userTransactions.map(item => `
                <div class="history-item">
                    <div class="history-details">
                        <p>${item.amount.toFixed(2)} TK - ${item.method}</p>
                        <small>${new Date(item.timestamp).toLocaleString()}</small>
                    </div>
                    <span class="history-status status-${item.status}">${item.status}</span>
                </div>`).join('');
        };
        
        const setupNavigation = () => {
            const navButtons = document.querySelectorAll('.nav-btn'); const pages = document.querySelectorAll('.page');
            navButtons.forEach(button => {
                button.addEventListener('click', () => {
                    const pageId = button.dataset.page;
                    pages.forEach(page => page.classList.remove('active'));
                    document.getElementById(pageId).classList.add('active');
                    navButtons.forEach(btn => btn.classList.remove('active'));
                    button.classList.add('active');
                    if (pageId === 'profile-page') { renderWithdrawalHistory(); }
                    if (pageId === 'leaderboard-page') { renderLeaderboard(); }
                });
            });
        };
        
        const setupEventListeners = () => {
            popup.addEventListener('click', (e) => { if (e.target.classList.contains('popup-close-btn') || e.target.id === 'popup') closePopup(); });
            document.querySelector('.leaderboard-toggle').addEventListener('click', (e) => {
                if(e.target.classList.contains('toggle-btn')) {
                    document.querySelectorAll('.toggle-btn').forEach(b => b.classList.remove('active'));
                    e.target.classList.add('active');
                    renderLeaderboard();
                }
            });
            document.getElementById('dynamic-tasks-container').addEventListener('click', (e) => handleClaimTask(e));
            document.getElementById('profile-page').addEventListener('submit', (e) => handleWithdraw(e));
            document.getElementById('referral-section').addEventListener('click', (e) => { if(e.target.id === 'copy-ref-link-btn') copyReferralLink(); });
            document.getElementById('move-to-balance-btn').addEventListener('click', handleMoveToBalance);
        };

        const renderAdTask = () => {
            const container = document.getElementById('ad-task-container');
            const now = Date.now();
            const onBreak = userState.breakUntil && now < userState.breakUntil;
            const limitReached = userState.dailyAdCount >= appConfig.dailyAdLimit;
            let html = '';
            if (onBreak) {
                const remaining = Math.ceil((userState.breakUntil - now) / 60000);
                html = `<div class="task-container"><img class="task-icon-img" src="https://img.icons8.com/ios-filled/100/hourglass.png" alt="icon"><div class="task-info"><h3>Break Time!</h3><p>Wait ${remaining} minutes.</p></div><button class="action-btn" disabled>Waiting</button></div>`;
            } else if (limitReached) {
                html = `<div class="task-container"><img class="task-icon-img" src="https://img.icons8.com/ios-filled/100/stop-circled.png" alt="icon"><div class="task-info"><h3>Daily Limit Reached</h3><p>Come back tomorrow.</p></div><button class="action-btn" disabled>Limit</button></div>`;
            } else {
                html = `<div class="task-container"><img class="task-icon-img" src="https://img.icons8.com/ios-filled/100/play-button-circled.png" alt="icon"><div class="task-info"><h3>Watch a Rewarded Ad</h3><p>Earn ${appConfig.adValue || 0} TK.</p></div><button id="watch-ad-btn" class="action-btn">Watch Ad</button></div>`;
            }
            container.innerHTML = html;
            if (!onBreak && !limitReached) { document.getElementById('watch-ad-btn').addEventListener('click', handleWatchAd); }
        };

        const handleClaimTask = async (e) => {
            if (e.target.classList.contains('action-btn') && e.target.dataset.taskId) {
                const taskId = e.target.dataset.taskId;
                const task = appConfig.tasks[taskId];
                if (!task || (userState.completedTasks && userState.completedTasks[taskId])) { tg.showAlert('Already claimed.'); return; }
                tg.openLink(e.target.dataset.taskUrl);
                e.target.disabled = true; e.target.textContent = 'Claiming...';
                setTimeout(async () => {
                    try {
                        const db = firebase.database(); const userRef = db.ref(`users/${tgUser.id}`);
                        await userRef.child(`completedTasks/${taskId}`).set(true);
                        await userRef.child('balance').set(firebase.database.ServerValue.increment(task.reward));
                        userState.balance += task.reward;
                        if (!userState.completedTasks) userState.completedTasks = {};
                        userState.completedTasks[taskId] = true;
                        tg.HapticFeedback.notificationOccurred('success');
                        tg.showAlert(`You received ${task.reward} bonus TK.`);
                        renderUI();
                    } catch (error) { tg.showAlert('Failed.'); e.target.disabled = false; e.target.textContent = 'Claim Bonus'; }
                }, 5000);
            }
        };

        const handleWithdraw = async (e) => {
            if(e.target.id !== 'withdraw-form') return;
            e.preventDefault();

            const minRefsRequired = appConfig.minimumWithdrawReferrals || 0;
            const userReferrals = userState.referrals || 0;
            
            if (minRefsRequired > 0 && userReferrals < minRefsRequired) {
                tg.showAlert(`Withdrawal failed. Need at least ${minRefsRequired} referrals.`);
                return;
            }

            const methodEl = document.getElementById('withdraw-method');
            const accountNumber = document.getElementById('account-number').value;
            const amount = parseFloat(document.getElementById('amount').value);
            const selectedMethod = appConfig.withdrawMethods.find(m => m.name === methodEl.value);
            
            if (isNaN(amount) || amount <= 0) { tg.showAlert("Invalid amount."); return; }
            if (!selectedMethod) { tg.showAlert("Invalid method."); return; }
            if (amount < selectedMethod.min) { tg.showAlert(`Min withdrawal for ${selectedMethod.name} is ${selectedMethod.min} TK.`); return; }
            if (amount > userState.balance) { tg.showAlert("Not enough balance."); return; }
            
            const btn = document.getElementById('withdraw-submit-btn'); btn.disabled = true; btn.textContent = 'Submitting...';
            const db = firebase.database();
            try {
                const userRef = db.ref(`users/${tgUser.id}`);
                userState.balance -= amount;
                renderUI(); 
                
                await userRef.child('balance').set(firebase.database.ServerValue.increment(-amount));
                const reqId = db.ref('withdrawals/pending').push().key;
                const newRequest = { 
                    id: reqId, userId: tgUser.id, userName: `${userState.firstName} ${userState.lastName}`, 
                    method: selectedMethod.name, account: accountNumber, amount: amount, 
                    status: 'pending', timestamp: firebase.database.ServerValue.TIMESTAMP 
                };
                await db.ref(`withdrawals/pending/${reqId}`).set(newRequest);
                
                userTransactions.unshift(newRequest);
                renderWithdrawalHistory(); 

                tg.HapticFeedback.notificationOccurred('success');
                tg.showAlert("Request submitted!");
                document.getElementById('withdraw-form').reset();
            } catch (error) {
                userState.balance += amount;
                tg.showAlert("Failed. Balance restored.");
            } finally {
                renderUI();
                btn.disabled = false;
                btn.textContent = 'Submit Request';
            }
        };

        const copyReferralLink = () => {
            const refLinkInput = document.getElementById('referral-link');
            if(navigator.clipboard) {
                navigator.clipboard.writeText(refLinkInput.value).then(() => tg.showAlert("Link copied!"));
            } else {
                refLinkInput.select();
                document.execCommand('copy');
                tg.showAlert("Link copied!");
            }
        };

        const renderLeaderboard = () => {
            const activeBoard = document.querySelector('.toggle-btn.active').dataset.board;
            const listEl = document.getElementById(activeBoard === 'referral' ? 'referral-leaderboard-list' : 'earning-leaderboard-list');
            const data = leaderboardData[activeBoard];
            document.getElementById('referral-board').style.display = activeBoard === 'referral' ? 'block' : 'none';
            document.getElementById('earning-board').style.display = activeBoard === 'earning' ? 'block' : 'none';
            listEl.innerHTML = '';
            if (data.length === 0) { listEl.innerHTML = '<li>No data.</li>'; return; }
            data.forEach((user, index) => {
                const score = activeBoard === 'referral' ? (user.referrals || 0) : (user.totalEarned || 0).toFixed(2);
                const item = document.createElement('li'); item.className = 'leaderboard-item';
                item.innerHTML = `<span class="rank">#${index + 1}</span><img src="${user.photoUrl || 'https://via.placeholder.com/40'}" alt="User"><span class="leaderboard-name">${user.firstName}</span><span class="leaderboard-score">${score}</span>`;
                listEl.appendChild(item);
            });
        };

        const renderDynamicTasks = () => {
            const container = document.getElementById('dynamic-tasks-container'); container.innerHTML = '';
            if (appConfig.tasks) {
                const tasks = [];
                for(const key in appConfig.tasks) { tasks.push({key, ...appConfig.tasks[key]}); }
                tasks.reverse();
                tasks.forEach(task => {
                    const isCompleted = userState.completedTasks && userState.completedTasks[task.key];
                    const el = document.createElement('div'); el.className = 'task-container';
                    el.innerHTML = `<img class="task-icon-img" src="${task.icon}" alt="icon"><div class="task-info"><h3>${task.name}</h3><p>Get ${task.reward} bonus TK.</p></div><button class="action-btn" data-task-id="${task.key}" data-task-url="${task.url}" ${isCompleted ? 'disabled' : ''}>${isCompleted ? 'Claimed' : 'Claim Bonus'}</button>`;
                    container.appendChild(el);
                });
            }
        };

        const renderReferralSection = () => {
            const container = document.getElementById('referral-section');
            const refLink = `https://t.me/${appConfig.botUsername}/app?startapp=${tgUser.id}`;
            container.innerHTML = `<div class="task-container" style="flex-direction: column; align-items: stretch;"><h3>Refer & Earn More!</h3><p>Invite friends and earn ${appConfig.referralBonus || 0} TK per referral.</p><input type="text" id="referral-link" value="${refLink}" readonly style="padding: 10px; text-align: center; border-radius: 8px; margin: 10px 0;"><button id="copy-ref-link-btn" class="action-btn">Copy Link</button></div>`;
        };

        const renderWithdrawSection = () => {
            const container = document.getElementById('withdraw-section'); let optionsHtml = '';
            if (appConfig.withdrawMethods) { appConfig.withdrawMethods.forEach(method => { optionsHtml += `<option value="${method.name}">${method.name} (Min: ${method.min} TK)</option>`; }); }
            
            const minRefs = appConfig.minimumWithdrawReferrals || 0;
            let referralNotice = '';
            if (minRefs > 0) {
                referralNotice = `<p style="font-size: 0.85rem; text-align: center; margin-bottom: 15px; opacity: 0.8;"><b>Note:</b> Minimum ${minRefs} referrals required for withdrawal.</p>`;
            }

            container.innerHTML = `<form id="withdraw-form" class="task-container" style="flex-direction: column; align-items: stretch;"><h3>Request Withdraw</h3>${referralNotice}<div class="form-group" style="width:100%"><label for="withdraw-method">Method:</label><select id="withdraw-method" required style="width: 100%; padding: 10px; border-radius: 8px;">${optionsHtml}</select></div><div class="form-group" style="width:100%"><label for="account-number">Account Number/ID:</label><input type="text" id="account-number" required style="width: 100%; padding: 10px; border-radius: 8px;"></div><div class="form-group" style="width:100%"><label for="amount">Amount:</label><input type="number" step="any" id="amount" required style="width: 100%; padding: 10px; border-radius: 8px;"></div><button type="submit" id="withdraw-submit-btn" class="action-btn">Submit Request</button></form>`;
        };

        const renderDynamicLinks = () => {
            const container = document.getElementById('dynamic-links-container'); container.innerHTML = '';
            if (appConfig.links) {
                const links = [];
                for (const key in appConfig.links) { links.push({key, ...appConfig.links[key]}); }
                links.reverse();
                links.forEach(link => {
                    const el = document.createElement('div'); el.className = 'task-container';
                    el.innerHTML = `<img class="task-icon-img" src="${link.icon}" alt="icon"><div class="task-info"><h3>${link.name}</h3></div><button class="action-btn" onclick="window.Telegram.WebApp.openLink('${link.url}')">Visit</button>`;
                    container.appendChild(el);
                });
            }
        };

        initApp();
    });
    </script>
</body>
</html>                    </div>
                    <div class="mb-6">
                        <label class="block text-gray-700 text-sm font-bold mb-2">Password</label>
                        <input type="password" id="login-pass" placeholder="Password" class="w-full px-4 py-3 rounded-lg border border-gray-300 focus:outline-none focus:border-sky-500">
                    </div>
                    <button onclick="handleLogin()" class="w-full bg-sky-600 text-white font-bold py-3 rounded-lg shadow-lg hover:bg-sky-700">Login</button>
                    <div class="mt-6 text-center text-sm text-gray-600">New user? <button onclick="toggleAuth('register')" class="text-sky-600 font-bold hover:underline">Register Now</button></div>
                </div>

                <div id="register-view" class="hidden">
                    <h2 class="text-2xl font-bold text-gray-800 mb-6">Register</h2>
                    <button onclick="handleGoogleLogin()" class="w-full bg-white border border-gray-300 text-gray-700 font-semibold py-2.5 rounded-lg shadow-sm hover:bg-gray-50 transition flex items-center justify-center gap-3 mb-6">
                        <img src="https://www.svgrepo.com/show/475656/google-color.svg" class="w-5 h-5" alt="Google"> Sign up with Google
                    </button>
                    <div class="relative flex py-2 items-center mb-6">
                        <div class="flex-grow border-t border-gray-200"></div>
                        <span class="flex-shrink-0 mx-4 text-gray-400 text-xs font-medium">Or sign up with credentials</span>
                        <div class="flex-grow border-t border-gray-200"></div>
                    </div>
                    <div class="space-y-4 mb-6">
                        <div><label class="block text-gray-700 text-sm font-bold mb-1">Name</label><input type="text" id="reg-name" placeholder="Name" class="w-full px-4 py-2.5 rounded-lg border border-gray-300 focus:outline-none focus:border-sky-500"></div>
                        <div><label class="block text-gray-700 text-sm font-bold mb-1">Phone</label><input type="number" id="reg-phone" placeholder="Phone" class="w-full px-4 py-2.5 rounded-lg border border-gray-300 focus:outline-none focus:border-sky-500"></div>
                        <div><label class="block text-gray-700 text-sm font-bold mb-1">Email</label><input type="email" id="reg-email" placeholder="Email" class="w-full px-4 py-2.5 rounded-lg border border-gray-300 focus:outline-none focus:border-sky-500"></div>
                        <div><label class="block text-gray-700 text-sm font-bold mb-1">Password</label><input type="password" id="reg-pass" placeholder="Password" class="w-full px-4 py-2.5 rounded-lg border border-gray-300 focus:outline-none focus:border-sky-500"></div>
                    </div>
                    <button onclick="handleRegister()" class="w-full bg-sky-600 text-white font-bold py-3 rounded-lg shadow-lg hover:bg-sky-700">Register</button>
                    <div class="mt-6 text-center text-sm text-gray-600">Already member? <button onclick="toggleAuth('login')" class="text-sky-600 font-bold hover:underline">Login Now</button></div>
                </div>
            </div>
        </section>

        <section id="home-sec" class="animate-fade-in pb-8">
            <div id="banner-container" class="mb-6 overflow-hidden shadow-md h-44 bg-gray-200 relative group">
                <div id="slider-wrapper" class="flex h-full w-full transition-transform duration-500 ease-out">
                    <img src="https://placehold.co/600x300/29b6f6/FFF?text=Loading..." class="w-full h-full object-cover flex-shrink-0">
                </div>
            </div>

            <div class="grid grid-cols-3 gap-3 mb-5 px-2">
                <a id="home-whatsapp" href="#" class="bg-white py-3 px-3 rounded-xl shadow-sm border border-gray-100 flex items-center justify-between active:scale-95 transition h-12 no-underline">
                    <div class="w-8 h-8 rounded-full flex items-center justify-center bg-green-50 flex-shrink-0"><i class="fab fa-whatsapp text-xl text-[#25D366]"></i></div>
                    <span class="text-[12px] font-bold text-gray-700 lang-auto">WhatsApp</span>
                </a>
                <a id="home-telegram" href="#" class="bg-white py-3 px-3 rounded-xl shadow-sm border border-gray-100 flex items-center justify-between active:scale-95 transition h-12 no-underline">
                    <div class="w-8 h-8 rounded-full flex items-center justify-center bg-blue-50 flex-shrink-0"><i class="fab fa-telegram-plane text-xl text-[#0088cc]"></i></div>
                    <span class="text-[12px] font-bold text-gray-700 lang-auto">Telegram</span>
                </a>
                <a id="home-app-dl" href="#" target="_blank" class="bg-white py-3 px-3 rounded-xl shadow-sm border border-gray-100 flex items-center justify-between active:scale-95 transition h-12 no-underline">
                    <div class="w-8 h-8 rounded-full overflow-hidden shadow-sm flex-shrink-0"><img src="https://i.ibb.co/WvrTGr5Y/google-play-store-logo-1273375-804.jpg" class="w-full h-full object-cover" alt="App"></div>
                    <span class="text-[12px] font-bold text-gray-700 leading-tight lang-auto">App<br>Download</span>
                </a>
            </div>
            
            <div id="services-list" class="space-y-6 mb-8"><div class="col-span-3 text-center py-10"><i class="fas fa-spinner fa-spin text-sky-500"></i> Loading Games...</div></div>

            <div class="mt-8 mb-6">
                <h3 class="text-center font-bold text-gray-700 text-xl mb-3 lang-auto">Latest Orders</h3>
                <div id="latest-orders-list" class="space-y-3 px-1"></div>
            </div>

            <div id="promo-banner-container" class="mt-8 mb-6 px-1 hidden">
                <a id="promo-banner-link" href="#" target="_blank" class="block w-full aspect-video rounded-xl overflow-hidden shadow-md"><img id="promo-banner-img" src="" class="w-full h-full object-cover"></a>
            </div>
            
            <div class="mt-8 bg-white p-4 rounded-xl shadow-sm border border-gray-100">
                <div class="text-center mb-5">
                    <img id="about-logo-img" src="https://placehold.co/150x50?text=Logo" class="h-10 mx-auto object-contain mb-2">
                    <h2 id="about-site-name" class="text-lg font-bold text-gray-800 lang-auto">Nenox Shop</h2>
                    <p class="text-sm text-gray-500 mt-1">Free Fire Diamond TopUp<br>Largest TopUp Site In Bangladesh</p>
                </div>
                <p class="text-xs text-center text-gray-600 mb-5 px-2 leading-relaxed"><b>নোটিশ:</b> যেকোন অর্ডার জনিত সমস্যায় মেসেজারে অর্ডারের স্ক্রিনশট এবং পেমেন্টের ট্রানজেকশন আইডি লিখে মেসেজ দিন।</p>
                <div class="grid grid-cols-2 gap-3 mb-6">
                    <a id="about-facebook" href="#" target="_blank" class="flex items-center justify-center gap-2 bg-gray-50 p-3 rounded-lg border border-gray-200 hover:bg-gray-100 transition no-underline"><i class="fab fa-facebook-f text-lg text-[#1877F2]"></i> <span class="font-semibold text-sm text-gray-700">Facebook</span></a>
                    <a id="about-youtube" href="#" target="_blank" class="flex items-center justify-center gap-2 bg-gray-50 p-3 rounded-lg border border-gray-200 hover:bg-gray-100 transition no-underline"><i class="fab fa-youtube text-lg text-[#FF0000]"></i> <span class="font-semibold text-sm text-gray-700">YouTube</span></a>
                    <a id="about-telegram" href="#" target="_blank" class="flex items-center justify-center gap-2 bg-gray-50 p-3 rounded-lg border border-gray-200 hover:bg-gray-100 transition no-underline"><i class="fab fa-telegram-plane text-lg text-[#0088cc]"></i> <span class="font-semibold text-sm text-gray-700">Telegram</span></a>
                    <a id="about-whatsapp" href="#" target="_blank" class="flex items-center justify-center gap-2 bg-gray-50 p-3 rounded-lg border border-gray-200 hover:bg-gray-100 transition no-underline"><i class="fab fa-whatsapp text-lg text-[#25D366]"></i> <span class="font-semibold text-sm text-gray-700">WhatsApp</span></a>
                </div>
                <div class="text-center border-t border-gray-200 pt-4 mt-6"><p class="text-xs text-gray-500">© 2026 Nano TopUp BD | Developed By <a href="https://t.me/gfxrahulvai" target="_blank" class="font-bold text-sky-600 hover:underline">Team Nexa</a></p></div>
            </div>
        </section>

        <section id="tutorial-sec" class="hidden animate-fade-in pb-20"><div class="bg-white"><div class="p-6 border-b border-gray-100"><h2 class="font-semibold text-xl text-gray-800 mb-1">Video Tutorials</h2><p class="text-sm text-gray-500">Learn how to use our services</p></div><div class="p-4"><div id="tutorial-list-container" class="space-y-4"></div></div></div></section>
        <section id="history-sec" class="hidden animate-fade-in pb-24"><div class="bg-white border-b border-gray-200 px-4 py-3"><h2 class="text-lg font-bold text-gray-800">Order History</h2></div><div id="history-list" class="px-4 py-3"></div></section>

        <section id="topup-sec" class="hidden animate-fade-in pb-10 bg-slate-50">
            <div class="bg-white p-3 rounded-xl shadow-sm flex items-center gap-4 mb-4 border border-gray-100 mx-2 mt-2">
                <button onclick="showSec('home')" class="text-gray-600 hover:text-sky-500 w-8 h-8 flex items-center justify-center rounded-full hover:bg-gray-200 absolute top-4 right-4"><i class="fas fa-times text-lg"></i></button>
                <img id="topup-game-img" src="" class="w-16 h-16 rounded-xl object-cover shadow-sm">
                <div><h3 class="font-bold text-gray-800 text-base" id="topup-game-title">Game Name</h3><p class="text-xs text-gray-700 font-medium" id="topup-sub-title">Game / Topup</p></div>
            </div>
            <div class="bg-white p-3 rounded-xl shadow-sm mb-4 border border-gray-100 mx-2">
                <div class="flex items-center gap-2 mb-3"><span class="bg-red-600 text-white w-5 h-5 flex items-center justify-center rounded-full text-xs font-bold shadow-md">1</span><h3 class="font-bold text-gray-800 text-sm">Select Recharge</h3></div>
                <div id="product-loader" class="text-center py-10 hidden"><i class="fas fa-spinner fa-spin text-sky-500 text-2xl"></i></div>
                <div id="products-grid" class="grid grid-cols-2 gap-2"></div>
                <div class="mt-3 text-right"><a href="#" onclick="showSec('tutorial')" class="text-xs text-gray-600 hover:text-sky-600 no-underline"><i class="fas fa-external-link-alt"></i> কিভাবে অর্ডার করবেন ?</a></div>
            </div>
            <div id="player-id-section" class="bg-white p-3 rounded-xl shadow-sm mb-4 border border-gray-100 mx-2">
                <div class="flex items-center gap-2 mb-2"><span class="bg-red-600 text-white w-5 h-5 flex items-center justify-center rounded-full text-xs font-bold shadow-md">2</span><h3 class="font-bold text-gray-800 text-sm">Account Info</h3></div>
                <div class="mb-2"><label class="block text-xs font-bold text-gray-700 mb-1">Player Id</label><input type="text" id="game-player-id" placeholder="Player Id" class="w-full bg-white border border-gray-300 p-2 rounded focus:outline-none focus:border-sky-500 text-sm text-gray-700"></div>
                <div id="uidCheckBtn" onclick="uidCheck()" class="w-full bg-gradient-to-r from-red-700 to-orange-500 text-white text-center py-2 rounded font-bold text-sm shadow cursor-pointer relative overflow-hidden">Click to check player name</div>
            </div>
            <div class="bg-white p-3 rounded-xl shadow-sm mb-4 border border-gray-100 mx-2">
                <div class="flex items-center gap-2 mb-3"><span class="bg-red-600 text-white w-5 h-5 flex items-center justify-center rounded-full text-xs font-bold shadow-md">3</span><h3 class="font-bold text-gray-800 text-sm">Select one option</h3></div>
                <div class="grid grid-cols-2 gap-3 mb-2">
                    <div onclick="selectPaymentOption('wallet')" id="opt-wallet" class="pay-option-card selected"><div class="h-20 flex flex-col items-center justify-center p-2"><div class="flex items-center gap-1"><i class="fas fa-wallet text-orange-500 text-2xl"></i><span class="font-bold text-lg text-gray-800">ওয়ালেট</span></div></div><div class="bg-gray-200 text-center py-1 text-xs font-bold text-gray-600">Wallet Pay</div></div>
                    <div onclick="selectPaymentOption('instant')" id="opt-instant" class="pay-option-card">
                        <div class="h-20 flex items-center justify-center gap-1 p-2">
                             <img src="https://i.ibb.co.com/9kCCTNsx/1656227518bkash-logo-png.png" class="h-5 object-contain">
                             <img src="https://i.ibb.co.com/VcqJTvpn/Nagad-Logo-horizontally-og.png" class="h-4 object-contain">
                             <!-- Upay REMOVED from here -->
                        </div>
                        <div class="bg-gray-200 text-center py-1 text-xs font-bold text-gray-600">Instant Pay</div>
                    </div>
                </div>
                <div class="flex items-center gap-2 mb-4 px-1"><i class="fas fa-info-circle text-gray-400 text-xs"></i><p class="text-[10px] text-gray-500">আপনার একাউন্ট ব্যালেন্স <span class="text-blue-600 font-bold">৳<span id="topup-balance-display">0.00</span></span></p></div>
                <button onclick="processOrder()" class="w-full bg-[#4a148c] text-white py-3 rounded-lg font-bold shadow-lg hover:bg-[#380e6d] transition text-sm">Buy Now</button>
            </div>
            <div id="game-rules-box" class="bg-white p-3 rounded-xl shadow-sm mb-6 border border-gray-100 mx-2"><h3 class="font-bold text-gray-800 text-sm mb-2">Rules</h3><div id="game-rules-text" class="text-xs text-gray-700 leading-relaxed space-y-2"></div></div>
            <span id="total-price" class="hidden">0</span>
        </section>

        <!-- ADD MONEY SECTION (Upay Removed) -->
        <section id="addmoney-sec" class="hidden animate-fade-in pb-20 bg-[#f8fafc] min-h-screen">
            <div id="am-step-1" class="step-transition">
                <div class="bg-white p-6 shadow-sm border-b border-gray-100 mb-6"><h2 class="font-bold text-lg text-gray-800 uppercase tracking-wide">Add Money</h2></div>
                <div class="px-4">
                    <div class="bg-white rounded-lg p-8 shadow-sm border border-gray-200 mb-6 text-center">
                        <label class="block text-gray-500 text-sm font-bold mb-4 uppercase tracking-wider">Enter Amount to Deposit</label>
                        <div class="relative mb-6 max-w-[200px] mx-auto">
                            <span class="absolute left-3 top-1/2 transform -translate-y-1/2 text-2xl font-bold text-gray-400">৳</span>
                            <input type="number" id="new-amount-input" value="100" class="w-full border-b-2 border-gray-300 py-2 pl-8 pr-2 text-center text-3xl font-bold text-gray-800 focus:outline-none focus:border-red-500 bg-transparent" placeholder="100">
                        </div>
                        <button onclick="goToMethodStep()" class="w-full bg-sky-600 text-white font-bold py-3.5 rounded shadow-lg hover:bg-sky-700 active:scale-95 transition-all text-sm uppercase tracking-wide">PROCEED TO PAYMENT</button>
                    </div>
                    <div class="mt-4">
                        <div class="flex items-center gap-2 mb-2"><i class="fab fa-youtube text-red-600"></i><h3 class="font-bold text-gray-700 text-sm">Tutorial: How to Add Money</h3></div>
                        <div class="bg-white rounded-lg overflow-hidden shadow-sm border border-gray-200 p-1">
                            <div class="relative w-full aspect-video"><iframe id="new-tutorial-video" class="w-full h-full rounded" src="https://www.youtube.com/embed/dQw4w9WgXcQ" title="How to Add Money" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe></div>
                        </div>
                    </div>
                </div>
            </div>

            <div id="am-step-2" class="hidden step-transition h-screen flex flex-col bg-[#f0f4f7]">
                <div class="bg-white p-3 flex items-center justify-between shadow-sm sticky top-0 z-10">
                    <button onclick="backToAmountStep()" class="text-gray-500 w-8 h-8 flex items-center justify-center"><i class="fas fa-arrow-left text-lg"></i></button>
                    <div class="text-right"><img src="" id="step2-logo" class="h-8 object-contain inline-block"></div>
                    <button onclick="showSec('home')" class="text-gray-400"><i class="fas fa-times"></i></button>
                </div>
                <div class="flex-1 flex flex-col justify-center items-center px-6">
                    <div class="bg-white rounded-full w-32 h-32 flex items-center justify-center shadow-sm border border-gray-100 mb-6 p-4"><img src="" id="step2-center-logo" class="w-full object-contain"></div>
                    <h2 class="text-gray-600 font-medium mb-8 text-lg">Auto Pay</h2>
                    <div class="w-full bg-[#003399] text-white py-2.5 text-center text-sm font-bold rounded-t-lg mb-0 shadow-md">মোবাইল ব্যাংকিং</div>
                    <div class="w-full grid grid-cols-2 gap-4 bg-white p-4 rounded-b-lg shadow-sm border border-gray-200 border-t-0">
                        <button onclick="goToPaymentPage('bKash', 'theme-bkash', 'https://i.ibb.co.com/9kCCTNsx/1656227518bkash-logo-png.png')" class="bg-white border border-gray-200 rounded-lg p-2 flex items-center justify-center hover:border-pink-500 hover:shadow-md transition h-16"><img src="https://i.ibb.co.com/9kCCTNsx/1656227518bkash-logo-png.png" class="h-8 object-contain"></button>
                        <button onclick="goToPaymentPage('Nagad', 'theme-nagad', 'https://i.ibb.co.com/VcqJTvpn/Nagad-Logo-horizontally-og.png')" class="bg-white border border-gray-200 rounded-lg p-2 flex items-center justify-center hover:border-orange-500 hover:shadow-md transition h-16"><img src="https://i.ibb.co.com/VcqJTvpn/Nagad-Logo-horizontally-og.png" class="h-6 object-contain"></button>
                        <!-- Upay Button REMOVED from here -->
                    </div>
                </div>
            </div>

            <div id="am-step-3" class="hidden step-transition min-h-screen bg-[#f0f4f7] relative pb-24">
                <div class="bg-white p-3 shadow-sm flex justify-between items-center sticky top-0 z-20">
                    <button onclick="backToMethodStep()" class="w-8 h-8 rounded-full bg-gray-100 flex items-center justify-center text-gray-600"><i class="fas fa-arrow-left"></i></button>
                    <div class="flex items-center gap-2"><span class="bg-gray-100 px-2 py-0.5 rounded text-xs font-bold text-gray-500">English <i class="fas fa-caret-down ml-1"></i></span><button onclick="showSec('home')" class="text-gray-400 text-lg"><i class="fas fa-times"></i></button></div>
                </div>
                <div class="text-center py-6"><img id="pay-method-logo" src="" class="h-12 mx-auto object-contain drop-shadow-sm"></div>
                <div class="px-4">
                    <div class="bg-white rounded-xl p-4 flex items-center gap-4 mb-4 shadow-sm"><div class="w-12 h-12 rounded-full border border-gray-100 p-1"><img id="pay-site-logo-small" src="" class="w-full h-full rounded-full object-cover"></div><div><p class="text-sm text-gray-800 font-bold">Auto Pay</p></div></div>
                    <div class="bg-white rounded-xl p-4 mb-4 shadow-sm flex items-center gap-2"><span class="text-lg font-bold text-gray-700">৳</span><span id="pay-amount-display" class="text-xl font-bold text-gray-800">100</span></div>
                    <div id="payment-theme-card" class="rounded-xl shadow-lg overflow-hidden text-white mb-6 relative transition-colors duration-300">
                        <div class="p-5 text-center"><h3 class="font-bold text-lg mb-4 drop-shadow-md">ট্রানজেকশন আইডি দিন</h3><input type="text" id="pay-trx-input" class="w-full bg-white text-gray-800 text-center py-3 rounded text-sm font-bold shadow-inner focus:outline-none placeholder-gray-400" placeholder="ট্রানজেকশন আইডি দিন"></div>
                        <div class="bg-black/10 p-5 text-sm space-y-4 backdrop-blur-sm border-t border-white/20">
                            <div class="flex gap-2"><span class="mt-1">•</span><p>*247# ডায়াল করে আপনার <span class="method-name font-bold">bKash</span> মোবাইল মেনুতে যান।</p></div>
                            <div class="flex gap-2"><span class="mt-1">•</span><p>"Send Money" -এ ক্লিক করুন।</p></div>
                            <div class="flex flex-col gap-1 pl-3">
                                <p>• প্রাপক নম্বর হিসেবে এই নম্বরটি লিখুন:</p>
                                <div class="flex items-center gap-2 mt-1">
                                    <span id="pay-admin-number" class="text-2xl font-bold text-yellow-300 tracking-wider">Loading...</span>
                                    <button onclick="copyPayNumber()" class="bg-black/30 hover:bg-black/50 text-white px-2 py-1 rounded text-[10px] font-bold transition flex items-center gap-1 border border-white/30"><i class="far fa-copy"></i> COPY</button>
                                </div>
                            </div>
                            <div class="flex gap-2"><span class="mt-1">•</span><p>টাকার পরিমাণ: <span id="pay-amount-text" class="font-bold text-yellow-300">100</span></p></div>
                            <div class="flex gap-2"><span class="mt-1">•</span><p class="font-bold text-yellow-200">এখন উপরের বক্সে আপনার Transaction ID দিন এবং VERIFY বাটনে ক্লিক করুন।</p></div>
                        </div>
                    </div>
                </div>
                <div class="fixed bottom-0 left-0 right-0 p-4 bg-white border-t border-gray-100 max-w-md mx-auto z-50 rounded-t-xl">
                    <button onclick="handleNewVerify()" class="w-full bg-[#b71c1c] text-white font-bold py-3 rounded shadow-lg hover:bg-red-800 transition active:scale-95 text-lg tracking-wider">VERIFY</button>
                </div>
            </div>
        </section>

        <section id="account-sec" class="hidden animate-fade-in pb-24 bg-[#f6f8fa] min-h-screen">
            <div class="flex flex-col items-center pt-8 pb-4">
                <div class="relative mb-2"><div class="w-24 h-24 rounded-full bg-gradient-to-b from-[#4DD0E1] to-[#0288D1] p-1 shadow-md"><div class="w-full h-full rounded-full bg-white p-[2px]"><div id="profile-img-container" class="w-full h-full rounded-full bg-[#00e676] overflow-hidden flex items-center justify-center"><span id="profile-initial" class="text-4xl font-normal text-black hidden"></span><img id="acc-profile-img" src="" class="w-full h-full object-cover hidden"></div></div></div></div>
                <h2 class="text-sm font-bold text-[#1976d2] mb-1">Hi, <span id="acc-profile-name">User</span></h2>
                <div class="flex items-center gap-1 text-black font-bold text-[11px]"><span>Available Balance : <span id="acc-profile-balance">0</span> Tk</span><i class="fas fa-redo-alt text-[10px] cursor-pointer text-gray-800 hover:rotate-180 transition-transform"></i></div>
            </div>
            <div class="grid grid-cols-2 gap-3 px-4 mb-6">
                <div class="bg-white py-3 px-2 rounded-lg border border-[#2196F3] text-center shadow-sm"><h3 class="text-[#2196F3] font-bold text-base mb-1" id="acc-support-pin">...</h3><p class="text-black font-bold text-[11px]">Support Pin</p></div>
                <div class="bg-white py-3 px-2 rounded-lg border border-[#2196F3] text-center shadow-sm"><h3 class="text-[#2196F3] font-bold text-base mb-1"><span id="acc-weekly-spent">0</span> ৳</h3><p class="text-black font-bold text-[11px]">Weekly Spent</p></div>
                <div class="bg-white py-3 px-2 rounded-lg border border-[#2196F3] text-center shadow-sm"><h3 class="text-[#2196F3] font-bold text-base mb-1" id="acc-total-spent">0</h3><p class="text-black font-bold text-[11px]">Total Spent</p></div>
                <div class="bg-white py-3 px-2 rounded-lg border border-[#2196F3] text-center shadow-sm"><h3 class="text-[#2196F3] font-bold text-base mb-1" id="acc-total-orders">0</h3><p class="text-black font-bold text-[11px]">Total Order</p></div>
            </div>
            <div class="px-4 mb-4">
                <div class="bg-white shadow-sm">
                    <div class="px-4 py-3 flex items-center gap-2"><i class="fas fa-folder-open text-black text-sm"></i><h3 class="font-bold text-sm text-black">Account Information</h3></div>
                    <div class="border-t border-gray-100 p-4 space-y-4">
                        <div class="bg-white rounded-lg border border-gray-100 shadow-sm py-4 text-center"><div class="flex items-center justify-center gap-2 mb-1 text-gray-500"><i class="fas fa-redo-alt text-xs"></i><span class="text-sm font-medium">৳<span id="acc-balance-card">0.00</span></span></div><p class="font-bold text-sm text-black">Available Balance</p></div>
                        <div class="bg-white rounded-lg border border-gray-100 shadow-sm py-4 text-center"><div class="flex justify-center mb-2"><div class="relative text-[#007bff]"><i class="fas fa-certificate text-4xl"></i><i class="fas fa-check text-white absolute top-1/2 left-1/2 transform -translate-x-1/2 -translate-y-1/2 text-sm"></i></div></div><p class="font-bold text-sm text-black">Account Verified!</p></div>
                    </div>
                </div>
            </div>
            <div class="px-4 mb-6">
                <div class="bg-white shadow-sm"><div class="px-4 py-3 flex items-center gap-2"><i class="fas fa-info-circle text-black text-sm"></i><h3 class="font-bold text-sm text-black">User Information</h3></div><div class="border-t border-gray-100 p-4"><p class="text-[11px] font-bold text-black mb-2">email : <span id="acc-email" class="font-bold text-black">user@email.com</span></p><p class="text-[11px] font-bold text-black">Phone : <span id="acc-phone" class="font-bold text-black">Not Available</span></p></div></div>
                <div class="mt-4"><button onclick="handleLogout()" class="w-full p-3 bg-white shadow-sm flex items-center justify-center gap-2 hover:bg-red-50 transition duration-300"><i class="fas fa-sign-out-alt text-red-500"></i><span class="font-bold text-sm text-red-500">Log Out</span></button></div>
            </div>
        </section>
    </main>
    
    <div id="fab-container" class="fixed bottom-20 right-4 z-50">
        <div class="flex items-center gap-2">
            <div id="fab-help-text" class="bg-red-500 text-white text-xs font-semibold px-2 py-1.5 rounded-lg shadow-md transition-opacity duration-300">সাহায্য লাগবে ?</div>
            <div class="relative flex flex-col items-center">
                <div id="fab-options" class="absolute bottom-14 space-y-1.5 transition-all duration-300 ease-out transform translate-y-4 opacity-0 pointer-events-none">
                     <a id="link-whatsapp" href="#" target="_blank" class="flex items-center justify-center w-10 h-10 bg-white rounded-full shadow-lg hover:bg-gray-100 no-underline"><i class="fab fa-whatsapp text-xl text-[#25D366]"></i></a>
                     <a id="link-telegram" href="#" target="_blank" class="flex items-center justify-center w-10 h-10 bg-white rounded-full shadow-lg hover:bg-gray-100 no-underline"><i class="fab fa-telegram-plane text-xl text-[#0088cc]"></i></a>
                </div>
                <button id="fab-toggler" onclick="toggleFabMenu()" class="relative w-12 h-12 bg-red-500 rounded-full flex items-center justify-center text-white text-xl shadow-xl transition-all duration-300 ease-in-out hover:bg-red-600 focus:outline-none">
                    <i id="fab-icon-open" class="fas fa-phone-alt transition-all duration-300 transform"></i>
                    <i id="fab-icon-close" class="fas fa-times absolute transition-all duration-300 transform scale-0 opacity-0 -rotate-180"></i>
                </button>
            </div>
        </div>
    </div>

    <nav class="fixed bottom-0 w-full bg-white/95 backdrop-blur-sm border-t border-gray-100/50 py-3 px-6 shadow-[0_-5px_20px_rgba(0,0,0,0.03)] z-40 max-w-md mx-auto left-0 right-0 rounded-t-[20px]">
        <div class="flex justify-between items-center">
            <button onclick="showSec('home')" class="nav-item flex flex-col items-center gap-1.5 text-gray-700 transition-all duration-300 hover:text-sky-500 active-tab group" id="nav-home"><img src="https://img.icons8.com/?size=100&id=OXVih02dFZ53&format=png&color=000000" class="w-6 h-6 group-hover:scale-110 transition-transform duration-300" alt="Home"><span class="text-[10px] font-semibold tracking-wide">Home</span></button>
            <button onclick="showSec('tutorial')" class="nav-item flex flex-col items-center gap-1.5 text-gray-700 transition-all duration-300 hover:text-sky-500 group" id="nav-tutorial"><img src="https://img.icons8.com/?size=100&id=15979&format=png&color=000000" class="w-6 h-6 group-hover:scale-110 transition-transform duration-300" alt="Tutorial"><span class="text-[10px] font-semibold tracking-wide">Tutorial</span></button>
            <button onclick="showSec('addmoney')" class="nav-item flex flex-col items-center gap-1.5 text-gray-700 transition-all duration-300 hover:text-sky-500 group" id="nav-addmoney"><img src="https://img.icons8.com/?size=100&id=95782&format=png&color=000000" class="w-6 h-6 group-hover:scale-110 transition-transform duration-300" alt="Add Money"><span class="text-[10px] font-semibold tracking-wide">Add Money</span></button>
            <button onclick="showSec('history')" class="nav-item flex flex-col items-center gap-1.5 text-gray-700 transition-all duration-300 hover:text-sky-500 group" id="nav-history"><img src="https://img.icons8.com/?size=100&id=41117&format=png&color=000000" class="w-6 h-6 group-hover:scale-110 transition-transform duration-300" alt="History"><span class="text-[10px] font-semibold tracking-wide">History</span></button>
            <button onclick="showSec('account')" class="nav-item flex flex-col items-center gap-1.5 text-gray-700 transition-all duration-300 hover:text-sky-500 group" id="nav-account"><img src="https://img.icons8.com/?size=100&id=7820&format=png&color=000000" class="w-6 h-6 group-hover:scale-110 transition-transform duration-300" alt="Account"><span class="text-[10px] font-semibold tracking-wide">Account</span></button>
        </div>
    </nav>
</div>

<script>
    // --- 1. FIREBASE CONFIGURATION ---
    const firebaseConfig = {
        apiKey: "AIzaSyAWwsIduoa9RMl-pzBM0sYz50rpPnzs2co",
        authDomain: "top-up-bazaar-c07ec.firebaseapp.com",
        databaseURL: "https://top-up-bazaar-c07ec-default-rtdb.firebaseio.com",
        projectId: "top-up-bazaar-c07ec",
        storageBucket: "top-up-bazaar-c07ec.firebasestorage.app",
        messagingSenderId: "698979959333",
        appId: "1:698979959333:web:69dbfe89d9bea99ccaf5cc"
    };
    if (!firebase.apps.length) firebase.initializeApp(firebaseConfig);
    const db = firebase.firestore();
    const auth = firebase.auth();
    const rtdb = firebase.database();

    let currentUser = null;
    let userBalance = 0;
    let selectedProduct = null;
    let howToVideoId = "dQw4w9WgXcQ"; 

    function loadHowToVideo() { db.collection("settings").doc("videos").get().then((doc) => { if (doc.exists && doc.data().howToAddMoney) { howToVideoId = doc.data().howToAddMoney; updateVideoFrame(); } }); }
    function updateVideoFrame() { const videoFrame = document.getElementById("new-tutorial-video"); if (videoFrame) { videoFrame.src = `https://www.youtube.com/embed/${howToVideoId}`; } }
    
    // --- Load Payment Numbers (Upay Removed) ---
    function loadPaymentNumbers() {
        db.collection("admin").doc("payment").onSnapshot((doc) => {
            if (doc.exists) {
                const data = doc.data();
                window.adminPaymentNumbers = {
                    bKash: data.bKash || "Not Set",
                    Nagad: data.Nagad || "Not Set"
                };
            }
        });
    }

    let selectedGameId = null; let selectedGameName = ""; let selectedGameType = 'uid'; let gamesData = {}; let isAutoPayEnabled = false; let selectedPaymentMethod = 'wallet'; let isDirectBuy = false; let pendingDirectOrder = null;

    function loadGlobalSettings() {
        db.collection("settings").doc("general").onSnapshot((doc) => {
            if (doc.exists) {
                const data = doc.data(); const logoUrl = data.logoUrl || "https://placehold.co/150x50?text=No+Logo"; const siteName = data.siteName || "Nenox Shop"; isAutoPayEnabled = data.autoPayEnabled === true;
                document.getElementById('header-logo-img').src = logoUrl; document.getElementById('about-logo-img').src = logoUrl; document.getElementById('about-site-name').innerText = siteName;
                document.getElementById('step2-logo').src = logoUrl; document.getElementById('step2-center-logo').src = logoUrl; document.getElementById('pay-site-logo-small').src = logoUrl; document.title = siteName;
            }
        });
    }
    
    loadGlobalSettings(); loadHowToVideo(); loadPaymentNumbers(); 

    // --- 2. AUTH LOGIC ---
    function toggleAuth(view) { if(view === 'login') { document.getElementById('login-view').classList.remove('hidden'); document.getElementById('register-view').classList.add('hidden'); } else { document.getElementById('login-view').classList.add('hidden'); document.getElementById('register-view').classList.remove('hidden'); } }
    function handleRegister() { const name = document.getElementById('reg-name').value; const email = document.getElementById('reg-email').value; const pass = document.getElementById('reg-pass').value; if(!name || !email || !pass) return Swal.fire('Error', 'Please fill all fields', 'error'); Swal.showLoading(); auth.createUserWithEmailAndPassword(email, pass).then((cred) => db.collection('users').doc(cred.user.uid).set({ name: name, email: email, balance: 0, createdAt: new Date() })).then(() => { Swal.fire('Success', 'Account Created!', 'success'); showSec('home'); }).catch((err) => Swal.fire('Error', err.message, 'error')); }
    function handleLogin() { const email = document.getElementById('login-email').value; const pass = document.getElementById('login-pass').value; if(!email || !pass) return; Swal.showLoading(); auth.signInWithEmailAndPassword(email, pass).then(() => { showSec('home'); Swal.close(); }).catch((err) => Swal.fire('Login Failed', err.message, 'error')); }
    function handleGoogleLogin() { var provider = new firebase.auth.GoogleAuthProvider(); auth.signInWithPopup(provider).then((result) => { const user = result.user; const userRef = db.collection('users').doc(user.uid); userRef.get().then((docSnapshot) => { if (!docSnapshot.exists) { userRef.set({ name: user.displayName, email: user.email, balance: 0, createdAt: new Date(), photoURL: user.photoURL }); } }); showSec('home'); Swal.fire({ position: 'top-end', icon: 'success', title: 'Logged in successfully', showConfirmButton: false, timer: 1500 }); }).catch((error) => { Swal.fire('Error', error.message, 'error'); }); }
    function showAuthPage() { showSec('auth'); }
    function handleLogout() { auth.signOut().then(() => { showSec('home'); }); }

    auth.onAuthStateChanged((user) => {
        if (user) {
            currentUser = user; if(!document.getElementById('auth-sec').classList.contains('hidden')){ showSec('home'); }
            document.getElementById('header-login-container').classList.add('hidden'); document.getElementById('header-balance-container').classList.remove('hidden');
            loadUserData(user.uid); calculateUserStats(user.uid);
        } else {
            currentUser = null; document.getElementById('header-login-container').classList.remove('hidden'); document.getElementById('header-balance-container').classList.add('hidden');
            const emptyName = "-"; if(document.getElementById('acc-profile-name')) document.getElementById('acc-profile-name').innerText = emptyName; if(document.getElementById('acc-email')) document.getElementById('acc-email').innerText = emptyName;
        }
    });
    
    loadAppContent();

    function loadUserData(uid) {
        db.collection('users').doc(uid).onSnapshot((doc) => {
            if (doc.exists) {
                const data = doc.data(); userBalance = data.balance || 0; const userName = data.name || "User"; const userEmail = data.email || "";
                document.getElementById('header-balance').innerText = userBalance; document.getElementById('topup-balance-display').innerText = userBalance.toFixed(2);
                const accNameEl = document.getElementById('acc-profile-name'); const accEmailEl = document.getElementById('acc-email'); const accBalEl = document.getElementById('acc-profile-balance'); const accBalCardEl = document.getElementById('acc-balance-card'); const accPinEl = document.getElementById('acc-support-pin');
                const imgContainer = document.getElementById('acc-profile-img'); const initialContainer = document.getElementById('profile-initial');
                if(data.photoURL) { imgContainer.src = data.photoURL; imgContainer.classList.remove('hidden'); initialContainer.classList.add('hidden'); } else { imgContainer.classList.add('hidden'); initialContainer.classList.remove('hidden'); initialContainer.innerText = userName.charAt(0).toUpperCase(); }
                if(accNameEl) accNameEl.innerText = userName; if(accEmailEl) accEmailEl.innerText = userEmail; if(accBalEl) accBalEl.innerText = userBalance; if(accBalCardEl) accBalCardEl.innerText = userBalance.toFixed(2); if(accPinEl) accPinEl.innerText = uid.substring(0, 6).toUpperCase();
            }
        });
    }

    function calculateUserStats(uid) {
        const totalOrdersEl = document.getElementById('acc-total-orders'); const totalSpentEl = document.getElementById('acc-total-spent'); const weeklySpentEl = document.getElementById('acc-weekly-spent');
        if(totalOrdersEl) totalOrdersEl.innerText = "0"; if(totalSpentEl) totalSpentEl.innerText = "0"; if(weeklySpentEl) weeklySpentEl.innerText = "0";
        db.collection('orders').where('userId', '==', uid).get().then((snapshot) => {
            let totalOrders = 0; let totalSpent = 0; let weeklySpent = 0;
            const now = new Date(); const oneWeekAgo = new Date(now.getTime() - 7 * 24 * 60 * 60 * 1000);
            snapshot.forEach((doc) => {
                const data = doc.data(); totalOrders++; const status = data.status ? data.status.toLowerCase() : '';
                if (status === 'success' || status === 'complete' || status === 'completed' || status === 'pending') { const price = parseInt(data.price || 0); totalSpent += price; if (data.date) { const orderDate = data.date.toDate ? data.date.toDate() : new Date(data.date); if (orderDate >= oneWeekAgo) { weeklySpent += price; } } }
            });
            if(totalOrdersEl) totalOrdersEl.innerText = totalOrders; if(totalSpentEl) totalSpentEl.innerText = totalSpent; if(weeklySpentEl) weeklySpentEl.innerText = weeklySpent;
        });
    }

    function loadAppContent() {
        db.collection("settings").doc("general").onSnapshot((doc) => {
            if(doc.exists) {
                const data = doc.data(); if(data.notice) document.getElementById('notice-text').innerText = data.notice;
                const waNum = data.whatsapp ? String(data.whatsapp).replace(/\s/g, '') : "0"; const tgLink = data.telegram || "#"; const appLink = data.appLink || "#"; const fbLink = data.facebook || "#"; const ytLink = data.youtube || "#"; const waLink = `https://wa.me/${waNum}`;
                const promoContainer = document.getElementById('promo-banner-container'); const promoImg = document.getElementById('promo-banner-img'); const promoLink = document.getElementById('promo-banner-link');
                if(data.promoImageUrl && promoContainer && promoImg && promoLink) { promoImg.src = data.promoImageUrl; promoLink.href = data.promoImageLink || "#"; promoContainer.classList.remove('hidden'); } else if(promoContainer) { promoContainer.classList.add('hidden'); }
                document.getElementById('home-whatsapp').href = waLink; document.getElementById('home-telegram').href = tgLink;
                const homeAppBtn = document.getElementById('home-app-dl'); homeAppBtn.href = appLink;
                homeAppBtn.onclick = (e) => { if(appLink === "#") { e.preventDefault(); Swal.fire('Notice', 'App coming soon!', 'info'); } };
                document.getElementById('link-whatsapp').href = waLink; document.getElementById('link-telegram').href = tgLink;
                document.getElementById('about-facebook').href = fbLink; document.getElementById('about-youtube').href = ytLink; document.getElementById('about-telegram').href = tgLink; document.getElementById('about-whatsapp').href = waLink; document.getElementById('contact-whatsapp').href = waLink; document.getElementById('contact-telegram').href = tgLink; document.getElementById('contact-facebook').href = fbLink;
            }
        });
        loadGamesAndCategories(); loadTutorials(); loadBanners(); loadRecentOrdersGlobal(); 
    }

    async function loadGamesAndCategories() {
        const listContainer = document.getElementById('services-list'); listContainer.innerHTML = `<div class="text-center py-10"><i class="fas fa-spinner fa-spin text-sky-500"></i> Loading Games...</div>`; gamesData = {};
        try {
            const [categoriesSnap, servicesSnap] = await Promise.all([ db.collection("categories").orderBy("slot").get(), db.collection("services").get() ]);
            if (servicesSnap.empty) { listContainer.innerHTML = `<div class="col-span-3 text-center text-gray-700">No Games Found</div>`; return; }
            const categoriesMap = new Map(); categoriesSnap.forEach(doc => { categoriesMap.set(doc.id, { ...doc.data(), games: [] }); }); let uncategorizedGames = [];
            servicesSnap.forEach(doc => { const game = { id: doc.id, ...doc.data() }; gamesData[doc.id] = game; if (game.categoryId && categoriesMap.has(game.categoryId)) { categoriesMap.get(game.categoryId).games.push(game); } else { uncategorizedGames.push(game); } });
            listContainer.innerHTML = "";
            categoriesMap.forEach((category) => {
                if (category.games.length > 0) { let gamesHTML = ''; category.games.forEach(item => { gamesHTML += `<div onclick="openTopUpPage('${item.id}', '${item.name}', '${item.image}')" class="bg-white rounded-lg shadow-[0_2px_0px_0px_rgba(3,169,244,0.5)] overflow-hidden cursor-pointer transform hover:-translate-y-1 transition-all duration-300 border border-gray-100 group"><div class="w-full aspect-square bg-gray-50 overflow-hidden relative"><img src="${item.image}" class="w-full h-full object-cover group-hover:scale-110 transition-transform duration-500" onerror="this.src='https://placehold.co/200'"></div><div class="p-2 text-center bg-white border-t border-gray-50"><p class="text-xs font-bold text-gray-700 line-clamp-2 leading-tight lang-auto">${item.name}</p></div></div>`; });
                listContainer.innerHTML += `<div><h3 class="font-bold text-gray-700 text-xl mb-3 text-center lang-auto">${category.name}</h3><div class="grid grid-cols-3 gap-6 px-4">${gamesHTML}</div></div>`; }
            });
            if(uncategorizedGames.length > 0) { let gamesHTML = ''; uncategorizedGames.forEach(item => { gamesHTML += `<div onclick="openTopUpPage('${item.id}', '${item.name}', '${item.image}')" class="bg-white rounded-lg shadow-[0_2px_0px_0px_rgba(3,169,244,0.5)] overflow-hidden cursor-pointer transform hover:-translate-y-1 transition-all duration-300 border border-gray-100 group"><div class="w-full aspect-square bg-gray-50 overflow-hidden relative"><img src="${item.image}" class="w-full h-full object-cover group-hover:scale-110 transition-transform duration-500" onerror="this.src='https://placehold.co/200'"></div><div class="p-1 text-center bg-white border-t border-gray-50"><p class="text-[8px] font-bold text-gray-700 truncate">${item.name}</p></div></div>`; }); listContainer.innerHTML += `<div><h3 class="font-bold text-gray-500 text-lg border-l-4 border-gray-400 pl-2 mb-3">Others</h3><div class="grid grid-cols-3 gap-6 px-4">${gamesHTML}</div></div>`; }
        } catch (error) { console.error("Failed to load games and categories:", error); listContainer.innerHTML = `<div class="text-center text-red-500">Failed to load games.</div>`; }
    }

    function loadTutorials() { db.collection("tutorials").onSnapshot((snapshot) => { const container = document.getElementById('tutorial-list-container'); container.innerHTML = ""; if (snapshot.empty) { container.innerHTML = `<div class="text-center py-12"><div class="w-16 h-16 bg-gray-100 rounded-full flex items-center justify-center mx-auto mb-4"><i class="fas fa-video text-gray-700 text-2xl"></i></div><p class="text-gray-500 text-sm">No tutorials available yet</p></div>`; return; } snapshot.forEach(doc => { const data = doc.data(); container.innerHTML += `<div class="bg-white rounded-2xl shadow-sm border border-gray-100 overflow-hidden hover:shadow-md transition-all duration-300 group"><div class="relative" onclick="window.open('${data.link}', '_blank')"><div class="aspect-video bg-gray-100 overflow-hidden"><img src="${data.thumbnail}" class="w-full h-full object-cover group-hover:scale-105 transition-transform duration-300"></div><div class="absolute inset-0 bg-black/20 flex items-center justify-center opacity-0 group-hover:opacity-100 transition-opacity duration-300"><div class="bg-white/90 backdrop-blur-sm rounded-full w-14 h-14 flex items-center justify-center shadow-lg"><i class="fas fa-play text-sky-600 text-xl ml-1"></i></div></div><div class="absolute top-3 right-3 bg-black/60 text-white px-2 py-1 rounded-full text-xs font-medium backdrop-blur-sm"><i class="fas fa-play mr-1"></i> Tutorial</div></div><div class="p-4"><h3 class="font-semibold text-gray-800 text-sm leading-tight line-clamp-2">${data.title}</h3><div class="mt-3 flex items-center justify-between"><span class="text-xs text-gray-500"><i class="fas fa-clock mr-1"></i> Watch now</span><button onclick="window.open('${data.link}', '_blank')" class="bg-sky-50 text-sky-600 px-3 py-1.5 rounded-lg text-xs font-medium hover:bg-sky-100 transition-colors"><i class="fas fa-external-link-alt mr-1"></i> Watch</button></div></div></div>`; }); }); }

    // --- 4. TOPUP SYSTEM ---
    function openTopUpPage(gameId, gameName, gameImg) {
        selectedGameId = gameId; selectedGameName = gameName; selectedProduct = null; selectedGameType = gamesData[gameId].type || 'uid'; 
        selectPaymentOption('wallet'); isDirectBuy = false;
        document.getElementById('total-price').innerText = "0"; document.getElementById('game-player-id').value = "";
        document.getElementById('topup-game-title').innerText = gameName; document.getElementById('topup-sub-title').innerText = gameName + " "; document.getElementById('topup-game-img').src = gameImg;
        const playerSection = document.getElementById('player-id-section'); playerSection.classList.toggle('hidden', selectedGameType === 'code');
        if (selectedGameType === 'code') { document.getElementById('topup-sub-title').innerText += "(Instant Voucher)"; } else if (selectedGameType === 'auto-topup') { document.getElementById('topup-sub-title').innerText += "(Auto Delivery)"; } else { document.getElementById('topup-sub-title').innerText += "(Player ID)"; }
        const rulesBox = document.getElementById('game-rules-box'); const rulesText = document.getElementById('game-rules-text');
        if(gamesData[gameId] && gamesData[gameId].rules) { const rules = gamesData[gameId].rules.split('\n'); let rulesHtml = ""; rules.forEach(rule => { if(rule.trim()) rulesHtml += `<p class="flex items-start gap-1"><i class="far fa-dot-circle mt-1 text-[8px] text-gray-400"></i> ${rule}</p>`; }); rulesText.innerHTML = rulesHtml || gamesData[gameId].rules; rulesBox.classList.remove('hidden'); } else { rulesBox.classList.add('hidden'); }
        const uidCheckBtn = document.getElementById('uidCheckBtn'); uidCheckBtn.innerHTML = "Click to check player name"; uidCheckBtn.classList.remove("bg-black"); uidCheckBtn.classList.add("bg-gradient-to-r", "from-red-700", "to-orange-500");
        showSec('topup');
        const grid = document.getElementById('products-grid'); const loader = document.getElementById('product-loader'); grid.innerHTML = ""; loader.classList.remove('hidden');
        db.collection('services').doc(gameId).collection('products').orderBy('price', 'asc').get().then((snapshot) => {
            loader.classList.add('hidden');
            if(snapshot.empty) { grid.innerHTML = `<div class="col-span-2 text-center text-gray-700 py-4">No packages available</div>`; return; }
            snapshot.forEach(doc => {
                const prod = doc.data(); const card = document.createElement('div'); card.className = "product-card";
                card.onclick = () => selectProduct(card, prod.name, prod.price, doc.id);
                let stockBadge = "";
                const isStockOn = prod.isInStock === undefined ? true : prod.isInStock;
                if (!isStockOn) { stockBadge = `<span class="text-[9px] text-red-500 bg-red-100 px-1 rounded ml-1">Stock Out</span>`; card.classList.add('opacity-50'); }
                else if(selectedGameType === 'code' || selectedGameType === 'auto-topup') { const count = prod.stockCount || 0; if(count <= 0) stockBadge = `<span class="text-[9px] text-red-500 bg-red-100 px-1 rounded ml-1">Stock Out</span>`; }
                card.innerHTML = `<div class="flex items-center"><div class="product-bullet"></div><div><span class="text-xs font-bold text-gray-600 block">${prod.name} ${stockBadge}</span></div></div><span class="text-sky-600 font-bold text-sm">৳${prod.price}</span>`;
                grid.appendChild(card);
            });
        });
    }

    function selectProduct(element, name, price, id) { document.querySelectorAll('.product-card').forEach(el => el.classList.remove('selected')); element.classList.add('selected'); selectedProduct = { id: id, name: name, price: parseInt(price) }; document.getElementById('total-price').innerText = selectedProduct.price; }
    function selectPaymentOption(option) { selectedPaymentMethod = option; document.querySelectorAll('.pay-option-card').forEach(el => el.classList.remove('selected')); document.getElementById('opt-' + option).classList.add('selected'); }

    // --- UPDATED PROCESS ORDER FUNCTION (Includes Stock Check) ---
    async function processOrder() {
        if (!currentUser) { Swal.fire({ title: 'Login Required', text: 'Please login to purchase this item', icon: 'info', confirmButtonText: 'Login', showCancelButton: true }).then((result) => { if (result.isConfirmed) { showSec('auth'); } }); return; }
        let playerId = "N/A";
        if(selectedGameType === 'uid' || selectedGameType === 'auto-topup') { playerId = document.getElementById('game-player-id').value.trim(); if(!playerId) return Swal.fire({ icon: 'warning', title: 'Missing ID', text: 'Please enter Player ID!' }); } else { playerId = "Voucher Request"; }
        if(!selectedProduct) return Swal.fire({ icon: 'warning', title: 'Select Pack', text: 'Please select a recharge package!' });

        // --- NEW STOCK CHECK LOGIC ---
        const prodRef = db.collection('services').doc(selectedGameId).collection('products').doc(selectedProduct.id);
        const prodDoc = await prodRef.get();
        if (prodDoc.exists) {
            const prodData = prodDoc.data();
            if (prodData.isInStock === false) { return Swal.fire('Out of Stock', 'This product is currently unavailable.', 'error'); }
            if ((selectedGameType === 'code' || selectedGameType === 'auto-topup') && (prodData.stockCount === undefined || prodData.stockCount <= 0)) { return Swal.fire('Out of Stock', 'No codes available for this product.', 'error'); }
        }
        // --- END STOCK CHECK ---

        if (selectedPaymentMethod === 'instant') {
            isDirectBuy = true; currentDepositAmount = selectedProduct.price;
            pendingDirectOrder = { playerId: playerId, product: selectedProduct, gameId: selectedGameId, gameName: selectedGameName, gameType: selectedGameType };
            document.getElementById('topup-sec').classList.add('hidden'); document.getElementById('addmoney-sec').classList.remove('hidden');
            document.getElementById('am-step-1').classList.add('hidden'); document.getElementById('am-step-2').classList.remove('hidden'); document.getElementById('am-step-3').classList.add('hidden');
            window.scrollTo({ top: 0, behavior: 'instant' }); return;
        }
        if(userBalance < selectedProduct.price) return Swal.fire({ icon: 'error', title: 'Low Balance', text: 'Please Add Money first.' });
        const result = await Swal.fire({ title: 'Confirm Purchase?', html: `Item: ${selectedProduct.name}<br>Price: ৳${selectedProduct.price}`, icon: 'question', showCancelButton: true, confirmButtonColor: '#03a9f4', confirmButtonText: 'Yes, Buy Now' });
        if (result.isConfirmed) { Swal.showLoading(); if (selectedGameType === 'code') { processVoucherOrder(playerId); } else if (selectedGameType === 'auto-topup') { processAutoTopUpOrder(playerId); } else { processUidOrder(playerId); } }
    }
    
    async function processVoucherOrder(playerId) {
        const stockRef = db.collection('services').doc(selectedGameId).collection('products').doc(selectedProduct.id).collection('stock');
        const availableStockSnap = await stockRef.where('status', '==', 'available').limit(1).get();
        if (availableStockSnap.empty) { return Swal.fire('Out of Stock', 'Sorry, this product is currently unavailable.', 'error'); }
        const stockDoc = availableStockSnap.docs[0]; const voucherCode = stockDoc.data().code; const userRef = db.collection('users').doc(currentUser.uid);
        try {
            await db.runTransaction(async (t) => { const uDoc = await t.get(userRef); const newBal = uDoc.data().balance - selectedProduct.price; if(newBal < 0) throw new Error("Insufficient Balance"); t.update(userRef, { balance: newBal }); t.delete(stockDoc.ref); t.update(db.collection('services').doc(selectedGameId).collection('products').doc(selectedProduct.id), { stockCount: firebase.firestore.FieldValue.increment(-1) }); });
            await db.collection('orders').add({ userId: currentUser.uid, userName: currentUser.email, gameName: selectedGameName, gameId: selectedGameId, productName: selectedProduct.name, price: selectedProduct.price, playerId: playerId, type: 'code', status: 'success', redeemCode: voucherCode, date: new Date() });
            Swal.fire('Success', 'Voucher Purchased! Check History for Code.', 'success'); showSec('home');
        } catch (e) { Swal.fire('Error', 'Transaction Failed: ' + e.message, 'error'); }
    }

    function processUidOrder(playerId) {
        const newBalance = userBalance - selectedProduct.price;
        db.collection('users').doc(currentUser.uid).update({ balance: newBalance })
        .then(() => { return db.collection('orders').add({ userId: currentUser.uid, userName: currentUser.email, gameName: selectedGameName, gameId: selectedGameId, playerId: playerId, productName: selectedProduct.name, price: selectedProduct.price, type: 'uid', status: 'pending', date: new Date() }); })
        .then(() => { Swal.fire('Success', 'Order Placed Successfully!', 'success'); showSec('home'); })
        .catch(err => Swal.fire('Error', err.message, 'error'));
    }

    async function processAutoTopUpOrder(playerId) {
        const settingsDoc = await db.collection("settings").doc("general").get();
        if (!settingsDoc.exists) { return Swal.fire('Configuration Error', 'Admin settings not found.', 'error'); }
        const settings = settingsDoc.data(); const apiKey = settings.autoTopUpApiKey; const apiUrl = settings.autoTopUpApiUrl; const callbackUrl = settings.autoTopUpCallbackUrl;
        if (!apiKey || !apiUrl || !callbackUrl) { return Swal.fire('Configuration Error', 'Auto TopUp is not configured correctly.', 'error'); }
        const stockRef = db.collection('services').doc(selectedGameId).collection('products').doc(selectedProduct.id).collection('stock');
        const availableStockSnap = await stockRef.where('status', '==', 'available').limit(1).get();
        if (availableStockSnap.empty) { return Swal.fire('Out of Stock', 'Sorry, this product is currently unavailable.', 'error'); }
        const stockDoc = availableStockSnap.docs[0]; const uniPinCode = stockDoc.data().code;
        const userRef = db.collection('users').doc(currentUser.uid); const orderRef = db.collection('orders').doc(); 
        try {
            await db.runTransaction(async (t) => { const userDoc = await t.get(userRef); const newBalance = userDoc.data().balance - selectedProduct.price; if (newBalance < 0) throw new Error("Insufficient Balance"); t.update(userRef, { balance: newBalance }); t.delete(stockDoc.ref); t.update(db.collection('services').doc(selectedGameId).collection('products').doc(selectedProduct.id), { stockCount: firebase.firestore.FieldValue.increment(-1) }); t.set(orderRef, { userId: currentUser.uid, userName: currentUser.email, gameName: selectedGameName, gameId: selectedGameId, playerId: playerId, productName: selectedProduct.name, price: selectedProduct.price, type: 'auto-topup', status: 'pending', date: new Date(), usedUniPinCode: uniPinCode }); });
            const payload = { api: apiKey, playerid: playerId, code: uniPinCode, orderid: orderRef.id, url: callbackUrl };
            fetch(apiUrl, { method: 'POST', headers: { 'Content-Type': 'application/json' }, body: JSON.stringify(payload) }).catch(err => console.error("Auto TopUp API Error:", err)); 
            Swal.fire({ icon: 'info', title: 'Processing Order', text: 'Your top-up is being processed automatically. Please wait.', timer: 7000, timerProgressBar: true, didOpen: () => { Swal.showLoading() } });
            setTimeout(() => { orderRef.update({ status: 'success' }).then(() => { Swal.fire('Success!', 'Your Top-Up has been completed successfully.', 'success'); showSec('home'); }); }, 7000);
        } catch (e) { Swal.fire('Transaction Failed', e.message, 'error'); }
    }

    function uidCheck() { const uid = document.getElementById("game-player-id").value; const box = document.getElementById("uidCheckBtn"); if (!uid) { box.innerHTML = "Please enter a UID!"; box.classList.remove("bg-gradient-to-r", "from-red-700", "to-orange-500"); box.classList.add("bg-black"); return; } box.innerHTML = '<i class="fas fa-spinner fa-spin"></i> Name is loading...'; box.classList.remove("bg-gradient-to-r", "from-red-700", "to-orange-500"); box.classList.add("bg-black"); const apiURL = `https://bhauxinfo2.vercel.app/bhau?uid=${uid}&region=BD`; fetch(apiURL).then(res => res.json()).then(data => { let name = (data.basicInfo && data.basicInfo.nickname) || data.nickname || "Invalid UID "; box.innerHTML = name; }).catch(() => { box.innerHTML = "No nickname found!"; }); }

    function showSec(secId) {
        if ((secId === 'history' || secId === 'account' || secId === 'addmoney') && !currentUser) { showSec('auth'); return; }
        document.querySelectorAll('main section').forEach(s => s.classList.add('hidden')); document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active-tab')); document.getElementById(secId + '-sec').classList.remove('hidden'); const navBtn = document.getElementById('nav-' + secId); if(navBtn) navBtn.classList.add('active-tab');
        const noticeSection = document.getElementById('notice-section'); if(secId === 'home') { noticeSection.classList.remove('hidden'); } else { noticeSection.classList.add('hidden'); }
        if(secId === 'addmoney') { isDirectBuy = false; document.getElementById('am-step-1').classList.remove('hidden'); document.getElementById('am-step-2').classList.add('hidden'); document.getElementById('am-step-3').classList.add('hidden'); }
        if(secId === 'history') { loadHistory(); }
        window.scrollTo({ top: 0, behavior: 'instant' }); document.documentElement.scrollTop = 0; document.body.scrollTop = 0;
    }
    
    function loadHistory() { const list = document.getElementById('history-list'); if (!currentUser) return; list.innerHTML = '<div class="text-center py-4"><i class="fas fa-spinner fa-spin text-sky-500"></i> Loading History...</div>'; db.collection('orders').where('userId', '==', currentUser.uid).limit(50).get().then(snapshot => { if (snapshot.empty) { list.innerHTML = `<div class="text-center py-8 text-gray-500"><p>No orders found</p></div>`; return; } const orders = []; snapshot.forEach(doc => { orders.push(doc.data()); }); orders.sort((a, b) => { const dateA = a.date ? a.date.toDate() : new Date(0); const dateB = b.date ? b.date.toDate() : new Date(0); return dateB - dateA; }); list.innerHTML = ""; orders.forEach(d => { let statusColor = "bg-yellow-100 text-yellow-700 border-yellow-500"; let icon = "fa-clock"; const status = d.status ? d.status.toLowerCase() : 'pending'; if (status === 'success' || status === 'complete' || status === 'completed') { statusColor = "bg-green-100 text-green-700 border-green-500"; icon = "fa-check-circle"; } else if (status === 'cancel' || status === 'rejected') { statusColor = "bg-red-100 text-red-700 border-red-500"; icon = "fa-times-circle"; } let dateStr = "N/A"; if (d.date && d.date.toDate) { dateStr = d.date.toDate().toLocaleString('en-US', { month: 'short', day: 'numeric', hour: 'numeric', minute: 'numeric', hour12: true }); } let detailsInfo = d.redeemCode ? `<div class="mt-2 bg-green-50 border border-green-200 rounded p-2 text-center"><p class="text-[10px] text-green-600 font-bold uppercase">Your Voucher Code</p><p class="text-sm font-mono font-bold text-gray-800 select-all tracking-wider break-all">${d.redeemCode}</p></div>` : `<div class="mt-1 inline-block bg-gray-50 border border-gray-200 rounded px-2 py-0.5"><p class="text-[10px] text-gray-500 font-mono">ID: <span class="text-gray-700 font-bold select-all">${d.playerId}</span></p></div>`; list.innerHTML += `<div class="bg-white border border-gray-200 rounded-lg mb-3"><div class="p-3"><div class="flex justify-between items-start mb-2"><div><h4 class="font-bold text-gray-800 text-sm">${d.gameName}</h4><p class="text-xs text-gray-500">${dateStr}</p></div><div class="text-right"><p class="text-lg font-bold text-sky-600">৳${d.price}</p><span class="text-xs font-medium ${statusColor.includes('green') ? 'text-green-600' : statusColor.includes('red') ? 'text-red-600' : 'text-yellow-600'}">${d.status}</span></div></div><div class="border-t border-gray-100 pt-2 mt-2"><p class="text-xs font-medium text-gray-700 mb-1">${d.productName}</p>${detailsInfo}</div></div></div>`; }); }).catch(error => { console.error("Error loading order history: ", error); list.innerHTML = `<div class="text-center text-red-500 py-4 text-xs font-mono">Error: ${error.message}</div>`; }); }

    function loadRecentOrdersGlobal() { const container = document.getElementById('latest-orders-list'); db.collection('orders').orderBy('date', 'desc').limit(5).onSnapshot((snapshot) => { if (snapshot.empty) { container.innerHTML = '<div class="text-center text-gray-700 text-xs py-2">No recent orders</div>'; return; } container.innerHTML = ""; snapshot.forEach(doc => { const data = doc.data(); let displayName = "Customer"; if(data.userName) { displayName = data.userName.includes('@') ? data.userName.split('@')[0] : data.userName; } const firstChar = displayName.charAt(0).toUpperCase(); let statusText = (data.status || 'pending').toLowerCase(); let statusBadgeClass = 'bg-red-500 text-white'; if (['success', 'complete', 'completed'].includes(statusText)) { statusBadgeClass = 'bg-[#198754] text-white'; statusText = 'completed'; } else if (['pending', 'processing'].includes(statusText)) { statusBadgeClass = 'bg-[#ffc107] text-gray-800'; statusText = 'processing'; } container.innerHTML += `<div class="bg-white p-3 rounded-xl shadow-[0_2px_10px_rgba(0,0,0,0.03)] border border-gray-100 flex items-center justify-between"><div class="flex items-center gap-3 overflow-hidden"><div class="w-11 h-11 min-w-[44px] bg-[#0d6efd] rounded-full flex items-center justify-center text-white text-lg font-sans font-medium shadow-sm">${firstChar}</div><div class="flex flex-col truncate"><span class="text-gray-600 font-bold text-sm truncate">${displayName}</span><span class="text-gray-500 text-[11px] font-medium truncate">${data.productName} - ${data.price} ৳</span></div></div><div class="${statusBadgeClass} px-3 py-1 rounded-full text-[10px] font-bold capitalize shadow-sm whitespace-nowrap ml-2">${statusText}</div></div>`; }); }, (error) => { console.error("Error loading recent orders: ", error); container.innerHTML = `<div class="text-center text-red-500 py-4 text-xs font-mono">Could not load recent orders.</div>`; }); }

    function toggleFabMenu() { const fabContainer = document.getElementById('fab-container'); const helpText = document.getElementById('fab-help-text'); fabContainer.classList.toggle('is-open'); if (fabContainer.classList.contains('is-open')) { helpText.classList.add('opacity-0'); } else { helpText.classList.remove('opacity-0'); } }
    
    function loadBanners() { db.collection("banners").onSnapshot((snapshot) => { const wrapper = document.getElementById('slider-wrapper'); let slidesHTML = ""; snapshot.forEach((doc) => { if(doc.data().image) slidesHTML += `<img src="${doc.data().image}" class="w-full h-full object-cover flex-shrink-0">`; }); if(slidesHTML) { wrapper.innerHTML = slidesHTML; startSlider(); } }); }
    
    let sliderInterval;
    function startSlider() { clearInterval(sliderInterval); let idx = 0; const wrapper = document.getElementById('slider-wrapper'); const count = wrapper.children.length; if(count <= 1) return; sliderInterval = setInterval(() => { idx = (idx + 1) % count; wrapper.style.transform = `translateX(-${idx * 100}%)`; }, 3000); }

    // --- ADD MONEY LOGIC (Upay Removed) ---
    let currentDepositAmount = 0; let currentPaymentMethod = '';
    function goToMethodStep() { const inputVal = document.getElementById('new-amount-input').value; if (!inputVal || inputVal < 10) { return Swal.fire('Error', 'Please enter a valid amount (Min 10 Tk)', 'error'); } currentDepositAmount = parseInt(inputVal); document.getElementById('am-step-1').classList.add('hidden'); document.getElementById('am-step-2').classList.remove('hidden'); }
    function backToAmountStep() { if (isDirectBuy) { document.getElementById('am-step-2').classList.add('hidden'); document.getElementById('addmoney-sec').classList.add('hidden'); document.getElementById('topup-sec').classList.remove('hidden'); isDirectBuy = false; } else { document.getElementById('am-step-2').classList.add('hidden'); document.getElementById('am-step-1').classList.remove('hidden'); } }
    function goToPaymentPage(method, themeClass, logoUrl) { currentPaymentMethod = method; document.getElementById('pay-method-logo').src = logoUrl; document.getElementById('pay-amount-display').innerText = currentDepositAmount; document.getElementById('pay-amount-text').innerText = currentDepositAmount; document.querySelectorAll('.method-name').forEach(el => el.innerText = method); const themeCard = document.getElementById('payment-theme-card'); themeCard.className = `rounded-xl shadow-lg overflow-hidden text-white mb-6 relative transition-colors duration-300 ${themeClass}`; const num = window.adminPaymentNumbers ? (window.adminPaymentNumbers[method] || "Not Available") : "Loading..."; document.getElementById('pay-admin-number').innerText = num; document.getElementById('am-step-2').classList.add('hidden'); document.getElementById('am-step-3').classList.remove('hidden'); }
    function backToMethodStep() { document.getElementById('am-step-3').classList.add('hidden'); document.getElementById('am-step-2').classList.remove('hidden'); }
    function copyPayNumber() { const num = document.getElementById('pay-admin-number').innerText; navigator.clipboard.writeText(num); Swal.fire({ toast: true, icon: 'success', title: 'Number Copied!', position: 'top', showConfirmButton: false, timer: 1500 }); }
    function handleNewVerify() { const trxId = document.getElementById('pay-trx-input').value.trim(); if (!trxId) { return Swal.fire('Required', 'Please enter the Transaction ID.', 'warning'); } if (isAutoPayEnabled) { verifyAutoPayment(trxId, currentDepositAmount); } else { if (isDirectBuy) { Swal.fire('Notice', 'Manual verification is not supported for Instant Buy.', 'warning'); } else { submitDeposit(trxId, currentDepositAmount); } } }
    function submitDeposit(trxId, amount) { Swal.showLoading(); db.collection('deposits').add({ userId: currentUser.uid, amount: parseInt(amount), trxId: trxId.toUpperCase(), status: 'pending', date: new Date(), userEmail: currentUser.email, method: currentPaymentMethod }).then(() => { Swal.fire({ title: 'Request Submitted', text: 'Your deposit request has been sent.', icon: 'success', confirmButtonColor: '#d33' }).then(() => { showSec('home'); }); }).catch(err => Swal.fire('Error', err.message, 'error')); }

    async function verifyAutoPayment(trxId, amount) {
        trxId = trxId.toUpperCase();
        Swal.fire({ title: 'Verifying...', text: 'Please wait while we check your transaction.', didOpen: () => { Swal.showLoading() } });
        const existingTrxCheck = await db.collection('deposits').where('trxId', '==', trxId).limit(1).get();
        if (!existingTrxCheck.empty) { return Swal.fire('Used', 'This Transaction ID has already been used.', 'error'); }
        rtdb.ref('XNXANIKPAY').orderByChild('txid').equalTo(trxId).once('value', async (snapshot) => {
            if (!snapshot.exists()) { return Swal.fire('Not Found', 'Transaction ID not found.', 'error'); }
            let paymentData = null; snapshot.forEach((childSnapshot) => { paymentData = childSnapshot.val(); });
            if (parseInt(paymentData.amount) !== parseInt(amount)) { return Swal.fire('Mismatch', `Amount mismatch!`, 'error'); }
            const amountToAdd = parseInt(paymentData.amount); const userRef = db.collection('users').doc(currentUser.uid);
            try {
                await db.runTransaction(async (transaction) => { const userDoc = await transaction.get(userRef); const newBalance = (userDoc.data().balance || 0) + amountToAdd; transaction.update(userRef, { balance: newBalance }); });
                await db.collection('deposits').add({ userId: currentUser.uid, userEmail: currentUser.email, amount: amountToAdd, trxId: trxId, status: 'approved', method: currentPaymentMethod + ' (Auto)', date: new Date() });
                if (isDirectBuy && pendingDirectOrder) {
                    userBalance += amountToAdd; 
                    Swal.fire({ title: 'Payment Verified!', text: 'Processing your order now...', icon: 'success', timer: 2000, showConfirmButton: false });
                    if (pendingDirectOrder.gameType === 'code') { await processVoucherOrder(pendingDirectOrder.playerId); } else if (pendingDirectOrder.gameType === 'auto-topup') { await processAutoTopUpOrder(pendingDirectOrder.playerId); } else { processUidOrder(pendingDirectOrder.playerId); }
                    isDirectBuy = false; pendingDirectOrder = null;
                } else { Swal.fire('Success', `Successfully added ৳${amountToAdd} to your wallet.`, 'success').then(() => showSec('home')); }
            } catch (error) { console.error("Auto payment error:", error); Swal.fire('Failed', 'System error.', 'error'); }
        });
    }
</script>
</body>
</html>
