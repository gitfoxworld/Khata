<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Khata — Shop Ledger & GST</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Fraunces:opsz,wght@9..144,400;9..144,500;9..144,600;9..144,700&family=Inter:wght@400;500;600;700&family=IBM+Plex+Mono:wght@400;500;600&display=swap" rel="stylesheet">
<style>
  :root{
    --ink:#1B2420;
    --ink-soft:#4A544D;
    --paper:#F3F1E4;
    --paper-raised:#FBFAF3;
    --green:#1F3D34;
    --green-light:#2F5347;
    --green-pale:#E4EAE4;
    --brass:#A8813C;
    --brass-light:#C9A15C;
    --red:#8B3A3A;
    --red-pale:#F3E3DE;
    --line:#DAD5C3;
    --shadow: 0 1px 2px rgba(27,36,32,0.06), 0 4px 16px rgba(27,36,32,0.06);
  }
  *{box-sizing:border-box;}
  html,body{margin:0;padding:0;}
  body{
    background:var(--paper);
    color:var(--ink);
    font-family:'Inter',sans-serif;
    min-height:100vh;
  }
  .storage-warning{
    background:var(--red);
    color:#fff;
    text-align:center;
    font-size:13px;
    font-weight:600;
    padding:9px 16px;
  }
  h1,h2,h3,.display{
    font-family:'Fraunces',serif;
    font-weight:600;
    letter-spacing:-0.01em;
  }
  .mono{font-family:'IBM Plex Mono',monospace;}
  a{color:inherit;}
  button{font-family:inherit;cursor:pointer;}
  input,select,textarea{font-family:'Inter',sans-serif;}

  /* ---------- App shell ---------- */
  .app{display:flex;min-height:100vh;}

  .sidebar{
    width:220px;
    flex-shrink:0;
    background:var(--green);
    color:#EDEAD9;
    display:flex;
    flex-direction:column;
    padding:24px 0;
  }
  .brand{
    display:flex;
    align-items:center;
    gap:10px;
    padding:0 20px 22px 20px;
    border-bottom:1px solid rgba(237,234,217,0.15);
    margin-bottom:14px;
  }
  .brand-mark{
    width:34px;height:34px;border-radius:50%;
    background:var(--brass);
    display:flex;align-items:center;justify-content:center;
    font-family:'Fraunces',serif;font-weight:700;color:var(--green);
    font-size:16px;flex-shrink:0;
  }
  .brand-name{font-family:'Fraunces',serif;font-size:19px;font-weight:600;line-height:1.1;}
  .brand-sub{font-size:10.5px;color:#B9C4BB;text-transform:uppercase;letter-spacing:0.08em;margin-top:2px;}

  .nav{display:flex;flex-direction:column;gap:2px;padding:0 10px;}
  .nav-item{
    display:flex;align-items:center;gap:11px;
    padding:10px 12px;border-radius:7px;
    font-size:14px;font-weight:500;
    color:#D7DFD4;
    background:transparent;border:none;
    text-align:left;width:100%;
    transition:background .15s ease, color .15s ease;
  }
  .nav-item svg{width:17px;height:17px;flex-shrink:0;opacity:.85;}
  .nav-item:hover{background:rgba(237,234,217,0.08);color:#fff;}
  .nav-item.active{background:var(--brass);color:var(--green);font-weight:600;}
  .nav-item.active svg{opacity:1;}

  .sidebar-foot{
    margin-top:auto;padding:16px 20px 0 20px;
    font-size:11px;color:#8FA090;
    border-top:1px solid rgba(237,234,217,0.12);
    padding-top:14px;
  }
  .logout-btn{
    display:flex;align-items:center;gap:8px;width:100%;margin-top:10px;
    padding:9px 12px;background:transparent;border:1px solid rgba(237,234,217,0.25);
    border-radius:8px;color:#EDEAD9;font-family:'Inter',sans-serif;font-size:12.5px;
    font-weight:600;cursor:pointer;transition:background .15s ease;
  }
  .logout-btn:hover{background:rgba(237,234,217,0.1);}
  .logout-btn svg{width:15px;height:15px;flex-shrink:0;}

  /* ---------- Auth (Login / Register) ---------- */
  .auth-screen{
    position:fixed;inset:0;z-index:500;
    background:var(--green);
    background-image:radial-gradient(circle at 20% 20%, rgba(255,255,255,0.04), transparent 40%),
                      radial-gradient(circle at 80% 80%, rgba(255,255,255,0.03), transparent 40%);
    display:flex;align-items:center;justify-content:center;
    padding:24px;overflow-y:auto;
  }
  .auth-card{
    background:var(--paper-raised);
    border-radius:16px;
    box-shadow:0 24px 70px rgba(0,0,0,0.35);
    padding:36px 38px 30px 38px;
    width:100%;max-width:420px;
  }
  .auth-brand{display:flex;align-items:center;gap:11px;margin-bottom:22px;}
  .auth-brand .brand-mark{
    width:38px;height:38px;border-radius:9px;background:var(--brass);
    color:var(--green);display:flex;align-items:center;justify-content:center;
    font-family:'Fraunces',serif;font-weight:700;font-size:19px;flex-shrink:0;
  }
  .auth-brand .brand-name{font-family:'Fraunces',serif;font-weight:700;font-size:19px;color:var(--ink);line-height:1.1;}
  .auth-brand .brand-sub{font-size:11.5px;color:var(--ink-soft);margin-top:1px;}
  .auth-title{font-family:'Fraunces',serif;font-size:23px;font-weight:600;margin:0 0 4px 0;color:var(--ink);}
  .auth-sub{font-size:13.5px;color:var(--ink-soft);margin-bottom:22px;line-height:1.5;}
  .auth-btn{width:100%;justify-content:center;margin-top:6px;padding:12px;font-size:14.5px;}
  .auth-remember{
    display:flex;align-items:center;gap:8px;font-size:13px;color:var(--ink-soft);
    margin:2px 0 6px 0;cursor:pointer;user-select:none;
  }
  .auth-remember input{width:auto;margin:0;accent-color:var(--brass);}
  .auth-error{
    color:var(--red);font-size:12.5px;font-weight:600;min-height:0;margin-bottom:2px;
  }
  .auth-error:not(:empty){margin-bottom:10px;}
  .required-star{color:var(--red);margin-left:2px;}
  .field.input-error input{border-color:var(--red);background:rgba(139,58,58,0.05);}
  .field.input-error label{color:var(--red);}
  .auth-note{font-size:11.5px;color:var(--ink-soft);line-height:1.6;margin-top:18px;text-align:center;}
  .auth-note a{color:var(--brass);font-weight:600;text-decoration:none;}
  .auth-note a:hover{text-decoration:underline;}
  .auth-gate-btns{display:flex;gap:10px;flex-wrap:wrap;margin-top:4px;}
  .auth-gate-btn{flex:1;min-width:150px;justify-content:center;}
  @media(max-width:480px){
    .auth-card{padding:28px 22px 24px 22px;}
  }

  .main{flex:1;min-width:0;padding:32px 40px 60px 40px;}
  .page{display:none;animation:fade .25s ease;}
  .page.active{display:block;}
  @keyframes fade{from{opacity:0;transform:translateY(4px);}to{opacity:1;transform:none;}}

  .page-head{display:flex;align-items:baseline;justify-content:space-between;margin-bottom:26px;flex-wrap:wrap;gap:12px;}
  .page-head h1{font-size:26px;margin:0;}
  .page-head .sub{color:var(--ink-soft);font-size:13.5px;margin-top:3px;}

  .btn{
    display:inline-flex;align-items:center;gap:7px;
    padding:10px 16px;border-radius:7px;border:none;
    font-size:13.5px;font-weight:600;
    background:var(--green);color:#fff;
    box-shadow:var(--shadow);
  }
  .btn:hover{background:var(--green-light);}
  .btn.secondary{background:var(--paper-raised);color:var(--ink);border:1px solid var(--line);box-shadow:none;}
  .btn.secondary:hover{border-color:var(--brass);}
  .btn.backup{background:var(--green);color:#fff;border:1px solid var(--brass-light);}
  .btn.backup:hover{background:var(--green-light);}
  .btn.ghost{background:transparent;color:var(--green);box-shadow:none;padding:8px 10px;}
  .btn.danger{background:var(--red-pale);color:var(--red);box-shadow:none;}
  .btn.gold{background:var(--brass);color:#fff;box-shadow:none;padding:8px 12px;}
  .btn.gold:hover{background:var(--brass-light);}
  .btn.red{background:var(--red);color:#fff;box-shadow:none;padding:8px 12px;}
  .btn.red:hover{background:#a24747;}
  .btn:disabled{opacity:.45;cursor:not-allowed;}
  .btn svg{width:15px;height:15px;}

  /* ---------- Cards / stats ---------- */
  .stat-grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(200px,1fr));gap:14px;margin-bottom:30px;}
  .stat-card{
    background:var(--paper-raised);border:1px solid var(--line);border-radius:10px;
    padding:18px 20px;box-shadow:var(--shadow);position:relative;overflow:hidden;
  }
  .stat-card .label{font-size:11.5px;text-transform:uppercase;letter-spacing:.07em;color:var(--ink-soft);font-weight:600;}
  .stat-card .value{font-family:'Fraunces',serif;font-size:26px;margin-top:6px;font-weight:600;}
  .stat-card .accent-bar{position:absolute;left:0;top:0;bottom:0;width:4px;background:var(--brass);}

  .card{
    background:var(--paper-raised);border:1px solid var(--line);border-radius:10px;
    padding:22px 24px;box-shadow:var(--shadow);margin-bottom:20px;
  }
  .card h3{font-size:16px;margin:0 0 14px 0;}

  table{width:100%;border-collapse:collapse;font-size:13.5px;}
  th{text-align:left;font-size:11px;text-transform:uppercase;letter-spacing:.06em;color:var(--ink-soft);
     padding:0 10px 10px 10px;border-bottom:1px solid var(--line);font-weight:600;}
  td{padding:11px 10px;border-bottom:1px solid var(--line);vertical-align:middle;}
  tr:last-child td{border-bottom:none;}
  tbody tr:hover{background:var(--green-pale);}
  .empty-row td{color:var(--ink-soft);text-align:center;padding:34px 10px;font-style:italic;}

  .badge{display:inline-block;padding:3px 9px;border-radius:20px;font-size:11px;font-weight:600;}
  .badge.paid{background:var(--green-pale);color:var(--green);}
  .badge.partial{background:#F1E2C4;color:#8A5A1D;}
  .badge.due{background:var(--red-pale);color:var(--red);}

  /* ---------- Forms ---------- */
  .form-grid{display:grid;grid-template-columns:1fr 1fr;gap:14px;}
  .field{display:flex;flex-direction:column;gap:5px;margin-bottom:14px;}
  .field label{font-size:12px;font-weight:600;color:var(--ink-soft);}
  .field input, .field select, .field textarea{
    width:100%;
    padding:9px 11px;border:1px solid var(--line);border-radius:7px;
    background:#fff;font-size:14px;color:var(--ink);outline:none;
  }
  .field input:focus, .field select:focus, .field textarea:focus{border-color:var(--brass);box-shadow:0 0 0 3px rgba(168,129,60,0.15);}
  .field.full{grid-column:1/-1;}

  .radio-row{display:flex;gap:10px;}
  .radio-opt{
    flex:1;border:1px solid var(--line);border-radius:8px;padding:11px 13px;
    display:flex;align-items:center;gap:9px;cursor:pointer;background:#fff;font-size:13.5px;
  }
  .radio-opt.selected{border-color:var(--brass);background:var(--green-pale);}
  .radio-opt input{margin:0;}

  /* ---------- Invoice item builder ---------- */
  .item-row{display:grid;grid-template-columns:2fr 60px 90px 92px 78px 30px;gap:8px;align-items:center;margin-bottom:8px;}
  .item-row input, .item-row select{padding:8px 9px;border:1px solid var(--line);border-radius:6px;font-size:13.5px;}
  .item-row .rm{background:none;border:none;color:var(--red);font-size:17px;padding:0 4px;}
  .item-head{display:grid;grid-template-columns:2fr 60px 90px 92px 78px 30px;gap:8px;font-size:10.5px;text-transform:uppercase;letter-spacing:.05em;color:var(--ink-soft);margin-bottom:6px;font-weight:600;}
  .item-head .hint-tag{background:var(--green-pale);color:var(--green);border-radius:4px;padding:1px 5px;font-weight:600;text-transform:none;letter-spacing:0;font-size:9.5px;margin-left:3px;}
  .item-price.edited{border-color:var(--brass);background:var(--green-pale);}

  /* ---------- Invoice preview (ledger sheet) ---------- */
  .ledger-sheet{
    background:#fff;border:1px solid var(--line);border-radius:4px;
    position:relative;padding:36px 32px 32px 54px;box-shadow:var(--shadow);
    max-width:720px;
  }
  .ledger-sheet::before{
    content:"";position:absolute;left:34px;top:0;bottom:0;width:1.5px;background:var(--red);opacity:.55;
  }
  .ledger-sheet::after{
    content:"";position:absolute;left:0;right:0;top:0;bottom:0;
    background-image:repeating-linear-gradient(transparent, transparent 27px, var(--line) 28px);
    opacity:.35;pointer-events:none;
  }
  .inv-top{display:flex;justify-content:space-between;align-items:flex-start;position:relative;z-index:1;margin-bottom:18px;}
  .inv-brand{display:flex;gap:12px;align-items:center;}
  .inv-brand img{width:48px;height:48px;object-fit:contain;border-radius:6px;}
  .inv-brand-name{font-family:'Fraunces',serif;font-size:19px;font-weight:600;}
  .inv-brand-meta{font-size:11.5px;color:var(--ink-soft);line-height:1.5;margin-top:2px;}
  .inv-num{text-align:right;font-size:12.5px;color:var(--ink-soft);}
  .inv-num .no{font-family:'IBM Plex Mono',monospace;font-size:15px;color:var(--ink);font-weight:600;}

  .inv-parties{display:flex;justify-content:space-between;gap:20px;margin-bottom:18px;position:relative;z-index:1;}
  .inv-party .k{font-size:10.5px;text-transform:uppercase;letter-spacing:.06em;color:var(--ink-soft);font-weight:600;margin-bottom:4px;}
  .inv-party .v{font-size:13.5px;line-height:1.5;}

  .inv-table{position:relative;z-index:1;}
  .inv-table table{font-size:12.5px;}
  .inv-table th{font-size:10px;padding-bottom:7px;}
  .inv-table th:not(:first-child), .inv-table td:not(:first-child){text-align:right;}
  .inv-totals{display:flex;justify-content:flex-end;margin-top:14px;position:relative;z-index:1;}
  .inv-totals table{width:280px;}
  .inv-totals td{padding:5px 0;border:none;font-size:13px;}
  .inv-totals .grand td{font-weight:700;font-size:16px;border-top:1.5px solid var(--ink);padding-top:9px;font-family:'IBM Plex Mono',monospace;}

  .stamp{
    position:absolute;right:44px;top:120px;width:88px;height:88px;border-radius:50%;
    border:2.5px solid var(--brass);color:var(--brass);
    display:flex;align-items:center;justify-content:center;text-align:center;
    font-family:'Fraunces',serif;font-weight:700;font-size:13px;letter-spacing:.05em;
    transform:rotate(-14deg);opacity:.85;z-index:2;pointer-events:none;
  }
  .stamp.stamp-paid{border-color:var(--green);color:var(--green);}
  .stamp.stamp-partial{border-color:#8A5A1D;color:#8A5A1D;font-size:11.5px;}
  .stamp.stamp-due{border-color:var(--red);color:var(--red);}

  .inv-due-note{
    margin-top:14px;padding:13px 16px;border:1.4px solid var(--red);
    border-radius:9px;background:var(--red-pale);color:var(--ink);
    font-size:12.5px;line-height:1.7;position:relative;z-index:1;
  }
  .inv-due-note strong{color:var(--red);}
  .inv-due-note .due-title{font-weight:700;color:var(--red);font-size:11px;text-transform:uppercase;letter-spacing:.05em;margin-bottom:4px;}

  .inv-signatures{display:flex;justify-content:space-between;gap:40px;margin-top:56px;position:relative;z-index:1;}
  .sig-block{flex:1;text-align:center;}
  .sig-space{height:46px;}
  .sig-line{border-top:1.4px solid var(--ink);margin-bottom:8px;}
  .sig-label{font-size:12px;color:var(--ink);font-weight:600;}
  .sig-sub{font-size:10.5px;color:var(--ink-soft);font-weight:400;}

  /* ---------- Workshop item lists ---------- */
  .workshop-items-title{
    font-family:'Fraunces',serif;font-weight:600;font-size:14.5px;color:var(--ink);
    margin-bottom:12px;padding-bottom:8px;border-bottom:1.4px solid var(--line);
  }
  .workshop-items-list{display:flex;flex-direction:column;gap:9px;}
  .workshop-item-row{
    display:flex;align-items:center;gap:12px;padding:10px 14px;flex-wrap:wrap;
    border:1px solid var(--line);border-radius:9px;background:#fff;
    transition:border-color .15s ease;
  }
  .workshop-item-row:focus-within{border-color:var(--brass);}
  .workshop-item-number{
    font-family:'Fraunces',serif;font-weight:700;font-size:14px;color:var(--brass);
    min-width:26px;flex-shrink:0;
  }
  .workshop-item-row input{
    flex:1;border:none;background:transparent;font-size:14px;color:var(--ink);
    outline:none;padding:3px 0;font-family:'Inter',sans-serif;
  }
  .btn.ghost.workshop-item-remove{
    flex-shrink:0;background:var(--red-pale);color:var(--red);border:1px solid transparent;
  }
  .btn.ghost.workshop-item-remove:hover{background:var(--red);color:#fff;}
  .workshop-items-empty{font-size:13px;color:var(--ink-soft);padding:8px 0 2px 0;}
  .workshop-finished-head{
    display:flex;align-items:flex-end;gap:12px;
    font-size:10.5px;text-transform:uppercase;letter-spacing:.05em;color:var(--ink-soft);
    font-weight:600;margin-bottom:6px;padding:0 14px;
  }
  .workshop-finished-head .fh-num{width:26px;flex-shrink:0;}
  .workshop-finished-head .fh-item{flex:1;}
  .workshop-finished-head .fh-rawavail{width:130px;flex-shrink:0;}
  .workshop-finished-head .fh-stock{width:118px;flex-shrink:0;}
  .workshop-finished-head .fh-produced{width:172px;flex-shrink:0;padding-left:14px;}
  .workshop-finished-head .fh-sellout{width:172px;flex-shrink:0;padding-left:14px;}
  .workshop-rawavail-value{
    font-family:'IBM Plex Mono',monospace;font-size:13.5px;font-weight:600;
    width:130px;flex-shrink:0;
  }
  .workshop-finished-qty{
    width:90px;padding:8px 10px;border:1px solid var(--line);border-radius:7px;
    font-size:14px;color:var(--ink);outline:none;background:#fff;font-family:'IBM Plex Mono',monospace;
    text-align:right;
  }
  .workshop-finished-qty:focus{border-color:var(--brass);}
  .workshop-finished-qty-unit{font-size:12.5px;color:var(--ink-soft);margin-left:8px;}
  .workshop-finished-add-wrap{display:flex;align-items:center;gap:6px;padding-left:14px;border-left:1px solid var(--line);width:172px;flex-shrink:0;box-sizing:border-box;}
  .workshop-finished-add-input{
    width:58px;padding:7px 8px;border:1px solid var(--line);border-radius:6px;
    font-size:13px;color:var(--ink);outline:none;background:#fff;text-align:right;
    font-family:'IBM Plex Mono',monospace;
  }
  .workshop-finished-add-input:focus{border-color:var(--brass);}
  .workshop-finished-add-input::placeholder{font-family:'Inter',sans-serif;font-size:11px;}
  .workshop-finished-add-btn{
    flex-shrink:0;padding:7px 12px !important;background:var(--green-pale);color:var(--green);
    border:1px solid transparent;font-size:13px !important;
  }
  .workshop-finished-add-btn:hover{background:var(--brass);color:#fff;}
  .workshop-finished-sell-wrap{display:flex;align-items:center;gap:6px;padding-left:14px;border-left:1px solid var(--line);width:172px;flex-shrink:0;box-sizing:border-box;}
  .workshop-finished-sell-input{
    width:58px;padding:7px 8px;border:1px solid var(--line);border-radius:6px;
    font-size:13px;color:var(--ink);outline:none;background:#fff;text-align:right;
    font-family:'IBM Plex Mono',monospace;
  }
  .workshop-finished-sell-input:focus{border-color:var(--red);}
  .workshop-finished-sell-input::placeholder{font-family:'Inter',sans-serif;font-size:11px;}
  .workshop-finished-sell-btn{
    flex-shrink:0;padding:7px 12px !important;background:var(--red-pale);color:var(--red);
    border:1px solid transparent;font-size:13px !important;
  }
  .workshop-finished-sell-btn:hover{background:var(--red);color:#fff;}

  /* ---------- Workshop: Workers ---------- */
  .workshop-worker-table-wrap{overflow-x:auto;}
  .workshop-worker-head{
    display:grid;grid-template-columns:26px 180px 100px 120px 120px 100px 78px;gap:10px;
    font-size:10.5px;text-transform:uppercase;letter-spacing:.05em;color:var(--ink-soft);
    font-weight:600;margin-bottom:6px;padding:0 14px;min-width:800px;
  }
  .workshop-worker-row{
    display:grid;grid-template-columns:26px 180px 100px 120px 120px 100px 78px;gap:10px;align-items:center;
    padding:10px 14px;border:1px solid var(--line);border-radius:9px;background:#fff;
    transition:border-color .15s ease;margin-bottom:9px;min-width:800px;
  }
  .workshop-worker-row:focus-within{border-color:var(--brass);}
  .workshop-worker-row input{
    width:100%;border:none;background:transparent;font-size:14px;color:var(--ink);
    outline:none;padding:3px 0;font-family:'Inter',sans-serif;
  }
  .workshop-worker-wage-input{font-family:'IBM Plex Mono',monospace !important;}
  .btn.ghost.workshop-worker-attendance-btn{
    background:var(--green-pale);color:var(--green);border:1px solid transparent;white-space:nowrap;
  }
  .btn.ghost.workshop-worker-attendance-btn:hover{background:var(--brass);color:#fff;}
  .btn.ghost.workshop-worker-remove{background:var(--red-pale);color:var(--red);border:1px solid transparent;}
  .btn.ghost.workshop-worker-remove:hover{background:var(--red);color:#fff;}

  .workshop-attendance-form{
    display:flex;flex-wrap:wrap;gap:12px;align-items:flex-end;
    padding:14px;background:var(--green-pale);border-radius:10px;margin-bottom:18px;
  }
  .workshop-attendance-form .field{margin-bottom:0;min-width:120px;}
  .workshop-attendance-head{
    display:grid;grid-template-columns:100px 90px 70px 70px 70px 90px 60px 30px;gap:8px;
    font-size:10.5px;text-transform:uppercase;letter-spacing:.05em;color:var(--ink-soft);
    font-weight:600;margin-bottom:6px;padding:0 4px;
  }
  .workshop-attendance-row{
    display:grid;grid-template-columns:100px 90px 70px 70px 70px 90px 60px 30px;gap:8px;align-items:center;
    padding:8px 4px;border-bottom:1px solid var(--line);font-size:13px;
  }
  .btn.ghost.workshop-item-materials-btn{
    flex-shrink:0;white-space:nowrap;background:var(--green-pale);color:var(--green);border:1px solid transparent;
  }
  .btn.ghost.workshop-item-materials-btn:hover{background:var(--brass);color:#fff;}

  .workshop-material-head{
    display:grid;grid-template-columns:2fr 80px 120px 70px 90px 80px 30px;gap:10px;
    font-size:10.5px;text-transform:uppercase;letter-spacing:.05em;color:var(--ink-soft);
    font-weight:600;margin-bottom:6px;
  }
  .workshop-material-row{display:grid;grid-template-columns:2fr 80px 120px 70px 90px 80px 30px;gap:10px;align-items:center;}
  .workshop-material-row input{
    padding:8px 9px;border:1px solid var(--line);border-radius:6px;
    font-size:13.5px;color:var(--ink);outline:none;background:#fff;font-family:'Inter',sans-serif;
  }
  .workshop-material-row input:focus{border-color:var(--brass);}
  .workshop-piece-value{
    font-family:'IBM Plex Mono',monospace;font-size:13.5px;font-weight:600;color:var(--green);
    text-align:center;padding:8px 4px;background:var(--green-pale);border-radius:6px;
  }
  .workshop-restock-wrap{display:flex;align-items:center;gap:5px;}
  .workshop-restock-input{
    width:58px;padding:7px 7px !important;text-align:right;font-family:'IBM Plex Mono',monospace !important;
  }
  .workshop-restock-btn{
    flex-shrink:0;padding:7px 10px !important;background:var(--green-pale);color:var(--green);
    border:1px solid transparent;font-size:12.5px !important;white-space:nowrap;
  }
  .workshop-restock-btn:hover{background:var(--brass);color:#fff;}

  /* ---------- Empty state ---------- */
  .empty-state{text-align:center;padding:60px 20px;color:var(--ink-soft);}
  .empty-state svg{width:44px;height:44px;color:var(--brass);margin-bottom:14px;opacity:.8;}
  .empty-state h3{color:var(--ink);margin-bottom:6px;font-size:17px;}
  .empty-state p{font-size:13.5px;margin:0 0 16px 0;}

  .logo-drop{
    border:1.5px dashed var(--line);border-radius:8px;padding:18px;text-align:center;
    background:#fff;cursor:pointer;position:relative;
  }
  .logo-drop img{max-width:100px;max-height:60px;object-fit:contain;}
  .logo-drop .hint{font-size:12px;color:var(--ink-soft);margin-top:6px;}
  .logo-drop input[type=file]{position:absolute;inset:0;opacity:0;cursor:pointer;}

  .toast{
    position:fixed;bottom:24px;right:24px;background:var(--green);color:#fff;
    padding:12px 18px;border-radius:8px;font-size:13.5px;font-weight:500;
    box-shadow:var(--shadow);opacity:0;transform:translateY(8px);transition:.25s ease;pointer-events:none;z-index:50;
  }
  .toast.show{opacity:1;transform:none;}

  .modal-overlay{position:fixed;inset:0;background:rgba(27,36,32,0.4);display:none;align-items:center;justify-content:center;z-index:60;padding:20px;}
  .modal-overlay.show{display:flex;}
  .modal{background:var(--paper);border-radius:12px;max-width:640px;width:100%;max-height:88vh;overflow:auto;padding:26px 28px;box-shadow:0 20px 60px rgba(0,0,0,0.25);}
  .modal-close-row{display:flex;justify-content:flex-end;gap:10px;margin-top:18px;}

  @media print{
    .sidebar,.no-print{display:none !important;}
    .main{padding:0;}
    body{background:#fff;}
  }

  @media (max-width:820px){
    .sidebar{width:72px;}
    .brand-name,.brand-sub,.nav-item span.label{display:none;}
    .nav-item{justify-content:center;}
    .brand{justify-content:center;padding:0 0 18px 0;}
    .main{padding:22px 16px 60px 16px;}
    .form-grid{grid-template-columns:1fr;}
    .inv-parties{flex-direction:column;gap:14px;}
  }
</style>
</head>
<body>

<div class="storage-warning no-print" id="storage-warning" style="display:none;">
  ⚠ This file isn't connected to persistent storage right now, so nothing you enter will be saved. Open it from within Claude.ai (not as a locally double-clicked file) for your data to save automatically — and always keep a recent Export Backup as a safety copy.
</div>

<!-- ===================== REGISTRATION PAGE ===================== -->
<div class="auth-screen" id="auth-register-screen">
  <div class="auth-card">
    <div class="auth-brand">
      <div class="brand-mark">₹</div>
      <div>
        <div class="brand-name">Khata</div>
        <div class="brand-sub">Shop Ledger &amp; GST Invoicing</div>
      </div>
    </div>

    <div class="auth-gate" id="reg-gate">
      <h1 class="auth-title">Welcome to Khata</h1>
      <div class="auth-sub">Do you already have a Khata username and password?</div>
      <div class="auth-gate-btns">
        <button class="btn secondary auth-gate-btn" id="reg-gate-no">No, create one now</button>
        <button class="btn auth-gate-btn" id="reg-gate-yes">Yes, I have an account</button>
      </div>
    </div>

    <div id="reg-form-body" style="display:none;">
      <h1 class="auth-title">Create Your Account</h1>
      <div class="auth-sub">Set up your login once — you'll use it every time you open Khata on this device.</div>

      <div class="field" id="reg-name-field">
        <label>Your Name<span class="required-star">*</span></label>
        <input id="reg-name" placeholder="e.g. Anirban Das">
      </div>
      <div class="field" id="reg-username-field">
        <label>Username<span class="required-star">*</span></label>
        <input id="reg-username" placeholder="Choose a username" autocomplete="username">
      </div>
      <div class="field" id="reg-password-field">
        <label>Password<span class="required-star">*</span></label>
        <input id="reg-password" type="password" placeholder="Choose a password" autocomplete="new-password">
      </div>
      <div class="field" id="reg-password2-field">
        <label>Confirm Password<span class="required-star">*</span></label>
        <input id="reg-password2" type="password" placeholder="Re-enter your password" autocomplete="new-password">
      </div>
      <div class="auth-error" id="reg-error"></div>
      <button class="btn auth-btn" id="reg-submit-btn">Register</button>
      <div class="auth-note">This app runs entirely on your own device — there's no server or account recovery, so please remember your username and password, or use "Remember me" on the login page.</div>
      <div class="auth-note">Already have an account? <a href="#" id="reg-switch-to-login">Log in instead</a></div>
    </div>
  </div>
</div>

<!-- ===================== LOGIN PAGE ===================== -->
<div class="auth-screen" id="auth-login-screen" style="display:none;">
  <div class="auth-card">
    <div class="auth-brand">
      <div class="brand-mark">₹</div>
      <div>
        <div class="brand-name">Khata</div>
        <div class="brand-sub">Shop Ledger &amp; GST Invoicing</div>
      </div>
    </div>

    <div class="auth-gate" id="login-gate">
      <h1 class="auth-title">Welcome Back</h1>
      <div class="auth-sub">Do you already have a Khata username and password?</div>
      <div class="auth-gate-btns">
        <button class="btn secondary auth-gate-btn" id="login-gate-no">No, I need to register</button>
        <button class="btn auth-gate-btn" id="login-gate-yes">Yes, I have an account</button>
      </div>
    </div>

    <div id="login-form-body" style="display:none;">
      <h1 class="auth-title">Log In</h1>
      <div class="auth-sub" id="login-welcome-sub">Log in to continue to your ledger.</div>

      <div class="field" id="login-username-field">
        <label>Username<span class="required-star">*</span></label>
        <input id="login-username" list="login-username-list" placeholder="Your username" autocomplete="username">
        <datalist id="login-username-list"></datalist>
      </div>
      <div class="field" id="login-password-field">
        <label>Password<span class="required-star">*</span></label>
        <input id="login-password" type="password" placeholder="Your password" autocomplete="current-password">
      </div>
      <label class="auth-remember">
        <input type="checkbox" id="login-remember"> Remember my username and password on this device
      </label>
      <div class="auth-error" id="login-error"></div>
      <button class="btn auth-btn" id="login-submit-btn">Log In</button>
      <div class="auth-note">
        Forgot your password? <a href="#" id="login-reset-link">Reset account</a> and register again. (Your invoices, products, and accounts data stay safe — only the login is reset.)
      </div>
      <div class="auth-note">New to Khata? <a href="#" id="login-switch-to-register">Create an account</a></div>
    </div>
  </div>
</div>

<div class="app" id="main-app" style="display:none;">
  <!-- SIDEBAR -->
  <nav class="sidebar no-print">
    <div class="brand">
      <div class="brand-mark">₹</div>
      <div>
        <div class="brand-name">Khata</div>
        <div class="brand-sub">Shop Ledger</div>
      </div>
    </div>
    <div class="nav">
      <button class="nav-item active" data-page="dashboard">
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8"><rect x="3" y="3" width="7" height="9" rx="1.5"/><rect x="14" y="3" width="7" height="5" rx="1.5"/><rect x="14" y="12" width="7" height="9" rx="1.5"/><rect x="3" y="16" width="7" height="5" rx="1.5"/></svg>
        <span class="label">Dashboard</span>
      </button>
      <button class="nav-item" data-page="invoice">
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8"><path d="M12 5v14M5 12h14"/></svg>
        <span class="label">New Invoice</span>
      </button>
      <button class="nav-item" data-page="history">
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8"><path d="M3 3v18h18"/><path d="M7 14l4-4 3 3 5-6"/></svg>
        <span class="label">History</span>
      </button>
      <button class="nav-item" data-page="gst">
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8"><circle cx="12" cy="12" r="9"/><path d="M9 12h6M12 9v6"/></svg>
        <span class="label">GST Summary</span>
      </button>
      <button class="nav-item" data-page="accounts">
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8"><rect x="2" y="6" width="20" height="13" rx="2"/><path d="M2 10h20"/><path d="M6 15h4"/></svg>
        <span class="label">Accounts</span>
      </button>
      <button class="nav-item" data-page="products">
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8"><path d="M20 7l-8-4-8 4m16 0l-8 4m8-4v10l-8 4m0-10L4 7m8 4v10M4 7v10l8 4"/></svg>
        <span class="label">Products</span>
      </button>
      <button class="nav-item" data-page="workshop">
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8"><path d="M14.7 6.3a1 1 0 0 0 0 1.4l1.6 1.6a1 1 0 0 0 1.4 0l3.77-3.77a6 6 0 0 1-7.94 7.94l-6.91 6.91a2.12 2.12 0 0 1-3-3l6.91-6.91a6 6 0 0 1 7.94-7.94l-3.76 3.76z"/></svg>
        <span class="label">Workshop</span>
      </button>
      <button class="nav-item" data-page="profile">
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8"><rect x="3" y="4" width="18" height="16" rx="2"/><path d="M3 9h18"/></svg>
        <span class="label">Business Profile</span>
      </button>
    </div>
    <div class="sidebar-foot">
      Saved automatically<br>on this device.
      <button class="logout-btn" id="logout-btn">
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M9 21H5a2 2 0 0 1-2-2V5a2 2 0 0 1 2-2h4"/><path d="M16 17l5-5-5-5"/><path d="M21 12H9"/></svg>
        Log Out
      </button>
    </div>
  </nav>

  <!-- MAIN -->
  <main class="main">

    <!-- DASHBOARD -->
    <section class="page active" id="page-dashboard">
      <div class="page-head">
        <div>
          <h1>Dashboard</h1>
          <div class="sub" id="dash-date"></div>
        </div>
        <button class="btn" data-goto="invoice">
          <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M12 5v14M5 12h14"/></svg>
          New Invoice
        </button>
      </div>

      <div class="stat-grid">
        <div class="stat-card"><div class="accent-bar"></div><div class="label">Today's Sales</div><div class="value mono" id="stat-today">₹0</div></div>
        <div class="stat-card"><div class="accent-bar"></div><div class="label">This Month</div><div class="value mono" id="stat-month">₹0</div></div>
        <div class="stat-card"><div class="accent-bar"></div><div class="label">GST Collected (Month)</div><div class="value mono" id="stat-gst">₹0</div></div>
        <div class="stat-card"><div class="accent-bar"></div><div class="label">Invoices (Month)</div><div class="value mono" id="stat-count">0</div></div>
      </div>

      <div class="card">
        <h3>Recent Transactions</h3>
        <table>
          <thead><tr><th>Invoice</th><th>Date</th><th>Customer</th><th>Tax Type</th><th>Amount</th><th>Status</th><th></th></tr></thead>
          <tbody id="recent-body"></tbody>
        </table>
      </div>
    </section>

    <!-- NEW INVOICE -->
    <section class="page" id="page-invoice">
      <div class="page-head">
        <div><h1>New Invoice</h1><div class="sub">Create a GST invoice for a sale.</div></div>
      </div>

      <div class="card">
        <h3>Customer</h3>
        <div class="form-grid">
          <div class="field"><label>Customer Name</label><input id="cust-name" placeholder="Walk-in / Customer name"></div>
          <div class="field"><label>Phone</label><input id="cust-phone" placeholder="Optional"></div>
          <div class="field full"><label>Address</label><input id="cust-address" placeholder="Optional"></div>
          <div class="field"><label>State</label><select id="cust-state"></select></div>
          <div class="field"><label>PIN Code</label><input id="cust-pin" type="tel" maxlength="6" inputmode="numeric" pattern="[0-9]*" placeholder="e.g. 700001"></div>
          <div class="field"><label>GSTIN (if registered)</label><input id="cust-gstin" placeholder="Optional"></div>
          <div class="field">
            <label>Tax Type</label>
            <div class="radio-row">
              <label class="radio-opt selected" id="opt-intra"><input type="radio" name="taxtype" value="intra" checked> Same state (CGST + SGST)</label>
              <label class="radio-opt" id="opt-inter"><input type="radio" name="taxtype" value="inter"> Different state (IGST)</label>
            </div>
            <div class="sub" style="margin-top:6px;color:var(--ink-soft);font-size:12px;" id="taxtype-hint">Auto-set from customer's State — change it above if the customer state differs from your business.</div>
          </div>
        </div>
      </div>

      <div class="card">
        <h3>Items</h3>
        <div class="item-head"><div>Product</div><div>Qty</div><div>Price (₹) <span class="hint-tag">editable</span></div><div>Price Type</div><div>GST %</div><div></div></div>
        <div id="items-wrap"></div>
        <button class="btn secondary" id="add-item-btn" style="margin-top:8px;">+ Add Item</button>
      </div>

      <div class="card">
        <h3>Totals</h3>
        <div class="inv-totals" style="justify-content:flex-start;">
          <table>
            <tr><td>Taxable Value</td><td class="mono" id="calc-taxable">₹0.00</td></tr>
            <tr id="row-cgst"><td>CGST</td><td class="mono" id="calc-cgst">₹0.00</td></tr>
            <tr id="row-sgst"><td>SGST</td><td class="mono" id="calc-sgst">₹0.00</td></tr>
            <tr id="row-igst" style="display:none;"><td>IGST</td><td class="mono" id="calc-igst">₹0.00</td></tr>
            <tr class="grand"><td>Grand Total</td><td class="mono" id="calc-grand">₹0.00</td></tr>
          </table>
        </div>
        <div style="margin-top:18px;display:flex;gap:10px;">
          <button class="btn" id="save-invoice-btn">Save Invoice</button>
          <button class="btn secondary" id="clear-invoice-btn">Clear</button>
        </div>
      </div>
    </section>

    <!-- HISTORY -->
    <section class="page" id="page-history">
      <div class="page-head">
        <div><h1>Transaction History</h1><div class="sub">All saved invoices.</div></div>
        <input type="text" id="history-search" placeholder="Search customer or invoice no." style="padding:9px 13px;border:1px solid var(--line);border-radius:7px;font-size:13.5px;width:240px;">
      </div>
      <div class="card">
        <table>
          <thead><tr><th>Invoice</th><th>Date</th><th>Customer</th><th>Tax Type</th><th>Amount</th><th>Status</th><th></th></tr></thead>
          <tbody id="history-body"></tbody>
        </table>
      </div>
    </section>

    <!-- GST SUMMARY -->
    <section class="page" id="page-gst">
      <div class="page-head">
        <div><h1>GST Summary</h1><div class="sub">Monthly totals for filing reference.</div></div>
      </div>
      <div class="card">
        <table>
          <thead><tr><th>Month</th><th>Invoices</th><th>Taxable Value</th><th>CGST</th><th>SGST</th><th>IGST</th><th>Total GST</th></tr></thead>
          <tbody id="gst-body"></tbody>
        </table>
      </div>
    </section>

    <!-- PRODUCTS -->
    <section class="page" id="page-products">
      <div class="page-head">
        <div><h1>Products</h1><div class="sub">Save frequent items for faster invoicing.</div></div>
        <button class="btn" id="add-product-btn">
          <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M12 5v14M5 12h14"/></svg>
          Add Product
        </button>
      </div>
      <div class="card">
        <table>
          <thead><tr><th>Name</th><th>Price (₹)</th><th>GST %</th><th></th></tr></thead>
          <tbody id="products-body"></tbody>
        </table>
      </div>
    </section>

    <!-- ACCOUNTS -->
    <section class="page" id="page-accounts">
      <div class="page-head">
        <div><h1>Accounts</h1><div class="sub">Track money in and out across cash and bank accounts.</div></div>
        <div style="display:flex;gap:10px;">
          <button class="btn secondary" id="add-account-btn">
            <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M12 5v14M5 12h14"/></svg>
            Add Account
          </button>
          <button class="btn" id="add-txn-btn">
            <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M12 5v14M5 12h14"/></svg>
            Add Transaction
          </button>
        </div>
      </div>

      <div class="stat-grid">
        <div class="stat-card">
          <div class="accent-bar"></div>
          <div class="label">Total Money In</div>
          <div class="value mono" id="acc-in">₹0.00</div>
        </div>
        <div class="stat-card">
          <div class="accent-bar" style="background:var(--red);"></div>
          <div class="label">Total Money Out</div>
          <div class="value mono" id="acc-out">₹0.00</div>
        </div>
        <div class="stat-card">
          <div class="accent-bar" style="background:var(--green-light);"></div>
          <div class="label">Total Balance (All Accounts)</div>
          <div class="value mono" id="acc-balance">₹0.00</div>
        </div>
      </div>

      <div class="card">
        <div style="display:flex;justify-content:space-between;align-items:center;flex-wrap:wrap;gap:10px;">
          <h3 style="margin:0;">Payment Accounts</h3>
          <div style="display:flex;gap:8px;align-items:center;">
            <input id="account-search-input" type="text" placeholder="Search by name, bank, or account number" style="min-width:260px;">
            <button class="btn secondary" id="account-search-btn">
              <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" style="width:16px;height:16px;"><circle cx="11" cy="11" r="7"/><path d="m21 21-4.3-4.3"/></svg>
              Search
            </button>
          </div>
        </div>
        <div class="sub" style="margin:10px 0 14px 0;color:var(--ink-soft);font-size:13px;">
          Each account tracks its own balance. Add your bank accounts with account numbers to see exactly where your money sits.
        </div>
        <div id="accounts-list-body" style="display:flex;flex-direction:column;gap:10px;"></div>
      </div>

      <div class="card">
        <div style="display:flex;justify-content:space-between;align-items:center;flex-wrap:wrap;gap:10px;margin-bottom:4px;">
          <h3 style="margin:0;">Transactions</h3>
          <select id="txn-filter-account" style="max-width:220px;">
            <option value="all">All Accounts</option>
          </select>
        </div>
        <table>
          <thead><tr><th>Date</th><th>Account</th><th>Type</th><th>Note</th><th>Amount</th><th>Balance</th><th></th></tr></thead>
          <tbody id="accounts-body"></tbody>
        </table>
      </div>
    </section>

    <!-- ADD TRANSACTION MODAL -->
    <div class="modal-overlay" id="txn-modal-overlay">
      <div class="modal">
        <h3 style="margin:0 0 16px 0;">Add Transaction</h3>
        <div class="radio-row" style="margin-bottom:14px;">
          <label class="radio-opt selected" id="txn-type-in-opt">
            <input type="radio" name="txn-type" value="in" checked> Money In
          </label>
          <label class="radio-opt" id="txn-type-out-opt">
            <input type="radio" name="txn-type" value="out"> Money Out
          </label>
          <label class="radio-opt" id="txn-type-transfer-opt">
            <input type="radio" name="txn-type" value="transfer"> Transfer
          </label>
        </div>
        <div class="field">
          <label id="txn-account-label">Account</label>
          <select id="txn-account"></select>
        </div>
        <div class="field" id="txn-to-account-field" style="display:none;">
          <label>To Account</label>
          <select id="txn-to-account"></select>
        </div>
        <div class="field">
          <label>Date</label>
          <input type="date" id="txn-date">
        </div>
        <div class="field">
          <label>Amount (₹)</label>
          <input type="number" id="txn-amount" placeholder="0.00" min="0" step="0.01">
        </div>
        <div class="field">
          <label>Note</label>
          <input type="text" id="txn-note" placeholder="e.g. Cash sale, Rent, Supplier payment, Deposited to bank">
        </div>
        <div class="modal-close-row">
          <button class="btn secondary" id="txn-cancel">Cancel</button>
          <button class="btn" id="txn-save">Save</button>
        </div>
      </div>
    </div>

    <!-- VIEW TRANSACTION MODAL -->
    <div class="modal-overlay" id="view-txn-modal-overlay">
      <div class="modal">
        <h3 style="margin:0 0 4px 0;">Transaction Details</h3>
        <div style="font-size:12px;color:var(--ink-soft);margin-bottom:16px;" id="vt-id"></div>

        <div style="display:flex;flex-direction:column;gap:9px;font-size:14px;">
          <div style="display:flex;justify-content:space-between;"><span style="color:var(--ink-soft);">Date</span><span id="vt-date" style="font-weight:600;"></span></div>
          <div style="display:flex;justify-content:space-between;"><span style="color:var(--ink-soft);">Type</span><span id="vt-type" style="font-weight:600;"></span></div>
          <div style="display:flex;justify-content:space-between;"><span style="color:var(--ink-soft);">Note</span><span id="vt-note" style="font-weight:600;text-align:right;max-width:60%;"></span></div>
          <div style="display:flex;justify-content:space-between;border-top:1px solid var(--line);padding-top:9px;margin-top:2px;"><span style="color:var(--ink-soft);">Amount</span><span id="vt-amount" class="mono" style="font-weight:700;font-size:16px;"></span></div>
        </div>

        <div id="vt-from-section" style="margin-top:18px;">
          <div style="font-weight:600;font-size:13px;margin-bottom:8px;" id="vt-from-heading">Account</div>
          <div style="display:flex;flex-direction:column;gap:8px;font-size:13.5px;background:var(--card-alt,rgba(0,0,0,0.02));border:1px solid var(--line);border-radius:8px;padding:12px 14px;">
            <div style="display:flex;justify-content:space-between;"><span style="color:var(--ink-soft);">Account</span><span id="vt-from-name" style="font-weight:600;"></span></div>
            <div style="display:flex;justify-content:space-between;"><span style="color:var(--ink-soft);">Opening Balance</span><span id="vt-from-opening" class="mono"></span></div>
            <div style="display:flex;justify-content:space-between;"><span style="color:var(--ink-soft);">Balance Before</span><span id="vt-from-before" class="mono"></span></div>
            <div style="display:flex;justify-content:space-between;"><span style="color:var(--ink-soft);">Balance After</span><span id="vt-from-after" class="mono" style="font-weight:700;"></span></div>
            <div style="display:flex;justify-content:space-between;"><span style="color:var(--ink-soft);">Current Balance (Today)</span><span id="vt-from-current" class="mono"></span></div>
          </div>
        </div>

        <div id="vt-to-section" style="margin-top:14px;display:none;">
          <div style="font-weight:600;font-size:13px;margin-bottom:8px;">To Account</div>
          <div style="display:flex;flex-direction:column;gap:8px;font-size:13.5px;background:var(--card-alt,rgba(0,0,0,0.02));border:1px solid var(--line);border-radius:8px;padding:12px 14px;">
            <div style="display:flex;justify-content:space-between;"><span style="color:var(--ink-soft);">Account</span><span id="vt-to-name" style="font-weight:600;"></span></div>
            <div style="display:flex;justify-content:space-between;"><span style="color:var(--ink-soft);">Opening Balance</span><span id="vt-to-opening" class="mono"></span></div>
            <div style="display:flex;justify-content:space-between;"><span style="color:var(--ink-soft);">Balance Before</span><span id="vt-to-before" class="mono"></span></div>
            <div style="display:flex;justify-content:space-between;"><span style="color:var(--ink-soft);">Balance After</span><span id="vt-to-after" class="mono" style="font-weight:700;"></span></div>
            <div style="display:flex;justify-content:space-between;"><span style="color:var(--ink-soft);">Current Balance (Today)</span><span id="vt-to-current" class="mono"></span></div>
          </div>
        </div>

        <div class="modal-close-row">
          <button class="btn secondary" id="vt-close">Close</button>
        </div>
      </div>
    </div>

    <!-- ADD/EDIT PAYMENT ACCOUNT MODAL -->
    <div class="modal-overlay" id="account-modal-overlay">
      <div class="modal">
        <h3 style="margin:0 0 16px 0;" id="am-title">Add Account</h3>
        <div class="field">
          <label>Account Type</label>
          <div class="radio-row">
            <label class="radio-opt selected" id="am-kind-cash-opt">
              <input type="radio" name="am-kind" value="cash" checked> Cash
            </label>
            <label class="radio-opt" id="am-kind-bank-opt">
              <input type="radio" name="am-kind" value="bank"> Bank
            </label>
          </div>
        </div>
        <div class="field">
          <label>Account Name</label>
          <input id="am-name" placeholder="e.g. Cash in Hand, HDFC Current A/c">
        </div>
        <div id="am-bank-fields" style="display:none;">
          <div class="field">
            <label>Bank Name</label>
            <input id="am-bank-name" placeholder="e.g. HDFC Bank">
          </div>
          <div class="field">
            <label>Account Number</label>
            <input id="am-account-number" placeholder="e.g. 50100XXXXXXXX" inputmode="numeric">
          </div>
          <div class="field">
            <label>IFSC Code (optional)</label>
            <input id="am-ifsc" placeholder="e.g. HDFC0001234">
          </div>
        </div>
        <div class="field">
          <label>Opening Balance (₹)</label>
          <input id="am-opening-balance" type="number" min="0" step="0.01" placeholder="0.00">
        </div>
        <div class="modal-close-row">
          <button class="btn secondary" id="am-cancel">Cancel</button>
          <button class="btn" id="am-save">Save</button>
        </div>
      </div>
    </div>

    <!-- VIEW PAYMENT ACCOUNT MODAL -->
    <div class="modal-overlay" id="view-account-modal-overlay">
      <div class="modal">
        <h3 style="margin:0 0 16px 0;" id="va-title">Account Details</h3>
        <div style="display:flex;flex-direction:column;gap:10px;font-size:14px;">
          <div style="display:flex;justify-content:space-between;"><span style="color:var(--ink-soft);">Type</span><span id="va-type" style="font-weight:600;"></span></div>
          <div style="display:flex;justify-content:space-between;"><span style="color:var(--ink-soft);">Account Name</span><span id="va-name" style="font-weight:600;"></span></div>
          <div id="va-bank-row" style="display:flex;justify-content:space-between;"><span style="color:var(--ink-soft);">Bank Name</span><span id="va-bank-name" style="font-weight:600;"></span></div>
          <div id="va-number-row" style="display:flex;justify-content:space-between;"><span style="color:var(--ink-soft);">Account Number</span><span id="va-account-number" class="mono" style="font-weight:600;"></span></div>
          <div id="va-ifsc-row" style="display:flex;justify-content:space-between;"><span style="color:var(--ink-soft);">IFSC Code</span><span id="va-ifsc" class="mono" style="font-weight:600;"></span></div>
          <div style="display:flex;justify-content:space-between;"><span style="color:var(--ink-soft);">Opening Balance</span><span id="va-opening" class="mono" style="font-weight:600;"></span></div>
          <div style="display:flex;justify-content:space-between;border-top:1px solid var(--line);padding-top:10px;margin-top:4px;"><span style="color:var(--ink-soft);">Current Balance</span><span id="va-balance" class="mono" style="font-weight:700;font-size:16px;"></span></div>
        </div>
        <div style="margin-top:18px;">
          <div style="font-weight:600;font-size:13px;margin-bottom:8px;">Recent Transactions</div>
          <div id="va-txn-list" style="display:flex;flex-direction:column;gap:6px;max-height:220px;overflow-y:auto;"></div>
        </div>
        <div class="modal-close-row">
          <button class="btn secondary" id="va-close">Close</button>
          <button class="btn" id="va-edit">Edit This Account</button>
        </div>
      </div>
    </div>

    <!-- WORKSHOP -->
    <section class="page" id="page-workshop">
      <div class="page-head">
        <div><h1>Workshop</h1><div class="sub">Items manufactured in-house at your connected workshop.</div></div>
        <div style="display:flex;gap:10px;flex-wrap:wrap;">
          <button class="btn" id="workshop-tab-raw">Raw Materials</button>
          <button class="btn secondary" id="workshop-tab-finished">Finished Products</button>
          <button class="btn secondary" id="workshop-tab-workers">Workers</button>
        </div>
      </div>

      <div class="card" id="workshop-raw-panel">
        <h3>Raw Materials</h3>
        <div class="sub" style="color:var(--ink-soft);font-size:13.5px;line-height:1.7;margin-bottom:18px;">
          Raw materials used across your workshop, organised by the item they're used for.
        </div>
        <div class="workshop-items-title">Raw Materials for the Items:</div>
        <div id="workshop-raw-items-list" class="workshop-items-list"></div>
        <button class="btn secondary" id="workshop-raw-add-btn" style="margin-top:12px;">+ Add Item</button>
      </div>

      <div class="card" id="workshop-finished-panel" style="display:none;">
        <h3>Finished Products</h3>
        <div class="sub" style="color:var(--ink-soft);font-size:13.5px;line-height:1.7;margin-bottom:18px;">
          Finished products manufactured and ready from your workshop. These items are the same items listed under Raw Materials — add, rename, or remove an item there and it updates here automatically.
        </div>
        <div class="workshop-items-title">Finished Products for the Items:</div>
        <div class="workshop-finished-head">
          <div class="fh-num"></div><div class="fh-item">Item</div><div class="fh-rawavail">Raw Material Available</div><div class="fh-stock">Stock Available</div><div class="fh-produced">QTY Produced</div><div class="fh-sellout">Sell Out</div>
        </div>
        <div id="workshop-finished-items-list" class="workshop-items-list"></div>
      </div>

      <div id="workshop-workers-panel" style="display:none;">
        <div class="card">
          <h3>Worker Directory</h3>
          <div class="sub" style="color:var(--ink-soft);font-size:13.5px;line-height:1.7;margin-bottom:18px;">
            Workers connected with your workshop. Set each worker's normal arrival and departure time (their standard shift). Overtime is paid, and short days are deducted, at that worker's own wages-per-hour rate (Wage / Day ÷ Normal Hours) — calculated automatically from the actual In/Out time logged in Attendance.
          </div>
          <div class="workshop-items-title">Workers:</div>
          <div class="workshop-worker-table-wrap">
            <div class="workshop-worker-head">
              <div></div><div>Name</div><div>Wage / Day (₹)</div><div>Normal In</div><div>Normal Out</div><div></div><div></div>
            </div>
            <div id="workshop-workers-list" class="workshop-items-list"></div>
          </div>
          <button class="btn secondary" id="workshop-worker-add-btn" style="margin-top:12px;">+ Add Worker</button>
        </div>

        <div class="card">
          <div style="display:flex;justify-content:space-between;align-items:center;flex-wrap:wrap;gap:12px;margin-bottom:4px;">
            <h3 style="margin:0;">Weekly Wage Summary</h3>
            <div style="display:flex;align-items:center;gap:10px;">
              <label style="font-size:12.5px;color:var(--ink-soft);font-weight:600;">Week starting</label>
              <input type="date" id="workshop-week-start">
            </div>
          </div>
          <div class="sub" style="color:var(--ink-soft);font-size:13.5px;line-height:1.7;margin:10px 0 18px 0;">
            Wages and payments are calculated for the 7 days from the date above (Monday–Sunday). Opening Balance carries forward automatically from last week's unpaid Balance Due — shown with a dashed border. Type a value to set a manual opening balance just for that week instead (shown with a solid border).
          </div>
          <table>
            <thead><tr><th>Worker</th><th>Opening Balance (₹)</th><th>Days Present</th><th>OT Hours</th><th>This Week's Wage</th><th>Paid This Week</th><th>Balance Due</th><th></th></tr></thead>
            <tbody id="workshop-weekly-body"></tbody>
          </table>
        </div>
      </div>
    </section>

    <!-- Workshop: Attendance modal for a worker -->
    <div class="modal-overlay" id="workshop-attendance-modal-overlay">
      <div class="modal" style="max-width:720px;">
        <h3 style="margin:0 0 4px 0;" id="wa-title">Attendance</h3>
        <div class="sub" style="color:var(--ink-soft);font-size:12.5px;margin-bottom:16px;">Log the actual In and Out time for each day. Hours worked, overtime, and daily wage are calculated automatically by comparing this against the worker's Normal In/Out time set in the Worker Directory.</div>

        <div class="workshop-attendance-form">
          <div class="field"><label>Date</label><input type="date" id="wa-date"></div>
          <div class="field"><label>In Time</label><input type="time" id="wa-in-time"></div>
          <div class="field"><label>Out Time</label><input type="time" id="wa-out-time"></div>
          <button class="btn" id="wa-add-entry-btn" style="align-self:flex-end;">+ Add Entry</button>
          <button class="btn secondary" id="wa-cancel-edit-btn" style="align-self:flex-end;display:none;">Cancel Edit</button>
        </div>

        <div class="workshop-attendance-head">
          <div>Date</div><div>In</div><div>Out</div><div>Hours</div><div>OT (hrs)</div><div>Daily Wage</div><div></div><div></div>
        </div>
        <div id="workshop-attendance-list" style="display:flex;flex-direction:column;gap:6px;max-height:260px;overflow-y:auto;"></div>

        <div class="modal-close-row" style="justify-content:space-between;">
          <button class="btn secondary" id="wa-back-btn">Back</button>
          <button class="btn" id="wa-close-btn">Done</button>
        </div>
      </div>
    </div>

    <!-- Workshop: Worker payment modal -->
    <div class="modal-overlay" id="workshop-payment-modal-overlay">
      <div class="modal" style="max-width:400px;">
        <h3 style="margin:0 0 6px 0;" id="wp-title">Record Payment</h3>
        <div class="sub" style="margin-bottom:16px;color:var(--ink-soft);font-size:13px;" id="wp-sub"></div>
        <div class="field">
          <label>Payment Type</label>
          <div class="radio-row">
            <label class="radio-opt selected" id="wp-type-wage-opt"><input type="radio" name="wp-type" value="wage" checked> Wage Payment</label>
            <label class="radio-opt" id="wp-type-advance-opt"><input type="radio" name="wp-type" value="advance"> Advance Payment</label>
            <label class="radio-opt" id="wp-type-extra-opt"><input type="radio" name="wp-type" value="extra"> Extra Payment</label>
          </div>
        </div>
        <div class="field"><label>Date</label><input type="date" id="wp-date"></div>
        <div class="field"><label>Amount (₹)</label><input type="number" id="wp-amount" min="0" step="0.01" placeholder="0.00"></div>
        <div class="field"><label>Note (optional)</label><input type="text" id="wp-note" placeholder="e.g. Week ending 14 Sep"></div>
        <div class="auth-error" id="wp-error"></div>
        <div class="modal-close-row">
          <button class="btn secondary" id="wp-cancel-btn">Cancel</button>
          <button class="btn" id="wp-save-btn">Save Payment</button>
        </div>
      </div>
    </div>

    <!-- PROFILE -->
    <section class="page" id="page-profile">
      <div class="page-head">
        <div><h1>Business Profile</h1><div class="sub">Appears on every invoice.</div></div>
      </div>
      <div class="card" style="max-width:640px;">
        <div class="form-grid">
          <div class="field full">
            <label>Business Logo</label>
            <div class="logo-drop" id="logo-drop">
              <div id="logo-preview">Click or drop an image</div>
              <div class="hint">PNG or JPG, shown on invoices</div>
              <input type="file" id="logo-input" accept="image/*">
            </div>
          </div>
          <div class="field full"><label>Business Name</label><input id="biz-name" placeholder="Your shop name"></div>
          <div class="field full"><label>Address</label><input id="biz-address" placeholder="Shop address"></div>
          <div class="field"><label>GSTIN</label><input id="biz-gstin" placeholder="e.g. 22AAAAA0000A1Z5"></div>
          <div class="field"><label>State</label><select id="biz-state"></select></div>
          <div class="field"><label>PIN Code</label><input id="biz-pin" type="tel" maxlength="6" inputmode="numeric" pattern="[0-9]*" placeholder="e.g. 700001"></div>
          <div class="field"><label>Phone</label><input id="biz-phone" placeholder="Optional"></div>
        </div>
        <button class="btn" id="save-profile-btn">Save Profile</button>
      </div>

      <div class="card" style="max-width:640px;">
        <h3>Data Backup</h3>
        <div class="sub" style="margin-bottom:14px;color:var(--ink-soft);font-size:13px;">
          Export all your data (profile, products, invoices) as a file, or restore from a previous backup.
        </div>
        <div style="display:flex;gap:10px;flex-wrap:wrap;">
          <button class="btn backup" id="export-backup-btn">Export Backup</button>
          <button class="btn backup" id="import-backup-btn">Import Backup</button>
          <input type="file" id="import-backup-input" accept="application/json,.json,text/plain" style="display:none;">
        </div>
        <div style="font-size:12px;color:var(--ink-soft);line-height:1.7;margin-top:14px;">
          <strong>Export Backup</strong> downloads a file named <span class="mono">khata-backup-[date].json</span> to your device's usual Downloads location (or prompts you to choose where to save it).<br>
          <strong>Import Backup</strong> opens your device's file picker — select a <span class="mono">khata-backup-*.json</span> file you exported earlier, and everything in it (profile, products, invoices, accounts) replaces what's currently on this device.
        </div>
      </div>

      <div class="card" style="max-width:640px;">
        <h3>Google Sheet Sync</h3>
        <div class="sub" style="margin-bottom:14px;color:var(--ink-soft);font-size:13px;line-height:1.6;">
          Auto-push every invoice and transaction to a Google Sheet in your own account, in real time. One-time setup required — see steps below.
        </div>
        <div class="field full">
          <label>Apps Script Web App URL</label>
          <input id="sheet-sync-url" placeholder="https://script.google.com/macros/s/XXXXX/exec">
        </div>
        <div id="sheet-sync-status" style="font-size:12.5px;color:var(--ink-soft);margin-bottom:12px;"></div>
        <div style="display:flex;gap:10px;flex-wrap:wrap;margin-bottom:18px;">
          <button class="btn" id="save-sync-url-btn">Save URL</button>
          <button class="btn secondary" id="sync-now-btn">Sync All Data Now</button>
        </div>
        <details>
          <summary style="cursor:pointer;font-size:13px;font-weight:600;color:var(--green);">How to set this up (one-time, ~2 minutes)</summary>
          <ol style="font-size:13px;color:var(--ink-soft);line-height:1.9;padding-left:20px;margin-top:10px;">
            <li>Create a new blank Google Sheet in your own Google account.</li>
            <li>In the Sheet, go to <strong>Extensions → Apps Script</strong>.</li>
            <li>Delete any starter code, paste in the script shown below, then click <strong>Save</strong>.</li>
            <li>Click <strong>Deploy → New deployment → type: Web app</strong>. Set "Execute as" to <strong>Me</strong> and "Who has access" to <strong>Anyone</strong>. Click <strong>Deploy</strong> and authorize it with your Google account.</li>
            <li>Copy the <strong>Web app URL</strong> Google gives you and paste it into the field above, then click <strong>Save URL</strong>.</li>
            <li>Click <strong>Sync All Data Now</strong> once to push everything you already have. After that, every new invoice or transaction syncs automatically.</li>
          </ol>
          <div style="position:relative;margin-top:10px;">
            <pre id="apps-script-code" style="background:#1B2420;color:#EDEAD9;padding:16px;border-radius:8px;font-size:11.5px;line-height:1.6;overflow-x:auto;white-space:pre;font-family:'IBM Plex Mono',monospace;">function doPost(e) {
  var data = JSON.parse(e.postData.contents);
  var ss = SpreadsheetApp.getActiveSpreadsheet();

  function getSheet(name, headers) {
    var sh = ss.getSheetByName(name);
    if (!sh) {
      sh = ss.insertSheet(name);
      sh.appendRow(headers);
    }
    return sh;
  }

  if (data.type === 'invoice') {
    var sh = getSheet('Invoices', ['Invoice No','Date','Customer','Phone','Tax Type','Taxable','CGST','SGST','IGST','Grand Total','Status']);
    var inv = data.record;
    sh.appendRow([inv.invoiceNo, inv.date, inv.customer.name, inv.customer.phone||'', inv.taxType, inv.taxable, inv.totalCgst, inv.totalSgst, inv.totalIgst, inv.grandTotal, inv.status]);
  } else if (data.type === 'transaction') {
    var sh = getSheet('Transactions', ['Date','Account','Account Number','Type','To Account','Note','Amount']);
    var t = data.record;
    sh.appendRow([t.date, t.accountName||'', t.accountNumber||'', t.type, t.toAccountName||'', t.note||'', t.amount]);
  } else if (data.type === 'fullsync') {
    var invSh = getSheet('Invoices', ['Invoice No','Date','Customer','Phone','Tax Type','Taxable','CGST','SGST','IGST','Grand Total','Status']);
    invSh.clearContents();
    invSh.appendRow(['Invoice No','Date','Customer','Phone','Tax Type','Taxable','CGST','SGST','IGST','Grand Total','Status']);
    (data.invoices||[]).forEach(function(inv){
      invSh.appendRow([inv.invoiceNo, inv.date, inv.customer.name, inv.customer.phone||'', inv.taxType, inv.taxable, inv.totalCgst, inv.totalSgst, inv.totalIgst, inv.grandTotal, inv.status]);
    });

    var accById = {};
    (data.paymentAccounts||[]).forEach(function(a){ accById[a.id] = a; });

    var accSh = getSheet('Accounts', ['Account Name','Type','Bank Name','Account Number','IFSC','Opening Balance']);
    accSh.clearContents();
    accSh.appendRow(['Account Name','Type','Bank Name','Account Number','IFSC','Opening Balance']);
    (data.paymentAccounts||[]).forEach(function(a){
      accSh.appendRow([a.name, a.kind, a.bankName||'', a.accountNumber||'', a.ifsc||'', a.openingBalance||0]);
    });

    var txSh = getSheet('Transactions', ['Date','Account','Account Number','Type','To Account','Note','Amount']);
    txSh.clearContents();
    txSh.appendRow(['Date','Account','Account Number','Type','To Account','Note','Amount']);
    (data.transactions||[]).forEach(function(t){
      var acc = accById[t.accountId] || {};
      var toAcc = accById[t.toAccountId] || {};
      txSh.appendRow([t.date, acc.name||'', acc.accountNumber||'', t.type, toAcc.name||'', t.note||'', t.amount]);
    });
  }

  return ContentService.createTextOutput(JSON.stringify({ok:true})).setMimeType(ContentService.MimeType.JSON);
}</pre>
            <button class="btn secondary" id="copy-script-btn" style="margin-top:8px;">Copy Script</button>
          </div>
        </details>
      </div>
    </section>

  </main>
</div>

<div class="toast" id="toast"></div>

<!-- Invoice view/print modal -->
<div class="modal-overlay" id="inv-modal-overlay">
  <div class="modal">
    <div id="inv-modal-body"></div>
    <div class="modal-close-row no-print">
      <button class="btn secondary" id="mark-paid-btn">Record Payment</button>
      <button class="btn secondary" onclick="window.print()">Print</button>
      <button class="btn" id="close-modal-btn">Close</button>
    </div>
  </div>
</div>

<!-- Record Payment modal -->
<div class="modal-overlay" id="payment-modal-overlay">
  <div class="modal" style="max-width:400px;">
    <h3 style="margin:0 0 6px 0;">Record Payment</h3>
    <div class="sub" style="margin-bottom:16px;color:var(--ink-soft);font-size:13px;" id="payment-modal-sub"></div>
    <div class="field">
      <label>Amount Received (₹)</label>
      <input id="payment-amount-input" type="number" min="0" step="0.01" placeholder="0.00">
    </div>
    <div class="sub" style="font-size:12px;color:var(--ink-soft);margin-bottom:6px;" id="payment-remaining-hint"></div>
    <div class="auth-error" id="payment-error" style="margin-top:6px;"></div>
    <div class="modal-close-row">
      <button class="btn secondary" id="payment-cancel-btn">Cancel</button>
      <button class="btn" id="payment-save-btn">Save Payment</button>
    </div>
  </div>
</div>

<!-- Workshop: Raw Materials for an item -->
<div class="modal-overlay" id="workshop-materials-modal-overlay">
  <div class="modal" style="max-width:860px;">
    <h3 style="margin:0 0 4px 0;" id="wm-title">Raw Materials</h3>
    <div class="sub" style="color:var(--ink-soft);font-size:12.5px;margin-bottom:16px;">List the raw materials that go into manufacturing this item. "Piece" shows how many pieces your current Stock can produce (Stock ÷ Per Piece). Use "+ Add Stock" whenever new material arrives for your running business.</div>
    <div class="workshop-material-head">
      <div>Material</div><div>Stock</div><div>+ Add Stock</div><div>Unit</div><div>Per Piece</div><div>Piece</div><div></div>
    </div>
    <div id="workshop-materials-list" style="display:flex;flex-direction:column;gap:8px;"></div>
    <button class="btn secondary" id="wm-add-material-btn" style="margin-top:14px;">+ Add Material</button>
    <div class="modal-close-row" style="justify-content:space-between;">
      <button class="btn secondary" id="wm-back-btn">Back</button>
      <button class="btn" id="wm-close-btn">Done</button>
    </div>
  </div>
</div>

<!-- Add product modal -->
<div class="modal-overlay" id="product-modal-overlay">
  <div class="modal" style="max-width:420px;">
    <h3 style="margin-top:0;" id="pm-title">Add Product</h3>
    <div class="field"><label>Name</label><input id="pm-name"></div>
    <div class="field"><label>Price (₹)</label><input id="pm-price" type="number" min="0" step="0.01"></div>
    <div class="field">
      <label>GST %</label>
      <select id="pm-gst">
        <option value="0">0%</option><option value="5">5%</option>
        <option value="18" selected>18%</option><option value="28">28%</option>
      </select>
    </div>
    <div class="modal-close-row">
      <button class="btn secondary" id="pm-cancel">Cancel</button>
      <button class="btn" id="pm-save">Save</button>
    </div>
  </div>
</div>

<!-- Confirm delete modal -->
<div class="modal-overlay" id="confirm-modal-overlay">
  <div class="modal" style="max-width:380px;">
    <h3 style="margin-top:0;" id="confirm-title">Are you sure?</h3>
    <p id="confirm-message" style="color:var(--ink-soft);font-size:14px;line-height:1.5;margin:0 0 6px 0;"></p>
    <div class="modal-close-row">
      <button class="btn secondary" id="confirm-no">No</button>
      <button class="btn red" id="confirm-yes">Yes, Remove</button>
    </div>
  </div>
</div>

<script>
/* ================= STATE ================= */
let profile = { name:"", address:"", gstin:"", state:"", pin:"", phone:"", logo:"", sheetSyncUrl:"" };
let products = [];
let invoices = [];
let transactions = [];
let paymentAccounts = [];
let workshopRawItems = [];
let workshopWorkers = [];
let workshopAttendance = [];
let workshopWagePayments = [];
let workshopOpeningBalances = {};
let itemRowId = 0;
let currentInvoiceIdForModal = null;
let editingAccountId = null;

const INDIA_STATES = [
  "West Bengal",
  "Andaman and Nicobar Islands","Andhra Pradesh","Arunachal Pradesh","Assam","Bihar",
  "Chandigarh","Chhattisgarh","Dadra and Nagar Haveli and Daman and Diu","Delhi","Goa",
  "Gujarat","Haryana","Himachal Pradesh","Jammu and Kashmir","Jharkhand","Karnataka",
  "Kerala","Ladakh","Lakshadweep","Madhya Pradesh","Maharashtra","Manipur","Meghalaya",
  "Mizoram","Nagaland","Odisha","Puducherry","Punjab","Rajasthan","Sikkim","Tamil Nadu",
  "Telangana","Tripura","Uttar Pradesh","Uttarakhand"
];
function populateStateSelect(selectEl, includeBlank){
  selectEl.innerHTML = (includeBlank ? '<option value="">Select State</option>' : '') +
    INDIA_STATES.map(s=>`<option value="${s}">${s}</option>`).join('');
}
function normalizeState(s){ return (s||'').trim().toLowerCase(); }

/* ================= STORAGE HELPERS (browser localStorage) ================= */
const LS_PREFIX = 'khata:';
const storage = {
  async get(key){
    const raw = localStorage.getItem(LS_PREFIX+key);
    return raw===null ? null : { key, value: raw };
  },
  async set(key, value){
    localStorage.setItem(LS_PREFIX+key, value);
    return { key, value };
  },
  async delete(key){
    localStorage.removeItem(LS_PREFIX+key);
    return { key, deleted:true };
  }
};
async function loadAll(){
  try{
    const p = await storage.get('business-profile');
    if(p) profile = JSON.parse(p.value);
  }catch(e){}
  try{
    const pr = await storage.get('products');
    if(pr) products = JSON.parse(pr.value);
  }catch(e){}
  try{
    const inv = await storage.get('invoices');
    if(inv) invoices = JSON.parse(inv.value);
  }catch(e){}
  try{
    const t = await storage.get('transactions');
    if(t) transactions = JSON.parse(t.value);
  }catch(e){}
  try{
    const pa = await storage.get('payment-accounts');
    if(pa) paymentAccounts = JSON.parse(pa.value);
  }catch(e){}
  try{
    const wr = await storage.get('workshop-raw-items');
    workshopRawItems = wr ? JSON.parse(wr.value) : [{name:'Wheel hoe',materials:[]},{name:'Plunger',materials:[]},{name:'Hand Pump',materials:[]}];
  }catch(e){ workshopRawItems = [{name:'Wheel hoe',materials:[]},{name:'Plunger',materials:[]},{name:'Hand Pump',materials:[]}]; }
  try{
    const ww = await storage.get('workshop-workers');
    workshopWorkers = ww ? JSON.parse(ww.value) : [];
  }catch(e){ workshopWorkers = []; }
  try{
    const wa = await storage.get('workshop-attendance');
    workshopAttendance = wa ? JSON.parse(wa.value) : [];
  }catch(e){ workshopAttendance = []; }
  try{
    const wp = await storage.get('workshop-wage-payments');
    workshopWagePayments = wp ? JSON.parse(wp.value) : [];
  }catch(e){ workshopWagePayments = []; }
  try{
    const wob = await storage.get('workshop-opening-balances');
    workshopOpeningBalances = wob ? JSON.parse(wob.value) : {};
  }catch(e){ workshopOpeningBalances = {}; }
  ensureDefaultAccount();
  await migrateLegacyGstRate();
}
async function saveWorkshopRawItems(){
  try{ await storage.set('workshop-raw-items', JSON.stringify(workshopRawItems)); }catch(e){ showToast('Could not save raw materials list'); }
}
async function saveWorkshopWorkers(){
  try{ await storage.set('workshop-workers', JSON.stringify(workshopWorkers)); }catch(e){ showToast('Could not save worker list'); }
}
async function saveWorkshopAttendance(){
  try{ await storage.set('workshop-attendance', JSON.stringify(workshopAttendance)); }catch(e){ showToast('Could not save attendance'); }
}
async function saveWorkshopWagePayments(){
  try{ await storage.set('workshop-wage-payments', JSON.stringify(workshopWagePayments)); }catch(e){ showToast('Could not save payment record'); }
}
async function saveWorkshopOpeningBalances(){
  try{ await storage.set('workshop-opening-balances', JSON.stringify(workshopOpeningBalances)); }catch(e){ showToast('Could not save opening balance'); }
}
async function migrateLegacyGstRate(){
  // The 12% GST slab was removed by the Government of India. Any product still
  // saved at the old 12% rate is moved to 18% so it stays usable in new invoices;
  // past invoices already issued are left untouched since they reflect what was
  // actually charged at the time.
  const affected = products.filter(p=>p.gstRate===12 || p.gstRate==='12');
  if(affected.length===0) return;
  products.forEach(p=>{ if(p.gstRate===12 || p.gstRate==='12') p.gstRate = 18; });
  await saveProducts();
  showToast(`${affected.length} product(s) using the removed 12% GST slab were updated to 18% — please review`);
}
function ensureDefaultAccount(){
  if(!Array.isArray(paymentAccounts) || paymentAccounts.length===0){
    paymentAccounts = [{ id:'acc_cash', name:'Cash in Hand', kind:'cash', bankName:'', accountNumber:'', ifsc:'', openingBalance:0, createdAt:Date.now() }];
  }
  const validIds = new Set(paymentAccounts.map(a=>a.id));
  let migrated = false;
  transactions.forEach(t=>{
    if(!t.accountId || !validIds.has(t.accountId)){
      t.accountId = paymentAccounts[0].id;
      migrated = true;
    }
  });
  if(migrated) saveTransactions();
}
async function saveProfile(){
  try{ await storage.set('business-profile', JSON.stringify(profile)); }catch(e){ showToast('Could not save profile'); }
}
async function saveProducts(){
  try{ await storage.set('products', JSON.stringify(products)); }catch(e){ showToast('Could not save products'); }
}
async function saveInvoices(){
  try{ await storage.set('invoices', JSON.stringify(invoices)); }catch(e){ showToast('Could not save invoice'); }
}
async function saveTransactions(){
  try{ await storage.set('transactions', JSON.stringify(transactions)); }catch(e){ showToast('Could not save transaction'); }
}
async function saveAccountsList(){
  try{ await storage.set('payment-accounts', JSON.stringify(paymentAccounts)); }catch(e){ showToast('Could not save accounts'); }
}

/* ================= AUTH: REGISTER / LOGIN / SESSION ================= */
async function getAuthUser(){
  try{
    const r = await storage.get('auth-user');
    return r ? JSON.parse(r.value) : null;
  }catch(e){ return null; }
}
async function saveAuthUser(user){
  await storage.set('auth-user', JSON.stringify(user));
}
async function clearAuthUser(){
  try{ await storage.delete('auth-user'); }catch(e){}
}
async function getRememberedCreds(){
  try{
    const r = await storage.get('remember-credentials');
    return r ? JSON.parse(r.value) : null;
  }catch(e){ return null; }
}
async function setSessionActive(active){
  if(active) await storage.set('session-active', 'true');
  else { try{ await storage.delete('session-active'); }catch(e){} }
}
async function isSessionActive(){
  try{
    const r = await storage.get('session-active');
    return !!r && r.value === 'true';
  }catch(e){ return false; }
}

function showAuthScreen(which){
  document.getElementById('auth-register-screen').style.display = which==='register' ? 'flex' : 'none';
  document.getElementById('auth-login-screen').style.display = which==='login' ? 'flex' : 'none';
  document.getElementById('main-app').style.display = which==='app' ? 'flex' : 'none';
  if(which==='register') revealRegisterGate();
  if(which==='login') revealLoginGate();
}
function revealRegisterGate(){
  document.getElementById('reg-gate').style.display = 'block';
  document.getElementById('reg-form-body').style.display = 'none';
}
function revealRegisterForm(){
  document.getElementById('reg-gate').style.display = 'none';
  document.getElementById('reg-form-body').style.display = 'block';
  document.getElementById('reg-name').focus();
}
function revealLoginGate(){
  document.getElementById('login-gate').style.display = 'block';
  document.getElementById('login-form-body').style.display = 'none';
}
function revealLoginForm(){
  document.getElementById('login-gate').style.display = 'none';
  document.getElementById('login-form-body').style.display = 'block';
  document.getElementById('login-username').focus();
}
function populateLoginUsernameList(username){
  const list = document.getElementById('login-username-list');
  list.innerHTML = username ? `<option value="${escapeHtml(username)}"></option>` : '';
}

document.getElementById('reg-gate-no').addEventListener('click', revealRegisterForm);
document.getElementById('reg-gate-yes').addEventListener('click', ()=>{
  showAuthScreen('login');
  revealLoginForm();
});
document.getElementById('login-gate-yes').addEventListener('click', revealLoginForm);
document.getElementById('login-gate-no').addEventListener('click', ()=>{
  showAuthScreen('register');
  revealRegisterForm();
});
document.getElementById('reg-switch-to-login').addEventListener('click', (e)=>{
  e.preventDefault();
  showAuthScreen('login');
  revealLoginForm();
});
document.getElementById('login-switch-to-register').addEventListener('click', (e)=>{
  e.preventDefault();
  showAuthScreen('register');
  revealRegisterForm();
});

function markFieldError(fieldId, isError){
  document.getElementById(fieldId).classList.toggle('input-error', isError);
}
function clearFieldErrorOnInput(inputId, fieldId){
  document.getElementById(inputId).addEventListener('input', ()=> markFieldError(fieldId, false));
}
clearFieldErrorOnInput('reg-name','reg-name-field');
clearFieldErrorOnInput('reg-username','reg-username-field');
clearFieldErrorOnInput('reg-password','reg-password-field');
clearFieldErrorOnInput('reg-password2','reg-password2-field');
clearFieldErrorOnInput('login-username','login-username-field');
clearFieldErrorOnInput('login-password','login-password-field');

document.getElementById('reg-submit-btn').addEventListener('click', async ()=>{
  const name = document.getElementById('reg-name').value.trim();
  const username = document.getElementById('reg-username').value.trim();
  const password = document.getElementById('reg-password').value;
  const password2 = document.getElementById('reg-password2').value;
  const errEl = document.getElementById('reg-error');
  errEl.textContent = '';
  ['reg-name-field','reg-username-field','reg-password-field','reg-password2-field'].forEach(id=>markFieldError(id,false));

  const missing = [];
  if(!name) missing.push('reg-name-field');
  if(!username) missing.push('reg-username-field');
  if(!password) missing.push('reg-password-field');
  if(!password2) missing.push('reg-password2-field');
  if(missing.length>0){
    missing.forEach(id=>markFieldError(id,true));
    errEl.textContent = 'Please fill in all the required fields marked with *.';
    return;
  }
  if(password.length<4){ markFieldError('reg-password-field',true); errEl.textContent = 'Password should be at least 4 characters.'; return; }
  if(password !== password2){ markFieldError('reg-password-field',true); markFieldError('reg-password2-field',true); errEl.textContent = 'Passwords do not match.'; return; }
  await saveAuthUser({ name, username, password });
  populateLoginUsernameList(username);
  showToast('Account created — please log in');
  document.getElementById('login-welcome-sub').textContent = `Welcome, ${name}! Log in to continue.`;
  document.getElementById('login-username').value = username;
  document.getElementById('login-password').value = '';
  showAuthScreen('login');
  revealLoginForm();
});

document.getElementById('login-submit-btn').addEventListener('click', async ()=>{
  const username = document.getElementById('login-username').value.trim();
  const password = document.getElementById('login-password').value;
  const remember = document.getElementById('login-remember').checked;
  const errEl = document.getElementById('login-error');
  errEl.textContent = '';
  markFieldError('login-username-field', false);
  markFieldError('login-password-field', false);

  const missing = [];
  if(!username) missing.push('login-username-field');
  if(!password) missing.push('login-password-field');
  if(missing.length>0){
    missing.forEach(id=>markFieldError(id,true));
    errEl.textContent = 'Please fill in all the required fields marked with *.';
    return;
  }
  const user = await getAuthUser();
  if(!user){ showAuthScreen('register'); return; }
  if(username !== user.username || password !== user.password){
    errEl.textContent = 'Incorrect username or password.';
    return;
  }
  if(remember){
    await storage.set('remember-credentials', JSON.stringify({ username, password }));
  } else {
    try{ await storage.delete('remember-credentials'); }catch(e){}
  }
  await setSessionActive(true);
  await enterApp();
});
document.getElementById('login-username').addEventListener('keydown', (e)=>{ if(e.key==='Enter') document.getElementById('login-password').focus(); });
document.getElementById('login-password').addEventListener('keydown', (e)=>{ if(e.key==='Enter') document.getElementById('login-submit-btn').click(); });
document.getElementById('reg-password2').addEventListener('keydown', (e)=>{ if(e.key==='Enter') document.getElementById('reg-submit-btn').click(); });

document.getElementById('login-reset-link').addEventListener('click', async (e)=>{
  e.preventDefault();
  const ok = await confirmAction('Reset your login? You will need to register a new username and password. Your invoices, products, and accounts data will NOT be deleted.', 'Reset Account');
  if(!ok) return;
  await clearAuthUser();
  try{ await storage.delete('remember-credentials'); }catch(err){}
  await setSessionActive(false);
  populateLoginUsernameList('');
  document.getElementById('reg-name').value = '';
  document.getElementById('reg-username').value = '';
  document.getElementById('reg-password').value = '';
  document.getElementById('reg-password2').value = '';
  showAuthScreen('register');
  revealRegisterForm();
});

document.getElementById('logout-btn').addEventListener('click', async ()=>{
  const ok = await confirmAction('Log out of Khata on this device?', 'Log Out');
  if(!ok) return;
  await setSessionActive(false);
  document.getElementById('login-password').value = '';
  document.getElementById('login-error').textContent = '';
  document.getElementById('login-welcome-sub').textContent = 'Log in to continue to your ledger.';
  showAuthScreen('login');
});

async function enterApp(){
  showAuthScreen('app');
  const healthy = await checkStorageHealth();
  if(!healthy){
    document.getElementById('storage-warning').style.display = 'block';
  }
  await loadAll();
  renderDashboard();
  addItemRow();
  recalc();
}

async function initAuth(){
  const user = await getAuthUser();
  if(!user){
    showAuthScreen('register');
    return;
  }
  populateLoginUsernameList(user.username);
  const remembered = await getRememberedCreds();
  if(remembered){
    document.getElementById('login-username').value = remembered.username||'';
    document.getElementById('login-password').value = remembered.password||'';
    document.getElementById('login-remember').checked = true;
  }
  if(await isSessionActive()){
    await enterApp();
  } else {
    showAuthScreen('login');
    if(remembered) revealLoginForm();
  }
}

/* ================= BACKUP: EXPORT / IMPORT ================= */
function exportBackup(){
  const backup = {
    exportedAt: new Date().toISOString(),
    profile: profile,
    products: products,
    invoices: invoices,
    transactions: transactions,
    paymentAccounts: paymentAccounts
  };
  const blob = new Blob([JSON.stringify(backup, null, 2)], {type:'application/json'});
  const url = URL.createObjectURL(blob);
  const a = document.createElement('a');
  const stamp = todayISO();
  a.href = url;
  a.download = `khata-backup-${stamp}.json`;
  document.body.appendChild(a);
  a.click();
  a.remove();
  URL.revokeObjectURL(url);
  showToast('Backup exported');
}

async function readFileAsText(file){
  if(typeof file.text === 'function'){
    try{ return await file.text(); }catch(e){ /* fall through to FileReader */ }
  }
  return new Promise((resolve, reject)=>{
    const reader = new FileReader();
    reader.onload = ()=> resolve(reader.result);
    reader.onerror = ()=> reject(reader.error);
    reader.readAsText(file);
  });
}

async function importBackupFromFile(file){
  try{
    const text = await readFileAsText(file);
    const data = JSON.parse(text);

    if(typeof data !== 'object' || data === null){
      showToast('Invalid backup file');
      return;
    }

    const hasProfile = data.profile && typeof data.profile === 'object';
    const hasProducts = Array.isArray(data.products);
    const hasInvoices = Array.isArray(data.invoices);
    const hasTransactions = Array.isArray(data.transactions);
    const hasAccounts = Array.isArray(data.paymentAccounts);

    if(!hasProfile && !hasProducts && !hasInvoices && !hasTransactions && !hasAccounts){
      showToast('This file does not look like a Khata backup');
      return;
    }

    if(hasProfile){
      profile = Object.assign({ name:"", address:"", gstin:"", state:"", pin:"", phone:"", logo:"", sheetSyncUrl:"" }, data.profile);
      await saveProfile();
    }
    if(hasProducts){
      products = data.products;
      await saveProducts();
    }
    if(hasInvoices){
      invoices = data.invoices;
      await saveInvoices();
    }
    if(hasTransactions){
      transactions = data.transactions;
      await saveTransactions();
    }
    if(hasAccounts){
      paymentAccounts = data.paymentAccounts;
      ensureDefaultAccount();
      await saveAccountsList();
    }

    renderProfileForm();
    renderProducts();
    renderDashboard();
    renderHistory();
    renderGst();
    renderAccounts();

    showToast('Backup imported successfully');
  }catch(e){
    showToast('Could not read backup file — is it valid JSON?');
  }
}

document.getElementById('export-backup-btn').addEventListener('click', exportBackup);
document.getElementById('import-backup-btn').addEventListener('click', ()=>{
  document.getElementById('import-backup-input').click();
});
document.getElementById('import-backup-input').addEventListener('change', (e)=>{
  const file = e.target.files[0];
  if(!file) return;
  importBackupFromFile(file);
  e.target.value = '';
});

/* ================= GOOGLE SHEET SYNC ================= */
function setSyncStatus(msg, isError){
  const el = document.getElementById('sheet-sync-status');
  el.textContent = msg;
  el.style.color = isError ? 'var(--red)' : 'var(--ink-soft)';
}
async function pushToSheet(payload){
  const url = (profile.sheetSyncUrl||'').trim();
  if(!url) return;
  try{
    await fetch(url, {
      method:'POST',
      mode:'no-cors',
      headers:{'Content-Type':'text/plain;charset=utf-8'},
      body: JSON.stringify(payload)
    });
    // no-cors means we can't read the response, so we optimistically assume success.
  }catch(e){
    showToast('Saved locally, but Google Sheet sync failed — check your connection or Web App URL');
  }
}
document.getElementById('save-sync-url-btn').addEventListener('click', async ()=>{
  profile.sheetSyncUrl = document.getElementById('sheet-sync-url').value.trim();
  await saveProfile();
  setSyncStatus(profile.sheetSyncUrl ? 'URL saved. Click "Sync All Data Now" to push existing data.' : 'Sync URL cleared — auto-sync is off.');
  showToast('Sync settings saved');
});
document.getElementById('sync-now-btn').addEventListener('click', async ()=>{
  const url = (document.getElementById('sheet-sync-url').value||'').trim();
  if(!url){ showToast('Enter and save your Web App URL first'); return; }
  profile.sheetSyncUrl = url;
  await saveProfile();
  setSyncStatus('Syncing all data…');
  await pushToSheet({ type:'fullsync', invoices, transactions, paymentAccounts });
  setSyncStatus('Full sync sent. Open your Google Sheet to confirm the rows appeared.');
  showToast('Full sync sent to Google Sheet');
});
document.getElementById('copy-script-btn').addEventListener('click', async ()=>{
  const code = document.getElementById('apps-script-code').textContent;
  try{
    await navigator.clipboard.writeText(code);
    showToast('Script copied to clipboard');
  }catch(e){
    showToast('Could not copy — please select and copy manually');
  }
});

/* ================= UTIL ================= */
function fmt(n){ return '₹' + (Math.round(n*100)/100).toLocaleString('en-IN', {minimumFractionDigits:2, maximumFractionDigits:2}); }
function todayISO(){ return new Date().toISOString().slice(0,10); }
function showToast(msg){
  const t = document.getElementById('toast');
  t.textContent = msg; t.classList.add('show');
  setTimeout(()=>t.classList.remove('show'), 2200);
}
function nextInvoiceNo(){
  const n = invoices.length + 1;
  const yr = new Date().getFullYear();
  return `INV-${yr}-${String(n).padStart(4,'0')}`;
}

/* ================= NAVIGATION ================= */
document.querySelectorAll('.nav-item').forEach(btn=>{
  btn.addEventListener('click', ()=>gotoPage(btn.dataset.page));
});
document.querySelectorAll('[data-goto]').forEach(btn=>{
  btn.addEventListener('click', ()=>gotoPage(btn.dataset.goto));
});
function gotoPage(name){
  document.querySelectorAll('.nav-item').forEach(b=>b.classList.toggle('active', b.dataset.page===name));
  document.querySelectorAll('.page').forEach(p=>p.classList.toggle('active', p.id==='page-'+name));
  if(name==='dashboard') renderDashboard();
  if(name==='history') renderHistory();
  if(name==='gst') renderGst();
  if(name==='products') renderProducts();
  if(name==='accounts') renderAccounts();
  if(name==='workshop') renderWorkshop();
  if(name==='profile') renderProfileForm();
}

/* ================= WORKSHOP ================= */
function switchWorkshopTab(tab){
  document.getElementById('workshop-tab-raw').className = tab==='raw' ? 'btn' : 'btn secondary';
  document.getElementById('workshop-tab-finished').className = tab==='finished' ? 'btn' : 'btn secondary';
  document.getElementById('workshop-tab-workers').className = tab==='workers' ? 'btn' : 'btn secondary';
  document.getElementById('workshop-raw-panel').style.display = tab==='raw' ? 'block' : 'none';
  document.getElementById('workshop-finished-panel').style.display = tab==='finished' ? 'block' : 'none';
  document.getElementById('workshop-workers-panel').style.display = tab==='workers' ? 'block' : 'none';
}
document.getElementById('workshop-tab-raw').addEventListener('click', ()=>switchWorkshopTab('raw'));
document.getElementById('workshop-tab-finished').addEventListener('click', ()=>switchWorkshopTab('finished'));
document.getElementById('workshop-tab-workers').addEventListener('click', ()=>switchWorkshopTab('workers'));

function renderWorkshopItemsList(containerId, items, saveFn, withMaterialsBtn){
  const box = document.getElementById(containerId);
  box.innerHTML = '';
  if(items.length===0){
    box.innerHTML = `<div class="workshop-items-empty">No items yet — click "+ Add Item" below to add one.</div>`;
    return;
  }
  items.forEach((item, idx)=>{
    const row = document.createElement('div');
    row.className = 'workshop-item-row';

    const num = document.createElement('div');
    num.className = 'workshop-item-number';
    num.textContent = (idx+1) + ')';

    const input = document.createElement('input');
    input.type = 'text';
    input.value = item.name || '';
    input.placeholder = 'Item name';
    input.addEventListener('input', ()=>{ item.name = input.value; saveFn(); renderFinishedProductsList(); });

    row.appendChild(num);
    row.appendChild(input);

    if(withMaterialsBtn){
      const matCount = (item.materials||[]).length;
      const matBtn = document.createElement('button');
      matBtn.className = 'btn ghost workshop-item-materials-btn';
      matBtn.textContent = 'Raw Materials' + (matCount>0 ? ` (${matCount})` : '');
      matBtn.addEventListener('click', ()=> openMaterialsModal(item));
      row.appendChild(matBtn);
    }

    const removeBtn = document.createElement('button');
    removeBtn.className = 'btn ghost workshop-item-remove';
    removeBtn.textContent = 'Remove';
    removeBtn.addEventListener('click', ()=>{
      const i = items.indexOf(item);
      if(i>-1) items.splice(i,1);
      renderWorkshopItemsList(containerId, items, saveFn, withMaterialsBtn);
      saveFn();
      renderFinishedProductsList();
    });
    row.appendChild(removeBtn);

    box.appendChild(row);
  });
}
// When Stock Available for a finished item increases (a new batch is produced), the raw
// materials it's made from are consumed proportionally — deduct = amount produced × Per Piece,
// for every material listed under that item's Raw Materials. If a material doesn't have enough
// stock to support the batch, production is blocked so raw material stock never goes negative.
function checkAndConsumeRawMaterials(item, amount){
  const materials = item.materials || [];
  if(materials.length===0) return { ok:true };
  let maxPieces = Infinity;
  let limitingMaterial = null;
  materials.forEach(m=>{
    const perPiece = parseFloat(m.perPiece)||0;
    if(perPiece>0){
      const stock = parseFloat(m.qty)||0;
      const possible = stock/perPiece;
      if(possible < maxPieces){ maxPieces = possible; limitingMaterial = m; }
    }
  });
  if(maxPieces===Infinity) return { ok:true }; // no material has a Per Piece rate set — can't constrain
  if(amount > maxPieces + 0.001){
    return { ok:false, limitingMaterial, maxPieces: Math.floor(maxPieces*100)/100 };
  }
  materials.forEach(m=>{
    const perPiece = parseFloat(m.perPiece)||0;
    if(perPiece>0){
      const stock = parseFloat(m.qty)||0;
      m.qty = Math.round((stock - amount*perPiece)*100)/100;
    }
  });
  return { ok:true };
}
// Read-only version of the same calculation, for display: the maximum number of pieces
// currently producible, limited by whichever raw material would run out first.
function computeMaxProducible(item){
  const materials = item.materials || [];
  if(materials.length===0) return null;
  let maxPieces = Infinity;
  materials.forEach(m=>{
    const perPiece = parseFloat(m.perPiece)||0;
    if(perPiece>0){
      const stock = parseFloat(m.qty)||0;
      const possible = stock/perPiece;
      if(possible < maxPieces) maxPieces = possible;
    }
  });
  if(maxPieces===Infinity) return null;
  return Math.floor(maxPieces);
}
function renderFinishedProductsList(){
  const box = document.getElementById('workshop-finished-items-list');
  box.innerHTML = '';
  if(workshopRawItems.length===0){
    box.innerHTML = `<div class="workshop-items-empty">No items yet — add items in the Raw Materials tab first.</div>`;
    return;
  }
  workshopRawItems.forEach((item, idx)=>{
    const row = document.createElement('div');
    row.className = 'workshop-item-row';

    const num = document.createElement('div');
    num.className = 'workshop-item-number';
    num.textContent = (idx+1) + ')';

    const label = document.createElement('div');
    label.style.cssText = 'flex:1;font-size:14px;color:var(--ink);padding:8px 0;';
    label.textContent = item.name || '(Unnamed item)';

    const rawAvailValue = document.createElement('div');
    rawAvailValue.className = 'workshop-rawavail-value';
    const maxProducible = computeMaxProducible(item);
    if(maxProducible===null){
      rawAvailValue.textContent = '—';
      rawAvailValue.style.color = 'var(--ink-soft)';
      rawAvailValue.title = 'No raw materials with a Per Piece rate set yet for this item';
    } else {
      rawAvailValue.textContent = maxProducible + ' pcs';
      rawAvailValue.style.color = maxProducible<=0 ? 'var(--red)' : 'var(--ink)';
      rawAvailValue.title = 'Maximum pieces that can currently be made, limited by the raw material with the least stock relative to its Per Piece requirement';
    }

    const qtyWrap = document.createElement('div');
    qtyWrap.style.cssText = 'display:flex;align-items:center;flex-shrink:0;width:118px;';
    const qtyInput = document.createElement('input');
    qtyInput.type = 'number';
    qtyInput.min = '0';
    qtyInput.step = '1';
    qtyInput.className = 'workshop-finished-qty';
    qtyInput.placeholder = '0';
    qtyInput.title = 'Number of finished pieces reported by the workshop';
    qtyInput.value = (item.finishedQty===undefined || item.finishedQty===null || item.finishedQty==='') ? '' : item.finishedQty;
    qtyInput.addEventListener('input', ()=>{
      item.finishedQty = qtyInput.value===''?'':parseFloat(qtyInput.value)||0;
      saveWorkshopRawItems();
    });
    const qtyUnit = document.createElement('span');
    qtyUnit.className = 'workshop-finished-qty-unit';
    qtyUnit.textContent = 'pcs';

    qtyWrap.appendChild(qtyInput);
    qtyWrap.appendChild(qtyUnit);

    const addWrap = document.createElement('div');
    addWrap.className = 'workshop-finished-add-wrap';
    const addInput = document.createElement('input');
    addInput.type = 'number';
    addInput.min = '0';
    addInput.step = '1';
    addInput.className = 'workshop-finished-add-input';
    addInput.placeholder = '+ add';
    addInput.title = 'Enter a newly reported batch quantity, then click Add';
    const addBtn = document.createElement('button');
    addBtn.type = 'button';
    addBtn.className = 'btn workshop-finished-add-btn';
    addBtn.textContent = 'Add';
    function commitAdd(){
      const amount = parseFloat(addInput.value);
      if(!isFinite(amount) || amount<=0) return;
      const check = checkAndConsumeRawMaterials(item, amount);
      if(!check.ok){
        showToast(`Not enough ${check.limitingMaterial.name||'raw material'} in stock — can only make ${check.maxPieces} more pcs of ${item.name||'this item'}`);
        return;
      }
      const newTotal = (parseFloat(item.finishedQty)||0) + amount;
      item.finishedQty = Math.round(newTotal*100)/100;
      qtyInput.value = item.finishedQty;
      addInput.value = '';
      saveWorkshopRawItems();
      showToast(`Added ${amount} pcs to ${item.name||'this item'} — new total: ${item.finishedQty}`);
      renderFinishedProductsList();
    }
    addBtn.addEventListener('click', commitAdd);
    addInput.addEventListener('keydown', (e)=>{ if(e.key==='Enter') commitAdd(); });
    addWrap.appendChild(addInput);
    addWrap.appendChild(addBtn);

    const sellWrap = document.createElement('div');
    sellWrap.className = 'workshop-finished-sell-wrap';
    const sellInput = document.createElement('input');
    sellInput.type = 'number';
    sellInput.min = '0';
    sellInput.step = '1';
    sellInput.className = 'workshop-finished-sell-input';
    sellInput.placeholder = 'qty';
    sellInput.title = 'Enter the quantity sold out, then click Sell Out to deduct it from Stock Available';
    const sellBtn = document.createElement('button');
    sellBtn.type = 'button';
    sellBtn.className = 'btn workshop-finished-sell-btn';
    sellBtn.textContent = 'Sell Out';
    function commitSellOut(){
      const amount = parseFloat(sellInput.value);
      if(!isFinite(amount) || amount<=0) return;
      const currentStock = parseFloat(item.finishedQty)||0;
      if(amount > currentStock){
        showToast(`Only ${currentStock} pcs of ${item.name||'this item'} in stock — cannot sell out ${amount}`);
        return;
      }
      const newTotal = Math.round((currentStock-amount)*100)/100;
      item.finishedQty = newTotal;
      qtyInput.value = item.finishedQty;
      sellInput.value = '';
      saveWorkshopRawItems();
      showToast(`Sold out ${amount} pcs of ${item.name||'this item'} — remaining stock: ${item.finishedQty}`);
    }
    sellBtn.addEventListener('click', commitSellOut);
    sellInput.addEventListener('keydown', (e)=>{ if(e.key==='Enter') commitSellOut(); });
    sellWrap.appendChild(sellInput);
    sellWrap.appendChild(sellBtn);

    row.appendChild(num);
    row.appendChild(label);
    row.appendChild(rawAvailValue);
    row.appendChild(qtyWrap);
    row.appendChild(addWrap);
    row.appendChild(sellWrap);
    box.appendChild(row);
  });
}
function renderWorkshop(){
  renderWorkshopItemsList('workshop-raw-items-list', workshopRawItems, saveWorkshopRawItems, true);
  renderFinishedProductsList();
  renderWorkers();
}
document.getElementById('workshop-raw-add-btn').addEventListener('click', async ()=>{
  workshopRawItems.push({name:'', materials:[]});
  await saveWorkshopRawItems();
  renderWorkshopItemsList('workshop-raw-items-list', workshopRawItems, saveWorkshopRawItems, true);
  renderFinishedProductsList();
  const inputs = document.querySelectorAll('#workshop-raw-items-list input');
  if(inputs.length) inputs[inputs.length-1].focus();
});

/* ---- Workers: wage math helpers ---- */
function timeToMinutes(t){
  if(!t) return null;
  const parts = t.split(':');
  const h = parseInt(parts[0],10), m = parseInt(parts[1],10);
  if(isNaN(h) || isNaN(m)) return null;
  return h*60+m;
}
function computeAttendanceStats(worker, entry){
  if(!worker) return { status:'Absent', hoursWorked:0, otHours:0, deduction:0, wage:0 };
  const base = parseFloat(worker.wagePerDay)||0;
  const inMin = timeToMinutes(entry.inTime);
  const outMin = timeToMinutes(entry.outTime);

  if(inMin===null || outMin===null || outMin<=inMin){
    return { status:'Absent', hoursWorked:0, otHours:0, deduction:0, wage:0 };
  }
  const hoursWorked = Math.round(((outMin-inMin)/60)*100)/100;

  const normInMin = timeToMinutes(worker.normalInTime);
  const normOutMin = timeToMinutes(worker.normalOutTime);
  // Overtime and deductions are calculated strictly from this worker's own Normal In/Out time.
  // If it isn't set yet, fall back to a standard 8-hour day so calculations don't break.
  const normalHours = (normInMin!==null && normOutMin!==null && normOutMin>normInMin)
    ? Math.round(((normOutMin-normInMin)/60)*100)/100
    : 8;
  // Wages per hour (or any fractional part of an hour) — this single rate is used both to
  // pay overtime and to deduct pay for hours short of the normal shift.
  const hourlyRate = normalHours>0 ? base/normalHours : 0;

  let otHours = 0, deduction = 0, regularPay = base;
  if(hoursWorked >= normalHours){
    otHours = Math.round((hoursWorked-normalHours)*100)/100;
  } else {
    const shortfallHours = Math.round((normalHours-hoursWorked)*100)/100;
    deduction = Math.round((shortfallHours*hourlyRate)*100)/100;
    regularPay = Math.round((base-deduction)*100)/100;
  }
  const otPay = Math.round((otHours*hourlyRate)*100)/100;
  const wage = Math.round((regularPay+otPay)*100)/100;
  const status = hoursWorked>=normalHours ? 'Present' : 'Partial';
  return { status, hoursWorked, otHours, deduction, wage };
}
function computeDailyWage(worker, entry){
  return computeAttendanceStats(worker, entry).wage;
}
function getMondayISO(d){
  const date = new Date(d);
  const day = date.getDay(); // 0=Sun..6=Sat
  const diff = (day===0 ? -6 : 1-day);
  date.setDate(date.getDate()+diff);
  return date.toISOString().slice(0,10);
}
function workerWeekPaid(workerId, startISO){
  const start = new Date(startISO);
  const end = new Date(start); end.setDate(end.getDate()+6);
  return Math.round(workshopWagePayments.filter(p=>{
    if(p.workerId!==workerId) return false;
    const d = new Date(p.date);
    return d>=start && d<=end;
  }).reduce((s,p)=>s+(parseFloat(p.amount)||0),0)*100)/100;
}
function workerWeekStats(workerId, startISO){
  const worker = workshopWorkers.find(w=>w.id===workerId);
  const start = new Date(startISO);
  const end = new Date(start); end.setDate(end.getDate()+6);
  const entries = workshopAttendance.filter(a=>{
    if(a.workerId!==workerId) return false;
    const d = new Date(a.date);
    return d>=start && d<=end;
  });
  const statsList = entries.map(e=>computeAttendanceStats(worker,e));
  const daysPresent = statsList.filter(s=>s.hoursWorked>0).length;
  const otHours = Math.round(statsList.reduce((s,st)=>s+st.otHours,0)*100)/100;
  const wage = Math.round(statsList.reduce((s,st)=>s+st.wage,0)*100)/100;
  return { daysPresent, otHours, wage };
}

/* ---- Worker Directory ---- */
function renderWorkerDirectory(){
  const box = document.getElementById('workshop-workers-list');
  box.innerHTML = '';
  if(workshopWorkers.length===0){
    box.innerHTML = `<div class="workshop-items-empty">No workers yet — click "+ Add Worker" below to add one.</div>`;
    return;
  }
  workshopWorkers.forEach((w, idx)=>{
    const row = document.createElement('div');
    row.className = 'workshop-worker-row';

    const num = document.createElement('div');
    num.className = 'workshop-item-number';
    num.textContent = (idx+1) + ')';

    const nameInput = document.createElement('input');
    nameInput.type = 'text';
    nameInput.placeholder = 'Worker name';
    nameInput.value = w.name || '';
    nameInput.addEventListener('input', ()=>{ w.name = nameInput.value; saveWorkshopWorkers(); renderWeeklySummary(); });

    const wageInput = document.createElement('input');
    wageInput.type = 'number';
    wageInput.min = '0';
    wageInput.step = '1';
    wageInput.className = 'workshop-worker-wage-input';
    wageInput.placeholder = '0.00';
    wageInput.value = (w.wagePerDay===undefined || w.wagePerDay===null || w.wagePerDay==='') ? '' : w.wagePerDay;
    wageInput.addEventListener('input', ()=>{
      w.wagePerDay = wageInput.value===''?'':parseFloat(wageInput.value)||0;
      saveWorkshopWorkers();
      renderWeeklySummary();
    });

    const normalInInput = document.createElement('input');
    normalInInput.type = 'time';
    normalInInput.title = 'Normal arrival time for this worker\'s standard shift';
    normalInInput.value = w.normalInTime || '';
    normalInInput.addEventListener('input', ()=>{
      w.normalInTime = normalInInput.value;
      saveWorkshopWorkers();
      renderWeeklySummary();
    });

    const normalOutInput = document.createElement('input');
    normalOutInput.type = 'time';
    normalOutInput.title = 'Normal departure time for this worker\'s standard shift';
    normalOutInput.value = w.normalOutTime || '';
    normalOutInput.addEventListener('input', ()=>{
      w.normalOutTime = normalOutInput.value;
      saveWorkshopWorkers();
      renderWeeklySummary();
    });

    const attBtn = document.createElement('button');
    attBtn.className = 'btn ghost workshop-worker-attendance-btn';
    attBtn.textContent = 'Attendance';
    attBtn.addEventListener('click', ()=> openAttendanceModal(w.id));

    const removeBtn = document.createElement('button');
    removeBtn.className = 'btn ghost workshop-worker-remove';
    removeBtn.textContent = 'Remove';
    removeBtn.addEventListener('click', async ()=>{
      const hasData = workshopAttendance.some(a=>a.workerId===w.id) || workshopWagePayments.some(p=>p.workerId===w.id);
      if(hasData){
        const ok = await confirmAction(`Remove ${w.name||'this worker'}? Their attendance and payment history will also be deleted. This cannot be undone.`, 'Remove Worker');
        if(!ok) return;
      }
      workshopWorkers = workshopWorkers.filter(x=>x.id!==w.id);
      workshopAttendance = workshopAttendance.filter(a=>a.workerId!==w.id);
      workshopWagePayments = workshopWagePayments.filter(p=>p.workerId!==w.id);
      await saveWorkshopWorkers();
      await saveWorkshopAttendance();
      await saveWorkshopWagePayments();
      renderWorkers();
    });

    row.appendChild(num);
    row.appendChild(nameInput);
    row.appendChild(wageInput);
    row.appendChild(normalInInput);
    row.appendChild(normalOutInput);
    row.appendChild(attBtn);
    row.appendChild(removeBtn);
    box.appendChild(row);
  });
}
document.getElementById('workshop-worker-add-btn').addEventListener('click', async ()=>{
  workshopWorkers.push({id:'wk_'+Date.now(), name:'', wagePerDay:'', normalInTime:'', normalOutTime:'', createdAt:Date.now()});
  await saveWorkshopWorkers();
  renderWorkers();
  const inputs = document.querySelectorAll('#workshop-workers-list input');
  if(inputs.length) inputs[inputs.length-4].focus();
});

/* ---- Weekly Wage Summary ---- */
function openingBalanceKey(workerId, startISO){ return workerId+'::'+startISO; }
function addDaysISO(startISO, days){
  const d = new Date(startISO);
  d.setDate(d.getDate()+days);
  return d.toISOString().slice(0,10);
}
// A week's Opening Balance is whatever the user manually set for that week; if they never
// set one, it automatically carries forward as the previous week's closing Balance Due —
// all the way back to the week the worker was added (before that, there's no history, so 0).
function computeWeekOpeningBalance(workerId, startISO){
  const obKey = openingBalanceKey(workerId, startISO);
  if(workshopOpeningBalances[obKey]!==undefined){
    return parseFloat(workshopOpeningBalances[obKey])||0;
  }
  const worker = workshopWorkers.find(w=>w.id===workerId);
  if(worker && worker.createdAt){
    const createdWeekStart = getMondayISO(new Date(worker.createdAt));
    if(new Date(startISO) <= new Date(createdWeekStart)) return 0;
  }
  const prevStartISO = addDaysISO(startISO, -7);
  const prevOpening = computeWeekOpeningBalance(workerId, prevStartISO);
  const prevStats = workerWeekStats(workerId, prevStartISO);
  const prevPaid = workerWeekPaid(workerId, prevStartISO);
  return Math.round((prevOpening+prevStats.wage-prevPaid)*100)/100;
}
function renderWeeklySummary(){
  const startInput = document.getElementById('workshop-week-start');
  if(!startInput.value) startInput.value = getMondayISO(new Date());
  const startISO = startInput.value;
  const body = document.getElementById('workshop-weekly-body');
  body.innerHTML = '';
  if(workshopWorkers.length===0){
    body.innerHTML = `<tr class="empty-row"><td colspan="8">No workers added yet.</td></tr>`;
    return;
  }
  workshopWorkers.forEach(w=>{
    const stats = workerWeekStats(w.id, startISO);
    const paidWeek = workerWeekPaid(w.id, startISO);
    const obKey = openingBalanceKey(w.id, startISO);
    const isManual = workshopOpeningBalances[obKey]!==undefined;
    const openingBalance = computeWeekOpeningBalance(w.id, startISO);
    const due = Math.round((openingBalance+stats.wage-paidWeek)*100)/100;
    const tr = document.createElement('tr');
    tr.innerHTML = `
      <td>${escapeHtml(w.name||'(Unnamed)')}</td>
      <td></td>
      <td class="mono">${stats.daysPresent}</td>
      <td class="mono">${stats.otHours}</td>
      <td class="mono">${fmt(stats.wage)}</td>
      <td class="mono">${fmt(paidWeek)}</td>
      <td class="mono" style="${due>0?'color:var(--red);font-weight:700;':(due<0?'color:var(--green);font-weight:700;':'')}">${fmt(due)}</td>
      <td></td>
    `;
    const obInput = document.createElement('input');
    obInput.type = 'number';
    obInput.step = '0.01';
    obInput.value = openingBalance!==0 || isManual ? openingBalance : '';
    obInput.placeholder = '0.00';
    obInput.style.cssText = 'width:90px;padding:6px 8px;border-radius:6px;font-family:\'IBM Plex Mono\',monospace;font-size:13px;'
      + (isManual ? 'border:1px solid var(--brass);' : 'border:1px dashed var(--line);color:var(--ink-soft);');
    obInput.title = isManual
      ? "Manually set for this week. Clear the field to go back to auto-carrying last week's balance due."
      : "Auto-carried from last week's Balance Due. Type a value to set a manual opening balance just for this week.";
    obInput.addEventListener('input', async ()=>{
      const val = obInput.value;
      if(val===''){ delete workshopOpeningBalances[obKey]; } else { workshopOpeningBalances[obKey] = parseFloat(val)||0; }
      await saveWorkshopOpeningBalances();
      renderWeeklySummary();
    });
    tr.children[1].appendChild(obInput);

    const payBtn = document.createElement('button');
    payBtn.className = 'btn ghost';
    payBtn.textContent = 'Record Payment';
    payBtn.addEventListener('click', ()=> openWorkerPaymentModal(w.id));
    tr.lastElementChild.appendChild(payBtn);
    body.appendChild(tr);
  });
}
document.getElementById('workshop-week-start').addEventListener('change', renderWeeklySummary);

function renderWorkers(){
  renderWorkerDirectory();
  renderWeeklySummary();
}

/* ---- Attendance modal ---- */
let currentAttendanceWorkerId = null;
let editingAttendanceEntryId = null;
function exitAttendanceEditMode(){
  editingAttendanceEntryId = null;
  document.getElementById('wa-add-entry-btn').textContent = '+ Add Entry';
  document.getElementById('wa-cancel-edit-btn').style.display = 'none';
  document.getElementById('wa-date').value = todayISO();
  const worker = workshopWorkers.find(w=>w.id===currentAttendanceWorkerId);
  // Default In Time to AM and Out Time to PM — using the worker's own Normal In/Out
  // shift when set, otherwise a sensible 9 AM–5 PM fallback.
  document.getElementById('wa-in-time').value = (worker && worker.normalInTime) ? worker.normalInTime : '09:00';
  document.getElementById('wa-out-time').value = (worker && worker.normalOutTime) ? worker.normalOutTime : '17:00';
}
function openAttendanceModal(workerId){
  currentAttendanceWorkerId = workerId;
  const worker = workshopWorkers.find(w=>w.id===workerId);
  document.getElementById('wa-title').textContent = 'Attendance — ' + (worker && worker.name ? worker.name : 'Unnamed Worker');
  exitAttendanceEditMode();
  renderAttendanceList();
  document.getElementById('workshop-attendance-modal-overlay').classList.add('show');
}
function renderAttendanceList(){
  const box = document.getElementById('workshop-attendance-list');
  box.innerHTML = '';
  const worker = workshopWorkers.find(w=>w.id===currentAttendanceWorkerId);
  const entries = workshopAttendance.filter(a=>a.workerId===currentAttendanceWorkerId)
    .sort((a,b)=> b.date.localeCompare(a.date) || b.createdAt-a.createdAt);
  if(entries.length===0){
    box.innerHTML = `<div class="workshop-items-empty">No attendance entries yet.</div>`;
    return;
  }
  entries.forEach(entry=>{
    const row = document.createElement('div');
    row.className = 'workshop-attendance-row';
    const stats = computeAttendanceStats(worker, entry);
    row.innerHTML = `
      <div>${new Date(entry.date).toLocaleDateString('en-IN',{day:'numeric',month:'short'})}</div>
      <div>${entry.inTime||'—'}</div>
      <div>${entry.outTime||'—'}</div>
      <div class="mono">${stats.hoursWorked}</div>
      <div class="mono">${stats.otHours}</div>
      <div class="mono">${fmt(stats.wage)}</div>
      <div></div>
      <div></div>
    `;
    const cells = row.children;
    const editBtn = document.createElement('button');
    editBtn.className = 'btn ghost';
    editBtn.textContent = 'Edit';
    editBtn.title = 'Edit this entry';
    editBtn.addEventListener('click', ()=>{
      editingAttendanceEntryId = entry.id;
      document.getElementById('wa-date').value = entry.date;
      document.getElementById('wa-in-time').value = entry.inTime || '';
      document.getElementById('wa-out-time').value = entry.outTime || '';
      document.getElementById('wa-add-entry-btn').textContent = 'Save Changes';
      document.getElementById('wa-cancel-edit-btn').style.display = 'inline-flex';
      document.getElementById('wa-date').focus();
    });
    cells[6].appendChild(editBtn);

    const removeBtn = document.createElement('button');
    removeBtn.className = 'btn ghost';
    removeBtn.textContent = '×';
    removeBtn.title = 'Remove this entry';
    removeBtn.addEventListener('click', async ()=>{
      workshopAttendance = workshopAttendance.filter(a=>a.id!==entry.id);
      await saveWorkshopAttendance();
      if(editingAttendanceEntryId===entry.id) exitAttendanceEditMode();
      renderAttendanceList();
    });
    cells[7].appendChild(removeBtn);

    box.appendChild(row);
  });
}
document.getElementById('wa-add-entry-btn').addEventListener('click', async ()=>{
  const date = document.getElementById('wa-date').value || todayISO();
  const inTime = document.getElementById('wa-in-time').value;
  const outTime = document.getElementById('wa-out-time').value;
  if(!inTime || !outTime){ showToast('Enter both In Time and Out Time'); return; }

  if(editingAttendanceEntryId){
    const entry = workshopAttendance.find(a=>a.id===editingAttendanceEntryId);
    if(entry){
      entry.date = date; entry.inTime = inTime; entry.outTime = outTime;
      await saveWorkshopAttendance();
      showToast('Attendance entry updated');
    }
    exitAttendanceEditMode();
    renderAttendanceList();
    return;
  }

  const existing = workshopAttendance.find(a=>a.workerId===currentAttendanceWorkerId && a.date===date);
  if(existing){
    existing.inTime = inTime; existing.outTime = outTime;
  } else {
    workshopAttendance.push({id:'att_'+Date.now(), workerId: currentAttendanceWorkerId, date, inTime, outTime, createdAt: Date.now()});
  }
  await saveWorkshopAttendance();
  renderAttendanceList();
  showToast(existing ? 'Attendance entry updated' : 'Attendance entry added');
  const worker = workshopWorkers.find(w=>w.id===currentAttendanceWorkerId);
  document.getElementById('wa-in-time').value = (worker && worker.normalInTime) ? worker.normalInTime : '09:00';
  document.getElementById('wa-out-time').value = (worker && worker.normalOutTime) ? worker.normalOutTime : '17:00';
});
document.getElementById('wa-cancel-edit-btn').addEventListener('click', ()=>{
  exitAttendanceEditMode();
});
document.getElementById('wa-back-btn').addEventListener('click', ()=>{
  exitAttendanceEditMode();
  document.getElementById('workshop-attendance-modal-overlay').classList.remove('show');
});
document.getElementById('wa-close-btn').addEventListener('click', ()=>{
  exitAttendanceEditMode();
  document.getElementById('workshop-attendance-modal-overlay').classList.remove('show');
  renderWorkers();
});

/* ---- Worker payment modal ---- */
let currentPaymentWorkerId = null;
function setWpType(type){
  document.querySelector(`input[name="wp-type"][value="${type}"]`).checked = true;
  document.getElementById('wp-type-wage-opt').classList.toggle('selected', type==='wage');
  document.getElementById('wp-type-advance-opt').classList.toggle('selected', type==='advance');
  document.getElementById('wp-type-extra-opt').classList.toggle('selected', type==='extra');
}
document.getElementById('wp-type-wage-opt').addEventListener('click', ()=>setWpType('wage'));
document.getElementById('wp-type-advance-opt').addEventListener('click', ()=>setWpType('advance'));
document.getElementById('wp-type-extra-opt').addEventListener('click', ()=>setWpType('extra'));
function openWorkerPaymentModal(workerId){
  currentPaymentWorkerId = workerId;
  const worker = workshopWorkers.find(w=>w.id===workerId);
  const startISO = document.getElementById('workshop-week-start').value || getMondayISO(new Date());
  const stats = workerWeekStats(workerId, startISO);
  const paidWeek = workerWeekPaid(workerId, startISO);
  const openingBalance = computeWeekOpeningBalance(workerId, startISO);
  const due = Math.round((openingBalance+stats.wage-paidWeek)*100)/100;
  document.getElementById('wp-title').textContent = 'Record Payment — ' + (worker && worker.name ? worker.name : 'Unnamed Worker');
  const obPart = openingBalance!==0 ? `Opening Balance: ${fmt(openingBalance)}  |  ` : '';
  document.getElementById('wp-sub').textContent = `${obPart}This Week's Wage: ${fmt(stats.wage)}  |  Paid This Week: ${fmt(paidWeek)}  |  Due: ${fmt(due)}`;
  document.getElementById('wp-date').value = todayISO();
  document.getElementById('wp-amount').value = due>0 ? due : '';
  document.getElementById('wp-note').value = '';
  document.getElementById('wp-error').textContent = '';
  setWpType('wage');
  document.getElementById('workshop-payment-modal-overlay').classList.add('show');
}
document.getElementById('wp-cancel-btn').addEventListener('click', ()=>{
  document.getElementById('workshop-payment-modal-overlay').classList.remove('show');
});
document.getElementById('wp-save-btn').addEventListener('click', async ()=>{
  const errEl = document.getElementById('wp-error');
  const date = document.getElementById('wp-date').value || todayISO();
  const amount = parseFloat(document.getElementById('wp-amount').value);
  const note = document.getElementById('wp-note').value.trim();
  const type = document.querySelector('input[name="wp-type"]:checked').value;
  if(!amount || amount<=0){ errEl.textContent = 'Enter a valid amount.'; return; }
  workshopWagePayments.push({id:'wpay_'+Date.now(), workerId: currentPaymentWorkerId, date, amount, type, note, createdAt: Date.now()});
  await saveWorkshopWagePayments();
  document.getElementById('workshop-payment-modal-overlay').classList.remove('show');
  const typeLabel = type==='extra' ? 'Extra payment' : (type==='advance' ? 'Advance payment' : 'Wage payment');
  showToast(typeLabel+' recorded');
  renderWeeklySummary();
});

/* ---- Raw Materials modal (per manufactured item) ---- */
let currentWorkshopMaterialItem = null;
function openMaterialsModal(item){
  currentWorkshopMaterialItem = item;
  if(!Array.isArray(item.materials)) item.materials = [];
  document.getElementById('wm-title').textContent = 'Raw Materials — ' + (item.name || 'Unnamed Item');
  renderMaterialsList();
  document.getElementById('workshop-materials-modal-overlay').classList.add('show');
}
function computePieceText(mat){
  const stock = parseFloat(mat.qty);
  const perPiece = parseFloat(mat.perPiece);
  if(!isFinite(stock) || !isFinite(perPiece) || perPiece===0) return '—';
  return (Math.round((stock/perPiece)*100)/100).toString();
}
function renderMaterialsList(){
  const box = document.getElementById('workshop-materials-list');
  box.innerHTML = '';
  const materials = currentWorkshopMaterialItem.materials;
  if(materials.length===0){
    box.innerHTML = `<div class="workshop-items-empty">No raw materials added yet.</div>`;
    return;
  }
  materials.forEach(mat=>{
    const row = document.createElement('div');
    row.className = 'workshop-material-row';

    const nameInput = document.createElement('input');
    nameInput.type = 'text';
    nameInput.placeholder = 'Material name';
    nameInput.value = mat.name || '';
    nameInput.addEventListener('input', ()=>{ mat.name = nameInput.value; saveWorkshopRawItems(); });

    const pieceValue = document.createElement('div');
    pieceValue.className = 'workshop-piece-value';
    pieceValue.textContent = computePieceText(mat);

    const qtyInput = document.createElement('input');
    qtyInput.type = 'number';
    qtyInput.min = '0';
    qtyInput.step = '0.01';
    qtyInput.placeholder = 'Stock';
    qtyInput.value = (mat.qty===undefined || mat.qty===null || mat.qty==='') ? '' : mat.qty;
    qtyInput.addEventListener('input', ()=>{
      mat.qty = qtyInput.value===''?'':parseFloat(qtyInput.value)||0;
      saveWorkshopRawItems();
      pieceValue.textContent = computePieceText(mat);
    });

    const restockWrap = document.createElement('div');
    restockWrap.className = 'workshop-restock-wrap';
    const restockInput = document.createElement('input');
    restockInput.type = 'number';
    restockInput.min = '0';
    restockInput.step = '0.01';
    restockInput.className = 'workshop-restock-input';
    restockInput.placeholder = 'qty';
    restockInput.title = 'Enter the quantity of new material that arrived, then click Add to restock';
    const restockBtn = document.createElement('button');
    restockBtn.type = 'button';
    restockBtn.className = 'btn workshop-restock-btn';
    restockBtn.textContent = 'Add';
    function commitRestock(){
      const amount = parseFloat(restockInput.value);
      if(!isFinite(amount) || amount<=0) return;
      const newStock = Math.round(((parseFloat(mat.qty)||0)+amount)*100)/100;
      mat.qty = newStock;
      qtyInput.value = mat.qty;
      pieceValue.textContent = computePieceText(mat);
      restockInput.value = '';
      saveWorkshopRawItems();
      showToast(`Added ${amount} ${mat.unit||''} of ${mat.name||'this material'} — new stock: ${mat.qty}`);
    }
    restockBtn.addEventListener('click', commitRestock);
    restockInput.addEventListener('keydown', (e)=>{ if(e.key==='Enter') commitRestock(); });
    restockWrap.appendChild(restockInput);
    restockWrap.appendChild(restockBtn);

    const unitInput = document.createElement('input');
    unitInput.type = 'text';
    unitInput.placeholder = 'kg / pcs';
    unitInput.value = mat.unit || '';
    unitInput.addEventListener('input', ()=>{ mat.unit = unitInput.value; saveWorkshopRawItems(); });

    const perPieceInput = document.createElement('input');
    perPieceInput.type = 'number';
    perPieceInput.min = '0';
    perPieceInput.step = '0.01';
    perPieceInput.placeholder = 'e.g. 1.25';
    perPieceInput.title = 'Quantity of this material needed to manufacture one piece';
    perPieceInput.value = (mat.perPiece===undefined || mat.perPiece===null || mat.perPiece==='') ? '' : mat.perPiece;
    perPieceInput.addEventListener('input', ()=>{
      mat.perPiece = perPieceInput.value===''?'':parseFloat(perPieceInput.value)||0;
      saveWorkshopRawItems();
      pieceValue.textContent = computePieceText(mat);
    });

    const removeBtn = document.createElement('button');
    removeBtn.className = 'btn ghost';
    removeBtn.textContent = '×';
    removeBtn.title = 'Remove this material';
    removeBtn.addEventListener('click', ()=>{
      const i = materials.indexOf(mat);
      if(i>-1) materials.splice(i,1);
      renderMaterialsList();
      saveWorkshopRawItems();
    });

    row.appendChild(nameInput);
    row.appendChild(qtyInput);
    row.appendChild(restockWrap);
    row.appendChild(unitInput);
    row.appendChild(perPieceInput);
    row.appendChild(pieceValue);
    row.appendChild(removeBtn);
    box.appendChild(row);
  });
}
document.getElementById('wm-add-material-btn').addEventListener('click', ()=>{
  currentWorkshopMaterialItem.materials.push({name:'', qty:'', unit:'', perPiece:''});
  renderMaterialsList();
  saveWorkshopRawItems();
  const rows = document.querySelectorAll('#workshop-materials-list .workshop-material-row');
  if(rows.length) rows[rows.length-1].querySelector('input').focus();
});
document.getElementById('wm-back-btn').addEventListener('click', ()=>{
  document.getElementById('workshop-materials-modal-overlay').classList.remove('show');
});
document.getElementById('wm-close-btn').addEventListener('click', ()=>{
  document.getElementById('workshop-materials-modal-overlay').classList.remove('show');
  renderWorkshop();
});

/* ================= DASHBOARD ================= */
function renderDashboard(){
  document.getElementById('dash-date').textContent = new Date().toLocaleDateString('en-IN', {weekday:'long', year:'numeric', month:'long', day:'numeric'});
  const today = todayISO();
  const monthKey = today.slice(0,7);

  let todaySales=0, monthSales=0, monthGst=0, monthCount=0;
  invoices.forEach(inv=>{
    if(inv.date===today) todaySales += inv.grandTotal;
    if(inv.date.slice(0,7)===monthKey){
      monthSales += inv.grandTotal;
      monthGst += (inv.totalCgst+inv.totalSgst+inv.totalIgst);
      monthCount++;
    }
  });
  document.getElementById('stat-today').textContent = fmt(todaySales);
  document.getElementById('stat-month').textContent = fmt(monthSales);
  document.getElementById('stat-gst').textContent = fmt(monthGst);
  document.getElementById('stat-count').textContent = monthCount;

  const recent = [...invoices].sort((a,b)=> b.createdAt - a.createdAt).slice(0,6);
  const body = document.getElementById('recent-body');
  body.innerHTML = '';
  if(recent.length===0){
    body.innerHTML = `<tr class="empty-row"><td colspan="7">No invoices yet — create your first one.</td></tr>`;
  }else{
    recent.forEach(inv=> body.appendChild(invoiceRow(inv)));
  }
}

/* ================= INVOICE ITEMS BUILDER ================= */
function addItemRow(prefill){
  itemRowId++;
  const id = 'row'+itemRowId;
  const wrap = document.getElementById('items-wrap');
  const row = document.createElement('div');
  row.className = 'item-row';
  row.id = id;

  const nameOptions = products.map(p=>`<option value="${p.id}">${escapeHtml(p.name)}</option>`).join('');

  row.innerHTML = `
    <input class="item-name" list="dl-${id}" placeholder="Item name" value="${prefill?escapeHtml(prefill.name):''}">
    <datalist id="dl-${id}">${products.map(p=>`<option value="${escapeHtml(p.name)}">`).join('')}</datalist>
    <input class="item-qty" type="number" min="1" value="${prefill?prefill.qty:1}">
    <input class="item-price" type="number" min="0" step="0.01" value="${prefill?prefill.price:''}" placeholder="0.00">
    <select class="item-price-mode" title="Does the price above already include GST?">
      <option value="excl" selected>Excl. GST</option>
      <option value="incl">Incl. GST</option>
    </select>
    <select class="item-gst">
      <option value="0">0%</option><option value="5">5%</option>
      <option value="18">18%</option><option value="28">28%</option>
    </select>
    <button class="rm">&times;</button>
  `;
  wrap.appendChild(row);
  if(prefill) row.querySelector('.item-gst').value = prefill.gst;
  if(prefill && prefill.priceMode) row.querySelector('.item-price-mode').value = prefill.priceMode;

  row.querySelector('.item-name').addEventListener('input', (e)=>{
    const match = products.find(p=>p.name.toLowerCase()===e.target.value.toLowerCase());
    if(match && match.id !== row.dataset.lastMatchId){
      row.querySelector('.item-price').value = match.price;
      row.querySelector('.item-gst').value = match.gstRate;
      row.querySelector('.item-price-mode').value = 'excl';
      row.querySelector('.item-price').classList.remove('edited');
      row.dataset.lastMatchId = match.id;
      recalc();
    } else if(!match){
      row.dataset.lastMatchId = '';
    }
  });
  row.querySelector('.item-price').addEventListener('input', ()=>{
    row.querySelector('.item-price').classList.add('edited');
  });
  row.querySelectorAll('input,select').forEach(el=> el.addEventListener('input', recalc));
  row.querySelector('.rm').addEventListener('click', ()=>{ row.remove(); recalc(); });
}
function escapeHtml(s){ return (s||'').replace(/[&<>"']/g, c=>({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;'}[c])); }

document.getElementById('add-item-btn').addEventListener('click', ()=>addItemRow());

populateStateSelect(document.getElementById('cust-state'), true);

function enforceDigitsOnly(inputEl, maxLen){
  inputEl.addEventListener('input', ()=>{
    const digitsOnly = inputEl.value.replace(/\D/g, '').slice(0, maxLen);
    if(inputEl.value !== digitsOnly) inputEl.value = digitsOnly;
  });
  inputEl.addEventListener('keydown', (e)=>{
    const allowedKeys = ['Backspace','Delete','Tab','Escape','Enter','ArrowLeft','ArrowRight','ArrowUp','ArrowDown','Home','End'];
    if(allowedKeys.includes(e.key) || e.ctrlKey || e.metaKey) return;
    if(!/^[0-9]$/.test(e.key)) e.preventDefault();
  });
  inputEl.addEventListener('paste', (e)=>{
    e.preventDefault();
    const pasted = (e.clipboardData||window.clipboardData).getData('text').replace(/\D/g, '');
    const start = inputEl.selectionStart, end = inputEl.selectionEnd;
    const newValue = (inputEl.value.slice(0,start) + pasted + inputEl.value.slice(end)).replace(/\D/g, '').slice(0, maxLen);
    inputEl.value = newValue;
    inputEl.dispatchEvent(new Event('input', {bubbles:true}));
  });
}
enforceDigitsOnly(document.getElementById('cust-pin'), 6);
enforceDigitsOnly(document.getElementById('biz-pin'), 6);
document.getElementById('cust-state').addEventListener('change', autoSetTaxTypeFromState);
function autoSetTaxTypeFromState(){
  const custState = document.getElementById('cust-state').value;
  const bizState = profile.state || '';
  if(!custState){
    document.getElementById('taxtype-hint').textContent = "Auto-set from customer's State — change it above if the customer state differs from your business.";
    return;
  }
  if(!bizState){
    document.getElementById('taxtype-hint').textContent = "Set your business State in Business Profile to enable auto tax-type detection.";
    return;
  }
  const isInter = normalizeState(custState) !== normalizeState(bizState);
  const targetValue = isInter ? 'inter' : 'intra';
  document.querySelector(`input[name="taxtype"][value="${targetValue}"]`).checked = true;
  document.getElementById('opt-intra').classList.toggle('selected', !isInter);
  document.getElementById('opt-inter').classList.toggle('selected', isInter);
  document.getElementById('taxtype-hint').textContent = isInter
    ? `Customer is in ${custState}, different from your state (${bizState}) — IGST applied automatically.`
    : `Customer is in ${custState}, same as your state — CGST + SGST applied automatically.`;
  recalc();
}

document.querySelectorAll('input[name=taxtype]').forEach(r=>{
  r.addEventListener('change', ()=>{
    document.getElementById('opt-intra').classList.toggle('selected', r.value==='intra' && r.checked);
    document.getElementById('opt-inter').classList.toggle('selected', r.value==='inter' && r.checked);
    recalc();
  });
});
document.getElementById('opt-intra').addEventListener('click', ()=>{ document.querySelector('input[value=intra]').checked=true; document.getElementById('opt-intra').classList.add('selected'); document.getElementById('opt-inter').classList.remove('selected'); recalc(); });
document.getElementById('opt-inter').addEventListener('click', ()=>{ document.querySelector('input[value=inter]').checked=true; document.getElementById('opt-inter').classList.add('selected'); document.getElementById('opt-intra').classList.remove('selected'); recalc(); });

function getItemsFromForm(){
  const rows = document.querySelectorAll('#items-wrap .item-row');
  const items = [];
  rows.forEach(row=>{
    const name = row.querySelector('.item-name').value.trim();
    const qty = parseFloat(row.querySelector('.item-qty').value)||0;
    const price = parseFloat(row.querySelector('.item-price').value)||0;
    const gst = parseFloat(row.querySelector('.item-gst').value)||0;
    const priceMode = row.querySelector('.item-price-mode').value; // 'excl' or 'incl'
    if(name && qty>0 && price>=0){
      // 'excl': price is the taxable (pre-GST) unit price — unchanged from previous behaviour.
      // 'incl': price already includes GST, so back-calculate the taxable unit price first.
      const taxable = priceMode==='incl' ? qty * (price / (1 + gst/100)) : qty*price;
      items.push({name, qty, price, gst, priceMode, taxable});
    }
  });
  return items;
}

function recalc(){
  const items = getItemsFromForm();
  const isInter = document.querySelector('input[name=taxtype]:checked').value === 'inter';
  let taxable=0, cgst=0, sgst=0, igst=0;
  items.forEach(it=>{
    taxable += it.taxable;
    const tax = it.taxable * (it.gst/100);
    if(isInter) igst += tax; else { cgst += tax/2; sgst += tax/2; }
  });
  document.getElementById('calc-taxable').textContent = fmt(taxable);
  document.getElementById('calc-cgst').textContent = fmt(cgst);
  document.getElementById('calc-sgst').textContent = fmt(sgst);
  document.getElementById('calc-igst').textContent = fmt(igst);
  document.getElementById('calc-grand').textContent = fmt(taxable+cgst+sgst+igst);
  document.getElementById('row-cgst').style.display = isInter?'none':'table-row';
  document.getElementById('row-sgst').style.display = isInter?'none':'table-row';
  document.getElementById('row-igst').style.display = isInter?'table-row':'none';
  return {items, isInter, taxable, cgst, sgst, igst, grand: taxable+cgst+sgst+igst};
}

document.getElementById('save-invoice-btn').addEventListener('click', async ()=>{
  const custName = document.getElementById('cust-name').value.trim();
  const {items, isInter, taxable, cgst, sgst, igst, grand} = recalc();
  if(!custName){ showToast('Enter a customer name'); return; }
  if(items.length===0){ showToast('Add at least one item'); return; }

  const inv = {
    id: 'i'+Date.now(),
    invoiceNo: nextInvoiceNo(),
    date: todayISO(),
    createdAt: Date.now(),
    customer: {
      name: custName,
      phone: document.getElementById('cust-phone').value.trim(),
      address: document.getElementById('cust-address').value.trim(),
      state: document.getElementById('cust-state').value,
      pin: document.getElementById('cust-pin').value.trim(),
      gstin: document.getElementById('cust-gstin').value.trim()
    },
    taxType: isInter ? 'inter' : 'intra',
    items, taxable,
    totalCgst: cgst, totalSgst: sgst, totalIgst: igst,
    grandTotal: grand,
    status: 'unpaid',
    paidAmount: 0
  };
  invoices.push(inv);
  await saveInvoices();
  showToast('Invoice '+inv.invoiceNo+' saved');
  pushToSheet({ type:'invoice', record: inv });
  clearInvoiceForm();
  openInvoiceModal(inv.id);
});

document.getElementById('clear-invoice-btn').addEventListener('click', clearInvoiceForm);
function clearInvoiceForm(){
  ['cust-name','cust-phone','cust-address','cust-pin','cust-gstin'].forEach(id=>document.getElementById(id).value='');
  document.getElementById('cust-state').value = '';
  document.getElementById('items-wrap').innerHTML='';
  document.querySelector('input[value=intra]').checked = true;
  document.getElementById('opt-intra').classList.add('selected');
  document.getElementById('opt-inter').classList.remove('selected');
  document.getElementById('taxtype-hint').textContent = "Auto-set from customer's State — change it above if the customer state differs from your business.";
  addItemRow();
  recalc();
}

/* ================= HISTORY ================= */
function paymentStatusInfo(inv){
  const grand = inv.grandTotal||0;
  // Legacy invoices (saved before payment tracking existed) have no paidAmount field —
  // infer it from their old binary status so an already-paid invoice doesn't show a false balance due.
  let paid = (inv.paidAmount===undefined || inv.paidAmount===null) ? (inv.status==='paid' ? grand : 0) : inv.paidAmount;
  const due = Math.max(0, Math.round((grand-paid)*100)/100);
  let status = inv.status;
  if(!status){ status = paid>=grand && grand>0 ? 'paid' : (paid>0 ? 'partial' : 'unpaid'); }
  const badgeClass = status==='paid' ? 'paid' : (status==='partial' ? 'partial' : 'due');
  const label = status==='paid' ? 'Paid' : (status==='partial' ? 'Partial' : 'Due');
  return { status, badgeClass, label, paid, due };
}
function invoiceRow(inv){
  const tr = document.createElement('tr');
  const pay = paymentStatusInfo(inv);
  tr.innerHTML = `
    <td class="mono">${inv.invoiceNo}</td>
    <td>${new Date(inv.date).toLocaleDateString('en-IN')}</td>
    <td>${escapeHtml(inv.customer.name)}</td>
    <td>${inv.taxType==='inter'?'IGST':'CGST+SGST'}</td>
    <td class="mono">${fmt(inv.grandTotal)}</td>
    <td><span class="badge ${pay.badgeClass}">${pay.label}</span></td>
    <td><button class="btn ghost view-btn" data-id="${inv.id}">View</button></td>
  `;
  tr.querySelector('.view-btn').addEventListener('click', ()=>openInvoiceModal(inv.id));
  return tr;
}
function renderHistory(){
  const body = document.getElementById('history-body');
  const q = (document.getElementById('history-search').value||'').toLowerCase();
  const list = [...invoices].sort((a,b)=>b.createdAt-a.createdAt)
    .filter(inv => !q || inv.customer.name.toLowerCase().includes(q) || inv.invoiceNo.toLowerCase().includes(q));
  body.innerHTML='';
  if(list.length===0){
    body.innerHTML = `<tr class="empty-row"><td colspan="7">No matching invoices.</td></tr>`;
  }else{
    list.forEach(inv=>body.appendChild(invoiceRow(inv)));
  }
}
document.getElementById('history-search').addEventListener('input', renderHistory);

/* ================= GST SUMMARY ================= */
function renderGst(){
  const map = {};
  invoices.forEach(inv=>{
    const key = inv.date.slice(0,7);
    if(!map[key]) map[key] = {count:0, taxable:0, cgst:0, sgst:0, igst:0};
    map[key].count++;
    map[key].taxable += inv.taxable;
    map[key].cgst += inv.totalCgst;
    map[key].sgst += inv.totalSgst;
    map[key].igst += inv.totalIgst;
  });
  const keys = Object.keys(map).sort().reverse();
  const body = document.getElementById('gst-body');
  body.innerHTML = '';
  if(keys.length===0){
    body.innerHTML = `<tr class="empty-row"><td colspan="7">No invoices yet.</td></tr>`;
    return;
  }
  keys.forEach(k=>{
    const m = map[k];
    const label = new Date(k+'-01').toLocaleDateString('en-IN', {year:'numeric', month:'long'});
    const total = m.cgst+m.sgst+m.igst;
    const tr = document.createElement('tr');
    tr.innerHTML = `<td>${label}</td><td>${m.count}</td><td class="mono">${fmt(m.taxable)}</td>
      <td class="mono">${fmt(m.cgst)}</td><td class="mono">${fmt(m.sgst)}</td><td class="mono">${fmt(m.igst)}</td>
      <td class="mono"><strong>${fmt(total)}</strong></td>`;
    body.appendChild(tr);
  });
}

/* ================= PRODUCTS ================= */
/* ================= CONFIRM DIALOG ================= */
let confirmResolver = null;
function confirmAction(message, title){
  document.getElementById('confirm-title').textContent = title || 'Are you sure?';
  document.getElementById('confirm-message').textContent = message;
  document.getElementById('confirm-modal-overlay').classList.add('show');
  return new Promise(resolve=>{ confirmResolver = resolve; });
}
document.getElementById('confirm-yes').addEventListener('click', ()=>{
  document.getElementById('confirm-modal-overlay').classList.remove('show');
  if(confirmResolver) confirmResolver(true);
  confirmResolver = null;
});
document.getElementById('confirm-no').addEventListener('click', ()=>{
  document.getElementById('confirm-modal-overlay').classList.remove('show');
  if(confirmResolver) confirmResolver(false);
  confirmResolver = null;
});

let editingProductId = null;

function renderProducts(){
  const body = document.getElementById('products-body');
  body.innerHTML='';
  if(products.length===0){
    body.innerHTML = `<tr class="empty-row"><td colspan="4">No saved products yet.</td></tr>`;
    return;
  }
  products.forEach(p=>{
    const tr = document.createElement('tr');
    tr.innerHTML = `<td>${escapeHtml(p.name)}</td><td class="mono">${fmt(p.price)}</td><td>${p.gstRate}%</td>
      <td>
        <button class="btn gold edit-prod" data-id="${p.id}">Edit</button>
        <button class="btn red del-prod" data-id="${p.id}">Remove</button>
      </td>`;
    tr.querySelector('.edit-prod').addEventListener('click', ()=>{
      editingProductId = p.id;
      document.getElementById('pm-title').textContent = 'Edit Product';
      document.getElementById('pm-name').value = p.name;
      document.getElementById('pm-price').value = p.price;
      document.getElementById('pm-gst').value = p.gstRate;
      document.getElementById('product-modal-overlay').classList.add('show');
    });
    tr.querySelector('.del-prod').addEventListener('click', async ()=>{
      const ok = await confirmAction(`Remove "${p.name}" from your products? This cannot be undone.`, 'Remove Product');
      if(!ok) return;
      products = products.filter(x=>x.id!==p.id);
      await saveProducts();
      renderProducts();
    });
    body.appendChild(tr);
  });
}
document.getElementById('add-product-btn').addEventListener('click', ()=>{
  editingProductId = null;
  document.getElementById('pm-title').textContent = 'Add Product';
  document.getElementById('pm-name').value='';
  document.getElementById('pm-price').value='';
  document.getElementById('pm-gst').value='18';
  document.getElementById('product-modal-overlay').classList.add('show');
});
document.getElementById('pm-cancel').addEventListener('click', ()=>document.getElementById('product-modal-overlay').classList.remove('show'));
document.getElementById('pm-save').addEventListener('click', async ()=>{
  const name = document.getElementById('pm-name').value.trim();
  const price = parseFloat(document.getElementById('pm-price').value)||0;
  const gstRate = parseFloat(document.getElementById('pm-gst').value)||0;
  if(!name){ showToast('Enter a product name'); return; }
  if(editingProductId){
    const idx = products.findIndex(x=>x.id===editingProductId);
    if(idx>-1) products[idx] = { ...products[idx], name, price, gstRate };
  }else{
    products.push({id:'p'+Date.now(), name, price, gstRate});
  }
  await saveProducts();
  document.getElementById('product-modal-overlay').classList.remove('show');
  renderProducts();
  showToast(editingProductId ? 'Product updated' : 'Product added');
  editingProductId = null;
});

/* ================= ACCOUNTS ================= */
/* ================= ACCOUNT BALANCE HELPERS ================= */
function computeAccountBalance(accountId){
  const acc = paymentAccounts.find(a=>a.id===accountId);
  if(!acc) return 0;
  let bal = acc.openingBalance||0;
  transactions.forEach(t=>{
    if(t.type==='in' && t.accountId===accountId) bal += t.amount;
    else if(t.type==='out' && t.accountId===accountId) bal -= t.amount;
    else if(t.type==='transfer'){
      if(t.accountId===accountId) bal -= t.amount;
      if(t.toAccountId===accountId) bal += t.amount;
    }
  });
  return bal;
}
// Returns a map of txnId -> {before, after} balance for one account, replayed in
// chronological order (same ordering used everywhere else in the app).
function computeAccountBalanceHistory(accountId){
  const acc = paymentAccounts.find(a=>a.id===accountId);
  const history = {};
  if(!acc) return history;
  const related = transactions.filter(t=> t.accountId===accountId || t.toAccountId===accountId);
  const sorted = [...related].sort((a,b)=> a.date===b.date ? a.createdAt-b.createdAt : a.date.localeCompare(b.date));
  let running = acc.openingBalance||0;
  sorted.forEach(t=>{
    const before = running;
    if(t.type==='in' && t.accountId===accountId) running += t.amount;
    else if(t.type==='out' && t.accountId===accountId) running -= t.amount;
    else if(t.type==='transfer'){
      if(t.accountId===accountId) running -= t.amount;
      if(t.toAccountId===accountId) running += t.amount;
    }
    history[t.id] = { before, after: running };
  });
  return history;
}
function accountLabel(acc){
  if(!acc) return 'Unknown Account';
  if(acc.kind==='bank'){
    const last4 = (acc.accountNumber||'').slice(-4);
    return `${acc.name}${last4 ? ' ••'+last4 : ''}`;
  }
  return acc.name;
}
function populateAccountSelects(){
  const opts = paymentAccounts.map(a=>`<option value="${a.id}">${escapeHtml(accountLabel(a))}</option>`).join('');
  document.getElementById('txn-account').innerHTML = opts;
  document.getElementById('txn-to-account').innerHTML = opts;
  const filterSel = document.getElementById('txn-filter-account');
  const prevFilter = filterSel.value || 'all';
  filterSel.innerHTML = `<option value="all">All Accounts</option>` + opts;
  filterSel.value = [...filterSel.options].some(o=>o.value===prevFilter) ? prevFilter : 'all';
}

function renderAccountsList(){
  const box = document.getElementById('accounts-list-body');
  box.innerHTML='';
  const term = (document.getElementById('account-search-input').value||'').trim().toLowerCase();
  const filteredAccounts = term ? paymentAccounts.filter(acc=>{
    return (acc.name||'').toLowerCase().includes(term)
      || (acc.bankName||'').toLowerCase().includes(term)
      || (acc.accountNumber||'').toLowerCase().includes(term)
      || (acc.ifsc||'').toLowerCase().includes(term);
  }) : paymentAccounts;

  if(filteredAccounts.length===0){
    box.innerHTML = `<div style="font-size:13px;color:var(--ink-soft);padding:10px 0;">No accounts match "${escapeHtml(term)}".</div>`;
    return;
  }

  filteredAccounts.forEach(acc=>{
    const bal = computeAccountBalance(acc.id);
    const row = document.createElement('div');
    row.style.cssText = 'display:flex;justify-content:space-between;align-items:center;padding:14px 16px;border:1px solid var(--line);border-radius:10px;flex-wrap:wrap;gap:10px;';
    row.innerHTML = `
      <div>
        <div style="font-weight:600;">${escapeHtml(acc.name)} ${acc.kind==='bank'?'<span class="badge due" style="margin-left:6px;">Bank</span>':'<span class="badge paid" style="margin-left:6px;">Cash</span>'}</div>
        <div style="font-size:12.5px;color:var(--ink-soft);margin-top:3px;">
          ${acc.kind==='bank' ? escapeHtml(acc.bankName||'') + (acc.accountNumber ? ' • A/c No. '+escapeHtml(acc.accountNumber) : '') + (acc.ifsc ? ' • IFSC '+escapeHtml(acc.ifsc) : '') : 'Physical cash on hand'}
        </div>
      </div>
      <div style="display:flex;align-items:center;gap:16px;">
        <div class="mono" style="font-weight:700;font-size:16px;color:${bal<0?'var(--red)':'var(--ink)'};">${fmt(bal)}</div>
        <button class="btn ghost view-acc" data-id="${acc.id}">View</button>
        <button class="btn ghost edit-acc" data-id="${acc.id}">Edit</button>
        ${paymentAccounts.length>1 ? `<button class="btn ghost del-acc" data-id="${acc.id}">Remove</button>` : ''}
      </div>`;
    row.querySelector('.view-acc').addEventListener('click', ()=>openViewAccountModal(acc.id));
    row.querySelector('.edit-acc').addEventListener('click', ()=>openAccountModal(acc.id));
    const delBtn = row.querySelector('.del-acc');
    if(delBtn){
      delBtn.addEventListener('click', async ()=>{
        const inUse = transactions.some(t=>t.accountId===acc.id || t.toAccountId===acc.id);
        if(inUse){ showToast('Cannot remove — this account has transactions. Reassign or delete them first.'); return; }
        const ok = await confirmAction(`Remove account "${acc.name}"? This cannot be undone.`, 'Remove Account');
        if(!ok) return;
        paymentAccounts = paymentAccounts.filter(a=>a.id!==acc.id);
        await saveAccountsList();
        renderAccounts();
      });
    }
    box.appendChild(row);
  });
}

function renderAccounts(){
  ensureDefaultAccount();
  populateAccountSelects();
  renderAccountsList();

  const body = document.getElementById('accounts-body');
  body.innerHTML='';
  let totalIn = 0, totalOut = 0;
  transactions.forEach(t=>{
    if(t.type==='in') totalIn += t.amount;
    else if(t.type==='out') totalOut += t.amount;
  });
  document.getElementById('acc-in').textContent = fmt(totalIn);
  document.getElementById('acc-out').textContent = fmt(totalOut);
  const totalOpening = paymentAccounts.reduce((s,a)=>s+(a.openingBalance||0),0);
  document.getElementById('acc-balance').textContent = fmt(totalOpening + totalIn - totalOut);

  const filterId = document.getElementById('txn-filter-account').value || 'all';
  const filtered = filterId==='all' ? transactions : transactions.filter(t=>t.accountId===filterId || t.toAccountId===filterId);

  if(filtered.length===0){
    body.innerHTML = `<tr class="empty-row"><td colspan="7">No transactions yet.</td></tr>`;
    return;
  }
  const sorted = [...filtered].sort((a,b)=> a.date===b.date ? a.createdAt-b.createdAt : a.date.localeCompare(b.date));
  const runningByAccount = {};
  paymentAccounts.forEach(a=>runningByAccount[a.id]=a.openingBalance||0);
  const rows = sorted.map(t=>{
    if(t.type==='in') runningByAccount[t.accountId] = (runningByAccount[t.accountId]||0) + t.amount;
    else if(t.type==='out') runningByAccount[t.accountId] = (runningByAccount[t.accountId]||0) - t.amount;
    else if(t.type==='transfer'){
      runningByAccount[t.accountId] = (runningByAccount[t.accountId]||0) - t.amount;
      runningByAccount[t.toAccountId] = (runningByAccount[t.toAccountId]||0) + t.amount;
    }
    const balanceAtPoint = filterId==='all' ? null : runningByAccount[filterId];
    return {t, balanceAtPoint};
  });
  rows.reverse().forEach(({t, balanceAtPoint})=>{
    const fromAcc = paymentAccounts.find(a=>a.id===t.accountId);
    const toAcc = paymentAccounts.find(a=>a.id===t.toAccountId);
    const tr = document.createElement('tr');
    let typeBadge, amountCell, accountCell;
    if(t.type==='transfer'){
      typeBadge = `<span class="badge due">Transfer</span>`;
      amountCell = `<span class="mono">${fmt(t.amount)}</span>`;
      accountCell = `${escapeHtml(accountLabel(fromAcc))} → ${escapeHtml(accountLabel(toAcc))}`;
    } else {
      typeBadge = `<span class="badge ${t.type==='in'?'paid':'due'}">${t.type==='in'?'In':'Out'}</span>`;
      amountCell = `<span class="mono">${t.type==='in'?'+':'-'}${fmt(t.amount)}</span>`;
      accountCell = escapeHtml(accountLabel(fromAcc));
    }
    tr.innerHTML = `<td>${new Date(t.date).toLocaleDateString('en-IN',{year:'numeric',month:'short',day:'numeric'})}</td>
      <td>${accountCell}</td>
      <td>${typeBadge}</td>
      <td>${escapeHtml(t.note||'—')}</td>
      <td class="mono">${amountCell}</td>
      <td class="mono">${balanceAtPoint===null ? '—' : fmt(balanceAtPoint)}</td>
      <td><button class="btn ghost view-txn" data-id="${t.id}">View</button> <button class="btn ghost del-txn" data-id="${t.id}">Remove</button></td>`;
    tr.querySelector('.view-txn').addEventListener('click', ()=>openViewTxnModal(t.id));
    tr.querySelector('.del-txn').addEventListener('click', async ()=>{
      const label = t.note ? `"${t.note}"` : (t.type==='transfer'?'this transfer':(t.type==='in'?'this Money In entry':'this Money Out entry'));
      const ok = await confirmAction(`Remove ${label} (${fmt(t.amount)})? This cannot be undone.`, 'Remove Transaction');
      if(!ok) return;
      transactions = transactions.filter(x=>x.id!==t.id);
      await saveTransactions();
      renderAccounts();
    });
    body.appendChild(tr);
  });
}
document.getElementById('txn-filter-account').addEventListener('change', renderAccounts);
document.getElementById('account-search-btn').addEventListener('click', renderAccountsList);
document.getElementById('account-search-input').addEventListener('input', renderAccountsList);
document.getElementById('account-search-input').addEventListener('keydown', (e)=>{
  if(e.key==='Enter') renderAccountsList();
});

function setTxnType(type){
  document.querySelector(`input[name="txn-type"][value="${type}"]`).checked = true;
  document.getElementById('txn-type-in-opt').classList.toggle('selected', type==='in');
  document.getElementById('txn-type-out-opt').classList.toggle('selected', type==='out');
  document.getElementById('txn-type-transfer-opt').classList.toggle('selected', type==='transfer');
  document.getElementById('txn-to-account-field').style.display = type==='transfer' ? 'block' : 'none';
  document.getElementById('txn-account-label').textContent = type==='transfer' ? 'From Account' : 'Account';
}
document.getElementById('txn-type-in-opt').addEventListener('click', ()=>setTxnType('in'));
document.getElementById('txn-type-out-opt').addEventListener('click', ()=>setTxnType('out'));
document.getElementById('txn-type-transfer-opt').addEventListener('click', ()=>setTxnType('transfer'));

document.getElementById('add-txn-btn').addEventListener('click', ()=>{
  if(paymentAccounts.length===0){ showToast('Add a payment account first'); return; }
  document.getElementById('txn-date').value = todayISO();
  document.getElementById('txn-amount').value = '';
  document.getElementById('txn-note').value = '';
  populateAccountSelects();
  setTxnType('in');
  document.getElementById('txn-modal-overlay').classList.add('show');
});
document.getElementById('txn-cancel').addEventListener('click', ()=>{
  document.getElementById('txn-modal-overlay').classList.remove('show');
});
document.getElementById('txn-save').addEventListener('click', async ()=>{
  const type = document.querySelector('input[name="txn-type"]:checked').value;
  const date = document.getElementById('txn-date').value || todayISO();
  const amount = parseFloat(document.getElementById('txn-amount').value)||0;
  const note = document.getElementById('txn-note').value.trim();
  const accountId = document.getElementById('txn-account').value;
  const toAccountId = document.getElementById('txn-to-account').value;
  if(amount<=0){ showToast('Enter a valid amount'); return; }
  if(type==='transfer' && accountId===toAccountId){ showToast('Choose two different accounts for a transfer'); return; }
  const txn = { id:'t'+Date.now(), type, date, amount, note, accountId, createdAt:Date.now() };
  if(type==='transfer') txn.toAccountId = toAccountId;
  transactions.push(txn);
  await saveTransactions();
  document.getElementById('txn-modal-overlay').classList.remove('show');
  renderAccounts();
  showToast(type==='transfer' ? 'Transfer recorded' : 'Transaction added');
  const acc = paymentAccounts.find(a=>a.id===accountId);
  const toAcc = paymentAccounts.find(a=>a.id===toAccountId);
  pushToSheet({ type:'transaction', record: Object.assign({}, txn, {
    accountName: acc ? acc.name : '', accountNumber: acc ? (acc.accountNumber||'') : '',
    toAccountName: toAcc ? toAcc.name : ''
  }) });
});

/* ================= PAYMENT ACCOUNTS (CASH & BANK) ================= */
function setAccountKind(kind){
  document.querySelector(`input[name="am-kind"][value="${kind}"]`).checked = true;
  document.getElementById('am-kind-cash-opt').classList.toggle('selected', kind==='cash');
  document.getElementById('am-kind-bank-opt').classList.toggle('selected', kind==='bank');
  document.getElementById('am-bank-fields').style.display = kind==='bank' ? 'block' : 'none';
}
document.getElementById('am-kind-cash-opt').addEventListener('click', ()=>setAccountKind('cash'));
document.getElementById('am-kind-bank-opt').addEventListener('click', ()=>setAccountKind('bank'));

function openViewAccountModal(accountId){
  const acc = paymentAccounts.find(a=>a.id===accountId);
  if(!acc) return;
  document.getElementById('va-title').textContent = acc.name;
  document.getElementById('va-type').textContent = acc.kind==='bank' ? 'Bank Account' : 'Cash';
  document.getElementById('va-name').textContent = acc.name;
  const isBank = acc.kind==='bank';
  document.getElementById('va-bank-row').style.display = isBank ? 'flex' : 'none';
  document.getElementById('va-number-row').style.display = isBank ? 'flex' : 'none';
  document.getElementById('va-ifsc-row').style.display = (isBank && acc.ifsc) ? 'flex' : 'none';
  document.getElementById('va-bank-name').textContent = acc.bankName||'—';
  document.getElementById('va-account-number').textContent = acc.accountNumber||'—';
  document.getElementById('va-ifsc').textContent = acc.ifsc||'—';
  document.getElementById('va-opening').textContent = fmt(acc.openingBalance||0);
  const bal = computeAccountBalance(acc.id);
  const balEl = document.getElementById('va-balance');
  balEl.textContent = fmt(bal);
  balEl.style.color = bal<0 ? 'var(--red)' : 'var(--ink)';

  const list = document.getElementById('va-txn-list');
  list.innerHTML = '';
  const related = transactions.filter(t=>t.accountId===acc.id || t.toAccountId===acc.id)
    .sort((a,b)=> b.date===a.date ? b.createdAt-a.createdAt : b.date.localeCompare(a.date))
    .slice(0, 10);
  if(related.length===0){
    list.innerHTML = `<div style="font-size:13px;color:var(--ink-soft);">No transactions for this account yet.</div>`;
  } else {
    related.forEach(t=>{
      const row = document.createElement('div');
      row.style.cssText = 'display:flex;justify-content:space-between;font-size:13px;padding:6px 0;border-bottom:1px solid var(--line);';
      let sign, color, desc;
      if(t.type==='transfer'){
        const toAcc = paymentAccounts.find(a=>a.id===t.toAccountId);
        const fromAcc = paymentAccounts.find(a=>a.id===t.accountId);
        if(t.accountId===acc.id){ sign='-'; color='var(--red)'; desc='Transfer to '+accountLabel(toAcc); }
        else { sign='+'; color='var(--green)'; desc='Transfer from '+accountLabel(fromAcc); }
      } else {
        sign = t.type==='in' ? '+' : '-';
        color = t.type==='in' ? 'var(--green)' : 'var(--red)';
        desc = t.note || (t.type==='in' ? 'Money In' : 'Money Out');
      }
      row.innerHTML = `<span>${new Date(t.date).toLocaleDateString('en-IN',{month:'short',day:'numeric'})} — ${escapeHtml(desc)}</span><span class="mono" style="color:${color};">${sign}${fmt(t.amount)}</span>`;
      list.appendChild(row);
    });
  }

  document.getElementById('view-account-modal-overlay').classList.add('show');
  document.getElementById('va-edit').onclick = ()=>{
    document.getElementById('view-account-modal-overlay').classList.remove('show');
    openAccountModal(acc.id);
  };
}
document.getElementById('va-close').addEventListener('click', ()=>{
  document.getElementById('view-account-modal-overlay').classList.remove('show');
});

function openViewTxnModal(txnId){
  const t = transactions.find(x=>x.id===txnId);
  if(!t) return;
  document.getElementById('vt-id').textContent = 'Reference: '+t.id;
  document.getElementById('vt-date').textContent = new Date(t.date).toLocaleDateString('en-IN',{year:'numeric',month:'long',day:'numeric'});
  document.getElementById('vt-note').textContent = t.note || '—';

  const fromAcc = paymentAccounts.find(a=>a.id===t.accountId);
  const fromHistory = computeAccountBalanceHistory(t.accountId)[t.id] || {before:0, after:0};

  if(t.type==='transfer'){
    const toAcc = paymentAccounts.find(a=>a.id===t.toAccountId);
    document.getElementById('vt-type').textContent = 'Transfer';
    document.getElementById('vt-amount').textContent = fmt(t.amount);
    document.getElementById('vt-amount').style.color = 'var(--ink)';
    document.getElementById('vt-from-heading').textContent = 'From Account';
    document.getElementById('vt-from-name').textContent = accountLabel(fromAcc);
    document.getElementById('vt-from-opening').textContent = fmt(fromAcc ? (fromAcc.openingBalance||0) : 0);
    document.getElementById('vt-from-before').textContent = fmt(fromHistory.before);
    document.getElementById('vt-from-after').textContent = fmt(fromHistory.after);
    document.getElementById('vt-from-current').textContent = fmt(computeAccountBalance(t.accountId));

    const toHistory = computeAccountBalanceHistory(t.toAccountId)[t.id] || {before:0, after:0};
    document.getElementById('vt-to-section').style.display = 'block';
    document.getElementById('vt-to-name').textContent = accountLabel(toAcc);
    document.getElementById('vt-to-opening').textContent = fmt(toAcc ? (toAcc.openingBalance||0) : 0);
    document.getElementById('vt-to-before').textContent = fmt(toHistory.before);
    document.getElementById('vt-to-after').textContent = fmt(toHistory.after);
    document.getElementById('vt-to-current').textContent = fmt(computeAccountBalance(t.toAccountId));
  } else {
    document.getElementById('vt-type').textContent = t.type==='in' ? 'Money In' : 'Money Out';
    document.getElementById('vt-amount').textContent = (t.type==='in'?'+':'-') + fmt(t.amount);
    document.getElementById('vt-amount').style.color = t.type==='in' ? 'var(--green)' : 'var(--red)';
    document.getElementById('vt-from-heading').textContent = 'Account';
    document.getElementById('vt-from-name').textContent = accountLabel(fromAcc);
    document.getElementById('vt-from-opening').textContent = fmt(fromAcc ? (fromAcc.openingBalance||0) : 0);
    document.getElementById('vt-from-before').textContent = fmt(fromHistory.before);
    document.getElementById('vt-from-after').textContent = fmt(fromHistory.after);
    document.getElementById('vt-from-current').textContent = fmt(computeAccountBalance(t.accountId));
    document.getElementById('vt-to-section').style.display = 'none';
  }

  document.getElementById('view-txn-modal-overlay').classList.add('show');
}
document.getElementById('vt-close').addEventListener('click', ()=>{
  document.getElementById('view-txn-modal-overlay').classList.remove('show');
});

function openAccountModal(accountId){
  editingAccountId = accountId || null;
  const acc = accountId ? paymentAccounts.find(a=>a.id===accountId) : null;
  document.getElementById('am-title').textContent = acc ? 'Edit Account' : 'Add Account';
  document.getElementById('am-name').value = acc ? acc.name : '';
  document.getElementById('am-bank-name').value = acc ? (acc.bankName||'') : '';
  document.getElementById('am-account-number').value = acc ? (acc.accountNumber||'') : '';
  document.getElementById('am-ifsc').value = acc ? (acc.ifsc||'') : '';
  document.getElementById('am-opening-balance').value = acc ? (acc.openingBalance||0) : '';
  setAccountKind(acc ? acc.kind : 'cash');
  document.getElementById('account-modal-overlay').classList.add('show');
}
document.getElementById('add-account-btn').addEventListener('click', ()=>openAccountModal(null));
document.getElementById('am-cancel').addEventListener('click', ()=>{
  document.getElementById('account-modal-overlay').classList.remove('show');
});
document.getElementById('am-save').addEventListener('click', async ()=>{
  const kind = document.querySelector('input[name="am-kind"]:checked').value;
  const name = document.getElementById('am-name').value.trim();
  const bankName = document.getElementById('am-bank-name').value.trim();
  const accountNumber = document.getElementById('am-account-number').value.trim();
  const ifsc = document.getElementById('am-ifsc').value.trim();
  const openingBalance = parseFloat(document.getElementById('am-opening-balance').value)||0;
  if(!name){ showToast('Enter an account name'); return; }
  if(kind==='bank' && !accountNumber){ showToast('Enter the bank account number'); return; }
  if(editingAccountId){
    const acc = paymentAccounts.find(a=>a.id===editingAccountId);
    Object.assign(acc, { name, kind, bankName: kind==='bank'?bankName:'', accountNumber: kind==='bank'?accountNumber:'', ifsc: kind==='bank'?ifsc:'', openingBalance });
  } else {
    paymentAccounts.push({ id:'acc_'+Date.now(), name, kind, bankName: kind==='bank'?bankName:'', accountNumber: kind==='bank'?accountNumber:'', ifsc: kind==='bank'?ifsc:'', openingBalance, createdAt:Date.now() });
  }
  await saveAccountsList();
  document.getElementById('account-modal-overlay').classList.remove('show');
  renderAccounts();
  showToast('Account saved');
});

/* ================= PROFILE ================= */
function findCanonicalState(rawValue){
  const match = INDIA_STATES.find(s=>normalizeState(s)===normalizeState(rawValue));
  return match || '';
}
function renderProfileForm(){
  document.getElementById('biz-name').value = profile.name||'';
  document.getElementById('biz-address').value = profile.address||'';
  document.getElementById('biz-gstin').value = profile.gstin||'';
  populateStateSelect(document.getElementById('biz-state'), true);
  document.getElementById('biz-state').value = findCanonicalState(profile.state);
  document.getElementById('biz-pin').value = profile.pin||'';
  document.getElementById('biz-phone').value = profile.phone||'';
  document.getElementById('sheet-sync-url').value = profile.sheetSyncUrl||'';
  setSyncStatus(profile.sheetSyncUrl ? 'Auto-sync is on for new invoices and transactions.' : 'Not connected — paste your Web App URL to enable auto-sync.');
  const preview = document.getElementById('logo-preview');
  preview.innerHTML = profile.logo ? `<img src="${profile.logo}">` : 'Click or drop an image';
}
document.getElementById('logo-input').addEventListener('change', (e)=>{
  const file = e.target.files[0];
  if(!file) return;
  const reader = new FileReader();
  reader.onload = ()=>{
    profile.logo = reader.result;
    document.getElementById('logo-preview').innerHTML = `<img src="${profile.logo}">`;
  };
  reader.readAsDataURL(file);
});
document.getElementById('save-profile-btn').addEventListener('click', async ()=>{
  profile.name = document.getElementById('biz-name').value.trim();
  profile.address = document.getElementById('biz-address').value.trim();
  profile.gstin = document.getElementById('biz-gstin').value.trim();
  profile.state = document.getElementById('biz-state').value;
  profile.pin = document.getElementById('biz-pin').value.trim();
  profile.phone = document.getElementById('biz-phone').value.trim();
  await saveProfile();
  showToast('Business profile saved');
});

/* ================= INVOICE MODAL ================= */
function openInvoiceModal(id){
  const inv = invoices.find(x=>x.id===id);
  if(!inv) return;
  currentInvoiceIdForModal = id;
  const body = document.getElementById('inv-modal-body');
  const isInter = inv.taxType==='inter';
  const pay = paymentStatusInfo(inv);

  const invDate = new Date(inv.date);
  const dueDate = new Date(invDate);
  dueDate.setDate(dueDate.getDate()+15);
  const dueDateStr = dueDate.toLocaleDateString('en-IN', {year:'numeric',month:'short',day:'numeric'});

  let stampHtml = '';
  if(pay.status==='paid') stampHtml = '<div class="stamp stamp-paid">PAID</div>';
  else if(pay.status==='partial') stampHtml = '<div class="stamp stamp-partial">PARTIALLY<br>PAID</div>';

  let dueNoteHtml = '';
  if(pay.due>0){
    if(pay.paid>0){
      dueNoteHtml = `<div class="inv-due-note">
        <div class="due-title">Part Payment Received</div>
        A part payment of <strong>${fmt(pay.paid)}</strong> has been received against this invoice. The remaining balance of <strong>${fmt(pay.due)}</strong> is requested to be paid within <strong>15 days</strong> from the invoice date, i.e. on or before <strong>${dueDateStr}</strong>.
      </div>`;
    } else {
      dueNoteHtml = `<div class="inv-due-note">
        <div class="due-title">Payment Due</div>
        The due amount of <strong>${fmt(pay.due)}</strong> is requested to be paid within <strong>15 days</strong> from the invoice date, i.e. on or before <strong>${dueDateStr}</strong>.
      </div>`;
    }
  }

  body.innerHTML = `
    <div class="ledger-sheet">
      ${stampHtml}
      <div class="inv-top">
        <div class="inv-brand">
          ${profile.logo ? `<img src="${profile.logo}">` : ''}
          <div>
            <div class="inv-brand-name">${escapeHtml(profile.name||'Your Business')}</div>
            <div class="inv-brand-meta">${escapeHtml(profile.address||'')}${profile.gstin?('<br>GSTIN: '+escapeHtml(profile.gstin)):''}${profile.state?('<br>State: '+escapeHtml(profile.state)+(profile.pin?(' - '+escapeHtml(profile.pin)):'')):''}</div>
          </div>
        </div>
        <div class="inv-num">
          <div class="no">${inv.invoiceNo}</div>
          <div>${new Date(inv.date).toLocaleDateString('en-IN', {year:'numeric',month:'short',day:'numeric'})}</div>
        </div>
      </div>
      <div class="inv-parties">
        <div class="inv-party">
          <div class="k">Billed To</div>
          <div class="v">${escapeHtml(inv.customer.name)}<br>${escapeHtml(inv.customer.address||'')}${(inv.customer.state||inv.customer.pin)?('<br>'+escapeHtml([inv.customer.state,inv.customer.pin].filter(Boolean).join(' - '))):''}${inv.customer.phone?('<br>'+escapeHtml(inv.customer.phone)):''}${inv.customer.gstin?('<br>GSTIN: '+escapeHtml(inv.customer.gstin)):''}</div>
        </div>
        <div class="inv-party">
          <div class="k">Tax Type</div>
          <div class="v">${isInter?'Inter-state (IGST)':'Intra-state (CGST + SGST)'}</div>
        </div>
      </div>
      <div class="inv-table">
        <table>
          <thead><tr><th>Item</th><th>Qty</th><th>Price</th><th>GST%</th><th>Taxable</th><th>GST</th></tr></thead>
          <tbody>
            ${inv.items.map(it=>`<tr><td>${escapeHtml(it.name)}</td><td>${it.qty}</td><td class="mono">${fmt(it.price)}${it.priceMode==='incl'?' <span style="font-size:9.5px;color:var(--ink-soft);">(incl. GST)</span>':''}</td><td>${it.gst}%</td><td class="mono">${fmt(it.taxable)}</td><td class="mono">${fmt(it.taxable*(it.gst/100))}</td></tr>`).join('')}
          </tbody>
        </table>
      </div>
      <div class="inv-totals">
        <table>
          <tr><td>Taxable Value</td><td class="mono">${fmt(inv.taxable)}</td></tr>
          ${isInter ? `<tr><td>IGST</td><td class="mono">${fmt(inv.totalIgst)}</td></tr>`
                    : `<tr><td>CGST</td><td class="mono">${fmt(inv.totalCgst)}</td></tr><tr><td>SGST</td><td class="mono">${fmt(inv.totalSgst)}</td></tr>`}
          <tr class="grand"><td>Grand Total</td><td class="mono">${fmt(inv.grandTotal)}</td></tr>
          ${pay.paid>0 ? `<tr><td>Amount Paid</td><td class="mono">${fmt(pay.paid)}</td></tr>` : ''}
          ${pay.due>0 ? `<tr><td style="color:var(--red);font-weight:700;">Balance Due</td><td class="mono" style="color:var(--red);font-weight:700;">${fmt(pay.due)}</td></tr>` : ''}
        </table>
      </div>
      ${dueNoteHtml}
      <div class="inv-signatures">
        <div class="sig-block">
          <div class="sig-space"></div>
          <div class="sig-line"></div>
          <div class="sig-label">Invoice Passed By</div>
          <div class="sig-sub">(Signature &amp; Seal)</div>
        </div>
        <div class="sig-block">
          <div class="sig-space"></div>
          <div class="sig-line"></div>
          <div class="sig-label">Payment Received By</div>
          <div class="sig-sub">(Signature &amp; Seal)</div>
        </div>
      </div>
    </div>
  `;
  document.getElementById('mark-paid-btn').style.display = pay.status==='paid' ? 'none' : 'inline-flex';
  document.getElementById('mark-paid-btn').textContent = pay.paid>0 ? 'Update Payment' : 'Record Payment';
  document.getElementById('inv-modal-overlay').classList.add('show');
}
document.getElementById('close-modal-btn').addEventListener('click', ()=>{
  document.getElementById('inv-modal-overlay').classList.remove('show');
  renderDashboard(); renderHistory();
});
document.getElementById('mark-paid-btn').addEventListener('click', ()=>{
  const inv = invoices.find(x=>x.id===currentInvoiceIdForModal);
  if(!inv) return;
  const pay = paymentStatusInfo(inv);
  document.getElementById('payment-modal-sub').textContent = `Grand Total: ${fmt(inv.grandTotal)}${pay.paid>0 ? ' | Already Paid: '+fmt(pay.paid) : ''}`;
  document.getElementById('payment-amount-input').value = pay.due>0 ? pay.due : '';
  document.getElementById('payment-remaining-hint').textContent = `Enter the full amount received so far (not just today's payment). Balance due will be calculated automatically.`;
  document.getElementById('payment-error').textContent = '';
  document.getElementById('payment-modal-overlay').classList.add('show');
});
document.getElementById('payment-amount-input').addEventListener('keydown', (e)=>{
  if(e.key==='Enter') document.getElementById('payment-save-btn').click();
});
document.getElementById('payment-cancel-btn').addEventListener('click', ()=>{
  document.getElementById('payment-modal-overlay').classList.remove('show');
});
document.getElementById('payment-save-btn').addEventListener('click', async ()=>{
  const inv = invoices.find(x=>x.id===currentInvoiceIdForModal);
  if(!inv) return;
  const errEl = document.getElementById('payment-error');
  const raw = document.getElementById('payment-amount-input').value;
  const amount = parseFloat(raw);
  if(raw===''||isNaN(amount)||amount<0){ errEl.textContent = 'Enter a valid amount (0 or more).'; return; }
  if(amount > inv.grandTotal + 0.001){ errEl.textContent = `Amount cannot exceed the Grand Total of ${fmt(inv.grandTotal)}.`; return; }
  inv.paidAmount = Math.round(amount*100)/100;
  inv.status = inv.paidAmount>=inv.grandTotal ? 'paid' : (inv.paidAmount>0 ? 'partial' : 'unpaid');
  await saveInvoices();
  document.getElementById('payment-modal-overlay').classList.remove('show');
  showToast(inv.status==='paid' ? 'Marked as fully paid' : (inv.status==='partial' ? 'Part payment recorded' : 'Payment updated'));
  openInvoiceModal(inv.id);
});

/* ================= INIT ================= */
async function checkStorageHealth(){
  try{
    if(!storage || typeof storage.set!=='function'){
      throw new Error('no storage API');
    }
    const probeKey = '__storage_health_check__';
    await storage.set(probeKey, String(Date.now()));
    const readBack = await storage.get(probeKey);
    if(!readBack) throw new Error('write not confirmed');
    return true;
  }catch(e){
    return false;
  }
}
initAuth();
</script>
</body>
</html>
