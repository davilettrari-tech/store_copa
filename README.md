[index.html](https://github.com/user-attachments/files/28101548/index.html)
<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>Store Copa — Figurinhas & Álbuns Oficiais Copa 2026</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Bebas+Neue&family=Archivo:wght@400;500;600;700;800;900&family=Archivo+Narrow:wght@500;600&family=JetBrains+Mono:wght@400;600&display=swap" rel="stylesheet">
<script src="https://cdn.jsdelivr.net/npm/@supabase/supabase-js@2"></script>

<style>
  :root{
    --bg: #0a1f3d; --bg-2: #06152b;
    --green: #00b86b; --green-deep: #008f53;
    --gold: #ffcc29; --gold-deep: #e8a90c;
    --cream: #f4ecd8; --red: #e23636;
    --ink: #0b1426; --muted: #6b7a99;
    --line: rgba(255,255,255,0.12);
  }
  *{ box-sizing: border-box; margin:0; padding:0; }
  html{ scroll-behavior: smooth; }
  body{ font-family: 'Archivo', sans-serif; background: var(--cream); color: var(--ink); -webkit-font-smoothing: antialiased; overflow-x: hidden; }
  body.modal-open, body.locked{ overflow: hidden; }
  a{ color: inherit; text-decoration: none; }
  img{ max-width: 100%; display: block; }

  /* ============= LOGIN GATE (tela cheia) ============= */
  .gate{
    position: fixed; inset: 0;
    background:
      radial-gradient(circle at 20% 30%, rgba(0,184,107,0.25), transparent 50%),
      radial-gradient(circle at 80% 70%, rgba(255,204,41,0.2), transparent 50%),
      linear-gradient(180deg, var(--bg) 0%, var(--bg-2) 100%);
    z-index: 10000;
    display: flex; align-items: center; justify-content: center;
    padding: 20px;
    overflow-y: auto;
  }
  .gate::before{
    content:""; position:absolute; inset:0;
    background-image: radial-gradient(rgba(255,255,255,0.06) 1px, transparent 1px);
    background-size: 22px 22px;
    pointer-events:none;
  }
  .gate.hide{ display: none; }
  .gate-card{
    background: #fff;
    border-radius: 14px;
    width: 100%; max-width: 440px;
    overflow: hidden;
    box-shadow: 0 30px 80px rgba(0,0,0,0.5);
    position: relative;
    z-index: 1;
  }
  .gate-head{
    background: var(--bg);
    color: #fff;
    padding: 36px 36px 28px;
    text-align: center;
    border-bottom: 4px solid var(--gold);
  }
  .gate-logo{
    display: inline-flex; align-items: center; gap: 12px;
    font-family: 'Bebas Neue', sans-serif;
    font-size: 32px;
    letter-spacing: 3px;
    margin-bottom: 8px;
  }
  .gate-logo-mark{
    width: 44px; height: 44px; border-radius: 50%;
    background: var(--gold); color: var(--bg);
    display: grid; place-items: center;
    font-size: 22px;
    box-shadow: 0 4px 0 var(--gold-deep);
  }
  .gate-logo .accent{ color: var(--gold); }
  .gate-tag{
    font-size: 11px; letter-spacing: 3px; text-transform: uppercase;
    color: rgba(255,255,255,0.6);
  }
  .gate-body{ padding: 32px 36px 36px; }
  .gate-title{
    font-family: 'Bebas Neue', sans-serif;
    font-size: 28px;
    letter-spacing: 1px;
    color: var(--bg);
    margin-bottom: 6px;
  }
  .gate-sub{
    font-size: 14px;
    color: var(--muted);
    margin-bottom: 24px;
  }
  .tabs{ display: flex; background: var(--cream); border-radius: 8px; padding: 4px; margin-bottom: 24px; }
  .tab{ flex: 1; background: transparent; border: none; padding: 10px; font-family: inherit; font-weight: 700; font-size: 13px; text-transform: uppercase; letter-spacing: 1px; color: var(--muted); cursor: pointer; border-radius: 6px; transition: all .15s; }
  .tab.active{ background: #fff; color: var(--bg); box-shadow: 0 2px 6px rgba(0,0,0,0.08); }
  .tab-panel{ display: none; }
  .tab-panel.active{ display: block; }

  .form-group{ margin-bottom: 14px; }
  .form-group label{ display: block; font-size: 11px; font-weight: 700; letter-spacing: 1px; text-transform: uppercase; color: var(--muted); margin-bottom: 6px; }
  .form-group input{ width: 100%; padding: 12px 14px; border: 2px solid rgba(0,0,0,0.1); border-radius: 6px; font-size: 15px; font-family: inherit; background: #fff; color: var(--ink); transition: border-color .15s; }
  .form-group input:focus{ outline: none; border-color: var(--green); }
  .form-error{ background: #fde8e8; color: #a31616; padding: 10px 14px; border-radius: 6px; font-size: 13px; margin-bottom: 14px; display: none; }
  .form-error.show{ display: block; }
  .form-success{ background: #e8f8ef; color: #086b3a; padding: 10px 14px; border-radius: 6px; font-size: 13px; margin-bottom: 14px; display: none; }
  .form-success.show{ display: block; }
  .form-submit{ width: 100%; background: var(--green); color: #fff; border: none; padding: 14px; font-family: inherit; font-weight: 800; font-size: 14px; text-transform: uppercase; letter-spacing: 1px; cursor: pointer; border-radius: 6px; box-shadow: 0 4px 0 var(--green-deep); transition: transform .15s, box-shadow .15s; margin-top: 6px; }
  .form-submit:hover{ transform: translateY(2px); box-shadow: 0 2px 0 var(--green-deep); }
  .form-foot{ text-align: center; margin-top: 14px; font-size: 13px; color: var(--muted); }
  .form-foot a{ color: var(--green-deep); font-weight: 700; cursor: pointer; }
  .gate-secure{
    margin-top: 18px;
    padding-top: 18px;
    border-top: 1px solid rgba(0,0,0,0.06);
    text-align: center;
    font-size: 11px;
    color: var(--muted);
    letter-spacing: 1px;
  }
  .gate-secure span{ display: inline-flex; align-items: center; gap: 6px; }

  /* ============= NAV ============= */
  .nav{ position: sticky; top:0; z-index: 50; background: var(--bg); border-bottom: 3px solid var(--gold); color: #fff; }
  .nav-inner{ max-width: 1280px; margin: 0 auto; display: flex; align-items: center; justify-content: space-between; padding: 14px 28px; gap: 16px; }
  .brand{ display:flex; align-items:center; gap:12px; font-family: 'Bebas Neue', sans-serif; font-size: 24px; letter-spacing: 2px; cursor: pointer; }
  .brand-mark{ width: 38px; height: 38px; border-radius: 50%; background: var(--gold); display:grid; place-items:center; color: var(--bg); font-weight: 900; box-shadow: 0 4px 0 var(--gold-deep); }
  .nav-links{ display:flex; gap: 24px; font-weight: 600; font-size: 13px; letter-spacing: .5px; text-transform: uppercase; }
  .nav-links a{ cursor: pointer; }
  .nav-links a:hover{ color: var(--gold); }
  .nav-right{ display: flex; align-items: center; gap: 10px; }
  .admin-btn{ background: var(--gold); color: var(--bg); border: none; padding: 8px 14px; font-weight: 800; letter-spacing: .5px; text-transform: uppercase; font-size: 12px; cursor: pointer; border-radius: 4px; font-family: inherit; display: none; align-items: center; gap: 6px; }
  .admin-btn.show{ display: inline-flex; }
  .admin-btn:hover{ background: var(--gold-deep); }

  .user-chip{ display: flex; align-items: center; gap: 8px; background: rgba(255,255,255,0.08); border: 1px solid rgba(255,255,255,0.15); padding: 5px 6px 5px 12px; border-radius: 999px; cursor: pointer; position: relative; }
  .user-chip .uname{ font-size: 13px; font-weight: 700; max-width: 110px; overflow: hidden; text-overflow: ellipsis; white-space: nowrap; }
  .user-chip .uavatar{ width: 28px; height: 28px; border-radius: 50%; background: var(--gold); color: var(--ink); display: grid; place-items: center; font-family: 'Bebas Neue', sans-serif; font-size: 14px; }
  .user-chip.is-admin .uavatar{ background: var(--red); color: #fff; }
  .user-menu{ position: absolute; top: calc(100% + 8px); right: 0; background: #fff; color: var(--ink); min-width: 240px; border-radius: 8px; box-shadow: 0 18px 40px rgba(0,0,0,0.2); overflow: hidden; display: none; z-index: 60; }
  .user-menu.show{ display: block; }
  .user-menu a, .user-menu button{ display: block; width: 100%; text-align: left; padding: 12px 16px; background: transparent; border: none; cursor: pointer; font-family: inherit; font-size: 14px; color: var(--ink); }
  .user-menu a:hover, .user-menu button:hover{ background: var(--cream); }
  .user-menu .divider{ border-top: 1px solid rgba(0,0,0,0.06); }
  .user-menu .logout{ color: var(--red); font-weight: 700; }
  .user-info-head{ padding: 14px 16px; background: var(--bg); color: #fff; font-size: 13px; }
  .user-info-head .em{ color: rgba(255,255,255,0.6); font-size: 11px; margin-top: 2px; }
  .user-info-head .role{ display: inline-block; margin-top: 6px; padding: 2px 8px; background: var(--gold); color: var(--bg); font-size: 10px; font-weight: 800; letter-spacing: 1px; border-radius: 3px; }
  .user-info-head .role.user{ background: var(--green); color: #fff; }

  @media (max-width: 820px){ .nav-links{ display:none; } }
  @media (max-width: 500px){
    .brand{ font-size: 20px; }
    .user-chip .uname{ display: none; }
  }

  /* ============= HERO ============= */
  .hero{ background: radial-gradient(circle at 20% 30%, rgba(0,184,107,0.18), transparent 50%), radial-gradient(circle at 80% 70%, rgba(255,204,41,0.15), transparent 50%), linear-gradient(180deg, var(--bg) 0%, var(--bg-2) 100%); color: #fff; position: relative; overflow: hidden; padding: 70px 28px 90px; }
  .hero::before{ content:""; position:absolute; inset:0; background-image: radial-gradient(rgba(255,255,255,0.06) 1px, transparent 1px); background-size: 22px 22px; pointer-events:none; }
  .hero-inner{ max-width: 1280px; margin: 0 auto; display: grid; grid-template-columns: 1.1fr 1fr; gap: 60px; align-items: center; position: relative; z-index: 1; }
  @media(max-width: 900px){ .hero-inner{ grid-template-columns: 1fr; gap: 50px; } }

  .hero-tag{ display: inline-flex; align-items: center; gap: 8px; background: rgba(255,204,41,0.15); border: 1px solid rgba(255,204,41,0.4); color: var(--gold); padding: 8px 14px; border-radius: 999px; font-size: 12px; font-weight: 700; letter-spacing: 1.5px; text-transform: uppercase; margin-bottom: 24px; }
  .hero-tag .pulse{ width:8px; height:8px; border-radius:50%; background: var(--gold); animation: pulse 2s infinite; }
  @keyframes pulse{ 0%{ box-shadow: 0 0 0 0 rgba(255,204,41,0.6); } 70%{ box-shadow: 0 0 0 12px rgba(255,204,41,0); } 100%{ box-shadow: 0 0 0 0 rgba(255,204,41,0); } }

  h1.hero-title{ font-family: 'Bebas Neue', sans-serif; font-size: clamp(48px, 7vw, 96px); line-height: 0.95; letter-spacing: 1px; text-transform: uppercase; }
  h1.hero-title .accent{ color: var(--gold); }
  h1.hero-title em{ font-style: normal; background: linear-gradient(90deg, var(--gold), var(--green)); -webkit-background-clip: text; background-clip: text; -webkit-text-fill-color: transparent; }
  .hero-sub{ margin-top: 22px; font-size: 18px; line-height: 1.55; color: rgba(255,255,255,0.78); max-width: 520px; }
  .hero-actions{ display:flex; gap: 14px; margin-top: 32px; flex-wrap: wrap; }
  .btn{ display: inline-flex; align-items:center; gap: 10px; padding: 16px 28px; font-weight: 800; text-transform: uppercase; letter-spacing: 1px; font-size: 14px; border: none; cursor: pointer; border-radius: 4px; transition: transform .15s, box-shadow .15s; font-family: inherit; text-decoration: none; }
  .btn-primary{ background: var(--gold); color: var(--ink); box-shadow: 0 5px 0 var(--gold-deep); }
  .btn-primary:hover{ transform: translateY(2px); box-shadow: 0 3px 0 var(--gold-deep); }
  .btn-ghost{ background: transparent; color: #fff; border: 2px solid rgba(255,255,255,0.25); }
  .btn-ghost:hover{ border-color: var(--gold); color: var(--gold); }

  .hero-stats{ display:flex; gap: 36px; margin-top: 50px; padding-top: 30px; border-top: 1px solid var(--line); flex-wrap: wrap; }
  .stat-num{ font-family: 'Bebas Neue', sans-serif; font-size: 38px; color: var(--gold); line-height: 1; }
  .stat-label{ font-size: 12px; text-transform: uppercase; letter-spacing: 1.5px; color: rgba(255,255,255,0.6); margin-top: 6px;}

  .hero-visual{ position: relative; aspect-ratio: 1 / 1; display: grid; place-items: center; }
  .hero-album-img{ width: 72%; aspect-ratio: 3/4; object-fit: cover; border-radius: 8px; box-shadow: 0 30px 60px -10px rgba(0,0,0,0.5), 0 0 0 6px var(--gold), 0 0 0 10px rgba(0,0,0,0.2); transform: rotate(-4deg); animation: floaty 6s ease-in-out infinite; background: var(--green); }
  @keyframes floaty{ 0%, 100%{ transform: rotate(-4deg) translateY(0); } 50%{ transform: rotate(-3deg) translateY(-12px); } }
  .year-badge{ position: absolute; top: 12%; left: 0; background: var(--red); color: #fff; font-family: 'Bebas Neue', sans-serif; font-size: 22px; letter-spacing: 3px; padding: 8px 18px; border-radius: 4px; box-shadow: 0 8px 20px rgba(0,0,0,0.3); transform: rotate(-8deg); z-index: 5; }
  .float-img{ position: absolute; width: 130px; aspect-ratio: 3/4; object-fit: cover; border-radius: 6px; box-shadow: 0 14px 30px rgba(0,0,0,0.45); border: 4px solid #fff; background: #fff; }
  .float-img.f1{ top: 4%; right: 0%; transform: rotate(8deg); animation: float1 5s ease-in-out infinite; }
  .float-img.f2{ bottom: 6%; left: -2%; transform: rotate(-10deg); animation: float2 6s ease-in-out infinite; }
  .float-img.f3{ top: 42%; right: -4%; transform: rotate(14deg); animation: float3 7s ease-in-out infinite; }
  @keyframes float1{ 0%,100%{ transform: rotate(8deg) translateY(0); } 50%{ transform: rotate(10deg) translateY(-10px); } }
  @keyframes float2{ 0%,100%{ transform: rotate(-10deg) translateY(0); } 50%{ transform: rotate(-12deg) translateY(-14px); } }
  @keyframes float3{ 0%,100%{ transform: rotate(14deg) translateY(0); } 50%{ transform: rotate(12deg) translateY(-8px); } }

  .ticker{ background: var(--gold); color: var(--ink); overflow: hidden; white-space: nowrap; padding: 10px 0; border-top: 3px solid var(--bg); border-bottom: 3px solid var(--bg); font-family: 'Bebas Neue', sans-serif; letter-spacing: 4px; font-size: 18px; }
  .ticker-track{ display: inline-flex; gap: 40px; animation: scroll 30s linear infinite; }
  .ticker-track span{ display:inline-flex; align-items:center; gap: 40px; }
  .ticker .dot{ width:8px; height:8px; background: var(--bg); border-radius:50%; display:inline-block; }
  @keyframes scroll{ from{ transform: translateX(0); } to{ transform: translateX(-50%); } }

  section{ padding: 90px 28px; }
  .container{ max-width: 1280px; margin: 0 auto; }
  .eyebrow{ display: inline-block; font-family: 'Archivo Narrow', sans-serif; text-transform: uppercase; letter-spacing: 4px; font-size: 12px; color: var(--green-deep); padding-bottom: 6px; border-bottom: 2px solid var(--gold); margin-bottom: 18px; }
  .section-title{ font-family: 'Bebas Neue', sans-serif; font-size: clamp(40px, 5vw, 64px); line-height: 1; letter-spacing: 1px; color: var(--bg); }
  .section-sub{ margin-top: 14px; max-width: 560px; font-size: 17px; color: var(--muted); line-height: 1.6; }

  .products{ background: var(--cream); }
  .products-head{ display:flex; justify-content: space-between; align-items: flex-end; margin-bottom: 50px; gap: 40px; flex-wrap: wrap; }
  .filters{ display:flex; gap: 10px; flex-wrap: wrap; }
  .chip{ border: 2px solid var(--ink); background: transparent; color: var(--ink); padding: 8px 16px; font-family: 'Archivo', sans-serif; font-weight: 700; font-size: 13px; text-transform: uppercase; letter-spacing: 1px; cursor: pointer; border-radius: 999px; transition: all .15s; }
  .chip.active, .chip:hover{ background: var(--ink); color: var(--cream); }

  .grid{ display: grid; grid-template-columns: repeat(auto-fill, minmax(250px, 1fr)); gap: 30px; }

  .card{ background: #fff; border-radius: 8px; overflow: hidden; border: 1px solid rgba(0,0,0,0.06); transition: transform .25s, box-shadow .25s; position: relative; display: flex; flex-direction: column; }
  .card:hover{ transform: translateY(-6px); box-shadow: 0 24px 40px -12px rgba(10,31,61,0.25); }
  .badge{ position: absolute; top: 14px; left: 14px; background: var(--red); color: #fff; font-family: 'Bebas Neue', sans-serif; padding: 4px 10px; font-size: 14px; letter-spacing: 2px; border-radius: 4px; z-index: 2; }
  .badge.gold{ background: var(--gold); color: var(--ink); }
  .badge.green{ background: var(--green); color: #fff; }
  .badge-2026{ position: absolute; top: 14px; right: 14px; background: var(--bg); color: var(--gold); font-family: 'Bebas Neue', sans-serif; padding: 4px 10px; font-size: 14px; letter-spacing: 2px; border-radius: 4px; z-index: 2; }

  .card-img-wrap{ aspect-ratio: 1; overflow: hidden; background: #f0e9d6; position: relative; }
  .card-img-wrap img{ width: 100%; height: 100%; object-fit: cover; transition: transform .4s ease; }
  .card:hover .card-img-wrap img{ transform: scale(1.06); }

  .card-body{ padding: 22px; display: flex; flex-direction: column; gap: 10px; flex: 1; }
  .card-cat{ font-family: 'Archivo Narrow', sans-serif; text-transform: uppercase; letter-spacing: 3px; font-size: 11px; color: var(--muted); }
  .card-name{ font-family: 'Bebas Neue', sans-serif; font-size: 22px; letter-spacing: 1px; line-height: 1.15; color: var(--bg); }
  .price{ font-family: 'Bebas Neue', sans-serif; font-size: 28px; color: var(--green-deep); letter-spacing: 1px; margin-top: 4px; }
  .price .old{ font-size: 14px; color: var(--muted); text-decoration: line-through; margin-right: 6px; letter-spacing: 0; }
  .buy-btn{ margin-top: 16px; background: var(--green); color: #fff; border: none; padding: 12px; font-family: inherit; font-weight: 800; font-size: 13px; text-transform: uppercase; letter-spacing: 1px; cursor: pointer; border-radius: 6px; box-shadow: 0 3px 0 var(--green-deep); transition: transform .15s, box-shadow .15s; display: flex; align-items: center; justify-content: center; gap: 8px; text-decoration: none; }
  .buy-btn:hover{ transform: translateY(2px); box-shadow: 0 1px 0 var(--green-deep); }
  .buy-btn.disabled{ background: #bbb; box-shadow: 0 3px 0 #999; cursor: not-allowed; }

  /* BANNER */
  .banner{ background: linear-gradient(135deg, rgba(10,31,61,0.94), rgba(6,21,43,0.94)), repeating-linear-gradient(45deg, transparent 0 20px, rgba(255,255,255,0.03) 20px 22px); color: #fff; padding: 70px 28px; text-align: center; position: relative; overflow: hidden; border-top: 4px solid var(--gold); border-bottom: 4px solid var(--green); }
  .banner h2{ font-family: 'Bebas Neue', sans-serif; font-size: clamp(36px, 5vw, 60px); letter-spacing: 2px; line-height: 1; }
  .banner h2 .hl{ color: var(--gold); }
  .banner p{ margin-top: 12px; color: rgba(255,255,255,0.7); font-size: 17px; }
  .countdown{ margin-top: 32px; display: inline-flex; gap: 18px; flex-wrap: wrap; justify-content: center; }
  .cd-box{ background: rgba(255,255,255,0.08); border: 1px solid rgba(255,255,255,0.15); border-radius: 8px; padding: 14px 18px; min-width: 80px; }
  .cd-num{ font-family: 'Bebas Neue', sans-serif; font-size: 38px; color: var(--gold); line-height: 1; }
  .cd-label{ font-size: 10px; text-transform: uppercase; letter-spacing: 2px; color: rgba(255,255,255,0.6); margin-top: 6px; }

  /* DEPOIMENTOS */
  .testimonials{ background: #fff; }
  .test-grid{ display: grid; grid-template-columns: repeat(auto-fit, minmax(280px, 1fr)); gap: 28px; margin-top: 50px; }
  .test-card{ background: var(--cream); border-radius: 8px; padding: 28px; position: relative; border-left: 4px solid var(--green); }
  .test-card::before{ content:"\201C"; position:absolute; top: -10px; right: 20px; font-family: 'Bebas Neue', sans-serif; font-size: 90px; color: var(--gold); line-height: 1; }
  .stars{ color: var(--gold); letter-spacing: 3px; margin-bottom: 14px; }
  .test-text{ font-size: 16px; line-height: 1.55; color: var(--ink); margin-bottom: 18px; }
  .test-author{ display:flex; align-items:center; gap: 12px; padding-top: 16px; border-top: 1px solid rgba(0,0,0,0.08); }
  .avatar{ width: 42px; height: 42px; border-radius: 50%; background: var(--bg); color: var(--gold); display: grid; place-items: center; font-family: 'Bebas Neue', sans-serif; font-size: 18px; }
  .author-name{ font-weight: 700; font-size: 14px; }
  .author-loc{ font-size: 12px; color: var(--muted); }

  /* FOOTER */
  footer{ background: var(--bg-2); color: rgba(255,255,255,0.7); padding: 70px 28px 30px; }
  .foot-grid{ max-width: 1280px; margin: 0 auto; display: grid; grid-template-columns: 1.4fr 1fr 1fr 1fr; gap: 40px; margin-bottom: 50px; }
  @media(max-width: 800px){ .foot-grid{ grid-template-columns: 1fr 1fr; } }
  @media(max-width: 500px){ .foot-grid{ grid-template-columns: 1fr; } }
  .foot-col h4{ font-family: 'Bebas Neue', sans-serif; font-size: 18px; letter-spacing: 3px; color: #fff; margin-bottom: 18px; }
  .foot-col ul{ list-style: none; display: flex; flex-direction: column; gap: 10px; font-size: 14px;}
  .foot-col a{ cursor: pointer; }
  .foot-col a:hover{ color: var(--gold); }
  .foot-brand-desc{ font-size: 14px; line-height: 1.6; margin-top: 18px; }
  .pay{ display: flex; gap: 8px; margin-top: 18px; flex-wrap: wrap; }
  .pay span{ background: rgba(255,255,255,0.08); border: 1px solid rgba(255,255,255,0.12); padding: 6px 10px; border-radius: 4px; font-size: 11px; letter-spacing: 1px; text-transform: uppercase; }
  .legal-notice{ max-width: 1280px; margin: 0 auto 24px; padding: 18px 22px; background: rgba(255,255,255,0.04); border-left: 3px solid var(--gold); border-radius: 4px; font-size: 12px; line-height: 1.6; color: rgba(255,255,255,0.65); }
  .legal-notice b{ color: var(--gold); }
  .copy{ border-top: 1px solid rgba(255,255,255,0.1); padding-top: 24px; text-align: center; font-size: 13px; color: rgba(255,255,255,0.5); max-width: 1280px; margin: 0 auto; }

  /* TOAST */
  .toast{ position: fixed; bottom: 24px; right: 24px; background: var(--bg); color: #fff; padding: 16px 24px; border-radius: 8px; border-left: 4px solid var(--green); box-shadow: 0 12px 30px rgba(0,0,0,0.25); z-index: 9000; font-size: 14px; transform: translateX(120%); transition: transform .3s ease; max-width: 320px; }
  .toast.show{ transform: translateX(0); }
  .toast.error{ border-left-color: var(--red); }

  /* ===================================================================== */
  /* ============= PAINEL ADMINISTRATIVO ============= */
  /* ===================================================================== */
  .admin{
    position: fixed; inset: 0;
    background: #f1f4f9;
    z-index: 9500;
    display: none;
    flex-direction: column;
    overflow: hidden;
  }
  .admin.show{ display: flex; }
  .admin-top{
    background: var(--bg);
    color: #fff;
    padding: 14px 24px;
    display: flex; justify-content: space-between; align-items: center;
    border-bottom: 3px solid var(--gold);
    flex-shrink: 0;
  }
  .admin-title{
    font-family: 'Bebas Neue', sans-serif;
    font-size: 22px;
    letter-spacing: 2px;
    display: flex; align-items: center; gap: 10px;
  }
  .admin-title .crown{ color: var(--gold); }
  .admin-actions{ display: flex; gap: 10px; }
  .adm-back{
    background: var(--green); color: #fff;
    border: none; padding: 8px 16px;
    font-family: inherit; font-weight: 700; font-size: 12px;
    text-transform: uppercase; letter-spacing: 1px;
    cursor: pointer; border-radius: 4px;
  }
  .adm-back:hover{ background: var(--green-deep); }
  .adm-out{
    background: transparent; color: #fff;
    border: 2px solid rgba(255,255,255,0.2);
    padding: 6px 14px;
    font-family: inherit; font-weight: 700; font-size: 12px;
    text-transform: uppercase; letter-spacing: 1px;
    cursor: pointer; border-radius: 4px;
  }
  .adm-out:hover{ border-color: var(--red); color: var(--red); }

  .admin-body{
    flex: 1;
    display: flex;
    overflow: hidden;
  }
  .adm-sidebar{
    width: 220px;
    background: #fff;
    border-right: 1px solid rgba(0,0,0,0.08);
    padding: 24px 0;
    flex-shrink: 0;
    overflow-y: auto;
  }
  .adm-nav{
    display: block;
    width: 100%;
    text-align: left;
    background: transparent;
    border: none;
    padding: 12px 24px;
    font-family: inherit;
    font-size: 14px;
    font-weight: 600;
    color: var(--muted);
    cursor: pointer;
    border-left: 3px solid transparent;
    transition: all .15s;
  }
  .adm-nav:hover{ background: var(--cream); color: var(--bg); }
  .adm-nav.active{ background: var(--cream); color: var(--bg); border-left-color: var(--gold); font-weight: 800; }
  .adm-nav .ic{ margin-right: 8px; }

  .adm-content{
    flex: 1;
    padding: 32px;
    overflow-y: auto;
  }
  .adm-page-title{
    font-family: 'Bebas Neue', sans-serif;
    font-size: 32px;
    letter-spacing: 1px;
    color: var(--bg);
    margin-bottom: 6px;
  }
  .adm-page-sub{
    color: var(--muted);
    font-size: 14px;
    margin-bottom: 28px;
  }
  .adm-section{ display: none; }
  .adm-section.active{ display: block; }

  /* KPI Cards */
  .kpis{
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
    gap: 16px;
    margin-bottom: 30px;
  }
  .kpi{
    background: #fff;
    border-radius: 10px;
    padding: 20px;
    border: 1px solid rgba(0,0,0,0.06);
    position: relative;
    overflow: hidden;
  }
  .kpi::after{
    content: ''; position: absolute; right: -10px; top: -10px;
    width: 60px; height: 60px; border-radius: 50%;
    background: var(--cream);
    opacity: .6;
  }
  .kpi-label{
    font-size: 11px;
    text-transform: uppercase;
    letter-spacing: 2px;
    color: var(--muted);
    font-weight: 700;
  }
  .kpi-value{
    font-family: 'Bebas Neue', sans-serif;
    font-size: 38px;
    color: var(--bg);
    line-height: 1.1;
    margin-top: 4px;
    position: relative;
    z-index: 1;
  }
  .kpi-meta{
    font-size: 12px;
    color: var(--green-deep);
    margin-top: 4px;
    font-weight: 600;
  }
  .kpi.gold .kpi-value{ color: var(--gold-deep); }
  .kpi.green .kpi-value{ color: var(--green-deep); }
  .kpi.red .kpi-value{ color: var(--red); }

  /* Tabela */
  .adm-card{
    background: #fff;
    border-radius: 10px;
    border: 1px solid rgba(0,0,0,0.06);
    overflow: hidden;
    margin-bottom: 24px;
  }
  .adm-card-head{
    padding: 18px 22px;
    border-bottom: 1px solid rgba(0,0,0,0.06);
    display: flex; justify-content: space-between; align-items: center;
    gap: 12px;
    flex-wrap: wrap;
  }
  .adm-card-title{
    font-weight: 800;
    font-size: 15px;
    color: var(--bg);
  }
  .adm-search{
    border: 1px solid rgba(0,0,0,0.1);
    border-radius: 6px;
    padding: 8px 12px;
    font-family: inherit;
    font-size: 13px;
    min-width: 220px;
  }
  .adm-search:focus{ outline: none; border-color: var(--green); }
  .adm-table{
    width: 100%;
    border-collapse: collapse;
    font-size: 13px;
  }
  .adm-table th{
    background: var(--cream);
    text-align: left;
    padding: 12px 22px;
    font-size: 11px;
    text-transform: uppercase;
    letter-spacing: 1.5px;
    color: var(--muted);
    font-weight: 800;
    border-bottom: 1px solid rgba(0,0,0,0.06);
  }
  .adm-table td{
    padding: 14px 22px;
    border-bottom: 1px solid rgba(0,0,0,0.04);
    vertical-align: middle;
  }
  .adm-table tr:last-child td{ border-bottom: none; }
  .adm-table tr:hover{ background: #fafbfd; }
  .adm-table .mono{
    font-family: 'JetBrains Mono', monospace;
    font-size: 12px;
  }
  .role-pill{
    display: inline-block;
    padding: 3px 8px;
    border-radius: 4px;
    font-size: 10px;
    font-weight: 800;
    letter-spacing: 1px;
    text-transform: uppercase;
  }
  .role-pill.admin{ background: var(--gold); color: var(--bg); }
  .role-pill.user{ background: #e5f5ed; color: var(--green-deep); }
  .log-pill{
    display: inline-block;
    padding: 3px 8px;
    border-radius: 4px;
    font-size: 10px;
    font-weight: 700;
    letter-spacing: 1px;
    text-transform: uppercase;
    font-family: 'JetBrains Mono', monospace;
  }
  .log-pill.login{ background: #e3f2fd; color: #1565c0; }
  .log-pill.register{ background: #e8f5e9; color: #2e7d32; }
  .log-pill.logout{ background: #f3e5f5; color: #6a1b9a; }
  .log-pill.buy{ background: #fff3e0; color: #e65100; }
  .log-pill.fail{ background: #ffebee; color: #c62828; }
  .log-pill.view{ background: #f5f5f5; color: #616161; }
  .log-pill.admin{ background: var(--gold); color: var(--bg); }

  .empty-state{
    text-align: center;
    padding: 60px 20px;
    color: var(--muted);
  }
  .empty-state-icon{ font-size: 48px; margin-bottom: 12px; }

  .adm-btn-danger{
    background: var(--red); color: #fff;
    border: none; padding: 5px 10px;
    font-family: inherit; font-size: 11px;
    font-weight: 700; text-transform: uppercase; letter-spacing: 1px;
    cursor: pointer; border-radius: 4px;
  }
  .adm-btn-danger:hover{ opacity: .85; }

  .adm-info-bar{
    background: #fffbeb;
    border: 1px solid #fbd95a;
    border-radius: 8px;
    padding: 12px 18px;
    font-size: 13px;
    color: #6b5410;
    margin-bottom: 20px;
  }
  .adm-info-bar b{ color: #4d3a08; }

  /* Botões admin v2 */
  .adm-btn-primary{
    background: var(--green); color: #fff;
    border: none; padding: 9px 16px;
    font-family: inherit; font-size: 12px;
    font-weight: 800; text-transform: uppercase; letter-spacing: .8px;
    cursor: pointer; border-radius: 6px;
    display: inline-flex; align-items: center; gap: 6px;
    transition: all .15s;
  }
  .adm-btn-primary:hover{ background: var(--green-deep); transform: translateY(-1px); }
  .adm-btn-secondary{
    background: #eef1f7; color: var(--ink);
    border: 1px solid #d8dde9; padding: 9px 16px;
    font-family: inherit; font-size: 12px;
    font-weight: 800; text-transform: uppercase; letter-spacing: .8px;
    cursor: pointer; border-radius: 6px;
    display: inline-flex; align-items: center; gap: 6px;
  }
  .adm-btn-secondary:hover{ background: #e2e7f1; }
  .adm-btn-edit{
    background: #1565c0; color: #fff;
    border: none; padding: 5px 10px;
    font-family: inherit; font-size: 11px;
    font-weight: 700; text-transform: uppercase; letter-spacing: 1px;
    cursor: pointer; border-radius: 4px; margin-right: 4px;
  }
  .adm-btn-edit:hover{ background: #0d47a1; }
  .adm-input{
    width: 100%;
    padding: 11px 14px;
    border: 1.5px solid #d8dde9;
    border-radius: 7px;
    font-family: inherit; font-size: 14px;
    transition: border-color .15s;
  }
  .adm-input:focus{ outline: none; border-color: var(--green); }

  /* Modal de produto */
  .prod-modal{
    position: fixed; inset: 0;
    background: rgba(11,20,38,0.7);
    backdrop-filter: blur(6px);
    z-index: 12000;
    display: none;
    align-items: center; justify-content: center;
    padding: 20px;
    overflow-y: auto;
  }
  .prod-modal.show{ display: flex; }
  .prod-modal-card{
    background: #fff;
    border-radius: 14px;
    width: 100%; max-width: 540px;
    overflow: hidden;
    box-shadow: 0 30px 80px rgba(0,0,0,0.5);
    animation: prodPop .25s ease;
  }
  @keyframes prodPop{ from{ transform: scale(.92); opacity: 0; } to{ transform: scale(1); opacity: 1; } }
  .prod-modal-head{
    background: var(--bg); color: #fff;
    padding: 18px 24px;
    border-bottom: 3px solid var(--gold);
    font-family: 'Bebas Neue', sans-serif;
    font-size: 22px; letter-spacing: 1.5px;
    display: flex; justify-content: space-between; align-items: center;
  }
  .prod-modal-x{
    background: transparent; border: none; color: #fff;
    font-size: 28px; cursor: pointer; line-height: 1;
    padding: 0 4px;
  }
  .prod-modal-x:hover{ color: var(--gold); }
  .prod-modal-body{
    padding: 24px;
    display: flex; flex-direction: column; gap: 14px;
    max-height: 70vh; overflow-y: auto;
  }
  .prod-row{ display: grid; grid-template-columns: 1fr 1fr; gap: 14px; }
  .prod-field label{
    display: block; font-size: 11px; font-weight: 800;
    letter-spacing: 1px; text-transform: uppercase;
    color: var(--muted); margin-bottom: 6px;
  }
  .prod-field input{
    width: 100%; padding: 11px 14px;
    border: 1.5px solid #d8dde9; border-radius: 7px;
    font-family: inherit; font-size: 14px;
  }
  .prod-field input:focus{ outline: none; border-color: var(--green); }
  .prod-modal-foot{
    padding: 16px 24px;
    background: #f7f9fc;
    border-top: 1px solid #e2e7f1;
    display: flex; justify-content: flex-end; gap: 10px;
  }

  /* Remember me checkbox no login */
  .remember-row{
    display: flex; align-items: center; gap: 8px;
    margin: 12px 0 4px;
    font-size: 13px; color: var(--muted);
    cursor: pointer;
  }
  .remember-row input{ width: 16px; height: 16px; cursor: pointer; accent-color: var(--green); }
  .remember-row label{ cursor: pointer; user-select: none; }

  /* Sidebar mobile do admin */
  @media (max-width: 700px){
    .admin-top{ flex-wrap: wrap; gap: 10px; padding: 14px 16px; }
    .admin-title{ font-size: 16px; }
    .admin-body{ flex-direction: column; }
    .adm-sidebar{
      display: flex !important;
      flex-direction: row;
      width: 100%;
      overflow-x: auto;
      padding: 8px;
      gap: 6px;
      border-right: none;
      border-bottom: 1px solid var(--line);
    }
    .adm-nav{
      flex: 0 0 auto; white-space: nowrap;
      font-size: 12px; padding: 10px 14px;
    }
    .adm-content{ padding: 18px; }
    .prod-row{ grid-template-columns: 1fr; }
    .adm-card-head{ flex-direction: column; align-items: stretch; gap: 10px; }
    .adm-table{ font-size: 12px; }
    .adm-table th, .adm-table td{ padding: 8px; }
  }
</style>
</style>
</head>
<body>

<!-- SPLASH DE LOADING (some quando o Supabase responde sobre sessão) -->
<div id="splash" style="position:fixed;inset:0;z-index:99999;background:linear-gradient(180deg,#0a1f3d 0%,#06152b 100%);display:flex;flex-direction:column;align-items:center;justify-content:center;gap:20px;">
  <div style="width:64px;height:64px;border-radius:50%;background:#ffcc29;display:grid;place-items:center;font-size:32px;animation:bounce 1.2s ease-in-out infinite">⚽</div>
  <div style="color:#fff;font-family:'Bebas Neue',sans-serif;letter-spacing:3px;font-size:20px">STORE COPA</div>
  <div style="color:rgba(255,255,255,0.5);font-size:12px;font-family:'Archivo',sans-serif;letter-spacing:1px;text-transform:uppercase">carregando...</div>
  <style>
    @keyframes bounce { 0%,100%{transform:translateY(0)} 50%{transform:translateY(-12px)} }
  </style>
</div>

<!-- ===================================================================== -->
<!-- LOGIN GATE - bloqueia tudo até login -->
<!-- ===================================================================== -->
<div class="gate hide" id="gate">
  <div class="gate-card">
    <div class="gate-head">
      <div class="gate-logo">
        <div class="gate-logo-mark">⚽</div>
        <div>STORE <span class="accent">COPA</span></div>
      </div>
      <div class="gate-tag">EDIÇÃO OFICIAL COPA 2026</div>
    </div>
    <div class="gate-body">
      <div class="gate-title" id="gateTitle">Faça login para entrar</div>
      <div class="gate-sub" id="gateSub">Acesso restrito a usuários cadastrados</div>

      <div class="tabs">
        <button class="tab active" id="tabLogin" onclick="switchTab('login')">Entrar</button>
        <button class="tab" id="tabRegister" onclick="switchTab('register')">Criar conta</button>
      </div>

      <div class="form-error" id="formError"></div>
      <div class="form-success" id="formSuccess"></div>

      <div class="tab-panel active" id="panelLogin">
        <div class="form-group">
          <label>E-mail</label>
          <input type="email" id="loginEmail" placeholder="seu@email.com" autocomplete="email">
        </div>
        <div class="form-group">
          <label>Senha</label>
          <input type="password" id="loginPassword" placeholder="••••••••" autocomplete="current-password">
        </div>
        <div class="remember-row">
          <input type="checkbox" id="rememberMe" checked>
          <label for="rememberMe">Manter conectado neste dispositivo</label>
        </div>
        <button class="form-submit" onclick="doLogin()">Entrar</button>
        <div class="form-foot">
          Ainda não tem conta? <a onclick="switchTab('register')">Cadastre-se grátis</a>
        </div>
      </div>

      <div class="tab-panel" id="panelRegister">
        <div class="form-group">
          <label>Nome completo</label>
          <input type="text" id="regName" placeholder="João da Silva" autocomplete="name">
        </div>
        <div class="form-group">
          <label>E-mail</label>
          <input type="email" id="regEmail" placeholder="seu@email.com" autocomplete="email">
        </div>
        <div class="form-group">
          <label>Senha (mín. 6 caracteres)</label>
          <input type="password" id="regPassword" placeholder="••••••••" autocomplete="new-password">
        </div>
        <div class="form-group">
          <label>Confirme a senha</label>
          <input type="password" id="regPassword2" placeholder="••••••••" autocomplete="new-password">
        </div>
        <button class="form-submit" onclick="doRegister()">Criar minha conta</button>
        <div class="form-foot">
          Já tem conta? <a onclick="switchTab('login')">Faça login</a>
        </div>
      </div>

      <div class="gate-secure">
        <span>🔒 ACESSO SEGURO · SENHA CRIPTOGRAFADA (SHA-256)</span>
      </div>
    </div>
  </div>
</div>

<!-- ===================================================================== -->
<!-- PAINEL ADMIN (escondido até admin logar) -->
<!-- ===================================================================== -->
<div class="admin" id="adminPanel">
  <div class="admin-top">
    <div class="admin-title">
      <span class="crown">👑</span>
      PAINEL ADMINISTRATIVO — STORE COPA
    </div>
    <div class="admin-actions">
      <button class="adm-back" onclick="closeAdmin()">← Voltar à loja</button>
      <button class="adm-out" onclick="logout()">Sair</button>
    </div>
  </div>
  <div class="admin-body">
    <aside class="adm-sidebar">
      <button class="adm-nav active" onclick="switchAdminSection('dashboard', this)"><span class="ic">📊</span>Dashboard</button>
      <button class="adm-nav" onclick="switchAdminSection('products', this)"><span class="ic">📦</span>Produtos</button>
      <button class="adm-nav" onclick="switchAdminSection('users', this)"><span class="ic">👥</span>Usuários</button>
      <button class="adm-nav" onclick="switchAdminSection('logs', this)"><span class="ic">📜</span>Logs</button>
      <button class="adm-nav" onclick="switchAdminSection('settings', this)"><span class="ic">⚙️</span>Configurações</button>
    </aside>
    <main class="adm-content">

      <!-- DASHBOARD -->
      <section class="adm-section active" id="sec-dashboard">
        <h1 class="adm-page-title">Dashboard</h1>
        <p class="adm-page-sub">Visão geral da Store Copa em tempo real</p>

        <div class="kpis" id="kpiGrid"></div>

        <div class="adm-card">
          <div class="adm-card-head">
            <div class="adm-card-title">🏆 Top 5 produtos mais clicados</div>
          </div>
          <table class="adm-table" id="topProductsTable">
            <thead>
              <tr><th>#</th><th>Produto</th><th>Cliques</th><th>Receita estimada</th></tr>
            </thead>
            <tbody></tbody>
          </table>
        </div>

        <div class="adm-card">
          <div class="adm-card-head">
            <div class="adm-card-title">📜 Últimas 10 atividades</div>
          </div>
          <table class="adm-table" id="recentLogsTable">
            <thead>
              <tr><th>Quando</th><th>Tipo</th><th>Usuário</th><th>Detalhes</th></tr>
            </thead>
            <tbody></tbody>
          </table>
        </div>
      </section>

      <!-- PRODUTOS -->
      <section class="adm-section" id="sec-products">
        <h1 class="adm-page-title">Catálogo de produtos</h1>
        <p class="adm-page-sub">Edite produtos, preços, imagens e links de pagamento direto pelo painel</p>

        <div class="adm-card">
          <div class="adm-card-head">
            <div class="adm-card-title">📦 Catálogo atual</div>
            <button class="adm-btn-primary" onclick="openProductModal()">+ Novo produto</button>
          </div>
          <table class="adm-table" id="productsTable">
            <thead>
              <tr><th>#</th><th>Produto</th><th>Categoria</th><th>Preço</th><th>Link Pix</th><th>Ações</th></tr>
            </thead>
            <tbody></tbody>
          </table>
        </div>
      </section>

      <!-- USUÁRIOS -->
      <section class="adm-section" id="sec-users">
        <h1 class="adm-page-title">Usuários cadastrados</h1>
        <p class="adm-page-sub">Lista de todos os usuários que criaram conta no site</p>

        <div class="adm-card">
          <div class="adm-card-head">
            <div class="adm-card-title">👥 Lista completa</div>
            <input type="text" class="adm-search" id="userSearch" placeholder="Buscar por nome ou e-mail..." oninput="renderUsers()">
          </div>
          <table class="adm-table" id="usersTable">
            <thead>
              <tr><th>Nome</th><th>E-mail</th><th>Papel</th><th>Cadastrado em</th><th>Último login</th><th>Ações</th></tr>
            </thead>
            <tbody></tbody>
          </table>
        </div>
      </section>

      <!-- LOGS -->
      <section class="adm-section" id="sec-logs">
        <h1 class="adm-page-title">Logs do sistema</h1>
        <p class="adm-page-sub">Histórico completo de tudo que acontece no site</p>

        <div class="adm-card">
          <div class="adm-card-head">
            <div class="adm-card-title">📜 Todos os logs (mais recentes primeiro)</div>
            <div style="display:flex; gap:8px; flex-wrap: wrap;">
              <select class="adm-search" id="logFilter" onchange="renderLogs()" style="max-width:140px">
                <option value="">Todos os tipos</option>
                <option value="login">Login</option>
                <option value="logout">Logout</option>
                <option value="register">Cadastro</option>
                <option value="buy">Compra</option>
                <option value="admin">Admin</option>
                <option value="fail">Falhas</option>
              </select>
              <input type="text" class="adm-search" id="logSearch" placeholder="Buscar nos logs..." oninput="renderLogs()">
              <button class="adm-btn-danger" onclick="clearLogs()">🗑️ Limpar logs</button>
            </div>
          </div>
          <table class="adm-table" id="logsTable">
            <thead>
              <tr><th>Quando</th><th>Tipo</th><th>Usuário</th><th>Detalhes</th><th>Dispositivo</th></tr>
            </thead>
            <tbody></tbody>
          </table>
        </div>
      </section>

      <!-- CONFIG -->
      <section class="adm-section" id="sec-settings">
        <h1 class="adm-page-title">Configurações</h1>
        <p class="adm-page-sub">Gerencie a conta de administrador e o sistema</p>

        <div class="adm-card">
          <div class="adm-card-head">
            <div class="adm-card-title">🔐 Trocar senha do administrador</div>
          </div>
          <div style="padding: 22px; display:flex; flex-direction:column; gap:14px; max-width:480px;">
            <div>
              <label style="display:block;font-size:12px;font-weight:700;letter-spacing:.5px;text-transform:uppercase;color:var(--muted);margin-bottom:6px">Senha atual</label>
              <input type="password" class="adm-input" id="curPwd" placeholder="Senha atual">
            </div>
            <div>
              <label style="display:block;font-size:12px;font-weight:700;letter-spacing:.5px;text-transform:uppercase;color:var(--muted);margin-bottom:6px">Nova senha (mín. 6 caracteres)</label>
              <input type="password" class="adm-input" id="newPwd" placeholder="Nova senha">
            </div>
            <div>
              <label style="display:block;font-size:12px;font-weight:700;letter-spacing:.5px;text-transform:uppercase;color:var(--muted);margin-bottom:6px">Confirme a nova senha</label>
              <input type="password" class="adm-input" id="newPwd2" placeholder="Confirme">
            </div>
            <button class="adm-btn-primary" onclick="changeAdminPassword()" style="align-self:flex-start;margin-top:4px">Salvar nova senha</button>
            <div id="pwdMsg" style="font-size:13px;font-weight:600;display:none"></div>
          </div>
        </div>

        <div class="adm-card">
          <div class="adm-card-head">
            <div class="adm-card-title">⚙️ Informações do sistema</div>
          </div>
          <div style="padding: 22px; font-size: 14px; line-height: 1.9;">
            <p><b>Versão:</b> Store Copa v2.0</p>
            <p><b>Admin master:</b> <code class="mono">davilettrari@gmail.com</code></p>
            <p><b>Criptografia:</b> SHA-256 + salt</p>
            <p><b>Armazenamento:</b> localStorage (navegador) — pronto para Vercel/Netlify/Hostinger</p>
            <p><b>Pagamento:</b> Pushin Pay (Pix/Boleto)</p>
          </div>
        </div>

        <div class="adm-card">
          <div class="adm-card-head">
            <div class="adm-card-title">💾 Backup & exportação</div>
          </div>
          <div style="padding: 22px;">
            <p style="margin-bottom: 14px; font-size: 14px; color: var(--muted);">Faça download de um arquivo com todos os usuários, produtos e logs (JSON).</p>
            <div style="display:flex;gap:10px;flex-wrap:wrap">
              <button class="adm-btn-primary" onclick="exportBackup()">📥 Baixar backup</button>
              <label class="adm-btn-secondary" style="cursor:pointer">
                📤 Restaurar backup
                <input type="file" accept="application/json" onchange="importBackup(event)" style="display:none">
              </label>
            </div>
          </div>
        </div>

        <div class="adm-card">
          <div class="adm-card-head">
            <div class="adm-card-title">⚠️ Zona de perigo</div>
          </div>
          <div style="padding: 22px;">
            <p style="margin-bottom: 14px; font-size: 14px; color: var(--muted);">Estas ações são irreversíveis.</p>
            <button class="adm-btn-danger" onclick="resetAllData()">🗑️ Resetar TODOS os dados (usuários + logs + produtos)</button>
          </div>
        </div>
      </section>

    </main>
  </div>
</div>

<!-- MODAL DE PRODUTO (editar/criar) -->
<div class="prod-modal" id="prodModal">
  <div class="prod-modal-card">
    <div class="prod-modal-head">
      <span id="prodModalTitle">Editar produto</span>
      <button class="prod-modal-x" onclick="closeProductModal()">×</button>
    </div>
    <div class="prod-modal-body">
      <input type="hidden" id="prodId">
      <div class="prod-field">
        <label>Nome do produto</label>
        <input type="text" id="prodName" placeholder="Ex: Álbum Oficial Copa 2026">
      </div>
      <div class="prod-row">
        <div class="prod-field">
          <label>Categoria</label>
          <input type="text" id="prodCat" placeholder="Álbuns, Pacotes, Combos...">
        </div>
        <div class="prod-field">
          <label>Selo (opcional)</label>
          <input type="text" id="prodBadge" placeholder="Ex: Mais vendido">
        </div>
      </div>
      <div class="prod-row">
        <div class="prod-field">
          <label>Preço (R$)</label>
          <input type="number" id="prodPrice" step="0.01" min="0" placeholder="59.90">
        </div>
        <div class="prod-field">
          <label>Preço antigo (opcional)</label>
          <input type="number" id="prodOldPrice" step="0.01" min="0" placeholder="89.00">
        </div>
      </div>
      <div class="prod-field">
        <label>URL da imagem</label>
        <input type="text" id="prodImg" placeholder="https://...">
      </div>
      <div class="prod-field">
        <label>Link Pushin Pay (Pix)</label>
        <input type="text" id="prodLink" placeholder="https://app.pushinpay.com.br/...">
      </div>
    </div>
    <div class="prod-modal-foot">
      <button class="adm-btn-secondary" onclick="closeProductModal()">Cancelar</button>
      <button class="adm-btn-primary" onclick="saveProduct()">💾 Salvar</button>
    </div>
  </div>
</div>

<!-- ===================================================================== -->
<!-- SITE PRINCIPAL (escondido até logar) -->
<!-- ===================================================================== -->
<div id="mainSite" style="display:none">

<nav class="nav">
  <div class="nav-inner">
    <div class="brand" onclick="window.scrollTo({top:0,behavior:'smooth'})">
      <div class="brand-mark">⚽</div>
      <div>STORE <span style="color:var(--gold)">COPA</span></div>
    </div>
    <div class="nav-links">
      <a onclick="document.getElementById('produtos').scrollIntoView()">Produtos</a>
      <a onclick="document.getElementById('depoimentos').scrollIntoView()">Avaliações</a>
      <a onclick="document.getElementById('contato').scrollIntoView()">Contato</a>
    </div>
    <div class="nav-right">
      <button class="admin-btn" id="adminBtn" onclick="openAdmin()">👑 Admin</button>
      <div class="user-chip" id="userChip" onclick="toggleUserMenu(event)">
        <span class="uname" id="userName">User</span>
        <div class="uavatar" id="userAvatar">U</div>
        <div class="user-menu" id="userMenu">
          <div class="user-info-head">
            <div id="menuName">Usuário</div>
            <div class="em" id="menuEmail">email@email.com</div>
            <div class="role user" id="menuRole">CLIENTE</div>
          </div>
          <a onclick="document.getElementById('produtos').scrollIntoView()">📦 Ver produtos</a>
          <a onclick="document.getElementById('contato').scrollIntoView()">📧 Contato</a>
          <div class="divider"></div>
          <button class="logout" onclick="logout()">🚪 Sair</button>
        </div>
      </div>
    </div>
  </div>
</nav>

<section class="hero">
  <div class="hero-inner">
    <div>
      <div class="hero-tag">
        <span class="pulse"></span>
        EDIÇÃO OFICIAL COPA DO MUNDO 2026
      </div>
      <h1 class="hero-title">
        Complete <em>sua coleção</em><br>
        da copa de <span class="accent">2026</span>
      </h1>
      <p class="hero-sub">
        Álbuns oficiais, pacotes de figurinhas, brilhantes e edições especiais da Copa do Mundo FIFA 2026. Pagamento via Pix com confirmação instantânea.
      </p>
      <div class="hero-actions">
        <button class="btn btn-primary" onclick="document.getElementById('produtos').scrollIntoView()">
          Ver Catálogo →
        </button>
      </div>
      <div class="hero-stats">
        <div class="stat">
          <div class="stat-num">670+</div>
          <div class="stat-label">Figurinhas no álbum</div>
        </div>
        <div class="stat">
          <div class="stat-num">48</div>
          <div class="stat-label">Seleções na Copa 2026</div>
        </div>
        <div class="stat">
          <div class="stat-num">+50mil</div>
          <div class="stat-label">Clientes felizes</div>
        </div>
      </div>
    </div>

    <div class="hero-visual">
      <div class="year-badge">★ 2026 ★</div>
      <img class="hero-album-img" src="https://images.unsplash.com/photo-1551038247-3d9af20df552?w=600&q=80" alt="Álbum Copa 2026" onerror="this.src='https://picsum.photos/seed/album2026/600/800'">
      <img class="float-img f1" src="https://images.unsplash.com/photo-1606925797300-0b35e9d1794e?w=300&q=80" alt="Figurinha" onerror="this.src='https://picsum.photos/seed/sticker1/300/400'">
      <img class="float-img f2" src="https://images.unsplash.com/photo-1574629810360-7efbbe195018?w=300&q=80" alt="Figurinha" onerror="this.src='https://picsum.photos/seed/sticker2/300/400'">
      <img class="float-img f3" src="https://images.unsplash.com/photo-1610201417828-29e25fe6c8a0?w=300&q=80" alt="Figurinha" onerror="this.src='https://picsum.photos/seed/sticker3/300/400'">
    </div>
  </div>
</section>

<div class="ticker">
  <div class="ticker-track">
    <span>★ STORE COPA 2026 ★ PIX INSTANTÂNEO <span class="dot"></span> ENVIO EM 24H <span class="dot"></span> PRODUTOS ORIGINAIS <span class="dot"></span> PAGAMENTO SEGURO PUSHIN PAY</span>
    <span>★ STORE COPA 2026 ★ PIX INSTANTÂNEO <span class="dot"></span> ENVIO EM 24H <span class="dot"></span> PRODUTOS ORIGINAIS <span class="dot"></span> PAGAMENTO SEGURO PUSHIN PAY</span>
  </div>
</div>

<section class="products" id="produtos">
  <div class="container">
    <div class="products-head">
      <div>
        <span class="eyebrow">Catálogo 2026</span>
        <h2 class="section-title">Produtos<br>em destaque</h2>
        <p class="section-sub">
          Tudo o que você precisa para montar a sua coleção da Copa do Mundo 2026. Pagamento 100% seguro via Pushin Pay.
        </p>
      </div>
      <div class="filters">
        <button class="chip active">Todos</button>
        <button class="chip">Álbuns</button>
        <button class="chip">Pacotes</button>
        <button class="chip">Caixas</button>
        <button class="chip">Especiais</button>
      </div>
    </div>
    <div class="grid" id="productGrid"></div>
  </div>
</section>

<section class="banner">
  <h2>Promoção <span class="hl">de Lançamento 2026</span></h2>
  <p>Aproveite descontos especiais por tempo limitado</p>
  <div class="countdown">
    <div class="cd-box"><div class="cd-num" id="cd-d">02</div><div class="cd-label">Dias</div></div>
    <div class="cd-box"><div class="cd-num" id="cd-h">14</div><div class="cd-label">Horas</div></div>
    <div class="cd-box"><div class="cd-num" id="cd-m">37</div><div class="cd-label">Min</div></div>
    <div class="cd-box"><div class="cd-num" id="cd-s">52</div><div class="cd-label">Seg</div></div>
  </div>
</section>

<section class="testimonials" id="depoimentos">
  <div class="container">
    <span class="eyebrow">Quem comprou aprova</span>
    <h2 class="section-title">O que dizem<br>nossos colecionadores</h2>
    <div class="test-grid">
      <div class="test-card">
        <div class="stars">★★★★★</div>
        <p class="test-text">Chegou rapidíssimo e tudo lacrado. O álbum capa dura da Copa 2026 é lindíssimo, vale muito o preço. Já estou pedindo mais pacotes!</p>
        <div class="test-author">
          <div class="avatar">RS</div>
          <div><div class="author-name">Rafael Souza</div><div class="author-loc">São Paulo, SP</div></div>
        </div>
      </div>
      <div class="test-card">
        <div class="stars">★★★★★</div>
        <p class="test-text">Comprei a caixa de 100 pacotes para o meu filho e foi um sucesso. Atendimento de primeira, pacotes originais, sem repetição absurda.</p>
        <div class="test-author">
          <div class="avatar">CM</div>
          <div><div class="author-name">Camila Mendes</div><div class="author-loc">Belo Horizonte, MG</div></div>
        </div>
      </div>
      <div class="test-card">
        <div class="stars">★★★★★</div>
        <p class="test-text">Achei as legendárias da Copa 2026 aqui por um preço justo. Pagamento via Pix foi instantâneo, super tranquilo. Recomendo!</p>
        <div class="test-author">
          <div class="avatar">JP</div>
          <div><div class="author-name">João Pedro</div><div class="author-loc">Curitiba, PR</div></div>
        </div>
      </div>
    </div>
  </div>
</section>

<footer id="contato">
  <div class="foot-grid">
    <div class="foot-col">
      <div class="brand" style="color:#fff">
        <div class="brand-mark">⚽</div>
        <div>STORE <span style="color:var(--gold)">COPA</span></div>
      </div>
      <p class="foot-brand-desc">
        Loja especializada em álbuns e figurinhas oficiais da Copa do Mundo 2026. Itens 100% originais, com envio para todo o Brasil.
      </p>
      <div class="pay">
        <span>Pix</span><span>Boleto</span>
      </div>
    </div>
    <div class="foot-col">
      <h4>Loja</h4>
      <ul>
        <li><a onclick="document.getElementById('produtos').scrollIntoView()">Álbuns</a></li>
        <li><a onclick="document.getElementById('produtos').scrollIntoView()">Pacotes</a></li>
        <li><a onclick="document.getElementById('produtos').scrollIntoView()">Caixas</a></li>
        <li><a onclick="document.getElementById('produtos').scrollIntoView()">Especiais</a></li>
      </ul>
    </div>
    <div class="foot-col">
      <h4>Ajuda</h4>
      <ul>
        <li>Como comprar</li>
        <li>Frete e envio</li>
        <li>Trocas</li>
        <li>Rastrear pedido</li>
      </ul>
    </div>
    <div class="foot-col">
      <h4>Contato</h4>
      <ul>
        <li>📧 contato@storecopa.com.br</li>
        <li>🕐 Seg a Sex, 9h–18h</li>
        <li>📍 Brasil</li>
      </ul>
    </div>
  </div>

  <div class="legal-notice">
    <b>Informação de pagamento:</b> A Store Copa utiliza a <b>PUSHIN PAY</b> como processadora de pagamentos.
    A PUSHIN PAY atua exclusivamente como gateway tecnológico para processamento de cobranças via Pix e boleto,
    não sendo responsável pela entrega, suporte, qualidade ou cumprimento de obrigações relacionadas aos produtos ofertados.
    Toda responsabilidade sobre os produtos vendidos é da Store Copa.
  </div>

  <div class="copy">
    © 2026 Store Copa · Todos os direitos reservados
  </div>
</footer>

</div> <!-- fim #mainSite -->

<div class="toast" id="toast"></div>

<script>
  // =====================================================================
  // 🔌 CONFIGURAÇÃO DO SUPABASE
  // =====================================================================
  const SUPABASE_URL = 'https://qgxrbwzrzmlshecfwont.supabase.co';
  const SUPABASE_KEY = 'sb_publishable_fXJVwfML2EEQ7mjpHLB4ig_Pa2Jgz9Z';
  const ADMIN_EMAIL  = 'davilettrari@gmail.com';

  const sb = window.supabase.createClient(SUPABASE_URL, SUPABASE_KEY, {
    auth: { persistSession: true, autoRefreshToken: true, storage: window.localStorage }
  });

  // =====================================================================
  // 📦 PRODUTOS PADRÃO (usados só se o banco estiver vazio)
  // =====================================================================
  const DEFAULT_PRODUCTS = [
    { name: 'Álbum Oficial Copa 2026 — Capa Dura', category: 'Álbuns', price: 59.90, old_price: 89.00, image_url: 'https://images.unsplash.com/photo-1551038247-3d9af20df552?w=500&q=80', pushin_pay_link: '', badge: 'Mais vendido', badge_class: 'gold' },
    { name: 'Box com 50 Pacotinhos (250 figurinhas)', category: 'Pacotes', price: 199.90, old_price: 249.00, image_url: 'https://images.unsplash.com/photo-1606925797300-0b35e9d1794e?w=500&q=80', pushin_pay_link: '', badge: 'Novo', badge_class: 'green' },
    { name: 'Caixa Premium 100 Pacotes', category: 'Caixas', price: 349.90, old_price: 499.00, image_url: 'https://images.unsplash.com/photo-1606925207923-c25c92be8fd0?w=500&q=80', pushin_pay_link: '', badge: '-30%', badge_class: '' },
    { name: 'Kit Figurinhas Legendárias (10un)', category: 'Especiais', price: 129.90, old_price: null, image_url: 'https://images.unsplash.com/photo-1574629810360-7efbbe195018?w=500&q=80', pushin_pay_link: '', badge: 'Limitado', badge_class: 'gold' },
    { name: 'Álbum Oficial 2026 — Versão Brochura', category: 'Álbuns', price: 39.90, old_price: null, image_url: 'https://images.unsplash.com/photo-1610201417828-29e25fe6c8a0?w=500&q=80', pushin_pay_link: '', badge: null, badge_class: '' },
    { name: 'Álbum + 30 Pacotes (Combo Coleção)', category: 'Combos', price: 159.90, old_price: 199.00, image_url: 'https://images.unsplash.com/photo-1614632537190-23e4b21e74dc?w=500&q=80', pushin_pay_link: '', badge: 'Combo', badge_class: 'green' },
    { name: 'Kit Iniciante — 10 Pacotinhos', category: 'Pacotes', price: 49.90, old_price: null, image_url: 'https://images.unsplash.com/photo-1551958219-acbc608c6377?w=500&q=80', pushin_pay_link: '', badge: null, badge_class: '' },
    { name: 'Kit Figurinhas Brilhantes (15un)', category: 'Especiais', price: 89.90, old_price: null, image_url: 'https://images.unsplash.com/photo-1577471488278-16eec37ffcc2?w=500&q=80', pushin_pay_link: '', badge: 'Top', badge_class: 'gold' }
  ];

  // =====================================================================
  // 🌐 ESTADO GLOBAL
  // =====================================================================
  let currentUser = null;     // { email, name, role, id }
  let PRODUCTS = [];          // produtos vindos do banco

  // =====================================================================
  // 🎨 HELPERS
  // =====================================================================
  const $ = id => document.getElementById(id);

  function showToast(msg, type='') {
    const t = $('toast');
    t.textContent = msg;
    t.className = 'toast show' + (type ? ' ' + type : '');
    setTimeout(() => t.classList.remove('show'), 3000);
  }
  function showError(msg) { $('formError').textContent = msg; $('formError').classList.add('show'); $('formSuccess').classList.remove('show'); }
  function showSuccess(msg) { $('formSuccess').textContent = msg; $('formSuccess').classList.add('show'); $('formError').classList.remove('show'); }
  function clearMessages() { $('formError').classList.remove('show'); $('formSuccess').classList.remove('show'); }

  function switchTab(tab) {
    clearMessages();
    if (tab === 'login') {
      $('tabLogin').classList.add('active'); $('tabRegister').classList.remove('active');
      $('panelLogin').classList.add('active'); $('panelRegister').classList.remove('active');
      $('gateTitle').textContent = 'Faça login para entrar';
      $('gateSub').textContent = 'Acesso restrito a usuários cadastrados';
    } else {
      $('tabRegister').classList.add('active'); $('tabLogin').classList.remove('active');
      $('panelRegister').classList.add('active'); $('panelLogin').classList.remove('active');
      $('gateTitle').textContent = 'Crie sua conta';
      $('gateSub').textContent = 'É rápido e grátis. Comece sua coleção!';
    }
  }

  function getBrowser() {
    const ua = navigator.userAgent;
    if (ua.includes('Edg/')) return 'Edge';
    if (ua.includes('Chrome')) return 'Chrome';
    if (ua.includes('Firefox')) return 'Firefox';
    if (ua.includes('Safari')) return 'Safari';
    return 'Outro';
  }

  // =====================================================================
  // 📜 LOGS (no Supabase)
  // =====================================================================
  async function logEvent(type, message, userEmail = null) {
    try {
      await sb.from('logs').insert({
        log_type: type,
        message: message,
        user_email: userEmail || (currentUser ? currentUser.email : 'anônimo'),
        device: /Mobi|Android/i.test(navigator.userAgent) ? 'Mobile' : 'Desktop',
        browser: getBrowser()
      });
    } catch (e) { console.warn('log falhou:', e); }
  }

  // =====================================================================
  // 🔐 AUTENTICAÇÃO (Supabase Auth)
  // =====================================================================
  async function doLogin() {
    const email = $('loginEmail').value.trim().toLowerCase();
    const pwd = $('loginPassword').value;
    if (!email || !pwd) { showError('Preencha e-mail e senha.'); return; }

    const { data, error } = await sb.auth.signInWithPassword({ email, password: pwd });

    if (error) {
      logEvent('fail', `❌ Falha no login: ${error.message}`, email);
      if (error.message.includes('Invalid login')) showError('E-mail ou senha incorretos.');
      else showError(error.message);
      return;
    }

    // Pega o usuário e checa se é admin
    const user = data.user;
    const meta = user.user_metadata || {};
    const isAdmin = meta.role === 'admin' || email === ADMIN_EMAIL;

    currentUser = {
      id: user.id,
      email: user.email,
      name: meta.name || user.email.split('@')[0],
      role: isAdmin ? 'admin' : 'user'
    };

    await logEvent(isAdmin ? 'admin' : 'login', isAdmin ? '👑 Login do administrador' : '✅ Login realizado', email);
    showSuccess('Login feito com sucesso!');
    setTimeout(() => enterSite(), 400);
  }

  async function doRegister() {
    const name = $('regName').value.trim();
    const email = $('regEmail').value.trim().toLowerCase();
    const pwd = $('regPassword').value;
    const pwd2 = $('regPassword2').value;

    if (!name || !email || !pwd || !pwd2) { showError('Preencha todos os campos.'); return; }
    if (!email.includes('@') || !email.includes('.')) { showError('E-mail inválido.'); return; }
    if (pwd.length < 6) { showError('A senha precisa de pelo menos 6 caracteres.'); return; }
    if (pwd !== pwd2) { showError('As senhas não coincidem.'); return; }
    if (email === ADMIN_EMAIL) { showError('Este e-mail é reservado.'); return; }

    const { data, error } = await sb.auth.signUp({
      email,
      password: pwd,
      options: { data: { name, role: 'user' } }
    });

    if (error) {
      logEvent('fail', `❌ Falha no cadastro: ${error.message}`, email);
      if (error.message.includes('already')) showError('Este e-mail já está cadastrado.');
      else showError(error.message);
      return;
    }

    // Se confirmação de email estiver desligada, já loga
    if (data.user && data.session) {
      currentUser = { id: data.user.id, email, name, role: 'user' };
      await logEvent('register', `🆕 Nova conta criada: ${name}`, email);
      showSuccess('Conta criada com sucesso!');
      setTimeout(() => enterSite(), 400);
    } else {
      showSuccess('Conta criada! Verifique seu e-mail para confirmar (se necessário) e depois faça login.');
      setTimeout(() => switchTab('login'), 1500);
    }
  }

  async function logout() {
    if (currentUser) await logEvent('logout', '🚪 Logout', currentUser.email);
    await sb.auth.signOut();
    currentUser = null;
    closeAdmin();
    $('mainSite').style.display = 'none';
    $('gate').classList.remove('hide');
    document.body.classList.add('locked');
    ['loginEmail','loginPassword','regName','regEmail','regPassword','regPassword2'].forEach(id => { if($(id)) $(id).value = ''; });
    switchTab('login');
    clearMessages();
  }

  // =====================================================================
  // 🚀 ENTRAR NO SITE
  // =====================================================================
  async function enterSite() {
    $('gate').classList.add('hide');
    $('mainSite').style.display = '';
    document.body.classList.remove('locked');
    updateUI();
    await loadProducts();
    renderProducts();
    showToast(`Olá, ${currentUser.name.split(' ')[0]}! Bem-vindo${currentUser.role === 'admin' ? ' Admin 👑' : ''} à Store Copa ⚽`);
  }

  function updateUI() {
    if (!currentUser) return;
    const firstName = currentUser.name.split(' ')[0];
    const initials = currentUser.name.split(' ').slice(0, 2).map(n => n[0]).join('').toUpperCase();
    $('userName').textContent = firstName;
    $('userAvatar').textContent = initials;
    $('menuName').textContent = currentUser.name;
    $('menuEmail').textContent = currentUser.email;

    if (currentUser.role === 'admin') {
      $('userChip').classList.add('is-admin');
      $('adminBtn').classList.add('show');
      $('menuRole').textContent = '👑 ADMIN MASTER';
      $('menuRole').className = 'role';
    } else {
      $('userChip').classList.remove('is-admin');
      $('adminBtn').classList.remove('show');
      $('menuRole').textContent = 'CLIENTE';
      $('menuRole').className = 'role user';
    }
  }

  function toggleUserMenu(e) { e.stopPropagation(); $('userMenu').classList.toggle('show'); }
  document.addEventListener('click', e => { if (!e.target.closest('#userChip')) $('userMenu').classList.remove('show'); });

  // =====================================================================
  // 📦 PRODUTOS (do Supabase)
  // =====================================================================
  async function loadProducts() {
    const { data, error } = await sb.from('products').select('*').order('id', { ascending: true });
    if (error) { console.error('Erro carregando produtos:', error); PRODUCTS = []; return; }

    // Se banco vazio e usuário é admin, popula com os padrões automaticamente
    if (data.length === 0 && currentUser && currentUser.role === 'admin') {
      const { data: inserted } = await sb.from('products').insert(DEFAULT_PRODUCTS).select();
      PRODUCTS = inserted || [];
    } else {
      PRODUCTS = data;
    }
  }

  function renderProducts() {
    const grid = $('productGrid');
    if (!grid) return;
    if (PRODUCTS.length === 0) {
      grid.innerHTML = '<p style="grid-column:1/-1;text-align:center;padding:40px;color:#666">Nenhum produto cadastrado ainda. ' + (currentUser && currentUser.role === 'admin' ? 'Vá no painel admin e crie produtos!' : 'Volte em breve.') + '</p>';
      return;
    }
    grid.innerHTML = PRODUCTS.map(p => {
      const link = p.pushin_pay_link;
      const isLinkSet = link && link.length > 5 && !link.startsWith('COLE_AQUI');
      const safeName = (p.name || '').replace(/'/g, '');
      const btn = isLinkSet
        ? `<a class="buy-btn" href="${link}" target="_blank" rel="noopener" onclick="trackBuy(${p.id}, '${safeName}')">💸 Comprar com Pix</a>`
        : `<button class="buy-btn disabled" onclick="alertNoLink()">⏳ Em breve</button>`;
      return `
        <article class="card">
          ${p.badge ? `<span class="badge ${p.badge_class||''}">${p.badge}</span>` : ''}
          <span class="badge-2026">★ 2026</span>
          <div class="card-img-wrap">
            <img src="${p.image_url}" alt="${p.name}" onerror="this.src='https://picsum.photos/seed/p${p.id}/500/500'">
          </div>
          <div class="card-body">
            <span class="card-cat">${p.category||''}</span>
            <h3 class="card-name">${p.name}</h3>
            <div class="price">${p.old_price ? `<span class="old">R$${Number(p.old_price).toFixed(0)}</span>` : ''}R$${Number(p.price).toFixed(2).replace('.', ',')}</div>
            ${btn}
          </div>
        </article>`;
    }).join('');
  }

  async function trackBuy(id, name) {
    await logEvent('buy', `🛒 Clique em comprar: "${name}" (ID #${id})`, currentUser.email);
    showToast('Redirecionando para pagamento seguro...');
  }

  function alertNoLink() {
    showToast('⚠️ O lojista ainda não cadastrou o link da Pushin Pay.', 'error');
  }

  // =====================================================================
  // 👑 PAINEL ADMIN
  // =====================================================================
  async function openAdmin() {
    if (!currentUser || currentUser.role !== 'admin') { showToast('Acesso negado.', 'error'); return; }
    await logEvent('admin', '👑 Admin abriu o painel');
    $('adminPanel').classList.add('show');
    document.body.classList.add('modal-open');
    $('userMenu').classList.remove('show');
    refreshAdminData();
  }
  function closeAdmin() { $('adminPanel').classList.remove('show'); document.body.classList.remove('modal-open'); }
  function switchAdminSection(section, btn) {
    document.querySelectorAll('.adm-nav').forEach(n => n.classList.remove('active'));
    btn.classList.add('active');
    document.querySelectorAll('.adm-section').forEach(s => s.classList.remove('active'));
    $('sec-' + section).classList.add('active');
    refreshAdminData();
  }

  async function refreshAdminData() {
    await renderKPIs();
    await renderTopProducts();
    await renderRecentLogs();
    await renderUsers();
    await renderLogs();
    renderProductsAdmin();
  }

  // ============== DASHBOARD ==============
  async function renderKPIs() {
    const [{ count: usersCount }, { data: logs }, { count: prodCount }] = await Promise.all([
      sb.from('user_profiles').select('*', { count: 'exact', head: true }).then(r => r.error ? { count: '—' } : r),
      sb.from('logs').select('log_type, message, created_at'),
      sb.from('products').select('*', { count: 'exact', head: true })
    ]);

    const totalBuys = (logs||[]).filter(l => l.log_type === 'buy').length;
    const totalLogins = (logs||[]).filter(l => l.log_type === 'login').length;
    const totalFails = (logs||[]).filter(l => l.log_type === 'fail').length;
    const today = new Date().toISOString().split('T')[0];
    const todayLogs = (logs||[]).filter(l => l.created_at && l.created_at.startsWith(today)).length;

    let estRevenue = 0;
    (logs||[]).filter(l => l.log_type === 'buy').forEach(l => {
      const m = l.message.match(/#(\d+)/);
      if (m) {
        const prod = PRODUCTS.find(p => p.id === parseInt(m[1]));
        if (prod) estRevenue += Number(prod.price);
      }
    });

    $('kpiGrid').innerHTML = `
      <div class="kpi"><div class="kpi-label">Usuários cadastrados</div><div class="kpi-value">${usersCount ?? '—'}</div><div class="kpi-meta">no banco Supabase</div></div>
      <div class="kpi green"><div class="kpi-label">Total de logins</div><div class="kpi-value">${totalLogins}</div><div class="kpi-meta">desde o início</div></div>
      <div class="kpi gold"><div class="kpi-label">Cliques em comprar</div><div class="kpi-value">${totalBuys}</div><div class="kpi-meta">intenções de compra</div></div>
      <div class="kpi green"><div class="kpi-label">Receita potencial</div><div class="kpi-value">R$ ${estRevenue.toFixed(2).replace('.',',')}</div><div class="kpi-meta">soma dos cliques</div></div>
      <div class="kpi red"><div class="kpi-label">Tentativas falhas</div><div class="kpi-value">${totalFails}</div><div class="kpi-meta">logins errados</div></div>
      <div class="kpi"><div class="kpi-label">Atividade hoje</div><div class="kpi-value">${todayLogs}</div><div class="kpi-meta">eventos registrados</div></div>
    `;
  }

  async function renderTopProducts() {
    const { data: logs } = await sb.from('logs').select('message').eq('log_type', 'buy');
    const counts = {};
    (logs||[]).forEach(l => {
      const m = l.message.match(/#(\d+)/);
      if (m) { const id = parseInt(m[1]); counts[id] = (counts[id]||0) + 1; }
    });
    const ranked = Object.entries(counts)
      .map(([id, c]) => ({ id: parseInt(id), clicks: c, prod: PRODUCTS.find(p => p.id === parseInt(id)) }))
      .filter(x => x.prod)
      .sort((a, b) => b.clicks - a.clicks)
      .slice(0, 5);

    const tbody = $('topProductsTable').querySelector('tbody');
    if (ranked.length === 0) {
      tbody.innerHTML = `<tr><td colspan="4"><div class="empty-state"><div class="empty-state-icon">📊</div>Nenhum clique em comprar ainda.</div></td></tr>`;
      return;
    }
    tbody.innerHTML = ranked.map((r, i) => `
      <tr>
        <td class="mono">${i+1}º</td>
        <td><b>${r.prod.name}</b></td>
        <td class="mono">${r.clicks}</td>
        <td class="mono">R$ ${(r.clicks * Number(r.prod.price)).toFixed(2).replace('.',',')}</td>
      </tr>`).join('');
  }

  async function renderRecentLogs() {
    const { data: logs } = await sb.from('logs').select('*').order('created_at', { ascending: false }).limit(10);
    const tbody = $('recentLogsTable').querySelector('tbody');
    if (!logs || logs.length === 0) { tbody.innerHTML = `<tr><td colspan="4"><div class="empty-state"><div class="empty-state-icon">📭</div>Nenhuma atividade ainda.</div></td></tr>`; return; }
    tbody.innerHTML = logs.map(l => `
      <tr><td class="mono">${fmtDate(l.created_at)}</td><td><span class="log-pill ${l.log_type}">${l.log_type}</span></td><td class="mono">${l.user_email||'—'}</td><td>${l.message}</td></tr>
    `).join('');
  }

  // ============== USUÁRIOS ==============
  async function renderUsers() {
    const search = $('userSearch') ? $('userSearch').value.toLowerCase() : '';
    const tbody = $('usersTable').querySelector('tbody');

    const { data, error } = await sb.from('user_profiles').select('*').order('created_at', { ascending: false });
    if (error) {
      tbody.innerHTML = `<tr><td colspan="6"><div class="empty-state"><div class="empty-state-icon">⚠️</div>
        <b>Tabela <code>user_profiles</code> não existe ainda.</b><br><br>
        Pra ver os usuários aqui, você precisa rodar 1 SQL extra no Supabase (cria a tabela + trigger que copia os usuários automaticamente).<br><br>
        Ver o guia <code>guia-banco-de-dados.md</code> seção "SQL extra opcional".
      </div></td></tr>`;
      return;
    }

    const filtered = data.filter(u => !search || (u.name||'').toLowerCase().includes(search) || (u.email||'').toLowerCase().includes(search));
    if (filtered.length === 0) { tbody.innerHTML = `<tr><td colspan="6"><div class="empty-state"><div class="empty-state-icon">🔍</div>Nenhum usuário encontrado.</div></td></tr>`; return; }

    tbody.innerHTML = filtered.map(u => {
      const isAdmin = u.role === 'admin' || u.email === ADMIN_EMAIL;
      return `
      <tr>
        <td><b>${u.name||u.email.split('@')[0]}</b></td>
        <td class="mono">${u.email}</td>
        <td><span class="role-pill ${isAdmin?'admin':'user'}">${isAdmin?'👑 ADMIN':'CLIENTE'}</span></td>
        <td class="mono">${fmtDate(u.created_at)}</td>
        <td class="mono">${u.last_login?fmtDate(u.last_login):'—'}</td>
        <td>${isAdmin?'<span style="color:var(--muted);font-size:11px">Protegido</span>':'<span style="color:var(--muted);font-size:11px">via Supabase</span>'}</td>
      </tr>`;
    }).join('');
  }

  // ============== LOGS ==============
  async function renderLogs() {
    const search = $('logSearch') ? $('logSearch').value.toLowerCase() : '';
    const filter = $('logFilter') ? $('logFilter').value : '';
    const tbody = $('logsTable').querySelector('tbody');

    let query = sb.from('logs').select('*').order('created_at', { ascending: false }).limit(500);
    if (filter) query = query.eq('log_type', filter);
    const { data: logs } = await query;

    const filtered = (logs||[]).filter(l => {
      if (search && !(l.message||'').toLowerCase().includes(search) && !(l.user_email||'').toLowerCase().includes(search) && !(l.log_type||'').toLowerCase().includes(search)) return false;
      return true;
    });

    if (filtered.length === 0) { tbody.innerHTML = `<tr><td colspan="5"><div class="empty-state"><div class="empty-state-icon">📭</div>Nenhum log encontrado.</div></td></tr>`; return; }
    tbody.innerHTML = filtered.map(l => `
      <tr><td class="mono">${fmtDate(l.created_at)}</td><td><span class="log-pill ${l.log_type}">${l.log_type}</span></td><td class="mono">${l.user_email||'—'}</td><td>${l.message}</td><td class="mono" style="color:var(--muted)">${l.device||'—'} · ${l.browser||'—'}</td></tr>
    `).join('');
  }

  async function clearLogs() {
    if (!confirm('Limpar TODOS os logs? Esta ação é irreversível.')) return;
    const { error } = await sb.from('logs').delete().neq('id', 0);
    if (error) { showToast('Erro ao limpar: ' + error.message, 'error'); return; }
    await logEvent('admin', '🗑️ Admin limpou todos os logs');
    showToast('Logs limpos.');
    refreshAdminData();
  }

  // ============== PRODUTOS (CRUD) ==============
  function renderProductsAdmin() {
    const tbody = $('productsTable').querySelector('tbody');
    if (PRODUCTS.length === 0) { tbody.innerHTML = `<tr><td colspan="6"><div class="empty-state"><div class="empty-state-icon">📦</div>Nenhum produto cadastrado. Clique em "+ Novo produto".</div></td></tr>`; return; }
    tbody.innerHTML = PRODUCTS.map(p => {
      const link = p.pushin_pay_link;
      const linkOk = link && link.length > 5 && !link.startsWith('COLE_AQUI');
      return `
        <tr>
          <td class="mono">#${p.id}</td>
          <td><b>${p.name}</b></td>
          <td>${p.category||'—'}</td>
          <td class="mono">R$ ${Number(p.price).toFixed(2).replace('.',',')}</td>
          <td>${linkOk?'<span style="color:var(--green-deep);font-weight:700">✓ Configurado</span>':'<span style="color:var(--red);font-weight:700">⚠️ Falta link</span>'}</td>
          <td>
            <button class="adm-btn-edit" onclick="openProductModal(${p.id})">Editar</button>
            <button class="adm-btn-danger" onclick="deleteProduct(${p.id})">Excluir</button>
          </td>
        </tr>`;
    }).join('');
  }

  function openProductModal(id) {
    const isNew = !id;
    $('prodModalTitle').textContent = isNew ? '✨ Novo produto' : '✏️ Editar produto';
    const p = isNew ? { id:'', name:'', category:'', price:'', old_price:'', image_url:'', pushin_pay_link:'', badge:'' } : PRODUCTS.find(x => x.id === id);
    if (!p) return;
    $('prodId').value = p.id || '';
    $('prodName').value = p.name || '';
    $('prodCat').value = p.category || '';
    $('prodPrice').value = p.price || '';
    $('prodOldPrice').value = p.old_price || '';
    $('prodImg').value = p.image_url || '';
    $('prodLink').value = p.pushin_pay_link || '';
    $('prodBadge').value = p.badge || '';
    $('prodModal').classList.add('show');
  }

  function closeProductModal() { $('prodModal').classList.remove('show'); }

  async function saveProduct() {
    const id = $('prodId').value;
    const name = $('prodName').value.trim();
    const category = $('prodCat').value.trim();
    const price = parseFloat($('prodPrice').value);
    const oldPrice = $('prodOldPrice').value ? parseFloat($('prodOldPrice').value) : null;
    const image_url = $('prodImg').value.trim();
    const pushin_pay_link = $('prodLink').value.trim();
    const badge = $('prodBadge').value.trim() || null;

    if (!name) return showToast('Nome é obrigatório.', 'error');
    if (isNaN(price) || price < 0) return showToast('Preço inválido.', 'error');
    if (!image_url) return showToast('URL da imagem é obrigatória.', 'error');

    const badge_class = badge ? (badge.toLowerCase().includes('vendido')||badge.toLowerCase().includes('top')||badge.toLowerCase().includes('limit')?'gold':badge.toLowerCase().includes('novo')||badge.toLowerCase().includes('combo')?'green':'') : '';

    const payload = { name, category, price, old_price: oldPrice, image_url, pushin_pay_link, badge, badge_class, updated_at: new Date().toISOString() };

    if (id) {
      const { error } = await sb.from('products').update(payload).eq('id', parseInt(id));
      if (error) return showToast('Erro: '+error.message, 'error');
      await logEvent('admin', `✏️ Produto editado: ${name} (#${id})`);
    } else {
      const { error } = await sb.from('products').insert(payload);
      if (error) return showToast('Erro: '+error.message, 'error');
      await logEvent('admin', `✨ Novo produto criado: ${name}`);
    }

    await loadProducts();
    closeProductModal();
    showToast('Produto salvo!');
    refreshAdminData();
    renderProducts();
  }

  async function deleteProduct(id) {
    const p = PRODUCTS.find(x => x.id === id);
    if (!p) return;
    if (!confirm(`Excluir o produto "${p.name}"? Esta ação é irreversível.`)) return;
    const { error } = await sb.from('products').delete().eq('id', id);
    if (error) return showToast('Erro: '+error.message, 'error');
    await logEvent('admin', `🗑️ Produto excluído: ${p.name} (#${id})`);
    await loadProducts();
    showToast('Produto excluído.');
    refreshAdminData();
    renderProducts();
  }

  // ============== TROCAR SENHA DO ADMIN ==============
  async function changeAdminPassword() {
    const n1 = $('newPwd').value;
    const n2 = $('newPwd2').value;
    const msg = $('pwdMsg');
    msg.style.display = 'block';

    if (!n1 || !n2) { msg.style.color = 'var(--red)'; msg.textContent = 'Preencha os dois campos de nova senha.'; return; }
    if (n1.length < 6) { msg.style.color = 'var(--red)'; msg.textContent = 'A nova senha precisa de pelo menos 6 caracteres.'; return; }
    if (n1 !== n2) { msg.style.color = 'var(--red)'; msg.textContent = 'As novas senhas não coincidem.'; return; }

    const { error } = await sb.auth.updateUser({ password: n1 });
    if (error) { msg.style.color = 'var(--red)'; msg.textContent = 'Erro: ' + error.message; return; }

    await logEvent('admin', '🔐 Senha do administrador alterada');
    msg.style.color = 'var(--green-deep)'; msg.textContent = '✅ Senha alterada com sucesso!';
    $('curPwd').value = ''; $('newPwd').value = ''; $('newPwd2').value = '';
    showToast('Senha do admin alterada.');
  }

  // ============== BACKUP / RESTORE ==============
  async function exportBackup() {
    const [{ data: products }, { data: logs }, { data: users }] = await Promise.all([
      sb.from('products').select('*'),
      sb.from('logs').select('*'),
      sb.from('user_profiles').select('*').then(r => r.error ? { data: [] } : r)
    ]);
    const data = { version: '3.0-supabase', exportedAt: new Date().toISOString(), products, logs, users };
    const blob = new Blob([JSON.stringify(data, null, 2)], { type: 'application/json' });
    const url = URL.createObjectURL(blob);
    const a = document.createElement('a');
    a.href = url;
    a.download = `store-copa-backup-${new Date().toISOString().split('T')[0]}.json`;
    a.click();
    URL.revokeObjectURL(url);
    await logEvent('admin', '📥 Backup exportado');
    showToast('Backup baixado!');
  }

  function importBackup(event) {
    showToast('No modo Supabase, restauração precisa ser feita via SQL Editor. Veja o guia.', 'error');
    event.target.value = '';
  }

  async function resetAllData() {
    if (!confirm('⚠️ ATENÇÃO: Isso vai apagar TODOS os produtos e logs do banco. Tem certeza?')) return;
    if (!confirm('Confirma DE NOVO? Esta ação é IRREVERSÍVEL.')) return;
    await sb.from('logs').delete().neq('id', 0);
    await sb.from('products').delete().neq('id', 0);
    await logEvent('admin', '💥 Admin resetou produtos e logs');
    showToast('Dados resetados.');
    await loadProducts();
    refreshAdminData();
    renderProducts();
  }

  // =====================================================================
  // 🛠️ UTILS
  // =====================================================================
  function fmtDate(iso) {
    if (!iso) return '—';
    const d = new Date(iso);
    const pad = n => String(n).padStart(2, '0');
    return `${pad(d.getDate())}/${pad(d.getMonth()+1)}/${d.getFullYear()} ${pad(d.getHours())}:${pad(d.getMinutes())}`;
  }

  // Countdown
  let cd_d=2, cd_h=14, cd_m=37, cd_s=52;
  const pad = n => String(n).padStart(2,'0');
  setInterval(()=>{
    cd_s--;
    if(cd_s<0){ cd_s=59; cd_m--; }
    if(cd_m<0){ cd_m=59; cd_h--; }
    if(cd_h<0){ cd_h=23; cd_d--; }
    if(cd_d<0){ cd_d=0; cd_h=0; cd_m=0; cd_s=0; }
    if($('cd-d')) { $('cd-d').textContent=pad(cd_d); $('cd-h').textContent=pad(cd_h); $('cd-m').textContent=pad(cd_m); $('cd-s').textContent=pad(cd_s); }
  }, 1000);

  document.addEventListener('click', e => {
    if (e.target.classList.contains('chip')) {
      document.querySelectorAll('.chip').forEach(c => c.classList.remove('active'));
      e.target.classList.add('active');
    }
  });

  document.addEventListener('keydown', e => {
    if (e.key === 'Enter' && !$('gate').classList.contains('hide')) {
      if ($('panelLogin').classList.contains('active')) doLogin();
      else doRegister();
    }
    if (e.key === 'Escape') { closeProductModal(); }
  });

  document.addEventListener('click', e => {
    if (e.target.id === 'prodModal') closeProductModal();
  });

  // =====================================================================
  // 🚀 INIT
  // =====================================================================
  (async function init() {
    document.body.classList.add('locked');

    // Verifica se já tem sessão ativa no Supabase
    const { data: { session } } = await sb.auth.getSession();

    if (session && session.user) {
      const user = session.user;
      const meta = user.user_metadata || {};
      const isAdmin = meta.role === 'admin' || user.email === ADMIN_EMAIL;
      currentUser = {
        id: user.id,
        email: user.email,
        name: meta.name || user.email.split('@')[0],
        role: isAdmin ? 'admin' : 'user'
      };
      // Esconde splash, mostra site
      $('splash').style.display = 'none';
      await enterSite();
    } else {
      // Sem sessão → esconde splash, mostra gate
      $('splash').style.display = 'none';
      $('gate').classList.remove('hide');
    }

    // Listener pra mudanças de auth (caso o token expire)
    sb.auth.onAuthStateChange((event, session) => {
      if (event === 'SIGNED_OUT' && currentUser) {
        currentUser = null;
        $('mainSite').style.display = 'none';
        closeAdmin();
        $('gate').classList.remove('hide');
        document.body.classList.add('locked');
      }
    });
  })();
</script>
</body>
</html>
