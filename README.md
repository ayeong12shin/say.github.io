<!doctype html>
<html lang="ko">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Green Challenge - 환경 & 에너지 퀴즈</title>
<!-- PDF 다운로드를 위한 jsPDF 및 autotable 로드 -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf-autotable/3.5.28/jspdf.plugin.autotable.min.js"></script>

<style>
:root{--green:#197443;--deep:#105b34;--mint:#e7f5e8;--line:#dce9df;--ink:#20352a;--muted:#718078;--bg:#f4faf4;--red:#d9534f;--yellow:#f0ad4e}
*{box-sizing:border-box}
body{margin:0;background:linear-gradient(140deg,#eff9ef,#f8fcf8);font-family:system-ui,-apple-system,"Noto Sans KR",sans-serif;color:var(--ink);min-height:100vh}
button,input,textarea,select{font:inherit}
button{cursor:pointer;border:0;border-radius:12px;padding:12px 16px;font-weight:750;background:var(--green);color:#fff;transition:all 0.2s}
button:hover{opacity:0.9}
button:disabled{opacity:.45;cursor:not-allowed}
.btn-secondary{background:#edf5ee;color:var(--green);border:1px solid var(--line)}
.btn-danger{background:var(--red);color:#fff}
.btn-warning{background:var(--yellow);color:#fff}
.btn-sm{padding:6px 12px;font-size:12px;border-radius:8px}

.wrap{max-width:880px;margin:auto;padding:16px}
.top{display:flex;justify-content:space-between;align-items:center;gap:10px;margin:6px 0 18px}
.brand{font-size:21px;font-weight:950;color:var(--deep);display:flex;align-items:center;gap:6px}
.tag,.pill{font-size:12px;background:#e4f3e4;color:var(--green);padding:6px 10px;border-radius:99px;font-weight:800}

.card{background:#fff;border:1px solid var(--line);border-radius:20px;padding:24px;box-shadow:0 8px 24px #194c2610;margin:14px 0}
.hero{text-align:center;background:linear-gradient(145deg,#e3f4df,#f8fff4);padding:28px 18px}
.hero h1{font-size:clamp(26px,5vw,36px);margin:8px 0;color:var(--deep)}
.leaf{font-size:38px}
.muted,.notice{color:var(--muted);font-size:13px;line-height:1.6}
.notice.error{color:var(--red);font-weight:700}
.center{text-align:center}

.field{display:grid;gap:7px;margin:13px 0}
.field label{font-weight:750;font-size:14px}
.field input,.field textarea,.field select{width:100%;border:1px solid #ccddd0;border-radius:12px;padding:12px;background:#fbfefb}
.field textarea{min-height:110px;resize:vertical}

.primary{width:100%;margin-top:8px}
.hidden{display:none!important}
.row{display:flex;align-items:center;justify-content:space-between;gap:10px;flex-wrap:wrap}

.progress{height:8px;background:#e7efe8;border-radius:20px;overflow:hidden;margin:12px 0 18px}
.progress>div{height:100%;background:linear-gradient(90deg,#72b64c,#16804a);width:0;transition:width 0.3s}

.timer{font-size:21px;font-weight:900;color:var(--green)}
.question{font-size:18px;font-weight:800;line-height:1.6;margin:16px 0}
.passage{padding:16px;border-radius:14px;background:#f3f8f2;border:1px solid var(--line);line-height:1.75;font-size:14px;white-space:pre-line;max-height:280px;overflow-y:auto;margin-bottom:16px}
.passage-title{font-weight:900;font-size:15px;color:var(--deep);margin-bottom:8px;border-bottom:2px solid #cce3d0;padding-bottom:4px}

.choice{display:block;width:100%;text-align:left;background:#f8fcf8;color:var(--ink);border:1px solid var(--line);margin:9px 0;padding:14px;border-radius:13px}
.choice:hover:not(:disabled){border-color:#78b889;background:#f0f8ef}
.choice.correct{border:2px solid #2d9959;background:#eaf7ed;font-weight:bold}
.choice.wrong{border:2px solid #d9796c;background:#fff0ee}

.feedback{padding:14px;border-radius:14px;background:#edf7ed;margin-top:14px;line-height:1.65}
.hint{padding:10px 12px;border-radius:10px;background:#edf7ed;color:#27663e;font-size:13px;margin:10px 0}
.pointline{display:flex;justify-content:space-between;align-items:center;margin-top:18px;padding-top:12px;border-top:1px solid var(--line);font-size:13px;color:var(--muted)}

.stats{display:grid;grid-template-columns:repeat(3,1fr);gap:9px;margin-top:18px}
.stat{background:#f1f8f0;border-radius:14px;padding:13px;text-align:center}
.stat b{display:block;font-size:22px;color:var(--green)}

table{width:100%;border-collapse:collapse;margin-top:10px}
td,th{text-align:left;padding:10px 8px;border-bottom:1px solid var(--line);font-size:13px}
th{color:#365d40;background:#eff7ed}

.footer{text-align:center;color:#72907a;font-size:12px;padding:20px}
.admin-nav{display:flex;gap:8px;margin-bottom:16px;border-bottom:2px solid var(--line);padding-bottom:10px;overflow-x:auto}
.admin-nav button{background:transparent;color:var(--ink);border:1px solid transparent;border-radius:8px;padding:8px 14px;white-space:nowrap}
.admin-nav button.active{background:var(--green);color:#fff}
.tab-content{display:none}
.tab-content.active{display:block}

.modal{position:fixed;top:0;left:0;width:100%;height:100%;background:rgba(0,0,0,0.5);display:flex;align-items:center;justify-content:center;z-index:999;padding:16px}
.modal-body{background:#fff;padding:24px;border-radius:20px;max-width:600px;width:100%;max-height:90vh;overflow-y:auto}

.dash-cards{display:grid;grid-template-columns:repeat(3,1fr);gap:10px;margin-bottom:16px}
.dash-card{background:#edf7ed;border-radius:14px;padding:14px;text-align:center;border:1px solid var(--line)}
.dash-card b{display:block;font-size:22px;color:var(--green);margin-top:4px}

@media(max-width:520px){
  .wrap{padding:10px}.card{padding:16px}.stats,.dash-cards{grid-template-columns:1fr}.question{font-size:16px}
}
</style>

<!-- Firebase SDK (v9 호환 버전) -->
<script src="https://www.gstatic.com/firebasejs/9.22.1/firebase-app-compat.js"></script>
<script src="https://www.gstatic.com/firebasejs/9.22.1/firebase-database-compat.js"></script>
</head>
<body>
<div class="wrap">
  <header class="top">
    <div class="brand">🌿 GREEN CHALLENGE</div>
    <div>
      <span class="tag">환경 & 에너지 퀴즈</span>
      <button class="btn-secondary btn-sm" onclick="showAdminLogin()" style="margin-left:6px">⚙️ 관리자</button>
    </div>
  </header>

  <!-- 메인 참여 화면 -->
  <section id="home">
    <div class="card hero">
      <div class="leaf">🌎</div>
      <h1>Green Challenge</h1>
      <p>환경과 에너지, 얼마나 알고 있나요?</p>
      <p class="muted" id="homeSubtitle">설정된 무작위 문항 · 총점 20점</p>
    </div>
    <div class="card">
      <h2 class="center">퀴즈 참여하기</h2>
      <p class="muted center">학번과 닉네임 중복을 실시간으로 확인한 후 퀴즈가 시작됩니다.</p>
      <div class="field">
        <label for="studentId">학번</label>
        <input id="studentId" maxlength="20" placeholder="예: 21501">
      </div>
      <div class="field">
        <label for="studentName">이름</label>
        <input id="studentName" maxlength="30" placeholder="이름을 입력하세요">
      </div>
      <div class="field">
        <label for="nickname">닉네임</label>
        <input id="nickname" maxlength="16" placeholder="순위표에 표시될 닉네임">
      </div>
      <p id="homeMsg" class="notice error"></p>
      <button id="startBtn" class="primary" onclick="startQuiz()">퀴즈 시작하기 →</button>
      <button class="btn-secondary" style="width:100%;margin-top:10px;" onclick="openPublicRankingModal()">🏆 실시간 전체 순위 보기</button>
    </div>
  </section>

  <!-- 퀴즈 진행 화면 -->
  <section id="quiz" class="hidden">
    <div class="card">
      <div class="row">
        <span id="qCount" class="pill">01 / 11</span>
        <span id="difficulty" class="pill">난이도</span>
        <span id="timer" class="timer">30초</span>
      </div>
      <div class="progress"><div id="bar"></div></div>
      
      <div id="passageBox" class="passage hidden">
        <div id="passageTitle" class="passage-title">관련 지문</div>
        <div id="passageText"></div>
      </div>
      
      <div id="questionText" class="question"></div>
      <div id="hint" class="hint hidden"></div>
      <div id="choices"></div>
      
      <div id="feedback" class="feedback hidden"></div>
      <div class="pointline">
        <span>이 문항의 배점</span>
        <b id="points">2점</b>
      </div>
      <div class="row" style="justify-content:flex-end;margin-top:12px">
        <button id="nextBtn" class="hidden" onclick="nextQuestion()">다음 문제 →</button>
      </div>
    </div>
    <p class="notice center">정답을 제출하면 변경할 수 없습니다. 제한 시간이 끝나면 미응답으로 처리됩니다.</p>
  </section>

  <!-- 퀴즈 결과 화면 -->
  <section id="result" class="hidden">
    <div class="card hero">
      <div class="leaf">🏆</div>
      <h1>퀴즈 완료!</h1>
      <p>환경을 지키는 첫걸음, 오늘부터 시작해요.</p>
      <div id="score" style="font-size:52px;font-weight:950;color:var(--deep)">0점</div>
      <b id="correctCount"></b>
      <div class="stats">
        <div class="stat"><b id="myRank">-</b><span>나의 순위</span></div>
        <div class="stat"><b id="elapsed">-</b><span>소요 시간</span></div>
        <div class="stat"><b id="accuracy">-</b><span>정답률</span></div>
      </div>
    </div>
    <div class="card">
      <div class="row">
        <h2>🏆 전체 실시간 순위표</h2>
        <span id="totalPeople" class="muted">동기화 중...</span>
      </div>
      <div id="topTable"></div>
    </div>
    <button class="btn-secondary" style="width:100%" onclick="resetHome()">처음 화면으로</button>
  </section>

  <!-- 대중용 실시간 순위 모달 -->
  <div id="publicRankingModal" class="modal hidden">
    <div class="modal-body">
      <div class="row" style="margin-bottom:12px">
        <h2>🏆 실시간 전체 순위표</h2>
        <button class="btn-secondary btn-sm" onclick="closePublicRankingModal()">닫기</button>
      </div>
      <div class="field">
        <input id="publicSearchInput" placeholder="닉네임을 검색하세요 (자동 스크롤)" oninput="filterPublicRanking()">
      </div>
      <p id="publicSearchMsg" class="notice error" style="margin:4px 0;"></p>
      <div style="max-height:400px;overflow-y:auto;border:1px solid var(--line);border-radius:12px;">
        <table id="publicRankingTable" style="margin-top:0">
          <thead>
            <tr>
              <th>순위</th>
              <th>닉네임</th>
              <th>점수</th>
              <th>소요시간</th>
            </tr>
          </thead>
          <tbody></tbody>
        </table>
      </div>
    </div>
  </div>

  <!-- 관리자 로그인 모달 -->
  <div id="adminLoginModal" class="modal hidden">
    <div class="modal-body" style="max-width:400px">
      <h2>⚙️ 관리자 로그인</h2>
      <div class="field">
        <label>관리자 ID</label>
        <input id="adminIdInput" type="text" placeholder="아이디 입력">
      </div>
      <div class="field">
        <label>비밀번호</label>
        <input id="adminPwInput" type="password" placeholder="비밀번호 입력">
      </div>
      <p id="adminLoginMsg" class="notice error"></p>
      <div style="display:flex;gap:8px;margin-top:14px">
        <button class="primary" onclick="loginAdmin()">로그인</button>
        <button class="btn-secondary" onclick="hideAdminLogin()">취소</button>
      </div>
    </div>
  </div>

  <!-- 관리자 페이지 -->
  <section id="adminPage" class="hidden">
    <div class="card">
      <div class="row" style="margin-bottom:12px">
        <h2>⚙️ 관리자 대시보드</h2>
        <button class="btn-danger btn-sm" onclick="logoutAdmin()">로그아웃</button>
      </div>

      <!-- 통계 카드 -->
      <div class="dash-cards">
        <div class="dash-card">전체 참여자<b id="statTotalUsers">0명</b></div>
        <div class="dash-card">평균 점수<b id="statAvgScore">0점</b></div>
        <div class="dash-card">최고 점수<b id="statMaxScore">0점</b></div>
      </div>
      
      <div class="admin-nav">
        <button id="tabBtn1" class="active" onclick="switchAdminTab('tab1')">👥 참가자 관리</button>
        <button id="tabBtn2" onclick="switchAdminTab('tab2')">📝 문제 관리</button>
        <button id="tabBtn3" onclick="switchAdminTab('tab3')">⚙️ 퀴즈 설정</button>
        <button id="tabBtn4" onclick="switchAdminTab('tab4')">📊 백업/복원</button>
      </div>

      <!-- TAB 1: 참가자 관리 & 검색 -->
      <div id="tab1" class="tab-content active">
        <div class="row" style="margin-bottom:10px">
          <h3>실시간 전체 응시 결과</h3>
          <div style="display:flex;gap:6px;flex-wrap:wrap">
            <button class="btn-secondary btn-sm" onclick="exportToCSV()">📥 CSV</button>
            <button class="btn-secondary btn-sm" onclick="exportToPDF()">📄 PDF</button>
            <button class="btn-danger btn-sm" onclick="clearResults()">🗑️ 전체기록 초기화</button>
          </div>
        </div>
        <div class="field">
          <input id="adminSearchInput" placeholder="🔍 이름, 닉네임 또는 학번으로 검색" oninput="renderAdminParticipants()">
        </div>
        <div style="overflow-x:auto">
          <table id="adminParticipantTable">
            <thead>
              <tr>
                <th>순위</th>
                <th>학번</th>
                <th>이름</th>
                <th>닉네임</th>
                <th>점수</th>
                <th>소요시간</th>
                <th>응시일시</th>
                <th>관리</th>
              </tr>
            </thead>
            <tbody></tbody>
          </table>
        </div>
      </div>

      <!-- TAB 2: 문제 직접 출제 관리 -->
      <div id="tab2" class="tab-content">
        <div class="row" style="margin-bottom:10px">
          <h3>등록된 문제 목록 (총 <span id="qTotalCount">0</span>개)</h3>
          <div>
            <button class="btn-secondary btn-sm" onclick="openAddQuestionModal()">➕ 새 문제 추가</button>
            <button class="btn-danger btn-sm" onclick="clearAllQuestions()">🗑️ 전체 문제 비우기</button>
          </div>
        </div>
        <div style="overflow-x:auto;max-height:400px;overflow-y:auto">
          <table id="adminQuestionTable">
            <thead>
              <tr>
                <th>ID</th>
                <th>유형</th>
                <th>질문 및 정답</th>
                <th>해설</th>
                <th>관리</th>
              </tr>
            </thead>
            <tbody></tbody>
          </table>
        </div>
      </div>

      <!-- TAB 3: 퀴즈 설정 탭 -->
      <div id="tab3" class="tab-content">
        <h3>퀴즈 환경 및 출제 규칙 설정</h3>
        <p class="muted">문항 수, 무작위 출제 개수, 점수 및 참여 허용 상태를 설정합니다.</p>
        
        <div class="field" style="background:#f3f8f2;padding:14px;border-radius:14px;border:1px solid var(--line);margin:16px 0;">
          <label style="font-size:16px;display:flex;align-items:center;gap:10px;">
            <input type="checkbox" id="settingQuizActive" style="width:20px;height:20px;"> 
            <span><b>퀴즈 참여 허용 (ON/OFF)</b></span>
          </label>
          <span class="muted" style="margin-left:30px;">체크 해제(OFF) 시 학생들이 참여하려고 할 때 '현재 참여 시간이 아닙니다' 문구가 표시됩니다.</span>
        </div>

        <h4 style="margin-top:20px;color:var(--deep)">무작위 출제 개수 및 배점 설정</h4>
        <div style="display:grid;grid-template-columns:1fr 1fr;gap:12px;">
          <div class="field"><label>객관식 [하] 출제 개수</label><input id="setCntLow" type="number" value="4"></div>
          <div class="field"><label>객관식 [하] 문항당 점수</label><input id="setPtsLow" type="number" value="1"></div>
          
          <div class="field"><label>객관식 [중] 출제 개수</label><input id="setCntMed" type="number" value="3"></div>
          <div class="field"><label>객관식 [중] 문항당 점수</label><input id="setPtsMed" type="number" value="2"></div>
          
          <div class="field"><label>주관식 출제 개수</label><input id="setCntSub" type="number" value="2"></div>
          <div class="field"><label>주관식 문항당 점수</label><input id="setPtsSub" type="number" value="3"></div>
          
          <div class="field"><label>지문형 세트 출제 수 (1세트=2문제)</label><input id="setCntPas" type="number" value="1"></div>
          <div class="field"><label>지문형 문항당 점수</label><input id="setPtsPas" type="number" value="2"></div>
        </div>

        <button class="primary" style="margin-top:16px;" onclick="saveQuizSettings()">⚙️ 설정 내용 서버에 저장하기</button>
      </div>

      <!-- TAB 4: 백업 및 시스템 설정 -->
      <div id="tab4" class="tab-content">
        <h3>서버 데이터 백업 및 복원</h3>
        <p class="muted">Firebase 클라우드에 저장된 모든 데이터(문제집 및 응시 결과)를 백업하거나 복원합니다.</p>
        <div style="display:flex;gap:10px;margin:16px 0">
          <button class="btn-secondary" onclick="exportFullData()">💾 전체 데이터 백업 (JSON)</button>
          <button class="btn-secondary" onclick="document.getElementById('importFileInput').click()">📂 데이터 복원 (JSON)</button>
          <input type="file" id="importFileInput" class="hidden" accept=".json" onchange="importFullData(event)">
        </div>
      </div>
    </div>
    <button class="btn-secondary" style="width:100%" onclick="hideAdminPage()">← 메인 화면으로 돌아가기</button>
  </section>

  <!-- 문제 추가/수정 모달 -->
  <div id="qModal" class="modal hidden">
    <div class="modal-body">
      <h3 id="qModalTitle">새 문제 추가</h3>
      <div class="field">
        <label>문제 유형 선택</label>
        <select id="qModalType" onchange="toggleQModalFields()">
          <option value="low">객관식 [하] (문제, 선지 4개, 정답, 해설)</option>
          <option value="medium">객관식 [중] (문제, 선지 4개, 정답, 해설)</option>
          <option value="subjective">주관식 (문제, 정답, 힌트, 해설)</option>
          <option value="passage">지문형 세트 (공통 지문, 질문, 선지 4개, 정답, 해설)</option>
        </select>
      </div>

      <div style="display:grid;grid-template-columns:1fr 1fr;gap:10px;">
        <div class="field">
          <label>제한시간 (초)</label>
          <input id="qModalTimeLimit" type="number" value="30">
        </div>
      </div>

      <!-- 지문형 전용 필드 -->
      <div id="qModalPassageField" class="field hidden">
        <label>공통 지문 내용 (세트 식별용)</label>
        <textarea id="qModalPassageText" placeholder="지문 내용을 입력하세요"></textarea>
      </div>

      <div class="field">
        <label>질문 내용</label>
        <textarea id="qModalQuestion" placeholder="질문을 입력하세요"></textarea>
      </div>
      
      <!-- 객관식 선지 필드 -->
      <div id="qModalChoiceFields">
        <label style="font-weight:750;font-size:14px">선택지 4개 및 정답 지정</label>
        <div class="field"><input id="qOpt1" placeholder="보기 1번"></div>
        <div class="field"><input id="qOpt2" placeholder="보기 2번"></div>
        <div class="field"><input id="qOpt3" placeholder="보기 3번"></div>
        <div class="field"><input id="qOpt4" placeholder="보기 4번"></div>
        <div class="field">
          <label>정답 선택지 번호</label>
          <select id="qCorrectChoice">
            <option value="1">1번</option>
            <option value="2">2번</option>
            <option value="3">3번</option>
            <option value="4">4번</option>
          </select>
        </div>
      </div>

      <!-- 주관식 필드 -->
      <div id="qModalSubjFields" class="hidden">
        <div class="field">
          <label>주관식 정답</label>
          <input id="qSubjAnswer" placeholder="정답 텍스트">
        </div>
        <div class="field">
          <label>초성 힌트</label>
          <input id="qSubjHint" placeholder="예: ㄷㅅㅇㅅㅎㅅ">
        </div>
      </div>

      <div class="field">
        <label>해설</label>
        <textarea id="qModalExplanation" placeholder="문제 정답 해설을 입력하세요"></textarea>
      </div>

      <div style="display:flex;gap:8px;margin-top:14px">
        <button class="primary" onclick="saveQuestionModal()">저장하기</button>
        <button class="btn-secondary" onclick="closeQModal()">취소</button>
      </div>
    </div>
  </div>

  <div class="footer">🌱 GREEN CHALLENGE · 함께하는 퀴즈로 더 푸른 지구를!</div>
</div>

<script>
// =================================================================
// 1. FIREBASE 설정 (본인 설정값 반영 완료)
// =================================================================
const firebaseConfig = {
  apiKey: "AIzaSyCkyAggm2Kd0YLPuAwFiCU_8Aw0JEHkbDk",
  authDomain: "green-challenge-6b48c.firebaseapp.com",
  databaseURL: "https://green-challenge-6b48c-default-rtdb.firebaseio.com",
  projectId: "green-challenge-6b48c",
  storageBucket: "green-challenge-6b48c.firebasestorage.app",
  messagingSenderId: "610467303039",
  appId: "1:610467303039:web:ae211039efd89fcf06f94a"
};

firebase.initializeApp(firebaseConfig);
const db = firebase.database();

// 전역 변수
let globalQuestionsCache = [];
let globalResultsCache = [];
let quizSettings = {
  quizActive: true,
  cntLow: 4, ptsLow: 1,
  cntMed: 3, ptsMed: 2,
  cntSub: 2, ptsSub: 3,
  cntPas: 1, ptsPas: 2
};

let currentQuizList = [];
let currentIndex = 0;
let score = 0;
let startTime = 0;
let timerInterval = null;
let timeLeft = 30;
let currentUser = { id: '', name: '', nickname: '' };
let editingQId = null;

// 실시간 동기화 리스너
window.addEventListener('DOMContentLoaded', () => {
  db.ref('questions').on('value', (snapshot) => {
    const data = snapshot.val();
    globalQuestionsCache = data ? (Array.isArray(data) ? data : Object.values(data)) : [];
    renderAdminQuestions();
  });

  db.ref('results').on('value', (snapshot) => {
    const data = snapshot.val();
    globalResultsCache = data ? Object.values(data) : [];
    renderRankings();
    renderAdminParticipants();
    updateAdminDashboardStats();
  });

  db.ref('settings').on('value', (snapshot) => {
    const data = snapshot.val();
    if (data) {
      quizSettings = { ...quizSettings, ...data };
      document.getElementById('settingQuizActive').checked = quizSettings.quizActive;
      document.getElementById('setCntLow').value = quizSettings.cntLow;
      document.getElementById('setPtsLow').value = quizSettings.ptsLow;
      document.getElementById('setCntMed').value = quizSettings.cntMed;
      document.getElementById('setPtsMed').value = quizSettings.ptsMed;
      document.getElementById('setCntSub').value = quizSettings.cntSub;
      document.getElementById('setPtsSub').value = quizSettings.ptsSub;
      document.getElementById('setCntPas').value = quizSettings.cntPas;
      document.getElementById('setPtsPas').value = quizSettings.ptsPas;

      let totalQs = parseInt(quizSettings.cntLow) + parseInt(quizSettings.cntMed) + parseInt(quizSettings.cntSub) + (parseInt(quizSettings.cntPas) * 2);
      let totalPts = (quizSettings.cntLow * quizSettings.ptsLow) + (quizSettings.cntMed * quizSettings.ptsMed) + (quizSettings.cntSub * quizSettings.ptsSub) + (quizSettings.cntPas * 2 * quizSettings.ptsPas);
      document.getElementById('homeSubtitle').innerText = `무작위 ${totalQs}문항 · 총점 ${totalPts}점`;
    }
  });
});

async function startQuiz() {
  const id = document.getElementById('studentId').value.trim();
  const name = document.getElementById('studentName').value.trim();
  const nickname = document.getElementById('nickname').value.trim();
  const msgEl = document.getElementById('homeMsg');

  if (!quizSettings.quizActive) {
    msgEl.innerText = "현재 참여 시간이 아닙니다. 관리자에 의해 퀴즈가 비활성화되어 있습니다.";
    return;
  }

  if (!id || !name || !nickname) {
    msgEl.innerText = "학번, 이름, 닉네임을 모두 입력해 주세요.";
    return;
  }

  document.getElementById('startBtn').disabled = true;
  msgEl.innerText = "서버 중복 확인 중...";

  try {
    const idSnapshot = await db.ref('results/' + id).once('value');
    if (idSnapshot.exists()) {
      msgEl.innerText = "이미 응시를 완료한 학번입니다.";
      document.getElementById('startBtn').disabled = false;
      return;
    }

    const nickSnapshot = await db.ref('results').orderByChild('nickname').equalTo(nickname).once('value');
    if (nickSnapshot.exists()) {
      msgEl.innerText = "이미 사용 중인 닉네임입니다. 다른 닉네임을 입력해 주세요.";
      document.getElementById('startBtn').disabled = false;
      return;
    }
  } catch (err) {
    console.error(err);
    msgEl.innerText = "서버 통신 오류가 발생했습니다. 다시 시도해 주세요.";
    document.getElementById('startBtn').disabled = false;
    return;
  }

  // 설정된 개수만큼 무작위 추출 (지문형은 마지막 배치, 1세트당 2문제)
  const lowPool = globalQuestionsCache.filter(q => q.type === 'low').sort(() => 0.5 - Math.random()).slice(0, quizSettings.cntLow);
  const medPool = globalQuestionsCache.filter(q => q.type === 'medium').sort(() => 0.5 - Math.random()).slice(0, quizSettings.cntMed);
  const subPool = globalQuestionsCache.filter(q => q.type === 'subjective').sort(() => 0.5 - Math.random()).slice(0, quizSettings.cntSub);

  // 지문형 세트 추출 (지문 내용이 같은 것끼리 그룹핑 후 세트 단위 무작위 추출)
  const passageMap = {};
  globalQuestionsCache.filter(q => q.type === 'passage' && q.passageText).forEach(q => {
    if (!passageMap[q.passageText]) passageMap[q.passageText] = [];
    passageMap[q.passageText].push(q);
  });

  let passageKeys = Object.keys(passageMap).sort(() => 0.5 - Math.random()).slice(0, quizSettings.cntPas);
  let passagePool = [];
  passageKeys.forEach(k => {
    // 1세트당 2문항씩 가져오기
    let setQs = passageMap[k].slice(0, 2);
    passagePool.push(...setQs);
  });

  // 점수 부여 일괄 적용
  lowPool.forEach(q => q.points = quizSettings.ptsLow);
  medPool.forEach(q => q.points = quizSettings.ptsMed);
  subPool.forEach(q => q.points = quizSettings.ptsSub);
  passagePool.forEach(q => q.points = quizSettings.ptsPas);

  // 지문형은 가장 마지막에 배치
  currentQuizList = [...lowPool, ...medPool, ...subPool, ...passagePool];

  if (currentQuizList.length === 0) {
    msgEl.innerText = "출제할 퀴즈 문제가 부족합니다. 관리자에게 문의하세요.";
    document.getElementById('startBtn').disabled = false;
    return;
  }

  msgEl.innerText = "";
  currentUser = { id, name, nickname };
  currentIndex = 0;
  score = 0;
  startTime = Date.now();

  document.getElementById('startBtn').disabled = false;
  document.getElementById('home').classList.add('hidden');
  document.getElementById('quiz').classList.remove('hidden');
  loadQuestion();
}

function loadQuestion() {
  if (currentIndex >= currentQuizList.length) {
    endQuiz();
    return;
  }

  const q = currentQuizList[currentIndex];
  document.getElementById('qCount').innerText = `${String(currentIndex + 1).padStart(2, '0')} / ${String(currentQuizList.length).padStart(2, '0')}`;
  document.getElementById('points').innerText = `${q.points}점`;
  document.getElementById('questionText').innerText = q.question;
  document.getElementById('feedback').classList.add('hidden');
  document.getElementById('nextBtn').classList.add('hidden');

  const passageBox = document.getElementById('passageBox');
  if (q.type === 'passage' && q.passageText) {
    document.getElementById('passageTitle').innerText = "관련 지문";
    document.getElementById('passageText').innerText = q.passageText;
    passageBox.classList.remove('hidden');
  } else {
    passageBox.classList.add('hidden');
  }

  const choicesEl = document.getElementById('choices');
  choicesEl.innerHTML = '';

  if (q.type === 'subjective') {
    document.getElementById('difficulty').innerText = '주관식';
    const hintEl = document.getElementById('hint');
    if (q.hint) {
      hintEl.innerText = `💡 초성 힌트: ${q.hint}`;
      hintEl.classList.remove('hidden');
    } else {
      hintEl.classList.add('hidden');
    }
    choicesEl.innerHTML = `
      <div class="field">
        <input type="text" id="subjectiveAnswer" placeholder="정답을 입력하세요" onkeydown="if(event.key==='Enter') submitSubjective()">
      </div>
      <button class="primary" onclick="submitSubjective()">정답 제출</button>
    `;
  } else {
    document.getElementById('difficulty').innerText = q.type === 'low' ? '객관식 [하]' : (q.type === 'medium' ? '객관식 [중]' : '지문형');
    document.getElementById('hint').classList.add('hidden');

    if (q.choices) {
      q.choices.forEach(ch => {
        const btn = document.createElement('button');
        btn.className = 'choice';
        btn.innerText = `${ch.id}. ${ch.text}`;
        btn.onclick = () => selectChoice(ch.correct, btn, q.explanation);
        choicesEl.appendChild(btn);
      });
    }
  }

  timeLeft = q.timeLimit ? parseInt(q.timeLimit) : 30;
  const maxTime = timeLeft;
  document.getElementById('timer').innerText = `${timeLeft}초`;
  document.getElementById('bar').style.width = '100%';

  clearInterval(timerInterval);
  timerInterval = setInterval(() => {
    timeLeft--;
    document.getElementById('timer').innerText = `${timeLeft}초`;
    document.getElementById('bar').style.width = `${(timeLeft / maxTime) * 100}%`;
    if (timeLeft <= 0) {
      clearInterval(timerInterval);
      timeOutAction(q);
    }
  }, 1000);
}

function selectChoice(isCorrect, selectedBtn, explanation) {
  clearInterval(timerInterval);
  const choices = document.querySelectorAll('.choice');
  choices.forEach(b => b.disabled = true);

  if (isCorrect) {
    selectedBtn.classList.add('correct');
    score += parseInt(currentQuizList[currentIndex].points || 2);
  } else {
    selectedBtn.classList.add('wrong');
  }

  showFeedback(isCorrect, explanation);
}

function submitSubjective() {
  const input = document.getElementById('subjectiveAnswer');
  if(!input || input.disabled) return;

  clearInterval(timerInterval);
  const userAns = input.value.trim().replace(/\s+/g, '');
  const q = currentQuizList[currentIndex];
  const correctAns = (q.answer || '').replace(/\s+/g, '');

  const isCorrect = userAns === correctAns;
  if (isCorrect) {
    score += parseInt(q.points || 3);
  }

  input.disabled = true;
  showFeedback(isCorrect, q.explanation);
}

function timeOutAction(q) {
  const choices = document.querySelectorAll('.choice');
  choices.forEach(b => b.disabled = true);
  const subInput = document.getElementById('subjectiveAnswer');
  if (subInput) subInput.disabled = true;

  showFeedback(false, `⏰ 제한 시간이 초과되었습니다.<br>정답 해설: ${q.explanation || q.answer || '-'}`);
}

function showFeedback(isCorrect, explanation) {
  const fb = document.getElementById('feedback');
  fb.innerHTML = `<b>${isCorrect ? '⭕ 정답입니다! 🎉' : '❌ 아쉽네요. 오답입니다.'}</b><br>${explanation || ''}`;
  fb.classList.remove('hidden');
  document.getElementById('nextBtn').classList.remove('hidden');
}

function nextQuestion() {
  currentIndex++;
  loadQuestion();
}

function endQuiz() {
  clearInterval(timerInterval);
  document.getElementById('quiz').classList.add('hidden');
  document.getElementById('result').classList.remove('hidden');

  const elapsedSec = Math.floor((Date.now() - startTime) / 1000);
  const dateStr = new Date().toLocaleString('ko-KR');

  let totalPossibleScore = currentQuizList.reduce((acc, q) => acc + parseInt(q.points || 2), 0);

  const newRecord = {
    studentId: currentUser.id,
    studentName: currentUser.name,
    nickname: currentUser.nickname,
    score: score,
    elapsed: elapsedSec,
    date: dateStr
  };

  db.ref('results/' + currentUser.id).set(newRecord);

  document.getElementById('score').innerText = `${score}점`;
  document.getElementById('correctCount').innerText = `총 ${totalPossibleScore}점 만점 중 ${score}점 획득`;
  document.getElementById('elapsed').innerText = `${elapsedSec}초`;
  document.getElementById('accuracy').innerText = `${Math.round((score / totalPossibleScore) * 100)}%`;
}

// 순위 기준: 점수 내림차순, 동점이면 소요 시간 오름차순
function sortResults(results) {
  return results.sort((a, b) => {
    if (b.score !== a.score) return b.score - a.score;
    return a.elapsed - b.elapsed;
  });
}

function renderRankings() {
  const results = sortResults([...globalResultsCache]);

  const myRankIndex = results.findIndex(r => r.studentId === currentUser.id);
  document.getElementById('myRank').innerText = myRankIndex !== -1 ? `${myRankIndex + 1}위` : '-';
  document.getElementById('totalPeople').innerText = `현재 실시간 총 ${results.length}명 참여 중`;

  let html = `<table><tr><th>순위</th><th>닉네임</th><th>점수</th><th>소요시간</th></tr>`;
  results.slice(0, 10).forEach((r, idx) => {
    const isMe = r.studentId === currentUser.id;
    html += `<tr style="${isMe ? 'background:#e7f5e8;font-weight:bold' : ''}">
      <td>${idx + 1}</td>
      <td>${r.nickname} ${isMe ? '(나)' : ''}</td>
      <td>${r.score}점</td>
      <td>${r.elapsed}초</td>
    </tr>`;
  });
  html += `</table>`;
  document.getElementById('topTable').innerHTML = html;
}

// 대중용 전체 순위 모달 및 검색 자동 스크롤
function openPublicRankingModal() {
  document.getElementById('publicRankingModal').classList.remove('hidden');
  document.getElementById('publicSearchInput').value = '';
  document.getElementById('publicSearchMsg').innerText = '';
  renderPublicRankingTable(globalResultsCache);
}

function closePublicRankingModal() {
  document.getElementById('publicRankingModal').classList.add('hidden');
}

function renderPublicRankingTable(results) {
  const sorted = sortResults([...results]);
  const tbody = document.querySelector('#publicRankingTable tbody');
  tbody.innerHTML = '';

  if (sorted.length === 0) {
    tbody.innerHTML = `<tr><td colspan="4" class="center muted">참여 기록이 없습니다.</td></tr>`;
    return;
  }

  sorted.forEach((r, idx) => {
    const tr = document.createElement('tr');
    tr.id = `pub_row_${r.nickname}`;
    tr.innerHTML = `
      <td><b>${idx + 1}</b></td>
      <td>${r.nickname}</td>
      <td>${r.score}점</td>
      <td>${r.elapsed}초</td>
    `;
    tbody.appendChild(tr);
  });
}

function filterPublicRanking() {
  const keyword = document.getElementById('publicSearchInput').value.trim().toLowerCase();
  const sorted = sortResults([...globalResultsCache]);
  const msgEl = document.getElementById('publicSearchMsg');

  renderPublicRankingTable(sorted);

  if (!keyword) {
    msgEl.innerText = '';
    return;
  }

  const found = sorted.find(r => r.nickname.toLowerCase().includes(keyword));
  if (found) {
    msgEl.innerText = '';
    const targetRow = document.getElementById(`pub_row_${found.nickname}`);
    if (targetRow) {
      targetRow.scrollIntoView({ behavior: 'smooth', block: 'center' });
      targetRow.style.background = '#e7f5e8';
      setTimeout(() => { targetRow.style.background = ''; }, 2000);
    }
  } else {
    msgEl.innerText = '참여 기록이 없습니다.';
  }
}

function resetHome() {
  document.getElementById('result').classList.add('hidden');
  document.getElementById('home').classList.remove('hidden');
  document.getElementById('studentId').value = '';
  document.getElementById('studentName').value = '';
  document.getElementById('nickname').value = '';
  document.getElementById('homeMsg').innerText = '';
}

// 관리자 기능
function showAdminLogin() {
  document.getElementById('adminLoginModal').classList.remove('hidden');
  document.getElementById('adminIdInput').value = '';
  document.getElementById('adminPwInput').value = '';
  document.getElementById('adminLoginMsg').innerText = '';
}

function hideAdminLogin() {
  document.getElementById('adminLoginModal').classList.add('hidden');
}

function loginAdmin() {
  const id = document.getElementById('adminIdInput').value.trim();
  const pw = document.getElementById('adminPwInput').value.trim();

  if (id === 'EK2026' && pw === 'EK1111') {
    hideAdminLogin();
    document.getElementById('home').classList.add('hidden');
    document.getElementById('result').classList.add('hidden');
    document.getElementById('adminPage').classList.remove('hidden');
    renderAdminPage();
  } else {
    document.getElementById('adminLoginMsg').innerText = "아이디 또는 비밀번호가 올바르지 않습니다.";
  }
}

function logoutAdmin() {
  document.getElementById('adminPage').classList.add('hidden');
  document.getElementById('home').classList.remove('hidden');
}

function hideAdminPage() {
  document.getElementById('adminPage').classList.add('hidden');
  document.getElementById('home').classList.remove('hidden');
}

function switchAdminTab(tabId) {
  document.querySelectorAll('.admin-nav button').forEach(b => b.classList.remove('active'));
  document.querySelectorAll('.tab-content').forEach(c => c.classList.remove('active'));

  if (tabId === 'tab1') document.getElementById('tabBtn1').classList.add('active');
  if (tabId === 'tab2') document.getElementById('tabBtn2').classList.add('active');
  if (tabId === 'tab3') document.getElementById('tabBtn3').classList.add('active');
  if (tabId === 'tab4') document.getElementById('tabBtn4').classList.add('active');

  document.getElementById(tabId).classList.add('active');
}

function renderAdminPage() {
  renderAdminParticipants();
  renderAdminQuestions();
  updateAdminDashboardStats();
}

function updateAdminDashboardStats() {
  const results = globalResultsCache;
  const total = results.length;
  document.getElementById('statTotalUsers').innerText = `${total}명`;

  if (total === 0) {
    document.getElementById('statAvgScore').innerText = '0점';
    document.getElementById('statMaxScore').innerText = '0점';
  } else {
    const sum = results.reduce((acc, r) => acc + r.score, 0);
    const avg = (sum / total).toFixed(1);
    const max = Math.max(...results.map(r => r.score));
    document.getElementById('statAvgScore').innerText = `${avg}점`;
    document.getElementById('statMaxScore').innerText = `${max}점`;
  }
}

// 참가자 관리 검색 기능 (이름, 닉네임, 학번)
function renderAdminParticipants() {
  const keyword = document.getElementById('adminSearchInput') ? document.getElementById('adminSearchInput').value.trim().toLowerCase() : '';
  const results = sortResults([...globalResultsCache]);

  const filtered = results.filter(r => 
    r.studentName.toLowerCase().includes(keyword) || 
    r.nickname.toLowerCase().includes(keyword) || 
    r.studentId.toLowerCase().includes(keyword)
  );

  const tbody = document.querySelector('#adminParticipantTable tbody');
  tbody.innerHTML = '';

  if (filtered.length === 0) {
    tbody.innerHTML = `<tr><td colspan="8" class="center muted">참가 기록이 없습니다.</td></tr>`;
    return;
  }

  filtered.forEach((r, idx) => {
    const originalIdx = results.findIndex(item => item.studentId === r.studentId);
    const tr = document.createElement('tr');
    tr.innerHTML = `
      <td><b>${originalIdx + 1}</b></td>
      <td>${r.studentId}</td>
      <td>${r.studentName}</td>
      <td>${r.nickname}</td>
      <td><b>${r.score}점</b></td>
      <td>${r.elapsed}초</td>
      <td>${r.date || '-'}</td>
      <td><button class="btn-danger btn-sm" onclick="deleteParticipant('${r.studentId}')">삭제</button></td>
    `;
    tbody.appendChild(tr);
  });
}

function deleteParticipant(studentId) {
  if (confirm(`학번 ${studentId} 참가자의 응시 기록을 서버에서 삭제하시겠습니까?`)) {
    db.ref('results/' + studentId).remove().then(() => {
      globalResultsCache = globalResultsCache.filter(r => r.studentId !== studentId);
      renderAdminParticipants();
      renderRankings();
      updateAdminDashboardStats();
    });
  }
}

function clearResults() {
  if (confirm("정말로 서버의 모든 응시자 성적 기록을 초기화하시겠습니까?")) {
    db.ref('results').remove().then(() => {
      globalResultsCache = [];
      renderAdminParticipants();
      renderRankings();
      updateAdminDashboardStats();
    });
  }
}

function saveQuizSettings() {
  const newSet = {
    quizActive: document.getElementById('settingQuizActive').checked,
    cntLow: parseInt(document.getElementById('setCntLow').value) || 0,
    ptsLow: parseInt(document.getElementById('setPtsLow').value) || 1,
    cntMed: parseInt(document.getElementById('setCntMed').value) || 0,
    ptsMed: parseInt(document.getElementById('setPtsMed').value) || 2,
    cntSub: parseInt(document.getElementById('setCntSub').value) || 0,
    ptsSub: parseInt(document.getElementById('setPtsSub').value) || 3,
    cntPas: parseInt(document.getElementById('setCntPas').value) || 0,
    ptsPas: parseInt(document.getElementById('setPtsPas').value) || 2
  };

  db.ref('settings').set(newSet).then(() => {
    alert("퀴즈 설정이 성공적으로 저장되었습니다!");
  });
}

// CSV 다운로드
function exportToCSV() {
  const results = sortResults([...globalResultsCache]);
  if (results.length === 0) {
    alert("다운로드할 응시 기록이 없습니다.");
    return;
  }

  let csvContent = "\uFEFF순위,학번,이름,닉네임,점수,소요시간(초),응시일시\n";
  results.forEach((r, idx) => {
    csvContent += `${idx + 1},"${r.studentId}","${r.studentName}","${r.nickname}",${r.score},${r.elapsed},"${r.date}"\n`;
  });

  const blob = new Blob([csvContent], { type: 'text/csv;charset=utf-8;' });
  const url = URL.createObjectURL(blob);
  const a = document.createElement('a');
  a.href = url;
  a.download = `GreenChallenge_응시결과_${new Date().toISOString().slice(0,10)}.csv`;
  document.body.appendChild(a);
  a.click();
  document.body.removeChild(a);
  URL.revokeObjectURL(url);
}

// PDF 다운로드
function exportToPDF() {
  const results = sortResults([...globalResultsCache]);
  if (results.length === 0) {
    alert("다운로드할 응시 기록이 없습니다.");
    return;
  }

  const { jsPDF } = window.jspdf;
  const doc = new jsPDF();

  doc.setFont("helvetica", "bold");
  doc.setFontSize(18);
  doc.text("Green Challenge - Quiz Results", 14, 20);

  doc.setFontSize(11);
  doc.setFont("helvetica", "normal");
  doc.text(`Total Participants: ${results.length} | Date: ${new Date().toLocaleDateString()}`, 14, 28);

  const tableData = results.map((r, idx) => [
    idx + 1,
    r.studentId,
    r.studentName,
    r.nickname,
    `${r.score} pts`,
    `${r.elapsed}s`,
    r.date || '-'
  ]);

  doc.autoTable({
    startY: 35,
    head: [['Rank', 'Student ID', 'Name', 'Nickname', 'Score', 'Time', 'Date']],
    body: tableData,
    theme: 'grid',
    headStyles: { fillColor: [25, 116, 67] },
    styles: { fontSize: 10, cellPadding: 4 }
  });

  doc.save(`GreenChallenge_Results_${new Date().toISOString().slice(0,10)}.pdf`);
}

function renderAdminQuestions() {
  const qBank = globalQuestionsCache;
  document.getElementById('qTotalCount').innerText = qBank.length;

  const tbody = document.querySelector('#adminQuestionTable tbody');
  tbody.innerHTML = '';

  if (qBank.length === 0) {
    tbody.innerHTML = `<tr><td colspan="5" class="center muted">등록된 문제가 없습니다. 상단 [새 문제 추가] 버튼을 눌러주세요.</td></tr>`;
    return;
  }

  qBank.forEach(q => {
    const tr = document.createElement('tr');
    let ansStr = '';
    if (q.type === 'subjective') {
      ansStr = `정답: ${q.answer} (힌트: ${q.hint || '없음'})`;
    } else {
      const correctCh = q.choices ? q.choices.find(c => c.correct) : null;
      ansStr = `정답: ${correctCh ? correctCh.id + '번 (' + correctCh.text + ')' : '-'}`;
    }

    let typeLabel = q.type === 'low' ? '객관식 [하]' : (q.type === 'medium' ? '객관식 [중]' : (q.type === 'subjective' ? '주관식' : '지문형'));

    tr.innerHTML = `
      <td><b>${q.id}</b></td>
      <td>${typeLabel}</td>
      <td><b>${q.question}</b><br><span class="muted">${ansStr}</span></td>
      <td class="muted">${q.explanation || '-'}</td>
      <td>
        <button class="btn-secondary btn-sm" onclick="openEditQuestionModal('${q.id}')">수정</button>
        <button class="btn-danger btn-sm" onclick="deleteQuestion('${q.id}')">삭제</button>
      </td>
    `;
    tbody.appendChild(tr);
  });
}

function clearAllQuestions() {
  if (confirm("정말로 등록된 모든 문제를 삭제하시겠습니까?")) {
    db.ref('questions').set(null).then(() => {
      globalQuestionsCache = [];
      renderAdminQuestions();
    });
  }
}

function deleteQuestion(qId) {
  if (confirm(`문제 ID [${qId}]를 서버에서 삭제하시겠습니까?`)) {
    const updated = globalQuestionsCache.filter(q => q.id !== qId);
    db.ref('questions').set(updated).then(() => {
      globalQuestionsCache = updated;
      renderAdminQuestions();
    });
  }
}

function openAddQuestionModal() {
  editingQId = null;
  document.getElementById('qModalTitle').innerText = "새 문제 추가";
  document.getElementById('qModalType').value = "low";
  document.getElementById('qModalTimeLimit').value = "30";
  document.getElementById('qModalPassageText').value = "";
  document.getElementById('qModalQuestion').value = "";
  document.getElementById('qOpt1').value = "";
  document.getElementById('qOpt2').value = "";
  document.getElementById('qOpt3').value = "";
  document.getElementById('qOpt4').value = "";
  document.getElementById('qCorrectChoice').value = "1";
  document.getElementById('qSubjAnswer').value = "";
  document.getElementById('qSubjHint').value = "";
  document.getElementById('qModalExplanation').value = "";
  
  toggleQModalFields();
  document.getElementById('qModal').classList.remove('hidden');
}

function openEditQuestionModal(qId) {
  const q = globalQuestionsCache.find(item => item.id === qId);
  if (!q) return;

  editingQId = qId;
  document.getElementById('qModalTitle').innerText = `문제 수정 [ID: ${qId}]`;
  document.getElementById('qModalType').value = q.type || "low";
  document.getElementById('qModalTimeLimit').value = q.timeLimit || 30;
  document.getElementById('qModalPassageText').value = q.passageText || "";
  document.getElementById('qModalQuestion').value = q.question || "";
  document.getElementById('qModalExplanation').value = q.explanation || "";

  if (q.type === 'subjective') {
    document.getElementById('qSubjAnswer').value = q.answer || "";
    document.getElementById('qSubjHint').value = q.hint || "";
  } else if (q.choices && q.choices.length === 4) {
    document.getElementById('qOpt1').value = q.choices[0].text;
    document.getElementById('qOpt2').value = q.choices[1].text;
    document.getElementById('qOpt3').value = q.choices[2].text;
    document.getElementById('qOpt4').value = q.choices[3].text;
    const correctIdx = q.choices.findIndex(c => c.correct);
    document.getElementById('qCorrectChoice').value = correctIdx !== -1 ? String(correctIdx + 1) : "1";
  }

  toggleQModalFields();
  document.getElementById('qModal').classList.remove('hidden');
}

function toggleQModalFields() {
  const type = document.getElementById('qModalType').value;
  const passageField = document.getElementById('qModalPassageField');
  const choiceFields = document.getElementById('qModalChoiceFields');
  const subjFields = document.getElementById('qModalSubjFields');

  if (type === 'passage') {
    passageField.classList.remove('hidden');
    choiceFields.classList.remove('hidden');
    subjFields.classList.add('hidden');
  } else if (type === 'subjective') {
    passageField.classList.add('hidden');
    choiceFields.classList.add('hidden');
    subjFields.classList.remove('hidden');
  } else {
    passageField.classList.add('hidden');
    choiceFields.classList.remove('hidden');
    subjFields.classList.add('hidden');
  }
}

function saveQuestionModal() {
  const type = document.getElementById('qModalType').value;
  const timeLimit = parseInt(document.getElementById('qModalTimeLimit').value) || 30;
  const question = document.getElementById('qModalQuestion').value.trim();
  const explanation = document.getElementById('qModalExplanation').value.trim();
  const passageText = document.getElementById('qModalPassageText').value.trim();

  if (!question) {
    alert("질문 내용을 입력해 주세요.");
    return;
  }

  let qBank = [...globalQuestionsCache];
  let newQ = {
    id: editingQId || ("Q_" + Date.now().toString().slice(-5)),
    type: type,
    timeLimit: timeLimit,
    question: question,
    explanation: explanation
  };

  if (type === 'passage') {
    if (!passageText) { alert("공통 지문 내용을 입력해 주세요."); return; }
    newQ.passageText = passageText;
  }

  if (type === 'subjective') {
    const ans = document.getElementById('qSubjAnswer').value.trim();
    if (!ans) { alert("주관식 정답을 입력해 주세요."); return; }
    newQ.answer = ans;
    newQ.hint = document.getElementById('qSubjHint').value.trim();
  } else {
    const o1 = document.getElementById('qOpt1').value.trim();
    const o2 = document.getElementById('qOpt2').value.trim();
    const o3 = document.getElementById('qOpt3').value.trim();
    const o4 = document.getElementById('qOpt4').value.trim();
    const correctVal = document.getElementById('qCorrectChoice').value;

    if (!o1 || !o2 || !o3 || !o4) { alert("모든 선택지(4개) 텍스트를 입력해 주세요."); return; }

    newQ.choices = [
      { id: "1", text: o1, correct: correctVal === "1" },
      { id: "2", text: o2, correct: correctVal === "2" },
      { id: "3", text: o3, correct: correctVal === "3" },
      { id: "4", text: o4, correct: correctVal === "4" }
    ];
  }

  if (editingQId) {
    const idx = qBank.findIndex(q => q.id === editingQId);
    if (idx !== -1) qBank[idx] = newQ;
  } else {
    qBank.push(newQ);
  }

  db.ref('questions').set(qBank).then(() => {
    globalQuestionsCache = qBank;
    renderAdminQuestions();
    closeQModal();
  });
}

function closeQModal() {
  document.getElementById('qModal').classList.add('hidden');
}

function exportFullData() {
  const data = {
    questions: globalQuestionsCache,
    results: globalResultsCache,
    settings: quizSettings
  };
  const jsonStr = JSON.stringify(data, null, 2);
  const blob = new Blob([jsonStr], { type: 'application/json' });
  const url = URL.createObjectURL(blob);
  const a = document.createElement('a');
  a.href = url;
  a.download = `GreenChallenge_서버백업_${new Date().toISOString().slice(0,10)}.json`;
  a.click();
}

function importFullData(event) {
  const file = event.target.files[0];
  if (!file) return;

  const reader = new FileReader();
  reader.onload = function(e) {
    try {
      const data = JSON.parse(e.target.result);
      if (data.questions && data.results) {
        db.ref('questions').set(data.questions);
        if (data.settings) db.ref('settings').set(data.settings);
        
        const resObj = {};
        data.results.forEach(r => { resObj[r.studentId] = r; });
        db.ref('results').set(resObj);

        alert("서버 데이터가 성공적으로 복원되었습니다!");
      } else {
        alert("올바르지 않은 백업 파일 형식입니다.");
      }
    } catch(err) {
      alert("JSON 파일을 읽는 중 오류가 발생했습니다.");
    }
  };
  reader.readAsText(file);
}
</script>
</body>
</html>
