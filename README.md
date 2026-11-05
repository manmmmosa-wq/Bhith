
<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, viewport-fit=cover">
<title>الباحث - منصة الذكاء الاصطناعي</title>
<link href="https://fonts.googleapis.com/css2?family=IBM+Plex+Sans+Arabic:wght@300;400;500;600;700&family=Amiri:wght@400;700&display=swap" rel="stylesheet">
<script src="https://cdn.jsdelivr.net/npm/marked/marked.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<style>
  :root {
    --bg: #F9F9F8; --sidebar: #F3F2EE; --surface: #FFFFFF;
    --text: #1F1F1F; --muted: #6B6B6B; --border: #E5E5E3;
    --accent: #C15F3C; --accent-hover: #A54E2F;
    --user-bubble: #F0EEE6; --radius: 16px;
    --font-sans: 'IBM Plex Sans Arabic', system-ui, sans-serif;
    --font-serif: 'Amiri', Georgia, serif;
    --shadow: 0 4px 12px rgba(0,0,0,0.05);
    
    --admin-bg: #0F172A; --admin-surface: #1E293B; --admin-border: #334155;
    --admin-text: #F8FAFC; --admin-muted: #94A3B8; --admin-primary: #3B82F6;
    --admin-success: #10B981; --admin-danger: #EF4444; --admin-warning: #F59E0B;
  }

  * { box-sizing: border-box; margin: 0; padding: 0; }
  body { font-family: var(--font-sans); background: var(--bg); color: var(--text); height: 100vh; overflow: hidden; }
  button { font-family: inherit; cursor: pointer; border: none; background: none; color: inherit; transition: 0.2s; }
  input, textarea { font-family: inherit; }
  .hidden { display: none !important; }

  .btn { padding: 10px 16px; border-radius: 10px; font-weight: 500; font-size: 14px; display: inline-flex; align-items: center; justify-content: center; gap: 8px; }
  .btn-primary { background: var(--accent); color: white; }
  .btn-primary:hover { background: var(--accent-hover); }
  .btn-loading { opacity: 0.7; cursor: not-allowed; }

  #view-auth { display: flex; align-items: center; justify-content: center; height: 100vh; padding: 20px; background: var(--bg); }
  .card { background: var(--surface); padding: 32px; border-radius: var(--radius); box-shadow: var(--shadow); width: 100%; max-width: 400px; border: 1px solid var(--border); }
  .auth-logo { font-size: 40px; text-align: center; margin-bottom: 16px; }
  .auth-title { font-family: var(--font-serif); font-size: 26px; font-weight: 700; text-align: center; margin-bottom: 24px; }
  .form-group { margin-bottom: 16px; text-align: right; }
  .form-group label { display: block; margin-bottom: 6px; font-size: 13px; font-weight: 500; color: var(--muted); }
  input { width: 100%; padding: 12px 16px; border: 1px solid var(--border); border-radius: 12px; background: var(--bg); color: var(--text); font-size: 15px; outline: none; }
  input:focus { border-color: var(--accent); }

  .chat-layout { display: flex; height: 100vh; }
  .sidebar { width: 280px; background: var(--sidebar); border-left: 1px solid var(--border); display: flex; flex-direction: column; }
  .sidebar-header { padding: 16px; border-bottom: 1px solid var(--border); display: flex; justify-content: space-between; align-items: center; }
  .brand { display: flex; align-items: center; gap: 8px; font-family: var(--font-serif); font-size: 20px; font-weight: 700; }
  .sidebar-menu { padding: 12px; display: flex; flex-direction: column; gap: 4px; }
  .menu-item { padding: 10px 12px; border-radius: 8px; font-size: 14px; font-weight: 500; text-align: right; display: flex; align-items: center; gap: 10px; cursor: pointer; }
  .menu-item:hover { background: rgba(0,0,0,0.05); }
  .chat-history { flex: 1; overflow-y: auto; padding: 12px; }
  .history-item { padding: 10px 12px; font-size: 13.5px; color: var(--muted); cursor: pointer; border-radius: 8px; white-space: nowrap; overflow: hidden; text-overflow: ellipsis; }
  .history-item:hover, .history-item.active { background: rgba(0,0,0,0.05); color: var(--text); }
  
  .main-chat { flex: 1; display: flex; flex-direction: column; background: var(--bg); min-width: 0; }
  .chat-header { padding: 16px 24px; border-bottom: 1px solid var(--border); display: flex; justify-content: space-between; align-items: center; }
  .chat-messages { flex: 1; overflow-y: auto; padding: 24px; display: flex; flex-direction: column; gap: 24px; }
  .msg { max-width: 800px; margin: 0 auto; width: 100%; display: flex; gap: 16px; line-height: 1.7; }
  .msg-avatar { width: 32px; height: 32px; border-radius: 50%; display: grid; place-items: center; background: var(--user-bubble); }
  .msg.assistant .msg-avatar { background: var(--accent); color: white; }
  .msg-content { flex: 1; overflow-x: auto; }
  .msg.user .msg-content { background: var(--user-bubble); padding: 12px 16px; border-radius: 12px; display: inline-block; max-width: 100%; }
  .markdown-body pre { background: #1E1E1E; color: #FFF; padding: 16px; border-radius: 8px; direction: ltr; text-align: left; overflow-x: auto; margin: 12px 0; }
  
  .chat-input-area { padding: 16px 24px; border-top: 1px solid var(--border); background: var(--bg); }
  .input-wrapper { max-width: 800px; margin: 0 auto; display: flex; gap: 8px; background: var(--surface); border: 1px solid var(--border); border-radius: 24px; padding: 8px 12px; }
  .input-wrapper textarea { flex: 1; border: none; background: transparent; resize: none; outline: none; padding: 8px; max-height: 100px; }
  .input-wrapper button { width: 36px; height: 36px; border-radius: 50%; background: var(--accent); color: white; display: grid; place-items: center; }

  #view-admin { height: 100vh; display: flex; background: var(--admin-bg); color: var(--admin-text); }
  .admin-sidebar { width: 260px; background: var(--admin-surface); border-left: 1px solid var(--admin-border); display: flex; flex-direction: column; }
  .admin-sidebar .brand { padding: 24px; color: white; border-bottom: 1px solid var(--admin-border); }
  .admin-nav { padding: 16px; display: flex; flex-direction: column; gap: 8px; }
  .admin-nav-item { padding: 12px 16px; border-radius: 8px; font-size: 14px; font-weight: 500; display: flex; align-items: center; gap: 12px; color: var(--admin-muted); cursor: pointer; }
  .admin-nav-item:hover, .admin-nav-item.active { background: rgba(59, 130, 246, 0.1); color: var(--admin-primary); }
  .admin-nav-item.danger { color: var(--admin-danger); margin-top: auto; }
  
  .admin-main { flex: 1; display: flex; flex-direction: column; overflow: hidden; }
  .admin-header { padding: 20px 32px; background: var(--admin-surface); border-bottom: 1px solid var(--admin-border); display: flex; justify-content: space-between; align-items: center; }
  .admin-content { padding: 32px; flex: 1; overflow-y: auto; }
  
  .kpi-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(240px, 1fr)); gap: 20px; margin-bottom: 32px; }
  .kpi-card { background: var(--admin-surface); padding: 24px; border-radius: 16px; border: 1px solid var(--admin-border); display: flex; justify-content: space-between; align-items: center; }
  .kpi-info h3 { font-size: 14px; color: var(--admin-muted); font-weight: 500; margin-bottom: 8px; }
  .kpi-value { font-size: 32px; font-weight: 700; color: white; }
  .kpi-icon { width: 48px; height: 48px; border-radius: 12px; display: grid; place-items: center; font-size: 24px; }
  .icon-blue { background: rgba(59, 130, 246, 0.15); color: var(--admin-primary); }
  .icon-green { background: rgba(16, 185, 129, 0.15); color: var(--admin-success); }
  .icon-gold { background: rgba(245, 158, 11, 0.15); color: var(--admin-warning); }

  .table-card { background: var(--admin-surface); border-radius: 16px; border: 1px solid var(--admin-border); overflow: hidden; }
  .table-header { padding: 20px; border-bottom: 1px solid var(--admin-border); display: flex; justify-content: space-between; align-items: center; }
  .table-header input { width: 250px; padding: 10px 16px; background: var(--admin-bg); border: 1px solid var(--admin-border); border-radius: 8px; color: white; }
  table { width: 100%; border-collapse: collapse; text-align: right; }
  th, td { padding: 16px 20px; border-bottom: 1px solid var(--admin-border); font-size: 14px; }
  th { background: rgba(0,0,0,0.2); color: var(--admin-muted); font-weight: 500; }
  
  .status-badge { padding: 6px 12px; border-radius: 20px; font-size: 12px; font-weight: 600; display: inline-block; }
  .status-active { background: rgba(16, 185, 129, 0.15); color: var(--admin-success); }
  .status-pending { background: rgba(245, 158, 11, 0.15); color: var(--admin-warning); }
  .status-inactive { background: rgba(148, 163, 184, 0.15); color: var(--admin-muted); }

  .action-btn { padding: 6px 12px; border-radius: 6px; font-size: 12px; font-weight: 600; margin-left: 4px; }
  .btn-approve { background: var(--admin-success); color: white; }
  .btn-reject { background: var(--admin-danger); color: white; }

  .modal { position: fixed; inset: 0; background: rgba(0,0,0,0.6); display: flex; align-items: center; justify-content: center; z-index: 1000; padding: 20px; backdrop-filter: blur(2px); }
  .modal-content { background: var(--surface); padding: 32px; border-radius: var(--radius); width: 100%; max-width: 400px; box-shadow: var(--shadow); }
  .admin-modal-content { background: var(--admin-surface); border: 1px solid var(--admin-border); padding: 32px; border-radius: 16px; width: 100%; max-width: 500px; color: white; }
  .admin-modal-content input { background: var(--admin-bg); border: 1px solid var(--admin-border); color: white; }
  
  #toast-container { position: fixed; top: 24px; left: 50%; transform: translateX(-50%); z-index: 9999; display: flex; flex-direction: column; gap: 8px; }
  .toast { background: #1E293B; color: white; padding: 12px 24px; border-radius: 8px; font-weight: 500; box-shadow: var(--shadow); }

  @media (max-width: 768px) {
    .sidebar { position: fixed; top: 0; bottom: 0; right: 0; transform: translateX(100%); z-index: 100; transition: 0.3s; }
    .sidebar.open { transform: translateX(0); }
    .admin-sidebar { position: fixed; right: -100%; height: 100%; transition: 0.3s; z-index: 100; }
    .admin-sidebar.open { right: 0; }
    .kpi-grid { grid-template-columns: 1fr; }
    .admin-content { padding: 16px; }
    .table-header input { width: 100%; }
  }
</style>
</head>
<body>

<div id="toast-container"></div>

<!-- 1. شاشة الدخول -->
<div id="view-auth">
  <div class="card">
    <div class="auth-logo">📖</div>
    <div class="auth-title" id="auth-title">مرحباً بك في الباحث</div>
    <form id="auth-form">
      <div class="form-group hidden" id="name-group"><label>الاسم الكامل</label><input type="text" id="auth-name" placeholder="أدخل اسمك"></div>
      <div class="form-group"><label>البريد الإلكتروني</label><input type="email" id="auth-email" required dir="ltr" placeholder="example@mail.com"></div>
      <div class="form-group"><label>كلمة المرور</label><input type="password" id="auth-password" required dir="ltr" placeholder="••••••••"></div>
      <button type="submit" class="btn btn-primary" style="width:100%;" id="auth-submit-btn">دخول</button>
    </form>
    <p style="margin-top: 20px; text-align: center; font-size: 14px; color: var(--muted);">
      <span id="auth-toggle-text">ليس لديك حساب؟</span> 
      <span id="auth-toggle-btn" style="color:var(--accent); cursor:pointer; font-weight:bold;">سجل الآن</span>
    </p>
  </div>
</div>

<!-- 2. واجهة المحادثة -->
<div id="view-chat" class="hidden">
  <div class="chat-layout">
    <aside class="sidebar" id="userSidebar">
      <div class="sidebar-header"><div class="brand"><span style="color:var(--accent);">✽</span> الباحث</div></div>
      <div class="sidebar-menu">
        <button class="btn btn-primary" id="new-chat-btn" style="margin-bottom:8px;">+ محادثة جديدة</button>
        <button class="menu-item" onclick="openPricing()">💳 باقة الاشتراك</button>
        <button class="menu-item" onclick="logout()" style="color:var(--accent);">🚪 خروج</button>
      </div>
      <div class="chat-history" id="chat-history-list"></div>
    </aside>
    <main class="main-chat">
      <header class="chat-header">
        <button class="btn btn-outline" onclick="document.getElementById('userSidebar').classList.toggle('open')" style="display:none; padding:4px 8px;" id="mobile-menu-btn">☰</button>
        <span id="chat-title-display">محادثة جديدة</span>
        <span id="user-status-header" style="font-size:12px;"></span>
      </header>
      <div class="chat-messages" id="chat-messages"></div>
      <div class="chat-input-area">
        <div class="input-wrapper">
          <textarea id="chat-input" rows="1" placeholder="اسأل..."></textarea>
          <button id="send-btn">➤</button>
        </div>
      </div>
    </main>
  </div>
</div>

<!-- 3. لوحة الإدارة -->
<div id="view-admin" class="hidden">
  <aside class="admin-sidebar" id="adminSidebar">
    <div class="brand"><span style="color:var(--admin-primary);">✽</span> مركز القيادة</div>
    <div class="admin-nav">
      <div class="admin-nav-item active" onclick="switchAdminTab('dashboard')">📊 لوحة القيادة</div>
      <div class="admin-nav-item" onclick="switchAdminTab('users')">👥 إدارة المستخدمين</div>
      <div class="admin-nav-item" onclick="openSettings()">⚙️ الإعدادات</div>
      <div class="admin-nav-item danger" onclick="logout()">🚪 تسجيل الخروج</div>
    </div>
  </aside>
  <main class="admin-main">
    <header class="admin-header">
      <h2 id="admin-page-title">نظرة عامة على المنصة</h2>
      <div class="admin-user-profile"><span>مرحباً، <strong id="admin-name-display">المدير</strong></span></div>
    </header>
    <div class="admin-content">
      <div id="admin-tab-dashboard">
        <div class="kpi-grid">
          <div class="kpi-card"><div class="kpi-info"><h3>إجمالي المستخدمين</h3><div class="kpi-value" id="kpi-total">0</div></div><div class="kpi-icon icon-blue">👥</div></div>
          <div class="kpi-card"><div class="kpi-info"><h3>المشتركون النشطون</h3><div class="kpi-value" id="kpi-active">0</div></div><div class="kpi-icon icon-green">✅</div></div>
          <div class="kpi-card"><div class="kpi-info"><h3>إجمالي الإيرادات (ل.س)</h3><div class="kpi-value" id="kpi-revenue">0</div></div><div class="kpi-icon icon-gold">💰</div></div>
        </div>
        <div style="background: var(--admin-surface); padding: 24px; border-radius: 16px; border: 1px solid var(--admin-border);">
          <h3 style="font-size: 16px; margin-bottom: 16px; color: white;">نمو الاشتراكات</h3>
          <canvas id="revenueChart" height="80"></canvas>
        </div>
      </div>
      <div id="admin-tab-users" class="hidden">
        <div class="table-card">
          <div class="table-header">
            <h3 style="color:white; font-size:16px;">سجل المستخدمين والمدفوعات</h3>
            <input type="text" id="admin-search" placeholder="🔍 ابحث بالاسم أو البريد..." oninput="loadAdminTable()">
          </div>
          <div class="table-wrap" style="overflow-x:auto;">
            <table>
              <thead><tr><th>الاسم</th><th>البريد</th><th>رقم التحويل (شام كاش)</th><th>الحالة</th><th>الإجراءات</th></tr></thead>
              <tbody id="admin-table-body"></tbody>
            </table>
          </div>
        </div>
      </div>
    </div>
  </main>
</div>

<!-- نافذة الدفع -->
<div id="payment-modal" class="modal hidden">
  <div class="modal-content" style="text-align:center;">
    <h3 style="font-family:var(--font-serif); margin-bottom:16px;">الدفع عبر شام كاش</h3>
    <div style="background:var(--bg); padding:16px; border-radius:12px; border:1px dashed var(--accent); margin-bottom:16px;">
      <p style="font-size:13px; color:var(--muted); margin-bottom:8px;">حول المبلغ إلى:</p>
      <div style="font-size:24px; font-weight:bold; color:var(--accent); direction:ltr;" id="modal-sham-cash">0912345678</div>
    </div>
    <div class="form-group" style="text-align:right;"><label>رقم عملية التحويل</label><input type="text" id="tx-id-input" placeholder="أدخل رقم العملية" dir="ltr"></div>
    <button class="btn btn-primary" style="width:100%;" id="submit-payment-btn">تأكيد وإرسال</button>
    <button class="btn" style="width:100%; margin-top:8px; border:1px solid var(--border);" onclick="document.getElementById('payment-modal').classList.add('hidden')">إلغاء</button>
  </div>
</div>

<!-- نافذة إعدادات الإدارة -->
<div id="admin-settings-modal" class="modal hidden">
  <div class="admin-modal-content">
    <h3 style="margin-bottom:20px;">⚙️ إعدادات المنصة</h3>
    <div class="form-group"><label>رقم محفظة شام كاش</label><input type="text" id="setting-sham-cash" dir="ltr"></div>
    <div class="form-group"><label>سعر الباقة السنوية (ل.س)</label><input type="number" id="setting-price"></div>
    <div class="form-group"><label>مفتاح Gemini API</label><input type="password" id="setting-api-key" dir="ltr" placeholder="AIzaSy..."></div>
    <div style="display:flex; gap:10px; margin-top:20px;">
      <button class="btn btn-primary" style="flex:1;" onclick="saveAdminSettings()">حفظ التغييرات</button>
      <button class="btn" style="flex:1; border:1px solid var(--admin-border); color:white;" onclick="document.getElementById('admin-settings-modal').classList.add('hidden')">إلغاء</button>
    </div>
  </div>
</div>

<!-- ================= سكربتات Firebase ================= -->
<script src="https://www.gstatic.com/firebasejs/10.8.0/firebase-app-compat.js"></script>
<script src="https://www.gstatic.com/firebasejs/10.8.0/firebase-auth-compat.js"></script>
<script src="https://www.gstatic.com/firebasejs/10.8.0/firebase-firestore-compat.js"></script>

<script>
/* ================= 1. إعدادات Firebase (الخاصة بك) ================= */
const firebaseConfig = {
  apiKey: "AIzaSyDVcPsnEBsH_WNtFvRKagNuGVwd4fPftuY",
  authDomain: "bahia-7976d.firebaseapp.com",
  projectId: "bahia-7976d",
  storageBucket: "bahia-7976d.firebasestorage.app",
  messagingSenderId: "949538123612",
  appId: "1:949538123612:web:959cc1c01b9fc6922c9b99"
};

firebase.initializeApp(firebaseConfig);
const auth = firebase.auth();
const db = firebase.firestore();

/* ================= 2. الحالة العامة ================= */
let currentUser = null;
let currentChatId = null;
let appSettings = { shamCash: '0912345678', price: 1000, apiKey: '' };

/* ================= أدوات مساعدة ================= */
function toast(msg) {
  const t = document.createElement('div'); t.className = 'toast'; t.textContent = msg;
  document.getElementById('toast-container').appendChild(t);
  setTimeout(() => t.remove(), 3000);
}

function navigate(id) {
  document.getElementById('view-auth').classList.add('hidden');
  document.getElementById('view-chat').classList.add('hidden');
  document.getElementById('view-admin').classList.add('hidden');
  document.getElementById('view-' + id).classList.remove('hidden');
}

async function getSettings() {
  try {
    const doc = await db.collection('settings').doc('app').get();
    if (doc.exists) {
      appSettings = doc.data();
    } else {
      await db.collection('settings').doc('app').set(appSettings);
    }
  } catch (e) { console.error("Error fetching settings:", e); }
}

/* ================= 3. المصادقة (Auth) ================= */
let isLogin = true;
document.getElementById('auth-toggle-btn').addEventListener('click', () => {
  isLogin = !isLogin;
  document.getElementById('auth-title').textContent = isLogin ? 'مرحباً بك في الباحث' : 'إنشاء حساب جديد';
  document.getElementById('name-group').classList.toggle('hidden', isLogin);
  document.getElementById('auth-submit-btn').textContent = isLogin ? 'دخول' : 'تسجيل';
  document.getElementById('auth-toggle-text').textContent = isLogin ? 'ليس لديك حساب؟' : 'لديك حساب؟';
  document.getElementById('auth-toggle-btn').textContent = isLogin ? 'سجل الآن' : 'دخول';
});

document.getElementById('auth-form').addEventListener('submit', async (e) => {
  e.preventDefault();
  const email = document.getElementById('auth-email').value;
  const pass = document.getElementById('auth-password').value;
  const name = document.getElementById('auth-name').value;
  const btn = document.getElementById('auth-submit-btn');
  
  btn.disabled = true; btn.classList.add('btn-loading'); btn.textContent = 'جاري المعالجة...';

  try {
    if (isLogin) {
      await auth.signInWithEmailAndPassword(email, pass);
    } else {
      if(!name) throw new Error("أدخل الاسم");
      const userCredential = await auth.createUserWithEmailAndPassword(email, pass);
      await db.collection('users').doc(userCredential.user.uid).set({
        name: name, email: email, role: 'user', status: 'inactive', txId: null
      });
    }
  } catch (error) {
    toast(error.message);
  } finally {
    btn.disabled = false; btn.classList.remove('btn-loading');
    btn.textContent = isLogin ? 'دخول' : 'تسجيل';
  }
});

function logout() {
  auth.signOut().then(() => {
    currentUser = null;
    document.getElementById('auth-form').reset();
  });
}

auth.onAuthStateChanged(async (user) => {
  if (user) {
    const doc = await db.collection('users').doc(user.uid).get();
    if (doc.exists) {
      currentUser = { uid: user.uid, ...doc.data() };
      await getSettings();
      
      if (currentUser.role === 'admin') {
        navigate('admin');
        initAdminDashboard();
      } else {
        navigate('chat');
        loadChats();
        if(currentUser.status !== 'active') {
          document.getElementById('user-status-header').innerHTML = `<span style="color:#EF4444;">بحاجة لتفعيل</span>`;
        } else {
          document.getElementById('user-status-header').innerHTML = `<span style="color:#10B981;">نشط</span>`;
        }
      }
    } else {
      // إذا سجل الدخول لكن لا يوجد له ملف في قاعدة البيانات (مثلاً تم إنشاؤه يدوياً في Auth فقط)
      toast("لم يتم العثور على بيانات المستخدم في قاعدة البيانات.");
    }
  } else {
    navigate('auth');
  }
});

/* ================= 4. المحادثة (للمستخدم) ================= */
async function loadChats() {
  if(!currentUser) return;
  const chatList = document.getElementById('chat-history-list');
  
  const snapshot = await db.collection('chats')
    .where('userId', '==', currentUser.uid)
    .orderBy('updatedAt', 'desc').get();
    
  chatList.innerHTML = '';
  if(snapshot.empty) {
    document.getElementById('chat-messages').innerHTML = `<div style="text-align:center; margin-top:20vh; color:var(--muted);"><h2>كيف يمكنني مساعدتك اليوم؟</h2></div>`;
    document.getElementById('chat-title-display').textContent = 'محادثة جديدة';
    return;
  }

  snapshot.forEach(doc => {
    const chat = doc.data();
    const div = document.createElement('div');
    div.className = `history-item ${doc.id === currentChatId ? 'active' : ''}`;
    div.textContent = chat.title || 'محادثة';
    div.onclick = () => { currentChatId = doc.id; renderChat(chat); };
    chatList.appendChild(div);
  });
  
  if(!currentChatId) {
    const latest = snapshot.docs[0];
    currentChatId = latest.id;
    renderChat(latest.data());
  }
}

function renderChat(chat) {
  document.getElementById('chat-title-display').textContent = chat.title || 'محادثة';
  const msgsContainer = document.getElementById('chat-messages');
  if (!chat.messages || chat.messages.length === 0) {
    msgsContainer.innerHTML = `<div style="text-align:center; margin-top:20vh; color:var(--muted);"><h2>كيف يمكنني مساعدتك اليوم؟</h2></div>`;
  } else {
    msgsContainer.innerHTML = chat.messages.map(m => `
      <div class="msg ${m.role}">
        <div class="msg-avatar">${m.role === 'user' ? 'أ' : '✽'}</div>
        <div class="msg-content ${m.role === 'assistant' ? 'markdown-body' : ''}">${m.role === 'assistant' ? marked.parse(m.text) : m.text}</div>
      </div>
    `).join('');
  }
  msgsContainer.scrollTop = msgsContainer.scrollHeight;
}

document.getElementById('new-chat-btn').addEventListener('click', () => {
  currentChatId = null;
  document.getElementById('chat-messages').innerHTML = `<div style="text-align:center; margin-top:20vh; color:var(--muted);"><h2>كيف يمكنني مساعدتك اليوم؟</h2></div>`;
  document.getElementById('chat-title-display').textContent = 'محادثة جديدة';
  document.getElementById('chat-input').focus();
});

document.getElementById('send-btn').addEventListener('click', async () => {
  const input = document.getElementById('chat-input');
  const text = input.value.trim();
  if (!text) return;
  if (currentUser.status !== 'active' && currentUser.role !== 'admin') return toast('حسابك بحاجة لتفعيل');

  input.value = ''; input.style.height = 'auto';
  const msgsContainer = document.getElementById('chat-messages');
  
  msgsContainer.innerHTML += `<div class="msg user"><div class="msg-avatar">أ</div><div class="msg-content">${text}</div></div>`;
  msgsContainer.innerHTML += `<div class="msg assistant" id="typing"><div class="msg-avatar">✽</div><div class="msg-content" style="color:var(--muted);">جاري التفكير...</div></div>`;
  msgsContainer.scrollTop = msgsContainer.scrollHeight;

  let reply = '';
  if (appSettings.apiKey) {
    try {
      const res = await fetch(`https://generativelanguage.googleapis.com/v1beta/models/gemini-1.5-flash:generateContent?key=${appSettings.apiKey}`, {
        method: 'POST', headers: {'Content-Type': 'application/json'},
        body: JSON.stringify({ contents: [{ parts: [{ text }] }] })
      });
      const json = await res.json();
      reply = json.candidates[0].content.parts[0].text;
    } catch (e) { reply = 'حدث خطأ في الاتصال بالذكاء الاصطناعي.'; }
  } else { reply = 'يرجى تفعيل مفتاح API من لوحة الإدارة.'; }

  document.getElementById('typing').remove();
  msgsContainer.innerHTML += `<div class="msg assistant"><div class="msg-avatar">✽</div><div class="msg-content markdown-body">${marked.parse(reply)}</div></div>`;
  msgsContainer.scrollTop = msgsContainer.scrollHeight;

  const chatData = {
    userId: currentUser.uid,
    title: text.substring(0, 25),
    updatedAt: firebase.firestore.FieldValue.serverTimestamp(),
  };

  if (!currentChatId) {
    chatData.messages = [{ role: 'user', text }, { role: 'assistant', text: reply }];
    const docRef = await db.collection('chats').add(chatData);
    currentChatId = docRef.id;
  } else {
    await db.collection('chats').doc(currentChatId).update({
      messages: firebase.firestore.FieldValue.arrayUnion({ role: 'user', text }, { role: 'assistant', text: reply }),
      updatedAt: firebase.firestore.FieldValue.serverTimestamp()
    });
  }
  loadChats();
});

/* ================= 5. لوحة الإدارة ================= */
async function initAdminDashboard() {
  document.getElementById('admin-name-display').textContent = currentUser.name;
  switchAdminTab('dashboard');
  await getSettings();
}

function switchAdminTab(tab) {
  document.querySelectorAll('.admin-nav-item').forEach(el => el.classList.remove('active'));
  document.querySelectorAll('.admin-nav-item').forEach(el => {
    if(el.textContent.includes('القيادة') && tab === 'dashboard') el.classList.add('active');
    if(el.textContent.includes('المستخدمين') && tab === 'users') el.classList.add('active');
  });

  document.getElementById('admin-tab-dashboard').classList.add('hidden');
  document.getElementById('admin-tab-users').classList.add('hidden');

  if (tab === 'dashboard') {
    document.getElementById('admin-tab-dashboard').classList.remove('hidden');
    document.getElementById('admin-page-title').textContent = 'نظرة عامة على المنصة';
    loadDashboardStats();
  } else {
    document.getElementById('admin-tab-users').classList.remove('hidden');
    document.getElementById('admin-page-title').textContent = 'إدارة المستخدمين';
    loadAdminTable();
  }
}

let revenueChartInstance = null;
async function loadDashboardStats() {
  const snapshot = await db.collection('users').where('role', '==', 'user').get();
  let activeCount = 0, totalRevenue = 0;

  snapshot.forEach(doc => {
    if (doc.data().status === 'active') { activeCount++; totalRevenue += Number(appSettings.price); }
  });

  document.getElementById('kpi-total').textContent = snapshot.size;
  document.getElementById('kpi-active').textContent = activeCount;
  document.getElementById('kpi-revenue').textContent = totalRevenue.toLocaleString();

  if (revenueChartInstance) revenueChartInstance.destroy();
  const ctx = document.getElementById('revenueChart').getContext('2d');
  Chart.defaults.color = '#94A3B8';
  Chart.defaults.font.family = "'IBM Plex Sans Arabic', sans-serif";

  revenueChartInstance = new Chart(ctx, {
    type: 'line',
    data: {
      labels: ['الشهر 1', 'الشهر 2', 'الشهر 3', 'الآن'],
      datasets: [{
        label: 'عدد المشتركين', data: [0, 1, Math.floor(activeCount/2), activeCount],
        borderColor: '#3B82F6', backgroundColor: 'rgba(59, 130, 246, 0.1)', borderWidth: 3, tension: 0.4, fill: true
      }]
    },
    options: { responsive: true, plugins: { legend: { display: false } }, scales: { y: { beginAtZero: true, grid: { color: '#334155' } }, x: { grid: { display: false } } } }
  });
}

async function loadAdminTable() {
  const search = document.getElementById('admin-search').value.toLowerCase();
  const tbody = document.getElementById('admin-table-body');
  const snapshot = await db.collection('users').where('role', '==', 'user').get();
  
  let html = '';
  snapshot.forEach(doc => {
    const u = { id: doc.id, ...doc.data() };
    if(search && !u.name.toLowerCase().includes(search) && !u.email.toLowerCase().includes(search)) return;

    let badge = u.status === 'active' ? '<span class="status-badge status-active">نشط</span>' : 
                u.status === 'pending' ? '<span class="status-badge status-pending">بانتظار التفعيل</span>' : 
                '<span class="status-badge status-inactive">غير مشترك</span>';

    let actions = u.status === 'pending' ? `
      <button class="action-btn btn-approve" onclick="approveUser('${u.id}')">✔ تفعيل</button>
      <button class="action-btn btn-reject" onclick="rejectUser('${u.id}')">✖ رفض</button>
    ` : `<button class="action-btn btn-delete" style="border:1px solid #475569; color:#94A3B8;" onclick="deleteUser('${u.id}')">🗑 حذف</button>`;

    html += `<tr><td><strong>${u.name}</strong></td><td dir="ltr" style="text-align:right;">${u.email}</td><td dir="ltr" style="text-align:right; color:var(--admin-primary);">${u.txId || '-'}</td><td>${badge}</td><td>${actions}</td></tr>`;
  });
  tbody.innerHTML = html || '<tr><td colspan="5" style="text-align:center;">لا يوجد نتائج</td></tr>';
}

window.approveUser = async (id) => {
  await db.collection('users').doc(id).update({ status: 'active' });
  toast('تم تفعيل الحساب'); loadAdminTable(); loadDashboardStats();
};

window.rejectUser = async (id) => {
  await db.collection('users').doc(id).update({ status: 'inactive', txId: null });
  toast('تم رفض الطلب'); loadAdminTable();
};

window.deleteUser = async (id) => {
  if(!confirm('هل أنت متأكد من الحذف؟')) return;
  await db.collection('users').doc(id).delete();
  toast('تم الحذف'); loadAdminTable();
};

/* ================= 6. الإعدادات والدفع ================= */
function openSettings() {
  document.getElementById('setting-sham-cash').value = appSettings.shamCash;
  document.getElementById('setting-price').value = appSettings.price;
  document.getElementById('setting-api-key').value = appSettings.apiKey || '';
  document.getElementById('admin-settings-modal').classList.remove('hidden');
}

async function saveAdminSettings() {
  appSettings.shamCash = document.getElementById('setting-sham-cash').value;
  appSettings.price = document.getElementById('setting-price').value;
  appSettings.apiKey = document.getElementById('setting-api-key').value;
  await db.collection('settings').doc('app').set(appSettings);
  document.getElementById('admin-settings-modal').classList.add('hidden');
  toast('تم حفظ الإعدادات'); loadDashboardStats();
}

function openPricing() {
  document.getElementById('modal-sham-cash').textContent = appSettings.shamCash;
  document.getElementById('payment-modal').classList.remove('hidden');
}

document.getElementById('submit-payment-btn').addEventListener('click', async () => {
  const txId = document.getElementById('tx-id-input').value.trim();
  if (!txId) return toast('أدخل رقم العملية');
  await db.collection('users').doc(currentUser.uid).update({ status: 'pending', txId: txId });
  currentUser.status = 'pending';
  document.getElementById('payment-modal').classList.add('hidden');
  document.getElementById('user-status-header').innerHTML = `<span style="color:#F59E0B;">قيد المراجعة</span>`;
  toast('تم إرسال الطلب للإدارة');
});

if(window.innerWidth <= 768) {
  document.getElementById('mobile-menu-btn').style.display = 'block';
}
</script>
</body>
</html>
