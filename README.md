<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>Тестирование АФР</title>
<style>
:root {
  /* Светлая тема по умолчанию */
  --bg-gradient-start: #f5f7fa;
  --bg-gradient-end: #c3cfe2;
  --container-bg: white;
  --text-color: #333;
  --text-secondary: #666;
  --text-light: #999;
  --border-color: #e0e0e0;
  --tab-bg: #f8f9fa;
  --tab-active-color: #16C23C;
  --answer-bg: #f8f9fa;
  --answer-border: #16C23C;
  --quiz-option-bg: white;
  --quiz-option-border: #e0e0e0;
  --quiz-option-hover: #f5f5f5;
  --quiz-correct-bg: #f0fff4;
  --quiz-correct-border: #16C23C;
  --quiz-incorrect-bg: #fff0f0;
  --quiz-incorrect-border: #ff4d4d;
  --quiz-feedback-correct-bg: #f0fff4;
  --quiz-feedback-incorrect-bg: #fff0f0;
  --shadow-color: rgba(0,0,0,0.1);
  --shadow-light: rgba(0,0,0,0.05);
  --button-gradient-start: #16C23C;
  --button-gradient-end: #2FA7CD;
  --button-shadow: rgba(22, 194, 60, 0.3);
  --menu-bg: rgba(255,255,255,0.95);
  --menu-bg-blur: blur(10px);
  --menu-border: rgba(0,0,0,0.08);
}

[data-theme="dark"] {
  /* Темная тема */
  --bg-gradient-start: #1a1a2e;
  --bg-gradient-end: #16213e;
  --container-bg: #2d2d44;
  --text-color: #e0e0e0;
  --text-secondary: #a0a0a0;
  --text-light: #808080;
  --border-color: #404055;
  --tab-bg: #252538;
  --tab-active-color: #4cd964;
  --answer-bg: #252538;
  --answer-border: #4cd964;
  --quiz-option-bg: #252538;
  --quiz-option-border: #404055;
  --quiz-option-hover: #303045;
  --quiz-correct-bg: #1e3a28;
  --quiz-correct-border: #4cd964;
  --quiz-incorrect-bg: #3a1e1e;
  --quiz-incorrect-border: #ff6b6b;
  --quiz-feedback-correct-bg: #1e3a28;
  --quiz-feedback-incorrect-bg: #3a1e1e;
  --shadow-color: rgba(0,0,0,0.3);
  --shadow-light: rgba(0,0,0,0.2);
  --button-gradient-start: #4cd964;
  --button-gradient-end: #2FA7CD;
  --button-shadow: rgba(76, 217, 100, 0.3);
  --menu-bg: rgba(37,37,56,0.92);
  --menu-border: rgba(255,255,255,0.08);
}

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  -webkit-tap-highlight-color: transparent;
  transition: background-color 0.3s ease, border-color 0.3s ease;
}

body {
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
  line-height: 1.6;
  padding: 20px;
  background: linear-gradient(135deg, var(--bg-gradient-start) 0%, var(--bg-gradient-end) 100%);
  min-height: 100vh;
  -webkit-text-size-adjust: 100%;
  color: var(--text-color);
}

.header {
  text-align: center;
  margin-bottom: 20px;
  padding: 15px 0;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 15px;
}

.theme-toggle {
  background: var(--container-bg);
  border: 2px solid var(--border-color);
  border-radius: 50px;
  padding: 8px 16px;
  cursor: pointer;
  font-size: 14px;
  font-weight: 600;
  color: var(--text-color);
  display: flex;
  align-items: center;
  gap: 8px;
  transition: all 0.3s ease;
  box-shadow: 0 2px 5px var(--shadow-light);
}

.theme-toggle:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 10px var(--shadow-light);
}

.theme-toggle:active {
  transform: translateY(0);
}

.theme-toggle i {
  font-size: 16px;
}

.logo {
  max-width: 200px;
  height: auto;
  filter: drop-shadow(0 2px 4px var(--shadow-light));
}

.logo path[fill="#4FB84E"] {
  fill: #4FB84E;
}

.logo path[fill="#055532"] {
  fill: #055532;
}

.logo .letter {
  fill: var(--text-color);
}

.container {
  max-width: 900px;
  margin: 0 auto;
  background: var(--container-bg);
  border-radius: 15px;
  box-shadow: 0 10px 30px var(--shadow-color);
  overflow: hidden;
  -webkit-overflow-scrolling: touch;
  position: relative;
}

/* ======= Global Menu ======= */
.global-menu-wrap {
  position: sticky;
  top: 0;
  z-index: 50;
  background: transparent;
  padding: 10px 12px 0 12px;
  display: flex;
  justify-content: flex-end;
  pointer-events: none;
}

.global-menu {
  pointer-events: auto;
  display: flex;
  align-items: center;
  gap: 10px;
}

.menu-btn {
  background: linear-gradient(135deg, var(--button-gradient-start) 0%, var(--button-gradient-end) 100%);
  color: #fff;
  border: none;
  border-radius: 12px;
  padding: 10px 14px;
  font-weight: 800;
  cursor: pointer;
  box-shadow: 0 8px 20px var(--button-shadow);
  display: flex;
  align-items: center;
  gap: 8px;
  user-select: none;
}

.menu-btn:hover {
  transform: translateY(-1px);
}

.menu-btn:active {
  transform: translateY(0);
}

.menu-panel {
  position: absolute;
  right: 12px;
  top: 58px;
  width: min(360px, calc(100% - 24px));
  background: var(--menu-bg);
  backdrop-filter: var(--menu-bg-blur);
  border: 1px solid var(--menu-border);
  border-radius: 14px;
  box-shadow: 0 10px 30px var(--shadow-color);
  padding: 14px;
  display: none;
  animation: fadeIn 0.25s ease-in;
}

.menu-panel.show {
  display: block;
}

.menu-title {
  font-weight: 900;
  font-size: 14px;
  color: var(--text-secondary);
  margin-bottom: 10px;
  letter-spacing: 0.2px;
}

.menu-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 10px;
}

.menu-action {
  padding: 12px 12px;
  border-radius: 12px;
  border: 1px solid var(--border-color);
  background: var(--container-bg);
  color: var(--text-color);
  cursor: pointer;
  font-weight: 800;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
  box-shadow: 0 2px 10px var(--shadow-light);
  user-select: none;
}

.menu-action:hover {
  transform: translateY(-1px);
}

.menu-action:active {
  transform: translateY(0);
}

.menu-action.secondary {
  background: var(--answer-bg);
  font-weight: 700;
  color: var(--text-secondary);
}

.menu-row {
  display: flex;
  gap: 10px;
  margin-top: 10px;
}

.menu-row .menu-action {
  flex: 1;
}

.menu-hint {
  margin-top: 10px;
  font-size: 12px;
  color: var(--text-light);
  text-align: center;
}

/* ======= Content sections ======= */
.tab-content {
  display: none;
  padding: 25px;
  min-height: 500px;
}

.tab-content.active {
  display: block;
  animation: fadeIn 0.5s ease-in;
}

/* ======= Home ======= */
.home-wrap {
  display: flex;
  flex-direction: column;
  gap: 16px;
  min-height: 500px;
}

.home-card {
  background: var(--answer-bg);
  border: 1px solid var(--border-color);
  border-radius: 14px;
  padding: 22px;
  box-shadow: 0 6px 18px var(--shadow-light);
}

.motivation-quote {
  font-size: 20px;
  font-weight: 900;
  line-height: 1.35;
  color: var(--text-color);
  background: var(--container-bg);
  border-left: 4px solid var(--answer-border);
  border-radius: 12px;
  padding: 18px 16px;
  white-space: pre-wrap;
  word-break: break-word;
  animation: fadeIn 0.35s ease-in;
}

.home-controls {
  display: flex;
  gap: 10px;
  margin-top: 14px;
  flex-wrap: wrap;
  justify-content: center;
}

.home-btn {
  padding: 12px 16px;
  border-radius: 10px;
  border: none;
  cursor: pointer;
  font-weight: 900;
  background: linear-gradient(135deg, var(--button-gradient-start) 0%, var(--button-gradient-end) 100%);
  color: #fff;
  box-shadow: 0 4px 15px var(--button-shadow);
  user-select: none;
  min-width: 130px;
}

.home-btn.secondary {
  background: var(--border-color);
  color: var(--text-color);
  box-shadow: none;
  font-weight: 800;
}

.home-btn:hover {
  transform: translateY(-1px);
}

.home-btn:active {
  transform: translateY(0);
}

.home-timer {
  margin-top: 10px;
  text-align: center;
  font-size: 13px;
  color: var(--text-secondary);
}

/* ======= Study ======= */
.study-container {
  display: flex;
  flex-direction: column;
  min-height: 500px;
}

.question {
  display: none;
}

.question.active {
  display: block;
  animation: fadeIn 0.5s ease-in;
}

.question-number {
  font-size: 14px;
  color: var(--text-secondary);
  margin-bottom: 10px;
  font-weight: bold;
}

.question-text {
  font-size: 18px;
  font-weight: bold;
  margin-bottom: 25px;
  color: var(--text-color);
  line-height: 1.5;
}

.answer {
  font-size: 16px;
  color: var(--text-color);
  background: var(--answer-bg);
  padding: 20px;
  border-radius: 10px;
  border-left: 4px solid var(--answer-border);
  white-space: pre-wrap;
  line-height: 1.6;
  word-wrap: break-word;
  overflow-wrap: break-word;
  margin-bottom: 20px;
}

/* ======= Test ======= */
.test-question-container {
  background: var(--answer-bg);
  border-radius: 10px;
  padding: 25px;
  margin-bottom: 20px;
}

.test-question-number {
  font-size: 14px;
  color: var(--text-secondary);
  margin-bottom: 15px;
  font-weight: bold;
  text-align: center;
}

.test-question-text {
  font-size: 18px;
  font-weight: bold;
  margin-bottom: 30px;
  color: var(--text-color);
  line-height: 1.5;
  text-align: center;
}

.test-answer {
  font-size: 16px;
  color: var(--text-color);
  background: var(--container-bg);
  padding: 20px;
  border-radius: 10px;
  border-left: 4px solid var(--answer-border);
  white-space: pre-wrap;
  line-height: 1.6;
  word-wrap: break-word;
  overflow-wrap: break-word;
  display: none;
  margin-top: 20px;
  animation: fadeIn 0.5s ease-in;
}

.test-answer.show {
  display: block;
}

.test-controls {
  display: flex;
  justify-content: center;
  gap: 15px;
  margin-top: 20px;
  flex-wrap: wrap;
}

.test-button {
  padding: 14px 25px;
  font-size: 16px;
  font-weight: bold;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  transition: all 0.3s ease;
  min-width: 150px;
  background: linear-gradient(135deg, var(--button-gradient-start) 0%, var(--button-gradient-end) 100%);
  color: white;
  box-shadow: 0 4px 15px var(--button-shadow);
  -webkit-appearance: none;
  -webkit-user-select: none;
  user-select: none;
}

.test-button:hover {
  transform: translateY(-2px);
  box-shadow: 0 6px 20px var(--button-shadow);
}

.test-button:active {
  transform: translateY(0);
}

.test-stats {
  text-align: center;
  color: var(--text-secondary);
  font-size: 14px;
  margin-bottom: 20px;
  padding: 10px;
  background: var(--container-bg);
  border-radius: 8px;
  box-shadow: 0 2px 10px var(--shadow-light);
}

/* ======= Navigation ======= */
.navigation {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-top: 30px;
  padding-top: 20px;
  border-top: 1px solid var(--border-color);
  gap: 15px;
  flex-wrap: wrap;
}

.nav-button {
  padding: 14px 25px;
  font-size: 16px;
  font-weight: bold;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  transition: all 0.3s ease;
  min-width: 120px;
  background: linear-gradient(135deg, var(--button-gradient-start) 0%, var(--button-gradient-end) 100%);
  color: white;
  box-shadow: 0 4px 15px var(--button-shadow);
  -webkit-appearance: none;
  -webkit-user-select: none;
  user-select: none;
}

.nav-button:hover {
  transform: translateY(-2px);
  box-shadow: 0 6px 20px var(--button-shadow);
}

.nav-button:active {
  transform: translateY(0);
}

.nav-button:disabled {
  background: var(--text-light);
  cursor: not-allowed;
  transform: none;
  box-shadow: none;
}

.question-counter {
  font-size: 14px;
  color: var(--text-secondary);
  font-weight: bold;
  text-align: center;
  flex: 1;
}

.quick-navigation {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 12px;
  margin: 20px 0;
  padding: 15px;
  background: var(--answer-bg);
  border-radius: 10px;
  flex-wrap: wrap;
}

.quick-navigation label {
  font-weight: bold;
  color: var(--text-color);
  font-size: 14px;
}

.number-input {
  padding: 12px;
  border: 2px solid var(--border-color);
  border-radius: 6px;
  font-size: 16px;
  width: 80px;
  text-align: center;
  background: var(--container-bg);
  color: var(--text-color);
  -webkit-appearance: none;
  appearance: none;
}

.number-input:focus {
  outline: none;
  border-color: var(--tab-active-color);
}

.go-button {
  padding: 12px 20px;
  background: linear-gradient(135deg, var(--button-gradient-start) 0%, var(--button-gradient-end) 100%);
  color: white;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  font-weight: bold;
  transition: all 0.3s ease;
  -webkit-appearance: none;
  -webkit-user-select: none;
  user-select: none;
}

.go-button:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 10px var(--button-shadow);
}

.go-button:active {
  transform: translateY(0);
}

/* ======= Quiz 10 ======= */
.quiz-container {
  max-width: 800px;
  margin: 0 auto;
}

.quiz-stats {
  text-align: center;
  font-size: 16px;
  color: var(--text-secondary);
  margin-bottom: 20px;
  padding: 15px;
  background: var(--container-bg);
  border-radius: 10px;
  box-shadow: 0 2px 10px var(--shadow-light);
}

.quiz-question {
  background: var(--answer-bg);
  border-radius: 10px;
  padding: 25px;
  margin-bottom: 20px;
}

.quiz-question-number {
  font-size: 14px;
  color: var(--text-secondary);
  margin-bottom: 10px;
  font-weight: bold;
}

.quiz-question-text {
  font-size: 18px;
  font-weight: bold;
  margin-bottom: 25px;
  color: var(--text-color);
  line-height: 1.5;
}

.quiz-options {
  display: flex;
  flex-direction: column;
  gap: 12px;
}

.quiz-option {
  background: var(--quiz-option-bg);
  border: 2px solid var(--quiz-option-border);
  border-radius: 8px;
  padding: 16px 20px;
  cursor: pointer;
  transition: all 0.3s ease;
  font-size: 16px;
  color: var(--text-color);
  position: relative;
}

.quiz-option:hover {
  background: var(--quiz-option-hover);
  transform: translateY(-2px);
}

.quiz-option.selected {
  border-color: var(--tab-active-color);
  background: var(--quiz-option-hover);
}

.quiz-option.correct {
  border-color: var(--quiz-correct-border);
  background: var(--quiz-correct-bg);
}

.quiz-option.incorrect {
  border-color: var(--quiz-incorrect-border);
  background: var(--quiz-incorrect-bg);
}

.quiz-feedback {
  margin-top: 20px;
  padding: 15px;
  border-radius: 8px;
  display: none;
  animation: fadeIn 0.5s ease-in;
}

.quiz-feedback.correct {
  background: var(--quiz-feedback-correct-bg);
  border-left: 4px solid var(--quiz-correct-border);
  color: var(--text-color);
}

.quiz-feedback.incorrect {
  background: var(--quiz-feedback-incorrect-bg);
  border-left: 4px solid var(--quiz-incorrect-border);
  color: var(--text-color);
}

.quiz-feedback.show {
  display: block;
}

.quiz-controls {
  display: flex;
  justify-content: center;
  gap: 15px;
  margin-top: 30px;
  flex-wrap: wrap;
}

.quiz-button {
  padding: 14px 25px;
  font-size: 16px;
  font-weight: bold;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  transition: all 0.3s ease;
  min-width: 150px;
  -webkit-appearance: none;
  -webkit-user-select: none;
  user-select: none;
  background: linear-gradient(135deg, var(--button-gradient-start) 0%, var(--button-gradient-end) 100%);
  color: white;
  box-shadow: 0 4px 15px var(--button-shadow);
}

.quiz-button:hover {
  transform: translateY(-2px);
  box-shadow: 0 6px 20px var(--button-shadow);
}

.quiz-button:disabled {
  background: var(--text-light);
  cursor: not-allowed;
  transform: none;
  box-shadow: none;
}

.quiz-result {
  text-align: center;
  padding: 30px;
  background: var(--container-bg);
  border-radius: 10px;
  box-shadow: 0 5px 20px var(--shadow-color);
  margin-top: 20px;
  display: none;
  animation: fadeIn 0.5s ease-in;
}

.quiz-result.show {
  display: block;
}

.quiz-score {
  font-size: 48px;
  font-weight: bold;
  color: var(--tab-active-color);
  margin: 20px 0;
}

.quiz-message {
  font-size: 18px;
  color: var(--text-secondary);
  margin-bottom: 25px;
}

.restart-quiz-btn {
  padding: 14px 30px;
  font-size: 16px;
  font-weight: bold;
  background: linear-gradient(135deg, var(--button-gradient-start) 0%, var(--button-gradient-end) 100%);
  color: white;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  transition: all 0.3s ease;
  margin-top: 20px;
}

.restart-quiz-btn:hover {
  transform: translateY(-2px);
  box-shadow: 0 6px 20px var(--button-shadow);
}

@keyframes fadeIn {
  from { opacity: 0; transform: translateY(10px); }
  to { opacity: 1; transform: translateY(0); }
}

/* iOS specific fixes */
@supports (-webkit-touch-callout: none) {
  .nav-button, .go-button, .test-button, .quiz-button, .restart-quiz-btn, .home-btn, .menu-btn {
    padding: 16px 25px;
  }
  .number-input {
    padding: 14px;
    font-size: 18px;
  }
  body {
    -webkit-overflow-scrolling: touch;
  }
}

@media (max-width: 768px) {
  .container {
    margin: 10px;
  }
  .tab-content {
    padding: 20px;
  }
  .quick-navigation {
    flex-direction: row;
    gap: 10px;
  }
  .number-input {
    width: 70px;
    font-size: 16px;
  }
  .go-button {
    padding: 12px 16px;
    font-size: 14px;
  }
  .question-counter {
    font-size: 13px;
  }
  .test-controls, .quiz-controls {
    flex-direction: column;
    align-items: center;
  }
  .test-button, .quiz-button {
    width: 100%;
    max-width: 200px;
  }
  .menu-grid {
    grid-template-columns: 1fr;
  }
}

@media (max-width: 480px) {
  .navigation {
    flex-direction: column;
    gap: 10px;
  }
  .nav-button {
    width: 100%;
    max-width: 200px;
  }
  .quick-navigation {
    flex-direction: column;
    gap: 8px;
  }
  .quick-navigation label {
    text-align: center;
  }
  .quiz-option {
    padding: 12px 15px;
    font-size: 14px;
  }
}

/* Убираем стрелки у number input */
input[type="number"]::-webkit-outer-spin-button,
input[type="number"]::-webkit-inner-spin-button {
  -webkit-appearance: none;
  margin: 0;
}

input[type="number"] {
  -moz-appearance: textfield;
}
</style>
</head>
<body>
<div class="header">
  <button class="theme-toggle" id="themeToggle" onclick="toggleTheme()">
    <i>🌙</i> Тёмная тема
  </button>
  <svg width="155" height="47" class="BffLogo logo" viewBox="0 0 155 47" fill="none" xmlns="http://www.w3.org/2000/svg">
    <path d="M20.1671 47C20.1671 47 0 41.0173 0 11.3571V0H40.3342V11.3571C40.3342 39.9018 20.1671 47 20.1671 47Z" fill="#4FB84E"></path>
    <path d="M31.8351 11.4585H15.5908V20.8889H31.8351V23.7282C31.8351 28.4941 30.4773 32.3474 23.3358 32.3474H15.5908V44.7692C18.2563 46.4423 20.1674 47 20.1674 47C20.1674 47 40.3345 39.9018 40.3345 11.3571V2.78857C40.3345 7.55449 38.2725 11.3571 31.8351 11.4585Z" fill="#055532"></path>
    <path class="letter" d="M51.5488 27.9363H55.7231C57.9862 27.9363 59.5956 28.9503 59.5956 30.9276C59.5956 31.891 59.1429 32.8543 58.288 33.3613V33.412C59.5955 33.8176 60.0482 34.9837 60.0482 35.9978C60.0482 38.4821 57.9862 39.3947 55.7231 39.3947H51.5488V27.9363ZM55.7734 32.3473C56.4774 32.3473 56.7792 31.8403 56.7792 31.2826C56.7792 30.7755 56.4774 30.3192 55.7231 30.3192H54.3652V32.3473H55.7734ZM55.9745 36.9611C56.7792 36.9611 57.1815 36.4034 57.1815 35.7443C57.1815 35.0851 56.7792 34.5781 55.9745 34.5781H54.3149V36.9611H55.9745Z" fill="black"></path>
    <path class="letter" d="M64.323 27.9363H67.2399L71.1124 39.344H68.2458L67.5417 36.9611H64.0212L63.3172 39.344H60.4505L64.323 27.9363ZM66.8879 34.7302L66.2341 32.4994C66.0329 31.7896 65.7815 30.522 65.7815 30.522H65.7312C65.7312 30.522 65.4797 31.7896 65.2786 32.4994L64.6248 34.7302H66.8879Z" fill="black"></path>
    <path class="letter" d="M72.2188 27.9363H75.0352L78.1533 33.2599C78.6059 34.0204 79.1089 35.1865 79.1089 35.1865H79.1591C79.1591 35.1865 79.0083 33.9697 79.0083 33.2599V27.9363H81.7743V39.344H79.0083L75.8399 34.0204C75.3872 33.2599 74.8843 32.0938 74.8843 32.0938H74.834C74.834 32.0938 74.9849 33.3106 74.9849 34.0204V39.344H72.2188V27.9363Z" fill="black"></path>
    <path class="letter" d="M86.9544 27.9363V32.3473H88.1111L90.4748 27.9363H93.4421L90.3743 33.412V33.4627L93.5929 39.3947H90.4748L88.0608 34.7809H86.9041V39.3947H84.1381V27.9363H86.9544Z" fill="black"></path>
    <path class="letter" d="M51.5488 22.765V6.74344H61.3055V7.90957C61.3055 9.1264 60.3499 10.0897 59.1429 10.0897H55.4213V13.436H60.2493V14.5514C60.2493 15.7683 59.2435 16.7823 58.0365 16.7823H55.4213V22.7143H51.5488V22.765Z" fill="black"></path>
    <path class="letter" d="M67.3905 13.9937H69.1004C69.7542 13.9937 70.2571 13.8416 70.6092 13.4867C70.9612 13.1318 71.1624 12.6755 71.1624 12.0164C71.1624 11.1544 70.8606 10.546 70.2068 10.2925C69.8548 10.1404 69.3519 10.0897 68.7484 10.0897H67.4408V13.9937H67.3905ZM63.4677 6.74344H68.9998C70.408 6.74344 71.4138 6.89555 72.0676 7.14905C72.9729 7.50396 73.7273 8.06167 74.2302 8.87289C74.7331 9.68411 74.9846 10.6474 74.9846 11.8136C74.9846 12.7262 74.7834 13.5881 74.3811 14.3993C73.9787 15.2105 73.3752 15.819 72.6208 16.2246V16.2753C72.7717 16.4781 72.9729 16.7823 73.2243 17.2386L76.2922 22.8664H72.0173L69.201 17.4414H67.3905V22.8664H63.4677V6.74344Z" fill="black"></path>
    <path class="letter" d="M78.1027 22.765V6.74344H88.1108V10.0897H82.0254V13.0304H86.8535V16.3767H82.0254V19.4187H88.4125V22.765H78.1027Z" fill="black"></path>
    <path class="letter" d="M90.8771 22.765V6.74344H100.835V10.0897H94.7999V13.0304H99.6279V16.3767H94.7999V19.4187H101.187V22.765H90.8771Z" fill="black"></path>
    <path class="letter" d="M107.524 19.4187H109.133C110.491 19.4187 111.547 19.0131 112.301 18.2019C113.056 17.3907 113.458 16.2246 113.458 14.7035C113.458 13.2332 113.056 12.0671 112.301 11.2558C111.547 10.4446 110.491 10.0897 109.133 10.0897H107.524V19.4187ZM103.651 22.765V6.74344H109.284C111.798 6.74344 113.81 7.45326 115.269 8.87289C116.727 10.2925 117.481 12.2192 117.481 14.7542C117.481 17.2893 116.727 19.2159 115.269 20.6356C113.81 22.0552 111.798 22.765 109.284 22.765H103.651Z" fill="black"></path>
    <path class="letter" d="M123.215 14.6531C123.215 16.0221 123.617 17.1882 124.422 18.1008C125.226 19.0134 126.283 19.4697 127.49 19.4697C128.747 19.4697 129.753 19.0134 130.557 18.1008C131.362 17.1882 131.764 16.0221 131.764 14.6531C131.764 13.3349 131.362 12.2195 130.557 11.3575C129.753 10.4956 128.747 10.0393 127.49 10.0393C126.232 10.0393 125.226 10.4956 124.422 11.3575C123.617 12.2195 123.215 13.3349 123.215 14.6531ZM119.191 14.6531C119.191 12.3209 119.946 10.3942 121.505 8.82249C123.064 7.25075 125.076 6.49023 127.49 6.49023C129.904 6.49023 131.915 7.25075 133.474 8.82249C135.033 10.3942 135.788 12.3209 135.788 14.6531C135.788 17.0361 135.033 19.0641 133.474 20.6359C131.915 22.2583 129.904 23.0188 127.49 23.0188C125.076 23.0188 123.064 22.2076 121.505 20.6359C119.946 19.0134 119.191 17.0361 119.191 14.6531Z" fill="black"></path>
    <path class="letter" d="M137.648 22.765L138.956 6.74344H143.181L145.494 13.5374L146.299 16.1739H146.349C146.651 15.1091 146.902 14.1965 147.154 13.5374L149.467 6.74344H153.692L154.999 22.765H151.127L150.624 15.5654C150.573 15.2105 150.573 14.8049 150.573 14.3486C150.573 13.8923 150.573 13.5374 150.573 13.2839V12.8783H150.523C150.171 13.9937 149.869 14.9063 149.618 15.5654L147.958 20.23H144.639L142.979 15.5654L142.074 12.8783H142.024C142.074 13.8416 142.074 14.7542 142.024 15.5654L141.521 22.765H137.648Z" fill="black"></path>
  </svg>
</div>

