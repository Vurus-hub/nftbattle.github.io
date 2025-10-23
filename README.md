<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>NFT Battle | Рулетка NFT подарков</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }
        
        :root {
            --primary: #8a2be2;
            --secondary: #4b0082;
            --accent: #ff00ff;
            --dark: #121212;
            --darker: #0a0a0a;
            --light: #f5f5f5;
            --card-bg: rgba(30, 30, 46, 0.8);
            --success: #00ff88;
            --danger: #ff4444;
            --wheel-border: #1a1a3a;
        }
        
        body {
            background: linear-gradient(135deg, var(--darker) 0%, var(--dark) 50%, var(--secondary) 100%);
            color: var(--light);
            min-height: 100vh;
            overflow-x: hidden;
        }
        
        .container {
            max-width: 1200px;
            margin: 0 auto;
            padding: 20px;
        }
        
        header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 20px 0;
            margin-bottom: 30px;
            border-bottom: 2px solid rgba(255, 255, 255, 0.1);
        }
        
        .logo {
            display: flex;
            align-items: center;
            gap: 15px;
        }
        
        .logo i {
            font-size: 2.5rem;
            color: var(--accent);
        }
        
        .logo h1 {
            font-size: 2rem;
            background: linear-gradient(90deg, var(--accent), var(--primary));
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            text-shadow: 0 0 10px rgba(255, 0, 255, 0.3);
        }
        
        .user-info {
            display: flex;
            align-items: center;
            gap: 15px;
            background: rgba(255, 255, 255, 0.1);
            padding: 10px 20px;
            border-radius: 50px;
            backdrop-filter: blur(10px);
        }
        
        .user-avatar {
            width: 40px;
            height: 40px;
            border-radius: 50%;
            background: linear-gradient(45deg, var(--primary), var(--accent));
            display: flex;
            align-items: center;
            justify-content: center;
        }
        
        .balance {
            display: flex;
            align-items: center;
            gap: 5px;
            font-weight: bold;
        }
        
        .balance i {
            color: gold;
        }
        
        .add-balance-btn {
            background: rgba(255, 255, 255, 0.1);
            border: none;
            color: white;
            width: 30px;
            height: 30px;
            border-radius: 50%;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 0.8rem;
            transition: all 0.3s ease;
        }
        
        .add-balance-btn:hover {
            background: rgba(255, 255, 255, 0.2);
            transform: scale(1.1);
        }
        
        .tabs {
            display: flex;
            gap: 10px;
            margin-bottom: 30px;
            background: rgba(255, 255, 255, 0.05);
            border-radius: 15px;
            padding: 10px;
            backdrop-filter: blur(10px);
        }
        
        .tab {
            padding: 12px 25px;
            border-radius: 10px;
            cursor: pointer;
            transition: all 0.3s ease;
            font-weight: bold;
            display: flex;
            align-items: center;
            gap: 8px;
        }
        
        .tab.active {
            background: var(--primary);
            box-shadow: 0 0 15px rgba(138, 43, 226, 0.5);
        }
        
        .tab-content {
            display: none;
        }
        
        .tab-content.active {
            display: block;
            animation: fadeIn 0.5s;
        }
        
        .main-content {
            display: grid;
            grid-template-columns: 1fr 300px;
            gap: 30px;
        }
        
        .cases-section {
            margin-bottom: 40px;
        }
        
        .section-title {
            font-size: 1.8rem;
            margin-bottom: 20px;
            display: flex;
            align-items: center;
            gap: 10px;
        }
        
        .section-title i {
            color: var(--accent);
        }
        
        .cases-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(180px, 1fr));
            gap: 20px;
        }
        
        .case-card {
            background: var(--card-bg);
            border-radius: 15px;
            overflow: hidden;
            transition: all 0.3s ease;
            cursor: pointer;
            border: 2px solid transparent;
            backdrop-filter: blur(10px);
        }
        
        .case-card:hover {
            transform: translateY(-10px);
            border-color: var(--accent);
            box-shadow: 0 10px 20px rgba(255, 0, 255, 0.2);
        }
        
        .case-card.active {
            border-color: var(--accent);
            box-shadow: 0 0 20px rgba(255, 0, 255, 0.5);
        }
        
        .case-image {
            height: 150px;
            background: linear-gradient(45deg, var(--primary), var(--secondary));
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 3rem;
            overflow: hidden;
            position: relative;
        }
        
        .case-image .nft-visual {
            width: 100%;
            height: 100%;
            display: flex;
            align-items: center;
            justify-content: center;
        }
        
        .case-info {
            padding: 15px;
        }
        
        .case-name {
            font-weight: bold;
            margin-bottom: 5px;
            font-size: 1.1rem;
        }
        
        .case-price {
            color: gold;
            font-weight: bold;
            display: flex;
            align-items: center;
            gap: 5px;
        }
        
        /* Стили для пополнения баланса */
        .balance-modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.8);
            z-index: 1000;
            align-items: center;
            justify-content: center;
            backdrop-filter: blur(5px);
        }
        
        .balance-modal.active {
            display: flex;
        }
        
        .balance-content {
            background: linear-gradient(135deg, var(--card-bg), var(--darker));
            border-radius: 20px;
            padding: 30px;
            max-width: 500px;
            width: 90%;
            border: 2px solid var(--accent);
            box-shadow: 0 0 30px rgba(255, 0, 255, 0.5);
        }
        
        .balance-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 25px;
        }
        
        .balance-title {
            font-size: 1.8rem;
            color: var(--accent);
        }
        
        .close-balance {
            background: none;
            border: none;
            color: white;
            font-size: 1.5rem;
            cursor: pointer;
        }
        
        .balance-options {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 15px;
            margin-bottom: 25px;
        }
        
        .balance-option {
            background: rgba(255, 255, 255, 0.1);
            border: 2px solid transparent;
            border-radius: 15px;
            padding: 20px;
            text-align: center;
            cursor: pointer;
            transition: all 0.3s ease;
        }
        
        .balance-option:hover {
            background: rgba(255, 255, 255, 0.2);
            transform: translateY(-5px);
        }
        
        .balance-option.selected {
            border-color: var(--accent);
            background: rgba(138, 43, 226, 0.2);
            box-shadow: 0 0 15px rgba(255, 0, 255, 0.5);
        }
        
        .balance-amount {
            font-size: 1.5rem;
            font-weight: bold;
            color: gold;
            margin-bottom: 5px;
        }
        
        .balance-desc {
            font-size: 0.9rem;
            opacity: 0.8;
        }
        
        .add-balance-confirm {
            background: linear-gradient(90deg, var(--success), #00cc66);
            color: white;
            border: none;
            padding: 15px 30px;
            font-size: 1.1rem;
            border-radius: 50px;
            cursor: pointer;
            transition: all 0.3s ease;
            box-shadow: 0 5px 15px rgba(0, 255, 136, 0.4);
            font-weight: bold;
            width: 100%;
            margin-top: 10px;
        }
        
        .add-balance-confirm:hover {
            transform: translateY(-3px);
            box-shadow: 0 8px 25px rgba(0, 255, 136, 0.6);
        }
        
        .add-balance-confirm:disabled {
            background: #555;
            cursor: not-allowed;
            transform: none;
            box-shadow: none;
        }
        
        /* Анимация открытия кейса */
        .case-opening-container {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.95);
            z-index: 1000;
            display: none;
            align-items: center;
            justify-content: center;
            flex-direction: column;
        }
        
        .case-opening-container.active {
            display: flex;
        }
        
        .case-view {
            width: 600px;
            height: 400px;
            background: linear-gradient(135deg, #1a1a2e, #16213e);
            border-radius: 20px;
            position: relative;
            overflow: hidden;
            border: 3px solid var(--accent);
            box-shadow: 0 0 50px rgba(255, 0, 255, 0.5);
        }
        
        .case-lid {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 40%;
            background: linear-gradient(135deg, #8a2be2, #4b0082);
            transform-origin: top;
            transition: transform 1s ease-in-out;
            z-index: 2;
            border-bottom: 3px solid var(--accent);
        }
        
        .case-lid.open {
            transform: rotateX(-120deg);
        }
        
        .case-content {
            position: absolute;
            bottom: 0;
            left: 0;
            width: 100%;
            height: 60%;
            background: linear-gradient(135deg, #0f3460, #1a1a2e);
            display: flex;
            align-items: center;
            justify-content: center;
            overflow: hidden;
        }
        
        .item-reveal {
            position: absolute;
            bottom: -100px;
            opacity: 0;
            transition: all 1.5s ease-out;
        }
        
        .item-reveal.show {
            bottom: 50px;
            opacity: 1;
            transform: scale(1.2);
        }
        
        .revealed-item {
            text-align: center;
            color: white;
            z-index: 3;
        }
        
        .revealed-item-image {
            width: 150px;
            height: 150px;
            margin: 0 auto 20px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 4rem;
            animation: glow 2s infinite alternate;
        }
        
        .revealed-item-name {
            font-size: 2rem;
            font-weight: bold;
            margin-bottom: 10px;
        }
        
        .revealed-item-rarity {
            font-size: 1.2rem;
            padding: 5px 15px;
            border-radius: 20px;
            display: inline-block;
        }
        
        .open-case-btn {
            background: linear-gradient(90deg, var(--primary), var(--accent));
            color: white;
            border: none;
            padding: 15px 40px;
            font-size: 1.2rem;
            border-radius: 50px;
            cursor: pointer;
            transition: all 0.3s ease;
            box-shadow: 0 5px 15px rgba(138, 43, 226, 0.4);
            font-weight: bold;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 10px;
            margin: 20px auto;
        }
        
        .open-case-btn:hover {
            transform: translateY(-3px);
            box-shadow: 0 8px 25px rgba(138, 43, 226, 0.6);
        }
        
        .open-case-btn:disabled {
            background: #555;
            cursor: not-allowed;
            transform: none;
            box-shadow: none;
        }
        
        .close-case {
            position: absolute;
            top: 20px;
            right: 20px;
            background: rgba(255, 255, 255, 0.1);
            border: none;
            color: white;
            width: 40px;
            height: 40px;
            border-radius: 50%;
            font-size: 1.2rem;
            cursor: pointer;
            z-index: 10;
        }
        
        .wheel-section {
            background: var(--card-bg);
            border-radius: 20px;
            padding: 30px;
            text-align: center;
            margin-bottom: 30px;
            backdrop-filter: blur(10px);
            border: 1px solid rgba(255, 255, 255, 0.1);
        }
        
        .wheel-grid {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 10px;
            margin-bottom: 30px;
            max-width: 400px;
            margin-left: auto;
            margin-right: auto;
        }
        
        .wheel-item {
            aspect-ratio: 1;
            background: rgba(255, 255, 255, 0.1);
            border-radius: 10px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 2rem;
            transition: all 0.3s ease;
            overflow: hidden;
            position: relative;
        }
        
        .wheel-item .nft-visual {
            width: 100%;
            height: 100%;
            display: flex;
            align-items: center;
            justify-content: center;
        }
        
        .wheel-item.active {
            transform: scale(1.1);
            z-index: 2;
            box-shadow: 0 0 15px rgba(255, 0, 255, 0.7);
        }
        
        .wheel-item.winner {
            animation: winnerPulse 1s infinite;
            border: 3px solid gold;
        }
        
        .spin-btn {
            background: linear-gradient(90deg, var(--primary), var(--accent));
            color: white;
            border: none;
            padding: 15px 40px;
            font-size: 1.2rem;
            border-radius: 50px;
            cursor: pointer;
            transition: all 0.3s ease;
            box-shadow: 0 5px 15px rgba(138, 43, 226, 0.4);
            font-weight: bold;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 10px;
            margin: 0 auto;
        }
        
        .spin-btn:hover {
            transform: translateY(-3px);
            box-shadow: 0 8px 25px rgba(138, 43, 226, 0.6);
        }
        
        .spin-btn:disabled {
            background: #555;
            cursor: not-allowed;
            transform: none;
            box-shadow: none;
        }
        
        .selected-case {
            background: var(--card-bg);
            border-radius: 15px;
            padding: 20px;
            margin-bottom: 20px;
            backdrop-filter: blur(10px);
            border: 1px solid rgba(255, 255, 255, 0.1);
        }
        
        .selected-case-header {
            display: flex;
            align-items: center;
            gap: 10px;
            margin-bottom: 15px;
        }
        
        .selected-case-header i {
            color: var(--accent);
            font-size: 1.5rem;
        }
        
        .case-preview {
            display: flex;
            align-items: center;
            gap: 15px;
        }
        
        .case-preview-image {
            width: 60px;
            height: 60px;
            border-radius: 10px;
            background: linear-gradient(45deg, var(--primary), var(--accent));
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 1.5rem;
            overflow: hidden;
        }
        
        .case-preview-image .nft-visual {
            width: 100%;
            height: 100%;
            display: flex;
            align-items: center;
            justify-content: center;
        }
        
        .case-preview-info {
            flex-grow: 1;
        }
        
        .inventory {
            background: var(--card-bg);
            border-radius: 15px;
            padding: 20px;
            backdrop-filter: blur(10px);
            border: 1px solid rgba(255, 255, 255, 0.1);
        }
        
        .inventory-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 15px;
        }
        
        .inventory-items {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 10px;
        }
        
        .inventory-item {
            background: rgba(255, 255, 255, 0.1);
            border-radius: 10px;
            height: 80px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 1.5rem;
            transition: all 0.3s ease;
            overflow: hidden;
            position: relative;
            cursor: pointer;
        }
        
        .inventory-item .nft-visual {
            width: 100%;
            height: 100%;
            display: flex;
            align-items: center;
            justify-content: center;
        }
        
        .inventory-item:hover {
            background: rgba(255, 255, 255, 0.2);
            transform: scale(1.05);
        }
        
        .inventory-item.selected {
            border: 2px solid var(--accent);
            box-shadow: 0 0 10px rgba(255, 0, 255, 0.5);
        }
        
        /* Стили для редактирования профиля */
        .profile-edit-modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.8);
            z-index: 1000;
            align-items: center;
            justify-content: center;
            backdrop-filter: blur(5px);
        }
        
        .profile-edit-modal.active {
            display: flex;
        }
        
        .profile-edit-content {
            background: linear-gradient(135deg, var(--card-bg), var(--darker));
            border-radius: 20px;
            padding: 30px;
            max-width: 500px;
            width: 90%;
            border: 2px solid var(--accent);
            box-shadow: 0 0 30px rgba(255, 0, 255, 0.5);
        }
        
        .profile-edit-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 25px;
        }
        
        .profile-edit-title {
            font-size: 1.8rem;
            color: var(--accent);
        }
        
        .close-edit {
            background: none;
            border: none;
            color: white;
            font-size: 1.5rem;
            cursor: pointer;
        }
        
        .form-group {
            margin-bottom: 20px;
        }
        
        .form-label {
            display: block;
            margin-bottom: 8px;
            font-weight: bold;
        }
        
        .form-input {
            width: 100%;
            padding: 12px 15px;
            border-radius: 10px;
            border: 2px solid rgba(255, 255, 255, 0.2);
            background: rgba(255, 255, 255, 0.1);
            color: white;
            font-size: 1rem;
            transition: all 0.3s ease;
        }
        
        .form-input:focus {
            outline: none;
            border-color: var(--accent);
            box-shadow: 0 0 10px rgba(255, 0, 255, 0.3);
        }
        
        .avatar-options {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 10px;
            margin-top: 10px;
        }
        
        .avatar-option {
            width: 60px;
            height: 60px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 1.5rem;
            cursor: pointer;
            transition: all 0.3s ease;
            border: 2px solid transparent;
        }
        
        .avatar-option:hover {
            transform: scale(1.1);
        }
        
        .avatar-option.selected {
            border-color: var(--accent);
            box-shadow: 0 0 15px rgba(255, 0, 255, 0.5);
        }
        
        .save-profile-btn {
            background: linear-gradient(90deg, var(--primary), var(--accent));
            color: white;
            border: none;
            padding: 15px 30px;
            font-size: 1.1rem;
            border-radius: 50px;
            cursor: pointer;
            transition: all 0.3s ease;
            box-shadow: 0 5px 15px rgba(138, 43, 226, 0.4);
            font-weight: bold;
            width: 100%;
            margin-top: 10px;
        }
        
        .save-profile-btn:hover {
            transform: translateY(-3px);
            box-shadow: 0 8px 25px rgba(138, 43, 226, 0.6);
        }
        
        .edit-profile-btn {
            background: rgba(255, 255, 255, 0.1);
            color: white;
            border: none;
            padding: 8px 15px;
            border-radius: 20px;
            cursor: pointer;
            transition: all 0.3s ease;
            font-size: 0.9rem;
            display: flex;
            align-items: center;
            gap: 5px;
        }
        
        .edit-profile-btn:hover {
            background: rgba(255, 255, 255, 0.2);
        }
        
        /* Стили для профиля */
        .profile-section {
            background: var(--card-bg);
            border-radius: 20px;
            padding: 30px;
            margin-bottom: 30px;
            backdrop-filter: blur(10px);
            border: 1px solid rgba(255, 255, 255, 0.1);
        }
        
        .profile-header {
            display: flex;
            align-items: center;
            gap: 20px;
            margin-bottom: 30px;
        }
        
        .profile-avatar {
            width: 100px;
            height: 100px;
            border-radius: 50%;
            background: linear-gradient(45deg, var(--primary), var(--accent));
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 2.5rem;
        }
        
        .profile-info {
            flex-grow: 1;
        }
        
        .profile-stats {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 20px;
            margin-bottom: 30px;
        }
        
        .stat-card {
            background: rgba(255, 255, 255, 0.05);
            border-radius: 15px;
            padding: 20px;
            text-align: center;
        }
        
        .stat-value {
            font-size: 2rem;
            font-weight: bold;
            margin-bottom: 5px;
        }
        
        .stat-label {
            opacity: 0.8;
        }
        
        /* Стили для улучшения */
        .upgrade-game-section {
            background: var(--card-bg);
            border-radius: 20px;
            padding: 30px;
            margin-bottom: 30px;
            backdrop-filter: blur(10px);
            border: 1px solid rgba(255, 255, 255, 0.1);
        }
        
        .upgrade-game-container {
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 30px;
        }
        
        .selection-area {
            display: flex;
            gap: 30px;
            width: 100%;
            justify-content: center;
        }
        
        .selection-box {
            background: rgba(255, 255, 255, 0.05);
            border-radius: 15px;
            padding: 20px;
            width: 45%;
            text-align: center;
        }
        
        .selection-title {
            font-size: 1.3rem;
            margin-bottom: 15px;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 10px;
        }
        
        .selected-nft {
            background: rgba(255, 255, 255, 0.1);
            border-radius: 10px;
            padding: 20px;
            margin-top: 15px;
            min-height: 120px;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            gap: 10px;
        }
        
        .selected-nft-image {
            width: 60px;
            height: 60px;
            border-radius: 10px;
            overflow: hidden;
            display: flex;
            align-items: center;
            justify-content: center;
        }
        
        .selected-nft-image .nft-visual {
            width: 100%;
            height: 100%;
            display: flex;
            align-items: center;
            justify-content: center;
        }
        
        .desired-nft-grid {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 10px;
            margin-top: 15px;
            max-height: 300px;
            overflow-y: auto;
            padding: 10px;
        }
        
        .desired-nft-item {
            background: rgba(255, 255, 255, 0.1);
            border-radius: 10px;
            padding: 10px;
            cursor: pointer;
            transition: all 0.3s ease;
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 5px;
        }
        
        .desired-nft-item:hover {
            background: rgba(255, 255, 255, 0.2);
            transform: scale(1.05);
        }
        
        .desired-nft-item.selected {
            background: var(--primary);
            box-shadow: 0 0 10px rgba(138, 43, 226, 0.5);
        }
        
        .desired-nft-image {
            width: 50px;
            height: 50px;
            border-radius: 8px;
            overflow: hidden;
            display: flex;
            align-items: center;
            justify-content: center;
        }
        
        .desired-nft-image .nft-visual {
            width: 100%;
            height: 100%;
            display: flex;
            align-items: center;
            justify-content: center;
        }
        
        .desired-nft-name {
            font-size: 0.8rem;
            text-align: center;
            font-weight: bold;
        }
        
        .desired-nft-rarity {
            font-size: 0.7rem;
            opacity: 0.8;
        }
        
        .game-wheel {
            position: relative;
            width: 300px;
            height: 300px;
            margin: 20px auto;
        }
        
        .wheel-circle {
            width: 100%;
            height: 100%;
            border-radius: 50%;
            background: conic-gradient(
                var(--danger) 0deg 180deg,
                var(--success) 180deg 360deg
            );
            position: relative;
            overflow: hidden;
            box-shadow: 0 0 20px rgba(0, 0, 0, 0.5);
            border: 5px solid var(--wheel-border);
        }
        
        .wheel-pointer {
            position: absolute;
            top: -20px;
            left: 50%;
            transform: translateX(-50%);
            width: 30px;
            height: 40px;
            background: var(--accent);
            clip-path: polygon(50% 100%, 0 0, 100% 0);
            z-index: 10;
            filter: drop-shadow(0 0 5px rgba(255, 0, 255, 0.7));
        }
        
        .wheel-center {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            width: 60px;
            height: 60px;
            background: var(--dark);
            border-radius: 50%;
            z-index: 5;
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: bold;
            border: 3px solid var(--accent);
        }
        
        .game-info {
            text-align: center;
            margin-bottom: 20px;
        }
        
        .chance-indicator {
            display: inline-block;
            padding: 8px 15px;
            background: rgba(255, 255, 255, 0.1);
            border-radius: 20px;
            font-weight: bold;
        }
        
        .game-upgrade-btn {
            background: linear-gradient(90deg, var(--primary), var(--accent));
            color: white;
            border: none;
            padding: 15px 40px;
            font-size: 1.2rem;
            border-radius: 50px;
            cursor: pointer;
            transition: all 0.3s ease;
            box-shadow: 0 5px 15px rgba(138, 43, 226, 0.4);
            font-weight: bold;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 10px;
            margin: 0 auto;
        }
        
        .game-upgrade-btn:hover {
            transform: translateY(-3px);
            box-shadow: 0 8px 25px rgba(138, 43, 226, 0.6);
        }
        
        .game-upgrade-btn:disabled {
            background: #555;
            cursor: not-allowed;
            transform: none;
            box-shadow: none;
        }
        
        /* Стили для NFT визуализации */
        .nft-visual {
            position: relative;
            border-radius: 10px;
            display: flex;
            align-items: center;
            justify-content: center;
            width: 100%;
            height: 100%;
        }
        
        .nft-shape {
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 2rem;
            filter: drop-shadow(0 0 5px rgba(0, 0, 0, 0.5));
        }
        
        .diamond {
            color: #00ffff;
            text-shadow: 0 0 10px #00ffff;
        }
        
        .gem {
            color: #ff00ff;
            text-shadow: 0 0 10px #ff00ff;
        }
        
        .crystal {
            color: #ffffff;
            text-shadow: 0 0 10px #ffffff;
        }
        
        .coin {
            color: gold;
            text-shadow: 0 0 10px gold;
        }
        
        .artifact {
            color: #ff4444;
            text-shadow: 0 0 10px #ff4444;
        }
        
        .orb {
            color: #00ff88;
            text-shadow: 0 0 10px #00ff88;
        }
        
        .crown {
            color: #ffaa00;
            text-shadow: 0 0 10px #ffaa00;
        }
        
        .star {
            color: #ffff00;
            text-shadow: 0 0 10px #ffff00;
        }
        
        .medal {
            color: #cccccc;
            text-shadow: 0 0 10px #cccccc;
        }
        
        .token {
            color: #8a2be2;
            text-shadow: 0 0 10px #8a2be2;
        }
        
        @keyframes fadeIn {
            from { opacity: 0; }
            to { opacity: 1; }
        }
        
        @keyframes winnerPulse {
            0% { transform: scale(1); box-shadow: 0 0 10px gold; }
            50% { transform: scale(1.1); box-shadow: 0 0 20px gold; }
            100% { transform: scale(1); box-shadow: 0 0 10px gold; }
        }
        
        @keyframes glow {
            0% { filter: drop-shadow(0 0 5px currentColor); }
            100% { filter: drop-shadow(0 0 20px currentColor); }
        }
        
        @keyframes wheelSpin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(1080deg); }
        }
        
        footer {
            text-align: center;
            padding: 30px 0;
            margin-top: 50px;
            border-top: 1px solid rgba(255, 255, 255, 0.1);
            font-size: 0.9rem;
            opacity: 0.7;
        }
        
        @media (max-width: 900px) {
            .main-content {
                grid-template-columns: 1fr;
            }
            
            .cases-grid {
                grid-template-columns: repeat(auto-fill, minmax(150px, 1fr));
            }
            
            .case-view {
                width: 90%;
                height: 300px;
            }
            
            .revealed-item-image {
                width: 100px;
                height: 100px;
                font-size: 3rem;
            }
            
            .revealed-item-name {
                font-size: 1.5rem;
            }
            
            .selection-area {
                flex-direction: column;
                align-items: center;
            }
            
            .selection-box {
                width: 100%;
                max-width: 400px;
            }
            
            .balance-options {
                grid-template-columns: 1fr;
            }
        }
        
        @media (max-width: 600px) {
            header {
                flex-direction: column;
                gap: 20px;
            }
            
            .cases-grid {
                grid-template-columns: repeat(auto-fill, minmax(130px, 1fr));
            }
            
            .avatar-options {
                grid-template-columns: repeat(3, 1fr);
            }
            
            .profile-header {
                flex-direction: column;
                text-align: center;
            }
            
            .profile-stats {
                grid-template-columns: 1fr;
            }
            
            .game-wheel {
                width: 250px;
                height: 250px;
            }
            
            .desired-nft-grid {
                grid-template-columns: repeat(2, 1fr);
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <header>
            <div class="logo">
                <i class="fas fa-gem"></i>
                <h1>NFT Battle</h1>
            </div>
            <div class="user-info">
                <div class="user-avatar" id="userAvatar">
                    <i class="fas fa-user"></i>
                </div>
                <div class="user-details">
                    <div class="username" id="username">@username</div>
                    <div class="balance">
                        <i class="fas fa-star" style="color: gold;"></i> 
                        <span id="userBalance">50</span>
                        <button class="add-balance-btn" id="addBalanceBtn" title="Пополнить баланс">
                            <i class="fas fa-plus"></i>
                        </button>
                    </div>
                </div>
            </div>
        </header>
        
        <div class="tabs">
            <div class="tab active" data-tab="cases">
                <i class="fas fa-gift"></i> Кейсы
            </div>
            <div class="tab" data-tab="upgrade">
                <i class="fas fa-magic"></i> Улучшение
            </div>
            <div class="tab" data-tab="upgrade-game">
                <i class="fas fa-sync-alt"></i> Upgrade
            </div>
            <div class="tab" data-tab="profile">
                <i class="fas fa-user"></i> Профиль
            </div>
        </div>
        
        <!-- Вкладка Кейсы -->
        <div class="tab-content active" id="cases-tab">
            <div class="main-content">
                <div class="left-column">
                    <section class="cases-section">
                        <h2 class="section-title"><i class="fas fa-gift"></i> Кейсы NFT</h2>
                        <div class="cases-grid" id="casesGrid">
                            <!-- Кейсы будут добавлены через JavaScript -->
                        </div>
                    </section>
                    
                    <section class="wheel-section">
                        <h2 class="section-title"><i class="fas fa-dice"></i> Крути рулетку</h2>
                        <div class="wheel-grid" id="wheelGrid">
                            <!-- Элементы рулетки будут добавлены через JavaScript -->
                        </div>
                        <button class="spin-btn" id="spinBtn">
                            <i class="fas fa-play"></i> Крутить за 100 <i class="fas fa-star" style="color: gold;"></i>
                        </button>
                    </section>
                </div>
                
                <div class="right-column">
                    <div class="selected-case">
                        <div class="selected-case-header">
                            <i class="fas fa-star"></i>
                            <h3>Выбранный кейс</h3>
                        </div>
                        <div class="case-preview">
                            <div class="case-preview-image" id="selectedCaseImage">
                                <i class="fas fa-cube"></i>
                            </div>
                            <div class="case-preview-info">
                                <div class="case-name" id="selectedCaseName">Не выбран</div>
                                <div class="case-price"><i class="fas fa-star" style="color: gold;"></i> <span id="selectedCasePrice">0</span></div>
                            </div>
                        </div>
                        <button class="open-case-btn" id="openCaseBtn" disabled>
                            <i class="fas fa-lock-open"></i> Открыть кейс
                        </button>
                    </div>
                    
                    <div class="inventory">
                        <div class="inventory-header">
                            <h3>Мои NFT</h3>
                            <span id="inventoryCount">0/50</span>
                        </div>
                        <div class="inventory-items" id="inventoryItems">
                            <!-- NFT будут добавлены через JavaScript -->
                        </div>
                    </div>
                </div>
            </div>
        </div>
        
        <!-- Вкладка Улучшение -->
        <div class="tab-content" id="upgrade-tab">
            <div class="upgrade-section">
                <div class="upgrade-header">
                    <h2 class="section-title"><i class="fas fa-magic"></i> Улучшение подарка</h2>
                    <div class="upgrade-price">
                        <i class="fas fa-star" style="color: gold;"></i> 1,499 звезд
                    </div>
                    <p>Создайте уникальный NFT подарок с абсолютно случайным результатом</p>
                </div>
                
                <div class="upgrade-steps">
                    <div class="upgrade-step">
                        <h3 class="step-title"><i class="fas fa-random"></i> Абсолютный рандом</h3>
                        <p>Все параметры вашего NFT будут выбраны случайным образом. Вы не можете повлиять на результат!</p>
                    </div>
                </div>
                
                <button class="open-case-btn" id="randomUpgradeBtn">
                    <i class="fas fa-bolt"></i> Улучшить за 1,499 <i class="fas fa-star" style="color: gold;"></i>
                </button>
            </div>
        </div>
        
        <!-- Вкладка Upgrade -->
        <div class="tab-content" id="upgrade-game-tab">
            <div class="upgrade-game-section">
                <h2 class="section-title"><i class="fas fa-sync-alt"></i> Upgrade NFT</h2>
                
                <div class="upgrade-game-container">
                    <div class="selection-area">
                        <div class="selection-box">
                            <h3 class="selection-title"><i class="fas fa-arrow-up"></i> Ваш NFT</h3>
                            <p>Выберите NFT для улучшения</p>
                            <div class="selected-nft" id="selectedNftForUpgrade">
                                <i class="fas fa-question-circle" style="font-size: 2rem; opacity: 0.5;"></i>
                                <div>Не выбран</div>
                            </div>
                            <div class="inventory-items" id="upgradeInventoryItems">
                                <!-- NFT для улучшения будут добавлены через JavaScript -->
                            </div>
                        </div>
                        
                        <div class="selection-box">
                            <h3 class="selection-title"><i class="fas fa-gift"></i> Желаемый NFT</h3>
                            <p>Выберите NFT, который хотите получить</p>
                            <div class="selected-nft" id="desiredNft">
                                <i class="fas fa-question-circle" style="font-size: 2rem; opacity: 0.5;"></i>
                                <div>Не выбран</div>
                            </div>
                            <div class="desired-nft-grid" id="desiredNftGrid">
                                <!-- Желаемые NFT будут добавлены через JavaScript -->
                            </div>
                        </div>
                    </div>
                    
                    <div class="game-info">
                        <div class="chance-indicator">Шанс успеха: 50%</div>
                        <p>Если стрелка остановится в зеленой зоне - вы получите желаемый NFT</p>
                        <p>Если стрелка остановится в серой зоне - вы потеряете свой NFT</p>
                    </div>
                    
                    <div class="game-wheel">
                        <div class="wheel-pointer"></div>
                        <div class="wheel-circle" id="wheelCircle">
                            <div class="wheel-center">50%</div>
                        </div>
                    </div>
                    
                    <button class="game-upgrade-btn" id="gameUpgradeBtn" disabled>
                        <i class="fas fa-sync-alt"></i> Улучшить
                    </button>
                </div>
            </div>
        </div>
        
        <!-- Вкладка Профиль -->
        <div class="tab-content" id="profile-tab">
            <div class="profile-section">
                <div class="profile-header">
                    <div class="profile-avatar" id="profileAvatar">
                        <i class="fas fa-user"></i>
                    </div>
                    <div class="profile-info">
                        <h2 id="profileUsername">@username</h2>
                        <p id="profileBio">Любитель NFT и коллекционных предметов</p>
                        <div class="balance">
                            <i class="fas fa-star" style="color: gold;"></i> 
                            <span id="profileBalance">50</span> звезд
                            <button class="add-balance-btn" id="addBalanceBtnProfile" title="Пополнить баланс">
                                <i class="fas fa-plus"></i>
                            </button>
                        </div>
                        <button class="edit-profile-btn" id="editProfileBtn">
                            <i class="fas fa-edit"></i> Редактировать профиль
                        </button>
                    </div>
                </div>
                
                <div class="profile-stats">
                    <div class="stat-card">
                        <div class="stat-value" id="statOpenedCases">0</div>
                        <div class="stat-label">Открыто кейсов</div>
                    </div>
                    <div class="stat-card">
                        <div class="stat-value" id="statTotalNFT">0</div>
                        <div class="stat-label">Всего NFT</div>
                    </div>
                    <div class="stat-card">
                        <div class="stat-value" id="statUpgradedNFT">0</div>
                        <div class="stat-label">Улучшено NFT</div>
                    </div>
                    <div class="stat-card">
                        <div class="stat-value" id="statLegendaryNFT">0</div>
                        <div class="stat-label">Легендарных NFT</div>
                    </div>
                </div>
                
                <div class="profile-collection">
                    <h3 class="section-title"><i class="fas fa-gem"></i> Моя коллекция NFT</h3>
                    <div class="inventory-items" id="profileInventoryItems">
                        <!-- NFT коллекции будут добавлены через JavaScript -->
                    </div>
                </div>
            </div>
        </div>
        
        <!-- Модальное окно открытия кейса -->
        <div class="case-opening-container" id="caseOpeningContainer">
            <button class="close-case" id="closeCase">
                <i class="fas fa-times"></i>
            </button>
            <div class="case-view">
                <div class="case-lid" id="caseLid"></div>
                <div class="case-content">
                    <div class="item-reveal" id="itemReveal">
                        <div class="revealed-item">
                            <div class="revealed-item-image" id="revealedItemImage">
                                <i class="fas fa-gem"></i>
                            </div>
                            <div class="revealed-item-name" id="revealedItemName">NFT Item</div>
                            <div class="revealed-item-rarity" id="revealedItemRarity">Редкий</div>
                        </div>
                    </div>
                </div>
            </div>
            <button class="open-case-btn" id="claimCaseItem">
                <i class="fas fa-check"></i> Забрать предмет
            </button>
        </div>
        
        <!-- Модальное окно редактирования профиля -->
        <div class="profile-edit-modal" id="profileEditModal">
            <div class="profile-edit-content">
                <div class="profile-edit-header">
                    <h2 class="profile-edit-title">Редактирование профиля</h2>
                    <button class="close-edit" id="closeEdit">
                        <i class="fas fa-times"></i>
                    </button>
                </div>
                
                <div class="form-group">
                    <label class="form-label">Имя пользователя</label>
                    <input type="text" class="form-input" id="editUsername" placeholder="Введите имя пользователя">
                </div>
                
                <div class="form-group">
                    <label class="form-label">Аватар</label>
                    <div class="avatar-options" id="avatarOptions">
                        <!-- Опции аватаров будут добавлены через JavaScript -->
                    </div>
                </div>
                
                <div class="form-group">
                    <label class="form-label">О себе</label>
                    <textarea class="form-input" id="editBio" placeholder="Расскажите о себе..." rows="3"></textarea>
                </div>
                
                <button class="save-profile-btn" id="saveProfileBtn">
                    <i class="fas fa-save"></i> Сохранить изменения
                </button>
            </div>
        </div>
        
        <!-- Модальное окно пополнения баланса -->
        <div class="balance-modal" id="balanceModal">
            <div class="balance-content">
                <div class="balance-header">
                    <h2 class="balance-title">Пополнение баланса</h2>
                    <button class="close-balance" id="closeBalance">
                        <i class="fas fa-times"></i>
                    </button>
                </div>
                
                <div class="balance-options" id="balanceOptions">
                    <div class="balance-option" data-amount="100">
                        <div class="balance-amount">100 звезд</div>
                        <div class="balance-desc">Базовый набор</div>
                    </div>
                    <div class="balance-option" data-amount="200">
                        <div class="balance-amount">200 звезд</div>
                        <div class="balance-desc">Популярный выбор</div>
                    </div>
                    <div class="balance-option" data-amount="500">
                        <div class="balance-amount">500 звезд</div>
                        <div class="balance-desc">Выгодный пакет</div>
                    </div>
                    <div class="balance-option" data-amount="1000">
                        <div class="balance-amount">1000 звезд</div>
                        <div class="balance-desc">Максимальный набор</div>
                    </div>
                </div>
                
                <button class="add-balance-confirm" id="addBalanceConfirm" disabled>
                    <i class="fas fa-star"></i> Пополнить баланс
                </button>
            </div>
        </div>
        
        <footer>
            <p>NFT Battle &copy; 2023 | Все права защищены</p>
            <p>Свяжитесь с поддержкой: @nftbattle_support</p>
        </footer>
    </div>

    <script>
        document.addEventListener('DOMContentLoaded', function() {
            // Типы NFT с разными формами и цветами
            const nftTypes = [
                { name: "Алмаз", class: "diamond", icon: "fas fa-gem", color: "#00ffff", bg: "linear-gradient(135deg, #001122, #004466)" },
                { name: "Рубин", class: "gem", icon: "fas fa-gem", color: "#ff00ff", bg: "linear-gradient(135deg, #330011, #660022)" },
                { name: "Кристалл", class: "crystal", icon: "fas fa-cube", color: "#ffffff", bg: "linear-gradient(135deg, #222233, #444466)" },
                { name: "Золотая монета", class: "coin", icon: "fas fa-coins", color: "gold", bg: "linear-gradient(135deg, #332200, #664400)" },
                { name: "Артефакт", class: "artifact", icon: "fas fa-monument", color: "#ff4444", bg: "linear-gradient(135deg, #330000, #660000)" },
                { name: "Магический шар", class: "orb", icon: "fas fa-circle", color: "#00ff88", bg: "linear-gradient(135deg, #003322, #006644)" },
                { name: "Корона", class: "crown", icon: "fas fa-crown", color: "#ffaa00", bg: "linear-gradient(135deg, #332200, #665500)" },
                { name: "Звезда", class: "star", icon: "fas fa-star", color: "#ffff00", bg: "linear-gradient(135deg, #333300, #666600)" },
                { name: "Медаль", class: "medal", icon: "fas fa-medal", color: "#cccccc", bg: "linear-gradient(135deg, #222222, #444444)" },
                { name: "Токен", class: "token", icon: "fas fa-circle", color: "#8a2be2", bg: "linear-gradient(135deg, #220033, #440066)" }
            ];
            
            // Редкости NFT
            const rarities = [
                { name: "Обычный", color: "#aaaaaa", multiplier: 1 },
                { name: "Редкий", color: "#36a2eb", multiplier: 2 },
                { name: "Эпический", color: "#9966ff", multiplier: 3 },
                { name: "Легендарный", color: "#ffaa00", multiplier: 5 },
                { name: "Мифический", color: "#ff00ff", multiplier: 10 }
            ];
            
            // Данные кейсов
            const cases = [
                { id: 1, name: "Базовый кейс", price: 50, rarity: "Обычный" },
                { id: 2, name: "Редкий кейс", price: 150, rarity: "Редкий" },
                { id: 3, name: "Эпический кейс", price: 300, rarity: "Эпический" },
                { id: 4, name: "Легендарный кейс", price: 500, rarity: "Легендарный" },
                { id: 5, name: "Арт кейс", price: 200, rarity: "Редкий" },
                { id: 6, name: "Музыкальный кейс", price: 250, rarity: "Эпический" },
                { id: 7, name: "Игровой кейс", price: 350, rarity: "Легендарный" },
                { id: 8, name: "Коллекционный кейс", price: 400, rarity: "Эпический" }
            ];
            
            // Опции аватаров
            const avatarOptions = [
                { icon: "fas fa-user", color: "#8a2be2", bg: "linear-gradient(45deg, #8a2be2, #4b0082)" },
                { icon: "fas fa-robot", color: "#00ff88", bg: "linear-gradient(45deg, #00ff88, #00cc66)" },
                { icon: "fas fa-crown", color: "gold", bg: "linear-gradient(45deg, gold, #ffaa00)" },
                { icon: "fas fa-dragon", color: "#ff4444", bg: "linear-gradient(45deg, #ff4444, #cc0000)" },
                { icon: "fas fa-wizard", color: "#9966ff", bg: "linear-gradient(45deg, #9966ff, #6633cc)" },
                { icon: "fas fa-knight", color: "#cccccc", bg: "linear-gradient(45deg, #cccccc, #999999)" },
                { icon: "fas fa-gem", color: "#00ffff", bg: "linear-gradient(45deg, #00ffff, #0088cc)" },
                { icon: "fas fa-mask", color: "#ff00ff", bg: "linear-gradient(45deg, #ff00ff, #cc00cc)" }
            ];
            
            // Элементы DOM
            const casesGrid = document.getElementById('casesGrid');
            const openCaseBtn = document.getElementById('openCaseBtn');
            const caseOpeningContainer = document.getElementById('caseOpeningContainer');
            const closeCase = document.getElementById('closeCase');
            const caseLid = document.getElementById('caseLid');
            const itemReveal = document.getElementById('itemReveal');
            const revealedItemImage = document.getElementById('revealedItemImage');
            const revealedItemName = document.getElementById('revealedItemName');
            const revealedItemRarity = document.getElementById('revealedItemRarity');
            const claimCaseItem = document.getElementById('claimCaseItem');
            const profileEditModal = document.getElementById('profileEditModal');
            const editProfileBtn = document.getElementById('editProfileBtn');
            const closeEdit = document.getElementById('closeEdit');
            const saveProfileBtn = document.getElementById('saveProfileBtn');
            const editUsername = document.getElementById('editUsername');
            const editBio = document.getElementById('editBio');
            const avatarOptionsContainer = document.getElementById('avatarOptions');
            const userAvatar = document.getElementById('userAvatar');
            const profileAvatar = document.getElementById('profileAvatar');
            const username = document.getElementById('username');
            const profileUsername = document.getElementById('profileUsername');
            const profileBio = document.getElementById('profileBio');
            const spinBtn = document.getElementById('spinBtn');
            const wheelGrid = document.getElementById('wheelGrid');
            const randomUpgradeBtn = document.getElementById('randomUpgradeBtn');
            const tabs = document.querySelectorAll('.tab');
            const tabContents = document.querySelectorAll('.tab-content');
            const gameUpgradeBtn = document.getElementById('gameUpgradeBtn');
            const selectedNftForUpgrade = document.getElementById('selectedNftForUpgrade');
            const desiredNft = document.getElementById('desiredNft');
            const upgradeInventoryItems = document.getElementById('upgradeInventoryItems');
            const desiredNftGrid = document.getElementById('desiredNftGrid');
            const wheelCircle = document.getElementById('wheelCircle');
            const addBalanceBtn = document.getElementById('addBalanceBtn');
            const addBalanceBtnProfile = document.getElementById('addBalanceBtnProfile');
            const balanceModal = document.getElementById('balanceModal');
            const closeBalance = document.getElementById('closeBalance');
            const balanceOptions = document.getElementById('balanceOptions');
            const addBalanceConfirm = document.getElementById('addBalanceConfirm');
            
            // Переменные состояния
            let selectedCase = null;
            let userNFTs = [];
            let userStats = {
                openedCases: 0,
                totalNFT: 0,
                upgradedNFT: 0,
                legendaryNFT: 0
            };
            let userStars = 50; // Начальный баланс 50 звезд
            let userProfile = {
                username: "@username",
                avatar: 0,
                bio: "Любитель NFT и коллекционных предметов",
                joinDate: "2023"
            };
            let currentCasePrize = null;
            let isSpinning = false;
            let wheelItems = [];
            let selectedNFTForUpgrade = null;
            let desiredNFT = null;
            let specialNFTs = [];
            let selectedBalanceAmount = 0;
            
            // Инициализация вкладок
            tabs.forEach(tab => {
                tab.addEventListener('click', () => {
                    const tabId = tab.getAttribute('data-tab');
                    
                    // Убираем активный класс у всех вкладок и контента
                    tabs.forEach(t => t.classList.remove('active'));
                    tabContents.forEach(tc => tc.classList.remove('active'));
                    
                    // Добавляем активный класс к выбранной вкладке и контенту
                    tab.classList.add('active');
                    document.getElementById(`${tabId}-tab`).classList.add('active');
                    
                    // Обновляем коллекцию в профиле
                    if (tabId === 'profile') {
                        updateProfileCollection();
                    }
                    
                    // Обновляем инвентарь для улучшения
                    if (tabId === 'upgrade-game') {
                        updateUpgradeInventory();
                        updateDesiredNFTs();
                    }
                });
            });
            
            // Функция для создания визуализации NFT
            function createNFTVisual(nftType, rarity) {
                const visual = document.createElement('div');
                visual.className = 'nft-visual';
                visual.style.background = nftType.bg;
                
                const shape = document.createElement('div');
                shape.className = `nft-shape ${nftType.class}`;
                shape.innerHTML = `<i class="${nftType.icon}"></i>`;
                shape.style.color = nftType.color;
                shape.style.fontSize = `${1.5 * rarity.multiplier}rem`;
                
                visual.appendChild(shape);
                return visual;
            }
            
            // Функция для генерации случайного NFT
            function generateRandomNFT() {
                const nftType = nftTypes[Math.floor(Math.random() * nftTypes.length)];
                const rarity = rarities[Math.floor(Math.random() * rarities.length)];
                
                return {
                    id: Date.now() + Math.floor(Math.random() * 1000),
                    name: `${rarity.name} ${nftType.name}`,
                    type: nftType,
                    rarity: rarity,
                    description: `Уникальный ${nftType.name.toLowerCase()} ${rarity.name.toLowerCase()} качества`,
                    value: Math.floor(50 * rarity.multiplier * (0.8 + Math.random() * 0.4))
                };
            }
            
            // Генерация специальных NFT для улучшения
            function generateSpecialNFTs() {
                const specialNFTs = [];
                
                // Создаем 12 уникальных специальных NFT для улучшения
                for (let i = 0; i < 12; i++) {
                    const nftType = nftTypes[Math.floor(Math.random() * nftTypes.length)];
                    // Для желаемых NFT используем более высокие редкости
                    const rarityIndex = Math.floor(Math.random() * 2) + 2; // Эпический или Легендарный
                    const rarity = rarities[rarityIndex];
                    
                    specialNFTs.push({
                        id: Date.now() + Math.floor(Math.random() * 1000) + i,
                        name: `${rarity.name} ${nftType.name}`,
                        type: nftType,
                        rarity: rarity,
                        description: `Эксклюзивный ${nftType.name.toLowerCase()} ${rarity.name.toLowerCase()} качества`,
                        value: Math.floor(100 * rarity.multiplier * (0.9 + Math.random() * 0.2)),
                        isSpecial: true
                    });
                }
                
                return specialNFTs;
            }
            
            // Инициализация аватаров
            function initAvatarOptions() {
                avatarOptionsContainer.innerHTML = '';
                avatarOptions.forEach((avatar, index) => {
                    const avatarOption = document.createElement('div');
                    avatarOption.className = `avatar-option ${index === userProfile.avatar ? 'selected' : ''}`;
                    avatarOption.style.background = avatar.bg;
                    avatarOption.innerHTML = `<i class="${avatar.icon}" style="color: ${avatar.color};"></i>`;
                    
                    avatarOption.addEventListener('click', () => {
                        document.querySelectorAll('.avatar-option').forEach(opt => {
                            opt.classList.remove('selected');
                        });
                        avatarOption.classList.add('selected');
                        userProfile.avatar = index;
                    });
                    
                    avatarOptionsContainer.appendChild(avatarOption);
                });
            }
            
            // Обновление профиля
            function updateProfile() {
                username.textContent = userProfile.username;
                profileUsername.textContent = userProfile.username;
                profileBio.textContent = userProfile.bio;
                
                // Обновляем аватар
                const avatar = avatarOptions[userProfile.avatar];
                userAvatar.style.background = avatar.bg;
                userAvatar.innerHTML = `<i class="${avatar.icon}" style="color: ${avatar.color};"></i>`;
                profileAvatar.style.background = avatar.bg;
                profileAvatar.innerHTML = `<i class="${avatar.icon}" style="color: ${avatar.color};"></i>`;
            }
            
            // Открытие модального окна редактирования профиля
            editProfileBtn.addEventListener('click', () => {
                editUsername.value = userProfile.username.replace('@', '');
                editBio.value = userProfile.bio;
                initAvatarOptions();
                profileEditModal.classList.add('active');
            });
            
            // Закрытие модального окна редактирования профиля
            closeEdit.addEventListener('click', () => {
                profileEditModal.classList.remove('active');
            });
            
            // Сохранение профиля
            saveProfileBtn.addEventListener('click', () => {
                const newUsername = editUsername.value.trim();
                if (newUsername) {
                    userProfile.username = '@' + newUsername;
                    userProfile.bio = editBio.value;
                    updateProfile();
                    profileEditModal.classList.remove('active');
                    
                    // Показываем уведомление об успешном сохранении
                    alert('Профиль успешно обновлен!');
                } else {
                    alert('Пожалуйста, введите имя пользователя');
                }
            });
            
            // Открытие модального окна пополнения баланса
            addBalanceBtn.addEventListener('click', () => {
                balanceModal.classList.add('active');
            });
            
            addBalanceBtnProfile.addEventListener('click', () => {
                balanceModal.classList.add('active');
            });
            
            // Закрытие модального окна пополнения баланса
            closeBalance.addEventListener('click', () => {
                balanceModal.classList.remove('active');
                selectedBalanceAmount = 0;
                addBalanceConfirm.disabled = true;
                document.querySelectorAll('.balance-option').forEach(option => {
                    option.classList.remove('selected');
                });
            });
            
            // Выбор суммы пополнения
            balanceOptions.querySelectorAll('.balance-option').forEach(option => {
                option.addEventListener('click', () => {
                    document.querySelectorAll('.balance-option').forEach(opt => {
                        opt.classList.remove('selected');
                    });
                    option.classList.add('selected');
                    selectedBalanceAmount = parseInt(option.getAttribute('data-amount'));
                    addBalanceConfirm.disabled = false;
                });
            });
            
            // Подтверждение пополнения баланса
            addBalanceConfirm.addEventListener('click', () => {
                if (selectedBalanceAmount > 0) {
                    userStars += selectedBalanceAmount;
                    updateBalance();
                    balanceModal.classList.remove('active');
                    selectedBalanceAmount = 0;
                    addBalanceConfirm.disabled = true;
                    document.querySelectorAll('.balance-option').forEach(option => {
                        option.classList.remove('selected');
                    });
                    alert(`Баланс успешно пополнен на ${selectedBalanceAmount} звезд!`);
                }
            });
            
            // Анимация открытия кейса
            function openCaseAnimation(prize) {
                currentCasePrize = prize;
                caseOpeningContainer.classList.add('active');
                
                // Сбрасываем анимацию
                caseLid.classList.remove('open');
                itemReveal.classList.remove('show');
                
                // Задаем стили для выпавшего предмета
                revealedItemImage.innerHTML = '';
                const prizeVisual = createNFTVisual(prize.type, prize.rarity);
                revealedItemImage.appendChild(prizeVisual);
                revealedItemName.textContent = prize.name;
                revealedItemRarity.textContent = prize.rarity.name;
                revealedItemRarity.style.background = prize.rarity.color + '40';
                revealedItemRarity.style.color = prize.rarity.color;
                
                // Запускаем анимацию
                setTimeout(() => {
                    caseLid.classList.add('open');
                }, 1000);
                
                setTimeout(() => {
                    itemReveal.classList.add('show');
                }, 2000);
            }
            
            // Обработчик открытия кейса
            openCaseBtn.addEventListener('click', () => {
                if (!selectedCase || userStars < selectedCase.price) return;
                
                userStars -= selectedCase.price;
                updateBalance();
                
                // Генерируем случайный приз
                const prize = generateRandomNFT();
                openCaseAnimation(prize);
            });
            
            // Обработчик закрытия окна открытия кейса
            closeCase.addEventListener('click', () => {
                caseOpeningContainer.classList.remove('active');
            });
            
            // Обработчик получения предмета из кейса
            claimCaseItem.addEventListener('click', () => {
                if (currentCasePrize) {
                    if (addNFTToInventory(currentCasePrize)) {
                        userStats.openedCases++;
                        updateStats();
                        caseOpeningContainer.classList.remove('active');
                        currentCasePrize = null;
                    } else {
                        alert('Инвентарь полон! Освободите место для нового NFT.');
                    }
                }
            });
            
            // Создание кейсов
            function initCases() {
                casesGrid.innerHTML = '';
                cases.forEach((caseItem, index) => {
                    const caseCard = document.createElement('div');
                    caseCard.className = 'case-card';
                    
                    // Создаем визуализацию для кейса
                    const nftType = nftTypes[index % nftTypes.length];
                    const rarity = rarities.find(r => r.name === caseItem.rarity) || rarities[0];
                    const caseVisual = createNFTVisual(nftType, rarity);
                    
                    caseCard.innerHTML = `
                        <div class="case-image">
                            ${caseVisual.outerHTML}
                        </div>
                        <div class="case-info">
                            <div class="case-name">${caseItem.name}</div>
                            <div class="case-price"><i class="fas fa-star" style="color: gold;"></i> ${caseItem.price}</div>
                        </div>
                    `;
                    
                    caseCard.addEventListener('click', () => {
                        document.querySelectorAll('.case-card').forEach(card => {
                            card.classList.remove('active');
                        });
                        caseCard.classList.add('active');
                        selectedCase = caseItem;
                        
                        // Обновляем превью выбранного кейса
                        document.getElementById('selectedCaseName').textContent = caseItem.name;
                        document.getElementById('selectedCasePrice').textContent = caseItem.price;
                        
                        // Обновляем изображение превью
                        const previewImage = document.getElementById('selectedCaseImage');
                        previewImage.innerHTML = '';
                        const previewVisual = createNFTVisual(nftType, rarity);
                        previewImage.appendChild(previewVisual);
                        
                        // Активируем кнопку открытия кейса
                        openCaseBtn.disabled = userStars < selectedCase.price;
                    });
                    
                    casesGrid.appendChild(caseCard);
                });
                
                // Автовыбор первого кейса
                if (casesGrid.firstChild) {
                    casesGrid.firstChild.click();
                }
            }
            
            // Инициализация рулетки
            function initWheel() {
                wheelGrid.innerHTML = '';
                wheelItems = [];
                
                // Создаем 12 элементов для рулетки
                for (let i = 0; i < 12; i++) {
                    const prize = generateRandomNFT();
                    const wheelItem = document.createElement('div');
                    wheelItem.className = 'wheel-item';
                    
                    const prizeVisual = createNFTVisual(prize.type, prize.rarity);
                    wheelItem.appendChild(prizeVisual);
                    
                    wheelGrid.appendChild(wheelItem);
                    wheelItems.push(wheelItem);
                }
            }
            
            // Кручение рулетки
            spinBtn.addEventListener('click', spinWheel);
            
            function spinWheel() {
                if (isSpinning) return;
                
                if (userStars < 100) {
                    alert('Недостаточно звезд для кручения рулетки!');
                    return;
                }
                
                isSpinning = true;
                spinBtn.disabled = true;
                
                // Вычитаем звезды за кручение
                userStars -= 100;
                updateBalance();
                
                // Сбрасываем предыдущие победители
                wheelItems.forEach(item => {
                    item.classList.remove('winner', 'active');
                });
                
                // Анимация выбора случайного элемента
                let currentIndex = 0;
                const totalSteps = 30 + Math.floor(Math.random() * 20);
                const interval = 100;
                
                const animation = setInterval(() => {
                    // Убираем подсветку с предыдущего элемента
                    wheelItems.forEach(item => {
                        item.classList.remove('active');
                    });
                    
                    // Подсвечиваем текущий элемент
                    wheelItems[currentIndex].classList.add('active');
                    
                    // Переходим к следующему элементу
                    currentIndex = (currentIndex + 1) % wheelItems.length;
                }, interval);
                
                // Останавливаем анимацию и определяем победителя
                setTimeout(() => {
                    clearInterval(animation);
                    
                    // Убираем подсветку со всех элементов
                    wheelItems.forEach(item => {
                        item.classList.remove('active');
                    });
                    
                    // Определяем победителя (последний подсвеченный элемент)
                    const winnerIndex = (currentIndex - 1 + wheelItems.length) % wheelItems.length;
                    const winnerItem = wheelItems[winnerIndex];
                    winnerItem.classList.add('winner');
                    
                    // Получаем приз
                    const prize = generateRandomNFT();
                    
                    // Показываем результат
                    setTimeout(() => {
                        if (addNFTToInventory(prize)) {
                            alert(`Поздравляем! Вы выиграли: ${prize.name}`);
                            userStats.openedCases++;
                            updateStats();
                        } else {
                            alert(`Вы выиграли: ${prize.name}, но инвентарь полон!`);
                        }
                        
                        isSpinning = false;
                        spinBtn.disabled = false;
                    }, 1000);
                    
                }, interval * totalSteps);
            }
            
            // Обработчик случайного улучшения
            randomUpgradeBtn.addEventListener('click', () => {
                if (userStars < 1499) {
                    alert('Недостаточно звезд для улучшения!');
                    return;
                }
                
                // Случайный выбор приза
                const randomPrize = generateRandomNFT();
                
                // Добавляем NFT в инвентарь
                if (addNFTToInventory(randomPrize)) {
                    userStars -= 1499;
                    updateBalance();
                    userStats.upgradedNFT++;
                    updateStats();
                    alert(`Успех! Вы получили: ${randomPrize.name}`);
                } else {
                    alert('Инвентарь полон! Освободите место для нового NFT.');
                }
            });
            
            // Обновление инвентаря для улучшения
            function updateUpgradeInventory() {
                upgradeInventoryItems.innerHTML = '';
                userNFTs.forEach((nft, index) => {
                    const inventoryItem = document.createElement('div');
                    inventoryItem.className = 'inventory-item';
                    
                    const nftVisual = createNFTVisual(nft.type, nft.rarity);
                    inventoryItem.appendChild(nftVisual);
                    
                    inventoryItem.addEventListener('click', () => {
                        document.querySelectorAll('.inventory-item').forEach(item => {
                            item.classList.remove('selected');
                        });
                        inventoryItem.classList.add('selected');
                        selectedNFTForUpgrade = nft;
                        updateSelectedNFTForUpgrade();
                        checkUpgradeReady();
                    });
                    
                    upgradeInventoryItems.appendChild(inventoryItem);
                });
                
                // Добавляем пустые слоты
                const emptySlots = 3 - (userNFTs.length % 3 === 0 ? 0 : 3 - (userNFTs.length % 3));
                for (let i = 0; i < emptySlots; i++) {
                    const emptyItem = document.createElement('div');
                    emptyItem.className = 'inventory-item';
                    emptyItem.style.visibility = 'hidden';
                    upgradeInventoryItems.appendChild(emptyItem);
                }
            }
            
            // Обновление желаемых NFT
            function updateDesiredNFTs() {
                desiredNftGrid.innerHTML = '';
                
                specialNFTs.forEach(nft => {
                    const desiredItem = document.createElement('div');
                    desiredItem.className = 'desired-nft-item';
                    
                    desiredItem.innerHTML = `
                        <div class="desired-nft-image">
                            ${createNFTVisual(nft.type, nft.rarity).outerHTML}
                        </div>
                        <div class="desired-nft-name">${nft.name}</div>
                        <div class="desired-nft-rarity">${nft.rarity.name}</div>
                    `;
                    
                    desiredItem.addEventListener('click', () => {
                        document.querySelectorAll('.desired-nft-item').forEach(item => {
                            item.classList.remove('selected');
                        });
                        desiredItem.classList.add('selected');
                        desiredNFT = nft;
                        updateDesiredNFT();
                        checkUpgradeReady();
                    });
                    
                    desiredNftGrid.appendChild(desiredItem);
                });
            }
            
            // Обновление выбранного NFT для улучшения
            function updateSelectedNFTForUpgrade() {
                if (selectedNFTForUpgrade) {
                    selectedNftForUpgrade.innerHTML = `
                        <div class="selected-nft-image">
                            ${createNFTVisual(selectedNFTForUpgrade.type, selectedNFTForUpgrade.rarity).outerHTML}
                        </div>
                        <div>${selectedNFTForUpgrade.name}</div>
                        <div style="font-size: 0.8rem; opacity: 0.8;">${selectedNFTForUpgrade.rarity.name}</div>
                    `;
                } else {
                    selectedNftForUpgrade.innerHTML = `
                        <i class="fas fa-question-circle" style="font-size: 2rem; opacity: 0.5;"></i>
                        <div>Не выбран</div>
                    `;
                }
            }
            
            // Обновление желаемого NFT
            function updateDesiredNFT() {
                if (desiredNFT) {
                    desiredNft.innerHTML = `
                        <div class="selected-nft-image">
                            ${createNFTVisual(desiredNFT.type, desiredNFT.rarity).outerHTML}
                        </div>
                        <div>${desiredNFT.name}</div>
                        <div style="font-size: 0.8rem; opacity: 0.8;">${desiredNFT.rarity.name}</div>
                    `;
                } else {
                    desiredNft.innerHTML = `
                        <i class="fas fa-question-circle" style="font-size: 2rem; opacity: 0.5;"></i>
                        <div>Не выбран</div>
                    `;
                }
            }
            
            // Проверка готовности к улучшению
            function checkUpgradeReady() {
                if (selectedNFTForUpgrade && desiredNFT) {
                    gameUpgradeBtn.disabled = false;
                } else {
                    gameUpgradeBtn.disabled = true;
                }
            }
            
            // Обработчик улучшения в игре
            gameUpgradeBtn.addEventListener('click', () => {
                if (!selectedNFTForUpgrade || !desiredNFT) return;
                
                // Анимация вращения колеса
                let rotation = 0;
                const targetRotation = 1080 + Math.random() * 360;
                const duration = 3000;
                const startTime = Date.now();
                
                function animateWheel() {
                    const currentTime = Date.now();
                    const elapsed = currentTime - startTime;
                    const progress = Math.min(elapsed / duration, 1);
                    
                    const easeOut = 1 - Math.pow(1 - progress, 3);
                    rotation = easeOut * targetRotation;
                    
                    wheelCircle.style.transform = `rotate(${rotation}deg)`;
                    
                    if (progress < 1) {
                        requestAnimationFrame(animateWheel);
                    } else {
                        // Определяем результат (50% шанс)
                        const isSuccess = Math.random() < 0.5;
                        const finalRotation = rotation % 360;
                        const isGreenZone = finalRotation > 180;
                        
                        // Показываем результат
                        setTimeout(() => {
                            if (isSuccess && isGreenZone) {
                                // Успех - добавляем желаемый NFT
                                if (addNFTToInventory(desiredNFT)) {
                                    removeNFTFromInventory(selectedNFTForUpgrade.id);
                                    alert(`Успех! Вы получили: ${desiredNFT.name}`);
                                    userStats.upgradedNFT++;
                                    updateStats();
                                } else {
                                    alert('Инвентарь полон! Освободите место для нового NFT.');
                                }
                            } else {
                                // Неудача - удаляем выбранный NFT
                                removeNFTFromInventory(selectedNFTForUpgrade.id);
                                alert(`Неудача! Вы потеряли: ${selectedNFTForUpgrade.name}`);
                            }
                            
                            // Сброс выбора
                            selectedNFTForUpgrade = null;
                            desiredNFT = null;
                            updateSelectedNFTForUpgrade();
                            updateDesiredNFT();
                            checkUpgradeReady();
                            updateUpgradeInventory();
                        }, 1000);
                    }
                }
                
                animateWheel();
            });
            
            // Обновление баланса
            function updateBalance() {
                document.getElementById('userBalance').textContent = userStars;
                document.getElementById('profileBalance').textContent = userStars;
                
                // Обновляем состояние кнопки открытия кейса
                if (selectedCase) {
                    openCaseBtn.disabled = userStars < selectedCase.price;
                }
            }
            
            // Добавление NFT в инвентарь
            function addNFTToInventory(nft) {
                if (userNFTs.length < 6) {
                    userNFTs.push(nft);
                    userStats.totalNFT = userNFTs.length;
                    updateInventory();
                    updateProfileCollection();
                    return true;
                }
                return false;
            }
            
            // Удаление NFT из инвентаря
            function removeNFTFromInventory(nftId) {
                const index = userNFTs.findIndex(nft => nft.id === nftId);
                if (index !== -1) {
                    userNFTs.splice(index, 1);
                    userStats.totalNFT = userNFTs.length;
                    updateInventory();
                    updateProfileCollection();
                    return true;
                }
                return false;
            }
            
            // Обновление инвентаря
            function updateInventory() {
                const inventoryItems = document.getElementById('inventoryItems');
                inventoryItems.innerHTML = '';
                
                userNFTs.forEach((nft, index) => {
                    const inventoryItem = document.createElement('div');
                    inventoryItem.className = 'inventory-item';
                    
                    const nftVisual = createNFTVisual(nft.type, nft.rarity);
                    inventoryItem.appendChild(nftVisual);
                    
                    inventoryItems.appendChild(inventoryItem);
                });
                
                // Добавляем пустые слоты
                const emptySlots = 6 - userNFTs.length;
                for (let i = 0; i < emptySlots; i++) {
                    const emptyItem = document.createElement('div');
                    emptyItem.className = 'inventory-item';
                    emptyItem.innerHTML = `<i class="fas fa-plus" style="opacity: 0.5;"></i>`;
                    inventoryItems.appendChild(emptyItem);
                }
                
                document.getElementById('inventoryCount').textContent = `${userNFTs.length}/6`;
                updateStats();
            }
            
            // Обновление коллекции в профиле
            function updateProfileCollection() {
                const profileInventoryItems = document.getElementById('profileInventoryItems');
                profileInventoryItems.innerHTML = '';
                
                if (userNFTs.length === 0) {
                    profileInventoryItems.innerHTML = `
                        <div style="grid-column: 1 / -1; text-align: center; padding: 40px; opacity: 0.7;">
                            <i class="fas fa-box-open" style="font-size: 3rem; margin-bottom: 15px;"></i>
                            <div>Ваша коллекция NFT пуста</div>
                            <div style="font-size: 0.9rem; margin-top: 10px;">Откройте кейсы, чтобы получить NFT!</div>
                        </div>
                    `;
                    return;
                }
                
                userNFTs.forEach((nft, index) => {
                    const collectionItem = document.createElement('div');
                    collectionItem.className = 'inventory-item';
                    collectionItem.style.position = 'relative';
                    
                    const nftVisual = createNFTVisual(nft.type, nft.rarity);
                    collectionItem.appendChild(nftVisual);
                    
                    // Добавляем информацию о стоимости
                    const valueBadge = document.createElement('div');
                    valueBadge.style.position = 'absolute';
                    valueBadge.style.bottom = '5px';
                    valueBadge.style.right = '5px';
                    valueBadge.style.background = 'rgba(0,0,0,0.7)';
                    valueBadge.style.color = 'gold';
                    valueBadge.style.padding = '2px 6px';
                    valueBadge.style.borderRadius = '10px';
                    valueBadge.style.fontSize = '0.7rem';
                    valueBadge.style.fontWeight = 'bold';
                    valueBadge.innerHTML = `<i class="fas fa-star"></i> ${nft.value}`;
                    collectionItem.appendChild(valueBadge);
                    
                    // Добавляем кнопки действий
                    const actionsContainer = document.createElement('div');
                    actionsContainer.style.position = 'absolute';
                    actionsContainer.style.top = '5px';
                    actionsContainer.style.right = '5px';
                    actionsContainer.style.display = 'flex';
                    actionsContainer.style.gap = '5px';
                    
                    const upgradeBtn = document.createElement('button');
                    upgradeBtn.innerHTML = '<i class="fas fa-sync-alt"></i>';
                    upgradeBtn.style.background = 'var(--primary)';
                    upgradeBtn.style.border = 'none';
                    upgradeBtn.style.borderRadius = '50%';
                    upgradeBtn.style.width = '25px';
                    upgradeBtn.style.height = '25px';
                    upgradeBtn.style.color = 'white';
                    upgradeBtn.style.cursor = 'pointer';
                    upgradeBtn.style.fontSize = '0.7rem';
                    upgradeBtn.title = 'Улучшить этот NFT';
                    
                    upgradeBtn.addEventListener('click', (e) => {
                        e.stopPropagation();
                        // Переключаемся на вкладку Upgrade и выбираем этот NFT
                        document.querySelector('[data-tab="upgrade-game"]').click();
                        selectedNFTForUpgrade = nft;
                        updateSelectedNFTForUpgrade();
                        updateUpgradeInventory();
                    });
                    
                    const sellBtn = document.createElement('button');
                    sellBtn.innerHTML = '<i class="fas fa-star"></i>';
                    sellBtn.style.background = 'var(--success)';
                    sellBtn.style.border = 'none';
                    sellBtn.style.borderRadius = '50%';
                    sellBtn.style.width = '25px';
                    sellBtn.style.height = '25px';
                    sellBtn.style.color = 'white';
                    sellBtn.style.cursor = 'pointer';
                    sellBtn.style.fontSize = '0.7rem';
                    sellBtn.title = 'Продать этот NFT';
                    
                    sellBtn.addEventListener('click', (e) => {
                        e.stopPropagation();
                        if (confirm(`Продать "${nft.name}" за ${nft.value} звезд?`)) {
                            userStars += nft.value;
                            removeNFTFromInventory(nft.id);
                            updateBalance();
                            updateProfileCollection();
                            alert(`NFT продан за ${nft.value} звезд!`);
                        }
                    });
                    
                    actionsContainer.appendChild(upgradeBtn);
                    actionsContainer.appendChild(sellBtn);
                    collectionItem.appendChild(actionsContainer);
                    
                    profileInventoryItems.appendChild(collectionItem);
                });
                
                // Добавляем пустые слоты для сетки
                const emptySlots = 6 - (userNFTs.length % 3 === 0 ? 0 : 3 - (userNFTs.length % 3));
                for (let i = 0; i < emptySlots; i++) {
                    const emptyItem = document.createElement('div');
                    emptyItem.className = 'inventory-item';
                    emptyItem.style.visibility = 'hidden';
                    profileInventoryItems.appendChild(emptyItem);
                }
            }
            
            // Обновление статистики
            function updateStats() {
                document.getElementById('statOpenedCases').textContent = userStats.openedCases;
                document.getElementById('statTotalNFT').textContent = userStats.totalNFT;
                document.getElementById('statUpgradedNFT').textContent = userStats.upgradedNFT;
                
                const legendaryCount = userNFTs.filter(nft => 
                    nft.rarity.name === "Легендарный" || nft.rarity.name === "Мифический"
                ).length;
                document.getElementById('statLegendaryNFT').textContent = legendaryCount;
                userStats.legendaryNFT = legendaryCount;
            }
            
            // Инициализация
            function init() {
                updateProfile();
                initCases();
                initWheel();
                updateBalance();
                specialNFTs = generateSpecialNFTs();
                
                // Добавляем несколько начальных NFT для демонстрации
                addNFTToInventory(generateRandomNFT());
                addNFTToInventory(generateRandomNFT());
            }
            
            // Запуск инициализации
            init();
        });
    </script>
</body>
</html>
