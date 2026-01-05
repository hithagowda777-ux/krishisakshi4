<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Krishi Sakshi – 2026 Edition</title>
  <style>
    :root { --green: #2e7d32; --green-soft: #e8f5e9; --yellow: #f9a825; --accent: #00bcd4; --radius: 16px; --shadow: 0 10px 30px rgba(0,0,0,0.1); }
    * { box-sizing: border-box; margin: 0; padding: 0; font-family: system-ui, sans-serif; }
    body { background: linear-gradient(135deg, #f1f8e9, #e0f2f1); min-height: 100vh; color: #333; }
    
    .container { max-width: 800px; margin: 0 auto; padding: 1rem; }
    .page { display: none; padding: 1rem; animation: fadeIn 0.4s ease-out; }
    .page.active { display: block; }
    @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
    
    .logo { text-align: center; margin-bottom: 2rem; margin-top: 2rem; }
    .logo-icon { width: 70px; height: 70px; background: radial-gradient(circle, #ffeb3b, #4caf50); border-radius: 50%; display: flex; align-items: center; justify-content: center; font-size: 1.8rem; margin: 0 auto 1rem; box-shadow: 0 10px 20px rgba(76,175,80,0.3); }
    
    h1 { font-size: 1.6rem; text-align: center; margin-bottom: 1rem; color: var(--green); }
    input, select { width: 100%; padding: 1rem; border: 2px solid #e0e0e0; border-radius: 12px; font-size: 1rem; margin-bottom: 1rem; background: white; }
    input:focus { outline: none; border-color: var(--green); }

    .btn { width: 100%; padding: 1rem; background: linear-gradient(135deg, var(--green), #43a047); color: white; border: none; border-radius: 12px; font-size: 1.1rem; font-weight: 600; cursor: pointer; margin: 0.5rem 0; }
    .btn-secondary { background: #fff; color: #666; border: 2px solid #e0e0e0; width: auto; padding: 0.6rem 1.2rem; cursor: pointer; border-radius: 8px; }
    
    .form-row { display: grid; grid-template-columns: 1fr 1fr; gap: 1rem; }
    label { font-weight: 600; color: var(--green); margin-bottom: 0.4rem; display: block; font-size: 0.9rem; }
    
    /* CROP GRID FIX */
    .crop-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(140px, 1fr)); gap: 0.8rem; margin: 1rem 0; }
    .crop-item { 
        padding: 1rem; 
        border: 2px solid #e0e0e0; 
        border-radius: 12px; 
        text-align: center; 
        cursor: pointer; 
        background: white; 
        transition: 0.2s; 
        user-select: none;
    }
    /* Style when the hidden checkbox is checked */
    .crop-item:has(input:checked) {
        border-color: var(--green);
        background: var(--green-soft);
        color: var(--green);
        font-weight: bold;
        box-shadow: 0 4px 12px rgba(46,125,50,0.1);
    }
    .crop-item input { display: none; }
    
    .farm-card { background: white; border-radius: var(--radius); padding: 1.2rem; margin-bottom: 1rem; box-shadow: var(--shadow); border-left: 5px solid var(--green); }
    .scheme-badge { display: inline-block; padding: 0.3rem 0.7rem; background: var(--green-soft); color: var(--green); border-radius: 20px; font-size: 0.8rem; font-weight: 600; margin: 0.2rem; border: 1px solid var(--green); }
    
    .chat-window { background: #f9f9f9; border-radius: 12px; padding: 1rem; height: 250px; overflow-y: auto; border: 1px solid #ddd; margin-bottom: 0.5rem; }
    .chat-msg { margin-bottom: 0.8rem; padding: 0.7rem; border-radius: 10px; max-width: 85%; font-size: 0.9rem; line-height: 1.4; }
    .chat-msg.bot { background: white; border: 1px solid #eee; }
    .chat-msg.user { background: var(--accent); color: white; margin-left: auto; }
    
    @media (max-width: 600px) { .form-row { grid-template-columns: 1fr; } }
  </style>
</head>
<body>

<div class="container">
  <div id="loginPage" class="page active">
    <div class="logo">
      <div class="logo-icon">🌾</div>
      <div class="logo-text" style="font-weight: bold; color: var(--green); font-size: 1.4rem;">Krishi Sakshi</div>
    </div>
    <h1>ನಮಸ್ಕಾರ 👋 Farmer Login</h1>
    <input type="tel" id="aadhar" placeholder="Aadhaar (12 digits)" maxlength="12">
    <input type="tel" id="mobile" placeholder="Mobile Number">
    <button class="btn" onclick="verifyLogin()">Verify & Start →</button>
  </div>

  <div id="profilePage" class="page">
    <button class="btn-secondary" onclick="showPage('loginPage')">← Back</button>
    <h1 style="margin-top: 1rem;">Farm Profile</h1>
    
    <div class="form-row">
      <div><label>Full Name</label><input type="text" id="name" placeholder="Enter Name"></div>
      <div><label>Village</label><input type="text" id="village" placeholder="Enter Village"></div>
    </div>

    <div class="form-row">
      <div>
        <label>State</label>
        <select id="state" onchange="loadDistricts()">
          <option value="Karnataka">Karnataka</option>
          <option value="Haryana">Haryana</option>
        </select>
      </div>
      <div>
        <label>District</label>
        <select id="district"></select>
      </div>
    </div>

    <div class="form-row">
      <div><label>Land (Acres)</label><input type="number" id="land" placeholder="e.g. 2.5"></div>
      <div><label>Total Crops This Year</label><input type="number" id="totalCrops" placeholder="e.g. 2"></div>
    </div>

    <label>Main Crops (Select All That Apply)</label>
    <div class="crop-grid" id="cropGrid">
      <label class="crop-item"><input type="checkbox" value="Tur"> Tur/Arhar</label>
      <label class="crop-item"><input type="checkbox" value="Moong"> Moong</label>
      <label class="crop-item"><input type="checkbox" value="Wheat"> Wheat</label>
      <label class="crop-item"><input type="checkbox" value="Maize"> Maize</label>
      <label class="crop-item"><input type="checkbox" value="Rice"> Rice/Paddy</label>
    </div>

    <button class="btn" onclick="generateSolutions()">Get AI Solutions →</button>
  </div>

  <div id="solutionsPage" class="page">
    <button class="btn-secondary" onclick="showPage('profilePage')">← Edit Profile</button>
    
    <div class="farm-card">
      <h3 style="color: var(--green); margin-bottom: 0.5rem;">✅ Profile Summary</h3>
      <div id="farmSummary"></div>
    </div>

    <div class="farm-card">
      <h3 style="color: var(--green); margin-bottom: 0.5rem;">💰 Eligible Schemes</h3>
      <div id="schemes"></div>
    </div>

    <div class="farm-card">
      <h3 style="color: var(--green); margin-bottom: 0.5rem;">🌤️ Weather & MSP</h3>
      <div id="weather"></div>
      <hr style="margin: 0.8rem 0; border: 0; border-top: 1px solid #eee;">
      <div id="cropsMspResult"></div>
    </div>

    <div class="farm-card">
      <h3 style="color: var(--green); margin-bottom: 0.5rem;">💬 Talk to Sakshi</h3>
      <div class="chat-window" id="chatWindow">
        <div class="chat-msg bot">Hello! Ask me about PM-Kisan, MSP prices, or Weather.</div>
      </div>
      <div style="display: flex; gap: 0.5rem;">
        <input type="text" id="chatInput" placeholder="Type here..." style="margin-bottom: 0;">
        <button class="btn" onclick="sendChat()" style="width: auto; padding: 0 1.5rem; margin: 0;">Send</button>
      </div>
    </div>
  </div>
</div>

<script>
  let farmer = {};
  
  const DISTRICTS = {
    Karnataka: ['Bagalkot', 'Ballari', 'Belagavi', 'Bengaluru Rural', 'Bengaluru Urban', 'Bidar', 'Chamarajanagar', 'Chikkaballapura', 'Chikkamagaluru', 'Chitradurga', 'Dakshina Kannada', 'Davanagere', 'Dharwad', 'Gadag', 'Hassan', 'Haveri', 'Kalaburagi', 'Kodagu', 'Kolar', 'Koppal', 'Mandya', 'Mysuru', 'Raichur', 'Ramanagara', 'Shivamogga', 'Tumakuru', 'Udupi', 'Uttara Kannada', 'Vijayanagara', 'Vijayapura', 'Yadgir'],
    Haryana: ['Ambala', 'Bhiwani', 'Charkhi Dadri', 'Faridabad', 'Fatehabad', 'Gurugram', 'Hisar', 'Jhajjar', 'Jind', 'Karnal', 'Kurukshetra', 'Mahendragarh', 'Narnaul', 'Nuh', 'Palwal', 'Panchkula', 'Panipat', 'Rewari', 'Rohtak', 'Sirsa', 'Sonipat', 'Yamunanagar']
  };

  const REAL_DATA = {
    pmKisan: { installment: 21, date: 'Nov 19, 2025', amount: '₹18,000cr' },
    msp: { Tur: '₹7,400', Moong: '₹8,682', Wheat: '₹2,275', Rice: '₹2,183', Maize: '₹2,090' },
    weather: { temp: 28, condition: 'Sunny ☀️', alert: 'Procurement centers active' }
  };

  function showPage(id) {
    document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
    document.getElementById(id).classList.add('active');
    window.scrollTo(0,0);
  }

  function loadDistricts() {
    const state = document.getElementById('state').value;
    const districtSelect = document.getElementById('district');
    districtSelect.innerHTML = '';
    DISTRICTS[state].forEach(d => {
      let opt = document.createElement('option');
      opt.value = d; opt.textContent = d;
      districtSelect.appendChild(opt);
    });
  }

  function verifyLogin() {
    const aadhar = document.getElementById('aadhar').value;
    if (aadhar.length === 12) {
      farmer.aadhar = aadhar;
      loadDistricts();
      showPage('profilePage');
    } else {
      alert('Please enter a 12-digit Aadhaar number');
    }
  }

  function generateSolutions() {
    farmer.name = document.getElementById('name').value;
    farmer.village = document.getElementById('village').value;
    farmer.state = document.getElementById('state').value;
    farmer.district = document.getElementById('district').value;
    farmer.land = document.getElementById('land').value || 0;
    
    // FETCHING SELECTED CROPS
    const selectedCrops = [];
    document.querySelectorAll('#cropGrid input:checked').forEach(cb => {
      selectedCrops.push(cb.value);
    });
    farmer.crops = selectedCrops;

    if (!farmer.name || farmer.crops.length === 0) {
      alert('Please enter your Name and select at least one Crop');
      return;
    }

    document.getElementById('farmSummary').innerHTML = `
      <strong>${farmer.name}</strong> from ${farmer.village}<br>
      ${farmer.district}, ${farmer.state} | ${farmer.land} Acres
    `;

    const schemeList = ['PM-Kisan DBT Ready', 'KCC Loan Eligibility', 'PM Fasal Bima Yojana'];
    if (parseFloat(farmer.land) <= 5) schemeList.push('Small Farmer Bonus');
    document.getElementById('schemes').innerHTML = schemeList.map(s => `<span class="scheme-badge">${s}</span>`).join('');

    document.getElementById('weather').innerHTML = `
      Current Weather: ${REAL_DATA.weather.temp}°C, ${REAL_DATA.weather.condition}<br>
      <small>Status: ${REAL_DATA.weather.alert}</small>
    `;

    document.getElementById('cropsMspResult').innerHTML = farmer.crops.map(c => `
      <div style="margin-bottom:0.5rem; display: flex; justify-content: space-between;">
        <span><strong>${c}</strong></span>
        <span>MSP: ${REAL_DATA.msp[c] || 'N/A'}/qtl</span>
      </div>
    `).join('');

    showPage('solutionsPage');
  }

  function sendChat() {
    const input = document.getElementById('chatInput');
    const chat = document.getElementById('chatWindow');
    const val = input.value.trim().toLowerCase();
    if (!val) return;

    chat.innerHTML += `<div class="chat-msg user">${input.value}</div>`;
    
    let reply = "I'm Sakshi. Ask about PM-Kisan, MSP, or Weather.";
    if (val.includes('pm') || val.includes('kisan')) reply = `The 21st installment was on ${REAL_DATA.pmKisan.date}.`;
    if (val.includes('msp') || val.includes('price')) reply = `Tur MSP is ${REAL_DATA.msp.Tur}. Check local mandis.`;
    if (val.includes('weather')) reply = `The weather in ${farmer.district} is ${REAL_DATA.weather.condition}.`;

    setTimeout(() => {
      chat.innerHTML += `<div class="chat-msg bot"><strong>Sakshi:</strong><br>${reply}</div>`;
      chat.scrollTop = chat.scrollHeight;
    }, 600);
    input.value = '';
  }

  // Initial Load
  loadDistricts();
</script>

</body>
</html>