<div class="container">
  <!-- Global menu -->
  <div class="global-menu-wrap">
    <div class="global-menu">
      <button class="menu-btn" onclick="toggleMenu()" id="menuBtn">☰ Меню</button>
      <div class="menu-panel" id="menuPanel">
        <div class="menu-title">Навигация</div>
        <div class="menu-grid">
          <div class="menu-action" onclick="switchTab('home'); closeMenu()">🏠 Главная</div>
          <div class="menu-action" onclick="switchTab('study'); closeMenu()">📚 Изучение</div>
          <div class="menu-action" onclick="switchTab('test'); closeMenu()">🎯 Тестирование</div>
          <div class="menu-action" onclick="switchTab('quiz'); closeMenu()">🧠 Quiz 10</div>
          <div class="menu-action" onclick="switchTab('new_study'); closeMenu()">🆕 Новые вопросы</div>
        </div>
        <div class="menu-title" style="margin-top:12px;">Действия</div>
        <div class="menu-row">
          <div class="menu-action" onclick="resetTestSilent(); closeMenu()">♻️ Сброс теста</div>
          <div class="menu-action" onclick="resetStudyTo1(); closeMenu()">🔢 Вопрос 1</div>
        </div>
        <div class="menu-hint">Меню управляет всеми режимами</div>
      </div>
    </div>
  </div>

  <!-- HOME -->
  <div class="tab-content active" id="home-tab">
    <div class="home-wrap">
      <div class="home-card">
        <div class="motivation-quote" id="motivationQuote">
          Нажми "Следующая" или подожди 1 минуту 🙂
        </div>
        <div class="home-controls">
          <button class="home-btn secondary" onclick="prevMotivation()">⬅️ Назад</button>
          <button class="home-btn" onclick="nextMotivation(true)">➡️ Следующая</button>
          <button class="home-btn secondary" id="motivationAutoBtn" onclick="toggleMotivationAuto()">⏸ Пауза</button>
        </div>
        <div class="home-timer" id="motivationTimer">
          Следующая фраза через: <b id="motivationCountdown">60</b> сек
        </div>
      </div>
    </div>
  </div>

  <!-- STUDY -->
  <div class="tab-content" id="study-tab">
    <div class="study-container"></div>
  </div>

  <!-- TEST -->
  <div class="tab-content" id="test-tab">
    <div class="test-stats" id="testStats">
      Вопросов пройдено: 0 | Осталось: 308
    </div>
    <div class="test-question-container">
      <div class="test-question-number" id="testQuestionNumber">Вопрос #</div>
      <div class="test-question-text" id="testQuestionText">Нажмите "Следующий вопрос" для начала тестирования</div>
      <div class="test-answer" id="testAnswer"></div>
      <div class="test-controls">
        <button class="test-button show-answer-btn" onclick="showTestAnswer()" id="showAnswerBtn" style="display: none;">
          Показать ответ
        </button>
        <button class="test-button next-question-btn" onclick="nextTestQuestion()">
          Следующий вопрос
        </button>
      </div>
    </div>
    <div class="navigation">
      <button class="nav-button" onclick="resetTest()">Начать заново</button>
      <div class="question-counter" id="testQuestionCounter">Случайный тест</div>
      <button class="nav-button" onclick="switchTab('study')">Вернуться к изучению</button>
    </div>
  </div>

  <!-- QUIZ -->
  <div class="tab-content" id="quiz-tab">
    <div class="quiz-container">
      <div class="quiz-stats" id="quizStats">
        Тест из 10 вопросов. Правильных ответов: <span id="correctCount">0</span> из <span id="totalCount">0</span>
      </div>
      <div id="quizContent"></div>
      <div class="quiz-controls">
        <button class="quiz-button next-quiz-btn" onclick="nextQuizQuestion()" id="nextQuizBtn" disabled style="display: none;">
          Следующий вопрос
        </button>
        <button class="quiz-button finish-quiz-btn" onclick="finishQuiz()" id="finishQuizBtn" style="display: none;">
          Завершить тест
        </button>
      </div>
      <div class="quiz-result" id="quizResult">
        <h2>Тест завершен!</h2>
        <div class="quiz-score" id="quizScore">0%</div>
        <div class="quiz-message" id="quizMessage"></div>
        <button class="restart-quiz-btn" onclick="restartQuiz()">Пройти тест заново</button>
      </div>
    </div>
  </div>

  <!-- NEW STUDY -->
  <div class="tab-content" id="new_study-tab">
    <div class="study-container-new"></div>
  </div>

</div>

<script>
// ==========================
// 1) Мотивационные фразы
// ==========================
const motivationPhrases = [
  "Учись, пока другие оправдываются.",
  "Знания не падают — их забирают.",
  "Твой прогресс начинается с «сегодня».",
  "Слабая мотивация — включай дисциплину.",
  "Учёба — это инвестиция без потолка.",
  "Каждый день без навыка — минус в будущем.",
  "Делай шаг, даже если маленький.",
  "Не жди идеальных условий — начни.",
  "Побеждает тот, кто повторяет.",
  "Учёба — это твоя личная броня.",
  "Не понял — перечитай, не сдался.",
  "Навык строится скучными днями.",
  "Отложил на завтра — украл у себя.",
  "Система сильнее вдохновения.",
  "Учись ради свободы, не ради оценки.",
  "Чем сложнее, тем полезнее.",
  "Твой мозг любит повторения — корми его.",
  "Привычка учиться — суперсила взрослого.",
  "Стабильность — это ежедневный минимум.",
  "Делай, даже когда не хочется.",
  "Учи не «когда будет время», а «пока есть жизнь».",
  "Мозг растёт там, где больно.",
  "Ошибка — это плата за развитие.",
  "Не сравнивай старт — сравнивай путь.",
  "Маленькая победа каждый день — большой результат.",
  "Стань человеком, который доводит.",
  "Учёба не забирает время — она его умножает.",
  "Результат любит регулярность.",
  "Делаешь мало? Делай ежедневно.",
  "Учись так, будто тебя никто не спасёт.",
  "Сегодня ты ученик — завтра эксперт.",
  "Меньше мыслей, больше практики.",
  "Воля — это навык, тренируй.",
  "Чем больше знаешь — тем меньше боишься.",
  "Не пропускай день — пропускаешь себя.",
  "Учёба — это уважение к мечте.",
  "Прогресс — это терпение плюс повторение.",
  "Страх уйдёт после первой попытки.",
  "Ты не ленивый — ты без системы.",
  "Большие результаты требуют честной оценки — и достойной оплаты.",
  "Сначала труд, потом лёгкость.",
  "Не будь зрителем — стань участником.",
  "Обучение — твоя личная эволюция.",
  "Постоянство делает талант бессмысленным.",
  "Кто учится — тот рулит.",
  "Устал? Учи 10 минут.",
  "Повышение зарплаты — это не расход, это инвестиция в стабильность и победы команды.",
  "Не можешь идеально — делай нормально.",
  "Учёба — это уважение к будущему «я».",
  "Лучшая форма уверенности — компетентность.",
  "Путь длинный, но ты не один — ты сильный.",
  "Рост начинается с честности к себе.",
  "Ты можешь больше, чем думаешь.",
  "Учись, пока другие шумят.",
  "Повторение — это не скука, это мастерство.",
  "Тебя спасёт навык, не удача.",
  "Сомнения уходят после результата.",
  "Твой темп важнее чужого мнения.",
  "Сильные люди остаются там, где их труд оценивают по заслугам.",
  "Сильные учатся молча и громко побеждают.",
  "Сегодня дисциплина — завтра свобода.",
  "Не ищи мотивацию — будь мотивацией.",
  "Выучи одно — и мир станет шире.",
  "Любопытство — это двигатель будущего.",
  "Не бойся быть новичком — бойся не расти.",
  "Сильный ум задаёт сильные вопросы.",
  "Книги — это чужая победа в твоих руках.",
  "Знания делают тебя независимым.",
  "Кто понимает — тот управляет.",
  "Учись видеть причины, не симптомы.",
  "Умный не тот, кто знает всё, а кто учится быстро.",
  "Твоя сила — в ясности мышления.",
  "Прокачай голову — и прокачаешь жизнь.",
  "Знание без действия — просто шум.",
  "Чем больше учишься, тем меньше сомнений.",
  "Понимание — это спокойная уверенность.",
  "Ум любит структуру — дай ему порядок.",
  "Ты растёшь там, где задаёшь «почему?».",
  "Каждая тема — новый уровень твоей игры.",
  "Мышление — это твой главный актив.",
  "Не копи знания — применяй.",
  "Привычка читать — привычка побеждать.",
  "Понимать — значит владеть.",
  "Глубина важнее скорости.",
  "Твоя ценность растёт с твоей экспертизой.",
  "Учись связывать идеи — это интеллект.",
  "Чем яснее мысли — тем проще решения.",
  "Учёба — это способ не зависеть от настроения.",
  "Знания — твой личный рычаг влияния.",
  "Ты не обязан знать — ты обязан учиться.",
  "Разум требует дисциплины, как тело.",
  "Хорошие решения рождаются из понимания.",
  "Не знаешь? Отлично — значит растёшь.",
  "Мышление — это навык, а не удача.",
  "Кто учится — тот всегда на шаг впереди.",
  "Путь к мудрости — это практика.",
  "Меньше эмоций — больше логики.",
  "Ты не тупишь — ты ищешь правильный ключ.",
  "Интерес — это топливо твоего мозга.",
  "Развивай вкус к сложности.",
  "Не бойся быть «не в теме» — бойся остаться.",
  "Вопросы делают тебя сильнее ответов.",
  "Мозг любит рост — не предавай его.",
  "Продажи — это не давление, это помощь.",
  "Клиент покупает уверенность, не товар.",
  "Сильная продажа начинается с понимания боли.",
  "Слушай больше, чем говоришь.",
  "Не продавай продукт — продавай результат.",
  "Цена забывается, ценность остаётся.",
  "Возражение — это интерес в форме защиты.",
  "Хороший вопрос продаёт лучше презентации.",
  "Твоя энергия — часть предложения.",
  "Уверенность передаётся голосом.",
  "Продажи — это дисциплина, не талант.",
  "Каждый отказ приближает к «да».",
  "Не бойся отказа — бойся бездействия.",
  "Чем чётче оффер — тем проще сделка.",
  "Не спорь с клиентом — веди его.",
  "Умей объяснить просто — значит ты профи.",
  "Продаёт тот, кто верит в пользу.",
  "Коммуникация решает больше, чем скидка.",
  "Холодный контакт — это тренировка силы.",
  "Продавай спокойно — уверенно — честно.",
  "Закрытие сделки начинается с первого вопроса.",
  "Настойчивость — это уважение к цели.",
  "Клиент любит ясность, а не шоу.",
  "Покажи выгоду цифрами — и будет доверие.",
  "Не умоляй — предлагай.",
  "Сильный продавец не давит, он направляет.",
  "В продажах выигрывает терпеливый.",
  "Доверие строится мелочами.",
  "Тон важнее текста.",
  "Говори фактами — это уважение.",
  "Лучший продавец — лучший слушатель.",
  "Дорогой продукт не пугает — пугает непонимание.",
  "Покажи путь клиента — и он согласится.",
  "Уверенность — это знание продукта.",
  "Работай не на продажу, а на отношения.",
  "Сначала ценность — потом цена.",
  "Продаёт спокойствие и контроль.",
  "Не объяснил — не продал.",
  "Каждый клиент — опыт, даже без сделки.",
  "Сильная аргументация = логика + выгода.",
  "Не обещай лишнего — продавай честно.",
  "Возражение — это дверь, не стена.",
  "Продажи любят регулярную практику.",
  "Побеждает тот, кто держит ритм.",
  "Умей кратко: «что, зачем, почему сейчас».",
  "Клиенту нужен смысл, не информация.",
  "Сильный оффер — половина закрытия.",
  "Твоя вера в продукт — главный триггер.",
  "Не продавай всем — продавай «своим».",
  "Продавай так, чтобы тебя рекомендовали."
];

// ==========================
// 2) Мотивация: авто-переключение раз в 1 минуту
// ==========================
let motivationIndex = 0;
let motivationInterval = null;
let countdownInterval = null;
let countdown = 60;
let motivationAuto = true;

function renderMotivation() {
  const quoteEl = document.getElementById('motivationQuote');
  if (!quoteEl) return;
  quoteEl.textContent = motivationPhrases[motivationIndex] || "Нет фраз 😅";
  localStorage.setItem('motivationIndex', String(motivationIndex));
}

function nextMotivation(manual = false) {
  motivationIndex = (motivationIndex + 1) % motivationPhrases.length;
  renderMotivation();
  if (manual) resetCountdown();
}

function prevMotivation() {
  motivationIndex = (motivationIndex - 1 + motivationPhrases.length) % motivationPhrases.length;
  renderMotivation();
  resetCountdown();
}

function resetCountdown() {
  countdown = 60;
  const cd = document.getElementById('motivationCountdown');
  if (cd) cd.textContent = countdown;
}

function tickCountdown() {
  const cd = document.getElementById('motivationCountdown');
  if (!cd) return;
  countdown -= 1;
  if (countdown <= 0) countdown = 60;
  cd.textContent = countdown;
}

function startMotivationAuto() {
  stopMotivationAuto();
  motivationAuto = true;
  const btn = document.getElementById('motivationAutoBtn');
  if (btn) btn.textContent = "⏸ Пауза";
  motivationInterval = setInterval(() => {
    nextMotivation(false);
  }, 60000);
  countdownInterval = setInterval(() => {
    tickCountdown();
    if (countdown === 60) {
      const cd = document.getElementById('motivationCountdown');
      if (cd) cd.textContent = 60;
    }
  }, 1000);
}

function stopMotivationAuto() {
  if (motivationInterval) clearInterval(motivationInterval);
  if (countdownInterval) clearInterval(countdownInterval);
  motivationInterval = null;
  countdownInterval = null;
}

function toggleMotivationAuto() {
  if (motivationAuto) {
    motivationAuto = false;
    stopMotivationAuto();
    const btn = document.getElementById('motivationAutoBtn');
    if (btn) btn.textContent = "▶️ Продолжить";
  } else {
    motivationAuto = true;
    resetCountdown();
    startMotivationAuto();
  }
}

