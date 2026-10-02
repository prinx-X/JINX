<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>JINX - Next Gen Social</title>
    <link rel="preconnect" href="https://googleapis.com">
    <link rel="preconnect" href="https://gstatic.com" crossorigin>
    <link href="https://googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;600;700&display=swap" rel="stylesheet">
    
    <style>
        :root {
            --bg-pure: #09090b;
            --glass-bg: rgba(18, 18, 22, 0.85);
            --glass-border: rgba(255, 255, 255, 0.08);
            --accent-pink: #ff0050;
            --accent-cyan: #00f2fe;
            --text-main: #f4f4f5;
            --text-muted: #a1a1aa;
            --wa-green: #25d366;
        }

        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Plus Jakarta Sans', sans-serif;
            -webkit-tap-highlight-color: transparent;
        }

        body {
            background-color: #030303;
            color: var(--text-main);
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            overflow: hidden;
        }

        .jinx-frame {
            width: 100%;
            max-width: 430px;
            height: 100vh;
            background: var(--bg-pure);
            position: relative;
            display: flex;
            flex-direction: column;
            overflow: hidden;
            box-shadow: 0 0 40px rgba(0, 0, 0, 0.8);
        }

        @media (min-width: 450px) {
            .jinx-frame {
                height: 880px;
                border-radius: 40px;
                border: 8px solid #1c1c1e;
            }
        }

        /* Branding Header */
        .jinx-header {
            position: absolute;
            top: 0;
            width: 100%;
            height: 64px;
            background: var(--glass-bg);
            backdrop-filter: blur(20px);
            -webkit-backdrop-filter: blur(20px);
            border-bottom: 1px solid var(--glass-border);
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 0 20px;
            z-index: 1000;
        }

        .brand-logo {
            font-size: 22px;
            font-weight: 800;
            letter-spacing: -0.5px;
            text-transform: uppercase;
            cursor: pointer;
        }

        .brand-logo span {
            color: var(--accent-pink);
            text-shadow: 0 0 10px rgba(255, 0, 80, 0.4);
        }

        .header-actions { display: flex; gap: 16px; }

        .yt-icon-btn {
            background: none;
            border: none;
            color: var(--text-main);
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
        }

        /* App Viewports */
        .app-panel {
            position: absolute;
            width: 100%;
            height: 100%;
            top: 0;
            left: 0;
            opacity: 0;
            visibility: hidden;
            transition: opacity 0.2s ease;
        }

        .app-panel.active {
            opacity: 1;
            visibility: visible;
        }

        /* --- TIKTOK STYLE FULL-SCREEN PLAYER --- */
        .tiktok-scroller { height: 100%; background: #000; position: relative; }
        
        .v-post {
            height: 100%;
            width: 100%;
            position: relative;
        }

        .v-post video {
            width: 100%;
            height: 100%;
            object-fit: cover;
        }

        .v-overlay {
            position: absolute;
            bottom: 100px;
            left: 16px;
            right: 80px;
            z-index: 10;
            text-shadow: 0 2px 4px rgba(0,0,0,0.6);
        }

        .v-overlay .user-tag { font-weight: 700; font-size: 16px; margin-bottom: 6px; color: #fff; }
        .v-overlay .caption { font-size: 14px; color: #e4e4e7; line-height: 1.4; font-weight: 300; }

        /* --- SEARCH EXPLORE PANEL --- */
        .search-explore-view {
            height: 100%;
            padding: 80px 16px 90px 16px;
            overflow-y: auto;
            background: #09090b;
            display: flex;
            flex-direction: column;
            gap: 20px;
        }
        .search-explore-view::-webkit-scrollbar { display: none; }

        .search-bar-wrapper {
            display: flex;
            background: #18181b;
            border: 1px solid var(--glass-border);
            border-radius: 24px;
            padding: 12px 16px;
            align-items: center;
            gap: 10px;
        }

        .search-input { flex: 1; background: none; border: none; outline: none; color: white; font-size: 14px; }

        .tags-wrapper { display: flex; flex-wrap: wrap; gap: 8px; margin-top: 8px; }
        
        .tag {
            background: rgba(255, 255, 255, 0.05);
            border: 1px solid var(--glass-border);
            padding: 8px 14px;
            border-radius: 20px;
            font-size: 13px;
            font-weight: 600;
            cursor: pointer;
        }
        .tag:hover { background: var(--accent-pink); }

        .video-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; margin-top: 12px; }

        .grid-card {
            background: #121214;
            border-radius: 12px;
            overflow: hidden;
            border: 1px solid var(--glass-border);
            cursor: pointer;
        }

        .card-thumb-placeholder { width: 100%; height: 120px; display: flex; align-items: center; justify-content: center; font-size: 24px; }
        .card-info { padding: 10px; }
        .card-title { font-size: 12px; font-weight: 600; display: -webkit-box; -webkit-line-clamp: 2; -webkit-box-orient: vertical; overflow: hidden; }

        /* --- WHATSAPP TEXT STYLE CHATS --- */
        .wa-chat-view { height: 100%; display: flex; flex-direction: column; background: #0c0a09; padding-top: 64px; padding-bottom: 74px; }
        .chat-user-header { padding: 12px 16px; background: rgba(20, 20, 25, 0.8); border-bottom: 1px solid var(--glass-border); display: flex; align-items: center; gap: 12px; }
        .wa-avatar { width: 40px; height: 40px; border-radius: 50%; background: linear-gradient(45deg, var(--accent-pink), var(--accent-cyan)); }
        .messages-box { flex: 1; padding: 20px 16px; overflow-y: auto; display: flex; flex-direction: column; gap: 12px; }
        .messages-box::-webkit-scrollbar { display: none; }
        .bubble { max-width: 75%; padding: 10px 14px; font-size: 14px; border-radius: 18px; }
        .bubble.incoming { background: #18181b; color: var(--text-main); align-self: flex-start; border-bottom-left-radius: 4px; }
        .bubble.outgoing { background: var(--accent-pink); color: white; align-self: flex-end; border-bottom-right-radius: 4px; }

        /* --- PROFILE VIEW TAB --- */
        .profile-view { height: 100%; padding: 80px 20px; background: #09090b; display: flex; flex-direction: column; align-items: center; gap: 20px; }
        .big-avatar { width: 90px; height: 90px; border-radius: 50%; background: linear-gradient(45deg, var(--accent-pink), var(--accent-cyan)); border: 3px solid var(--glass-border); }
        .profile-name { font-size: 20px; font-weight: 700; }
        .auth-prompt-btn { background: var(--accent-pink); color: white; border: none; padding: 12px 30px; border-radius: 25px; font-weight: 700; cursor: pointer; margin-top: 10px; width: 100%; text-align: center; }

        /* --- AUTH REGISTRATION POPUP MODAL --- */
        .auth-overlay {
            position: absolute;
            top: 0; left: 0; width: 100%; height: 100%;
            background: rgba(0, 0, 0, 0.9);
            z-index: 3000;
            display: none;
            justify-content: center;
            align-items: center;
            padding: 20px;
        }
        .auth-overlay.open { display: flex; }

        .auth-card {
            background: #121214;
            width: 100%;
            max-width: 360px;
            border-radius: 24px;
            border: 1px solid var(--glass-border);
            padding: 24px;
            display: flex;
            flex-direction: column;
            gap: 16px;
        }

        .auth-tabs { display: flex; border-bottom: 1px solid var(--glass-border); padding-bottom: 10px; gap: 20px; }
        .auth-tab-btn { background: none; border: none; color: var(--text-muted); font-weight: 700; font-size: 15px; cursor: pointer; padding-bottom: 4px; }
        .auth-tab-btn.active { color: white; border-bottom: 2px solid var(--accent-pink); }

        .auth-input-group { display: flex; flex-direction: column; gap: 6px; }
        .auth-input-group label { font-size: 12px; color: var(--text-muted); font-weight: 600; }
        .auth-input { background: #1c1c1f; border: 1px solid var(--glass-border); padding: 12px 16px; border-radius: 12px; color: white; outline: none; font-size: 14px; }

        .search-modal {
            position: absolute;
            top: -100%; left: 0; width: 100%; height: 100%;
            background: #09090b; z-index: 2000; padding: 80px 20px;
            transition: top 0.3s ease-in-out; display: flex; flex-direction: column; gap: 20px;
        }
        .search-modal.open { top: 0; }

        /* Bottom Glass Tab Navbar */
        .jinx-navbar {
            position: absolute;
            bottom: 0; width: 100%; height: 74px;
            background: var(--glass-bg); backdrop-filter: blur(30px); border-top: 1px solid var(--glass-border);
            display: flex; justify-content: space-around; align-items: center; padding: 0 10px 12px 10px; z-index: 1000;
        }

 
