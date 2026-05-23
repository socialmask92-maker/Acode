<!DOCTYPE html>
<html lang="th">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>KT Group Employee Portal</title>
    <style>
        /* --- ตั้งค่าเริ่มต้นและการไล่เฉดสีพื้นหลัง --- */
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
        }
        body {
            background: radial-gradient(circle at 50% 55%, #2cb64f 0%, #032b0f 85%);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: flex-start;
            padding: 20px 16px;
            color: white;
            overflow-x: hidden;
        }

        /* --- โครงสร้างหลักควบคุมความกว้างแอป --- */
        .app-container {
            width: 100%;
            max-width: 410px;
            margin: 0 auto;
            text-align: center;
        }

        /* --- ส่วนหัวโลโก้บริษัท --- */
        .header-brand {
            margin-bottom: 25px;
            margin-top: 10px;
        }
        .logo-circle {
            width: 110px;
            height: 110px;
            border: 1px solid rgba(255, 255, 255, 0.4);
            border-radius: 50%;
            display: flex;
            justify-content: center;
            align-items: center;
            margin: 0 auto 15px;
            font-size: 52px;
            font-family: 'Times New Roman', serif;
            font-weight: 300;
            background: rgba(255, 255, 255, 0.03);
        }
        .brand-name { font-size: 19px; font-weight: bold; letter-spacing: 3px; margin-bottom: 4px; }
        .brand-sub { font-size: 15px; letter-spacing: 6px; opacity: 0.8; }

        /* --- สไตล์กล่องกระจกฝ้า (Glassmorphism Card) --- */
        .glass-card {
            background: rgba(255, 255, 255, 0.12);
            backdrop-filter: blur(15px);
            -webkit-backdrop-filter: blur(15px);
            border: 1px solid rgba(255, 255, 255, 0.2);
            border-radius: 28px;
            padding: 30px 20px;
            width: 100%;
            text-align: left;
            box-shadow: 0 8px 32px 0 rgba(0, 0, 0, 0.3);
            margin-bottom: 20px;
        }
        .glass-card h2 {
            font-size: 24px;
            font-weight: 500;
            text-align: center;
            margin-bottom: 5px;
        }
        .card-subtitle {
            font-size: 13px;
            color: rgba(255, 255, 255, 0.7);
            text-align: center;
            margin-bottom: 25px;
        }

        /* --- การซ่อน/แสดงหน้าจอ --- */
        .page {
            display: none;
            width: 100%;
        }
        .page.active {
            display: block;
        }

        /* --- สไตล์ฟอร์มและปุ่มกด --- */
        .input-box {
            width: 100%;
            padding: 16px 20px;
            background: rgba(255, 255, 255, 0.08);
            border: 1px solid rgba(255, 255, 255, 0.15);
            border-radius: 25px;
            color: white;
            font-size: 16px;
            outline: none;
            margin-bottom: 16px;
            text-align: center;
        }
        .input-box::placeholder { color: rgba(255, 255, 255, 0.5); }
        
        .btn-primary {
            width: 100%;
            padding: 15px;
            background: white;
            border: none;
            border-radius: 25px;
            color: black;
            font-size: 17px;
            font-weight: bold;
            cursor: pointer;
            margin-top: 10px;
            transition: 0.2s;
            text-align: center;
        }
        .btn-primary:active { background: #e0e0e0; }

        .btn-back {
            background: rgba(255, 255, 255, 0.15);
            color: white;
            border: 1px solid rgba(255, 255, 255, 0.2);
            padding: 10px 20px;
            border-radius: 20px;
            cursor: pointer;
            font-size: 14px;
            margin-top: 15px;
            display: inline-block;
            text-decoration: none;
            text-align: center;
        }

        /* --- สไตล์หน้าแดชบอร์ดหลัก (Menu List) --- */
        .menu-list {
            display: flex;
            flex-direction: column;
            gap: 14px;
        }
        .menu-item {
            background: rgba(255, 255, 255, 0.08);
            border: 1px solid rgba(255, 255, 255, 0.15);
            padding: 16px 20px;
            border-radius: 20px;
            display: flex;
            align-items: center;
            color: white;
            text-decoration: none;
            font-size: 16px;
            cursor: pointer;
            transition: 0.2s;
        }
        .menu-item:active { background: rgba(255, 255, 255, 0.2); }
        .menu-icon { margin-right: 15px; font-size: 20px; }

        /* --- สไตล์ภายในแต่ละเมนู --- */
        .profile-section {
            text-align: center;
            margin-bottom: 20px;
        }
        .profile-avatar {
            width: 90px; height: 90px;
            background: rgba(255, 255, 255, 0.2);
            border-radius: 50%;
            margin: 0 auto 15px;
            display: flex; justify-content: center; align-items: center;
            font-size: 40px; border: 2px solid rgba(255, 255, 255, 0.5);
        }
        .info-grid {
            display: flex; flex-direction: column; gap: 12px;
            background: rgba(255, 255, 255, 0.05); padding: 15px; border-radius: 15px;
        }
        .info-row { display: flex; justify-content: space-between; border-bottom: 1px solid rgba(255, 255, 255, 0.1); padding-bottom: 8px; }
        .info-row:last-child { border: none; padding-bottom: 0; }
        .info-label { color: rgba(255, 255, 255, 0.7); font-size: 14px; }
        .info-value { font-weight: 500; font-size: 14px; }

        /* บันทึกเวลา */
        .time-badge { text-align: center; margin: 20px 0; }
        .gps-status { font-size: 12px; color: #a3ffb4; text-align: center; margin-top: 8px; }
        .stat-box { background: rgba(255, 255, 255, 0.05); padding: 15px; border-radius: 15px; margin: 15px 0; text-align: center; }
        
        /* สถานะการเบิก */
        .status-tag { padding: 4px 10px; border-radius: 12px; font-size: 12px; font-weight: bold; }
        .status-success { background: #2cd64f; color: black; }
        .status-warning { background: #ffcc00; color: black; }
        .status-danger { background: #ff4d4d; color: white; }
    </style>
</head>
<body>

    <div class="app-container">
        
        <div class="header-brand">
            <div class="logo-circle">KT</div>
            <div class="brand-name">GROUP COMPANY</div>
            <div class="brand-sub">LIMITED</div>
        </div>

        <div id="page-login" class="page active">
            <div class="glass-card">
                <h2>เข้าสู่ระบบ</h2>
                <div class="card-subtitle">(ชื่อผู้ใช้: admin / รหัสผ่าน: 1234)</div>
                
                <input type="text" id="username" class="input-box" placeholder="ชื่อผู้ใช้">
                <input type="password" id="password" class="input-box" placeholder="รหัสผ่าน">
                
                <button class="btn-primary" onclick="handleOfflineLogin()">ล็อกอิน</button>
            </div>
        </div>

        <div id="page-dashboard" class="page">
            <div class="glass-card">
                <h2 style="margin-bottom: 5px;">KT Group Portal</h2>
                <div class="card-subtitle" id="welcome-text">ยินดีต้อนรับคุณพนักงาน</div>
                
                <div class="menu-list">
                    <div class="menu-item" onclick="navigateTo('page-profile')">
                        <span class="menu-icon">👤</span> ประวัติส่วนตัว (Profile)
                    </div>
                    <div class="menu-item" onclick="navigateTo('page-time')">
                        <span class="menu-icon">🕒</span> บันทึกเวลาและระบบลา (Time & Leave)
                    </div>
                    <div class="menu-item" onclick="navigateTo('page-payroll')">
                        <span class="menu-icon">💵</span> การเงินและเงินเดือน (Payroll)
                    </div>
                    <div class="menu-item" onclick="navigateTo('page-claims')">
                        <span class="menu-icon">📄</span> ระบบเบิกสวัสดิการ (Claims)
                    </div>
                </div>
                
                <center><button class="btn-back" onclick="navigateTo('page-login')">ออกจากระบบ</button></center>
            </div>
        </div>

        <div id="page-profile" class="page">
            <div class="glass-card">
                <h2>ข้อมูลประวัติส่วนตัว</h2>
                <div class="card-subtitle">ข้อมูลพนักงานภายในบริษัท</div>
                
                <div class="profile-section">
                    <div class="profile-avatar">👨‍💼</div>
                    <h3 id="prof-name">สมชาย ใจดี</h3>
                    <p id="prof-role" style="font-size: 13px; opacity: 0.7; margin-top: 4px;">พนักงานอาวุโส</p>
                </div>
                
                <div class="info-grid">
                    <div class="info-row"><span class="info-label">แผนก:</span><span class="info-value">ฝ่ายปฏิบัติการ</span></div>
                    <div class="info-row"><span class="info-label">รหัสพนักงาน:</span><span class="info-value">KT0042</span></div>
                    <div class="info-row"><span class="info-label">อายุการทำงาน:</span><span class="info-value">5 ปี 8 เดือน</span></div>
                    <div class="info-row"><span class="info-label">อีเมล:</span><span class="info-value">somchaimt@gmail.com</span></div>
                </div>
                
                <center><button class="btn-back" onclick="navigateTo('page-dashboard')">⬅️ กลับหน้าหลัก</button></center>
            </div>
        </div>

        <div id="page-time" class="page">
            <div class="glass-card">
                <h2>บันทึกเวลาและระบบลา</h2>
                <div class="card-subtitle">จัดการเวลาเข้างานและวันลา</div>
                <div class="stat-box">
                    <p style="font-size: 15px; margin-bottom: 10px;">📍 ลงเวลาทำงาน (Check-In)</p>
                    <button class="btn-primary" style="background: #2cd64f; color: white;" onclick="alert('บันทึกเวลาสำเร็จ!')">Check-In</button>
                    <div class="gps-status">🟢 GPS Active: Site A</div>
                </div>
                <center><button class="btn-back" onclick="navigateTo('page-dashboard')">⬅️ กลับหน้าหลัก</button></center>
            </div>
        </div>

        <div id="page-payroll" class="page">
            <div class="glass-card">
                <h2>การเงินและเงินเดือน</h2>
                <div class="card-subtitle">ตรวจสอบสลิปเงินเดือนประจำเดือน</div>
                <div class="stat-box">
                    <p style="margin-bottom: 15px;">📅 ประจำเดือน: มิถุนายน 2569</p>
                    <button class="btn-primary" onclick="alert('กำลังดาวน์โหลดไฟล์ PDF สลิปเงินเดือน...')">ดาวน์โหลด PDF</button>
                </div>
                <center><button class="btn-back" onclick="navigateTo('page-dashboard')">⬅️ กลับหน้าหลัก</button></center>
            </div>
        </div>

        <div id="page-claims" class="page">
            <div class="glass-card">
                <h2>ระบบเบิกสวัสดิการ</h2>
                <div class="card-subtitle">ติดตามสถานะการเคลมค่าใช้จ่าย</div>
                <div class="info-grid">
                    <div class="info-row"><span class="info-label">ค่าเดินทาง (Medical):</span><span class="status-tag status-success">อนุมัติแล้ว</span></div>
                    <div class="info-row"><span class="info-label">ค่ารักษาพยาบาล:</span><span class="status-tag status-warning">รอตรวจสอบ</span></div>
                </div>
                <center><button class="btn-back" onclick="navigateTo('page-dashboard')">⬅️ กลับหน้าหลัก</button></center>
            </div>
        </div>

    </div>

    <script>
        function navigateTo(pageId) {
            const pages = document.querySelectorAll('.page');
            pages.forEach(page => page.classList.remove('active'));
            document.getElementById(pageId).classList.add('active');
            window.scrollTo(0, 0);
        }

        // ฟังก์ชันล็อกอินจำลองแบบไม่ต้องง้อลิงก์หลังบ้าน
        function handleOfflineLogin() {
            const user = document.getElementById('username').value.trim();
            const pass = document.getElementById('password').value.trim();

            // 🔐 คุณสามารถทดสอบพิมพ์ตรงนี้ในหน้าแอปได้เลยครับ
            if (user === "admin" && pass === "1234") {
                document.getElementById('welcome-text').innerText = "ยินดีต้อนรับคุณ: สมชาย ใจดี";
                navigateTo('page-dashboard'); // ล็อกอินผ่าน สลับไปหน้าเมนูหลักทันที
            } else {
                alert("ชื่อผู้ใช้หรือรหัสผ่านไม่ถูกต้อง! (กรุณาใช้ ชื่อผู้ใช้: admin / รหัสผ่าน: 1234)");
            }
        }
    </script>
</body>
</html>