// ==========================
// Quiz 10
// ==========================
const quizQuestions = [
  {
    "question": "1. Активы клиентов у кастодиана учитываются?",
    "options": [
      "A. Отражаются на балансовых и внебалансовых счетах",
      "B. Учитываются только на балансовых счетах",
      "C. Учитываются только на внебалансовых счетах"
    ],
    "correct": 0
  },
  {
    "question": "2. Может ли физлицо быть банковским холдингом БВУ РК?",
    "options": [
      "A. Нет, банковским холдингом может быть только юрлицо",
      "B. Да, может любое физлицо при наличии дохода",
      "C. Да, если доля владения превышает 10%"
    ],
    "correct": 0
  },
  {
    "question": "3. Форма соглашения о неустойке?",
    "options": [
      "A. Соглашение о неустойке оформляется в письменной форме",
      "B. Допускается устная форма при малой сумме",
      "C. Форма не важна, если стороны согласны"
    ],
    "correct": 0
  },
  {
    "question": "4. Кто такой корреспондент?",
    "options": [
      "A. Банк/небанковская организация, у которой открыт лоро-счёт другого банка",
      "B. Любой банк, который выдаёт кредиты юрлицам",
      "C. Только расчётная организация без банковской лицензии"
    ],
    "correct": 0
  },
  {
    "question": "5. Кто определяет порядок инвестирования КФГД?",
    "options": [
      "A. Национальный Банк определяет порядок инвестирования",
      "B. Правительство РК",
      "C. Совет директоров КФГД"
    ],
    "correct": 0
  },
  {
    "question": "6. Риск ликвидности — это?",
    "options": [
      "A. Риск потерь из-за неспособности вовремя исполнить обязательства",
      "B. Риск потерь от изменения валютного курса",
      "C. Риск потерь из-за снижения процентной маржи"
    ],
    "correct": 0
  },
  {
    "question": "7. Когда предоставляется выписка корреспондента?",
    "options": [
      "A. После завершения операционного дня",
      "B. До начала операционного дня",
      "C. Один раз в неделю по запросу"
    ],
    "correct": 0
  },
  {
    "question": "8. Кто не может быть акционером банка?",
    "options": [
      "A. Госкомпании и офшорные юрлица (кроме установленных исключений)",
      "B. Резиденты РК",
      "C. Международные финансовые организации"
    ],
    "correct": 0
  },
  {
    "question": "9. Дополнительный взнос в КФГД — ограничение?",
    "options": [
      "A. Не более 2× календарного взноса за предыдущий квартал",
      "B. Не более 5× календарного взноса за текущий квартал",
      "C. Без ограничений, по решению банка"
    ],
    "correct": 0
  },
  {
    "question": "10. Срок информирования КФГД о ликвидации и выплатах?",
    "options": [
      "A. 30 рабочих дней с даты лишения лицензии на все операции",
      "B. 30 календарных дней с даты введения временной администрации",
      "C. 10 рабочих дней с даты публикации в СМИ"
    ],
    "correct": 0
  },
  {
    "question": "11. Когда действует гарантирование КФГД?",
    "options": [
      "A. При лишении лицензии на все банковские операции",
      "B. При снижении рейтинга банка",
      "C. При замене руководства банка"
    ],
    "correct": 0
  },
  {
    "question": "12. Срок обращения в банк-агент за возмещением?",
    "options": [
      "A. 1 год с начала выплат КФГД",
      "B. 6 месяцев с даты закрытия банка",
      "C. 3 года с даты лишения лицензии"
    ],
    "correct": 0
  },
  {
    "question": "13. Курс для валютных вкладов при возмещении?",
    "options": [
      "A. Рыночный курс на дату лишения лицензии",
      "B. Курс на дату обращения вкладчика",
      "C. Средний курс за последний месяц"
    ],
    "correct": 0
  },
  {
    "question": "14. Специальный резерв КФГД?",
    "options": [
      "A. Не менее 5% от всех депозитов БВУ",
      "B. Не менее 1% от активов БВУ",
      "C. Не менее 10% от собственного капитала КФГД"
    ],
    "correct": 0
  },
  {
    "question": "15. Пруденциальные нормативы — что входит?",
    "options": [
      "A. Мин. УК, мин. СК, достаточность СК, риск на 1 заёмщика, ликвидность, валютная позиция",
      "B. Только прибыльность и ROE",
      "C. Только нормативы по операционному риску"
    ],
    "correct": 0
  },
  {
    "question": "16. Уведомление КФГД о перерегистрации наименования?",
    "options": [
      "A. В течение 5 рабочих дней после получения справки",
      "B. В течение 15 календарных дней",
      "C. В течение 30 рабочих дней"
    ],
    "correct": 0
  },
  {
    "question": "17. Клиент отвечает за неисполнение должником требования фин. агента?",
    "options": [
      "A. Нет, если договором прямо не предусмотрено иное",
      "B. Да, всегда отвечает солидарно",
      "C. Да, если должник не найден"
    ],
    "correct": 0
  },
  {
    "question": "18. Полномочия реабилитационного управляющего в АО?",
    "options": [
      "A. Ему переходят все полномочия по управлению",
      "B. Только контроль финансовой отчётности",
      "C. Только право согласовывать сделки свыше лимита"
    ],
    "correct": 0
  },
  {
    "question": "19. Одностороннее изменение ставки по займу банком?",
    "options": [
      "A. Допускается только в сторону улучшения для клиента",
      "B. Возможно в любую сторону при уведомлении",
      "C. Возможно при ухудшении финсостояния банка"
    ],
    "correct": 0
  },
  {
    "question": "20. Как доводится решение о разрешении на открытие банка?",
    "options": [
      "A. Уведомлением заявителю",
      "B. Публикацией в депозитарии финансовой отчётности",
      "C. Через рассылку всем банкам"
    ],
    "correct": 0
  },
  {
    "question": "21. Страховая сумма по имуществу не должна превышать?",
    "options": [
      "A. Действительную стоимость имущества на момент договора",
      "B. Рыночную стоимость через 1 год",
      "C. Стоимость по желанию страхователя"
    ],
    "correct": 0
  },
  {
    "question": "22. Двойное страхование — это?",
    "options": [
      "A. Один и тот же объект застрахован у разных страховщиков",
      "B. Страхование объекта и ответственности одновременно",
      "C. Продление одного полиса два раза"
    ],
    "correct": 0
  },
  {
    "question": "23. Имущество в доверительном управлении (ГК 885)?",
    "options": [
      "A. Деньги, ценные бумаги, имущественные права",
      "B. Только недвижимость",
      "C. Только движимое имущество"
    ],
    "correct": 0
  },
  {
    "question": "24. Мнимая сделка — это?",
    "options": [
      "A. Сделка для вида, без цели правовых последствий (недействительна)",
      "B. Сделка с отлагательным условием",
      "C. Сделка без нотариального удостоверения"
    ],
    "correct": 0
  },
  {
    "question": "25. Кредитор доказывает убытки при неустойке?",
    "options": [
      "A. Нет, доказывать убытки не требуется",
      "B. Да, всегда обязан доказать размер убытков",
      "C. Да, если сумма неустойки превышает долг"
    ],
    "correct": 0
  },
  {
    "question": "26. Порядок перевода денег без ИИН?",
    "options": [
      "A. Определяется актами, регулирующими банковскую деятельность",
      "B. Определяется исключительно ГК РК",
      "C. Устанавливается договором между клиентами"
    ],
    "correct": 0
  },
  {
    "question": "27. Договор банковского счёта бессрочный?",
    "options": [
      "A. Да, если не установлен срок (или по соглашению сторон)",
      "B. Нет, всегда ровно на 1 год",
      "C. Нет, максимум на 5 лет"
    ],
    "correct": 0
  },
  {
    "question": "28. При согласии на банковский холдинг одновременно выдаётся?",
    "options": [
      "A. Разрешение на значительное участие или создание дочернего банка",
      "B. Лицензия на все банковские операции",
      "C. Разрешение на выпуск облигаций банка"
    ],
    "correct": 0
  },
  {
    "question": "29. Эквайер — кто это?",
    "options": [
      "A. Банк/организация, принимающая платежи в пользу предпринимателя по картам",
      "B. Организация, выпускающая банковские карты",
      "C. Только платёжная система (Visa/Mastercard)"
    ],
    "correct": 0
  },
  {
    "question": "30. Перерегистрация юрлица — когда?",
    "options": [
      "A. Уменьшение УК, смена наименования, изменения участников в ХТ (с исключением для реестра ЦБ)",
      "B. Любое изменение адреса юрлица",
      "C. Любое изменение директора"
    ],
    "correct": 0
  },
  {
    "question": "31. ЦБ могут быть предметом лизинга?",
    "options": [
      "A. Нет, ценные бумаги не являются предметом лизинга",
      "B. Да, при согласии лизингодателя",
      "C. Да, если ЦБ государственные"
    ],
    "correct": 0
  },
  {
    "question": "32. Что не является юрлицом?",
    "options": [
      "A. Простое товарищество",
      "B. ТОО",
      "C. АО"
    ],
    "correct": 0
  },
  {
    "question": "33. Изъятие денег в БВУ со счетов физ/юрлиц возможно?",
    "options": [
      "A. По решению суда и в случаях, предусмотренных НК и спецзаконами",
      "B. По заявлению банка в одностороннем порядке",
      "C. По письму работодателя"
    ],
    "correct": 0
  },
  {
    "question": "34. Значительное участие в капитале — это?",
    "options": [
      "A. 20% и более голосующих акций",
      "B. 10% и более голосующих акций",
      "C. 50% и более голосующих акций"
    ],
    "correct": 0
  },
  {
    "question": "35. Когда эмитент может изъять карту у клиента?",
    "options": [
      "A. Срок истёк, нарушение договора, отказ клиента от карты",
      "B. При любой просрочке по кредиту 1 день",
      "C. Только по решению суда"
    ],
    "correct": 0
  },
  {
    "question": "36. Неаффилированные лица — кто?",
    "options": [
      "A. Акционеры НКО и кредитного бюро, а также недееспособные (как указано в норме)",
      "B. Любые родственники руководства банка",
      "C. Все клиенты банка по умолчанию"
    ],
    "correct": 0
  },
  {
    "question": "37. Срок отказа эмитента по платёжному документу?",
    "options": [
      "A. Не позднее 1 операционного дня",
      "B. Не позднее 3 календарных дней",
      "C. В течение 10 рабочих дней"
    ],
    "correct": 0
  },
  {
    "question": "38. Договор банковского обслуживания включает?",
    "options": [
      "A. Счёт, перевод, вклад и другие услуги (в рамках норм ГК)",
      "B. Только выпуск карты",
      "C. Только открытие депозита"
    ],
    "correct": 0
  },
  {
    "question": "39. Предельное участие БВУ в УК других юрлиц?",
    "options": [
      "A. Не более 10% собственного капитала банка",
      "B. Не более 25% собственного капитала",
      "C. Без ограничений при согласии акционеров"
    ],
    "correct": 0
  },
  {
    "question": "40. Банк-нерезидент РК открывает представительство?",
    "options": [
      "A. Вправе открыть филиал за пределами РК",
      "B. Вправе открыть представительство в РК без условий",
      "C. Не вправе открывать подразделения вообще"
    ],
    "correct": 0
  },

  {
    "question": "41. Условие открытия филиала банком-нерезидентом РК?",
    "options": [
      "A. Наличие соглашения об обмене информацией между регуляторами",
      "B. Наличие офиса и персонала в РК",
      "C. Разрешение местного акимата"
    ],
    "correct": 0
  },
  {
    "question": "42. Меры банка с неустойчивым финположением / крупными участниками / холдингом?",
    "options": [
      "A. Улучшение финсостояния и минимизация рисков по закону и требованиям регулятора",
      "B. Выплата дивидендов для стабилизации доверия",
      "C. Свободное распределение прибыли"
    ],
    "correct": 0
  },
  {
    "question": "43. С даты признания банка неустойчивым он не вправе?",
    "options": [
      "A. Распределять прибыль, дивиденды, обязательства перед крупными участниками/холдингом, вознаграждение руководству",
      "B. Принимать депозиты физлиц",
      "C. Выдавать кредиты юрлицам"
    ],
    "correct": 0
  },
  {
    "question": "44. Оплата непокрытых чеков?",
    "options": [
      "A. В пределах суммы на счёте + банковский заём по чековому договору",
      "B. Всегда за счёт банка без условий",
      "C. Только наличными через кассу"
    ],
    "correct": 0
  },
  {
    "question": "45. Срок оплаты УК нового банка?",
    "options": [
      "A. 30 календарных дней после регистрации",
      "B. 10 рабочих дней после регистрации",
      "C. До подачи документов на регистрацию"
    ],
    "correct": 0
  },
  {
    "question": "46. После регистрации филиала банк предоставляет регулятору?",
    "options": [
      "A. Нотариальную копию положения о филиале + копию доверенности руководителю",
      "B. Только уведомление по e-mail",
      "C. Только бухгалтерский баланс"
    ],
    "correct": 0
  },
  {
    "question": "47. Лицензия для переводов юрлицам включает?",
    "options": [
      "A. Приём депозитов, открытие/ведение счетов юрлиц, корр.счетов банков + отдельные виды операций",
      "B. Только кассовые операции",
      "C. Только валютно-обменные операции"
    ],
    "correct": 0
  },
  {
    "question": "48. Срок доведения решения об отнесении к неустойчивому финсостоянию?",
    "options": [
      "A. 5 рабочих дней с даты решения",
      "B. 30 календарных дней",
      "C. 1 рабочий день"
    ],
    "correct": 0
  },
  {
    "question": "49. Конфискация денег/имущества в банке возможна?",
    "options": [
      "A. На основании вступившего в силу судебного решения",
      "B. По решению банка",
      "C. По требованию клиента"
    ],
    "correct": 0
  },
  {
    "question": "50. Требования к банку при создании дочерней организации?",
    "options": [
      "A. 2 года безубыточности (консолид.) + 3 месяца соблюдения пруденциалов до подачи",
      "B. Достаточно 1 месяца прибыли",
      "C. Только решение правления"
    ],
    "correct": 0
  },
  {
    "question": "51. При несоблюдении простой письменной формы нельзя ссылаться на?",
    "options": [
      "A. Свидетельские показания",
      "B. Переписку сторон",
      "C. Акт сверки"
    ],
    "correct": 0
  },
  {
    "question": "52. Заинтересованные лица при сделках?",
    "options": [
      "A. Аффилированные лица, участвующие как представители одной стороны",
      "B. Любые клиенты банка",
      "C. Все контрагенты-нерезиденты"
    ],
    "correct": 0
  },
  {
    "question": "53. Крупная сделка АО?",
    "options": [
      "A. Выкуп/отчуждение ≥25% ценных бумаг или активов",
      "B. Любая сделка свыше 1 млн тг",
      "C. Сделка только с государством"
    ],
    "correct": 0
  },
  {
    "question": "54. Арест на деньги в банке?",
    "options": [
      "A. Суды по актам + судебные исполнители с санкцией прокурора",
      "B. Только по письму банка",
      "C. По заявлению третьего лица"
    ],
    "correct": 0
  },
  {
    "question": "55. Расходы по ликвидационному производству?",
    "options": [
      "A. Погашаются вне очереди",
      "B. В 5-й очереди",
      "C. В последней очереди"
    ],
    "correct": 0
  },
  {
    "question": "56. Максимальная неустойка по кредиту?",
    "options": [
      "A. До 90 дней — 0,5%/день; после — 0,03%/день; ≤10% от суммы займа в год",
      "B. 1%/день без ограничений",
      "C. Всегда фиксировано 20% в год"
    ],
    "correct": 0
  },
  {
    "question": "57. Приостановка обязательств после передачи активов стаббанку?",
    "options": [
      "A. 12 месяцев",
      "B. 6 месяцев",
      "C. 24 месяца"
    ],
    "correct": 0
  },
  {
    "question": "58. Новация — это?",
    "options": [
      "A. Замена одного обязательства другим",
      "B. Частичное исполнение обязательства",
      "C. Взыскание долга через суд"
    ],
    "correct": 0
  },
  {
    "question": "59. Личные неимущественные блага?",
    "options": [
      "A. Жизнь, здоровье, достоинство, честь, имя, репутация, частная жизнь, тайна, авторство",
      "B. Депозиты и ценные бумаги",
      "C. Имущество и недвижимость"
    ],
    "correct": 0
  },
  {
    "question": "60. Цена продажи акций присоединяемого общества?",
    "options": [
      "A. Собственный капитал / количество размещённых акций",
      "B. Рыночная стоимость по рекламе",
      "C. По номиналу всегда"
    ],
    "correct": 0
  },
  {
    "question": "61. Выплата дивидендов по кварталу?",
    "options": [
      "A. После аудита ФО и решения акционеров",
      "B. Сразу после закрытия месяца",
      "C. По решению директора"
    ],
    "correct": 0
  },
  {
    "question": "62. Объекты гражданских прав?",
    "options": [
      "A. Имущественные + личные неимущественные блага и права",
      "B. Только вещи",
      "C. Только деньги"
    ],
    "correct": 0
  },
  {
    "question": "63. Очередь удовлетворения залоговых кредиторов при ликвидации банка?",
    "options": [
      "A. 3-я очередь",
      "B. 1-я очередь",
      "C. Последняя очередь"
    ],
    "correct": 0
  },
  {
    "question": "64. Требования кредиторов к лишённому лицензии банку предъявляются?",
    "options": [
      "A. В ликвидационном производстве (кроме текущих расходов)",
      "B. Через коллекторов",
      "C. Только через нотариуса"
    ],
    "correct": 0
  },
  {
    "question": "65. Заявление юр/физлиц — основание для ликвидации банка?",
    "options": [
      "A. Да, заявление физлиц/юрлиц/госорганов может быть основанием",
      "B. Нет, только решение акционеров",
      "C. Да, но только по заявлению самого банка"
    ],
    "correct": 0
  },
  {
    "question": "66. Срок обмена активов стаббанком?",
    "options": [
      "A. До вступления решения суда о ликвидации",
      "B. Только после завершения ликвидации",
      "C. Только в течение 1 дня"
    ],
    "correct": 0
  },
  {
    "question": "67. Меры при приобретении статуса крупного участника без согласования?",
    "options": [
      "A. Надзорное реагирование + принудительные меры + реализация акций в 6 месяцев",
      "B. Никаких мер, если это нерезидент",
      "C. Только предупреждение без сроков"
    ],
    "correct": 0
  },
  {
    "question": "68. Обеспечение возвратности кредитов?",
    "options": [
      "A. Неустойка, залог, поручительство или договор",
      "B. Только залог недвижимости",
      "C. Только устная гарантия клиента"
    ],
    "correct": 0
  },
  {
    "question": "69. Информация по гарантиям в кредитное бюро передаётся?",
    "options": [
      "A. Да: номер, дата договора, сумма",
      "B. Нет, гарантии не передаются",
      "C. Передаётся только ФИО поручителя"
    ],
    "correct": 0
  },
  {
    "question": "70. Операции с депозитами/счетами физлиц требуют?",
    "options": [
      "A. Участия в КФГД + лицензии на банковские операции",
      "B. Только регистрации юрлица",
      "C. Только договора страхования"
    ],
    "correct": 0
  },
  {
    "question": "71. К учредительным документам относится?",
    "options": [
      "A. Учредительный договор + устав",
      "B. Только протокол собрания",
      "C. Только выписка из банка"
    ],
    "correct": 0
  },
  {
    "question": "72. Последствия несоблюдения письменной формы сделки?",
    "options": [
      "A. Лишает права на свидетельские показания; сделка ничтожна (в предусмотренных случаях)",
      "B. Никаких последствий",
      "C. Сделка всегда становится действительной"
    ],
    "correct": 0
  },
  {
    "question": "73. Когда нерезиденту не нужно согласие регулятора на статус холдинга/крупного участника?",
    "options": [
      "A. При покупке 10% у другого нерезидента-крупного участника с минимальным рейтингом",
      "B. Всегда не нужно",
      "C. Только если покупка до 1%"
    ],
    "correct": 0
  },
  {
    "question": "74. Течение исковой давности не приостанавливается при?",
    "options": [
      "A. ЧП/отсрочке по указу/военном положении/отсутствии представителя у недееспособного и т.д.",
      "B. При отпуске должника",
      "C. При смене адреса кредитора"
    ],
    "correct": 0
  },
  {
    "question": "75. Уступка требования финансовым агентом допускается?",
    "options": [
      "A. Нет, если иное не предусмотрено договором",
      "B. Да, всегда без ограничений",
      "C. Да, только через суд"
    ],
    "correct": 0
  },
  {
    "question": "76. Противоречие между учредительным договором и уставом?",
    "options": [
      "A. Договор — для учредителей; устав — для третьих лиц",
      "B. Устав всегда отменяет договор",
      "C. Договор важнее для всех"
    ],
    "correct": 0
  },
  {
    "question": "77. Исковая давность не распространяется на?",
    "options": [
      "A. Нематериальные блага, вкладчиков к банку, вред жизни/здоровью (≤3 года до иска), права собственника",
      "B. Любые денежные долги",
      "C. Только на кредиты юрлиц"
    ],
    "correct": 0
  },
  {
    "question": "78. Франшиза в страховании — это?",
    "options": [
      "A. Освобождение страховщика от ущерба ≤ определённого размера",
      "B. Дополнительная премия страхователю",
      "C. Полное покрытие без исключений"
    ],
    "correct": 0
  },
  {
    "question": "79. Договор доверительного управления включает?",
    "options": [
      "A. Предмет, срок, состав имущества, выгодоприобретатель, отчётность, получатель при прекращении",
      "B. Только предмет и цену",
      "C. Только срок и адрес сторон"
    ],
    "correct": 0
  },
  {
    "question": "80. Фьючерс — это?",
    "options": [
      "A. Обязательство купить/продать актив в срок по стандартам организованного рынка",
      "B. Договор аренды имущества",
      "C. Договор займа наличными"
    ],
    "correct": 0
  },
  {
    "question": "81. Возмещение ущерба при условной франшизе?",
    "options": [
      "A. Выплачивается, если ущерб превышает франшизу",
      "B. Не выплачивается никогда",
      "C. Выплачивается только половина"
    ],
    "correct": 0
  },
  {
    "question": "82. Какие АО не вправе выпускать конвертируемые ЦБ?",
    "options": [
      "A. Если это не предусмотрено уставом + некоммерческие АО",
      "B. Любые АО не вправе",
      "C. Только АО с убытками"
    ],
    "correct": 0
  },
  {
    "question": "83. Способы обеспечения обязательств?",
    "options": [
      "A. Неустойка, залог, гарантия/поручительство, задаток, имущество должника, гарантийный взнос, договор",
      "B. Только залог",
      "C. Только штраф"
    ],
    "correct": 0
  },
  {
    "question": "84. Договор доверительного управления включает (повтор)?",
    "options": [
      "A. Предмет, срок, состав, выгодоприобретатель, отчётность, получатель при прекращении",
      "B. Только отчётность",
      "C. Только предмет"
    ],
    "correct": 0
  },
  {
    "question": "85. Доля прямого/косвенного владения акциями считается?",
    "options": [
      "A. От размещённых (без привилегированных и выкупленных) или голосующих",
      "B. От уставного капитала в тенге",
      "C. От количества сотрудников"
    ],
    "correct": 0
  },
  {
    "question": "86. Уведомление о мерах по улучшению финсостояния?",
    "options": [
      "A. Исполнение по предписанию в указанные сроки",
      "B. Делается только после года",
      "C. Не требуется вовсе"
    ],
    "correct": 0
  },
  {
    "question": "87. Срок уведомления регулятора о выборе аудитора?",
    "options": [
      "A. 10 рабочих дней",
      "B. 30 календарных дней",
      "C. 1 рабочий день"
    ],
    "correct": 0
  },
  {
    "question": "88. Предмет уступки денежного требования?",
    "options": [
      "A. Требования с наступившим сроком + будущие деньги",
      "B. Только будущие требования",
      "C. Только просроченные долги"
    ],
    "correct": 0
  },
  {
    "question": "89. Сделки банка по ЦБ (разрешённые виды)?",
    "options": [
      "A. Международные облигации (по списку НБ), проектное финансирование/секьюритизация, свои/дочки, в обмен на реструктурированные",
      "B. Любые акции любых компаний без ограничений",
      "C. Только криптоактивы"
    ],
    "correct": 0
  },
  {
    "question": "90. Срок сложения полномочий временной администрацией?",
    "options": [
      "A. Не более 1 месяца с назначения ликвидационной комиссии",
      "B. Не ранее чем через 1 год",
      "C. В день назначения комиссии"
    ],
    "correct": 0
  },
  {
    "question": "91. Безупречная деловая репутация — это?",
    "options": [
      "A. Профессионализм + отсутствие судимости",
      "B. Только высшее образование",
      "C. Только стаж 10 лет"
    ],
    "correct": 0
  },
  {
    "question": "92. Срок исполнения обязанностей руководителем без согласования НБ?",
    "options": [
      "A. Не более 60 календарных дней",
      "B. Не более 10 рабочих дней",
      "C. Без ограничений"
    ],
    "correct": 0
  },
  {
    "question": "93. Максимальная доля привилегированных акций?",
    "options": [
      "A. 25% от общего количества",
      "B. 10%",
      "C. 50%"
    ],
    "correct": 0
  },
  {
    "question": "94. Момент вступления банка в КФГД?",
    "options": [
      "A. С внесения в реестр КФГД",
      "B. С даты регистрации банка",
      "C. С даты открытия первого филиала"
    ],
    "correct": 0
  },
  {
    "question": "95. Не входят в лимиты по корр.счетам?",
    "options": [
      "A. Межбанковский клиринг, зачёт лоро/ностро в одном банке, биржевые сделки через ЦД, переводы родительский-дочерний после реструктуризации",
      "B. Любые переводы физлицам",
      "C. Все операции в иностранной валюте"
    ],
    "correct": 0
  },
  {
    "question": "96. Респондент — это?",
    "options": [
      "A. Банк/небанк/участник МФЦА с ностро-счётом в другом банке",
      "B. Клиент, который отвечает на звонок банка",
      "C. Организация, выдающая ипотеку"
    ],
    "correct": 0
  },
  {
    "question": "97. Уведомление регулятора об изменении акционеров >10%?",
    "options": [
      "A. 15 календарных дней",
      "B. 5 рабочих дней",
      "C. 30 рабочих дней"
    ],
    "correct": 0
  },
  {
    "question": "98. Экземпляры акта передачи имущества временная админ → ликв.комиссия?",
    "options": [
      "A. 4 экземпляра (в суд, в АФР и т.д.)",
      "B. 1 экземпляр",
      "C. 2 экземпляра"
    ],
    "correct": 0
  },
  {
    "question": "99. Срок действия лицензии на банковские операции?",
    "options": [
      "A. Неограниченный",
      "B. 1 год",
      "C. 5 лет"
    ],
    "correct": 0
  },
  {
    "question": "100. Меры регулятора при неодобрении плана раннего реагирования?",
    "options": [
      "A. Меры надзорного реагирования",
      "B. Автоматическая ликвидация банка",
      "C. Отмена всех операций банка"
    ],
    "correct": 0
  },
  {
    "question": "101. Заимодатель по госзайму?",
    "options": [
      "A. Гражданин или юридическое лицо",
      "B. Только Национальный Банк",
      "C. Только иностранные банки"
    ],
    "correct": 0
  },
  {
    "question": "102. Подразделения банка для регистрации в юстиции?",
    "options": [
      "A. Филиалы + представительства",
      "B. Только кассы",
      "C. Только отделы продаж"
    ],
    "correct": 0
  },
  {
    "question": "103. Содержание бизнес-плана для открытия банка?",
    "options": [
      "A. Стратегия, бюджет, баланс/ПиУ за 3 года, маркетинг, трудовые ресурсы, управление рисками",
      "B. Только прогноз прибыли",
      "C. Только оргструктура"
    ],
    "correct": 0
  },
  {
    "question": "104. Меры надзорного реагирования — какие?",
    "options": [
      "A. Рекомендательная, улучшение финсостояния/минимизация рисков, принудительные",
      "B. Только штраф",
      "C. Только предупреждение"
    ],
    "correct": 0
  },
  {
    "question": "105. Срок оплаты УК после регистрации?",
    "options": [
      "A. 3 рабочих дня",
      "B. 30 календарных дней",
      "C. 1 год"
    ],
    "correct": 0
  },
  {
    "question": "106. Полномочия органов банка при временной администрации?",
    "options": [
      "A. Переходят в полном объёме к временной администрации",
      "B. Сохраняются у правления",
      "C. Делятся поровну между СД и правлением"
    ],
    "correct": 0
  },
  {
    "question": "107. Общий срок особого режима регулирования?",
    "options": [
      "A. 5 лет",
      "B. 1 год",
      "C. 10 лет"
    ],
    "correct": 0
  },
  {
    "question": "108. Кто утверждает правила общих условий банковских операций?",
    "options": [
      "A. Совет директоров",
      "B. Главный бухгалтер",
      "C. Кредитный комитет"
    ],
    "correct": 0
  },
  {
    "question": "109. Стаж главного бухгалтера банка?",
    "options": [
      "A. Не менее 3 лет в аудите/финансовых услугах",
      "B. Не менее 6 месяцев в банке",
      "C. Не требуется стаж"
    ],
    "correct": 0
  },
  {
    "question": "110. Квалифицированное большинство — это?",
    "options": [
      "A. Не менее ¾ голосов",
      "B. ½ голосов",
      "C. ⅔ голосов"
    ],
    "correct": 0
  },
  {
    "question": "111. Ежеквартально не позднее 10 числа регулятору предоставляется?",
    "options": [
      "A. Список крупных участников с акциями/долями",
      "B. Список всех клиентов банка",
      "C. Отчёт по рекламе банка"
    ],
    "correct": 0
  },
  {
    "question": "112. Срок предоставления плана раннего реагирования?",
    "options": [
      "A. 5 рабочих дней",
      "B. 30 календарных дней",
      "C. 1 рабочий день"
    ],
    "correct": 0
  },
  {
    "question": "113. Элементы финансовой отчётности?",
    "options": [
      "A. Активы, обязательства, капитал",
      "B. Только прибыль и убыток",
      "C. Только движение денег"
    ],
    "correct": 0
  },
  {
    "question": "114. Кредитор вправе не принимать исполнение по частям?",
    "options": [
      "A. Да, если не предусмотрено договором",
      "B. Нет, обязан принять частями всегда",
      "C. Да, но только при валютном долге"
    ],
    "correct": 0
  },
  {
    "question": "115. Сделки банка по ЦБ осуществляются?",
    "options": [
      "A. На организованном рынке или по нормативам регулятора",
      "B. Только через кассу банка",
      "C. Только по устному соглашению"
    ],
    "correct": 0
  },
  {
    "question": "116. Назначение главного бухгалтера-профессионала обязательно?",
    "options": [
      "A. В организациях публичного интереса",
      "B. Только в ТОО",
      "C. Только в микрофинансовых организациях"
    ],
    "correct": 0
  },
  {
    "question": "117. Восстановление прав на утраченные ЦБ?",
    "options": [
      "A. Через суд",
      "B. Через нотариуса",
      "C. Через банк-кастодиан"
    ],
    "correct": 0
  },
  {
    "question": "118. Долевая ценная бумага — это?",
    "options": [
      "A. ЦБ, дающая право на долю в имуществе",
      "B. ЦБ, подтверждающая долг",
      "C. ЦБ, подтверждающая залог"
    ],
    "correct": 0
  },
  {
    "question": "119. Зависимое АО — это?",
    "options": [
      "A. Другое юрлицо владеет ≥20% голосующих акций",
      "B. Владение ≥5%",
      "C. Владение 100%"
    ],
    "correct": 0
  },
  {
    "question": "120. Основания прекращения обязательств?",
    "options": [
      "A. Исполнение, зачёт, новация, прощение долга, смерть, ликвидация",
      "B. Только исполнение",
      "C. Только решение суда"
    ],
    "correct": 0
  },

  {
    "question": "121. Солидарные кредиторы — предъявление требования?",
    "options": [
      "A. Да, любой кредитор может требовать в полном объёме",
      "B. Нет, только совместно",
      "C. Да, но только по частям"
    ],
    "correct": 0
  },
  {
    "question": "122. Не включаются в коэффициенты валютной ликвидности?",
    "options": [
      "A. До востребования + овернайт от банков",
      "B. Срочные вклады физлиц",
      "C. Кредиты юрлицам"
    ],
    "correct": 0
  },
  {
    "question": "123. Залог по ипотеке с историко-культурной ценностью?",
    "options": [
      "A. Запрет внесудебной реализации",
      "B. Разрешена любая реализация",
      "C. Реализация только через акимат"
    ],
    "correct": 0
  },
  {
    "question": "124. Невыполнение нормативов ликвидности — это?",
    "options": [
      "A. Просрочки перед вкладчиками/кредиторами в отчётном периоде",
      "B. Снижение прибыли банка",
      "C. Рост комиссионных доходов"
    ],
    "correct": 0
  },
  {
    "question": "125. Бизнес-модель банка включает?",
    "options": [
      "A. Стратегия + продукты + процессы + планирование + доходность",
      "B. Только маркетинг",
      "C. Только бюджет на год"
    ],
    "correct": 0
  },
  {
    "question": "126. Оплата акций банка имуществом допускается?",
    "options": [
      "A. Крупные участники-физлица; стоимость ≥ совокупной стоимости акций",
      "B. Любой акционер может оплатить чем угодно",
      "C. Запрещено всегда"
    ],
    "correct": 0
  },
  {
    "question": "127. Состав финансовой отчётности?",
    "options": [
      "A. Баланс, ОПиУ, движение ДС, изменение капитала, пояснительная записка",
      "B. Только баланс",
      "C. Только отчёт о прибыли"
    ],
    "correct": 0
  },
  {
    "question": "128. Совокупный риск на заёмщика >10% капитала?",
    "options": [
      "A. ≤ 5× собственный капитал банка",
      "B. ≤ 1× капитал",
      "C. Без лимита"
    ],
    "correct": 0
  },
  {
    "question": "129. Эмиссионные ЦБ — это?",
    "options": [
      "A. Однородные признаки/реквизиты, единые условия обращения",
      "B. Разовые расписки",
      "C. Наличная валюта"
    ],
    "correct": 0
  },
  {
    "question": "130. Банк с ≥⅓ акций нерезидентов?",
    "options": [
      "A. Банк с иностранным участием",
      "B. Национальный банк",
      "C. Исламский банк"
    ],
    "correct": 0
  },
  {
    "question": "131. Основание бухгалтерских записей?",
    "options": [
      "A. Первичные документы",
      "B. Устные распоряжения",
      "C. Слухи/письма клиентов"
    ],
    "correct": 0
  },
  {
    "question": "132. Риск гибели/повреждения залога несёт?",
    "options": [
      "A. Залогодатель (если иное не предусмотрено)",
      "B. Банк всегда",
      "C. Нотариус"
    ],
    "correct": 0
  },
  {
    "question": "133. Качественные характеристики ФО?",
    "options": [
      "A. Понятность, уместность, надёжность, сопоставимость",
      "B. Секретность и закрытость",
      "C. Только прибыльность"
    ],
    "correct": 0
  },
  {
    "question": "134. Кому предоставляется ФО?",
    "options": [
      "A. Учредителям, статистика, регулятор, материнская компания",
      "B. Только клиентам банка",
      "C. Только СМИ"
    ],
    "correct": 0
  },
  {
    "question": "135. Срок устранения нарушений в особом режиме?",
    "options": [
      "A. 60 рабочих дней с уведомления",
      "B. 10 календарных дней",
      "C. 1 год"
    ],
    "correct": 0
  },
  {
    "question": "136. Принципы бухучёта и ФО?",
    "options": [
      "A. Начисление + непрерывность",
      "B. Только кассовый метод",
      "C. Только по желанию банка"
    ],
    "correct": 0
  },
  {
    "question": "137. Передача списков депозиторов в КФГД после лишения лицензии?",
    "options": [
      "A. 25 рабочих дней",
      "B. 5 календарных дней",
      "C. 90 дней"
    ],
    "correct": 0
  },
  {
    "question": "138. Запрещённая реклама банка?",
    "options": [
      "A. Не соответствующая действительности на дату публикации",
      "B. Любая реклама в интернете",
      "C. Реклама на двух языках"
    ],
    "correct": 0
  },
  {
    "question": "139. При решении СД о реструктуризации регулятор?",
    "options": [
      "A. Заключает письменное соглашение",
      "B. Автоматически отзывает лицензию",
      "C. Не участвует"
    ],
    "correct": 0
  },
  {
    "question": "140. Владелец источника повышенной опасности не отвечает за вред?",
    "options": [
      "A. При хищении третьими лицами (вина на завладевших)",
      "B. Если вред маленький",
      "C. Если есть страховка"
    ],
    "correct": 0
  },
  {
    "question": "141. Отлагательная сделка?",
    "options": [
      "A. Права зависят от неизвестного обстоятельства",
      "B. Сделка без условий",
      "C. Сделка на 1 день"
    ],
    "correct": 0
  },
  {
    "question": "142. ЦБ с правами по индоссаменту?",
    "options": [
      "A. Ордерная",
      "B. Именная",
      "C. Предъявительская"
    ],
    "correct": 0
  },
  {
    "question": "143. Письменное предписание регулятора?",
    "options": [
      "A. Улучшение финсостояния + минимизация рисков + план",
      "B. Только письмо “для сведения”",
      "C. Только штраф"
    ],
    "correct": 0
  },
  {
    "question": "144. Что удостоверяет ценная бумага?",
    "options": [
      "A. Имущественные права",
      "B. Личные неимущественные блага",
      "C. Только факт оплаты"
    ],
    "correct": 0
  },
  {
    "question": "145. Очередь погашения налогов/платежей юрлица при ликвидации?",
    "options": [
      "A. 4-я очередь",
      "B. 1-я",
      "C. 10-я"
    ],
    "correct": 0
  },
  {
    "question": "146. Учредители сохраняют обязательства по имуществу?",
    "options": [
      "A. ХТ, АО, кооперативы",
      "B. Только ТОО",
      "C. Только фонды"
    ],
    "correct": 0
  },
  {
    "question": "147. Источник принудительной ликвидации банка оплачивается?",
    "options": [
      "A. Средствами банка (искл. труд + СМИ)",
      "B. Государственным бюджетом всегда",
      "C. Средствами вкладчиков"
    ],
    "correct": 0
  },
  {
    "question": "148. Обратная сила новых актов ГК?",
    "options": [
      "A. Нет",
      "B. Да всегда",
      "C. Только для банков"
    ],
    "correct": 0
  },
  {
    "question": "149. Разделительный баланс?",
    "options": [
      "A. При разделении/выделении",
      "B. При покупке недвижимости",
      "C. При выпуске карт"
    ],
    "correct": 0
  },
  {
    "question": "150. Перерегистрация филиалов/представительств?",
    "options": [
      "A. При изменении наименования",
      "B. При смене бухгалтера",
      "C. При росте прибыли"
    ],
    "correct": 0
  },

  {
    "question": "151. Годовой валовый доход банка?",
    "options": [
      "A. Совокупный доход − КПН − ассигнования на обеспечение",
      "B. Только комиссионный доход",
      "C. Только чистая прибыль"
    ],
    "correct": 0
  },
  {
    "question": "152. Состав банковского конгломерата?",
    "options": [
      "A. Холдинг + банк + дочерние + значительное участие",
      "B. Только банк и филиалы",
      "C. Только МФО"
    ],
    "correct": 0
  },
  {
    "question": "153. Передача сомнительных активов дочке?",
    "options": [
      "A. Основание для отказа в разрешении на создание дочки/приобретение активов",
      "B. Разрешена всегда",
      "C. Обязательна по закону"
    ],
    "correct": 0
  },
  {
    "question": "154. Срок действия разрешения на открытие банка?",
    "options": [
      "A. До решения по лицензии",
      "B. 5 лет",
      "C. 30 дней"
    ],
    "correct": 0
  },
  {
    "question": "155. Отраслевой банк?",
    "options": [
      "A. Специализированный, отдельные законы",
      "B. Любой универсальный банк",
      "C. Только банк-агент"
    ],
    "correct": 0
  },
  {
    "question": "156. Риск от отказа ИС банка?",
    "options": [
      "A. Риск информационных технологий",
      "B. Кредитный риск",
      "C. Страновой риск"
    ],
    "correct": 0
  },
  {
    "question": "157. Стресс-тестирование?",
    "options": [
      "A. Оценка влияния событий на финсостояние",
      "B. Проверка печати документов",
      "C. Реклама новых продуктов"
    ],
    "correct": 0
  },
  {
    "question": "158. Кредитоспособность?",
    "options": [
      "A. Комплексная оценка способности платить в будущем",
      "B. Наличие карты банка",
      "C. Любой доход без подтверждения"
    ],
    "correct": 0
  },
  {
    "question": "159. Решения по дочкам банка при госучастии в акциях?",
    "options": [
      "A. Совет директоров банка",
      "B. Главный бухгалтер",
      "C. Клиенты голосованием"
    ],
    "correct": 0
  },
  {
    "question": "160. Условия субординированного долга?",
    "options": [
      "A. ≥5 лет; запрет досрочного требования; погашение без снижения пруденциалов; 10-я очередь",
      "B. 1 год и можно требовать досрочно",
      "C. Без срока и без правил"
    ],
    "correct": 0
  },
  {
    "question": "161. Макс. риски по ЛСБОО на всех заёмщиков?",
    "options": [
      "A. ≤ собственный капитал банка",
      "B. ≤ 10% капитала",
      "C. Без лимита"
    ],
    "correct": 0
  },
  {
    "question": "162. Собственный капитал?",
    "options": [
      "A. Капитал 1-го уровня + 2-го уровня",
      "B. Только прибыль банка",
      "C. Только активы"
    ],
    "correct": 0
  },
  {
    "question": "163. К7 (краткосрочные обязательства нерезидентов)?",
    "options": [
      "A. Обязательства нерезидентов / собственный капитал",
      "B. Активы / обязательства",
      "C. Доход / расход"
    ],
    "correct": 0
  },
  {
    "question": "164. Не высоколиквидные бумаги?",
    "options": [
      "A. С обратным выкупом + в залоге",
      "B. ГосЦБ",
      "C. Наличные деньги"
    ],
    "correct": 0
  },
  {
    "question": "165. Земля с/х назначения в ипотеке?",
    "options": [
      "A. Отсрочка реализации до 1 года",
      "B. Немедленная реализация",
      "C. Запрет любой ипотеки"
    ],
    "correct": 0
  },
  {
    "question": "166. План рекапитализации при нарушении достаточности капитала?",
    "options": [
      "A. 1 месяц с даты нарушения",
      "B. 1 год",
      "C. 5 дней"
    ],
    "correct": 0
  },
  {
    "question": "167. Размер риска на заёмщика соотносится с?",
    "options": [
      "A. Собственным капиталом банка",
      "B. Числом сотрудников",
      "C. Рекламным бюджетом"
    ],
    "correct": 0
  },
  {
    "question": "168. Учёт бессрочных фин.инструментов в капитале?",
    "options": [
      "A. По фактически поступившим деньгам",
      "B. По заявленной сумме",
      "C. По курсу на завтра"
    ],
    "correct": 0
  },
  {
    "question": "169. Подтверждение аудитором ежеквартальных ОПиУ/баланса?",
    "options": [
      "A. Нет, банк публикует самостоятельно по МСФО",
      "B. Да, обязательно каждый квартал",
      "C. Только по требованию клиента"
    ],
    "correct": 0
  },
  {
    "question": "170. Источники для физлица-крупного участника?",
    "options": [
      "A. Предпринимательство/труд, накопления, дарение/выигрыш/продажа дарёного (≤25%)",
      "B. Только кредит в этом же банке",
      "C. Только займы друзей"
    ],
    "correct": 0
  },
  {
    "question": "171. Освобождение залогодержателя на торгах?",
    "options": [
      "A. Гарантийный взнос + оплата цены при выигрыше",
      "B. Бесплатно без условий",
      "C. Только через суд"
    ],
    "correct": 0
  },
  {
    "question": "172. Банк создаёт для надёжности?",
    "options": [
      "A. Провизии по МСФО",
      "B. Рекламные посты",
      "C. Подарки клиентам"
    ],
    "correct": 0
  },
  {
    "question": "173. Лицензии Казпочты для банковских операций?",
    "options": [
      "A. Депозиты/счета — только с лицензией НБ; остальное — без",
      "B. Лицензия не нужна ни для чего",
      "C. Нужна лицензия для отправки писем"
    ],
    "correct": 0
  },
  {
    "question": "174. Контроль процессов в банке — обязанность?",
    "options": [
      "A. Руководства (должностных лиц)",
      "B. Любого клиента",
      "C. Только IT-отдела"
    ],
    "correct": 0
  },
  {
    "question": "175. Информация по гарантиям/поручителям в кредитное бюро?",
    "options": [
      "A. Да, с согласия",
      "B. Нет, запрещено всегда",
      "C. Только по решению суда"
    ],
    "correct": 0
  },
  {
    "question": "176. Юрлицо с коммерческой деятельностью не в форме?",
    "options": [
      "A. Фонд, учреждение, религиозное/общественное объединение, потребкооператив",
      "B. ТОО",
      "C. АО"
    ],
    "correct": 0
  },
  {
    "question": "177. Предпринимательство на имуществе госфонда?",
    "options": [
      "A. Вещное право использования",
      "B. Право собственности",
      "C. Без прав"
    ],
    "correct": 0
  },
  {
    "question": "178. Типовой устав общества утверждает?",
    "options": [
      "A. Минюст",
      "B. Акимат",
      "C. КФГД"
    ],
    "correct": 0
  },
  {
    "question": "179. Возврат по займу родовыми вещами вместо денег?",
    "options": [
      "A. Да, с согласия сторон",
      "B. Нет, никогда",
      "C. Да, но только без согласия"
    ],
    "correct": 0
  },
  {
    "question": "180. Возникновение ипотеки?",
    "options": [
      "A. С регистрации договора",
      "B. С устной договорённости",
      "C. С момента первого платежа"
    ],
    "correct": 0
  },
  {
    "question": "181. Ипотечное свидетельство?",
    "options": [
      "A. Ордерная ЦБ: исполнение + взыскание на залог",
      "B. Просто расписка",
      "C. Акция банка"
    ],
    "correct": 0
  },
  {
    "question": "182. Депозитарий ФО?",
    "options": [
      "A. Электронная база: годовая ФО, аудит, аффилированные, события",
      "B. Касса банка",
      "C. Сайт объявлений"
    ],
    "correct": 0
  },
  {
    "question": "183. Обязательный взнос в КФГД?",
    "options": [
      "A. ≤0,5% депозитов на 1-е число квартала; чрезвычайный ≤ суммы квартальных",
      "B. 5% от капитала",
      "C. Любой размер"
    ],
    "correct": 0
  },
  {
    "question": "184. Принципы КФГД?",
    "options": [
      "A. Обязательность, прозрачность, снижение рисков, накопительный резерв",
      "B. Добровольность участия",
      "C. Без резервов"
    ],
    "correct": 0
  },
  {
    "question": "185. Открытие филиала резидентом за рубежом?",
    "options": [
      "A. Решение СД + соглашение регуляторов + уведомление НБ в 30 дней",
      "B. Только приказ директора",
      "C. Без уведомлений"
    ],
    "correct": 0
  },
  {
    "question": "186. Уведомление КФГД об изменении наименования?",
    "options": [
      "A. 10 рабочих дней после получения документов",
      "B. 1 день",
      "C. 3 месяца"
    ],
    "correct": 0
  },
  {
    "question": "187. Основания исключения из КФГД?",
    "options": [
      "A. Лишение/сдача лицензии, реорганизация/ликвидация, неисполнение обязанностей",
      "B. Смена директора",
      "C. Рост активов"
    ],
    "correct": 0
  },
  {
    "question": "188. Разрешённая деятельность банка на РЦБ?",
    "options": [
      "A. Брокер/дилер с госЦБ, кастодиальная, трансфер-агент",
      "B. Только продажа авто",
      "C. Только криптотрейдинг"
    ],
    "correct": 0
  },
  {
    "question": "189. Макс. доля банка в других организациях?",
    "options": [
      "A. 10% СК банка + 10% от размещённых акций",
      "B. 50% без ограничений",
      "C. 100% всегда"
    ],
    "correct": 0
  },
  {
    "question": "190. Комитет по кадрам и вознаграждению?",
    "options": [
      "A. Председатель — независимый член СД; члены с опытом HR-управления",
      "B. Председатель — главный бухгалтер",
      "C. Только внешние консультанты"
    ],
    "correct": 0
  },
  {
    "question": "191. Периодичность аудита?",
    "options": [
      "A. Общая ≤1 раз/год; один вопрос ≤1 раз/3 года",
      "B. Каждый день",
      "C. Раз в 10 лет"
    ],
    "correct": 0
  },
  {
    "question": "192. Собрание СД после фингода?",
    "options": [
      "A. ≤5 месяцев",
      "B. ≤30 дней",
      "C. Через 2 года"
    ],
    "correct": 0
  },
  {
    "question": "193. Отрицательный собственный капитал?",
    "options": [
      "A. Обязательства больше активов",
      "B. Активы больше обязательств",
      "C. Доход больше расхода"
    ],
    "correct": 0
  },
  {
    "question": "194. Запрет на должность председателя правления?",
    "options": [
      "A. Крупный участник",
      "B. Любой сотрудник банка",
      "C. Клиент банка"
    ],
    "correct": 0
  },
  {
    "question": "195. Однородные фин.инструменты со ставкой?",
    "options": [
      "A. Один эмитент, равная доходность/валюта/срок",
      "B. Разные эмитенты, разные условия",
      "C. Только наличные"
    ],
    "correct": 0
  },
  {
    "question": "196. Повторное избрание руководителя после отказа регулятора?",
    "options": [
      "A. Через 90 календарных дней (≤2 раза за 12 мес.)",
      "B. На следующий день",
      "C. Через 5 лет"
    ],
    "correct": 0
  },
  {
    "question": "197. Срок реструктуризации при неспособности исполнять отдельным кредиторам?",
    "options": [
      "A. Не определён",
      "B. Жёстко 30 дней",
      "C. Жёстко 10 лет"
    ],
    "correct": 0
  },
  {
    "question": "198. Стаж кандидата в правление (банки/аудит/АФР)?",
    "options": [
      "A. ≥2 года (≥1 год руководящая)",
      "B. 3 месяца",
      "C. Стаж не нужен"
    ],
    "correct": 0
  },
  {
    "question": "199. Банк-кастодиан — аффилированное лицо клиента?",
    "options": [
      "A. Нет (искл. НБ)",
      "B. Да, всегда",
      "C. Да, если клиент физлицо"
    ],
    "correct": 0
  },
  {
    "question": "200. Информация при рассмотрении займа?",
    "options": [
      "A. Скоринг свой + данные кредитного бюро",
      "B. Только анкета клиента",
      "C. Только поручитель"
    ],
    "correct": 0
  },

  {
    "question": "201. Реализация акций в залоге юрлица банком?",
    "options": [
      "A. В срок ≤12 месяцев",
      "B. В срок ≤30 дней",
      "C. Без срока"
    ],
    "correct": 0
  },
  {
    "question": "202. Информация при рассмотрении займа (повтор)?",
    "options": [
      "A. Скоринг банка + кредитное бюро",
      "B. Только справка с работы",
      "C. Только паспорт"
    ],
    "correct": 0
  },
  {
    "question": "203. Банк не изменил рекламу по требованию регулятора?",
    "options": [
      "A. Регулятор публикует опровержение за счёт банка",
      "B. Ничего не происходит",
      "C. Рекламу удаляет клиент"
    ],
    "correct": 0
  },
  {
    "question": "204. Зачисление принятых наличных в рабочее время?",
    "options": [
      "A. В тот же день",
      "B. На следующий месяц",
      "C. Через 3 дня"
    ],
    "correct": 0
  },
  {
    "question": "205. Кредитная политика банка утверждается?",
    "options": [
      "A. Кредитным комитетом",
      "B. Кассиром",
      "C. Клиентами"
    ],
    "correct": 0
  },
  {
    "question": "206. Передача в залог без СД?",
    "options": [
      "A. ≤10% капитала",
      "B. ≤50% капитала",
      "C. Без ограничений"
    ],
    "correct": 0
  },
  {
    "question": "207. Консервация банка за счёт?",
    "options": [
      "A. Средств банка",
      "B. Средств вкладчиков",
      "C. Госбюджета всегда"
    ],
    "correct": 0
  },
  {
    "question": "208. Извещение регулятора дочкой об изменениях учредительных документов?",
    "options": [
      "A. ≤30 календарных дней",
      "B. 1 день",
      "C. 1 год"
    ],
    "correct": 0
  },
  {
    "question": "209. Обязательный аудит?",
    "options": [
      "A. Все банки по году (кроме лишённых лицензии/в ликвидации)",
      "B. Только по желанию",
      "C. Только раз в 5 лет"
    ],
    "correct": 0
  },
  {
    "question": "210. Политика комплаенс-риска?",
    "options": [
      "A. Комитет по рискам правления",
      "B. Отдел продаж",
      "C. Клиентский сервис"
    ],
    "correct": 0
  },
  {
    "question": "211. ЦБ исламского банка?",
    "options": [
      "A. Акции, паи ИФ, арендные/участия сертификаты, иные",
      "B. Только облигации",
      "C. Только фьючерсы"
    ],
    "correct": 0
  },
  {
    "question": "212. Не относятся к операциям исламского банка?",
    "options": [
      "A. Факторинг + форфейтинг",
      "B. Мурабаха",
      "C. Иджара"
    ],
    "correct": 0
  },
  {
    "question": "213. Предельный срок консервации?",
    "options": [
      "A. 1 год",
      "B. 10 лет",
      "C. 30 дней"
    ],
    "correct": 0
  },
  {
    "question": "214. Исковая давность по требованиям к заёмщикам?",
    "options": [
      "A. 5 лет",
      "B. 1 год",
      "C. 10 лет"
    ],
    "correct": 0
  },
  {
    "question": "215. Изъятие денег со счетов без согласия?",
    "options": [
      "A. Да (ошибка зачисления + подделка документов)",
      "B. Нет, никогда",
      "C. Только по просьбе клиента"
    ],
    "correct": 0
  },
  {
    "question": "216. Документы для >25% крупного участника?",
    "options": [
      "A. Бизнес-план на 5 лет, утверждённый НБ",
      "B. Только паспорт",
      "C. Только справка о зарплате"
    ],
    "correct": 0
  },
  {
    "question": "217. При консервации банка регулятор назначает?",
    "options": [
      "A. Временную администрацию/управляющего на 1 год",
      "B. Нового кассира",
      "C. Только аудитора"
    ],
    "correct": 0
  },
  {
    "question": "218. Передача информации по счетам юрлица?",
    "options": [
      "A. Первый руководитель/следователь + санкция прокурора",
      "B. Любой сотрудник банка",
      "C. Любой контрагент"
    ],
    "correct": 0
  },
  {
    "question": "219. Очередь требований ЛСБОО по депозитам исламского банка?",
    "options": [
      "A. 4-я (включая пенсионные/страховые)",
      "B. 1-я",
      "C. 10-я"
    ],
    "correct": 0
  },
  {
    "question": "220. Приобретение акций при переходе залога в собственность?",
    "options": [
      "A. Да, ≤10% капитала банка",
      "B. Нет, запрещено",
      "C. Да, без ограничений"
    ],
    "correct": 0
  },
  {
    "question": "221. Исполнение решения банковского омбудсмена?",
    "options": [
      "A. Обязательно",
      "B. Рекомендательно",
      "C. По желанию клиента"
    ],
    "correct": 0
  },
  {
    "question": "222. Рассмотрение ходатайства о добровольной реорганизации?",
    "options": [
      "A. ≤2 месяца с приёма",
      "B. 1 день",
      "C. 1 год"
    ],
    "correct": 0
  },
  {
    "question": "223. Справки о счетах при смерти владельца?",
    "options": [
      "A. Завещанные, суды/нотариусы, консульства, наследники",
      "B. Любой родственник без документов",
      "C. Только работодатель"
    ],
    "correct": 0
  },
  {
    "question": "224. Не банковская тайна по кредитам?",
    "options": [
      "A. Кредиты в ликвидации",
      "B. Все кредиты всегда",
      "C. Только ипотека"
    ],
    "correct": 0
  },
  {
    "question": "225. Банковский депозитный сертификат?",
    "options": [
      "A. Именная неэмиссионная: номинал + вознаграждение в сроки",
      "B. Акция на предъявителя",
      "C. Электронный чек"
    ],
    "correct": 0
  },
  {
    "question": "226. СМИ для дополнительных публикаций?",
    "options": [
      "A. Устав общества (помимо депозитария)",
      "B. Любой блог",
      "C. Только телевидение"
    ],
    "correct": 0
  },
  {
    "question": "227. Запрещённые действия временной администрации?",
    "options": [
      "A. Взаимозачёты + уступка прав кредиторам",
      "B. Проведение инвентаризации",
      "C. Подготовка отчётности"
    ],
    "correct": 0
  },
  {
    "question": "228. Холдинг-нерезидент с ≥25% в банке РК?",
    "options": [
      "A. Финорганизация под консолидированным надзором в своей стране",
      "B. Любая компания без условий",
      "C. Только офшор"
    ],
    "correct": 0
  },
  {
    "question": "229. Консервация исламского банка?",
    "options": [
      "A. Собрание акционеров + согласование регулятора",
      "B. Только решение кассира",
      "C. Автоматически при убытке"
    ],
    "correct": 0
  },
  {
    "question": "230. Прекращение ипотечного свидетельства?",
    "options": [
      "A. Исполнение/передача/нет требования 30 дней/утрата предмета",
      "B. Только по решению клиента",
      "C. Только через 10 лет"
    ],
    "correct": 0
  },
  {
    "question": "231. Запреты банку-кастодиану?",
    "options": [
      "A. Использовать активы в своих интересах + передавать в залог",
      "B. Хранить активы клиентов",
      "C. Вести учёт"
    ],
    "correct": 0
  },
  {
    "question": "232. Стабилизационный банк?",
    "options": [
      "A. Создан регулятором для передачи активов/обязательств из консервации",
      "B. Частный банк для рекламы",
      "C. Банк-агент КФГД"
    ],
    "correct": 0
  },
  {
    "question": "233. Макс. доля участия банка в юрлице?",
    "options": [
      "A. ≤50% собственного капитала",
      "B. 100% всегда",
      "C. ≤5% всегда"
    ],
    "correct": 0
  },
  {
    "question": "234. Основания реструктуризации банка?",
    "options": [
      "A. Неспособность исполнять обязательства перед отдельными кредиторами из-за нехватки денег",
      "B. Рост прибыли",
      "C. Смена логотипа"
    ],
    "correct": 0
  },
  {
    "question": "235. Бланковый заём?",
    "options": [
      "A. Заём клиенту с высокой кредитоспособностью/надёжностью",
      "B. Заём без договора",
      "C. Заём только под залог"
    ],
    "correct": 0
  },
  {
    "question": "236. Преимущественная покупка акций при конвертации в реструктуризации?",
    "options": [
      "A. Нет",
      "B. Да всегда",
      "C. Да только нерезидентам"
    ],
    "correct": 0
  },
  {
    "question": "237. Операции ипотечной организации?",
    "options": [
      "A. Инвестиционная, литература по ипотеке, реализация своего имущества",
      "B. Приём депозитов физлиц",
      "C. Выпуск наличных денег"
    ],
    "correct": 0
  },
  {
    "question": "238. Признаки неустойчивого положения юрлица-крупного участника?",
    "options": [
      "A. <2 лет, обяз.>активов, убытки 2 года, просрочка банку, ухудшение, имущества недостаточно",
      "B. Рост выручки",
      "C. Наличие сайта"
    ],
    "correct": 0
  },
  {
    "question": "239. Срок избрания омбудсмена?",
    "options": [
      "A. 2 года",
      "B. 6 месяцев",
      "C. 5 лет"
    ],
    "correct": 0
  },
  {
    "question": "240. Выплата КФГД сверх спецрезерва?",
    "options": [
      "A. За счёт НБ",
      "B. За счёт вкладчиков",
      "C. За счёт акимата"
    ],
    "correct": 0
  },
  {
    "question": "241. Несоблюдение раскрытия общих условий?",
    "options": [
      "A. Приостановление/лишение лицензии (все/отдельные)",
      "B. Ничего",
      "C. Только штраф 1 МРП"
    ],
    "correct": 0
  },
  {
    "question": "242. Запрещённые сделки главбуха?",
    "options": [
      "A. С собой/близкими, юрлицом родственника-должностного, супругами/их родственниками",
      "B. Любые сделки с клиентами",
      "C. Только сделки в валюте"
    ],
    "correct": 0
  },
  {
    "question": "243. Запрет участия ипотечной организации?",
    "options": [
      "A. Финорганизации, юрлица с высшей категорией на фонде, автоматизация ипотеки, ТОО/АО",
      "B. Любые физлица",
      "C. Только нерезиденты"
    ],
    "correct": 0
  },
  {
    "question": "244. Уведомление регулятора при наступлении статуса холдинга/крупного участника?",
    "options": [
      "A. 10 календарных дней",
      "B. 1 день",
      "C. 3 месяца"
    ],
    "correct": 0
  },
  {
    "question": "245. Цель аудита?",
    "options": [
      "A. Точность/полнота отчётности, соответствие законам РК/ВНД банка",
      "B. Увеличить продажи",
      "C. Снизить ставки"
    ],
    "correct": 0
  },
  {
    "question": "246. Уведомление крупным участником об изменении количества акций?",
    "options": [
      "A. ≤30 календарных дней",
      "B. ≤3 дня",
      "C. Не требуется"
    ],
    "correct": 0
  },
  {
    "question": "247. Финансирование банковского омбудсмена?",
    "options": [
      "A. Взносы банков",
      "B. Бюджет государства",
      "C. Взносы клиентов"
    ],
    "correct": 0
  },
  {
    "question": "248. Размещение акций по требованию регулятора?",
    "options": [
      "A. 5 рабочих дней; публикация 2 языками; равные условия; цена по правлению",
      "B. 1 год на размещение",
      "C. Без публикаций"
    ],
    "correct": 0
  },
  {
    "question": "249. Счётная комиссия?",
    "options": [
      "A. При ≥100 акционеров",
      "B. При любом количестве",
      "C. Никогда не нужна"
    ],
    "correct": 0
  },
  {
    "question": "250. Не голосующие акции?",
    "options": [
      "A. Выкупленные обществом + в номинальном держании без учёта в ЦД",
      "B. Все привилегированные",
      "C. Все простые"
    ],
    "correct": 0
  },
  {
    "question": "251. Председатель АО может быть председателем в другом АО?",
    "options": [
      "A. Нет",
      "B. Да без ограничений",
      "C. Да, только у конкурента"
    ],
    "correct": 0
  },
  {
    "question": "252. Предложение преимущественной покупки акций АО срок?",
    "options": [
      "A. ≤10 календарных дней (уведомление 2 языками в ЦД)",
      "B. 30 дней",
      "C. 1 день"
    ],
    "correct": 0
  },
  {
    "question": "253. Кодекс корпоративного управления АО регулирует?",
    "options": [
      "A. Отношения акционеров/общества/органов/заинтересованных лиц",
      "B. Только бухгалтерию",
      "C. Только IT"
    ],
    "correct": 0
  },
  {
    "question": "254. Сведения об аффилированных в банк?",
    "options": [
      "A. ≤7 календарных дней с момента аффилированности",
      "B. 1 год",
      "C. Не подаются"
    ],
    "correct": 0
  },
  {
    "question": "255. Порядок предоставления сведений об аффилированных?",
    "options": [
      "A. Устав",
      "B. Реклама банка",
      "C. Договор аренды"
    ],
    "correct": 0
  },
  {
    "question": "256. Учёт аффилированных банком ведётся по?",
    "options": [
      "A. Инфо от лиц банка + ЦД по крупным акционерам",
      "B. Только по слухам",
      "C. Только по соцсетям"
    ],
    "correct": 0
  },
  {
    "question": "257. Кумулятивное голосование?",
    "options": [
      "A. Одна акция — один голос на СД (в рамках механизма кумуляции)",
      "B. Один человек — один голос",
      "C. Голосуют только нерезиденты"
    ],
    "correct": 0
  },
  {
    "question": "258. Выбор банка-агента для гарантированных депозитов?",
    "options": [
      "A. Правление КФГД",
      "B. Клиенты голосованием",
      "C. Национальный Банк напрямую всегда"
    ],
    "correct": 0
  },
  {
    "question": "259. Кто может созвать членов СД?",
    "options": [
      "A. 1 член СД, внутренний/внешний аудит, крупный акционер",
      "B. Только председатель правления",
      "C. Только HR"
    ],
    "correct": 0
  },
  {
    "question": "260. Требования к банку-агенту КФГД?",
    "options": [
      "A. Соглашение с условиями/порядком перечисления",
      "B. Только наличие сайта",
      "C. Только наличие отделений"
    ],
    "correct": 0
  },
  {
    "question": "261. Ухудшение финположения банка ведёт к?",
    "options": [
      "A. Принудительным мерам надзорного реагирования",
      "B. Росту дивидендов",
      "C. Отмене пруденциалов"
    ],
    "correct": 0
  },
  {
    "question": "262. Члены СД из физлиц?",
    "options": [
      "A. Акционеры, представители акционеров, не акционеры/представители",
      "B. Только госслужащие",
      "C. Только клиенты"
    ],
    "correct": 0
  },
  {
    "question": "263. Действительность чека?",
    "options": [
      "A. 10 календарных дней со следующего после выписки",
      "B. 1 год",
      "C. 1 день"
    ],
    "correct": 0
  },
  {
    "question": "264. Отказ чекодержателя от частичного платежа?",
    "options": [
      "A. Да",
      "B. Нет",
      "C. Только по решению суда"
    ],
    "correct": 0
  },
  {
    "question": "265. Участники особого режима?",
    "options": [
      "A. Финорганизации/юрлица в финсфере, концентрация ресурсов, платёжные услуги",
      "B. Только госорганы",
      "C. Только ИП"
    ],
    "correct": 0
  },
  {
    "question": "266. Необеспеченное обязательство при ликвидации?",
    "options": [
      "A. 10-я очередь",
      "B. 1-я очередь",
      "C. 3-я очередь"
    ],
    "correct": 0
  },
  {
    "question": "267. Санкция за ≥3 нарушения в 12 месяцев?",
    "options": [
      "A. Приостановление/лишение лицензии, отдельные операции",
      "B. Только устное замечание",
      "C. Ничего"
    ],
    "correct": 0
  },
  {
    "question": "268. Уведомление кредиторам о разделении АО?",
    "options": [
      "A. ≤2 месяца после решения",
      "B. 5 дней",
      "C. 1 год"
    ],
    "correct": 0
  },
  {
    "question": "269. Решение по статусу крупного участника/холдинга срок?",
    "options": [
      "A. ≤50 рабочих дней",
      "B. 5 дней",
      "C. 180 дней"
    ],
    "correct": 0
  },
  {
    "question": "270. Дочки банковского холдинга могут создавать свои дочки?",
    "options": [
      "A. Нет",
      "B. Да всегда",
      "C. Да без согласований"
    ],
    "correct": 0
  },
  {
    "question": "271. Повышение ставки по кредиту?",
    "options": [
      "A. Не ранее 3 лет по соглашению",
      "B. В любой день односторонне",
      "C. Только через 10 лет"
    ],
    "correct": 0
  },
  {
    "question": "272. Ассоциация Финансистов Казахстана?",
    "options": [
      "A. Некоммерческая организация",
      "B. Государственный орган",
      "C. Коммерческий банк"
    ],
    "correct": 0
  },
  {
    "question": "273. Срок предоставления ФО?",
    "options": [
      "A. Не позднее 31 мая за предыдущий год",
      "B. До 31 декабря",
      "C. В любой день"
    ],
    "correct": 0
  },
  {
    "question": "274. Дочка банка создаёт свою дочку?",
    "options": [
      "A. Нет (искл. нерезиденты по техсопровождению)",
      "B. Да всегда",
      "C. Да без ограничений"
    ],
    "correct": 0
  },
  {
    "question": "275. Финорганизации — публичный интерес?",
    "options": [
      "A. Да (искл. обменники с лицензией НБРК)",
      "B. Нет никогда",
      "C. Только ТОО"
    ],
    "correct": 0
  },
  {
    "question": "276. Кто может не вести бухучёт?",
    "options": [
      "A. 3 условия: спецналог + не на НДС + не монополия",
      "B. Любое ТОО",
      "C. Любой банк"
    ],
    "correct": 0
  },
  {
    "question": "277. Письменное соглашение с регулятором?",
    "options": [
      "A. Улучшение финсостояния, устранение рисков/нарушений, ограничения",
      "B. Только про рекламу",
      "C. Только про зарплаты"
    ],
    "correct": 0
  },
  {
    "question": "278. Кредиты участникам банковского конгломерата?",
    "options": [
      "A. Допускаются",
      "B. Запрещены всегда",
      "C. Только наличными"
    ],
    "correct": 0
  },
  {
    "question": "279. Документ учредителя-нерезидента ЮЛ для открытия банка?",
    "options": [
      "A. Уведомление регулятора своей страны о разрешении/что не требуется",
      "B. Любая справка из банка",
      "C. Только ИИН"
    ],
    "correct": 0
  },
  {
    "question": "280. Заимодатель?",
    "options": [
      "A. Банк или юрлицо с лицензией на кредиты",
      "B. Только физлицо",
      "C. Только государство"
    ],
    "correct": 0
  },
  {
    "question": "281. Содержание заявления на открытие банка?",
    "options": [
      "A. 2 языка + адрес заявителя",
      "B. Только на русском",
      "C. Только email"
    ],
    "correct": 0
  },
  {
    "question": "282. Дочерний банк от родительского нерезидента?",
    "options": [
      "A. При определённом рейтинге по регулятору",
      "B. Без условий",
      "C. Запрещено всегда"
    ],
    "correct": 0
  },
  {
    "question": "283. Крупный участник/холдинг при увеличении акций предоставляет?",
    "options": [
      "A. Источники средств + подтверждающие документы",
      "B. Только устное объяснение",
      "C. Только паспорт"
    ],
    "correct": 0
  },
  {
    "question": "284. Повторная подача кандидата-руководителя после 2 отказов?",
    "options": [
      "A. Через 12 месяцев",
      "B. Через 1 день",
      "C. Через 3 месяца"
    ],
    "correct": 0
  },
  {
    "question": "285. Выкуп акций правительством РК — срок вернуть свидетельство?",
    "options": [
      "A. ≤5 календарных дней",
      "B. 30 дней",
      "C. 1 год"
    ],
    "correct": 0
  },
  {
    "question": "286. Члены правления от правительства при покупке акций?",
    "options": [
      "A. ≤30% состава",
      "B. 100% состава",
      "C. Запрещено"
    ],
    "correct": 0
  },
  {
    "question": "287. Член правления в других организациях?",
    "options": [
      "A. Да, с согласия СД (кроме председателя в других банках)",
      "B. Нет никогда",
      "C. Да без условий"
    ],
    "correct": 0
  },
  {
    "question": "288. Действительный отчёт аудитора?",
    "options": [
      "A. Независим от акционеров + лицензия на аудит",
      "B. Любой отчёт без лицензии",
      "C. Отчёт сотрудника банка"
    ],
    "correct": 0
  },
  {
    "question": "289. Возмещение вреда при смерти?",
    "options": [
      "A. Иждивенцы, ребёнок, супруг/родители по уходу, родственники <14 лет",
      "B. Только работодатель",
      "C. Только друзья"
    ],
    "correct": 0
  },
  {
    "question": "290. Работники аудита в правление/СД?",
    "options": [
      "A. Нет",
      "B. Да всегда",
      "C. Да, если стаж 1 год"
    ],
    "correct": 0
  },
  {
    "question": "291. Сервитут?",
    "options": [
      "A. Ограниченное пользование чужой недвижимостью",
      "B. Полная собственность",
      "C. Аренда авто"
    ],
    "correct": 0
  },
  {
    "question": "292. Копии аудиторского отчёта юрлицам конгломерата?",
    "options": [
      "A. ≤10 календарных дней",
      "B. 1 год",
      "C. Не выдаются"
    ],
    "correct": 0
  },
  {
    "question": "293. Конвертирование ЦБ в простые акции (при реструктуризации)?",
    "options": [
      "A. Проспект + план реструктуризации + решение регулятора + план реабилитации",
      "B. Только решение правления",
      "C. Только письмо клиента"
    ],
    "correct": 0
  },
  {
    "question": "294. Отмена разрешения на открытие банка?",
    "options": [
      "A. Добровольное прекращение, суд, нет регистрации в НАО 2 мес., нет лицензии 1 год",
      "B. Только при смене названия",
      "C. Только при росте активов"
    ],
    "correct": 0
  },
  {
    "question": "295. Решения СД по дочкам при госакциях?",
    "options": [
      "A. При наличии акций банка у правительства РК",
      "B. Только при IPO",
      "C. Никогда"
    ],
    "correct": 0
  },
  {
    "question": "296. Уведомление акционеров об увеличении акций для пруденциалов?",
    "options": [
      "A. 10 раб. дней очно / 15 заочно",
      "B. 1 день",
      "C. 1 год"
    ],
    "correct": 0
  },
  {
    "question": "297. Периодичность/размер дивидендов на акцию?",
    "options": [
      "A. Определяется уставом",
      "B. Всегда ежемесячно",
      "C. Всегда 50% прибыли"
    ],
    "correct": 0
  },
  {
    "question": "298. Риск на 1 заёмщика к капиталу?",
    "options": [
      "A. ЛСБОО 0,10; беззалоговые 0,10; залоговые 0,25 +2 мес. платежей",
      "B. Всегда 1,0",
      "C. Без лимита"
    ],
    "correct": 0
  },
  {
    "question": "299. Стресс-тестирование включает?",
    "options": [
      "A. Достаточность капитала, риски среды, бизнес-модель, участие СД",
      "B. Только кассу",
      "C. Только рекламу"
    ],
    "correct": 0
  },
  {
    "question": "300. Не включается в риск на 1 заёмщика?",
    "options": [
      "A. Гос/квази/фонды по ЦБ + требования к дочке",
      "B. Любые кредиты физлицам",
      "C. Любые депозиты"
    ],
    "correct": 0
  },
  {
    "question": "301. Компоненты капитала 2 уровня?",
    "options": [
      "A. Субординированный долг − субордолг банка",
      "B. Только прибыль",
      "C. Только активы"
    ],
    "correct": 0
  },
  {
    "question": "302. Запрет выкупа размещённых акций обществом?",
    "options": [
      "A. До ГОСА/до отчёта учредителям/нарушение мин.УК/неплатёжеспособность/ликвидация",
      "B. Запрета нет",
      "C. Можно всегда"
    ],
    "correct": 0
  },
  {
    "question": "303. Ответственность клиента перед фин. агентом?",
    "options": [
      "A. Да, за недействительность требования",
      "B. Нет никогда",
      "C. Только при просрочке"
    ],
    "correct": 0
  },
  {
    "question": "304. Процедуры перед торгами залога?",
    "options": [
      "A. Уведомление, объявление ≥30 дней, запрет сделок, ≥10 дней между публикацией и торгами",
      "B. Сразу торги без уведомления",
      "C. Только звонок клиенту"
    ],
    "correct": 0
  },
  {
    "question": "305. Запрет акционерам-нерезидентам банка?",
    "options": [
      "A. Владение голосующими акциями резидентов из офшоров без письма регулятора своей страны",
      "B. Любое владение нерезидентов",
      "C. Владение до 1%"
    ],
    "correct": 0
  },
  {
    "question": "306. Запрет выплаты дивидендов?",
    "options": [
      "A. Отрицательный капитал, банкротство/реабилитация, снижение пруденциалов, риски/нарушения",
      "B. При росте прибыли",
      "C. Если клиент жалуется"
    ],
    "correct": 0
  },
  {
    "question": "307. Документы бухгалтерской деятельности?",
    "options": [
      "A. Первичные, регистры, ФО, учётная политика",
      "B. Только кассовые ордера",
      "C. Только договоры займа"
    ],
    "correct": 0
  },
  {
    "question": "308. Обязаны вести учёт по МСФО?",
    "options": [
      "A. Крупное предпринимательство + организации публичного интереса",
      "B. Любой ИП",
      "C. Только физлица"
    ],
    "correct": 0
  }
];

// ==========================
// Основные вопросы/ответы (308)
// ==========================
const questionsAndAnswers = [
        {"question":"1. В системе учета кастодиана активы клиентов учитываются", "answer":"На балансовых и внебалансовых счетах (НБ РК №184 п.32 №4)"},
        {"question":"2. Может ли Физ лицо быть банковским холдингом для БВУ РК", "answer":"Нет. Банковский Холдинг это юр. лицо"},
        {"question":"3. В какой форме соглашение о неустойке", "answer":"Письменной (ГК РК ст. 294)"},
        {"question":"4. Определение Корреспондента", "answer":"Банк или небанковская организация открывшие счета для другого банка (лоро счет) №210"},
        {"question":"5. Кто определяет порядок инвестирования КФГД", "answer":"Национальный Банк (Закон о гарантировании вкладов)"},
        {"question":"6. Вероятность финансовых потерь в результате неспособности выполнить обязательства в срок", "answer":"Риск ликвидности (НБ РК №188)"},
        {"question":"7. Когда корреспондент составляет выписку о движении денег", "answer":"После завершения операционного дня (НБРК № 210)"},
        {"question":"8. Кто не может быть акционером банка", "answer":"Гос компании, Юридические лица в офшорах (исключения Нац Холдинги, Проблемные фонды, Филиалы банков нерезидентов"},
        {"question":"9. В каком размере Банку нужно вносить доп взнос в КФГД", "answer":"Размер дополнительного взноса не должен превышать двухкратный размер календарного взноса за предыдущий квартал"},
        {"question":"10. В какой срок КФГД информирует о ликвидации банка и начале возмещения с указанием времени и наименования другого банка", "answer":"30 рабочих дней с даты лишения лицензии на проведение всех операции"},
        {"question":"11. В каком случае КФГД обеспечивает гарантирование депозитов", "answer":"В случае лишения Банка лицензии на все операции"},
        {"question":"12. В какой срок клиент вправе обратиться в банк-агент за возмещением депозита после ликвидации своего банка", "answer":"В течение 1 года с даты начала выплаты КФГД"},
        {"question":"13. Какой курс обмена валют при расчете гарантированного возмещения по вкладам в иностранной валюте", "answer":"Рыночный курс на дату лишения банковской лицензии"},
        {"question":"14. Какой размер специального резерва КФГД", "answer":"Не менее 5% от суммы всех депозитов БВУ"},
        {"question":"15. Состав пруденциальных нормативов", "answer":"Минимальный размер уставного капитала\nМинимальный размер собственного капитала\nКоэффициент достаточности собственного капитала\nМаксимальный размер риска на 1 заемщика\nКоэффициент ликвидности \nЛимиты валютной позиции"},
        {"question":"16. В какой срок банк уведомляет КФГД о перерегистрации наименования", "answer":"5 рабочих дней со дня получения банком справки о перерегистрации"},
        {"question":"17. Отвечает ли клиент (поставщик) за неисполнение должником требования предъявления финансовым агентом к исполнению", "answer":"Нет, если иное не предусмотрено договором между клиентом и агентом"},
        {"question":"18. Какие полномочия у реабилитационного управляющего при реабилитации и банкротстве акционерного общества", "answer":"Все полномочия по управлению обществом (Закон об АО)"},
        {"question":"19. Вправе ли банк в одностороннем порядке изменять ставку по займу", "answer":"Только при улучшении ставки для клиента"},
        {"question":"20. Каким образом доводится решение по итогам рассмотрения заявления на открытие банка", "answer":"Уведомления о выдаче разрешения, направленное заявителю"},
        {"question":"21. Какую стоимость не может превышать страховая сумма по имуществу", "answer":"Действительную стоимость имущества на момент заключения договора"},
        {"question":"22. Что означает двойное страхование", "answer":"Страхование одного и того же товара у разных страховщиков"},
        {"question":"23. Какое имущество может быть в доверительном управлений", "answer":"Деньги, ценные бумаги, имущсетвенные права (ГК 885)"},
        {"question":"24. Мнимая сделка", "answer":"Проведенная для вида без придания правовых последствий, признается недействительной"},
        {"question":"25. Обязан ли кредитор доказывать причинение убытков, при выставлении неустойки", "answer":"Нет"},
        {"question":"26. Чем устанавливается порядок перевода денег без ИИН", "answer":"Актами регулирующими банковскую деятельность."},
        {"question":"27. Договор банковского счета бессрочный?", "answer":"Да или соглашение сторон ГК 747"},
        {"question":"28. При выдаче согласие на приобритение банковского холдинга одновременно выдается", "answer":"Разрешение на значительное участие в капитале банка либо создлание дочернего банка"},
        {"question":"29. Эквайер", "answer":"Банк или организация которая при осуществлении платежа или перевода с ипользованием карточки, принимает деньги в пользу предпринимателя. Услуги держателям карточек не клиентам данного банка по платежам и переводам."},
        {"question":"30. В каком случае перерегистрация юридического лица", "answer":"Уменьшение уставного капитала, смена наименования, изменения участников в хозяйственных товариществах (исключением у кого реестр ценных бумаг)"},
        {"question":"31. Могут ли ценные бумаги быть лизингом", "answer":"Нет ГК 566"},
        {"question":"32. Не является юридическим лицом", "answer":"Простое товарищество"},
        {"question":"33. На каком основании производится изъятие денег в БВУ юридических и физических лиц", "answer":"На основании решения суда, а так же Налогового кодекса, Законами Пенсионного обеспечения, Социальном и медицинском страховании, Платежах и платежных системах."},
        {"question":"34. Значительное участие в капитале", "answer":"Владение 20% и более голосующих акции в уставном капитале"},
        {"question":"35. Эмитент в праве изъят карточку у клиента", "answer":"Окончания срока действия карты, нарушение договора, отказ и расторжение клиента от использования и открытии карты."},
        {"question":"36. Какие лица не являются аффилированными", "answer":"Акционеры некоммерческой организации и кредитного бюро, недееспособные лица"},
        {"question":"37. В какой период отказ эмитента по исполнению платежного документа", "answer":"Не позднее 1 операционного дня со дня получения НБРК 205"},
        {"question":"38. Договор банковского обслуживания", "answer":"Договора банковского счета, перевода денег, вклада, иные ГК 739"},
        {"question":"39. Предельный объем участия БВУ в уставных капиталах юридических лиц", "answer":"Не более 10% собственного капитала банка"},
        {"question":"40. Вправе ли банк нерезидент РК открыть свое представительство", "answer":"Вправе открыть филиал за пределами РК"},
        {"question":"41. При каком условии банк нерезидент РК вправе открыть филиал", "answer":"В случае наличия соглашения об обмене информации между органами регулирования двух стран"},
        {"question":"42. Банк, отнесенный к категории неустойчивым финансовым положением, крупные участники и банковский холдинг принимают меры", "answer":"По улучшению фин состояния, минимизация рисков, в рамках закона и требованию регулятора."},
        {"question":"43. С даты решения регулятором об отнесении банка к неустойчивым финансовым положением, не имеет право", "answer":"Принимать решения: распределение прибыли, дивидендов, финансовых обязательств перед крупными участниками и банковскими холдингами, выплата вознаграждение руководящему составу."},
        {"question":"44. Оплата непокрытых чеков осуществляется", "answer":"В пределах суммы на счете, за счет банковского займа по чековому договору"},
        {"question":"45. В какой срок нужно уплатить размер уставного капитала для нового банка", "answer":"30 календарных дней после регистрации"},
        {"question":"46. Банк с даты регистрации своего филиала должен предоставить регулятору", "answer":"Нотариальное копи положения о филиале, копии доверенности первому руководителю"},
        {"question":"47. Для осуществления переводов по платежам и денег юридическому лицу нужна лицензия", "answer":"На банковские операции прием депозитов, открытие и ведение счетов Юр лиц, корреспондентских счетов банков и организации отдельным видам банковских операции"},
        {"question":"48. В какие сроки доводится решение Регулятора об отнесении банка к категории неустойчивое финансовое состояние", "answer":"5 рабочих дней с даты решения"},
        {"question":"49. На основании каких документов будет произведена конфискация денег и имущества, находящихся в банке", "answer":"На основании судебного решения, вступившего в законную силу"},
        {"question":"50. Каким требованиям должен соответствовать банк при создании дочерней организации", "answer":"2 лет финансовой безубыточности по консолидированной основе\n3 месяца соблюдению прудиков до подачи заявления"},
        {"question":"51. На что нельзя ссылаться при несоблюдение простой письменной сделки в случае спора", "answer":"Свидетельские показания"},
        {"question":"52. Кто признается заинтересованными лицами при совершении сделок", "answer":"Аффилированные лица, которые участвуют в качестве представителя или юридического лица одной из сторон сделки"},
        {"question":"53. Сделка, при которой общество выкупает размещенные или проданные ценные бумаги в количестве 25% от общего количества бумаги", "answer":"Крупная сделка 25% от ценных бумаг или стоимости активов"},
        {"question":"54. Кем может быть наложен арест на деньги в банке", "answer":"Судами на основании судебных актов, судебные исполнители с санкцией прокурора"},
        {"question":"55. Расходы, связанные с ликвидационным производством в том числе по обеспечению банка в какую очередь производятся", "answer":"Вне очереди"},
        {"question":"56. Максимальный размер нестойки (штрафа и пени) за кредит", "answer":"До 90 дней просрочки 0,5% от суммы еж платежа\nПосле 90+ дней просрочки 0,03% от суммы еж платежа\nНе более 10% от суммы займа за каждый год"},
        {"question":"57. На какой срок после передачи активов стабилизационному банку приостанавливается исполнение обязательств перед клиентами", "answer":"12 месяцев"},
        {"question":"58. Под Новацией понимается", "answer":"Прекращение текущих обязательств путем замены на другое обязательство или способ исполнения"},
        {"question":"59. Что относится к личным неимущественным благам", "answer":"Жизнь, здоровье, достоинство личности, честь, имя, репутация, частная жизнь, личная и семейная тайна, авторство"},
        {"question":"60. Цена продажи акции присоединяемого общества определяется", "answer":"Собственного капитала к количеству его размещенных акции"},
        {"question":"61. Может ли быть осуществлена выплата дивидендов по итогам квартала", "answer":"После аудита финансовой отчетности и по решению акционеров"},
        {"question":"62. Объекты гражданских прав", "answer":"Имущественные и личные неимущественные блага и права"},
        {"question":"63. В какую очередь удовлетворяется требования кредиторов с залогом по ликвидируемому банку", "answer":"3 очередь ГК 51"},
        {"question":"64. Требования кредиторов к банку, лишенному лицензии", "answer":"В ликвидационном производстве (исключение требования с текущими расходами банка)"},
        {"question":"65. Является ли заявление (иск) юр и физ лиц основанием для ликвидации банка", "answer":"Да заявления физических, юр. лиц и государственных органов"},
        {"question":"66. До какого момента стабилизационный банк вправе обменять актив неплатежеспособного банка", "answer":"До вступления решения суда о ликвидации"},
        {"question":"67. Какие меры, если лицо приобретает статус крупного участника, без согласования регулятора", "answer":"Меры надзорного реагирования, принудительные меры, реализация акции банка в срок 6 месяцев"},
        {"question":"68. Возвратность кредитов обеспечивается", "answer":"Неустойкой залогом поручительством или договором"},
        {"question":"69. Подлежит ли предоставление информации по гарантиям в кредитное бюро", "answer":"Да передается номер и дата договора, сумму гарантии"},
        {"question":"70. Что нужно для проведения операции с депозитами счетами физ лиц", "answer":"Участником гарантирования вкладов КФГД и иметь лицензию на проведения банковских операции"},
        {"question":"71. К учредительным документам относится", "answer":"Учредительный договор и устав"},
        {"question":"72. Последствия несоблюдения письменный сделки", "answer":"Лишает права использовать свидетельские показания и доказательства, при несоблюдении сделка признается ничтожной"},
        {"question":"73. Отсутствует необходимость получить согласие регулятора для юридических лиц-нерезидентов на приобретение статуса банковского холдинга или крупного участника с минимальным рейтингом", "answer":"Для нерезидента, приобретающего 10% акции у нерезидента являющего крупным участником банка (10%) с минимальным рейтингом."},
        {"question":"74. Течение срока исковой давности не приостанавливается", "answer":"При ЧП, поручение Президента по отсрочке исполнения, военное положение или военнообязанный, нет законного представителя у недееспособного лица, приостановление закона"},
        {"question":"75. Допускается ли уступка требования финансовым агентом", "answer":"Нет или по соглашению договора"},
        {"question":"76. В случае противоречии между учредительным договором и уставом", "answer":"Учредительного договора, если относятся к учредителям\nУстава если отношения с 3-им лицами"},
        {"question":"77. Исковая давность не распространяется", "answer":"Защита нематериальных благ\nВкладчиков к банку\nВозмещение вреда жизни и здоровья не более 3 года до иска\nСобственника (владельца) если были нарушены его права"},
        {"question":"78. Франшиза", "answer":"Освобождение страховщика от ущерба не превышающий определенный объем"},
        {"question":"79. Договор доверительного управления имуществом включает", "answer":"Предмет и срок договора\nСостав имущества\nУказание выгодоприобретателем\nСроки и форму отчетности \nЛицо получающее имущество при прекращении договора"},
        {"question":"80. Фьючерс", "answer":"Финансовый инструмент на организованном рынке где покупатель и продавец берут обязательство в определенный срок купить или продать актив по стандартам организованного рынка"},
        {"question":"81. В каком случае страховщик должен возместить ущерб при условной франшизе", "answer":"При превышении суммы франшизы"},
        {"question":"82. Какие АО не праве выпускать конвертируемые ценные бумаги", "answer":"Если не прописано в уставе \nНекоммерческие организации"},
        {"question":"83. Способом обеспечения исполнения обязательств", "answer":"Неустойка, залог, гарантии и поручительство, задаток, имущество должника, гарантийный взнос, или договором"},
        {"question":"84. Договором доверительного управления имуществом включает", "answer":"Предмет и срок договора\nСостав имущества\nВыгодоприобретатель\nСроки и форму отчетности\nЛицо получающее имущество в случае прекращения договора"},
        {"question":"85. Как рассчитывается доли прямого или косвенного владения акциями", "answer":"От объема размещенных акции (за исключением привелиг и выкупленных) или количеству голосующих акции"},
        {"question":"86. Лицо, к которому применены меры по улучшению фин состояния уведомляет об", "answer":"Исполнении мер по письменному предписанию и в сроки, указанные в документе"},
        {"question":"87. В течение какого срока Банк должен уведомить уполномоченный орган о выборе аудиторской компании", "answer":"10 рабочих дней"},
        {"question":"88. Предметом уступки денежного требования", "answer":"Требования срок платежа, по которому наступили и право получение денег в будущем"},
        {"question":"89. Банк вправе осуществлять сделки по следующим ценным бумагам", "answer":"Облигации международные определяемые списком Нац Банка\nОблигации финансовой компании по проектному финансированию и секьюритизации \nСобственных облигации банка\nДочернего банка \nОблигации взамен ранее приобретенных организации в процессе реструктуризации"},
        {"question":"90. В какой срок временная администрация складывает полномочия", "answer":"Не более 1 месяца с даты назначения ликвидационной комиссии банка"},
        {"question":"91. Безупречная деловая репутация", "answer":"Факты профессионализма, отсутствие судимости,"},
        {"question":"92. Свыше какого период руководящие работники не могут исполнять обязанности без согласования Нац Банка", "answer":"60 календарных дней"},
        {"question":"93. Количество привилегированных акции не должно превышать", "answer":"25% от общего количества"},
        {"question":"94. С какого момента банк становится участником КФГД", "answer":"С момента внесения реестр КФГД"},
        {"question":"95. В лимиты платежей по корреспондентским счетам банков не входят платежи и переводы", "answer":"Межбанковсккую систему или систему межбанковского клиринга\nПо зачету требовании по Лоро и Ностро счетами в одном банке\nПо биржевым сделкам через корреспондентские счета открытые в центральном депозитарии\nПеревод денег между родительским и дочерним банком, по которому проведена реструктуризация"},
        {"question":"96. Кто такой Респондент", "answer":"Банк или не банк или участник МФЦА открывший корреспондентский счет в другом банке (ностро счет(наш))"},
        {"question":"97. В какой срок банк обязан уведомить регулятор об изменении состава акционеров владеющий более 10% голосующих акции", "answer":"15 календарных дней"},
        {"question":"98. Сколько экземпляров актов по передачи имущества между временной администрацией и ликвидационной комиссией", "answer":"4 экземпляра (1 в суд - 1 в АФР)"},
        {"question":"99. На какой срок выдается лицензия для банковской операции", "answer":"Неограниченный срок"},
        {"question":"100. Какие меры применяет уполномоченный орган, если не одобрен план мероприятии по мерах раннего реагирования", "answer":"Меры надзорного реагирования"},
        {"question":"101. Кто может быть заимодателем по государственному займу", "answer":"Гражданин или Юр лицо"},
        {"question":"102. Какие подразделения банка подлежат регистрации в юстиции", "answer":"Филиалы и представительства"},
        {"question":"103. Бизнес план должен содержать", "answer":"Стратегию, бюджет, расчетный баланс, счет прибыли и убытков за 3 года\n план маркетинга, план трудовых ресурсов, управление рисками"},
        {"question":"104. Меры надзорного реагирования", "answer":"Рекомендательная мера\nМера по улучшению финансового состояния и минимизация рисков\nПринудительные меры надзорного реагирования"},
        {"question":"105. Сроки оплаты уставного капитала", "answer":"3 рабочих дня после регистрации"},
        {"question":"106. Полномочия каких органов банка переходят к временной администрации", "answer":"Всех"},
        {"question":"107. Общий срок особого регулирования", "answer":"5 лет"},
        {"question":"108. Кем утверждается Правила об общих условиях банковских операции", "answer":"Советом Директоров"},
        {"question":"109. Какой стаж для главного бухгалтера Банка", "answer":"3 лет в сфере аудита и предоставления финансовых услуг"},
        {"question":"110. Квалифицированное большинство должно быть не менее", "answer":"Трех четвертей"},
        {"question":"111. Не позднее 10 числа ежеквартально Банк должен предоставлять регулятору", "answer":"Список крупных участников с указанием, размещенных акции или долей участия в уставном капитале"},
        {"question":"112. В течение какого срока банк должен предоставить план мероприятии по ранним мерам реагирования", "answer":"5 рабочих дней"},
        {"question":"113. Элементы финансовой отчетности", "answer":"Активы обязательства и капитал"},
        {"question":"114. Вправе ли кредитор не принимать обязательство по частям", "answer":"Вправе или по условиям договора"},
        {"question":"115. Где можно совершать сделки по ценным бумагам банками", "answer":"На организованном рынке ценных бумаг или по нормативам регулятора"},
        {"question":"116. Где назначают на должность Главного бухгалтера – профессиональных бухгалтеров", "answer":"В организациях публичного интереса"},
        {"question":"117. Кто производит восстановление прав на утраченные ценные бумаги", "answer":"суд"},
        {"question":"118. Долевая ценная бумага", "answer":"Ценная бумага дающее право владельцу на определенную долу в имуществе"},
        {"question":"119. Зависимое Акционерное общество", "answer":"Другое юридическое лицо имеет 20% голосующих акции"},
        {"question":"120. Основание для прекращения обязательств", "answer":"Исполнением, Зачетом, Новацией, Прощение долга, Смертью, Ликвидация"},
        {"question":"121. При солидарности требовании может ли любой из солидарных кредиторов предъявить должнику в полном объеме", "answer":"да"},
        {"question":"122. Какие обязательства не включаются в размер коэффициентов валютной ликвидности", "answer":"До востребования, Овернайт полученные от банков"},
        {"question":"123. В случае если залог по ипотеке имеет историческую и художественную культурную ценность", "answer":"Не допускается реализация во внесудебном порядке"},
        {"question":"124. В каком случае банк признается не выполнившим нормативы по ликвидности", "answer":"При наличии у банка в течение отчетного периода просрочек перед вкладчиками и кредиторами"},
        {"question":"125. Бизнес-модель банка", "answer":"Совокупность выбранной стратегии, продуктов, процессов планирования, доходность"},
        {"question":"126. Кто оплачивает акции банка не превышающем стоимости имущества на праве собственности", "answer":"Крупные участники физические лица При этом стоимость имущества должна покрывать совокупную стоимость всех акции"},
        {"question":"127. Финансовая отчетность", "answer":"Бухгалтерский баланс\nОтчёт прибыли и убытков \nДвижение денежных средств \nОтчет об изменении капитала \nПояснительная записка"},
        {"question":"128. Совокупная сумма риска на одного заемщика превышающее 10% от капитала", "answer":"Не должен превышать в 5 раз размер собственного капитала банка"},
        {"question":"129. Ценные бумаги, обладающие однородными признаками и реквизитами, обращающиеся в единых условиях", "answer":"Эмиссионные ценные бумаги"},
        {"question":"130. Банк с 1/3 акциями в собственности или управлении нерезидентов", "answer":"Банк с иностранным участием"},
        {"question":"131. Основание для бухгалтерских записей", "answer":"Первичные документы"},
        {"question":"132. Кто несет риск гибели или повреждения залога", "answer":"Залогодатель или по договору залога"},
        {"question":"133. Основные качественные характеристики финансовой отчетности", "answer":"Понятность уместность надежность и сопоставимость"},
        {"question":"134. Кому предоставляется финансовая отчетность", "answer":"Учредителям\nОргану в области статистики\nРегулятору\nМатеринской компании"},
        {"question":"135. В какой срок необходимо устранить нарушения для участника особого режима", "answer":"60 рабочих дней со дня уведомления"},
        {"question":"136. Принципы введения бухгалтерского учета и финансовой отчетности", "answer":"Начисление и непрерывность"},
        {"question":"137. В какие сроки после лишения лицензии временная администрация должна передать списки депозиторов в КФГД", "answer":"25 рабочих дней с даты лишения лицензии"},
        {"question":"138. Банкам запрещено рекламировать", "answer":"Деятельность, не соответствующая действительности на дату публикации"},
        {"question":"139. Что заключает Регулятор при получении решения от СД банка о реструктуризации", "answer":"Заключает письменное соглашение реструктуризации банка"},
        {"question":"140. Владелец источника повышенной опасности не отвечает за вред", "answer":"Потеря контроль в результате хищения 3 лиц\nВина несут лица, завладевшие источником"},
        {"question":"141. Отлагательная сделка", "answer":"Обстоятельства права не известно наступит оно или нет"},
        {"question":"142. Ценная бумага, по которой передаются права с надписью Индоссамента", "answer":"Ордерная ценная бумага"},
        {"question":"143. Письменное предписание Регулятора", "answer":"Меры по улучшению финансового состояния и минимизации рисков и плана мероприятия по их исполнению"},
        {"question":"144. Что удостоверяет ценная бумага", "answer":"Имущественные права"},
        {"question":"145. В какую очередь погашается задолженность юридического лица по налогам и платежам в бюджет при ликвидации", "answer":"В 4 очередь"},
        {"question":"146. Учредители сохраняют обязательства на имущество", "answer":"Хозяйственного товарищества Акционерного общества Кооперативы"},
        {"question":"147. За счет каких средств принудительная ликвидация банка", "answer":"Средства Банка за исключением денег на оплату труда и СМИ"},
        {"question":"148. Может ли до введения нового акта ГК распространяться на ранние отношения", "answer":"Нет акты не имеют обратной силы"},
        {"question":"149. Разделительный баланс", "answer":"При разделении и выделении"},
        {"question":"150. В каких случаях филиалы или представительства подлежат перерегистрации", "answer":"Изменение наименования"},
        {"question":"151. Годовой валовый продукт Банка", "answer":"Сумма свовокупного дохода корпоративного налога ассигнований оебеспечение"},
        {"question":"152. Кто входит в состав банковского конгломерата", "answer":"Баковский холдинг\nБанк\nДочерние компании холдинга и банка\nОрганизации, в которых имеется значительная доля банка или холдинга"},
        {"question":"153. Передача сомнительных активов дочерней компанией служит", "answer":"Отказ в выдаче разрешения на создание дочерней организации и приобретение сомнительных активов родительского банка"},
        {"question":"154. До какого момента имеет силу разрешение Регулятора на открытие банка", "answer":"До принятия решения по выдаче лицензии"},
        {"question":"155. Отраслевой Банк", "answer":"Банк специализированный регулируется отдельными законами"},
        {"question":"156. Вероятность получения ущерба от отказа информационных систем Банка", "answer":"Риск информационных технологии"},
        {"question":"157. Стресс тестирование", "answer":"Метод оценки потенциального влияния различных событий на финансовое состояние банка"},
        {"question":"158. Комплексная характеристика по финансовым показателям позволяющая оценить платежеспособность в будущем", "answer":"Кредитоспособность"},
        {"question":"159. Каким УО принимается решения по дочерним организациям банка при участии в акциях Правительство РК", "answer":"Совет директоров Банка"},
        {"question":"160. Какие Условия отнесения необеспеченного обязательства к субординированному долгу", "answer":"Срок обязательства не менее 5 лет \nКредиторы не могут требовать досрочного погашения\nБанка может досрочно погасить, если не приведет к снижению прудиков\nПри ликвидации банка обязательство удовлетворяется 10 очереди"},
        {"question":"161. Максимальная сумма рисков на всех заемщиков по ЛСБОО", "answer":"Не более размера собственного капитала"},
        {"question":"162. Как рассчитывается собственный капитал", "answer":"Сумма капитала первого уровня и второго уровня"},
        {"question":"163. Как рассчитывается краткосрочные обязательства нерезидентов коэффициента К7", "answer":"Сумма краткосрочных обязательств нерезидентов к собственному капитала банка"},
        {"question":"164. Какие бумаги не высоколиквидные", "answer":"Бумаги с обратным выкупом\nВ залоге"},
        {"question":"165. Если в залоге по Ипотеке имеется земля сельхоз назначения", "answer":"Отсрочка при реализации земли до 1 года"},
        {"question":"166. В какой срок Банк должен направить Регулятору план рекапитализации при нарушении достаточности капитала", "answer":"1 месяца со дня нарушения коэф достаточности капитала"},
        {"question":"167. К чему соотносится размер риска на одного заемщика", "answer":"К собственному капиталу банка"},
        {"question":"168. В каком размере учитывается в капитале сумма бессрочных фин инструментов", "answer":"В сумме фактических поступивших денег"},
        {"question":"169. При публикации банком ежеквартальных отчетов ОПиУ Баланс нужно ли подтверждение аудиторов", "answer":"Нет банк самостоятельно публикует на основе международных стандартов МСФО"},
        {"question":"170. Источники денег, из которых физическое лицо может стать крупных участником в банке", "answer":"Предпринимательская и трудовая деятельность\nНакопления \nДарения, выигрыш, продажа дарственного имущества, но не более 25% от стоимости акции"},
        {"question":"171. От чего освобождается Залогодержатель в торгах", "answer":"От Гарантийного взноса \nПри выигрыше торга от оплаты покупной цены"},
        {"question":"172. Что должен создавать банк для надлежащего уровня надежности банки", "answer":"Провизии по международным стандартам финансовой отчетности"},
        {"question":"173. Какие лицензии нужны Казпочте для банковских операции", "answer":"Прием депозитов и открытие счетов только на основании лицензии от Нац Банка \nПо всем остальным операциям без лицензии"},
        {"question":"174. Кто обеспечивает контроль процессов в Банке", "answer":"Руководство (должностные лица)"},
        {"question":"175. Кому предоставляют Банки информацию по Гарантиям и поручителям", "answer":"В Кредитное бюро на основании согласия"},
        {"question":"176. Юридическое лицо, занимающее коммерческой деятельностью, не может создана в виде", "answer":"Фонда, Учреждения, Религиозного объедения, Общественного объединения, Потребительского кооператива"},
        {"question":"177. Предпринимательство основное на хозяйственной деятельности государственного предприятия это", "answer":"Является вещным правом получившего имущество от государства на правах использования"},
        {"question":"178. Кто утверждает типовой устав общества", "answer":"Министерство Юстиции"},
        {"question":"179. Возможно ли вернуть по займу в счет долга вещей, определённых родовыми признаками", "answer":"Да согласия заимодателя\nПо договору займа денег - вещей\nПо договору займа вещей - денег"},
        {"question":"180. Право Ипотеки возникает", "answer":"Регистрация ипотечного договора"},
        {"question":"181. Ипотечное свидетельство является ордерной ценной бумагой дает право владельцу на", "answer":"Получение исполнения по основному обязательству \nНа обращение взыскания на заложенное имущество"},
        {"question":"182. Что такое Депозитарий финансовой отчетности", "answer":"Электронная база данных с открытым доступом \nГодовая финансовая отчетность\nАудиторские отчеты\nСписок аффилированных лиц\nКорпоративные события"},
        {"question":"183. В каком размере выплачивается обязательный взнос в КФГД", "answer":"Не более 0,5% от суммы всех депозитов на 1 число отчетного периода 1 раз в квартал\nГодовой размер чрезвычайного взноса не более суммы квартальных платежей\nРазмер определяется КФГД"},
        {"question":"184. Принципы КФГД", "answer":"Обязательность участия\nПрозрачность системы гарантирования\nСнижение рисков системы депозитов\nНакопительный характер (резерв накоплений)"},
        {"question":"185. Может ли Банк резидент открыть филиал зарубежом", "answer":"На основании Соевта директоров Банка\nНаличие соглашения между регуляторами стран\n30 дней после регистрации филиала, необходимо уведомить Нац Банк"},
        {"question":"186. В какой срок нужно уведомить КФГД при изменениях наименования банка", "answer":"10 рабочих дней со дня получения всех документов от банка"},
        {"question":"187. Основания для исключения из КФГД", "answer":"Лишения лицензии или добровольная сдача лицензии\nРеорганизация или ликвидация\nНеисполнения обязанности"},
        {"question":"188. Какая деятельность разрешена Банка на рынке ценных бумаг", "answer":"Брокерская с государственными бумагами\nДилерская с государственными бумагами\nКастодиальная \nТрансфер-вгентская"},
        {"question":"189. Максимальная доля акции Банка в организации со значительной долей в капитале", "answer":"10% размера собственного капитала банка\n10% от размещенных акции в уставном капитале"},
        {"question":"190. Требования к составу комитета по вопросам кадров и вознаграждения", "answer":"Председатель должен быть независимым членом СД\nЧлен комитете с опытом управления персоналом"},
        {"question":"191. Периодичность аудита", "answer":"Не более раз в год общей информации\nПо одному вопросу не более 1 раза в 3 года"},
        {"question":"192. В какие сроки после окончания финансового года нужно провести собрание СД", "answer":"5 месяцев"},
        {"question":"193. В каком случае собственный капитал банка будет отрицательным", "answer":"Сумма обязательств превышает стоимость активов"},
        {"question":"194. Нельзя назначать на должность руководителя Правления", "answer":"Крупный участник"},
        {"question":"195. Каким условиям должны соответствовать однородные финансовые инструменты связанные с изменением ставки", "answer":"Выпущенные одним эмитентом\nРавный размер доходности\nВ одной и той же валюте \nРавный срок погашения"},
        {"question":"196. В какой срок можно повторно избрать ЧП, при его отказе Регулятором", "answer":"90 календарных дней (не более 2 раз за 12 мес)"},
        {"question":"197. При превышении какого срока Банк должен осуществить реструктуризацию банка при неспособности исполнять обязательства перед отдельными кредиторами", "answer":"Срок не определен"},
        {"question":"198. Какой стаж должен быть у кандидата в члены Правления (Банки Аудит АФР)", "answer":"2 года (1 год на руководящей позиции)"},
        {"question":"199. Может ли банк-кастодиан быть аффилированным лицом для своего клиента", "answer":"Нет (кроме Нац Банка)"},
        {"question":"200. Какая информация учитывается при рассмотрении займа", "answer":"Скоринг на основании собственной методики \nСкоринг на основании кредитного бюро"},
        {"question":"201. В течение какого срока банк должен реализовать акции в залоге юридического лица при принятии в собственность", "answer":"12 месяцев"},
        {"question":"202. Какая информация учитывается при рассмотрении займа", "answer":"Скоринг на основании собственной методики \nСкоринг на основании кредитного бюро"},
        {"question":"203. Если Банк не внес изменения в рекламу по требованию регулятора", "answer":"Регулятор самостоятельно публикует опровержение или уточнение за счет средств Банка"},
        {"question":"204. В какой день должны быть зачислены наличные деньги клиенту если были приняты в течение рабочего дня", "answer":"В тот же день при приеме денег"},
        {"question":"205. Какой орган определяет кредитную политику банка", "answer":"Кредитный комитет"},
        {"question":"206. В каком размере от капитала можно передавать в залог без СД", "answer":"10% от капитала"},
        {"question":"207. За счет каких средств выполняется консервация Банка", "answer":"За счет средств Банка"},
        {"question":"208. В течение какого времени Дочка обязана извещать Регулятор обо всех изменениях в учредительный документ", "answer":"30 календарный дней"},
        {"question":"209. Обязательному аудиту предлежат", "answer":"Все банки по итогам года (за исключением лишенного лицензии и в процессе ликвидации)"},
        {"question":"210. Кто обеспечивает политику комплаенс риском", "answer":"Комитет по вопросам правления рисками"},
        {"question":"211. Какие ценные бумаге вправе выпускать Исламский Банк", "answer":"Акции и паи инвестиционных фондов\nАрендные сертификаты\nСертификаты участия \nИные ценные бумаги"},
        {"question":"212. Что не относится к операциям Исламского банка", "answer":"Факторинг\nФорфейтинг"},
        {"question":"213. Какой предельный срок консервации Банка", "answer":"1 год"},
        {"question":"214. Какой срок исковой давности по требованиям к заемщикам", "answer":"5 лет"},
        {"question":"215. Вправе ли банк изъять деньги со счетов клиентов без согласия", "answer":"Да (при ошибке зачисления и подделка документов)"},
        {"question":"216. Что должны предоставить Регулятору Физические и Юридические лица, которые приобретают статус крупного участника более 25% от акции Банка", "answer":"Бизнес план на 5 лет утвержденный Нац Банком"},
        {"question":"217. Что происходит при консервации Банка", "answer":"Регулятор назначает на 1 год временную администрацию или временного управляющего"},
        {"question":"218. На основании какого документы Банк вправе передавать информацию по банковских счетах юридического лица", "answer":"От первого руководителя следствия \nСледователя\nСанкции прокурора"},
        {"question":"219. В какую очередь выплачиваются требования ЛСБОО по депозитам и счетам исламского банка", "answer":"В 4 очередь в том числе по средствам привлеченным по пенсионным и страховым организациям"},
        {"question":"220. Вправе ли Банк приобретать акции и доли в юридических лицах, когда залог переходят в собственность Банка", "answer":"Да при этом участие в доли юридического лица, не более 10% от капитала банка"},
        {"question":"221. Обязательно ли исполнять Решение Банковского омбудсмена", "answer":"Да"},
        {"question":"222. В течение какого время регулятор рассматривает ходатайство о добровольной реорганизации банка", "answer":"2 месяца со дня приема"},
        {"question":"223. Кому выдаются справки о счетах и остатках при смерти владельца", "answer":"Указанные в завещании \nСудам и нотариусам\nКонсульским учреждениям при наследственных делах\nНаследникам"},
        {"question":"224. Что не относится к банковской тайне по кредитам", "answer":"Сведения по кредитам, находящимся в ликвидации"},
        {"question":"225. Банковский депозитный сертификат это", "answer":"Именная неэмиссионная бумага дающая права получить номинальную стоимость и вознаграждение в указанные сроки и условия"},
        {"question":"226. Каким документом определяются СМИ при дополнительных публикациях (помимо портала центрального депозитария)", "answer":"Устав общества"},
        {"question":"227. Какие действия не вправе совершать временная администрация", "answer":"Проведения взаимозачетов с кредиторами договорам уступки прав"},
        {"question":"228. Какой банковский холдинг-нерезидент с долей 25% может владеть банком в РК", "answer":"Финансовая организация-нерезидент РК, подлежащая консолидированному надзору в стране нахождения"},
        {"question":"229. Кто принимает решение о консервации исламского банка", "answer":"Собрание акционеров с согласованием Регулятора"},
        {"question":"230. Когда прекращаются действие ипотечного свидетельства", "answer":"При исполнении прав\nПри добровольной передаче залогодателю \nПри отсутствии требовании должнику в течение 30 дней после наступления срока исполнения \nПри утрате предмета ипотеки"},
        {"question":"231. Что не вправе делать банк-кастодиан", "answer":"Не использовать вверенные активы в своих интересах\nНе закладывать активы"},
        {"question":"232. Кто такой стабилизационный банк", "answer":"Банк, созданный Регулятором в целях передачи активов и обязательств Банка находящейся в режиме консервации"},
        {"question":"233. Какая максимальная стоимость доли участия Банка в уставном капитале или акциях Юридического лица", "answer":"Не более 50% от собственного капитала Банка"},
        {"question":"234. Какие основания для проведения реструктуризации Банка", "answer":"Неспособность выполнять требования отдельных кредиторов в связи с отсутствием денег"},
        {"question":"235. Кому возможно предоставить Бланковый беззалоговый займа?", "answer":"Клиенту с высокой кредитоспособностью и надежностью"},
        {"question":"236. Предоставляется ли акционерам право на преимущественную покупку акции в случае конвертирования ценных бумаг в акции банка при реструктуризации", "answer":"Нет"},
        {"question":"237. Какие виды операции осуществляет Ипотечная организация", "answer":"Инвестиционная\nИздания литературы по вопросам Ипотеки\nРеализация собственного имущества"},
        {"question":"238. Какие признаки неустойчивого финансового положения у юридического лица который получает статус крупного участника", "answer":"Юр лицо действует менее 2 лет \nОбязательства превышают активы (за исключением доли в других компаниях)\nУбытки за последние 2 года\nПросрочка перед Банком\nУхудшение финансовых последствии для заявителя \nСтоимость имущества недостаточна для приобретение акции"},
        {"question":"239. На какой срок избирается Омбудсмен", "answer":"2 года"},
        {"question":"240. За счет каких средств КФГД будет производить выплату сверх специального резерва", "answer":"Нац Банк"},
        {"question":"241. Какие меры применяются Банку при несоблюдении по раскрытию общих условий", "answer":"Приостановление или лишение лицензии на все или отдельные операции"},
        {"question":"242. С кем запрещено проводить или рассматривать сделки Главному бухгалтеру", "answer":"По себе и близким родственникам \nЮридическим лицом, где родственник должностное лицо\nСупругами и их близкими родственниками"},
        {"question":"243. В каких организации запрещается участвовать Ипотечной организации", "answer":"В Финансовых организациях \nВ Юр лицах с акциями размещенными на фондовом рынке с наивысшей категорией\nВ компаниях по автоматизации процессов связанных с Ипотекой\nЗапрещается в компаниях (ТОО АО)"},
        {"question":"244. При наступлении признака Банковского холдинга (крупного участника) в какой срок должен уведомить Регулятора", "answer":"10 календарных дней"},
        {"question":"245. С какой целью проводится аудит", "answer":"Точность и полнота отчетности по банковским операциям\nСоответствия требованиям Законов РК\nСоответствия ВНД Банка"},
        {"question":"246. В течение какого времени крупный участник должен уведомить Регулятор об изменении кол-ва акции", "answer":"30 календарных дней"},
        {"question":"247. Кто финансирует банковского Омбудсмена", "answer":"Взносы от Банков"},
        {"question":"248. На каких условиях Финансовая организация должна разместить акции к продаже для акционеров по требованию Регулятора", "answer":"5 рабочих дней после принятия решения \nПубликация на 2 языках в интернет-ресурсе депозитария фин отчетности \nНа равных условия пропорционально кол-ву акции \nЦена размещения установленной Правлением банка"},
        {"question":"249. При каком количестве акционеров создается Счетная комиссия", "answer":"100 и более"},
        {"question":"250. Какие акции не входят в число голосующих", "answer":"Выкупленные обществом \nВ номинальном держании собственника (без учета в центр депозитарии)"},
        {"question":"251. Имеет ли право Председатель АО занимать должность Председателя в другом АО", "answer":"Не вправе"},
        {"question":"252. В какие сроки АО может предложить своим Акционерам выкупить акции по преимущественной покупки", "answer":"10 календарных дней с письменным уведомлением на 2 языках в центральном депозитарии"},
        {"question":"253. Что такое Кодекс корпоративного управления АО", "answer":"Документ, регулирующий отношения между \nАкционерами и обществом\nОрганами общества \nОбществом и заинтересованными лицами"},
        {"question":"254. В течение какого срока нужно предоставить сведения об аффилированных лицах в Банк", "answer":"7 календарных дней со дня аффилированности"},
        {"question":"255. Каким документом устанавливается порядок предоставления об их аффилированных лицах", "answer":"Устав"},
        {"question":"256. На основании каких сведении Банк ведет учет аффилированных лицах", "answer":"По информации от лиц Банка\nЦентральным депозитарием по крупным акционерам"},
        {"question":"257. Кумулятивное голосование это", "answer":"Способ голосования на СД одна Акция один Голос"},
        {"question":"258. Кто выбирает Банк агент при выплате Гарантированных депозитов", "answer":"на основании Правления КФГД"},
        {"question":"259. По каким причинам собираются члены СД", "answer":"Созыв 1 из членов СД\nВнутренний Аудит\nВнешний Аудит\nКрупный акционер"},
        {"question":"260. По какому документу определяется требования к Банку Агенту от КФГД при выплате гарантированных депозитов", "answer":"Определяется Соглашением с условиями и порядком перечисления сумм с КФГД"},
        {"question":"261. Какие меры применяет Регулятор, если Банк показывает ухудшение финансового положения", "answer":"Принудительные меры надзорного реагирования"},
        {"question":"262. Кто из физ лиц может стать членами СД", "answer":"Акционеры \nЛица представители акционера \nЛица не акционеры и не представители"},
        {"question":"263. На сколько дней действителен ЧЕК", "answer":"10 календарных дней со дня следующего после выписки чека"},
        {"question":"264. Допускается ли отказ Чекодержателя в принятии частичного платежа", "answer":"Да"},
        {"question":"265. Кто может быть участниками особого режима", "answer":"Финансовые организации или юридические лица \nФинансовой сфере\nКонцентрацией финансовых ресурсов\nПлатежными услугами"},
        {"question":"266. А какую очередь удовлетворяется необеспеченное обязательство при ликвидации", "answer":"10 очередь"},
        {"question":"267. Какую санкцию получит Банк при нарушении 3 и более раза в течение 12 месяцев", "answer":"Приостановление лицензии \nЛишение лицензии\nПостановление отдельных операции"},
        {"question":"268. В какой срок АО направляет кредиторам уведомления о разделении", "answer":"2 месяца после решения АО"},
        {"question":"269. В какие сроки Регулятор принимает решение по получению статуса Крупного участника или банковского Холдинга", "answer":"50 рабочих дней"},
        {"question":"270. Могут ли Дочки Банковского холдинга создавать свои дочерние компании", "answer":"Нет"},
        {"question":"271. Через сколько времени можно повысить ставку по кредиту", "answer":"Не ранее 3 лет по соглашению сторон"},
        {"question":"272. Ассоциация Финансистов Казахстана", "answer":"Некоммерческая организация"},
        {"question":"273. В какие сроки нужно предоставлять Финансовую отчетность", "answer":"Не позднее 31 мая за предыдущий год"},
        {"question":"274. Имеет ли право Дочка Банка создать свою дочернюю компанию", "answer":"Нет (за исключением Нерезидентов по техническому сопровождению)"},
        {"question":"275. Относятся ли финансовые организации к публичному интересу", "answer":"Да (исключение обменные пункты с лицензией от НБРК)"},
        {"question":"276. Кто может не вести бухгалтерский учет", "answer":"Одновременно по 3 категориям:\nСпециальный налоговый учет (Патент упрощенная декларация)\nНе состоящие на регистрации по НДС\nНе субъекты естественных монополии"},
        {"question":"277. Что такое письменное соглашение с Регулятором", "answer":"Меры по улучшению фин состояния \nУстранение недостатков (рисков нарушений)\nПеречня ограничений"},
        {"question":"278. Вправе ли Банк выдавать кредиты лицам участниками банковского конгломерата", "answer":"Вправе"},
        {"question":"279. Какой документ должен приложить учредитель банка Нерезидент ЮЛ для разрешения на открытие банка", "answer":"Уведомление Регулятора страны резидентства ЮЛ о разрешении или уведомление что согласие не требуется"},
        {"question":"280. Кто может быть заимодателем", "answer":"Банк или ЮЛ имеющий лицензию на кредиты"},
        {"question":"281. Что должно содержать заявление на открытие банка", "answer":"2 языка и адрес заявителя"},
        {"question":"282. В каком случае родительский банк нерезидент может открыть дочерний банк", "answer":"Имеющий определённый рейтинг, устанавливаемый Регулятором"},
        {"question":"283. Что должен предоставить Регулятору Крупный участник или банковский холдинг, в случае увеличения своих акции", "answer":"Источники средств для приобретения акции с копиями подтверждающих документов"},
        {"question":"284. В случае 2 отказов Регулятора на согласие согласовать на Руководящего работника, когда кандидат может подать повторно", "answer":"Через 12 мес"},
        {"question":"285. Что обязан сделать банк в случае выкупа акции Правительством РК", "answer":"5 календарных дней с даты регистрации акции возвратить предыдущее свидетельство о регистрации акции"},
        {"question":"286. Какое количество членов Правления Банка должно быть от Правительства РК при покупки акции Государством", "answer":"Не более 30% от состава"},
        {"question":"287. Вправе ли член Правления работать в других организациях", "answer":"Да с согласия СД (исключения быть Председателем в других банках)"},
        {"question":"288. При каких условиях отчет аудиторской компании признается действительным", "answer":"Независима от акционеров \nЛицензия на проведение аудита"},
        {"question":"289. В случае смерти человека кто имеет право на возмещение вреда", "answer":"Нетрудоспособные на иждевении \nРебенок \nСупруг, родители которые ухаживают за ребенком\nВнуки братья сестры не достигшие 14 лет"},
        {"question":"290. Могут ли работники Аудита стать членами Правления и СД", "answer":"Нет"},
        {"question":"291. Сервитут", "answer":"Право ограниченного пользования чужой недвижимости"},
        {"question":"292. В какой срок Банк обязан предоставить копиы аудиторского отчёта Юридическим лицам банковского конгломерата", "answer":"10 календарных дней"},
        {"question":"293. На основании каких документов осуществляется конвертирование ценных бумаг в простые акции", "answer":"Проспект выпуска ценных бумаг\nПлан реструктуризации\nРешения Регулятора о мерах урегулирования платежеспособности \nПлана реабилитации"},
        {"question":"294. Когда выданное разрешение на открытие банка считается отмененным", "answer":"Добровольное прекращение деятельности \nРешения суда о прекращении\nОтсутствие регистрации в НАО в течение 2 месяцев \nОтсутствие лицензии на банковские операции в течение 1 года"},
        {"question":"295. Когда СД Банка принимает решение по вопросам дочернего банка", "answer":"При наличии акции Банка у Правительства РК"},
        {"question":"296. В какие сроки уведомляют Акционеров об увеличении акции в целях исполнения пруденциальных нормативов", "answer":"10 рабочих дней очно\n15 рабочих дней заочно"},
        {"question":"297. Каким документом устанавливается периодичность выплаты дивидендов и размера на одну акцию", "answer":"Уставом"},
        {"question":"298. Какой размер риска на 1 заемщика к капиталу банка", "answer":"ЛСБОО – 0,10\nБеззалоговые – 0,10\nЗалоговые – 0,25 \n+2 мес платежей к ним"},
        {"question":"299. Что включает процесс стресс тестирования", "answer":"Достаточном капитала\nУровень риска внутренней и внешней среды\nБизнес-модели и операции и рыночные условия\nСД участвует и оценивает результаты принимает решения по минимизации риска"},
        {"question":"300. Что не включается в расчет риска на 1 заемщика", "answer":"Государственные и квазигосударственные и Фонды по ценным бумагам\nТребования банка к Дочерней организации"},
        {"question":"301. Какие компоненты капитала 2 уровня", "answer":"Сумма субординированного долга за минусом субор долга банка"},
        {"question":"302. В каких случаях общество не вправе выкупать свои размещенные акции", "answer":"До общего собрания акционеров\nДо утверждения отчета размещения среди учредителей\nПри выкупе нарушится минимальный размер уставного капитала \nПри неплатежеспособности и несостоятельности реабилитации банкротстве \nПринято решение о ликвидации"},
        {"question":"303. Несет ли ответственность клиент перед финансовым агентом", "answer":"Да, за недействительность денежного требования"},
        {"question":"304. Какие процедуры до проведения торгов с залоговым имуществом", "answer":"Уведомление залогодателю о начале проведения торгов\nПубликует объявление о торгах не ранее 30 дней после вручения залогодержателю\nЗапрещается проводить сделки с момента публикации на торгах\nС момента публикации и проведения торгов должно быть не менее 10 дней"},
        {"question":"305. Что нельзя акционерам банка являющиеся юридическим лицом нерезидентом", "answer":"Запрещено владеть голосующими акциями резидентов РК (при рег в офшорах)\n Иметь письмо от Регулятора страны регистрации"},
        {"question":"306. В каких случаях не допускается выплата дивидендов", "answer":"Отрицательном капитале \nБанкротство реабилитация неплатежеспособность\nСнижение пруденциальных нормативов\nНедостатки риски нарушения"},
        {"question":"307. Какие документы относятся к бухгалтерской деятельности", "answer":"Первичные документы, регистры бух учета, финансовая отчетность, учетная политика"},
        {"question":"308. Какие организации обязаны вести учет по МСФО", "answer":"Субъекты крупного предпринимательства\nОрганизации публичного интереса"}
    ];

// ==========================
// НОВЫЕ ВОПРОСЫ (добавьте свои вопросы здесь)
// ==========================
const newQuestionsAndAnswers = [
  {"question":"1. Активы клиентов у кастодиана?","answer":"На балансовых и внебалансовых счетах (НБ РК №184 п.32 №4)"},
  {"question":"2. Физлицо — банковский холдинг БВУ РК?","answer":"Нет, только юрлицо"},
  {"question":"3. Форма соглашения о неустойке?","answer":"Письменная (ГК РК ст. 294)"},
  {"question":"4. Корреспондент?","answer":"Банк/небанковская организация с лоро-счётом другого банка (НБРК №210)"},
  {"question":"5. Порядок инвестирования КФГД определяет?","answer":"Национальный Банк (Закон о гарантировании вкладов)"},
  {"question":"6. Риск ликвидности?","answer":"Потери от неспособности выполнить обязательства в срок (НБ РК №188)"},
  {"question":"7. Выписка корреспондента?","answer":"После завершения операционного дня (НБРК №210)"},
  {"question":"8. Кто не может быть акционером банка?","answer":"Госкомпании, офшорные юрлица (искл.: нацхолдинги, проблемные фонды, филиалы нерезидентов)"},
  {"question":"9. Допвзнос в КФГД?","answer":"Не более 2× календарного взноса за предыдущий квартал"},
  {"question":"10. Информирование КФГД о ликвидации и выплатах?","answer":"30 рабочих дней с даты лишения лицензии на все операции"},
  {"question":"11. Гарантирование КФГД?","answer":"При лишении лицензии на все операции"},
  {"question":"12. Срок обращения в банк-агент за возмещением?","answer":"1 год с начала выплат КФГД"},
  {"question":"13. Курс для валютных вкладов при возмещении?","answer":"Рыночный на дату лишения лицензии"},
  {"question":"14. Специальный резерв КФГД?","answer":"Не менее 5% от всех депозитов БВУ"},
  {"question":"15. Пруденциальные нормативы?","answer":"Мин. УК, мин. СК, достаточность СК, риск на 1 заёмщика, ликвидность, валютная позиция"},
  {"question":"16. Уведомление КФГД о перерегистрации наименования?","answer":"5 рабочих дней после получения справки"},
  {"question":"17. Клиент отвечает за неисполнение должником требования фин.агента?","answer":"Нет (если иное не предусмотрено договором)"},
  {"question":"18. Полномочия реабилитационного управляющего в АО?","answer":"Все полномочия по управлению (Закон об АО)"},
  {"question":"19. Одностороннее изменение ставки по займу банком?","answer":"Только в сторону улучшения для клиента"},
  {"question":"20. Доведение решения о разрешении на открытие банка?","answer":"Уведомлением заявителю"},
  {"question":"21. Страховая сумма по имуществу не должна превышать?","answer":"Действительную стоимость имущества на момент договора"},
  {"question":"22. Двойное страхование?","answer":"Один товар у разных страховщиков"},
  {"question":"23. Имущество в доверительном управлении?","answer":"Деньги, ценные бумаги, имущественные права (ГК 885)"},
  {"question":"24. Мнимая сделка?","answer":"Для вида, без правовых последствий — недействительная"},
  {"question":"25. Кредитор доказывает убытки при неустойке?","answer":"Нет"},
  {"question":"26. Порядок перевода денег без ИИН?","answer":"Актами, регулирующими банковскую деятельность"},
  {"question":"27. Договор банковского счёта бессрочный?","answer":"Да (или по соглашению, ГК 747)"},
  {"question":"28. При согласии на банковский холдинг одновременно выдаётся?","answer":"Разрешение на значительное участие или создание дочернего банка"},
  {"question":"29. Эквайер?","answer":"Банк/организация, принимающая деньги в пользу предпринимателя по картам"},
  {"question":"30. Перерегистрация юрлица?","answer":"Уменьшение УК, смена наименования, изменения участников в ХТ (искл. с реестром ЦБ)"},
  {"question":"31. ЦБ могут быть предметом лизинга?","answer":"Нет (ГК 566)"},
  {"question":"32. Не является юрлицом?","answer":"Простое товарищество"},
  {"question":"33. Изъятие денег в БВУ у физ/юрлиц?","answer":"Решение суда + НК и законы о пенсионном, соц/мед страховании, платежах"},
  {"question":"34. Значительное участие в капитале?","answer":"Не менее 20% голосующих акций"},
  {"question":"35. Эмитент может изъять карту у клиента?","answer":"Окончание срока, нарушение договора, отказ клиента"},
  {"question":"36. Не аффилированные лица?","answer":"Акционеры НКО и кредитного бюро, недееспособные"},
  {"question":"37. Срок отказа эмитента по платёжному документу?","answer":"Не позднее 1 операционного дня (НБРК №205)"},
  {"question":"38. Договор банковского обслуживания?","answer":"Счёт, перевод, вклад и иные (ГК 739)"},
  {"question":"39. Предельное участие БВУ в УК других юрлиц?","answer":"Не более 10% собственного капитала банка"},
  {"question":"40. Банк-нерезидент РК открывает представительство?","answer":"Вправе открыть филиал за пределами РК"},
  {"question":"41. Условие открытия филиала банком-нерезидентом РК?","answer":"Соглашение об обмене информацией между регуляторами"},
  {"question":"42. Меры банка с неустойчивым финположением, крупными участниками и холдингом?","answer":"Улучшение финсостояния, минимизация рисков по закону и требованиям регулятора"},
  {"question":"43. С даты отнесения к неустойчивым банк не вправе?","answer":"Распределять прибыль/дивиденды, обязательства перед крупными участниками/холдингом, вознаграждение руководству"},
  {"question":"44. Оплата непокрытых чеков?","answer":"В пределах суммы на счёте + банковский заём по чековому договору"},
  {"question":"45. Срок оплаты УК нового банка?","answer":"30 календарных дней после регистрации"},
  {"question":"46. После регистрации филиала банк предоставляет регулятору?","answer":"Нотариальную копию положения о филиале + копию доверенности руководителю"},
  {"question":"47. Лицензия для переводов юрлицам?","answer":"Приём депозитов, открытие/ведение счетов юрлиц, корр.счетов банков + отдельные виды операций"},
  {"question":"48. Срок доведения решения об отнесении к неустойчивому финсостоянию?","answer":"5 рабочих дней с даты решения"},
  {"question":"49. Конфискация денег/имущества в банке?","answer":"Судебное решение, вступившее в силу"},
  {"question":"50. Требования к банку при создании дочерней организации?","answer":"2 года безубыточности (консолид.) + 3 месяца соблюдения пруденциалов до подачи"},
  {"question":"51. При несоблюдении простой письменной формы нельзя ссылаться на?","answer":"Свидетельские показания"},
  {"question":"52. Заинтересованные лица при сделках?","answer":"Аффилированные, участвующие как представители одной стороны"},
  {"question":"53. Крупная сделка АО?","answer":"Выкуп ≥25% ЦБ или активов"},
  {"question":"54. Арест на деньги в банке?","answer":"Суды по актам + судебные исполнители с санкцией прокурора"},
  {"question":"55. Расходы по ликвидационному производству?","answer":"Вне очереди"},
  {"question":"56. Макс. неустойка по кредиту?","answer":"До 90 дней — 0,5%/день; после — 0,03%/день; ≤10% от суммы займа в год"},
  {"question":"57. Приостановка обязательств после передачи активов стаббанку?","answer":"12 месяцев"},
  {"question":"58. Новация?","answer":"Замена одного обязательства другим"},
  {"question":"59. Личные неимущественные блага?","answer":"Жизнь, здоровье, достоинство, честь, имя, репутация, частная жизнь, тайна, авторство"},
  {"question":"60. Цена продажи акций присоединяемого общества?","answer":"Собственный капитал / количество размещённых акций"},
  {"question":"61. Выплата дивидендов по кварталу?","answer":"После аудита ФО и решения акционеров"},
  {"question":"62. Объекты гражданских прав?","answer":"Имущественные + личные неимущественные блага и права"},
  {"question":"63. Очередь удовлетворения залоговых кредиторов при ликвидации банка?","answer":"3-я (ГК 51)"},
  {"question":"64. Требования кредиторов к лишённому лицензии банку?","answer":"В ликвидационном производстве (искл. текущие расходы)"},
  {"question":"65. Заявление юр/физлиц — основание для ликвидации банка?","answer":"Да (физлиц, юрлиц, госорганов)"},
  {"question":"66. Срок обмена активов стаббанком?","answer":"До вступления решения суда о ликвидации"},
  {"question":"67. Меры при приобретении статуса крупного участника без согласования?","answer":"Надзорное реагирование, принудительные меры, реализация акций в 6 месяцев"},
  {"question":"68. Обеспечение возвратности кредитов?","answer":"Неустойка, залог, поручительство или договор"},
  {"question":"69. Информация по гарантиям в кредитное бюро?","answer":"Да (номер, дата договора, сумма)"},
  {"question":"70. Операции с депозитами/счетами физлиц?","answer":"Участие в КФГД + лицензия на банковские операции"},
  {"question":"71. К учредительным документам относится?","answer":"Учредительный договор + устав"},
  {"question":"72. Последствия несоблюдения письменной формы сделки?","answer":"Лишает права на свидетельские показания; сделка ничтожна"},
  {"question":"73. Согласие регулятора не нужно нерезиденту на статус холдинга/крупного участника?","answer":"При покупке 10% у другого нерезидента-крупного участника с мин. рейтингом"},
  {"question":"74. Течение исковой давности не приостанавливается?","answer":"При ЧП, отсрочке по указу Президента, военном положении, отсутствии представителя у недееспособного и т.д."},
  {"question":"75. Уступка требования финансовым агентом?","answer":"Нет (или по договору)"},
  {"question":"76. Противоречие между учредительным договором и уставом?","answer":"Договор — для учредителей; устав — для третьих лиц"},
  {"question":"77. Исковая давность не распространяется на?","answer":"Нематериальные блага, вкладчиков к банку, вред жизни/здоровью (≤3 года до иска), права собственника"},
  {"question":"78. Франшиза?","answer":"Освобождение страховщика от ущерба ≤ определённого объёма"},
  {"question":"79. Договор доверительного управления включает?","answer":"Предмет, срок, состав имущества, выгодоприобретатель, отчётность, получатель при прекращении"},
  {"question":"80. Фьючерс?","answer":"Обязательство купить/продать актив в срок по стандартам организованного рынка"},
  {"question":"81. Возмещение ущерба при условной франшизе?","answer":"При превышении суммы франшизы"},
  {"question":"82. Какие АО не вправе выпускать конвертируемые ЦБ?","answer":"Если не предусмотрено уставом + некоммерческие"},
  {"question":"83. Способы обеспечения обязательств?","answer":"Неустойка, залог, гарантия/поручительство, задаток, имущество должника, гарантийный взнос, договор"},
  {"question":"84. Договор доверительного управления включает (повтор)?","answer":"Предмет, срок, состав имущества, выгодоприобретатель, отчётность, получатель при прекращении"},
  {"question":"85. Доля прямого/косвенного владения акциями?","answer":"От размещённых (без привилегированных и выкупленных) или голосующих"},
  {"question":"86. Уведомление о мерах по улучшению финсостояния?","answer":"Исполнение по предписанию в указанные сроки"},
  {"question":"87. Срок уведомления регулятора о выборе аудитора?","answer":"10 рабочих дней"},
  {"question":"88. Предмет уступки денежного требования?","answer":"Требования с наступившим сроком + будущие деньги"},
  {"question":"89. Сделки банка по ЦБ?","answer":"Международные облигации (список НБ), облигации по проектному финансированию/секьюритизации, свои/дочки, в обмен на реструктурированные"},
  {"question":"90. Срок сложения полномочий временной администрацией?","answer":"Не более 1 месяца с назначения ликвидационной комиссии"},
  {"question":"91. Безупречная деловая репутация?","answer":"Профессионализм + отсутствие судимости"},
  {"question":"92. Срок исполнения обязанностей руководителем без согласования НБ?","answer":"Не более 60 календарных дней"},
  {"question":"93. Макс. доля привилегированных акций?","answer":"25% от общего количества"},
  {"question":"94. Момент вступления банка в КФГД?","answer":"С внесения в реестр КФГД"},
  {"question":"95. Не входят в лимиты по корр.счетам?","answer":"Межбанковский клиринг, зачёт лоро/ностро в одном банке, биржевые сделки через ЦД, переводы родительский-дочерний после реструктуризации"},
  {"question":"96. Респондент?","answer":"Банк/небанк/участник МФЦА с ностро-счётом в другом банке"},
  {"question":"97. Уведомление регулятора об изменении акционеров >10%?","answer":"15 календарных дней"},
  {"question":"98. Экземпляры акта передачи имущества временная админ → ликв.комиссия?","answer":"4 (1 в суд, 1 в АФР, 2 по сторонам)"},
  {"question":"99. Срок действия лицензии на банковские операции?","answer":"Неограниченный"},
  {"question":"100. Меры регулятора при неодобрении плана раннего реагирования?","answer":"Меры надзорного реагирования"},
  {"question":"101. Заимодатель по госзайму?","answer":"Гражданин или юрлицо"},
  {"question":"102. Подразделения банка для регистрации в юстиции?","answer":"Филиалы + представительства"},
  {"question":"103. Содержание бизнес-плана для открытия банка?","answer":"Стратегия, бюджет, баланс/ПиУ за 3 года, маркетинг, трудовые ресурсы, управление рисками"},
  {"question":"104. Меры надзорного реагирования?","answer":"Рекомендательная, улучшение финсостояния/минимизация рисков, принудительные"},
  {"question":"105. Срок оплаты УК после регистрации?","answer":"3 рабочих дня"},
  {"question":"106. Полномочия органов банка при временной администрации?","answer":"Переходят все к временной администрации"},
  {"question":"107. Общий срок особого режима регулирования?","answer":"5 лет"},
  {"question":"108. Кто утверждает правила общих условий банковских операций?","answer":"Совет директоров"},
  {"question":"109. Стаж главного бухгалтера банка?","answer":"Не менее 3 лет в аудите/финансовых услугах"},
  {"question":"110. Квалифицированное большинство?","answer":"Не менее 3/4"},
  {"question":"111. Ежеквартально не позднее 10 числа — регулятору?","answer":"Список крупных участников с акциями/долями"},
  {"question":"112. Срок предоставления плана раннего реагирования?","answer":"5 рабочих дней"},
  {"question":"113. Элементы финансовой отчётности?","answer":"Активы, обязательства, капитал"},
  {"question":"114. Кредитор вправе не принимать исполнение по частям?","answer":"Да (если не предусмотрено договором)"},
  {"question":"115. Сделки банка по ЦБ?","answer":"На организованном рынке или по нормативам регулятора"},
  {"question":"116. Назначение главного бухгалтера-профессионала?","answer":"В организациях публичного интереса"},
  {"question":"117. Восстановление прав на утраченные ЦБ?","answer":"Суд"},
  {"question":"118. Долевая ЦБ?","answer":"Право на долю в имуществе"},
  {"question":"119. Зависимое АО?","answer":"Другое юрлицо владеет ≥20% голосующих акций"},
  {"question":"120. Основания прекращения обязательств?","answer":"Исполнение, зачёт, новация, прощение долга, смерть, ликвидация"},
  {"question":"121. Солидарные кредиторы — предъявление требования?","answer":"Да, любой в полном объёме"},
  {"question":"122. Не включаются в коэффициенты валютной ликвидности?","answer":"До востребования + овернайт от банков"},
  {"question":"123. Залог по ипотеке с исторической/культурной ценностью?","answer":"Запрет внесудебной реализации"},
  {"question":"124. Невыполнение нормативов ликвидности?","answer":"Просрочки перед вкладчиками/кредиторами в отчётном периоде"},
  {"question":"125. Бизнес-модель банка?","answer":"Стратегия + продукты + процессы + планирование + доходность"},
  {"question":"126. Оплата акций банка имуществом?","answer":"Крупные участники-физлица; стоимость имущества ≥ совокупной стоимости акций"},
  {"question":"127. Состав финансовой отчётности?","answer":"Баланс, ОПиУ, движение ДС, изменение капитала, пояснительная записка"},
  {"question":"128. Совокупный риск на заёмщика >10% капитала?","answer":"Не более 5× собственного капитала банка"},
  {"question":"129. Эмиссионные ЦБ?","answer":"Однородные признаки/реквизиты, единые условия обращения"},
  {"question":"130. Банк с ≥1/3 акций нерезидентов?","answer":"Банк с иностранным участием"},
  {"question":"131. Основание бухгалтерских записей?","answer":"Первичные документы"},
  {"question":"132. Риск гибели/повреждения залога?","answer":"Залогодатель (если иное не предусмотрено договором)"},
  {"question":"133. Качественные характеристики ФО?","answer":"Понятность, уместность, надёжность, сопоставимость"},
  {"question":"134. Кому предоставляется ФО?","answer":"Учредителям, статистика, регулятор, материнская компания"},
  {"question":"135. Срок устранения нарушений в особом режиме?","answer":"60 рабочих дней с уведомления"},
  {"question":"136. Принципы бухучёта и ФО?","answer":"Начисление + непрерывность"},
  {"question":"137. Передача списков депозиторов в КФГД после лишения лицензии?","answer":"25 рабочих дней"},
  {"question":"138. Запрещённая реклама банка?","answer":"Не соответствующая действительности на дату публикации"},
  {"question":"139. При решении СД о реструктуризации регулятор?","answer":"Заключает письменное соглашение"},
  {"question":"140. Владелец источника повышенной опасности не отвечает за вред?","answer":"При хищении третьими лицами (вина на завладевших)"},
  {"question":"141. Отлагательная сделка?","answer":"Права зависят от неизвестного обстоятельства"},
  {"question":"142. ЦБ с правами по индоссаменту?","answer":"Ордерная"},
  {"question":"143. Письменное предписание регулятора?","answer":"Меры улучшения финсостояния + минимизация рисков + план"},
  {"question":"144. Что удостоверяет ЦБ?","answer":"Имущественные права"},
  {"question":"145. Очередь погашения налогов/платежей юрлица при ликвидации?","answer":"4-я"},
  {"question":"146. Учредители сохраняют обязательства по имуществу?","answer":"ХТ, АО, кооперативы"},
  {"question":"147. Источник принудительной ликвидации банка?","answer":"Средства банка (искл. оплата труда + СМИ)"},
  {"question":"148. Обратная сила новых актов ГК?","answer":"Нет"},
  {"question":"149. Разделительный баланс?","answer":"При разделении/выделении"},
  {"question":"150. Перерегистрация филиалов/представительств?","answer":"При изменении наименования"},
  {"question":"151. Годовой валовый доход банка?","answer":"Совокупный доход − корпоративный налог − ассигнования на обеспечение"},
  {"question":"152. Состав банковского конгломерата?","answer":"Холдинг + банк + дочерние компании холдинга/банка + организации со значительной долей"},
  {"question":"153. Передача сомнительных активов дочке?","answer":"Отказ в разрешении на создание дочки/приобретение активов"},
  {"question":"154. Срок действия разрешения на открытие банка?","answer":"До решения по лицензии"},
  {"question":"155. Отраслевой банк?","answer":"Специализированный, отдельные законы"},
  {"question":"156. Риск от отказа ИС банка?","answer":"Риск информационных технологий"},
  {"question":"157. Стресс-тестирование?","answer":"Оценка влияния событий на финсостояние"},
  {"question":"158. Кредитоспособность?","answer":"Комплексная характеристика для оценки платежеспособности в будущем"},
  {"question":"159. Решения по дочкам банка при госучастии в акциях?","answer":"Совет директоров банка"},
  {"question":"160. Условия субординированного долга?","answer":"≥5 лет, запрет досрочного требования, досрочное погашение без снижения пруденциалов, 10-я очередь при ликвидации"},
  {"question":"161. Макс. риски по ЛСБОО на всех заёмщиков?","answer":"Не более собственного капитала"},
  {"question":"162. Собственный капитал?","answer":"Капитал 1-го уровня + капитал 2-го уровня"},
  {"question":"163. К7 (краткосрочные обязательства нерезидентов)?","answer":"Сумма обязательств нерезидентов / собственный капитал"},
  {"question":"164. Не высоколиквидные бумаги?","answer":"С обратным выкупом + в залоге"},
  {"question":"165. Земля с/х назначения в ипотеке?","answer":"Отсрочка реализации до 1 года"},
  {"question":"166. План рекапитализации при нарушении достаточности капитала?","answer":"1 месяц с даты нарушения"},
  {"question":"167. Размер риска на заёмщика соотносится с?","answer":"Собственным капиталом банка"},
  {"question":"168. Учёт бессрочных фин.инструментов в капитале?","answer":"Фактически поступившие деньги"},
  {"question":"169. Подтверждение аудитором ежеквартальных ОПиУ/баланса?","answer":"Нет, банк публикует самостоятельно по МСФО"},
  {"question":"170. Источники для физлица-крупного участника?","answer":"Предпринимательство/труд, накопления, дарение/выигрыш/продажа дарёного (≤25%)"},
  {"question":"171. Освобождение залогодержателя на торгах?","answer":"Гарантийный взнос + оплата цены при выигрыше"},
  {"question":"172. Банк создаёт для надёжности?","answer":"Провизии по МСФО"},
  {"question":"173. Лицензии Казпочты для банковских операций?","answer":"Депозиты/счёта — только с лицензией НБ; остальные — без"},
  {"question":"174. Контроль процессов в банке?","answer":"Руководство (должностные лица)"},
  {"question":"175. Информация по гарантиям/поручителям?","answer":"В кредитное бюро с согласия"},
  {"question":"176. Юрлицо с коммерческой деятельностью не в форме?","answer":"Фонд, учреждение, религиозное/общественное объединение, потребкооператив"},
  {"question":"177. Предпринимательство на имуществе госфонда?","answer":"Вещное право использования"},
  {"question":"178. Типовой устав общества утверждает?","answer":"Минюст"},
  {"question":"179. Возврат по займу родовыми вещами вместо денег?","answer":"Да с согласия; деньги/вещи по договору"},
  {"question":"180. Возникновение ипотеки?","answer":"Регистрация договора"},
  {"question":"181. Ипотечное свидетельство?","answer":"Ордерная ЦБ: исполнение по основному обязательству + взыскание на залог"},
  {"question":"182. Депозитарий ФО?","answer":"Электронная база: годовая ФО, аудит, аффилированные, корпоративные события"},
  {"question":"183. Обязательный взнос в КФГД?","answer":"≤0,5% от депозитов на 1-е число квартала; чрезвычайный ≤ суммы квартальных"},
  {"question":"184. Принципы КФГД?","answer":"Обязательность, прозрачность, снижение рисков, накопительный резерв"},
  {"question":"185. Открытие филиала резидентом за рубежом?","answer":"Решение СД + соглашение регуляторов + уведомление НБ в 30 дней"},
  {"question":"186. Уведомление КФГД об изменении наименования?","answer":"10 рабочих дней после получения документов"},
  {"question":"187. Основания исключения из КФГД?","answer":"Лишение/сдача лицензии, реорганизация/ликвидация, неисполнение обязанностей"},
  {"question":"188. Разрешённая деятельность банка на РЦБ?","answer":"Брокер/дилер с госЦБ, кастодиальная, трансфер-агент"},
  {"question":"189. Макс. доля банка в других организациях?","answer":"10% СК банка + 10% от размещённых акций"},
  {"question":"190. Комитет по кадрам и вознаграждению?","answer":"Председатель — независимый член СД; члены с опытом управления персоналом"},
  {"question":"191. Периодичность аудита?","answer":"Общая — ≤1 раз/год; один вопрос — ≤1 раз/3 года"},
  {"question":"192. Собрание СД после фингода?","answer":"≤5 месяцев"},
  {"question":"193. Отрицательный собственный капитал?","answer":"Обязательства > активы"},
  {"question":"194. Запрет на должность председателя правления?","answer":"Крупный участник"},
  {"question":"195. Однородные фин.инструменты со ставкой?","answer":"Один эмитент, равная доходность/валюта/срок погашения"},
  {"question":"196. Повторное избрание руководителя после отказа регулятора?","answer":"90 календарных дней (≤2 раза за 12 мес.)"},
  {"question":"197. Срок реструктуризации при неспособности исполнять отдельным кредиторам?","answer":"Не определён"},
  {"question":"198. Стаж кандидата в правление (банки/аудит/АФР)?","answer":"≥2 года (≥1 год руководящая)"},
  {"question":"199. Банк-кастодиан — аффилированное лицо клиента?","answer":"Нет (искл. НБ)"},
  {"question":"200. Информация при рассмотрении займа?","answer":"Скоринг свой + по кредитному бюро"},
  {"question":"201. Реализация акций в залоге юрлица банком?","answer":"≤12 месяцев"},
  {"question":"202. Информация при рассмотрении займа (повтор)?","answer":"Скоринг свой + по кредитному бюро"},
  {"question":"203. Банк не изменил рекламу по требованию регулятора?","answer":"Регулятор публикует опровержение за счёт банка"},
  {"question":"204. Зачисление принятых наличных в рабочее время?","answer":"В тот же день"},
  {"question":"205. Кредитная политика банка?","answer":"Кредитный комитет"},
  {"question":"206. Передача в залог без СД?","answer":"≤10% капитала"},
  {"question":"207. Консервация банка за счёт?","answer":"Средств банка"},
  {"question":"208. Извещение регулятора дочкой об изменениях учредительных документов?","answer":"≤30 календарных дней"},
  {"question":"209. Обязательный аудит?","answer":"Все банки по году (искл. лишённые лицензии/в ликвидации)"},
  {"question":"210. Политика комплаенс-риска?","answer":"Комитет по рискам правления"},
  {"question":"211. ЦБ исламского банка?","answer":"Акции, паи ИФ, арендные/участия сертификаты, иные"},
  {"question":"212. Не относятся к операциям исламского банка?","answer":"Факторинг + форфейтинг"},
  {"question":"213. Предельный срок консервации?","answer":"1 год"},
  {"question":"214. Исковая давность по требованиям к заёмщикам?","answer":"5 лет"},
  {"question":"215. Изъятие денег со счетов клиентов без согласия?","answer":"Да (ошибка зачисления + подделка документов)"},
  {"question":"216. Документы для >25% крупного участника?","answer":"Бизнес-план на 5 лет, утверждённый НБ"},
  {"question":"217. При консервации банка?","answer":"Регулятор назначает временную администрацию/управляющего на 1 год"},
  {"question":"218. Передача информации по счетам юрлица?","answer":"Первый руководитель/следователь + санкция прокурора"},
  {"question":"219. Очередь требований ЛСБОО по депозитам исламского банка?","answer":"4-я (вкл. пенсионные/страховые)"},
  {"question":"220. Приобретение акций/долей при переходе залога в собственность?","answer":"Да, ≤10% капитала банка"},
  {"question":"221. Исполнение решения банковского омбудсмена?","answer":"Обязательно"},
  {"question":"222. Рассмотрение ходатайства о добровольной реорганизации?","answer":"≤2 месяца с приёма"},
  {"question":"223. Справки о счетах при смерти владельца?","answer":"Завещанные, суды/нотариусы, консульства, наследники"},
  {"question":"224. Не банковская тайна по кредитам?","answer":"Кредиты в ликвидации"},
  {"question":"225. Банковский депозитный сертификат?","answer":"Именная неэмиссионная: номинал + вознаграждение в сроки"},
  {"question":"226. СМИ для дополнительных публикаций?","answer":"Устав общества (помимо ЦД)"},
  {"question":"227. Запрещённые действия временной администрации?","answer":"Взаимозачёты + уступка прав кредиторам"},
  {"question":"228. Холдинг-нерезидент с ≥25% в банке РК?","answer":"Финорганизация под консолидированным надзором в своей стране"},
  {"question":"229. Консервация исламского банка?","answer":"Собрание акционеров + согласование регулятора"},
  {"question":"230. Прекращение ипотечного свидетельства?","answer":"Исполнение, добровольная передача, отсутствие требования 30 дней после срока, утрата предмета"},
  {"question":"231. Запреты банку-кастодиану?","answer":"Использование активов в своих интересах + залог"},
  {"question":"232. Стабилизационный банк?","answer":"Создан регулятором для передачи активов/обязательств из консервации"},
  {"question":"233. Макс. доля участия банка в юрлице?","answer":"≤50% собственного капитала"},
  {"question":"234. Основания реструктуризации банка?","answer":"Неспособность исполнять отдельным кредиторам из-за отсутствия денег"},
  {"question":"235. Бланковый заём?","answer":"Клиенту с высокой кредитоспособностью/надёжностью"},
  {"question":"236. Преимущественная покупка акций при конвертации в реструктуризации?","answer":"Нет"},
  {"question":"237. Операции ипотечной организации?","answer":"Инвестиционная, литература по ипотеке, реализация своего имущества"},
  {"question":"238. Признаки неустойчивого положения юрлица-крупного участника?","answer":"<2 лет, обязательства > активы (без долей), убытки 2 года, просрочка банку, ухудшение, имущества недостаточно"},
  {"question":"239. Срок избрания омбудсмена?","answer":"2 года"},
  {"question":"240. Выплата КФГД сверх спецрезерва?","answer":"За счёт НБ"},
  {"question":"241. Несоблюдение раскрытия общих условий?","answer":"Приостановление/лишение лицензии (все/отдельные)"},
  {"question":"242. Запрещённые сделки главбуха?","answer":"С собой/близкими родственниками, юрлицом с родственником-должностным, супругами/их родственниками"},
  {"question":"243. Запрет участия ипотечной организации?","answer":"Финансовые организации, юрлица с высшей категорией на фонде, автоматизация ипотеки, ТОО/АО"},
  {"question":"244. Уведомление регулятора при наступлении статуса холдинга/крупного участника?","answer":"10 календарных дней"},
  {"question":"245. Цель аудита?","answer":"Точность/полнота отчётности, соответствие законам РК/ВНД банка"},
  {"question":"246. Уведомление крупным участником об изменении количества акций?","answer":"≤30 календарных дней"},
  {"question":"247. Финансирование банковского омбудсмена?","answer":"Взносы банков"},
  {"question":"248. Размещение акций по требованию регулятора?","answer":"5 рабочих дней, публикация 2 языками в депозитарии ФО, равные условия пропорционально, цена по правлению"},
  {"question":"249. Счётная комиссия?","answer":"≥100 акционеров"},
  {"question":"250. Не голосующие акции?","answer":"Выкупленные обществом + в номинальном держании без учёта в ЦД"},
  {"question":"251. Председатель АО — председатель в другом АО?","answer":"Нет"},
  {"question":"252. Предложение преимущественной покупки акций АО?","answer":"≤10 календарных дней (уведомление 2 языками в ЦД)"},
  {"question":"253. Кодекс корпоративного управления АО?","answer":"Регулирует отношения акционеров/общества/органов/заинтересованных лиц"},
  {"question":"254. Сведения об аффилированных в банк?","answer":"≤7 календарных дней с аффилированности"},
  {"question":"255. Порядок предоставления сведений об аффилированных?","answer":"Устав"},
  {"question":"256. Учёт аффилированных банком по?","answer":"Информация от лиц банка + ЦД по крупным акционерам"},
  {"question":"257. Кумулятивное голосование?","answer":"Одна акция — один голос на СД"},
  {"question":"258. Выбор банка-агента для гарантированных депозитов?","answer":"Правление КФГД"},
  {"question":"259. Созыв членов СД?","answer":"1 член СД, внутренний/внешний аудит, крупный акционер"},
  {"question":"260. Требования к банку-агенту КФГД?","answer":"Соглашение с условиями/порядком перечисления"},
  {"question":"261. Ухудшение финположения банка?","answer":"Принудительные меры надзорного реагирования"},
  {"question":"262. Члены СД из физлиц?","answer":"Акционеры, представители акционеров, не акционеры/представители"},
  {"question":"263. Действительность чека?","answer":"10 календарных дней со следующего после выписки"},
  {"question":"264. Отказ чекодержателя от частичного платежа?","answer":"Да"},
  {"question":"265. Участники особого режима?","answer":"Финорганизации/юрлица в финсфере, концентрация ресурсов, платёжные услуги"},
  {"question":"266. Необеспеченное обязательство при ликвидации?","answer":"10-я очередь"},
  {"question":"267. Санкция за ≥3 нарушения в 12 месяцев?","answer":"Приостановление/лишение лицензии, отдельные операции"},
  {"question":"268. Уведомление кредиторам о разделении АО?","answer":"≤2 месяца после решения"},
  {"question":"269. Решение по статусу крупного участника/холдинга?","answer":"≤50 рабочих дней"},
  {"question":"270. Дочки банковского холдинга создают свои дочки?","answer":"Нет"},
  {"question":"271. Повышение ставки по кредиту?","answer":"Не ранее 3 лет по соглашению"},
  {"question":"272. Ассоциация Финансистов Казахстана?","answer":"Некоммерческая организация"},
  {"question":"273. Предоставление ФО?","answer":"Не позднее 31 мая за предыдущий год"},
  {"question":"274. Дочка банка создаёт свою дочку?","answer":"Нет (искл. нерезиденты по техсопровождению)"},
  {"question":"275. Финорганизации — публичный интерес?","answer":"Да (искл. обменники с лицензией НБРК)"},
  {"question":"276. Кто может не вести бухучёт?","answer":"3 категории одновременно: спецналог (патент/упрощёнка), не на НДС, не монополия"},
  {"question":"277. Письменное соглашение с регулятором?","answer":"Улучшение финсостояния, устранение рисков/нарушений, ограничения"},
  {"question":"278. Кредиты участникам банковского конгломерата?","answer":"Вправе"},
  {"question":"279. Документ учредителя-нерезидента ЮЛ для открытия банка?","answer":"Уведомление регулятора своей страны о разрешении (или что не требуется)"},
  {"question":"280. Заимодатель?","answer":"Банк или юрлицо с лицензией на кредиты"},
  {"question":"281. Содержание заявления на открытие банка?","answer":"2 языка + адрес заявителя"},
  {"question":"282. Дочерний банк от родительского нерезидента?","answer":"С определённым рейтингом по регулятору"},
  {"question":"283. Крупный участник/холдинг при увеличении акций предоставляет?","answer":"Источники средств + подтверждающие документы"},
  {"question":"284. Повторная подача кандидата-руководителя после 2 отказов?","answer":"Через 12 месяцев"},
  {"question":"285. Выкуп акций правительством РК?","answer":"≤5 календарных дней вернуть предыдущее свидетельство регистрации"},
  {"question":"286. Члены правления от правительства при покупке акций?","answer":"≤30% состава"},
  {"question":"287. Член правления в других организациях?","answer":"Да с согласия СД (искл. председатель в других банках)"},
  {"question":"288. Действительный отчёт аудитора?","answer":"Независим от акционеров + лицензия на аудит"},
  {"question":"289. Возмещение вреда при смерти?","answer":"Нетрудоспособные иждивенцы, ребёнок, супруг/родители по уходу, внуки/братья/сёстры <14 лет"},
  {"question":"290. Работники аудита в правление/СД?","answer":"Нет"},
  {"question":"291. Сервитут?","answer":"Ограниченное пользование чужой недвижимостью"},
  {"question":"292. Копии аудитотчёта юрлицам конгломерата?","answer":"≤10 календарных дней"},
  {"question":"293. Конвертирование ЦБ в простые акции?","answer":"Проспект, план реструктуризации, решение регулятора, план реабилитации"},
  {"question":"294. Отмена разрешения на открытие банка?","answer":"Добровольное прекращение, суд, нет регистрации в НАО 2 мес., нет лицензии 1 год"},
  {"question":"295. Решения СД по дочкам при госакциях?","answer":"При наличии акций банка у правительства РК"},
  {"question":"296. Уведомление акционеров об увеличении акций для пруденциалов?","answer":"10 рабочих дней очно / 15 заочно"},
  {"question":"297. Периодичность/размер дивидендов на акцию?","answer":"Устав"},
  {"question":"298. Риск на 1 заёмщика к капиталу?","answer":"ЛСБОО 0,10; беззалоговые 0,10; залоговые 0,25 + 2 мес. платежей"},
  {"question":"299. Стресс-тестирование включает?","answer":"Достаточность капитала, риски среды, бизнес-модель, участие СД в оценке/минимизации"},
  {"question":"300. Не включается в риск на 1 заёмщика?","answer":"Гос/квази/фонды по ЦБ + требования к дочке"},
  {"question":"301. Компоненты капитала 2 уровня?","answer":"Субординированный долг − субордолг банка"},
  {"question":"302. Запрет выкупа размещённых акций обществом?","answer":"До ГОСА, до отчёта учредителям, нарушение мин.УК, неплатежеспособность/банкротство, решение о ликвидации"},
  {"question":"303. Ответственность клиента перед фин.агентом?","answer":"Да, за недействительность требования"},
  {"question":"304. Процедуры перед торгами залога?","answer":"Уведомление залогодателю, объявление ≥30 дней до вручения, запрет сделок с публикации, ≥10 дней между публикацией/торгами"},
  {"question":"305. Запрет акционерам-нерезидентам банка?","answer":"Владение голосующими акциями резидентов из офшоров без письма регулятора своей страны"},
  {"question":"306. Запрет выплаты дивидендов?","answer":"Отрицательный капитал, банкротство/реабилитация, снижение пруденциалов, риски/нарушения"},
  {"question":"307. Документы бухгалтерской деятельности?","answer":"Первичные, регистры, ФО, учётная политика"},
  {"question":"308. Обязаны вести учёт по МСФО?","answer":"Крупное предпринимательство + организации публичного интереса"}
];

// ==========================
// Локальные состояния
// ==========================
let currentQuestionIndex = 0;
let currentNewQuestionIndex = 0;
let currentTestQuestionIndex = -1;
let testQuestions = [];
let answeredQuestions = new Set();
let currentQuiz = [];
let currentQuizIndex = 0;
let quizScore = 0;
let userAnswers = [];
let isAnswerSelected = false;

// ===== Theme =====
function toggleTheme() {
  const themeToggle = document.getElementById('themeToggle');
  const html = document.documentElement;
  if (html.getAttribute('data-theme') === 'dark') {
    html.setAttribute('data-theme', 'light');
    themeToggle.innerHTML = '<i>🌙</i> Тёмная тема';
    localStorage.setItem('theme', 'light');
  } else {
    html.setAttribute('data-theme', 'dark');
    themeToggle.innerHTML = '<i>☀️</i> Светлая тема';
    localStorage.setItem('theme', 'dark');
  }
}

// ===== Menu =====
function toggleMenu() {
  const panel = document.getElementById('menuPanel');
  panel.classList.toggle('show');
}

function closeMenu() {
  const panel = document.getElementById('menuPanel');
  panel.classList.remove('show');
}

// закрытие по клику вне меню
document.addEventListener('click', (e) => {
  const panel = document.getElementById('menuPanel');
  const btn = document.getElementById('menuBtn');
  if (!panel || !btn) return;
  const isInside = panel.contains(e.target) || btn.contains(e.target);
  if (!isInside) panel.classList.remove('show');
});

// ===== Tabs =====
function switchTab(tabName) {
  // выключаем все секции
  document.querySelectorAll('.tab-content').forEach(content => {
    content.classList.remove('active');
  });
  
  // включаем нужную
  const target = document.getElementById(`${tabName}-tab`);
  if (target) target.classList.add('active');
  
  // если открыли тест — готовим
  if (tabName === 'test') {
    if (testQuestions.length === 0) initializeTest();
  }
  
  // если открыли quiz — показываем стартовый экран
  if (tabName === 'quiz') {
    startNewQuiz();
  }
  
  // если открыли новые вопросы — создаем их
  if (tabName === 'new_study') {
    createNewQuestions();
  }
  
  saveScrollPosition();
}

// ===== Study create =====
function createQuestions() {
  const container = document.querySelector('.study-container');
  if (!container) return;
  
  container.innerHTML = '';
  
  questionsAndAnswers.forEach((item, index) => {
    const questionDiv = document.createElement('div');
    questionDiv.classList.add('question');
    if (index === currentQuestionIndex) questionDiv.classList.add('active');
    
    questionDiv.innerHTML = `
      <div class="question-number">Вопрос ${index + 1}</div>
      <div class="question-text">${item.question}</div>
      <div class="answer">${item.answer}</div>
    `;
    container.appendChild(questionDiv);
  });
  
  const quickNav = document.createElement('div');
  quickNav.className = 'quick-navigation';
  quickNav.innerHTML = `
    <label for="questionNumber">Перейти к вопросу:</label>
    <input type="number" id="questionNumber" class="number-input" min="1" max="${questionsAndAnswers.length}" value="${currentQuestionIndex + 1}" pattern="[0-9]*" inputmode="numeric">
    <button class="go-button" onclick="goToQuestionByInput()">Перейти</button>
  `;
  container.appendChild(quickNav);
  
  const navigation = document.createElement('div');
  navigation.className = 'navigation';
  navigation.innerHTML = `
    <button class="nav-button" onclick="changeQuestion('prev')">Назад</button>
    <div class="question-counter" id="questionCounter">Вопрос ${currentQuestionIndex + 1} из ${questionsAndAnswers.length}</div>
    <button class="nav-button" onclick="changeQuestion('next')">Вперед</button>
  `;
  container.appendChild(navigation);
  
  const numberInput = document.getElementById('questionNumber');
  if (numberInput) {
    numberInput.addEventListener('keypress', function(e) {
      if (e.key === 'Enter') goToQuestionByInput();
    });
    numberInput.addEventListener('input', function(e) {
      this.value = this.value.replace(/[^0-9]/g, '');
    });
  }
}

// ===== New Study create =====
function createNewQuestions() {
  const container = document.querySelector('.study-container-new');
  if (!container) return;
  
  container.innerHTML = '';
  
  if (newQuestionsAndAnswers.length === 0) {
    container.innerHTML = `
      <div style="text-align: center; padding: 50px;">
        <h2 style="color: var(--text-color); margin-bottom: 20px;">Новые вопросы</h2>
        <p style="color: var(--text-secondary); margin-bottom: 30px;">
          Раздел с новыми вопросами находится в разработке.<br>
          Добавьте вопросы в массив <strong>newQuestionsAndAnswers</strong>.
        </p>
        <button class="home-btn" onclick="switchTab('study')">
          Перейти к основным вопросам
        </button>
      </div>
    `;
    return;
  }
  
  newQuestionsAndAnswers.forEach((item, index) => {
    const questionDiv = document.createElement('div');
    questionDiv.classList.add('question');
    if (index === currentNewQuestionIndex) questionDiv.classList.add('active');
    
    questionDiv.innerHTML = `
      <div class="question-number">Новый вопрос ${index + 1}</div>
      <div class="question-text">${item.question}</div>
      <div class="answer">${item.answer}</div>
    `;
    container.appendChild(questionDiv);
  });
  
  const quickNav = document.createElement('div');
  quickNav.className = 'quick-navigation';
  quickNav.innerHTML = `
    <label for="newQuestionNumber">Перейти к вопросу:</label>
    <input type="number" id="newQuestionNumber" class="number-input" min="1" max="${newQuestionsAndAnswers.length}" value="${currentNewQuestionIndex + 1}" pattern="[0-9]*" inputmode="numeric">
    <button class="go-button" onclick="goToNewQuestionByInput()">Перейти</button>
  `;
  container.appendChild(quickNav);
  
  const navigation = document.createElement('div');
  navigation.className = 'navigation';
  navigation.innerHTML = `
    <button class="nav-button" onclick="changeNewQuestion('prev')">Назад</button>
    <div class="question-counter" id="newQuestionCounter">Новый вопрос ${currentNewQuestionIndex + 1} из ${newQuestionsAndAnswers.length}</div>
    <button class="nav-button" onclick="changeNewQuestion('next')">Вперед</button>
  `;
  container.appendChild(navigation);
  
  const numberInput = document.getElementById('newQuestionNumber');
  if (numberInput) {
    numberInput.addEventListener('keypress', function(e) {
      if (e.key === 'Enter') goToNewQuestionByInput();
    });
    numberInput.addEventListener('input', function(e) {
      this.value = this.value.replace(/[^0-9]/g, '');
    });
  }
}

function shuffleArray(array) {
  for (let i = array.length - 1; i > 0; i--) {
    const j = Math.floor(Math.random() * (i + 1));
    [array[i], array[j]] = [array[j], array[i]];
  }
  return array;
}

// ===== TEST MODE =====
function initializeTest() {
  testQuestions = Array.from({length: questionsAndAnswers.length}, (_, i) => i);
  shuffleArray(testQuestions);
  answeredQuestions.clear();
  currentTestQuestionIndex = -1;
  updateTestStats();
  document.getElementById('showAnswerBtn').style.display = 'none';
  document.getElementById('testAnswer').classList.remove('show');
  document.getElementById('testQuestionNumber').textContent = 'Вопрос #';
  document.getElementById('testQuestionText').textContent = 'Нажмите "Следующий вопрос" для начала тестирования';
  document.getElementById('testAnswer').textContent = '';
}

function nextTestQuestion() {
  if (answeredQuestions.size >= questionsAndAnswers.length) {
    resetTestSilent();
    return;
  }
  
  let nextIndex;
  do {
    currentTestQuestionIndex = (currentTestQuestionIndex + 1) % testQuestions.length;
    nextIndex = testQuestions[currentTestQuestionIndex];
  } while (answeredQuestions.has(nextIndex));
  
  const question = questionsAndAnswers[nextIndex];
  document.getElementById('testQuestionNumber').textContent = `Вопрос ${nextIndex + 1}`;
  document.getElementById('testQuestionText').textContent = question.question;
  document.getElementById('testAnswer').textContent = question.answer;
  document.getElementById('testAnswer').classList.remove('show');
  document.getElementById('showAnswerBtn').style.display = 'block';
  updateTestStats();
  saveScrollPosition();
}

function showTestAnswer() {
  const ans = document.getElementById('testAnswer');
  if (!ans) return;
  ans.classList.add('show');
  if (currentTestQuestionIndex >= 0) {
    const questionIndex = testQuestions[currentTestQuestionIndex];
    answeredQuestions.add(questionIndex);
    updateTestStats();
  }
}

function updateTestStats() {
  const stats = document.getElementById('testStats');
  if (!stats) return;
  stats.textContent = `Вопросов пройдено: ${answeredQuestions.size} | Осталось: ${questionsAndAnswers.length - answeredQuestions.size}`;
}

function resetTest() {
  if (confirm('Вы уверены, что хотите начать тест заново? Весь прогресс будет сброшен.')) {
    initializeTest();
  }
}

function resetTestSilent() {
  initializeTest();
}

// ===== Study navigation =====
function goToQuestionByInput() {
  const input = document.getElementById('questionNumber');
  if (!input) return;
  let questionNumber = parseInt(input.value);
  if (isNaN(questionNumber) || questionNumber < 1 || questionNumber > questionsAndAnswers.length) {
    alert(`Пожалуйста, введите число от 1 до ${questionsAndAnswers.length}`);
    input.value = currentQuestionIndex + 1;
    return;
  }
  goToQuestion(questionNumber - 1);
}

function goToQuestion(index) {
  currentQuestionIndex = index;
  showQuestion(currentQuestionIndex);
  updateQuestionCounter();
  updateInputValue();
  saveScrollPosition();
}

function updateInputValue() {
  const input = document.getElementById('questionNumber');
  if (input) input.value = currentQuestionIndex + 1;
}

function showQuestion(index) {
  const questions = document.querySelectorAll('.question');
  questions.forEach(q => q.classList.remove('active'));
  if (questions[index]) questions[index].classList.add('active');
}

function updateQuestionCounter() {
  const counter = document.getElementById('questionCounter');
  if (counter) counter.textContent = `Вопрос ${currentQuestionIndex + 1} из ${questionsAndAnswers.length}`;
}

function changeQuestion(direction) {
  if (direction === 'next') {
    currentQuestionIndex = (currentQuestionIndex + 1) % questionsAndAnswers.length;
  } else if (direction === 'prev') {
    currentQuestionIndex = (currentQuestionIndex - 1 + questionsAndAnswers.length) % questionsAndAnswers.length;
  }
  showQuestion(currentQuestionIndex);
  updateQuestionCounter();
  updateInputValue();
  saveScrollPosition();
}

// ===== New Study navigation =====
function goToNewQuestionByInput() {
  const input = document.getElementById('newQuestionNumber');
  if (!input) return;
  let questionNumber = parseInt(input.value);
  if (isNaN(questionNumber) || questionNumber < 1 || questionNumber > newQuestionsAndAnswers.length) {
    alert(`Пожалуйста, введите число от 1 до ${newQuestionsAndAnswers.length}`);
    input.value = currentNewQuestionIndex + 1;
    return;
  }
  goToNewQuestion(questionNumber - 1);
}

function goToNewQuestion(index) {
  currentNewQuestionIndex = index;
  showNewQuestion(currentNewQuestionIndex);
  updateNewQuestionCounter();
  updateNewInputValue();
  saveScrollPosition();
}

function updateNewInputValue() {
  const input = document.getElementById('newQuestionNumber');
  if (input) input.value = currentNewQuestionIndex + 1;
}

function showNewQuestion(index) {
  const questions = document.querySelectorAll('#new_study-tab .question');
  questions.forEach(q => q.classList.remove('active'));
  if (questions[index]) questions[index].classList.add('active');
}

function updateNewQuestionCounter() {
  const counter = document.getElementById('newQuestionCounter');
  if (counter) counter.textContent = `Новый вопрос ${currentNewQuestionIndex + 1} из ${newQuestionsAndAnswers.length}`;
}

function changeNewQuestion(direction) {
  if (direction === 'next') {
    currentNewQuestionIndex = (currentNewQuestionIndex + 1) % newQuestionsAndAnswers.length;
  } else if (direction === 'prev') {
    currentNewQuestionIndex = (currentNewQuestionIndex - 1 + newQuestionsAndAnswers.length) % newQuestionsAndAnswers.length;
  }
  showNewQuestion(currentNewQuestionIndex);
  updateNewQuestionCounter();
  updateNewInputValue();
  saveScrollPosition();
}

// ===== Меню действие =====
function resetStudyTo1() {
  goToQuestion(0);
  switchTab('study');
}

// ===== Quiz 10 =====
function startNewQuiz() {
  const quizContent = document.getElementById('quizContent');
  quizContent.innerHTML = `
    <div class="quiz-question" style="text-align: center; padding: 50px 25px;">
      <div class="quiz-question-text">Готовы начать тест из 10 вопросов?</div>
      <div style="margin-top: 30px;">
        <button class="quiz-button" onclick="startQuizConfirmed()" style="margin: 10px;">
          Да, начать тест
        </button>
        <button class="quiz-button" onclick="switchTab('study')" style="margin: 10px; background: var(--border-color);">
          Нет, вернуться
        </button>
      </div>
    </div>
  `;
  document.getElementById('nextQuizBtn').style.display = 'none';
  document.getElementById('finishQuizBtn').style.display = 'none';
  document.getElementById('quizResult').classList.remove('show');
  quizContent.style.display = 'block';
  document.getElementById('correctCount').textContent = '0';
  document.getElementById('totalCount').textContent = '0';
}

function startQuizConfirmed() {
  const shuffledQuestions = [...quizQuestions];
  shuffleArray(shuffledQuestions);
  currentQuiz = shuffledQuestions.slice(0, 10);
  
  currentQuiz.forEach(question => {
    const correctOption = question.options[question.correct];
    shuffleArray(question.options);
    question.correct = question.options.indexOf(correctOption);
  });
  
  currentQuizIndex = 0;
  quizScore = 0;
  userAnswers = [];
  isAnswerSelected = false;
  
  document.getElementById('nextQuizBtn').style.display = 'block';
  document.getElementById('nextQuizBtn').disabled = true;
  document.getElementById('finishQuizBtn').style.display = 'none';
  document.getElementById('quizResult').classList.remove('show');
  
  displayQuizQuestion();
}

function displayQuizQuestion() {
  const quizContent = document.getElementById('quizContent');
  const question = currentQuiz[currentQuizIndex];
  isAnswerSelected = false;
  
  document.getElementById('correctCount').textContent = quizScore;
  document.getElementById('totalCount').textContent = currentQuizIndex;
  
  let optionsHTML = '';
  question.options.forEach((option, index) => {
    optionsHTML += `
      <div class="quiz-option" onclick="selectQuizOption(${index})" id="quizOption${index}">
        ${option}
      </div>
    `;
  });
  
  quizContent.innerHTML = `
    <div class="quiz-question">
      <div class="quiz-question-number">Вопрос ${currentQuizIndex + 1} из 10</div>
      <div class="quiz-question-text">${question.question}</div>
      <div class="quiz-options">
        ${optionsHTML}
      </div>
      <div class="quiz-feedback" id="quizFeedback"></div>
    </div>
  `;
  
  if (currentQuizIndex === 9) {
    document.getElementById('nextQuizBtn').textContent = 'Завершить тест';
    document.getElementById('finishQuizBtn').style.display = 'none';
  } else {
    document.getElementById('nextQuizBtn').textContent = 'Следующий вопрос';
  }
}

function selectQuizOption(optionIndex) {
  if (isAnswerSelected) return;
  
  const question = currentQuiz[currentQuizIndex];
  const options = document.querySelectorAll('.quiz-option');
  const feedback = document.getElementById('quizFeedback');
  
  options.forEach(option => option.classList.remove('selected', 'correct', 'incorrect'));
  options[optionIndex].classList.add('selected');
  
  const isCorrect = optionIndex === question.correct;
  
  if (isCorrect) {
    options[optionIndex].classList.add('correct');
    feedback.textContent = "✓ Правильно!";
    feedback.className = "quiz-feedback correct show";
  } else {
    options[optionIndex].classList.add('incorrect');
    options[question.correct].classList.add('correct');
    feedback.textContent = "✗ Неправильно.";
    feedback.className = "quiz-feedback incorrect show";
  }
  
  userAnswers[currentQuizIndex] = {
    question: question.question,
    userAnswer: optionIndex,
    correctAnswer: question.correct,
    isCorrect: isCorrect,
    options: question.options
  };
  
  if (isCorrect && !userAnswers[currentQuizIndex].counted) {
    quizScore++;
    userAnswers[currentQuizIndex].counted = true;
  }
  
  document.getElementById('nextQuizBtn').disabled = false;
  options.forEach(option => {
    option.style.pointerEvents = 'none';
    option.style.opacity = '0.7';
  });
  
  isAnswerSelected = true;
}

function nextQuizQuestion() {
  if (currentQuizIndex < 9) {
    currentQuizIndex++;
    displayQuizQuestion();
    document.getElementById('nextQuizBtn').disabled = true;
  } else {
    finishQuiz();
  }
}

function finishQuiz() {
  document.getElementById('quizContent').style.display = 'none';
  const resultElement = document.getElementById('quizResult');
  const scoreElement = document.getElementById('quizScore');
  const messageElement = document.getElementById('quizMessage');
  
  const percentage = Math.round((quizScore / 10) * 100);
  scoreElement.textContent = `${percentage}%`;
  
  let message = `Вы правильно ответили на ${quizScore} из 10 вопросов.`;
  if (percentage >= 90) message += " Отличный результат!";
  else if (percentage >= 70) message += " Хороший результат!";
  else if (percentage >= 50) message += " Удовлетворительный результат.";
  else message += " Вам нужно больше практики.";
  
  messageElement.textContent = message;
  resultElement.classList.add('show');
  document.getElementById('nextQuizBtn').style.display = 'none';
  document.getElementById('finishQuizBtn').style.display = 'none';
}

function restartQuiz() {
  document.getElementById('quizResult').classList.remove('show');
  startNewQuiz();
}

function saveScrollPosition() {
  localStorage.setItem('scrollPosition', window.scrollY);
  localStorage.setItem('currentQuestion', currentQuestionIndex);
  localStorage.setItem('currentNewQuestion', currentNewQuestionIndex);
}

function restoreScrollPosition() {
  const savedPosition = localStorage.getItem('scrollPosition');
  const savedQuestion = localStorage.getItem('currentQuestion');
  const savedNewQuestion = localStorage.getItem('currentNewQuestion');
  
  if (savedQuestion) currentQuestionIndex = parseInt(savedQuestion);
  if (savedNewQuestion) currentNewQuestionIndex = parseInt(savedNewQuestion);
  if (savedPosition) window.scrollTo(0, parseInt(savedPosition));
}

// ===== onload =====
window.onload = function() {
  const savedTheme = localStorage.getItem('theme') || 'light';
  document.documentElement.setAttribute('data-theme', savedTheme);
  const themeToggle = document.getElementById('themeToggle');
  themeToggle.innerHTML = savedTheme === 'dark' ? '<i>☀️</i> Светлая тема' : '<i>🌙</i> Тёмная тема';
  
  restoreScrollPosition();
  createQuestions();
  showQuestion(currentQuestionIndex);
  updateQuestionCounter();
  updateInputValue();
  initializeTest();
  
  // Motivation restore
  const savedMotivationIndex = localStorage.getItem('motivationIndex');
  if (savedMotivationIndex) motivationIndex = parseInt(savedMotivationIndex) || 0;
  renderMotivation();
  resetCountdown();
  startMotivationAuto();
  
  // iOS Safari fix
  setTimeout(() => {
    document.body.style.overflow = 'auto';
  }, 100);
};

window.addEventListener('beforeunload', function() {
  saveScrollPosition();
});
</script>
</body>
</html>
